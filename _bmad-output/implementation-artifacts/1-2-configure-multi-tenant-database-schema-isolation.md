# Story 1.2: Configure Multi-Tenant Database Schema & Isolation

Status: ready-for-dev

---

## Story

As a developer,
I want to configure the database schema with multi-tenant isolation using Prisma middleware,
So that each company's data is completely isolated and no cross-tenant data leakage is possible.

---

## Acceptance Criteria

### AC1: Base Multi-Tenant Schema
**Given** Prisma is initialized from Story 1.1
**When** I define the base multi-tenant schema
**Then** the Prisma schema includes:
- `Organization` model with fields: id (UUID), name, createdAt, updatedAt
- `User` model with fields: id (UUID), organizationId (FK), email (nullable), username (nullable), role (enum: ADMIN, MANAGER, DRIVER), passwordHash, phone, name, firstName, createdAt, updatedAt
- Unique constraint on `(organizationId, email)` for users with email
- Unique constraint on `(organizationId, username)` for users with username

### AC2: Prisma Middleware Configuration
**And** Prisma middleware is configured in `lib/db.ts` to:
- Automatically inject `organizationId` filter on all READ queries
- Require explicit `organizationId` in all UPDATE/DELETE WHERE clauses
- Block any query attempting to access data without organizationId context

### AC3: Multi-Tenant Isolation Tests
**And** I can run a test query that verifies:
- Query without orgId context throws error
- Query with orgId only returns data for that organization
- Attempting to query another org's data returns empty results

### AC4: Migrations Success
**And** database migrations are generated and applied successfully

### AC5: Prisma Client Regeneration
**And** Prisma Client is regenerated with the new schema types

---

## Tasks / Subtasks

