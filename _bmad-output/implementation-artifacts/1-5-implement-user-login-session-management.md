# Story 1.5: Implement User Login & Session Management

Status: ready-for-dev

---

## Story

As a gestionnaire,
I want to login using my email and password,
So that I can access my company's FlotteBox dashboard.

---

## Acceptance Criteria

### AC1: Login Form
**Given** I have previously registered with email/password (Story 1.3)
**When** I visit `/login` and enter my email and password
**Then** Better Auth validates credentials against bcrypt hash

### AC2: Successful Login
**And** if credentials are correct:
- A new session is created with 24h expiration (for ADMIN/MANAGER roles)
- Session cookie is set with secure flags
- I am redirected to `/dashboard`

### AC3: Failed Login - Wrong Credentials
**And** if credentials are incorrect:
- Login attempt is logged (for rate limiting)
- I see error: "Email ou mot de passe incorrect"
- After 5 failed attempts, account is temporarily locked for 15 minutes (NFR-S13)

### AC4: Account Locked
**And** if account is locked, I see error: "Compte temporairement bloque. Reessayez dans 15 minutes."

### AC5: Forgot Password Link
**And** when I click "Mot de passe oublie?", I am redirected to `/forgot-password`

### AC6: Logout Functionality
**And** logout functionality is available via `/api/auth/logout` endpoint that:
- Destroys the session
- Clears the session cookie
- Redirects to `/login`

---

## Tasks / Subtasks

