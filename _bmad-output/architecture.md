---
stepsCompleted: [1, 2, 3, 4, 5]
inputDocuments:
  - _bmad-output/prd.md
  - _bmad-output/project-planning-artifacts/ux-design-specification.md
workflowType: 'architecture'
project_name: 'FlotteBox'
user_name: 'Quentin'
date: '2026-01-08'
---

# Architecture Decision Document

_This document builds collaboratively through step-by-step discovery. Sections are appended as we work through each architectural decision together._

## Project Context Analysis

### Requirements Overview

**Functional Requirements (P0 MVP):**

FlotteBox est une application SaaS B2B de gestion documentaire pour flottes de véhicules (10-200 véhicules). Les fonctionnalités cœur architecturalement significatives :

1. **Authentification multi-rôles avec onboarding chauffeurs sans email**
   - OAuth Google + Email/Password via NextAuth.js
   - 3 rôles : Admin, Gestionnaire, Chauffeur (RBAC strict)
   - **Onboarding chauffeurs** : Gestionnaire crée compte → Identifiant custom + password générés → SMS automatique (identifiant + password + lien PWA) → Chauffeur clique lien → Install PWA → Login
   - SMS d'onboarding : Service OVH SMS (0,035€/SMS) avec **limite anti-abus** (rate limiting, captcha) pour éviter explosion coûts

2. **Gestion véhicules avec distinction moteur/remorque**
   - CRUD + Import CSV masse (50 véhicules en <2h)
   - **Pricing différencié évolutif** : Moteurs (4€ → 3€ → 2,50€ selon volume) vs Remorques (1,50€ fixe) → logique métier dans facturation avec paliers dégressifs
   - **Association chauffeur → véhicule** : Gestionnaire assigne manuellement chauffeurs aux véhicules (pas automatique)

3. **Upload documents + OCR assisté avec validation humaine obligatoire**
   - Dual-platform : Drag & drop desktop + scan caméra mobile PWA
   - Workflow OCR critique : scan → pré-remplissage auto → validation humaine obligatoire → enregistrement
   - OCR externe : Mistral 3 API ($2/1000 pages)
   - Gestion erreurs : documents illisibles marqués "à vérifier" + notification gestionnaire
   - **Mode offline-first mobile** : Scan sans réseau + sync automatique au retour (Service Workers + IndexedDB)

4. **Onboarding chauffeurs (CRITIQUE pour adoption >60%)**
   - SMS automatique avec identifiant + password + lien PWA (OVH SMS avec limite anti-abus)
   - Vidéo tutoriel 90 sec à la première connexion (rejouable)
   - QR code installation PWA depuis dashboard gestionnaire
   - Premier scan guidé avec tooltips interactifs
   - Gamification : badges "Chauffeur exemplaire", classements adoption
   - Support smartphones anciens (Android 7-8) → optimisation performance impérative

5. **Système d'alertes par email uniquement (P0 MVP)**
   - Calcul temps réel échéances : J-60, J-30, J-15, expiration
   - **Alertes email uniquement** : 3 adresses configurables (opérationnel, facturation, rapports)
   - Email de test après configuration
   - **Pas de push PWA en P0** : Gestionnaires sont des gens de bureau avec boîte mail
   - **Post-MVP P1** : SMS alertes échéances (add-on payant 0,50€/véh/mois)

6. **Dashboard conformité temps réel**
   - Statuts visuels type Google Maps (vert OK / orange surveillance / rouge critique)
   - Distinction visuelle moteurs vs remorques (icônes, codes couleur)
   - Calendrier échéances interactif
   - Métriques adoption chauffeurs en temps réel

7. **Facturation LemonSqueezy avec pricing dynamique évolutif**
   - Abonnement avec gestion TVA automatique
   - Pricing différencié avec paliers dégressifs :
     - Moteurs : 1-25 véh (4€) | 26-100 véh (3€) | 101+ véh (2,50€)
     - Remorques : 1,50€ fixe
   - Add-ons ligne séparée : OCR (0,10€/doc)
   - Essai 14 jours avec CB obligatoire (auto-renouvellement)

**Non-Functional Requirements (NFRs):**

**Performance :**
- Dashboard conformité : chargement <2 secondes (impératif)
- Scan mobile end-to-end : <30 secondes (critique adoption chauffeurs)
- Support smartphones anciens : Android 7-8 avec performances acceptables
- OCR processing : <5 secondes par document

**Sécurité & Conformité RGPD :**
- **Hébergement France OBLIGATOIRE dès MVP** (Scaleway ou OVH Cloud)
- Documents sensibles (cartes grises, permis conducteurs) → chiffrement au repos et en transit
- Traçabilité complète : logs d'audit (qui a consulté/modifié quoi, quand)
- Accès lecture seule pour tiers (comptables, auditeurs) - **P1 post-MVP**
- Conservation légale 10 ans automatique
- Isolation multi-tenant stricte (données clients jamais mélangées)
- ISO 27001 post-MVP si >50% bêta-testeurs l'exigent (à valider M3)

