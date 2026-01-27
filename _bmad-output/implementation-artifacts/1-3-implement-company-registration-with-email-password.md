# Story 1.3: Implement Company Registration with Email/Password

Status: ready-for-dev

---

## Story

As a gestionnaire,
I want to create a company account using email and password,
So that I can register my company and start using FlotteBox.

---

## Acceptance Criteria

### AC1: Registration Form
**Given** I am on the registration page `/register`
**When** I fill in the registration form with:
- Company name (required)
- Email address (required, valid format)
- Password (required, min 12 characters, must include uppercase, lowercase, number, special char)
- Password confirmation (must match)
- RGPD consent checkbox (required)
**Then** a new `Organization` record is created in the database

### AC2: User Record Creation
**And** a new `User` record is created with:
- Role: ADMIN
- organizationId: linked to the new organization
- passwordHash: bcrypt hashed with cost factor 12
- email: as provided

### AC3: Auto-Login After Registration
**And** I am automatically logged in with a valid session

### AC4: Redirect to Dashboard
**And** I am redirected to the dashboard page `/dashboard`

### AC5: Secure Session Cookie
**And** Better Auth session cookie is set with secure flags (httpOnly, secure, sameSite)

### AC6: Email Uniqueness Validation
**And** if email already exists for another organization, I see error: "Cet email est deja utilise"

### AC7: Password Strength Validation
**And** if password is weak, I see error: "Le mot de passe doit contenir au moins 12 caracteres, une majuscule, une minuscule, un chiffre et un caractere special"

### AC8: RGPD Consent Required
**And** if RGPD consent is not checked, registration is blocked with message: "Vous devez accepter la politique de confidentialite"

---

## Tasks / Subtasks

