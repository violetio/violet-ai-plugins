# VioletDashboard Code Patterns

## Overview

VioletDashboard is a Next.js application serving Channel and Merchant dashboards through subdomain routing.

**Tech Stack:**
- Next.js 12.x with React 17
- Redux Toolkit with Redux Persist
- SCSS modules
- TypeScript (strict)

**ALL patterns in this document are MANDATORY when working on VioletDashboard.**

---

## Redux Thunk Pattern (MANDATORY)

Use `createAppAsyncThunk` with `axiosWrapper`. Do NOT use `fetch` or standard `createAsyncThunk`.

```typescript
// redux/thunks/preRegistration.ts
import { createAppAsyncThunk, HasFulfilledMeta, HasRejectedMeta } from 'redux/thunks';
import axiosWrapper, { CONFIG_TYPE } from '@/utilities/axiosWrapper';
import { PreRegistrationActionType } from 'redux/actions/preRegistration';
import { PreRegistration, PreRegistrationListResult } from 'interfaces/preRegistration.interface';

// GET list - rejected meta only
export const getPreRegistrations = createAppAsyncThunk<
  PreRegistrationListResult,
  { page?: number; size?: number },
  HasRejectedMeta
>(
  PreRegistrationActionType.GET_PRE_REGISTRATIONS,
  async (params, { rejectWithValue }) => {
    try {
      const response = await axiosWrapper<PreRegistrationListResult>(
        CONFIG_TYPE.VIOLET_PROXY_API,
        {
          method: 'GET',
          path: 'api/merchants/pre-registrations',
          params,
        }
      );
      return response.data;
    } catch (err: any) {
      throw rejectWithValue(err, {
        errorMessage: err?.response?.data?.message || 'Failed to load pre-registrations',
      });
    }
  }
);

// POST create - fulfilled + rejected meta for success message
export const createPreRegistration = createAppAsyncThunk<
  PreRegistration,
  CreatePreRegistrationPayload,
  HasFulfilledMeta & HasRejectedMeta
>(
  PreRegistrationActionType.CREATE_PRE_REGISTRATION,
  async (data, { fulfillWithValue, rejectWithValue }) => {
    try {
      const response = await axiosWrapper<PreRegistration>(
        CONFIG_TYPE.VIOLET_PROXY_API,
        {
          method: 'POST',
          path: 'api/merchants/pre-register',
          data,
        }
      );
      return fulfillWithValue(response.data, {
        successMessage: 'Pre-registration created',
      });
    } catch (err: any) {
      throw rejectWithValue(err, {
        errorMessage: err?.response?.data?.message || 'Failed to create pre-registration',
      });
    }
  }
);
```

**Key points:**
- `CONFIG_TYPE.VIOLET_PROXY_API` for client-side calls (through Next.js API routes)
- `CONFIG_TYPE.VIOLET_API` for server-side calls (direct to backend)
- `rejectWithValue(err, { errorMessage })` enables automatic snackbar notifications
- `fulfillWithValue(data, { successMessage })` enables success notifications

---

## Redux Reducer Pattern (MANDATORY)

Use `createReducer` with `.addCase()`. Do NOT use `createSlice`.