**Disponibilité & Résilience :**
- **Mode offline-first mobile** : Chauffeurs terrain en zones blanches → scan fonctionnel sans réseau
- Sync automatique intelligente au retour réseau (gestion conflits, retry, idempotence)
- PWA installable (pas d'app stores iOS/Android)
- Uptime cible : 99.5% (MVP) → 99.9% (post-MVP)

**Scalabilité :**
- Cible M12 : 40-50 clients, ~1500-2500 véhicules, ~500-1000 documents/mois
- Architecture découplée avec ORM (Prisma/Drizzle) pour faciliter migration hébergeur
- Monitoring proactif : alertes si temps réponse >2s ou charge >70%
- Seuils migration : >100 clients → évaluer infrastructure dédiée

**Utilisabilité (UX-driven NFRs) :**
- Onboarding chauffeur : <30 secondes (SMS reçu → install PWA → login)
- Workflow scan : max 3 clics (règle stricte)
- Feedback immédiat : animations, confirmations visuelles
- Grandes cibles tactiles : 48×48px minimum (chauffeurs 50-60 ans)
- Accessibility : WCAG 2.1 AA minimum

### Scale & Complexity

**Complexité évaluée : MOYENNE-HAUTE**

- **Domaine technique primaire** : Full-stack SaaS B2B (PWA mobile-first + API backend + Storage + OCR externe)
- **Niveau de complexité** : Moyenne-haute (7/10)
- **Composants architecturaux estimés** : 8-12 composants majeurs

**Indicateurs de complexité :**
- ✅ Multi-tenancy avec RBAC (3 rôles : Admin, Gestionnaire, Chauffeur)
- ✅ Mode offline-first avec sync bidirectionnelle complexe
- ✅ OCR workflow avec validation humaine (état transitionnel critique)
- ✅ Conformité RGPD stricte + hébergement France dès MVP
- ✅ Dual-platform optimisée : PWA mobile (offline) + desktop (bulk operations)
- ✅ Facturation dynamique différenciée avec paliers dégressifs
- ✅ Système d'alertes email multi-destinataires (opérationnel/facturation/rapports)
- ✅ Gamification + analytics adoption temps réel

**Phase MVP (M0-M2) :**
- 10 entreprises bêta-testeuses
- ~300-500 véhicules totaux
- ~100-200 documents scannés/mois
- Charge faible, focus stabilité et UX

**Phase Croissance (M6-M12) :**
- 40-50 clients actifs
- ~1500-2500 véhicules
- ~500-1000 documents/mois
- Monitoring scalabilité critique

### Technical Constraints & Dependencies

**Contraintes d'hébergement et coûts :**
- ✅ **Hébergement France OBLIGATOIRE dès MVP** (collectivités locales, souveraineté données)
- ⚠️ **Contrainte coût critique** : Aucun client actuellement → infrastructure doit coûter **le minimum absolu**
- ⚠️ **DX critique** : Besoin simplicité déploiement type Vercel (push sur main → mise en ligne automatique)
- Stack habituelle : Vercel + Supabase (gratuit, zéro friction) → **pas d'option ici car hébergement France obligatoire**
- **Défi architectural** : Trouver équivalent "simple + pas cher + France" alors que Scaleway/OVH = setup manuel + coûts dès J1

**Options hébergement à évaluer :**
- Option 1 : Vercel (frontend) + Supabase (DB) → ❌ DB hébergée US (non conforme)
- Option 2 : Vercel (frontend) + Scaleway/OVH Managed DB → Coût ~50-80€/mois dès MVP
- Option 3 : Full Scaleway/OVH (frontend + backend + DB) → Setup complexe, coûts ~50-80€/mois
- Option 4 : Attendre clients avant France ? → ❌ Argument commercial perdu dès MVP
- **À décider dans step architecture** : Arbitrage coût vs conformité vs DX

**Stack technique proposée (à valider) :**
- **Frontend** : Next.js 16 + React 19 + TypeScript
- **Backend** : Next.js API Routes (monolithe initial) ou découplé ?
- **Database** : PostgreSQL (où ? Supabase US interdit, Scaleway/OVH coûteux)
- **ORM** : Prisma ou Drizzle (abstraction DB pour anti-lock-in)
- **Auth** : NextAuth.js (onboarding chauffeurs : gestionnaire crée identifiant/password → envoi SMS avec limite anti-abus)
- **Storage** : Scaleway Object Storage ou OVH Object Storage (documents PDF/images)
- **OCR** : Mistral OCR 3 via API ($2/1000 pages) vs alternatives (Google Vision $1,50, AWS Textract $1,50, Tesseract open-source gratuit)
- **Paiements** : LemonSqueezy avec gestion TVA automatique (pricing différencié avec paliers dégressifs supporté ?)
- **Emails** : Resend
- **SMS** : OVH SMS (0,035€/SMS) - uniquement onboarding chauffeurs avec **rate limiting strict** (anti-abus)
- **Hosting** : À décider selon contraintes coût + France + DX

**Dépendances externes critiques :**
- Service OCR (Mistral 3 ou équivalent) → SLA requis, fallback strategy
- LemonSqueezy : pricing différencié avec paliers dégressifs + add-ons dynamiques (à valider faisabilité)
- OVH SMS : taux de délivrabilité >95% + mécanisme anti-abus (rate limiting, captcha)
- Service Workers : support Android 7-8 avec performances acceptables (à tester)

**Contraintes UX-driven :**
- PWA installable : Manifest + Service Workers + HTTPS obligatoires
- Camera API native : support multi-browsers (Safari iOS, Chrome Android)
- Mode offline : IndexedDB pour stockage local documents scannés (quelle limite volumétrie ?)

### Cross-Cutting Concerns Identified

**1. Sécurité & Conformité RGPD**
- Chiffrement documents sensibles au repos (AES-256) et en transit (TLS 1.3)
- Gestion accès granulaire RBAC (Admin peut tout, Gestionnaire gère véhicules/docs/chauffeurs, Chauffeur scan uniquement véhicules assignés)
- Logs d'audit complets : toutes actions CRUD + consultations documents sensibles
- Isolation multi-tenant stricte : indexes DB, storage buckets séparés par client
- Anonymisation données utilisateurs si suppression compte (RGPD "droit à l'oubli")
- Conservation légale 10 ans : archivage automatique avec accès restreint
- **Rate limiting SMS** : Protection anti-abus onboarding chauffeurs (max X SMS/jour/compte)

**2. Observabilité & Monitoring**
- Métriques temps réponse : dashboard <2s, OCR <5s, sync <10s
- Alertes proactives : charge infrastructure >70%, temps réponse >seuils
- Analytics adoption chauffeurs : taux connexion, taux scan, KPI critique pour produit
- Tracking facturation add-on OCR : 0,10€/doc scanné
- **Monitoring coûts** : Alertes si SMS onboarding >seuil (abus détecté), OCR >budget mensuel
- Error tracking : Sentry ou équivalent pour bugs production

**3. Résilience & Gestion d'Erreurs**
- Sync offline → online : gestion conflits (last-write-wins ou custom), retry exponentiel, idempotence
- Gestion erreurs OCR : document illisible → marqué "à vérifier" + notification gestionnaire email (pas de blocage chauffeur)
- Fallbacks alertes : email principal échoue → email secondaire configuré
- Circuit breaker : si OCR externe down → fallback mode manuel avec alerte admin

**4. Performance & Optimisation**
- Lazy loading composants React (code splitting)
- Images optimisées : Next.js Image component avec formats WebP/AVIF
- Caching stratégique : dashboard conformité (stale-while-revalidate), documents (cache-first)
- CDN : Cloudflare devant Vercel pour assets statiques (si Vercel frontend)
- Database indexing : immatriculations, dates échéances, user_id, tenant_id (requêtes fréquentes)

**5. Testing Strategy**
- Tests e2e workflow OCR critique (scan → validation → enregistrement)
- Tests devices réels : Android 7-8 (smartphones anciens chauffeurs)
- Tests offline/online sync : scénarios interruption réseau, conflits
- Tests charge : simulation 50 clients, 2500 véhicules, 1000 docs/mois
- Tests RGPD : vérification isolation multi-tenant, chiffrement, logs audit
- Tests rate limiting SMS : vérification protection anti-abus

## Starter Template Evaluation

### Primary Technology Domain

**Full-stack SaaS B2B PWA** basé sur Next.js App Router avec authentification multi-tenant et gestion documentaire offline-first.

### Technical Preferences Established

**Stack confirmée pour FlotteBox** :
- **Framework** : Next.js 16 (App Router) + React 19 + TypeScript
- **Styling** : TailwindCSS + shadcn/ui
- **Database** : PostgreSQL (OVH Managed Database)
- **ORM** : Prisma
- **Authentication** : Better Auth (avec Organization plugin pour multi-tenant RBAC)
- **Hosting** : Vercel (frontend/backend) + OVH (database + object storage)

**Préférences développement** :
- DX optimal : déploiement type Vercel (push sur main → auto-deploy)
- Coût minimum : infrastructure ~20-30€/mois phase MVP
- Conformité France : données sensibles hébergées France (OVH)

### Starter Options Considered

**Option 1 : Better Auth Starter (devAaus)** ⭐ **SÉLECTIONNÉ**
- Repository : https://github.com/devAaus/better-auth
- Stack incluse : Next.js 15 App Router + Better Auth + Prisma + shadcn/ui + TailwindCSS + TypeScript
- Alignement : 100% avec stack cible FlotteBox

**Option 2 : Next.js SaaS Boilerplate Multi-Tenant (ixartz)**
- Repository : https://github.com/ixartz/SaaS-Boilerplate
- Stack incluse : Next.js + Multi-tenancy + Roles & Permissions + shadcn/ui + Drizzle ORM + Clerk Auth
- Avantages : Multi-tenant déjà implémenté, billing Stripe intégré
- Inconvénients : Clerk Auth (service payant) à remplacer par Better Auth, Drizzle ORM au lieu de Prisma

**Option 3 : Create Next App (from scratch)**
- Commande : `npx create-next-app@latest --typescript --tailwind --app --turbopack`
- Avantages : Contrôle total, zéro dépendance inutile
- Inconvénients : Setup auth + multi-tenant + shadcn/ui entièrement manuel (+2-3 jours)

### Selected Starter: Better Auth Starter (devAaus)

**Rationale for Selection** :

1. **Stack 100% alignée** : Next.js App Router + Better Auth + Prisma + shadcn/ui + TailwindCSS
   - Aucune technologie à remplacer ou retirer
   - Gain de temps immédiat sur configuration base

2. **Better Auth natif** : Authentification moderne avec support multi-tenant via Organization plugin
   - Credentials + OAuth Google déjà configurés
   - DX TypeScript supérieur à NextAuth.js/Auth.js
   - Support natif custom credentials (parfait pour onboarding chauffeurs sans email)

3. **Prisma + PostgreSQL ready** : ORM configuré, migrations prêtes
   - Compatible OVH Managed PostgreSQL
   - Schema extensible pour véhicules/documents/chauffeurs

4. **shadcn/ui intégré** : Composants accessibles (WCAG 2.1 AA) prêts à l'emploi
   - Dashboard components disponibles
   - Personnalisables pour mobile-first (grandes cibles tactiles)

5. **Focus sur métier FlotteBox** : Pas de temps perdu sur plomberie technique
   - Développement immédiat : OCR workflow, gestion véhicules, alertes
   - Configuration auth/db/UI déjà validée

**Initialization Command** :

```bash
# Cloner le starter
git clone https://github.com/devAaus/better-auth.git flottebox-mvp
cd flottebox-mvp

# Installer dépendances
pnpm install

# Upgrade vers dernières versions stables (2026)
pnpm update next@16 react@19 react-dom@19 typescript@latest
pnpm update @prisma/client@latest prisma@latest
pnpm update better-auth@latest

# Vérifier build après upgrades
pnpm run build

# Configuration initiale
cp .env.example .env
# Configurer DATABASE_URL avec OVH PostgreSQL
# Configurer BETTER_AUTH_SECRET
# Configurer GOOGLE_CLIENT_ID + GOOGLE_CLIENT_SECRET

# Initialiser Prisma
pnpm prisma generate
pnpm prisma db push
```

### Architectural Decisions Provided by Starter

**Language & Runtime** :
- TypeScript 5.x avec configuration stricte
- Node.js 20+ (runtime Vercel)
- ESM modules

**Styling Solution** :
- TailwindCSS 4.x avec configuration optimisée
- shadcn/ui components library
- CSS-in-JS via Tailwind (pas de runtime CSS-in-JS)
- Dark mode support natif

**Authentication & Authorization** :
- Better Auth configuré avec :
  - Credentials provider (email/password)
  - OAuth Google provider
  - Session management (JWT)
  - CSRF protection
- Organization plugin à ajouter pour multi-tenant :
  - Roles : Admin, Gestionnaire, Chauffeur
  - Permissions granulaires par organization
  - Team invitations

**Database & ORM** :
- Prisma ORM avec PostgreSQL adapter
- Schema de base : User, Session, Account tables
- Migrations Prisma (dev et production)
- Type-safe database queries

**Build Tooling** :
- Next.js 16 avec Turbopack (build 700x plus rapide)
- TypeScript compilation
- TailwindCSS JIT compiler
- Image optimization automatique (next/image)
- Code splitting automatique (App Router)

**Testing Framework** :
- Pas de tests inclus dans starter → à ajouter
- Recommandation : Vitest + Testing Library + Playwright (e2e)

**Code Organization** :
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

**Development Experience** :
- Hot reload avec Turbopack (< 500ms)
- TypeScript LSP intégré
- ESLint + Prettier configurés
- Better Auth dev tools
- Prisma Studio (GUI database)

### Post-Initialization Setup Required

**1. Upgrade to Latest Versions (2026)** :
- Next.js 16.x (App Router stable)
- React 19.x (avec React Compiler)
- TypeScript 5.7+
- Prisma 6.x
- Better Auth latest (vérifier changelog breaking changes)

**2. Better Auth Organization Plugin** :
- Installation : `pnpm add @better-auth/organization`
- Configuration multi-tenant RBAC :
  - Admin : accès total organization
  - Gestionnaire : gestion véhicules + documents + chauffeurs
  - Chauffeur : scan documents véhicules assignés uniquement
- Extend Prisma schema pour Organization, Member, Role tables

**3. OVH PostgreSQL Connection** :
- Configuration `.env` :
  ```
  DATABASE_URL="postgresql://user:password@postgres.ovh.net:5432/flottebox?schema=public"
  ```
- Test connexion : `pnpm prisma db pull`
- Activer SSL si requis par OVH

**4. shadcn/ui Components Installation** :
- Components nécessaires MVP :
  - Dashboard : `pnpm dlx shadcn@latest add card table badge`
  - Forms : `pnpm dlx shadcn@latest add form input select textarea`
  - Upload : `pnpm dlx shadcn@latest add dropzone`
  - Alerts : `pnpm dlx shadcn@latest add alert toast`
  - Calendar : `pnpm dlx shadcn@latest add calendar`

**5. PWA Configuration** :
- Install next-pwa : `pnpm add @ducanh2912/next-pwa`
- Configure Service Workers pour offline-first
- Create manifest.json (installable PWA)
- Configure IndexedDB pour sync offline documents

**6. Extend Prisma Schema for FlotteBox** :
```prisma
// Ajouter models FlotteBox au schema.prisma
model Vehicle {
  id          String   @id @default(cuid())
  type        VehicleType // MOTOR | TRAILER
  immat       String   @unique
  orgId       String
  org         Organization @relation(fields: [orgId], references: [id])
  documents   Document[]
  drivers     DriverVehicle[]
  createdAt   DateTime @default(now())
}

model Document {
  id          String   @id @default(cuid())
  type        DocType
  status      DocStatus // PENDING_OCR | PENDING_VALIDATION | VALIDATED
  vehicleId   String
  vehicle     Vehicle @relation(fields: [vehicleId], references: [id])
  storageUrl  String
  ocrData     Json?
  expiryDate  DateTime?
  createdAt   DateTime @default(now())
}

enum VehicleType {
  MOTOR
  TRAILER
}

enum DocType {
  CARTE_GRISE
  ASSURANCE
  CONTROLE_TECHNIQUE
  // ... autres types
}
```

**7. Additional Dependencies for MVP** :
```bash
# OVH Object Storage (S3-compatible)
pnpm add @aws-sdk/client-s3

# OCR Mistral API
pnpm add @mistralai/mistralai

# Email (Resend)
pnpm add resend

# SMS (OVH API)
pnpm add axios

# LemonSqueezy payments
pnpm add @lemonsqueezy/lemonsqueezy.js

# Date handling
pnpm add date-fns

# Form validation
pnpm add zod react-hook-form @hookform/resolvers

# PWA
pnpm add @ducanh2912/next-pwa workbox-window
```

### Deployment Strategy

**Vercel Deployment (Frontend + API Routes)** :
1. Connect GitHub repo to Vercel
2. Configure environment variables :
   - `DATABASE_URL` (OVH PostgreSQL)
   - `BETTER_AUTH_SECRET`
   - `GOOGLE_CLIENT_ID` + `GOOGLE_CLIENT_SECRET`
   - `OVH_OBJECT_STORAGE_*` (credentials)
   - `MISTRAL_API_KEY`
   - `RESEND_API_KEY`
   - `OVH_SMS_*` (credentials)
   - `LEMONSQUEEZY_API_KEY`
3. Auto-deploy on `git push main`

**OVH Infrastructure** :
- PostgreSQL : Managed Database instance (région France)
- Object Storage : Bucket pour documents PDF/images (région France)
- SMS : API OVH SMS avec rate limiting

**Estimated MVP Costs** :
- Vercel : Free tier (puis ~20$/mois si dépassement)
- OVH PostgreSQL : ~10-15€/mois (instance minimale)
- OVH Object Storage : ~5-10€/mois (stockage + transfert)
- **Total : ~20-30€/mois phase MVP**

**Note** : Project initialization using this command should be the **first implementation story** in Epics & Stories phase.

## Core Architectural Decisions

### Decision Priority Analysis

**Critical Decisions (Block Implementation) :**
- ✅ Data architecture & caching strategy
- ✅ Offline-first sync strategy
- ✅ OCR workflow & file processing
- ✅ Background jobs & alertes
- ✅ Monitoring & observabilité
- ✅ RGPD compliance architecture
- ✅ Cookies & legal pages

**Important Decisions (Shape Architecture) :**
- Multi-tenant isolation patterns
- Rate limiting SMS strategy
- Error handling & fallbacks
- Database indexing strategy

**Deferred Decisions (Post-MVP) :**
- Redis cache externe (M6+ si besoin performance)
- Message queue system (M6+ si charge importante)
- Analytics externe (M6+ Mixpanel/PostHog)
- Logs retention avancée (M6+ Axiom/Datadog)

### Data Architecture

**Caching Strategy - Next.js Native (MVP)**

**Décision** : Pas de cache externe Redis en MVP, utilisation caching Next.js natif
- `unstable_cache` pour données dashboard conformité (revalidate 60s)
- Route handlers avec `revalidate` pour listes véhicules/documents
- **Rationale** : Redis = +15€/mois + complexité, Next.js caching suffit charge MVP <50 clients
- **Upgrade path** : Upstash Redis à M6+ si temps réponse dashboard >2s

**OVH Object Storage Architecture**

**Structure buckets** :
```
org-{orgId}/
  └── vehicles/
      └── {vehicleId}/
          └── docs/
              └── {docId}-{timestamp}.pdf
```

**Décisions** :
- 1 bucket par organization (isolation multi-tenant stricte)
- Naming convention : `{docId}-{timestamp}.{ext}` pour éviter conflits
- Pas de CDN MVP (OVH France suffisamment performant, économie ~20€/mois Cloudflare)
- **Coût** : ~5-10€/mois MVP

**PostgreSQL Indexes Critiques**

```sql
-- Performance <2s dashboard
CREATE INDEX idx_vehicles_org_id ON vehicles(org_id);
CREATE INDEX idx_vehicles_org_type ON vehicles(org_id, type);
CREATE INDEX idx_documents_vehicle_id ON documents(vehicle_id);
CREATE INDEX idx_documents_expiry_date ON documents(expiry_date)
  WHERE expiry_date IS NOT NULL;
CREATE INDEX idx_documents_status ON documents(status);
CREATE INDEX idx_documents_org_vehicle ON documents(org_id, vehicle_id);
CREATE INDEX idx_users_org_id ON users(org_id);
CREATE INDEX idx_audit_logs_org_timestamp ON audit_logs(org_id, timestamp);
CREATE INDEX idx_audit_logs_user_action ON audit_logs(user_id, action);
```

**Rationale** : Indexes composites org_id + champ fréquent → garantit isolation + performance

**💰 Coût Catégorie Data : 5-10€/mois**

### Offline-First & Sync Strategy

**Service Workers - @ducanh2912/next-pwa**

**Décision** : next-pwa avec Workbox strategies intégrées
```typescript
// next.config.js PWA configuration
withPWA({
  dest: 'public',
  register: true,
  skipWaiting: true,
  runtimeCaching: [
    {
      urlPattern: /^https:\/\/api\.flottebox\.fr\/.*$/,
      handler: 'NetworkFirst',  // Sync priority
      options: {
        cacheName: 'api-cache',
        expiration: { maxEntries: 50, maxAgeSeconds: 300 }
      }
    },
    {
      urlPattern: /\.(?:png|jpg|jpeg|svg|gif)$/,
      handler: 'CacheFirst',  // Assets statiques
      options: {
        cacheName: 'image-cache',
        expiration: { maxEntries: 100, maxAgeSeconds: 86400 }
      }
    },
    {
      urlPattern: /^https:\/\/.*\.ovh\.net\/.*\.pdf$/,
      handler: 'StaleWhileRevalidate',  // Documents scannés
      options: {
        cacheName: 'document-cache',
        expiration: { maxEntries: 20, maxAgeSeconds: 3600 }
      }
    }
  ]
})
```

**IndexedDB Schema Offline**

```typescript
// lib/offline-storage.ts
interface OfflineDocument {
  id: string;
  vehicleId: string;
  type: DocType;
  imageBlob: Blob;        // Photo scannée (max 5MB)
  ocrData?: Json;
  timestamp: number;
  syncStatus: 'pending' | 'syncing' | 'synced' | 'error';
  retryCount: number;
}

// Limite volumétrie : 50MB max IndexedDB (~50 documents photos)
// Alert user si >40MB : "Synchronisez pour libérer espace"
```

**Sync Strategy - Last-Write-Wins Simple**

**Décision** : Last-write-wins (pas de conflict resolution complexe MVP)
```typescript
// lib/sync-engine.ts
async function syncOfflineDocuments() {
  const pendingDocs = await db.offlineDocuments
    .where('syncStatus').equals('pending')
    .toArray();

  for (const doc of pendingDocs) {
    try {
      // Upload vers OVH + OCR
      await uploadDocument(doc);

      // Marquer synced
      await db.offlineDocuments.update(doc.id, {
        syncStatus: 'synced'
      });

      // Supprimer local après sync success
      await db.offlineDocuments.delete(doc.id);

    } catch (error) {
      // Retry exponentiel : 1s, 2s, 4s, 8s, 16s, 30s max
      const retryDelay = Math.min(
        Math.pow(2, doc.retryCount) * 1000,
        30000
      );

      await db.offlineDocuments.update(doc.id, {
        syncStatus: 'error',
        retryCount: doc.retryCount + 1
      });

      setTimeout(() => syncOfflineDocuments(), retryDelay);
    }
  }
}

// Background Sync API
if ('serviceWorker' in navigator && 'sync' in ServiceWorkerRegistration.prototype) {
  navigator.serviceWorker.ready.then(reg => {
    reg.sync.register('sync-documents');
  });
}
```

**Rationale** :
- Last-write-wins suffit MVP (1 utilisateur mobile = 1 chauffeur par véhicule)
- Conflict resolution complexe = overhead inutile phase MVP
- Si besoin post-MVP : CRDTs ou vector clocks

**💰 Coût Catégorie Offline : 0€**

### OCR Workflow & File Processing

**Upload Flow - Direct OVH avec Presigned URLs**

**Décision** : Upload direct browser → OVH (pas de transit API)

```typescript
// app/api/documents/presigned-url/route.ts
export async function POST(req: Request) {
  const { fileName, vehicleId, orgId } = await req.json();

  // Vérifier permissions RBAC
  await verifyUserCanUpload(orgId, vehicleId);

  // Générer presigned URL OVH (expiration 5 min)
  const docId = generateId();
  const key = `org-${orgId}/vehicles/${vehicleId}/docs/${docId}-${Date.now()}.pdf`;
  const presignedUrl = await s3Client.getSignedUrl('putObject', {
    Bucket: 'flottebox-documents',
    Key: key,
    Expires: 300,  // 5 minutes
    ContentType: 'application/pdf'
  });

  return { presignedUrl, docId, storageKey: key };
}
```

**Frontend Upload Component** :
```typescript
// components/UploadDocument.tsx
const handleUpload = async (file: File) => {
  // 1. Get presigned URL
  setStatus('uploading');
  const { presignedUrl, docId } = await fetch('/api/documents/presigned-url', {
    method: 'POST',
    body: JSON.stringify({ fileName: file.name, vehicleId })
  }).then(r => r.json());

  // 2. Upload direct vers OVH
  await fetch(presignedUrl, {
    method: 'PUT',
    body: file,
    headers: { 'Content-Type': file.type }
  });

  // 3. Trigger OCR backend
  setStatus('ocr-processing');
  const ocrResult = await fetch('/api/documents/ocr', {
    method: 'POST',
    body: JSON.stringify({ docId })
  }).then(r => r.json());

  // 4. Pré-remplir formulaire validation
  setFormData(mapOcrToForm(ocrResult));
  setStatus('form-ready');
};
```

**OCR Processing - Async Simple (Pas de Queue MVP)**

```typescript
// app/api/documents/ocr/route.ts
export async function POST(req: Request) {
  const { docId } = await req.json();

  const doc = await prisma.document.findUnique({ where: { id: docId } });

  try {
    // Appel Mistral OCR (timeout 30s)
    const ocrResult = await mistralOCR(doc.storageUrl, { timeout: 30000 });

    await prisma.document.update({
      where: { id: docId },
      data: {
        ocrData: ocrResult,
        status: 'PENDING_VALIDATION'
      }
    });

    return { success: true, ocrData: ocrResult };

  } catch (error) {
    // Retry 3 fois avec backoff exponentiel
    if (retryCount < 3) {
      await sleep(Math.pow(2, retryCount) * 1000);
      return retryOCR(docId, retryCount + 1);
    }

    // Après 3 échecs → marquer "à vérifier manuellement"
    await prisma.document.update({
      where: { id: docId },
      data: {
        status: 'MANUAL_ENTRY_REQUIRED',
        ocrError: error.message
      }
    });

    // Email alert gestionnaire
    await sendEmail({
      to: org.alertEmail,
      subject: 'Document nécessite saisie manuelle',
      body: `Le document ${doc.type} du véhicule ${doc.vehicle.immat} n'a pas pu être analysé automatiquement.`
    });

    return { success: false, requiresManualEntry: true };
  }
}
```

**Fallback OCR - Circuit Breaker Simple**

**Décision** : Pas de fallback OCR secondaire MVP
- 3 retry exponentiels Mistral OCR
- Si échec final → mode manuel (formulaire vide)
- Email alert gestionnaire
- **Post-MVP** : Fallback vers Google Vision ou AWS Textract si Mistral down >30 min

**💰 Coût Catégorie OCR : 0,10-0,20€/mois** (~100 docs × $2/1000)

### Background Jobs & Alertes

**Calcul Échéances - Vercel Cron**

```typescript
// app/api/cron/calculate-alerts/route.ts
export async function GET(req: Request) {
  // Vérifier Vercel Cron secret
  if (req.headers.get('authorization') !== `Bearer ${process.env.CRON_SECRET}`) {
    return new Response('Unauthorized', { status: 401 });
  }

  const now = new Date();
  const dates = {
    j60: addDays(now, 60),
    j30: addDays(now, 30),
    j15: addDays(now, 15),
    expired: now
  };

  // Trouver documents expirant
  const expiringDocs = await prisma.document.findMany({
    where: {
      expiryDate: {
        lte: dates.j60,
        gte: now
      },
      alertSent: false
    },
    include: { vehicle: { include: { org: true } } }
  });

  // Grouper par organization + type alerte
  const alertsByOrg = groupBy(expiringDocs, doc => doc.vehicle.orgId);

  // Envoyer batch emails
  for (const [orgId, docs] of Object.entries(alertsByOrg)) {
    await sendAlertEmail(orgId, docs);

    // Marquer alertSent = true
    await prisma.document.updateMany({
      where: { id: { in: docs.map(d => d.id) } },
      data: { alertSent: true }
    });
  }

  return new Response('OK');
}
```

**Vercel Cron Configuration** :
```json
// vercel.json
{
  "crons": [
    {
      "path": "/api/cron/calculate-alerts",
      "schedule": "0 8 * * *"  // Tous les jours à 8h00 UTC
    }
  ]
}
```

**Email Sending - Resend Direct (Pas de Queue)**

```typescript
// lib/email.ts
export async function sendAlertEmail(orgId: string, docs: Document[]) {
  const org = await prisma.organization.findUnique({
    where: { id: orgId },
    include: { alertEmails: true }
  });

  const emailBody = renderAlertEmailTemplate(docs);

  try {
    await resend.emails.send({
      from: 'alertes@flottebox.fr',
      to: org.alertEmails.operational,  // Email opérationnel configuré
      subject: `${docs.length} échéances à surveiller`,
      html: emailBody
    });

    // Log envoi
    await prisma.emailLog.create({
      data: {
        orgId,
        type: 'ALERT',
        recipientCount: 1,
        status: 'SENT'
      }
    });

  } catch (error) {
    // Retry simple : 2 tentatives max
    if (!retried) {
      await sleep(5000);
      return sendAlertEmail(orgId, docs, true);
    }

    // Log erreur
    await prisma.emailLog.create({
      data: {
        orgId,
        type: 'ALERT',
        status: 'FAILED',
        error: error.message
      }
    });
  }
}
```

**SMS Onboarding Rate Limiting - DB-based**

```typescript
// lib/sms-rate-limiter.ts
export async function canSendSMS(orgId: string): Promise<boolean> {
  const today = startOfDay(new Date());

  const smsCount = await prisma.smsLog.count({
    where: {
      orgId,
      createdAt: { gte: today }
    }
  });

  // Limite : 3 SMS/org/jour
  return smsCount < 3;
}

