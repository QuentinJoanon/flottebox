# Story 1.4: Implement OAuth Google Authentication

Status: ready-for-dev

---

## Story

As a gestionnaire,
I want to authenticate using my Google account,
So that I can quickly register/login without managing a password.

---

## Acceptance Criteria

### AC1: Google OAuth Button
**Given** I am on the login page `/login` or registration page `/register`
**When** I click the "Continuer avec Google" button
**Then** I am redirected to Google OAuth consent screen

### AC2: OAuth Callback
**And** after granting permissions, I am redirected back to FlotteBox with OAuth code

### AC3: First-Time User (New Organization)
**And** if this is my first login (email not in database):
- A new `Organization` is created with name extracted from Google account
- A new `User` is created with role ADMIN, email from Google, and organizationId

### AC4: Existing User (Login)
**And** if my email already exists in the database:
- I am logged into the existing organization
- No duplicate user is created

### AC5: Redirect to Dashboard
**And** I am redirected to `/dashboard` with active session

### AC6: Secure Token Storage
**And** Better Auth stores OAuth tokens securely

### AC7: Error Handling
**And** if OAuth fails or is cancelled, I see error: "Authentification Google echouee. Veuillez reessayer."

### AC8: Environment Variables
**And** GOOGLE_CLIENT_ID and GOOGLE_CLIENT_SECRET are read from environment variables

---

## Tasks / Subtasks

