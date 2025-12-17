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