```typescript
// redux/reducers/preRegistration.ts
import { createReducer } from '@reduxjs/toolkit';
import { PageableResult } from 'interfaces/pageable.interface';
import { PreRegistration } from 'interfaces/preRegistration.interface';
import {
  getPreRegistrations,
  createPreRegistration,
  updatePreRegistration,
  deletePreRegistration,
} from '../thunks/preRegistration';
import { clearPreRegistrations } from '../actions/preRegistration';
import withClearableState from '@/redux/reducers/withClearableState';

export interface PreRegistrationState {
  list: PreRegistration[];
  loading: boolean;
  totalElements: number;
  totalPages: number;
}

const initialState: PreRegistrationState = {
  list: [],
  loading: false,
  totalElements: 0,
  totalPages: 0,
};

const preRegistrationReducer = createReducer(initialState, (builder) => {
  builder
    // Loading states via thunk.pending (NOT manual dispatch)
    .addCase(getPreRegistrations.pending, (state) => {
      state.loading = true;
    })

    // Success states via thunk.fulfilled
    .addCase(getPreRegistrations.fulfilled, (state, action) => {
      state.loading = false;
      state.list = action.payload.content;
      state.totalElements = action.payload.totalElements;
      state.totalPages = action.payload.totalPages;
    })

    // Error states via thunk.rejected
    .addCase(getPreRegistrations.rejected, (state) => {
      state.loading = false;
    })

    .addCase(createPreRegistration.fulfilled, (state, action) => {
      state.list.unshift(action.payload);
      state.totalElements += 1;
    })

    .addCase(updatePreRegistration.fulfilled, (state, action) => {
      const index = state.list.findIndex((p) => p.merchantId === action.payload.merchantId);
      if (index !== -1) {
        state.list[index] = action.payload;
      }
    })

    .addCase(deletePreRegistration.fulfilled, (state, action) => {
      state.list = state.list.filter((p) => p.merchantId !== action.meta.arg.merchantId);
      state.totalElements -= 1;
    })

    // Synchronous clear action
    .addCase(clearPreRegistrations, () => initialState);
});

// Wrap for automatic cleanup on logout
export default withClearableState(preRegistrationReducer, initialState);
```

**Key points:**
- Use `.addCase(thunk.pending)` for loading - NOT manual `dispatch(startLoading())`
- Use `.addCase(thunk.fulfilled)` for success
- Use `.addCase(thunk.rejected)` for errors
- Wrap with `withClearableState` for logout cleanup
- State mutations are allowed (immer handles immutability)

---

## Redux Actions Pattern

Action types and synchronous actions only. Thunks go in `redux/thunks/`.

```typescript
// redux/actions/preRegistration.ts
import { createAction } from '@reduxjs/toolkit';

// Action type enum
export enum PreRegistrationActionType {
  GET_PRE_REGISTRATIONS = 'GET_PRE_REGISTRATIONS',
  CREATE_PRE_REGISTRATION = 'CREATE_PRE_REGISTRATION',
  UPDATE_PRE_REGISTRATION = 'UPDATE_PRE_REGISTRATION',
  DELETE_PRE_REGISTRATION = 'DELETE_PRE_REGISTRATION',
  CLEAR_PRE_REGISTRATIONS = 'CLEAR_PRE_REGISTRATIONS',
}

// Payload types namespace
export namespace PayloadType {
  export interface CreatePreRegistration {
    merchantName: string;
    storeUrl: string;
    clientId: string;
    secret: string;
    installLink: string;
  }
  export interface UpdatePreRegistration {
    merchantId: number;
    merchantName?: string;
    clientId?: string;
    secret?: string;
  }
}

// Synchronous actions only
export const clearPreRegistrations = createAction(
  PreRegistrationActionType.CLEAR_PRE_REGISTRATIONS
);
```

---

## API Route Pattern (MANDATORY)

Use `nextConnectConfiguration()` with `axiosWrapper`. Do NOT use custom middleware or `fetch`.