// app/api/drivers/send-onboarding-sms/route.ts
export async function POST(req: Request) {
  const { driverId } = await req.json();

  const driver = await prisma.user.findUnique({
    where: { id: driverId },
    include: { org: true }
  });

  // Check rate limit
  if (!await canSendSMS(driver.orgId)) {
    return new Response('Rate limit dépassé (3 SMS/jour)', { status: 429 });
  }

  // Envoyer SMS via OVH API
  await sendOVHSMS(driver.phone, {
    message: `Bienvenue sur FlotteBox!\nIdentifiant: ${driver.username}\nMot de passe: ${driver.tempPassword}\nLien: ${process.env.APP_URL}/install`
  });

  // Log SMS
  await prisma.smsLog.create({
    data: {
      orgId: driver.orgId,
      userId: driverId,
      type: 'ONBOARDING',
      status: 'SENT'
    }
  });

  return new Response('OK');
}
```

**💰 Coût Catégorie Background Jobs : 0€/mois**

### Monitoring & Observabilité

**Error Tracking - Sentry Free Tier**

```typescript
// sentry.client.config.ts
import * as Sentry from "@sentry/nextjs";

Sentry.init({
  dsn: process.env.NEXT_PUBLIC_SENTRY_DSN,
  environment: process.env.NODE_ENV,
  tracesSampleRate: 0.1,  // 10% traces en prod
  beforeSend(event) {
    // Filtrer données sensibles
    if (event.request) {
      delete event.request.cookies;
      delete event.request.headers?.['authorization'];
    }
    return event;
  }
});
```

**Analytics Adoption - Custom Prisma**

```prisma
model UserActivity {
  id          String   @id @default(cuid())
  orgId       String
  userId      String
  action      String   // LOGIN, SCAN_DOCUMENT, VIEW_DASHBOARD
  metadata    Json?
  timestamp   DateTime @default(now())

  @@index([orgId, action, timestamp])
  @@index([userId, timestamp])
}
```

**Dashboard gestionnaire adoption chauffeurs** :
```typescript
// app/api/analytics/driver-adoption/route.ts
export async function GET(req: Request) {
  const { orgId } = await getSession(req);

  const drivers = await prisma.user.findMany({
    where: { orgId, role: 'DRIVER' }
  });

  const scansLast30Days = await prisma.document.groupBy({
    by: ['createdBy'],
    where: {
      orgId,
      createdAt: { gte: subDays(new Date(), 30) },
      createdBy: { in: drivers.map(d => d.id) }
    },
    _count: true
  });

  return {
    totalDrivers: drivers.length,
    activeDrivers: scansLast30Days.length,
    adoptionRate: (scansLast30Days.length / drivers.length) * 100,
    scansPerDriver: scansLast30Days
  };
}
```

**Logs Infrastructure - Vercel Logs**

**Décision MVP** : Vercel Logs inclus (1 jour retention)
- Suffisant MVP pour debug
- **Upgrade M6+** : Axiom (10€/mois, 30 jours retention) ou Betterstack (10€/mois)

**Uptime Monitoring - UptimeRobot Free**

Configuration :
- Monitor 1 : `https://flottebox.fr` (frontend)
- Monitor 2 : `https://flottebox.fr/api/health` (backend health check)
- Check interval : 5 minutes
- Alerts : Email + SMS (optionnel)