- [ ] **Task 1: Configure Google Cloud Console** (AC: #8)
  - [ ] 1.1 Create Google Cloud project (or use existing)
  - [ ] 1.2 Enable Google+ API / OAuth consent screen
  - [ ] 1.3 Create OAuth 2.0 credentials (Web application)
  - [ ] 1.4 Add authorized redirect URIs:
    - `http://localhost:3000/api/auth/callback/google` (dev)
    - `https://flottebox.fr/api/auth/callback/google` (prod)
  - [ ] 1.5 Document GOOGLE_CLIENT_ID and GOOGLE_CLIENT_SECRET

- [ ] **Task 2: Configure Better Auth Google Provider** (AC: #1, #2, #6)
  - [ ] 2.1 Update `lib/auth.ts` with Google OAuth provider
  - [ ] 2.2 Configure OAuth scopes (email, profile)
  - [ ] 2.3 Configure callback URL
  - [ ] 2.4 Handle token storage

- [ ] **Task 3: Add Google Button to Login/Register Pages** (AC: #1)
  - [ ] 3.1 Create GoogleOAuthButton component
  - [ ] 3.2 Add to `/app/(auth)/login/page.tsx`
  - [ ] 3.3 Add to `/app/(auth)/register/page.tsx`
  - [ ] 3.4 Style button according to Google branding guidelines
  - [ ] 3.5 Add separator "ou" between OAuth and email/password

- [ ] **Task 4: Implement First-Time User Flow** (AC: #3)
  - [ ] 4.1 Check if email exists in database after OAuth callback
  - [ ] 4.2 If new user: create Organization with Google display name
  - [ ] 4.3 Create User with ADMIN role
  - [ ] 4.4 Link to new organization

- [ ] **Task 5: Implement Existing User Flow** (AC: #4)
  - [ ] 5.1 If email exists: retrieve existing user and organization
  - [ ] 5.2 Create session without creating duplicate records
  - [ ] 5.3 Handle case where user registered via email/password

- [ ] **Task 6: Redirect After Authentication** (AC: #5)
  - [ ] 6.1 Redirect to `/dashboard` on success
  - [ ] 6.2 Pass session via cookie

- [ ] **Task 7: Error Handling** (AC: #7)
  - [ ] 7.1 Handle OAuth cancellation by user
  - [ ] 7.2 Handle OAuth provider errors
  - [ ] 7.3 Display French error message on failure
  - [ ] 7.4 Redirect to login page with error state

- [ ] **Task 8: Environment Variables Setup** (AC: #8)
  - [ ] 8.1 Add GOOGLE_CLIENT_ID to .env
  - [ ] 8.2 Add GOOGLE_CLIENT_SECRET to .env
  - [ ] 8.3 Update .env.example with placeholders
  - [ ] 8.4 Document in README

- [ ] **Task 9: Testing** (Validation)
  - [ ] 9.1 Test new user OAuth flow
  - [ ] 9.2 Test existing user OAuth flow
  - [ ] 9.3 Test OAuth cancellation handling
  - [ ] 9.4 Test missing env variables error

---

## Dev Notes

### Dependencies on Previous Stories

- **Story 1.1**: Project initialized with Better Auth
- **Story 1.2**: Multi-tenant schema with Organization and User models
- **Story 1.3**: Login/Register pages exist (add Google button)

### Better Auth Google Provider Configuration

```typescript
// lib/auth.ts
import { betterAuth } from 'better-auth';
import { google } from 'better-auth/providers';

export const auth = betterAuth({
  // ... existing config
  socialProviders: {
    google: {
      clientId: process.env.GOOGLE_CLIENT_ID!,
      clientSecret: process.env.GOOGLE_CLIENT_SECRET!,
      scope: ['email', 'profile'],
    },
  },
  callbacks: {
    async signIn({ user, account, profile }) {
      if (account?.provider === 'google') {
        // Check if user exists
        const existingUser = await prisma.user.findFirst({
          where: { email: profile?.email },
        });

        if (!existingUser) {
          // Create new organization and user
          const org = await prisma.organization.create({
            data: { name: profile?.name || 'Mon Entreprise' },
          });

          await prisma.user.create({
            data: {
              email: profile?.email,
              name: profile?.name,
              organizationId: org.id,
              role: 'ADMIN',
            },
          });
        }
      }
      return true;
    },
  },
});
```

### Google OAuth Button Component

```typescript
// components/auth/GoogleOAuthButton.tsx
'use client';

import { authClient } from '@/lib/auth-client';
import { Button } from '@/components/ui/button';

export function GoogleOAuthButton() {
  const handleGoogleSignIn = async () => {
    try {
      await authClient.signIn.social({
        provider: 'google',
        callbackURL: '/dashboard',
      });
    } catch (error) {
      // Handle error - will be caught by error boundary
      throw new Error('Authentification Google echouee. Veuillez reessayer.');
    }
  };

  return (
    <Button
      variant="outline"
      className="w-full"
      onClick={handleGoogleSignIn}
    >
      <GoogleIcon className="mr-2 h-4 w-4" />
      Continuer avec Google
    </Button>
  );
}
```

### Environment Variables

```bash
# .env
GOOGLE_CLIENT_ID="your-google-client-id.apps.googleusercontent.com"
GOOGLE_CLIENT_SECRET="your-google-client-secret"
```

### Google Cloud Console Setup

1. Go to https://console.cloud.google.com/
2. Create new project or select existing
3. Navigate to "APIs & Services" > "Credentials"
4. Click "Create Credentials" > "OAuth client ID"
5. Select "Web application"
6. Add authorized redirect URIs:
   - `http://localhost:3000/api/auth/callback/google`
   - `https://flottebox.fr/api/auth/callback/google`
7. Copy Client ID and Client Secret

### OAuth Flow Diagram

```
User clicks "Continuer avec Google"
    ↓
Redirect to Google OAuth consent
    ↓
User grants permissions
    ↓
Redirect to /api/auth/callback/google
    ↓
Better Auth receives OAuth code
    ↓
Exchange code for tokens
    ↓
Check if user exists in DB
    ↓
┌─── New user ────┐    ┌─── Existing user ───┐
│ Create Org      │    │ Load existing user  │
│ Create User     │    │ Create session      │
│ Create session  │    └─────────────────────┘
└─────────────────┘
    ↓
Redirect to /dashboard
```

### Error States

| Error | User Action | Message |
|-------|-------------|---------|
| User cancels OAuth | Clicks "Cancel" on Google | "Authentification Google echouee. Veuillez reessayer." |
| Google API error | None | "Authentification Google echouee. Veuillez reessayer." |
| Missing env vars | None | Server error logged, generic error to user |

### Security Considerations

- OAuth tokens stored securely by Better Auth
- GOOGLE_CLIENT_SECRET never exposed to client
- CSRF protection via state parameter (handled by Better Auth)
- Refresh tokens for long-lived sessions

---

### Project Structure Notes

- **Fichiers a creer:**
  - `components/auth/GoogleOAuthButton.tsx`
  - `components/icons/GoogleIcon.tsx`

- **Fichiers a modifier:**
  - `lib/auth.ts` - Add Google provider
  - `app/(auth)/login/page.tsx` - Add Google button
  - `app/(auth)/register/page.tsx` - Add Google button
  - `.env` - Add Google credentials
  - `.env.example` - Add placeholders

---

### References

- [Source: architecture.md#Authentication] - Better Auth with OAuth Google
- [Source: prd.md#Authentification] - OAuth Google + Email/password
- [Source: epics.md#Story 1.4] - Acceptance criteria originaux

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
