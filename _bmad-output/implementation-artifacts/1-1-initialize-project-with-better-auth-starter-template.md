# Story 1.1: Initialize Project with Better Auth Starter Template

Status: ready-for-dev

---

## Story

As a developer,
I want to initialize the project using the Better Auth starter template with all required dependencies,
So that the foundation is set with Next.js 15+, Prisma, Better Auth, and shadcn/ui pre-configured.

---

## Acceptance Criteria

### AC1: Clone and Initialize Better Auth Starter Template
**Given** the Better Auth starter template repository (https://github.com/devAaus/better-auth)
**When** I clone and initialize the project
**Then** the project structure includes:
- Next.js 15+ App Router configured with TypeScript strict mode
- Better Auth library installed and base configuration present
- Prisma ORM with PostgreSQL adapter configured
- shadcn/ui component library installed with TailwindCSS 4.x
- `.env.example` file with required environment variables template

### AC2: Dependencies Installation
**And** I can run `pnpm install` successfully without errors

### AC3: Build Success
**And** I can run `pnpm run build` and the project compiles without TypeScript errors

### AC4: Environment Variables Configuration
**And** `.env` file is configured with:
- `DATABASE_URL` pointing to OVH PostgreSQL instance
- `BETTER_AUTH_SECRET` generated and set
- `GOOGLE_CLIENT_ID` and `GOOGLE_CLIENT_SECRET` placeholders added

### AC5: Prisma Generation
**And** Prisma migrations can be generated with `pnpm prisma generate`

### AC6: Database Push
**And** database schema can be pushed with `pnpm prisma db push`

### AC7: Development Server Starts
**And** the development server starts successfully on `http://localhost:3000`

---

## Tasks / Subtasks

- [ ] **Task 1: Clone Better Auth Starter** (AC: #1)
  - [ ] 1.1 Clone repository: `git clone https://github.com/devAaus/better-auth.git flottebox-mvp`
  - [ ] 1.2 Navigate to project directory
  - [ ] 1.3 Verify project structure matches expected layout

- [ ] **Task 2: Install Dependencies** (AC: #2)
  - [ ] 2.1 Run `pnpm install`
  - [ ] 2.2 Verify no errors during installation
  - [ ] 2.3 Check `node_modules` is properly populated

- [ ] **Task 3: Upgrade ALL Dependencies to Latest Versions (2026)** (AC: #1, #3)
  - [ ] 3.1 Upgrade Next.js & React: `pnpm add next@16 react@19 react-dom@19`
  - [ ] 3.2 Upgrade TypeScript: `pnpm add -D typescript@latest @types/react@latest @types/node@latest`
  - [ ] 3.3 Upgrade TailwindCSS v4: `pnpm add tailwindcss@4 @tailwindcss/postcss` (Note: Tailwind v4 a une nouvelle config)
  - [ ] 3.4 Upgrade Prisma: `pnpm add @prisma/client@latest prisma@latest`
  - [ ] 3.5 Upgrade Better Auth: `pnpm add better-auth@latest`
  - [ ] 3.6 Upgrade shadcn/ui components: `pnpm dlx shadcn@latest update`
  - [ ] 3.7 Upgrade all other dependencies: `pnpm update --latest`
  - [ ] 3.8 Fix any breaking changes from upgrades
  - [ ] 3.9 Verify build still works after ALL upgrades

- [ ] **Task 4: Configure Environment Variables** (AC: #4)
  - [ ] 4.1 Copy `.env.example` to `.env`
  - [ ] 4.2 Generate `BETTER_AUTH_SECRET` using secure method
  - [ ] 4.3 Configure `DATABASE_URL` for OVH PostgreSQL (placeholder format for now)
  - [ ] 4.4 Add `GOOGLE_CLIENT_ID` and `GOOGLE_CLIENT_SECRET` placeholders
  - [ ] 4.5 Ensure `.env` is in `.gitignore`

- [ ] **Task 5: Initialize Prisma** (AC: #5, #6)
  - [ ] 5.1 Run `pnpm prisma generate` successfully
  - [ ] 5.2 Run `pnpm prisma db push` (with local or OVH PostgreSQL)
  - [ ] 5.3 Verify Prisma Client types are generated

- [ ] **Task 6: Verify Build** (AC: #3)
  - [ ] 6.1 Run `pnpm run build`
  - [ ] 6.2 Ensure no TypeScript errors
  - [ ] 6.3 Ensure no build warnings (or document acceptable ones)

- [ ] **Task 7: Start Development Server** (AC: #7)
  - [ ] 7.1 Run `pnpm run dev`
  - [ ] 7.2 Verify server starts on `http://localhost:3000`
  - [ ] 7.3 Verify basic pages load (login, register if present)

- [ ] **Task 8: Rename Project** (Housekeeping)
  - [ ] 8.1 Update `package.json` name to "flottebox-mvp"
  - [ ] 8.2 Update any other references to project name
  - [ ] 8.3 Initialize new git repository for FlotteBox project

---

## Dev Notes

### Why Better Auth Starter?

Cette story utilise le starter template Better Auth (devAaus) pour les raisons suivantes:
1. **Stack 100% alignee**: Next.js App Router + Better Auth + Prisma + shadcn/ui + TailwindCSS
2. **Better Auth natif**: Authentification moderne avec support multi-tenant via Organization plugin
3. **DX TypeScript superieur**: Meilleure experience de developpement vs NextAuth.js/Auth.js
4. **Support credentials custom**: Parfait pour onboarding chauffeurs sans email

### Architecture Context

**Stack confirmee pour FlotteBox:**
- **Framework**: Next.js 16 (App Router) + React 19 + TypeScript
- **Styling**: TailwindCSS + shadcn/ui
- **Database**: PostgreSQL (OVH Managed Database)
- **ORM**: Prisma
- **Authentication**: Better Auth (avec Organization plugin pour multi-tenant RBAC)
- **Hosting**: Vercel (frontend/backend) + OVH (database + object storage)

**Versions cibles (2026):**
- Next.js 16.x (App Router stable)
- React 19.x (avec React Compiler)
- TypeScript 5.7+
- TailwindCSS 4.x (nouvelle architecture CSS-first)
- Prisma 6.x
- Better Auth latest
- shadcn/ui latest

> **IMPORTANT**: Le starter template sera probablement sur des versions anterieures.
> Il est CRITIQUE de mettre a jour TOUTES les dependances vers les dernieres versions
> avant de commencer le developpement pour eviter des problemes de compatibilite plus tard.

### Project Structure Reference

Le projet doit avoir cette structure apres initialisation:

```
flottebox-mvp/
├── app/                    # Next.js App Router
│   ├── (auth)/            # Auth routes (login, register)
│   ├── (dashboard)/       # Protected dashboard routes
│   ├── api/               # API routes (Better Auth endpoints)
│   └── layout.tsx         # Root layout
├── components/            # React components
│   ├── ui/               # shadcn/ui components
│   └── ...               # Custom components
├── lib/                   # Utilities
│   ├── auth.ts           # Better Auth config
│   ├── db.ts             # Prisma client
│   └── utils.ts          # Helpers
├── prisma/
│   └── schema.prisma     # Database schema
└── public/               # Static assets
```

### Environment Variables Template

```bash
# Database (OVH PostgreSQL)
DATABASE_URL="postgresql://user:password@postgres.ovh.net:5432/flottebox?schema=public"

# Better Auth
BETTER_AUTH_SECRET="generate-secure-secret-here"

# OAuth Google (pour Story 1.4)
GOOGLE_CLIENT_ID="your-google-client-id"
GOOGLE_CLIENT_SECRET="your-google-client-secret"

# Base URL
NEXT_PUBLIC_APP_URL="http://localhost:3000"
```

### Initialization Commands Sequence

```bash
# 1. Cloner le starter
git clone https://github.com/devAaus/better-auth.git flottebox-mvp
cd flottebox-mvp

# 2. Installer dependances initiales
pnpm install

# ============================================================
# 3. UPGRADE COMPLET VERS VERSIONS 2026 (CRITIQUE!)
# ============================================================
# Le starter est probablement sur des versions anterieures.
# Mettre a jour TOUT avant de commencer le dev.

# 3a. Core framework
pnpm add next@16 react@19 react-dom@19

# 3b. TypeScript et types
pnpm add -D typescript@latest @types/react@latest @types/node@latest

# 3c. TailwindCSS v4 (nouvelle architecture!)
pnpm add tailwindcss@4 @tailwindcss/postcss
# Note: Tailwind v4 utilise CSS-first config, peut necessiter migration
# Voir: https://tailwindcss.com/docs/upgrade-guide

# 3d. Prisma
pnpm add @prisma/client@latest
pnpm add -D prisma@latest

# 3e. Better Auth
pnpm add better-auth@latest

# 3f. shadcn/ui (mettre a jour les composants existants)
pnpm dlx shadcn@latest update

# 3g. Toutes les autres dependances
pnpm update --latest

# 3h. Verifier et fixer les breaking changes
pnpm run build
# Si erreurs, consulter les changelogs et fixer

# ============================================================
# 4. Configuration initiale
# ============================================================
cp .env.example .env
# Configurer DATABASE_URL avec OVH PostgreSQL
# Configurer BETTER_AUTH_SECRET
# Configurer GOOGLE_CLIENT_ID + GOOGLE_CLIENT_SECRET

# 5. Initialiser Prisma
pnpm prisma generate
pnpm prisma db push

# 6. Demarrer le serveur de developpement
pnpm run dev
```

### Notes sur TailwindCSS v4

TailwindCSS v4 introduit des changements majeurs:
- **CSS-first configuration**: Plus de `tailwind.config.js`, config dans CSS
- **Nouvelle syntaxe**: `@import "tailwindcss"` au lieu de `@tailwind base/components/utilities`
- **PostCSS simplifie**: Utilise `@tailwindcss/postcss`

Si le starter utilise Tailwind v3, il faudra peut-etre:
1. Migrer la config vers le nouveau format CSS
2. Mettre a jour les imports dans `globals.css`
3. Verifier que shadcn/ui est compatible

Consulter le guide de migration officiel si necessaire.

### Critical Technical Constraints

1. **Hebergement France OBLIGATOIRE**: Donnees sensibles doivent etre hebergees en France (OVH)
2. **DX optimal**: Deploiement type Vercel (push sur main -> auto-deploy)
3. **Cout minimum MVP**: Infrastructure ~20-30€/mois phase MVP

### Post-Initialization Setup Required (Stories suivantes)

Les elements suivants seront configures dans les stories suivantes de l'Epic 1:

1. **Story 1.2**: Better Auth Organization Plugin pour multi-tenant RBAC
2. **Story 1.3**: Email/Password authentication flow
3. **Story 1.4**: OAuth Google provider configuration
4. **Story 1.5**: Session management et login flow
5. **Story 1.6**: RGPD consent management
6. **Story 1.7**: 2FA configuration pour admins

### Testing Standards

Pour cette story, les validations sont principalement manuelles:
- Build sans erreurs TypeScript
- Serveur de developpement demarre
- Pages de base accessibles

Les tests automatises seront ajoutes dans les stories suivantes avec Vitest + Testing Library + Playwright (e2e).

---

### Project Structure Notes

- **Alignement architecture**: 100% aligne avec architecture.md - starter selectionne specifiquement pour FlotteBox
- **Naming conventions**: Suivre conventions TypeScript strictes deja configurees dans le starter
- **Folder structure**: Structure App Router standard, extensible pour modules FlotteBox

---

### References

- [Source: architecture.md#Starter Template Evaluation] - Selection du starter Better Auth
- [Source: architecture.md#Selected Starter: Better Auth Starter] - Rationale et commandes d'initialisation
- [Source: architecture.md#Post-Initialization Setup Required] - Setup requis apres initialisation
- [Source: architecture.md#Core Architectural Decisions] - Decisions techniques
- [Source: epics.md#Story 1.1] - Acceptance criteria originaux

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