**💰 Coût Catégorie Monitoring : 0€/mois MVP**

### Security & RGPD Compliance

**Multi-Tenant Isolation - Prisma Middleware**

```typescript
// lib/prisma-middleware.ts
prisma.$use(async (params, next) => {
  // Récupérer orgId depuis contexte session
  const session = await getSession();

  // Forcer orgId dans toutes les queries
  if (params.model && ['Vehicle', 'Document', 'User'].includes(params.model)) {
    if (params.action === 'findMany' || params.action === 'findFirst') {
      params.args.where = {
        ...params.args.where,
        orgId: session.orgId
      };
    }

    if (params.action === 'create') {
      params.args.data = {
        ...params.args.data,
        orgId: session.orgId
      };
    }
  }

  return next(params);
});
```

**Audit Logs - Traçabilité Complète**

```prisma
model AuditLog {
  id          String      @id @default(cuid())
  orgId       String
  userId      String
  action      AuditAction
  resource    String      // "Vehicle", "Document"
  resourceId  String
  metadata    Json?
  ipAddress   String?
  userAgent   String?
  timestamp   DateTime    @default(now())

  @@index([orgId, timestamp])
  @@index([userId, action])
  @@index([resource, resourceId])
}

enum AuditAction {
  VIEW_DOCUMENT
  DOWNLOAD_DOCUMENT
  CREATE_DOCUMENT
  UPDATE_DOCUMENT
  DELETE_DOCUMENT
  CREATE_VEHICLE
  UPDATE_VEHICLE
  DELETE_VEHICLE
  EXPORT_DATA
}
```