- [ ] **Task 1: Install Better Auth Organization Plugin** (AC: #1)
  - [ ] 1.1 Run `pnpm add @better-auth/organization`
  - [ ] 1.2 Verify installation in package.json
  - [ ] 1.3 Check Better Auth documentation for Organization plugin setup

- [ ] **Task 2: Define Organization Model** (AC: #1)
  - [ ] 2.1 Add Organization model to `prisma/schema.prisma`
  - [ ] 2.2 Define fields: id (UUID with @default(cuid())), name (String), createdAt, updatedAt
  - [ ] 2.3 Add relation to User model

- [ ] **Task 3: Update User Model for Multi-Tenancy** (AC: #1)
  - [ ] 3.1 Add organizationId field (FK to Organization)
  - [ ] 3.2 Add role enum: ADMIN, MANAGER, DRIVER
  - [ ] 3.3 Make email nullable (chauffeurs use username)
  - [ ] 3.4 Add username field (nullable)
  - [ ] 3.5 Add phone, name, firstName fields
  - [ ] 3.6 Add unique constraint on (organizationId, email)
  - [ ] 3.7 Add unique constraint on (organizationId, username)

- [ ] **Task 4: Create Role Enum** (AC: #1)
  - [ ] 4.1 Define UserRole enum in schema.prisma
  - [ ] 4.2 Values: ADMIN, MANAGER, DRIVER

- [ ] **Task 5: Configure Prisma Multi-Tenant Middleware** (AC: #2)
  - [ ] 5.1 Create or update `lib/db.ts` with Prisma client singleton
  - [ ] 5.2 Implement middleware for automatic organizationId injection on READ
  - [ ] 5.3 Implement middleware to require organizationId on UPDATE/DELETE
  - [ ] 5.4 Implement context helper to set current organizationId
  - [ ] 5.5 Add error handling for queries without orgId context

- [ ] **Task 6: Generate and Apply Migrations** (AC: #4, #5)
  - [ ] 6.1 Run `pnpm prisma generate` to regenerate client
  - [ ] 6.2 Run `pnpm prisma db push` to apply schema changes
  - [ ] 6.3 Verify no migration errors
  - [ ] 6.4 Verify Prisma Client types include new models

- [ ] **Task 7: Create Isolation Test Script** (AC: #3)
  - [ ] 7.1 Create test file `scripts/test-multi-tenant.ts`
  - [ ] 7.2 Test: Query without orgId context throws error
  - [ ] 7.3 Test: Query with orgId returns only that org's data
  - [ ] 7.4 Test: Cannot access another org's data
  - [ ] 7.5 Document test results

- [ ] **Task 8: Verify Build** (Validation)
  - [ ] 8.1 Run `pnpm run build` successfully
  - [ ] 8.2 Ensure no TypeScript errors with new schema types
  - [ ] 8.3 Verify Better Auth integration still works

---

## Dev Notes

### Dependency on Story 1.1

Cette story depend de la completion de Story 1.1 qui initialise:
- Le projet avec le starter Better Auth
- Prisma configure avec PostgreSQL
- La structure de base du projet

### Better Auth Organization Plugin

Le plugin Organization de Better Auth fournit:
- Gestion des organisations (tenants)
- Roles et permissions par organisation
- Team invitations

**Installation:**
```bash
pnpm add @better-auth/organization
```

**Configuration dans `lib/auth.ts`:**
```typescript
import { betterAuth } from 'better-auth';
import { organization } from '@better-auth/organization';

export const auth = betterAuth({
  // ... config existante
  plugins: [
    organization({
      // Configuration multi-tenant
    })
  ]
});
```

### Prisma Schema Multi-Tenant

**Schema a implementer:**

```prisma
// prisma/schema.prisma

enum UserRole {
  ADMIN
  MANAGER
  DRIVER
}

model Organization {
  id        String   @id @default(cuid())
  name      String
  users     User[]
  createdAt DateTime @default(now())
  updatedAt DateTime @updatedAt
}

model User {
  id             String       @id @default(cuid())
  organizationId String
  organization   Organization @relation(fields: [organizationId], references: [id])
  email          String?
  username       String?
  role           UserRole     @default(DRIVER)
  passwordHash   String?
  phone          String?
  name           String?
  firstName      String?
  createdAt      DateTime     @default(now())
  updatedAt      DateTime     @updatedAt

  @@unique([organizationId, email])
  @@unique([organizationId, username])
  @@index([organizationId])
}
```

### Prisma Middleware Pattern

**Implementation dans `lib/db.ts`:**

```typescript
import { PrismaClient } from '@prisma/client';

// Context pour stocker l'organizationId courant
export const tenantContext = {
  organizationId: null as string | null,
};

const prismaClientSingleton = () => {
  const prisma = new PrismaClient();

  // Middleware pour injection automatique organizationId
  prisma.$use(async (params, next) => {
    // Models qui necessitent isolation multi-tenant
    const tenantModels = ['User', 'Vehicle', 'Document'];

    if (tenantModels.includes(params.model || '')) {
      // READ queries: injecter filtre organizationId
      if (params.action === 'findMany' || params.action === 'findFirst') {
        if (!tenantContext.organizationId) {
          throw new Error('Organization context required for query');
        }
        params.args = params.args || {};
        params.args.where = {
          ...params.args.where,
          organizationId: tenantContext.organizationId,
        };
      }

      // UPDATE/DELETE: verifier organizationId dans WHERE
      if (params.action === 'update' || params.action === 'delete') {
        if (!params.args?.where?.organizationId) {
          throw new Error('organizationId required in WHERE clause');
        }
      }
    }

    return next(params);
  });

  return prisma;
};

// Singleton pattern
const globalForPrisma = globalThis as unknown as {
  prisma: ReturnType<typeof prismaClientSingleton> | undefined;
};

export const prisma = globalForPrisma.prisma ?? prismaClientSingleton();

if (process.env.NODE_ENV !== 'production') {
  globalForPrisma.prisma = prisma;
}

// Helper pour definir le contexte tenant
export function withTenant<T>(orgId: string, fn: () => T): T {
  const previousOrgId = tenantContext.organizationId;
  tenantContext.organizationId = orgId;
  try {
    return fn();
  } finally {
    tenantContext.organizationId = previousOrgId;
  }
}
```

### Test Multi-Tenant Isolation

**Script de test `scripts/test-multi-tenant.ts`:**

```typescript
import { prisma, withTenant, tenantContext } from '../lib/db';

async function testMultiTenantIsolation() {
  console.log('Testing multi-tenant isolation...');

  // Test 1: Query sans context doit echouer
  try {
    tenantContext.organizationId = null;
    await prisma.user.findMany();
    console.error('FAIL: Query without orgId should throw');
  } catch (e) {
    console.log('PASS: Query without orgId throws error');
  }

  // Test 2: Query avec context retourne uniquement les donnees de l'org
  const org1Id = 'test-org-1';
  const org2Id = 'test-org-2';

  const users = await withTenant(org1Id, () =>
    prisma.user.findMany()
  );

  const allBelongToOrg = users.every(u => u.organizationId === org1Id);
  console.log(allBelongToOrg
    ? 'PASS: All returned users belong to org1'
    : 'FAIL: Data leak detected');

  // Test 3: Ne peut pas acceder aux donnees d'une autre org
  const otherOrgUsers = await withTenant(org1Id, () =>
    prisma.user.findMany({ where: { organizationId: org2Id } })
  );

  console.log(otherOrgUsers.length === 0
    ? 'PASS: Cannot access other org data'
    : 'FAIL: Cross-tenant access possible');
}

testMultiTenantIsolation().catch(console.error);
```

### Architecture Context (from Story 1.1)

- **ORM**: Prisma avec PostgreSQL adapter
- **Database**: OVH Managed PostgreSQL (France)
- **Auth**: Better Auth avec Organization plugin
- **Roles**: ADMIN (acces total), MANAGER (gestion vehicules/docs/chauffeurs), DRIVER (scan uniquement)

### Security Considerations

1. **Isolation stricte**: Aucune donnee d'un tenant ne doit etre accessible par un autre
2. **Index sur organizationId**: Performance des requetes filtrees
3. **Middleware obligatoire**: Toute requete sans context org est bloquee
4. **Logs d'audit**: Preparer pour Story future sur audit logs

### Future Models (Epic 2+)

Les models suivants utiliseront le meme pattern multi-tenant:
- `Vehicle` (Epic 2)
- `Document` (Epic 4)
- `Alert` (Epic 7)
- `AuditLog` (Epic 13)

Tous auront `organizationId` et seront proteges par le middleware.

---

### Project Structure Notes

- **Fichiers a creer/modifier:**
  - `prisma/schema.prisma` - Ajout Organization, modification User
  - `lib/db.ts` - Prisma client avec middleware multi-tenant
  - `scripts/test-multi-tenant.ts` - Script de test isolation

- **Alignement architecture**: Conforme a architecture.md section "Cross-Cutting Concerns" - Isolation multi-tenant stricte

---

### References

- [Source: architecture.md#Cross-Cutting Concerns] - Isolation multi-tenant stricte, indexes DB
- [Source: architecture.md#Better Auth Organization Plugin] - Configuration multi-tenant RBAC
- [Source: architecture.md#Extend Prisma Schema] - Schema models de reference
- [Source: epics.md#Story 1.2] - Acceptance criteria originaux
- [Source: Story 1.1] - Prerequis et context technique

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
