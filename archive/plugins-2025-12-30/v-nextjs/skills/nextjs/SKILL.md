---
name: nextjs
description: Next.js conventions for VioletDashboard and Connect
---

# Next.js Conventions

## Stack Overview
- **Framework**: Next.js 12.x with TypeScript
- **React Version**: React 17
- **State**: Redux Toolkit with redux-saga for side effects
- **Styling**: SCSS modules
- **Testing**: Jest + Playwright

## Dynamic Routing Pattern
Violet Connect uses nested dynamic routes:

```
pages/
├── [appAlias]/
│   └── [platformsMode]/
│       └── [platform]/
│           └── index.tsx
```

- `[appAlias]` - App/channel subdomain (e.g., 'andydev')
- `[platformsMode]` - Either 'platforms' or 'merchant-migration'
- `[platform]` - E-commerce platform (shopify, bigcommerce, etc.)

## API Routes
Next.js API routes with middleware pipeline:

```typescript
// pages/api/validate_cookie.ts
import { withValidation, withAuth } from '@/middlewares';

async function handler(req: NextApiRequest, res: NextApiResponse) {
  // Handler logic
}

export default withAuth(withValidation(handler));
```

## Authentication Flow
- Cookie-based authentication via `/api/validate_cookie`
- AppContext component handles auth state and routing
- Supports merchant login and BigCommerce embedded app flows

```typescript
// Using auth context
const { isAuthenticated, user } = useAuth();

if (!isAuthenticated) {
  return <LoginPage />;
}
```

---

## AppContext Pattern (VioletConnect)

**CRITICAL**: AppContext is VioletConnect's application-wide context provider. **ALWAYS check what AppContext already handles before creating new contexts or duplicating functionality.**

### What AppContext Manages

AppContext already handles:
- `appId` - Current application ID
- `appAlias` - URL alias for the app (e.g., 'andydev')
- **`merchantId`** - Query parameter detection (**already implemented**)
- `channelName` - Channel display name
- Session and authentication state
- OAuth redirect state

### AppContext Structure

```typescript
interface AppContextValue {
  appId: number;
  appAlias: string;
  merchantId?: string;     // From ?merchantId= query param
  channelName: string;
  isAuthenticated: boolean;
  // ... other app-wide state
}

// Access AppContext
import { useContext } from 'react';
import { AppContext } from '@/contexts/AppContext';

function MyComponent() {
  const { appId, merchantId, channelName } = useContext(AppContext);

  // Use context values...
}
```

### ❌ ANTI-PATTERN: Re-Implementing AppContext Functionality

**DON'T modify the catchall route or re-implement query param detection:**

```typescript
// ❌ WRONG - pages/[appAlias]/[[...catchall]].tsx
function CatchallPage() {
  const router = useRouter();

  // ❌ NO - merchantId detection already exists in AppContext
  const merchantId = router.query.merchantId;

  useEffect(() => {
    if (merchantId) {
      // ❌ This breaks existing functionality
      dispatch(lookupByMerchantId(merchantId));
    }
  }, [merchantId]);

  return <ErrorPage />;
}
```

**Why this is wrong**:
- AppContext **already detects** `merchantId` from query params
- Catchall route is for 404 handling only
- Adding feature logic here breaks graceful error handling

### ✅ CORRECT: Extend AppContext

When you need to add application-wide behavior based on query params or app state, extend AppContext:

```typescript
// ✅ CORRECT - contexts/AppContext.tsx
import { useEffect } from 'react';
import { useRouter } from 'next/router';
import { useDispatch } from 'react-redux';
import { lookupByMerchantId } from '@/redux/slices/preRegistration';

const AppProvider = ({ children }: AppProviderProps) => {
  const router = useRouter();
  const dispatch = useDispatch();

  // Existing merchantId detection (already exists)
  const merchantId = router.query.merchantId as string;

  // Add new behavior: pre-registration lookup
  useEffect(() => {
    if (merchantId && !isNaN(Number(merchantId))) {
      dispatch(lookupByMerchantId(Number(merchantId)));
    }
  }, [merchantId, dispatch]);

  return (
    <AppContext.Provider value={{
      appId,
      appAlias,
      merchantId,
      channelName,
      // ... other values
    }}>
      {children}
    </AppContext.Provider>
  );
};
```

### When to Extend AppContext

✅ **DO extend AppContext when**:
- Detecting query parameters (e.g., `?merchantId=`, `?token=`)
- Adding application-wide state changes
- Handling OAuth redirect parameters
- Managing session initialization
- App-level routing logic

❌ **DON'T use AppContext for**:
- Feature-specific state (use Redux)
- Page-specific data (use component state)
- Form data (use React Hook Form)
- Temporary UI state (use component state)

### Example: Pre-Registration Flow Integration

```typescript
// contexts/AppContext.tsx - Extended with pre-registration support
const AppProvider = ({ children }: AppProviderProps) => {
  const router = useRouter();
  const dispatch = useDispatch();
  const { current, found } = useSelector((state) => state.preRegistration);

  const merchantId = router.query.merchantId as string;

  // Check pre-registration on merchantId param
  useEffect(() => {
    if (merchantId && !isNaN(Number(merchantId))) {
      dispatch(lookupByMerchantId(Number(merchantId)));
    }
  }, [merchantId, dispatch]);

  // Redirect to confirmation page if pre-registration found
  useEffect(() => {
    if (found && current && merchantId) {
      router.push(`/${appAlias}/pre-registration/confirm`);
    }
  }, [found, current, merchantId, appAlias, router]);

  return (
    <AppContext.Provider value={{ appId, appAlias, merchantId, channelName }}>
      {children}
    </AppContext.Provider>
  );
};
```

### Rule of Thumb

**Before adding new context or query param detection:**
1. Check if AppContext already handles it (**it probably does**)
2. If it does, extend AppContext instead of duplicating
3. If it doesn't, consider if it's truly app-wide state

**Default to using AppContext for query params and app-level state.**

---

## State Management
Redux with typed hooks:

```typescript
// redux/hooks.ts
export const useAppDispatch = () => useDispatch<AppDispatch>();
export const useAppSelector: TypedUseSelectorHook<RootState> = useSelector;

// Usage
const dispatch = useAppDispatch();
const { merchants } = useAppSelector(state => state.merchants);
```

## Environment Configuration
Required in `.env.local`:
- Violet API endpoints and URLs
- Redis configuration for session storage
- Stripe and payment credentials
- Testing credentials for E2E

## Form Handling
Use react-hook-form:

```typescript
import { useForm } from 'react-hook-form';

const { register, handleSubmit, errors } = useForm<FormData>();

const onSubmit = (data: FormData) => {
  // Submit logic
};
```

## Axios Wrapper
Use the centralized axios wrapper:

```typescript
import { axiosWrapper } from '@/utilities/axiosWrapper';

const response = await axiosWrapper.get('/api/endpoint');
```

## Internal Dependencies
- `@violet/shared-components` - Component library
- `@violet/shared-types` - Type definitions

```bash
# Login to AWS CodeArtifact for internal packages
npm run co:login
```

## Commands
```bash
npm run dev           # Development server (port 3001)
npm run build         # Production build
npm run lint          # ESLint
npm run test          # Unit tests
npm run test:e2e      # Playwright E2E tests
npm run lighthouse    # Performance tests
```

## Subdomain Routing
Local development requires `/etc/hosts` configuration:

```
127.0.0.1 appname.localhost
```

## Performance
- Use virtualized lists for large datasets
- Implement proper loading states
- Cache API responses where appropriate
- Run Lighthouse tests for performance monitoring