**Middleware Audit automatique** :
```typescript
// middleware.ts
export async function middleware(req: NextRequest) {
  const session = await getSession(req);

  // Logger actions sensibles
  if (req.nextUrl.pathname.startsWith('/api/documents/download')) {
    await prisma.auditLog.create({
      data: {
        orgId: session.orgId,
        userId: session.userId,
        action: 'DOWNLOAD_DOCUMENT',
        resource: 'Document',
        resourceId: req.nextUrl.searchParams.get('id'),
        ipAddress: req.ip,
        userAgent: req.headers.get('user-agent')
      }
    });
  }

  return NextResponse.next();
}
```

**Droit à l'Oubli - Anonymisation**

```typescript
// app/api/organizations/delete/route.ts
export async function POST(req: Request) {
  const { orgId } = await req.json();

  // 1. Anonymiser utilisateurs
  await prisma.user.updateMany({
    where: { orgId },
    data: {
      email: sql`CONCAT('deleted-', gen_random_uuid(), '@anonymized.local')`,
      name: 'Utilisateur supprimé',
      phone: null,
      deletedAt: new Date()
    }
  });

  // 2. Supprimer documents OVH Storage
  const bucketName = `org-${orgId}`;
  await deleteOVHBucket(bucketName);

  // 3. Soft delete organization
  await prisma.organization.update({
    where: { id: orgId },
    data: {
      status: 'DELETED',
      deletedAt: new Date()
    }
  });

  // 4. Conserver audit logs (obligation légale 10 ans)
  // → AuditLog non supprimé, mais données user anonymisées

  return { success: true };
}
```

**Chiffrement Données**

**Au repos** :
- OVH Object Storage : AES-256 natif (inclus)
- PostgreSQL : Chiffrement at-rest OVH Managed DB (inclus)

**En transit** :
- HTTPS/TLS 1.3 obligatoire (Vercel + OVH)
- Presigned URLs OVH : expiration 5 min max

**💰 Coût Catégorie RGPD : 0€** (inclus architecture)

### Cookies & Legal Compliance

**Cookies Strategy - Pas de Banner (Uniquement Nécessaires)**

**Décision** : FlotteBox utilise uniquement cookies strictement nécessaires
- Session authentification (Better Auth)
- CSRF protection (Next.js)
- **Exemption consentement RGPD** : Cookies nécessaires = pas de banner obligatoire

**Pages Légales Obligatoires** :

1. **`/cookies`** - Politique d'utilisation des cookies
2. **`/mentions-legales`** - Mentions légales (éditeur, hébergeur)
3. **`/confidentialite`** - Politique de confidentialité RGPD
4. **`/cgu`** - Conditions générales d'utilisation

**Footer links** :
```tsx
// components/Footer.tsx
<footer>
  <Link href="/mentions-legales">Mentions légales</Link>
  <Link href="/confidentialite">Confidentialité</Link>
  <Link href="/cookies">Cookies</Link>
  <Link href="/cgu">CGU</Link>
</footer>
```

**💰 Coût Catégorie Legal : 0€**

### Infrastructure Cost Summary

| Catégorie | Service | Coût/mois MVP |
|-----------|---------|---------------|
| Hosting | Vercel Hobby | 0€ (puis ~20$ si dépassement) |
| Database | OVH PostgreSQL | 10-15€ |
| Storage | OVH Object Storage | 5-10€ |
| Caching | Next.js natif | 0€ |
| Offline | next-pwa + IndexedDB | 0€ |
| OCR | Mistral API | 0,10-0,20€ |
| Email | Resend | 0€ (3k/mois gratuit) |
| SMS | OVH SMS pay-per-use | ~0,50€ |
| Monitoring | Sentry + UptimeRobot | 0€ |
| Cron | Vercel Cron | 0€ |
| **TOTAL MVP** | | **15-30€/mois** |

**Post-MVP Upgrades (M6+)** :
- Redis cache : +15€/mois (Upstash)
- Queue system : +10-19€/mois (Upstash/Inngest)
- Logs retention : +10€/mois (Axiom)
- Sentry paid : +26$/mois (>5k events)

### Decision Impact Analysis

**Implementation Sequence** :

1. **Phase Setup (Story 1)** :
   - Init projet Better Auth starter
   - Upgrade Next.js 16 + React 19
   - Config OVH PostgreSQL + Object Storage
   - Setup Vercel deployment