- [ ] **Task 1: Create Login Page UI** (AC: #1)
  - [ ] 1.1 Create `/app/(auth)/login/page.tsx`
  - [ ] 1.2 Build form with shadcn/ui Form component
  - [ ] 1.3 Add Email input field
  - [ ] 1.4 Add Password input field with show/hide toggle
  - [ ] 1.5 Add "Se connecter" submit button
  - [ ] 1.6 Add "Mot de passe oublie?" link
  - [ ] 1.7 Add "Pas encore de compte? S'inscrire" link
  - [ ] 1.8 Style for mobile-first responsive design

- [ ] **Task 2: Implement Login API** (AC: #1, #2)
  - [ ] 2.1 Configure Better Auth login endpoint
  - [ ] 2.2 Validate credentials against bcrypt hash
  - [ ] 2.3 Create session with 24h expiration
  - [ ] 2.4 Set secure cookie flags

- [ ] **Task 3: Implement Rate Limiting** (AC: #3, #4)
  - [ ] 3.1 Create login attempts tracking table/model
  - [ ] 3.2 Increment counter on failed login
  - [ ] 3.3 Block after 5 failed attempts
  - [ ] 3.4 Set 15-minute lockout duration
  - [ ] 3.5 Reset counter on successful login
  - [ ] 3.6 Display appropriate error messages

- [ ] **Task 4: Implement Forgot Password Page** (AC: #5)
  - [ ] 4.1 Create `/app/(auth)/forgot-password/page.tsx`
  - [ ] 4.2 Add email input form
  - [ ] 4.3 Display confirmation message (MVP: placeholder, no actual email)

- [ ] **Task 5: Implement Logout** (AC: #6)
  - [ ] 5.1 Create logout API endpoint via Better Auth
  - [ ] 5.2 Destroy session
  - [ ] 5.3 Clear session cookie
  - [ ] 5.4 Create logout button component
  - [ ] 5.5 Redirect to `/login` after logout

- [ ] **Task 6: Session Management** (AC: #2)
  - [ ] 6.1 Configure session duration (24h for ADMIN/MANAGER)
  - [ ] 6.2 Implement session refresh on activity
  - [ ] 6.3 Add session validation middleware

- [ ] **Task 7: Protected Routes** (Middleware)
  - [ ] 7.1 Create auth middleware for dashboard routes
  - [ ] 7.2 Redirect to `/login` if no valid session
  - [ ] 7.3 Store intended destination for redirect after login

- [ ] **Task 8: Testing** (Validation)
  - [ ] 8.1 Test successful login flow
  - [ ] 8.2 Test invalid credentials error
  - [ ] 8.3 Test rate limiting (5 attempts)
  - [ ] 8.4 Test account lockout message
  - [ ] 8.5 Test logout clears session
  - [ ] 8.6 Test protected route redirect

---

## Dev Notes

### Dependencies on Previous Stories

- **Story 1.1**: Better Auth configured
- **Story 1.2**: User model with passwordHash
- **Story 1.3**: Users exist in database

### Better Auth Login Configuration

```typescript
// lib/auth.ts
export const auth = betterAuth({
  // ... existing config
  emailAndPassword: {
    enabled: true,
    password: {
      minLength: 12,
      maxLength: 128,
    },
  },
  session: {
    expiresIn: 60 * 60 * 24, // 24 hours
    updateAge: 60 * 60, // Update session every hour
    cookieCache: {
      enabled: true,
      maxAge: 60 * 5, // 5 minutes
    },
  },
  rateLimit: {
    enabled: true,
    window: 60 * 15, // 15 minutes
    max: 5, // 5 attempts
  },
});
```

### Login Page Component

```typescript
// app/(auth)/login/page.tsx
'use client';

import { useForm } from 'react-hook-form';
import { zodResolver } from '@hookform/resolvers/zod';
import { loginSchema } from '@/lib/validations/auth';
import { authClient } from '@/lib/auth-client';

export default function LoginPage() {
  const form = useForm({
    resolver: zodResolver(loginSchema),
  });

  const onSubmit = async (data) => {
    try {
      await authClient.signIn.email({
        email: data.email,
        password: data.password,
        callbackURL: '/dashboard',
      });
    } catch (error) {
      if (error.code === 'RATE_LIMITED') {
        form.setError('root', {
          message: 'Compte temporairement bloque. Reessayez dans 15 minutes.',
        });
      } else {
        form.setError('root', {
          message: 'Email ou mot de passe incorrect',
        });
      }
    }
  };

  return (
    <form onSubmit={form.handleSubmit(onSubmit)}>
      {/* Form fields */}
    </form>
  );
}
```

### Rate Limiting Model (if not built-in)

```prisma
model LoginAttempt {
  id        String   @id @default(cuid())
  email     String
  success   Boolean
  ipAddress String?
  createdAt DateTime @default(now())

  @@index([email, createdAt])
}
```

### Rate Limiting Logic

```typescript
// lib/rate-limit.ts
const MAX_ATTEMPTS = 5;
const LOCKOUT_DURATION = 15 * 60 * 1000; // 15 minutes

export async function checkRateLimit(email: string): Promise<boolean> {
  const cutoff = new Date(Date.now() - LOCKOUT_DURATION);

  const recentAttempts = await prisma.loginAttempt.count({
    where: {
      email,
      success: false,
      createdAt: { gte: cutoff },
    },
  });

  return recentAttempts < MAX_ATTEMPTS;
}

export async function recordLoginAttempt(email: string, success: boolean) {
  await prisma.loginAttempt.create({
    data: { email, success },
  });

  if (success) {
    // Clear failed attempts on success
    await prisma.loginAttempt.deleteMany({
      where: { email, success: false },
    });
  }
}
```

### Logout Component

```typescript
// components/auth/LogoutButton.tsx
'use client';

import { authClient } from '@/lib/auth-client';
import { useRouter } from 'next/navigation';

export function LogoutButton() {
  const router = useRouter();

  const handleLogout = async () => {
    await authClient.signOut();
    router.push('/login');
  };

  return (
    <button onClick={handleLogout}>
      Se deconnecter
    </button>
  );
}
```

### Auth Middleware

```typescript
// middleware.ts
import { NextResponse } from 'next/server';
import type { NextRequest } from 'next/server';

export function middleware(request: NextRequest) {
  const session = request.cookies.get('better-auth.session');

  if (!session && request.nextUrl.pathname.startsWith('/dashboard')) {
    const loginUrl = new URL('/login', request.url);
    loginUrl.searchParams.set('callbackUrl', request.nextUrl.pathname);
    return NextResponse.redirect(loginUrl);
  }

  return NextResponse.next();
}

export const config = {
  matcher: ['/dashboard/:path*'],
};
```

### French Error Messages

| Error | Message |
|-------|---------|
| Wrong credentials | "Email ou mot de passe incorrect" |
| Account locked | "Compte temporairement bloque. Reessayez dans 15 minutes." |
| Session expired | "Votre session a expire. Veuillez vous reconnecter." |

### Session Configuration

- **ADMIN/MANAGER**: 24h session, refresh on activity
- **DRIVER**: (Future Story 3.3) 7 days session for mobile convenience
- **Cookie flags**: httpOnly, secure (production), sameSite=strict

---

### Project Structure Notes

- **Fichiers a creer:**
  - `app/(auth)/login/page.tsx`
  - `app/(auth)/forgot-password/page.tsx`
  - `components/auth/LogoutButton.tsx`
  - `lib/rate-limit.ts`
  - `middleware.ts`

- **Fichiers a modifier:**
  - `lib/auth.ts` - Rate limiting config
  - `prisma/schema.prisma` - LoginAttempt model (if needed)

---

### References

- [Source: architecture.md#Authentication] - Better Auth session config
- [Source: prd.md#Authentification] - Login with rate limiting
- [Source: epics.md#Story 1.5] - Acceptance criteria originaux
- [Source: NFR-S13] - 5 attempts, 15 min lockout

---

## Dev Agent Record

### Agent Model Used

{{agent_model_name_version}}

### Debug Log References

_A remplir pendant l'implementation_

### Completion Notes List

_A remplir pendant l'implementation_

### File List

_A remplir pendant l'implementation avec la liste des fichiers crees/modifies_