```typescript
// pages/api/merchants/pre-register/[id].ts
import type { NextApiRequest, NextApiResponse } from 'next';
import snakecaseKeys from 'snakecase-keys';
import { NextHandler } from 'next-connect';
import { VIOLET_TOKEN_HEADER } from '@violet/shared-types/constants/headers';
import axiosWrapper, { CONFIG_TYPE } from '@/utilities/axiosWrapper';
import { getTokenWithDecodedData } from '@/utilities/auth';
import { nextConnectConfiguration } from '@/middlewares/globalApiMiddleware';

const apiRoute = nextConnectConfiguration();

// GET handler
apiRoute.get(
  async (req: NextApiRequest, res: NextApiResponse, next: NextHandler) => {
    try {
      const tokenWithDecodedData = getTokenWithDecodedData(req, res);
      const { id } = req.query;

      const result = await axiosWrapper(CONFIG_TYPE.VIOLET_API, {
        method: 'GET',
        path: `merchants/external/pre-register/${id}`,
        headers: {
          [VIOLET_TOKEN_HEADER]: tokenWithDecodedData.token,
        },
      });

      res.status(result.status).json(result.data);
    } catch (err: any) {
      next(err); // Pass to central error handler
    }
  }
);

// PUT handler
apiRoute.put(
  async (req: NextApiRequest, res: NextApiResponse, next: NextHandler) => {
    try {
      const tokenWithDecodedData = getTokenWithDecodedData(req, res);
      const { id } = req.query;

      const result = await axiosWrapper(CONFIG_TYPE.VIOLET_API, {
        method: 'PUT',
        path: `merchants/external/pre-register/${id}`,
        headers: {
          [VIOLET_TOKEN_HEADER]: tokenWithDecodedData.token,
        },
        data: snakecaseKeys(req.body),
      });

      res.status(result.status).json(result.data);
    } catch (err: any) {
      next(err);
    }
  }
);

// DELETE handler
apiRoute.delete(
  async (req: NextApiRequest, res: NextApiResponse, next: NextHandler) => {
    try {
      const tokenWithDecodedData = getTokenWithDecodedData(req, res);
      const { id } = req.query;

      const result = await axiosWrapper(CONFIG_TYPE.VIOLET_API, {
        method: 'DELETE',
        path: `merchants/external/pre-register/${id}`,
        headers: {
          [VIOLET_TOKEN_HEADER]: tokenWithDecodedData.token,
        },
      });

      res.status(result.status).json(result.data);
    } catch (err: any) {
      next(err);
    }
  }
);

export default apiRoute;
```

**Key points:**
- `nextConnectConfiguration()` includes Helmet, CORS, auth middleware
- `getTokenWithDecodedData(req, res)` extracts auth from cookies
- `snakecaseKeys(req.body)` converts to backend format
- Pass errors to `next(err)` - NOT manual `res.status(500).json()`
- Response automatically converts snake_case to camelCase

---

## Interface Organization

- **Shared interfaces** go in `interfaces/*.ts`
- **Component props** go INLINE in the component file

```typescript
// interfaces/preRegistration.interface.ts
import { PageableResult } from './pageable.interface';

export interface PreRegistration {
  merchantId: number;
  merchantName: string;
  storeUrl: string;
  status: PreRegistrationStatus;
  dateCreated: string;
}

export enum PreRegistrationStatus {
  PENDING = 'PENDING',
  ACTIVE = 'ACTIVE',
  FOR_DELETION = 'FOR_DELETION',
  EXPIRED = 'EXPIRED',
}

export type PreRegistrationListResult = PageableResult<PreRegistration>;
```

```typescript
// components/merchants/PreRegTable.tsx - Props inline
interface PreRegTableProps {
  data: PreRegistration[];
  loading: boolean;
  onEdit: (item: PreRegistration) => void;
  onDelete: (merchantId: number) => void;
}

export const PreRegTable: React.FC<PreRegTableProps> = ({ data, loading, onEdit, onDelete }) => {
  // Component implementation
};
```

---

## Anti-Patterns (DO NOT USE)

| Wrong | Correct |
|-------|---------|
| `fetch('/api/...')` | `axiosWrapper(CONFIG_TYPE.VIOLET_PROXY_API, {...})` |
| `createSlice({...})` | `createReducer(initialState, (builder) => {...})` |
| `createAsyncThunk()` | `createAppAsyncThunk<R, P, M>()` |
| `dispatch(setLoading(true))` | `.addCase(thunk.pending, ...)` in reducer |
| Props in `interfaces/` | Props inline in component file |
| Manual CORS/Helmet | `nextConnectConfiguration()` |
| `res.status(500).json({...})` | `next(err)` in API routes |
| `console.log()` in commits | Remove before PR |

---

## Path Aliases

Always use path aliases, not relative imports:

```typescript
// Correct
import { Button } from '@/components/Button';
import { useAuth } from '@/hooks/useAuth';
import axiosWrapper from '@/utilities/axiosWrapper';

// Wrong
import { Button } from '../../../components/Button';
```

---

## Component Location

| Type | Location |
|------|----------|
| Reusable UI components | `components/` |
| Page-specific components | `pageComponents/` |
| Custom hooks | `hooks/` |
| Utilities | `utilities/` |
| Shared interfaces | `interfaces/` |
| Redux | `redux/actions/`, `redux/reducers/`, `redux/thunks/` |