2. **Phase Auth & Multi-tenant (Stories 2-3)** :
   - Better Auth Organization plugin
   - Prisma schema FlotteBox
   - RBAC middleware
   - Multi-tenant isolation

3. **Phase PWA & Offline (Stories 4-5)** :
   - next-pwa configuration
   - IndexedDB schema
   - Sync engine
   - Service Workers strategies

4. **Phase OCR & Upload (Stories 6-8)** :
   - OVH presigned URLs
   - Upload component
   - Mistral OCR integration
   - Validation form workflow

5. **Phase Alertes & Cron (Stories 9-10)** :
   - Vercel Cron calculate-alerts
   - Resend email integration
   - SMS onboarding + rate limiting

6. **Phase Monitoring & RGPD (Stories 11-12)** :
   - Sentry setup
   - Audit logs middleware
   - Analytics adoption dashboard
   - Pages légales (cookies, confidentialité, CGU)

**Cross-Component Dependencies** :

```
Multi-tenant isolation (Prisma middleware)
  ↓
  ├─→ OCR workflow (vérifie orgId avant upload)
  ├─→ Alertes email (filtre par orgId)
  ├─→ Analytics adoption (scope orgId)
  └─→ Audit logs (orgId required)

Offline-first sync
  ↓
  ├─→ Upload component (gère mode offline)
  ├─→ OCR workflow (retry après retour réseau)
  └─→ Dashboard (affiche docs pending sync)

Better Auth Organization
  ↓
  ├─→ RBAC permissions (Admin/Gestionnaire/Chauffeur)
  ├─→ Onboarding chauffeurs (création users)
  └─→ Multi-tenant isolation (orgId)
```

## Implementation Patterns & Consistency Rules

### Pattern Categories Overview

**Identified Conflict Points**: 18 areas where different AI agents could make incompatible implementation choices have been identified and standardized to ensure code consistency across FlotteBox.

**Critical for Multi-Agent Development**: These patterns prevent conflicts when multiple AI agents implement different features simultaneously. All patterns are mandatory for implementation phase.

---

### Naming Patterns

#### **Language Convention - Code Anglais, UI Français**

**MANDATORY PATTERN**: All code (models, functions, variables, API routes) MUST be in English. UI text and legal business terms remain in French.

**Rationale**:
- Better Auth, Prisma, Next.js ecosystems are English-first
- Stack Overflow and documentation in English
- Avoids confusion with external library imports (`User`, `Organization` already English in Better Auth)
- Standard for international codebases

**Code Examples**:

```typescript
// ✅ CORRECT - English code, French UI
model Vehicle {
  licensePlate  String
  type          VehicleType
}

model Driver {
  firstName     String
  lastName      String
}

// API Routes
/api/vehicles
/api/drivers
/api/documents

// Functions
const getUserVehicles = async (userId: string) => { ... }
const sendDriverOnboardingSMS = async (driverId: string) => { ... }

// UI Components (French text)
<Card title="Véhicule" description="Gérer vos véhicules" />
<Button>Ajouter un chauffeur</Button>
```

```typescript
// ❌ FORBIDDEN - Mixed French/English
model Vehicule { ... }  // NO
const getUtilisateurVehicles = () => { ... }  // NO
/api/vehicules  // NO
```

**Exception - Legal Business Terms**: Business-specific enums can use French legal terms:

```typescript
enum DocumentType {
  CARTE_GRISE           // ✅ Legal French term
  ASSURANCE             // ✅ Legal French term
  CONTROLE_TECHNIQUE    // ✅ Legal French term
  PERMIS_CONDUIRE       // ✅ Legal French term
}

enum VehicleType {
  MOTOR     // ✅ English for code consistency
  TRAILER   // ✅ English for code consistency
}
```

---

#### **Database Naming - Prisma Conventions**

**MANDATORY PATTERN**: PascalCase models, camelCase fields, `@map("snake_case")` for PostgreSQL compatibility, plural relations.

```prisma
// ✅ CORRECT Prisma Schema
model Vehicle {
  id            String        @id @default(cuid())
  orgId         String        // camelCase fields
  licensePlate  String        @map("license_plate")  // DB column: snake_case
  type          VehicleType

  // Relations: plural
  org           Organization  @relation(fields: [orgId], references: [id])
  documents     Document[]    // Plural relation name
  drivers       DriverVehicle[]

  @@index([orgId])
  @@index([orgId, licensePlate])
  @@map("vehicles")  // DB table: lowercase plural
}

model Document {
  id          String      @id @default(cuid())
  vehicleId   String      @map("vehicle_id")
  expiryDate  DateTime?   @map("expiry_date")

  vehicle     Vehicle     @relation(fields: [vehicleId], references: [id])

  @@map("documents")
}
```

```prisma
// ❌ FORBIDDEN
model vehicle { ... }  // NO - lowercase model
model Vehicle {
  vehicle_id  String  // NO - snake_case field
  documentList Document[]  // NO - use plural 'documents'
}
```

**Foreign Key Naming**:
```prisma
// ✅ CORRECT
model Document {
  vehicleId   String
  vehicle     Vehicle  @relation(fields: [vehicleId], references: [id])
}

// ❌ FORBIDDEN
model Document {
  vehicle_id  String   // NO
  fk_vehicle  String   // NO
}
```

---

#### **API Route Naming - RESTful Conventions**

**MANDATORY PATTERN**: Plural resource names, lowercase, kebab-case for multi-word.

```typescript
// ✅ CORRECT API Routes
/api/vehicles                    // Plural
/api/vehicles/[id]
/api/documents
/api/documents/[id]
/api/documents/presigned-url     // kebab-case
/api/drivers
/api/drivers/send-onboarding-sms // kebab-case

// Query params: camelCase
/api/vehicles?orgId=xxx&type=MOTOR

// Route params: [id], [vehicleId], etc.
/app/dashboard/vehicles/[vehicleId]/documents/[documentId]
```

```typescript
// ❌ FORBIDDEN
/api/vehicule      // NO - singular
/api/vehicle       // NO - singular
/api/Vehicles      // NO - uppercase
/api/vehicles/presignedUrl  // NO - use kebab-case
```

---

#### **Component & File Naming - Next.js Conventions**

**MANDATORY PATTERN**: PascalCase for React components, kebab-case for non-component files.

```
components/
├── VehicleCard.tsx              // ✅ PascalCase component
├── VehicleList.tsx
├── DocumentUploadForm.tsx
├── DriverOnboardingWizard.tsx
└── ui/                          // shadcn/ui components
    ├── button.tsx               // ✅ kebab-case (shadcn convention)
    ├── card.tsx
    └── dialog.tsx

lib/
├── auth.ts                      // ✅ kebab-case utilities
├── prisma-middleware.ts
├── sms-rate-limiter.ts
├── offline-storage.ts
└── utils.ts

app/
├── (dashboard)/
│   └── vehicles/
│       ├── page.tsx             // ✅ Next.js convention
│       └── [vehicleId]/
│           └── page.tsx
└── api/
    └── vehicles/
        └── route.ts             // ✅ Next.js convention
```

**Function Naming**:
```typescript
// ✅ CORRECT - camelCase
const getUserVehicles = async () => { ... }
const sendAlertEmail = async () => { ... }
const canSendSMS = async () => { ... }

// ❌ FORBIDDEN
const GetUserVehicles = () => { ... }  // NO - PascalCase
const send_alert_email = () => { ... }  // NO - snake_case
```

---

### API Response Formats

#### **Success Response - Direct Data**

**MANDATORY PATTERN**: Return data directly using `Response.json()`. No wrapper object.

```typescript
// ✅ CORRECT - Direct data response
export async function GET(req: Request) {
  const vehicles = await prisma.vehicle.findMany({ ... });
  return Response.json(vehicles);
}

// Returns: [{ id: "...", licensePlate: "..." }, ...]
```

```typescript
// ❌ FORBIDDEN - Wrapper objects
return Response.json({ success: true, data: vehicles });  // NO
return Response.json({ result: vehicles });                // NO
```

---

#### **Error Response - Structured Format**

**MANDATORY PATTERN**: All errors MUST use this exact structure with appropriate HTTP status codes.

```typescript
// Type Definition (MANDATORY)
// lib/api-types.ts
export type ApiError = {
  error: {
    message: string;     // Human-readable error (French for UI)
    code: string;        // Machine-readable code (UPPERCASE_SNAKE)
    field?: string;      // Single field error (optional)
    fields?: Array<{     // Multi-field validation errors (optional)
      field: string;
      message: string;
    }>;
  };
};

export type ApiResponse<T> = T | ApiError;
```

**Single Error Example**:
```typescript
// ✅ CORRECT - Single error
export async function POST(req: Request) {
  const { file } = await req.json();

  if (!file) {
    return Response.json(
      {
        error: {
          message: "Fichier requis",
          code: "FILE_REQUIRED",
          field: "file"
        }
      },
      { status: 400 }
    );
  }
}
```