- [ ] **Task 1: Create Registration Page UI** (AC: #1)
  - [ ] 1.1 Create `/app/(auth)/register/page.tsx`
  - [ ] 1.2 Build form with shadcn/ui Form component
  - [ ] 1.3 Add Company name input field
  - [ ] 1.4 Add Email input field with validation
  - [ ] 1.5 Add Password input field with show/hide toggle
  - [ ] 1.6 Add Password confirmation field
  - [ ] 1.7 Add RGPD consent checkbox with link to privacy policy
  - [ ] 1.8 Style form for mobile-first responsive design

- [ ] **Task 2: Implement Form Validation with Zod** (AC: #6, #7, #8)
  - [ ] 2.1 Create registration schema in `lib/validations/auth.ts`
  - [ ] 2.2 Validate email format
  - [ ] 2.3 Validate password strength (12+ chars, uppercase, lowercase, number, special)
  - [ ] 2.4 Validate password confirmation matches
  - [ ] 2.5 Validate RGPD consent is checked
  - [ ] 2.6 Display French error messages

- [ ] **Task 3: Create Registration API Endpoint** (AC: #1, #2)
  - [ ] 3.1 Configure Better Auth for email/password registration
  - [ ] 3.2 Create organization in database first
  - [ ] 3.3 Create user with ADMIN role linked to organization
  - [ ] 3.4 Hash password with bcrypt cost factor 12
  - [ ] 3.5 Store RGPD consent timestamp

- [ ] **Task 4: Implement Auto-Login** (AC: #3, #5)
  - [ ] 4.1 Create session after successful registration
  - [ ] 4.2 Set secure session cookie (httpOnly, secure, sameSite=strict)
  - [ ] 4.3 Configure 24h session expiration for ADMIN

- [ ] **Task 5: Implement Redirect** (AC: #4)
  - [ ] 5.1 Redirect to `/dashboard` after successful registration
  - [ ] 5.2 Handle redirect with Next.js router

- [ ] **Task 6: Email Uniqueness Check** (AC: #6)
  - [ ] 6.1 Query database before registration
  - [ ] 6.2 Return error if email exists
  - [ ] 6.3 Display French error message

- [ ] **Task 7: Create Basic Dashboard Page** (Placeholder)
  - [ ] 7.1 Create `/app/(dashboard)/dashboard/page.tsx`
  - [ ] 7.2 Add protected route middleware
  - [ ] 7.3 Display welcome message with organization name

- [ ] **Task 8: Testing** (Validation)
  - [ ] 8.1 Test successful registration flow
  - [ ] 8.2 Test validation errors display
  - [ ] 8.3 Test duplicate email rejection
  - [ ] 8.4 Test session creation and cookie

---

## Dev Notes

### Dependencies on Previous Stories

- **Story 1.1**: Project initialized with Better Auth starter
- **Story 1.2**: Multi-tenant schema with Organization and User models

### Better Auth Registration Configuration

```typescript
// lib/auth.ts
import { betterAuth } from 'better-auth';
import { prismaAdapter } from 'better-auth/adapters/prisma';
import { organization } from '@better-auth/organization';

export const auth = betterAuth({
  database: prismaAdapter(prisma, {
    provider: 'postgresql',
  }),
  emailAndPassword: {
    enabled: true,
    requireEmailVerification: false, // MVP: skip email verification
    password: {
      minLength: 12,
      maxLength: 128,
    },
  },
  session: {
    expiresIn: 60 * 60 * 24, // 24 hours
    cookieCache: {
      enabled: true,
      maxAge: 60 * 5, // 5 minutes
    },
  },
  plugins: [
    organization(),
  ],
});
```

### Zod Validation Schema

```typescript
// lib/validations/auth.ts
import { z } from 'zod';

export const registerSchema = z.object({
  companyName: z.string()
    .min(2, 'Le nom de l\'entreprise doit contenir au moins 2 caracteres')
    .max(100),
  email: z.string()
    .email('Format d\'email invalide'),
  password: z.string()
    .min(12, 'Le mot de passe doit contenir au moins 12 caracteres')
    .regex(/[A-Z]/, 'Le mot de passe doit contenir au moins une majuscule')
    .regex(/[a-z]/, 'Le mot de passe doit contenir au moins une minuscule')
    .regex(/[0-9]/, 'Le mot de passe doit contenir au moins un chiffre')
    .regex(/[^A-Za-z0-9]/, 'Le mot de passe doit contenir au moins un caractere special'),
  confirmPassword: z.string(),
  rgpdConsent: z.literal(true, {
    errorMap: () => ({ message: 'Vous devez accepter la politique de confidentialite' }),
  }),
}).refine((data) => data.password === data.confirmPassword, {
  message: 'Les mots de passe ne correspondent pas',
  path: ['confirmPassword'],
});
```

### Registration API Flow

1. Validate form data with Zod
2. Check email uniqueness in database
3. Create Organization record
4. Hash password with bcrypt (cost 12)
5. Create User record with ADMIN role
6. Store RGPD consent timestamp
7. Create session via Better Auth
8. Set secure cookie
9. Return success with redirect URL

### UI Components (shadcn/ui)

```bash
# Install required components
pnpm dlx shadcn@latest add form input button checkbox label card
```

### Security Considerations

- **bcrypt cost factor 12**: Balance between security and performance
- **Secure cookie flags**: httpOnly prevents XSS, secure requires HTTPS, sameSite prevents CSRF
- **Password validation**: Enforced on both client and server
- **RGPD consent**: Timestamp stored for compliance audit

### French Error Messages

| Error | Message |
|-------|---------|
| Email exists | "Cet email est deja utilise" |
| Password weak | "Le mot de passe doit contenir au moins 12 caracteres, une majuscule, une minuscule, un chiffre et un caractere special" |
| RGPD not checked | "Vous devez accepter la politique de confidentialite" |
| Passwords don't match | "Les mots de passe ne correspondent pas" |

---

### Project Structure Notes

- **Fichiers a creer:**
  - `app/(auth)/register/page.tsx` - Registration page
  - `lib/validations/auth.ts` - Zod schemas
  - `app/(dashboard)/dashboard/page.tsx` - Dashboard placeholder
  - `app/(dashboard)/layout.tsx` - Protected layout

- **Fichiers a modifier:**
  - `lib/auth.ts` - Better Auth configuration

---

### References

- [Source: architecture.md#Authentication] - Better Auth configuration
- [Source: prd.md#Authentification] - Email/password + RGPD consent
- [Source: ux-design-specification.md#Onboarding] - Registration UX patterns
- [Source: epics.md#Story 1.3] - Acceptance criteria originaux

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