**Validation Errors Example** (Multi-field):
```typescript
// ✅ CORRECT - Validation errors
export async function POST(req: Request) {
  const result = CreateVehicleSchema.safeParse(await req.json());

  if (!result.success) {
    return Response.json(
      {
        error: {
          message: "Validation échouée",
          code: "VALIDATION_ERROR",
          fields: result.error.issues.map(issue => ({
            field: issue.path.join('.'),
            message: issue.message
          }))
        }
      },
      { status: 422 }
    );
  }
}

// Returns:
// {
//   "error": {
//     "message": "Validation échouée",
//     "code": "VALIDATION_ERROR",
//     "fields": [
//       { "field": "licensePlate", "message": "Format invalide" },
//       { "field": "expiryDate", "message": "Date doit être future" }
//     ]
//   }
// }
```

**HTTP Status Codes (MANDATORY)**:
```typescript
// ✅ CORRECT Status Codes
400  // Bad Request - invalid input
401  // Unauthorized - not authenticated
403  // Forbidden - not authorized (RBAC)
404  // Not Found - resource doesn't exist
422  // Unprocessable Entity - validation failed
429  // Too Many Requests - rate limit exceeded
500  // Internal Server Error - unexpected error
```

---

### Multi-Tenant orgId Injection

#### **Backend - Prisma Middleware (CRITICAL SECURITY)**

**MANDATORY PATTERN**: ALL database queries MUST go through Prisma middleware that automatically injects `orgId` filtering.

```typescript
// lib/prisma-middleware.ts (MANDATORY)
import { getServerSession } from '@/lib/auth';

const TENANT_MODELS = [
  'Vehicle',
  'Document',
  'Driver',
  'Alert',
  'UserActivity',
  'EmailLog',
  'SmsLog'
];

prisma.$use(async (params, next) => {
  const session = await getServerSession();

  if (!session?.orgId) {
    throw new Error('Session orgId required for database access');
  }

  if (params.model && TENANT_MODELS.includes(params.model)) {

    // READ queries: auto-inject orgId filter
    if (['findMany', 'findFirst', 'findUnique', 'count', 'aggregate'].includes(params.action)) {
      params.args.where = {
        ...params.args.where,
        orgId: session.orgId
      };
    }

    // CREATE queries: auto-inject orgId
    if (params.action === 'create') {
      params.args.data = {
        ...params.args.data,
        orgId: session.orgId
      };
    }

    // UPDATE/DELETE: REQUIRE orgId in where clause (security check)
    if (['update', 'updateMany', 'delete', 'deleteMany'].includes(params.action)) {
      if (!params.args.where?.orgId) {
        throw new Error(`SECURITY: orgId required in where clause for ${params.action}`);
      }
    }
  }

  return next(params);
});
```

**Usage in API Routes** (middleware handles orgId automatically):
```typescript
// ✅ CORRECT - Middleware injects orgId automatically
export async function GET(req: Request) {
  // No need to manually add orgId - middleware handles it
  const vehicles = await prisma.vehicle.findMany({
    where: {
      type: 'MOTOR'
      // orgId automatically injected by middleware
    }
  });

  return Response.json(vehicles);
}

// ✅ CORRECT - Explicit orgId for UPDATE/DELETE (security requirement)
export async function DELETE(req: Request) {
  const session = await getServerSession();
  const { id } = await req.json();

  await prisma.vehicle.delete({
    where: {
      id,
      orgId: session.orgId  // REQUIRED for security
    }
  });
}
```

```typescript
// ❌ FORBIDDEN - Queries without middleware
const vehicles = await db.query('SELECT * FROM vehicles');  // NO - bypass middleware
```

---

#### **Frontend - React Context (UI State Only)**

**MANDATORY PATTERN**: Use React Context for UI-level organization state. Do NOT use for data queries (Server Actions handle this).

```typescript
// components/providers/OrganizationProvider.tsx
'use client';

import { createContext, useContext } from 'react';

type OrgContext = {
  orgId: string;
  orgName: string;
  role: 'ADMIN' | 'MANAGER' | 'DRIVER';
  permissions: string[];
};

const OrganizationContext = createContext<OrgContext | null>(null);

export function useOrganization() {
  const ctx = useContext(OrganizationContext);
  if (!ctx) throw new Error('useOrganization must be used within OrganizationProvider');
  return ctx;
}

export function OrganizationProvider({
  children,
  value
}: {
  children: React.ReactNode;
  value: OrgContext;
}) {
  return (
    <OrganizationContext.Provider value={value}>
      {children}
    </OrganizationContext.Provider>
  );
}
```

**Usage in Components**:
```typescript
// ✅ CORRECT - Use context for UI logic only
'use client';

export function VehicleList() {
  const { orgId, role } = useOrganization();

  // UI-level permissions check
  const canAddVehicle = role === 'ADMIN' || role === 'MANAGER';

  return (
    <div>
      <h1>Organization: {orgId}</h1>
      {canAddVehicle && <Button>Ajouter véhicule</Button>}
    </div>
  );
}
```

---

### Offline Storage Patterns

#### **IndexedDB Structure - Dexie.js**

**MANDATORY PATTERN**: Use Dexie.js with strict naming and schema conventions.

```typescript
// lib/offline-db.ts
import Dexie, { Table } from 'dexie';

export interface OfflineDocument {
  id: string;                // Pre-generated client-side (cuid)
  vehicleId: string;
  type: DocumentType;
  imageBlob: Blob;           // Scanned photo (max 5MB)
  timestamp: number;         // Unix timestamp
  syncStatus: 'pending' | 'syncing' | 'synced' | 'error';
  retryCount: number;
  lastError?: string;
}

export class OfflineDB extends Dexie {
  documents!: Table<OfflineDocument, string>;

  constructor() {
    super('FlotteBoxOffline');  // PascalCase database name

    this.version(1).stores({
      documents: 'id, vehicleId, syncStatus, timestamp'  // lowercase plural store
    });
  }
}

export const db = new OfflineDB();
```

**Naming Conventions**:
- Database name: `FlotteBoxOffline` (PascalCase)
- Store names: `documents` (lowercase plural)
- Status values: `'pending' | 'syncing' | 'synced' | 'error'` (lowercase strings)
- Field names: camelCase

**Storage Limits**:
```typescript
// MANDATORY: Storage quota management
const MAX_STORAGE_MB = 50;  // 50MB limit (~50 document photos)
const WARN_THRESHOLD_MB = 40;  // Warn user at 40MB

export async function checkStorageQuota(): Promise<{
  used: number;
  limit: number;
  shouldWarn: boolean;
}> {
  const estimate = await navigator.storage.estimate();
  const usedMB = (estimate.usage || 0) / (1024 * 1024);

  return {
    used: usedMB,
    limit: MAX_STORAGE_MB,
    shouldWarn: usedMB > WARN_THRESHOLD_MB
  };
}
```

---

### Validation Patterns - Zod Schemas

#### **Schema Organization - Shared Library**

**MANDATORY PATTERN**: Use Prisma Zod Generator + custom schemas in `/lib/schemas/`.

**Installation**:
```bash
pnpm add zod prisma-zod-generator
pnpm add -D @hookform/resolvers react-hook-form
```

**Prisma Configuration**:
```prisma
// prisma/schema.prisma
generator zod {
  provider = "prisma-zod-generator"
  output   = "../lib/schemas/generated"
}

model Vehicle {
  id           String      @id @default(cuid())
  licensePlate String
  type         VehicleType
  orgId        String
  // ...
}
```

**Directory Structure**:
```
lib/schemas/
├── generated/              # Auto-generated from Prisma (DO NOT EDIT)
│   ├── vehicle.schema.ts
│   ├── document.schema.ts
│   └── driver.schema.ts
├── vehicle.schema.ts       # Custom schemas for forms/API
├── document.schema.ts
├── driver.schema.ts
└── index.ts               # Barrel exports
```

**Custom Schemas** (Business Logic Validation):
```typescript
// lib/schemas/vehicle.schema.ts
import { z } from 'zod';
import { VehicleSchema as PrismaVehicleSchema } from './generated/vehicle.schema';

// Form schema with French validation
export const CreateVehicleSchema = z.object({
  licensePlate: z.string()
    .regex(
      /^[A-Z]{2}-\d{3}-[A-Z]{2}$/,
      'Format immatriculation invalide (AA-123-BB)'
    )
    .transform(val => val.toUpperCase()),

  type: z.enum(['MOTOR', 'TRAILER']),

  brand: z.string().min(1, 'Marque requise'),

  model: z.string().optional()
});

export type CreateVehicleInput = z.infer<typeof CreateVehicleSchema>;

// Document validation with expiry date logic
export const CreateDocumentSchema = z.object({
  type: z.enum(['CARTE_GRISE', 'ASSURANCE', 'CONTROLE_TECHNIQUE', 'PERMIS_CONDUIRE']),

  expiryDate: z.string()
    .refine(val => {
      const date = new Date(val);
      return date > new Date();
    }, 'La date d\'échéance doit être dans le futur'),

  file: z.instanceof(File)
    .refine(file => file.size <= 5 * 1024 * 1024, 'Fichier max 5MB')
    .refine(
      file => ['image/jpeg', 'image/png', 'application/pdf'].includes(file.type),
      'Format accepté: JPEG, PNG, PDF'
    )
});
```

**Usage - Server Side** (API Routes):
```typescript
// app/api/vehicles/route.ts
import { CreateVehicleSchema } from '@/lib/schemas/vehicle.schema';

export async function POST(req: Request) {
  const body = await req.json();

  // Validate with Zod
  const result = CreateVehicleSchema.safeParse(body);

  if (!result.success) {
    return Response.json(
      {
        error: {
          message: 'Validation échouée',
          code: 'VALIDATION_ERROR',
          fields: result.error.issues.map(issue => ({
            field: issue.path.join('.'),
            message: issue.message
          }))
        }
      },
      { status: 422 }
    );
  }

  // Create with validated data
  const vehicle = await prisma.vehicle.create({
    data: result.data
  });

  return Response.json(vehicle);
}
```

**Usage - Client Side** (Forms):
```typescript
// components/VehicleForm.tsx
'use client';

import { useForm } from 'react-hook-form';
import { zodResolver } from '@hookform/resolvers/zod';
import { CreateVehicleSchema } from '@/lib/schemas/vehicle.schema';

export function VehicleForm() {
  const form = useForm({
    resolver: zodResolver(CreateVehicleSchema)
  });

  const onSubmit = async (data) => {
    const response = await fetch('/api/vehicles', {
      method: 'POST',
      body: JSON.stringify(data)
    });
    // ...
  };

  return (
    <Form {...form}>
      <FormField
        name="licensePlate"
        render={({ field }) => (
          <FormItem>
            <FormLabel>Immatriculation</FormLabel>
            <FormControl>
              <Input placeholder="AA-123-BB" {...field} />
            </FormControl>
            <FormMessage />
          </FormItem>
        )}
      />
    </Form>
  );
}
```

---

### Error Handling Patterns

#### **Global Error Boundary**

**MANDATORY PATTERN**: All client components must be wrapped in error boundaries.

```typescript
// app/error.tsx (MANDATORY)
'use client';

import { useEffect } from 'react';
import * as Sentry from '@sentry/nextjs';

export default function Error({
  error,
  reset
}: {
  error: Error & { digest?: string };
  reset: () => void;
}) {
  useEffect(() => {
    Sentry.captureException(error);
  }, [error]);

  return (
    <div className="flex flex-col items-center justify-center min-h-screen">
      <h2>Une erreur est survenue</h2>
      <p>{error.message}</p>
      <button onClick={reset}>Réessayer</button>
    </div>
  );
}
```

#### **API Error Handling Pattern**

**MANDATORY PATTERN**: Consistent try-catch with Sentry logging.

```typescript
// ✅ CORRECT - Consistent error handling
export async function POST(req: Request) {
  try {
    // Business logic
    const result = await someOperation();
    return Response.json(result);

  } catch (error) {
    // Log to Sentry
    Sentry.captureException(error, {
      tags: { route: '/api/vehicles' },
      extra: { body: await req.json() }
    });

    // Return structured error
    return Response.json(
      {
        error: {
          message: 'Erreur lors de la création du véhicule',
          code: 'VEHICLE_CREATE_ERROR'
        }
      },
      { status: 500 }
    );
  }
}
```

---

### Code Organization Patterns

#### **Project Structure - Feature-Based Organization**

**MANDATORY PATTERN**: Organize by feature, not by type.

```
app/
├── (auth)/                      # Auth routes group
│   ├── login/
│   │   └── page.tsx
│   └── register/
│       └── page.tsx
├── (dashboard)/                 # Protected routes group
│   ├── vehicles/
│   │   ├── page.tsx            # Vehicle list
│   │   ├── [vehicleId]/
│   │   │   ├── page.tsx        # Vehicle details
│   │   │   └── documents/
│   │   │       └── page.tsx    # Vehicle documents
│   │   └── components/         # ✅ Feature-scoped components
│   │       ├── VehicleCard.tsx
│   │       ├── VehicleForm.tsx
│   │       └── VehicleFilters.tsx
│   ├── drivers/
│   │   ├── page.tsx
│   │   └── components/
│   │       └── DriverOnboardingWizard.tsx
│   └── layout.tsx
├── api/
│   ├── vehicles/
│   │   ├── route.ts
│   │   └── [id]/
│   │       └── route.ts
│   └── documents/
│       ├── presigned-url/
│       │   └── route.ts
│       └── ocr/
│           └── route.ts
└── layout.tsx

components/                      # Shared components only
├── ui/                         # shadcn/ui components
│   ├── button.tsx
│   └── card.tsx
└── shared/                     # Truly shared components
    ├── Header.tsx
    ├── Footer.tsx
    └── LoadingSpinner.tsx

lib/                            # Utilities and configs
├── schemas/                    # Zod validation schemas
│   ├── generated/
│   ├── vehicle.schema.ts
│   └── document.schema.ts
├── auth.ts
├── prisma.ts
├── prisma-middleware.ts
├── offline-db.ts
├── sms-rate-limiter.ts
└── utils.ts
```

```
// ❌ FORBIDDEN - Type-based organization
components/
├── cards/           # NO - organize by feature
├── forms/           # NO
└── modals/          # NO
```

---

### Enforcement Guidelines

#### **All AI Agents MUST:**

1. **Use English for all code** (models, functions, variables, routes) with French UI text only
2. **Follow Prisma naming**: PascalCase models, camelCase fields, plural relations
3. **Use direct `Response.json(data)` for success**, structured `{ error: {...} }` for errors with HTTP status codes
4. **NEVER bypass Prisma middleware** - all queries go through `orgId` injection
5. **Use Dexie.js for IndexedDB** with lowercase plural stores and lowercase status values
6. **Share Zod schemas** in `/lib/schemas/` between client and server
7. **Organize by feature**, not by component type
8. **Log all errors to Sentry** with context tags
9. **Use TypeScript strict mode** with no `any` types
10. **Follow shadcn/ui conventions** for UI components

#### **Pattern Verification Checklist:**

Before submitting code, verify:

- [ ] No French in code (except enum legal terms)
- [ ] All Prisma models follow naming conventions
- [ ] API errors return `{ error: { message, code } }` format
- [ ] All database queries use Prisma (no raw SQL without middleware)
- [ ] Zod schemas defined in `/lib/schemas/`
- [ ] Components organized by feature in route folders
- [ ] TypeScript errors resolved (no `@ts-ignore`)
- [ ] Sentry error logging added to try-catch blocks

---

### Pattern Examples & Anti-Patterns

#### **Good Example - Complete Feature Implementation**

```typescript
// ✅ CORRECT - Full pattern compliance

// 1. Prisma Schema
model Vehicle {
  id            String   @id @default(cuid())
  orgId         String
  licensePlate  String   @map("license_plate")
  documents     Document[]
  @@map("vehicles")
}

// 2. Zod Schema
// lib/schemas/vehicle.schema.ts
export const CreateVehicleSchema = z.object({
  licensePlate: z.string().regex(/^[A-Z]{2}-\d{3}-[A-Z]{2}$/)
});

// 3. API Route
// app/api/vehicles/route.ts
export async function POST(req: Request) {
  try {
    const body = await req.json();
    const validated = CreateVehicleSchema.parse(body);

    // Prisma middleware auto-injects orgId
    const vehicle = await prisma.vehicle.create({
      data: validated
    });

    return Response.json(vehicle);  // Direct data

  } catch (error) {
    Sentry.captureException(error);
    return Response.json(
      { error: { message: "Erreur création", code: "CREATE_ERROR" } },
      { status: 500 }
    );
  }
}

// 4. Frontend Component
// app/(dashboard)/vehicles/components/VehicleForm.tsx
'use client';

export function VehicleForm() {
  const { orgId } = useOrganization();  // UI context only

  const form = useForm({
    resolver: zodResolver(CreateVehicleSchema)
  });

  return <Form>...</Form>;
}
```

#### **Anti-Patterns to Avoid**

```typescript
// ❌ WRONG - Multiple violations

// 1. French code names
model Vehicule { ... }  // NO
const getVehicules = () => { ... }  // NO

// 2. Wrong API response format
return { success: true, data: vehicles };  // NO - use direct Response.json(vehicles)

// 3. Bypassing Prisma middleware
const vehicles = await db.query('SELECT * FROM vehicles');  // NO - use Prisma

// 4. Hardcoded orgId instead of middleware
const vehicles = await prisma.vehicle.findMany({
  where: { orgId: 'hardcoded-org-123' }  // NO - let middleware inject
});

// 5. Duplicate Zod schemas
// In component file:
const schema = z.object({ ... });  // NO - use shared schema from /lib/schemas/

// 6. Type-based organization
components/forms/VehicleForm.tsx  // NO - should be in vehicles/components/

// 7. Missing error handling
export async function POST() {
  const data = await prisma.create({ ... });  // NO - wrap in try-catch
  return Response.json(data);
}
```
