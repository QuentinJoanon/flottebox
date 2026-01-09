---
stepsCompleted: [1, 2]
inputDocuments:
  - _bmad-output/prd.md
  - _bmad-output/architecture.md
  - _bmad-output/project-planning-artifacts/ux-design-specification.md
---

# FlotteBox - Epic Breakdown

## Overview

This document provides the complete epic and story breakdown for FlotteBox, decomposing the requirements from the PRD, UX Design, and Architecture requirements into implementable stories.

## Requirements Inventory

### Functional Requirements

**1. User Management & Authentication**

- **FR1**: Users can create an account with email/password or OAuth Google (Admin, Gestionnaire, Tiers roles)
- **FR2**: Companies can require credit card during 14-day free trial registration
- **FR3**: Gestionnaires can create chauffeur accounts with custom identifier (username), password, name, first name, and phone number (email optional)
- **FR4**: Gestionnaires can create, modify, and delete user accounts (Admin, Gestionnaire, Chauffeur, Tiers roles) within their company
- **FR5**: Chauffeurs authenticate using custom identifier + password (no email required)
- **FR6**: Chauffeurs cannot modify their own account information (read-only access to their profile)
- **FR7**: Gestionnaires can deactivate or delete chauffeur accounts at any time (disposable accounts for turnover management)
- **FR8**: Admins can configure 2FA (Two-Factor Authentication) for their account with trusted device management
- **FR9**: Companies can grant read-only access to external third parties (comptables, auditeurs) with granular permissions
- **FR10**: Comptables can access multiple client companies with a single account

**2. Fleet & Vehicle Management**

- **FR11**: Gestionnaires can add vehicles with distinction between motors (camions, VUL, voitures, engins) and trailers (remorques)
- **FR12**: Gestionnaires can import vehicles in bulk via CSV file with duplicate detection
- **FR13**: Gestionnaires can assign chauffeurs to specific vehicles
- **FR14**: Gestionnaires can edit and delete vehicles
- **FR15**: Chauffeurs can view vehicle information and documents for vehicles assigned to them
- **FR16**: System can automatically count active motors and trailers per company for billing

**3. Document Management & OCR**

- **FR17**: Gestionnaires can upload documents via drag-and-drop (desktop) or camera scan (mobile PWA)
- **FR18**: Chauffeurs can scan documents using mobile PWA in less than 30 seconds
- **FR19**: System can perform OCR-assisted extraction to pre-fill document fields (immatriculation, dates, type) with >90% precision
- **FR20**: Users must validate OCR-extracted data before document creation (mandatory human validation)
- **FR21**: System can classify document types automatically (carte grise, assurance, CT, permis, visite médicale, FIMO)
- **FR22**: System can mark illegible documents as "à vérifier" and notify gestionnaire
- **FR23**: Chauffeurs can scan documents offline and system syncs automatically when network returns
- **FR24**: System can archive documents (soft delete) instead of permanent deletion for 10-year legal retention
- **FR25**: Gestionnaires can import documents in bulk via ZIP with OCR matching to vehicles

**4. Alerts & Notifications**

- **FR26**: System can calculate expiration deadlines automatically and trigger alerts at 60 days, 30 days, 15 days, and expiration
- **FR27**: Users can receive email notifications (daily/weekly summaries) for upcoming expirations
- **FR28**: Users can receive push notifications via PWA for critical alerts
- **FR29**: Companies can activate SMS alerts add-on (0,50€/vehicle/month) with self-service toggle
- **FR30**: Chauffeurs can receive SMS alerts on their registered phone number if SMS add-on is activated
- **FR31**: Gestionnaires can customize alert frequency and types per vehicle
- **FR32**: System can send automatic reminders to gestionnaires if chauffeur hasn't scanned in 30 days

**5. Dashboard & Reporting**

- **FR33**: Gestionnaires can view real-time fleet compliance dashboard with visual status (vert/orange/rouge)
- **FR34**: Dirigeants can view upcoming deadlines calendar for next 30 days
- **FR35**: Gestionnaires can view chauffeur adoption metrics (% having scanned ≥1 document/month)
- **FR36**: Gestionnaires can export "Registre de conformité conducteurs" (permis, visites médicales, FIMO) as PDF for URSSAF audits
- **FR37**: Gestionnaires can export "Registre de conformité véhicules" (cartes grises, assurances, CT) as PDF/Excel
- **FR38**: Users can select custom date periods for compliance exports (e.g., last 3 years)
- **FR39**: Tiers (comptables) can export compliance registers for their client companies

**6. Onboarding & Driver Adoption**

- **FR40**: Gestionnaires can generate QR code for PWA installation to share with chauffeurs
- **FR41**: Gestionnaires can send onboarding SMS to chauffeur with login credentials (identifier + password) and PWA installation link
- **FR42**: Chauffeurs can install PWA in under 1 minute by scanning QR code
- **FR43**: System can send 90-second tutorial video via SMS to new chauffeurs
- **FR44**: System can guide chauffeurs through first scan with tooltips and step-by-step validation
- **FR45**: System can activate gamification (badges, leaderboard) automatically if company >10 users OR >5 scans/month
- **FR46**: Chauffeurs can earn "Chauffeur exemplaire" badge for consistent document scanning

**7. Billing & Subscription Management**

- **FR47**: System can calculate monthly billing using usage-based pricing (motors × tier rate + trailers × 1,50€)
- **FR48**: System can apply tiered pricing for motors (1-25: 4€, 26-100: 3€, 101+: 2,50€)
- **FR49**: System can charge OCR add-on at 0,10€/document scanned (pay-as-you-go)
- **FR50**: System can handle TVA automatically for French and European B2B invoicing
- **FR51**: Admins can activate/deactivate add-ons (SMS, API ANTAI) via self-service dashboard
- **FR52**: Admins can download monthly invoices as PDF from billing portal
- **FR53**: System can auto-renew subscriptions with email reminder at J-7 before charge
- **FR54**: Admins can cancel subscription self-service before end of free trial

**8. Compliance & Security**

- **FR55**: System can isolate data by company (multi-tenant) with no cross-tenant data leakage
- **FR56**: System can log all access and modifications to documents for audit trail (P1)
- **FR57**: Admins can view audit logs filtered by user, date, and action (P1)
- **FR58**: System can retain audit logs for minimum 3 years for URSSAF compliance (P1)
- **FR59**: System can enforce RGPD data privacy with user consent management
- **FR60**: Users can exercise RGPD rights (access, rectification, deletion) via self-service interface
- **FR61**: Gestionnaires can manage RGPD requests for chauffeur accounts (chauffeurs cannot self-manage)

**9. Super Admin & Analytics**

- **FR62**: Super Admin (Quentin) can view global analytics dashboard (MRR, churn, ARPU, NPS)
- **FR63**: Super Admin can identify at-risk clients with health score <40/100
- **FR64**: Super Admin can view feature adoption metrics (OCR usage, SMS add-on, etc.)
- **FR65**: Super Admin can activate demo mode to generate realistic fake company data for sales presentations
- **FR66**: Super Admin can track funnel activation and user engagement events

### Non-Functional Requirements

**Performance (NFR-P)**

- **NFR-P1**: Dashboard de conformité (statuts vert/orange/rouge) doit se charger en moins de 2 secondes sur connexion 4G standard
- **NFR-P2**: Calendrier des échéances (30 prochains jours) doit s'afficher en moins de 1,5 secondes
- **NFR-P3**: Actions utilisateur (clic sur véhicule, filtrage, export) doivent recevoir un feedback visuel en moins de 300ms
- **NFR-P4**: Traitement OCR d'un document scanné (extraction champs + pré-remplissage) doit s'effectuer en moins de 5 secondes
- **NFR-P5**: Si traitement OCR dépasse 5 secondes, afficher indicateur de progression pour éviter frustration utilisateur
- **NFR-P6**: Upload d'un document (mobile PWA → serveur) doit se compléter en moins de 3 secondes sur 4G standard
- **NFR-P7**: Synchronisation automatique des documents scannés hors ligne doit débuter dans les 10 secondes suivant le retour réseau
- **NFR-P8**: Synchronisation complète de tous documents offline doit se terminer en moins de 5 minutes (jusqu'à 20 documents)
- **NFR-P9**: Interface doit afficher la progression de sync en temps réel (X/Y documents synchronisés)
- **NFR-P10**: 95% des appels API backend doivent répondre en moins de 500ms (mesure P95)
- **NFR-P11**: Aucun appel API ne doit dépasser 3 secondes (timeout serveur)

**Security (NFR-S)**

- **NFR-S1**: Toutes les données personnelles (permis, visites médicales, cartes grises) doivent être hébergées en France (Scaleway/OVH)
- **NFR-S2**: Consentement explicite RGPD obligatoire avant traitement de toute donnée personnelle
- **NFR-S3**: Interface self-service pour droits RGPD (accès, rectification, suppression) accessible en moins de 3 clics depuis paramètres
- **NFR-S4**: Suppression compte utilisateur (RGPD "droit à l'oubli") doit anonymiser toutes données personnelles en moins de 24h
- **NFR-S5**: Politique de confidentialité et CGU doivent être affichées et acceptées avant création de compte
- **NFR-S6**: Toutes les communications client-serveur doivent utiliser HTTPS/TLS 1.3 minimum (encryption in transit)
- **NFR-S7**: Documents stockés (Scaleway/OVH Object Storage) doivent être encryptés at rest (AES-256)
- **NFR-S8**: Mots de passe utilisateurs doivent être hashés avec bcrypt (cost factor ≥12) ou Argon2
- **NFR-S9**: Aucune requête base de données ne doit pouvoir accéder à des données d'un autre tenant (0% fuite cross-tenant)
- **NFR-S10**: Middleware Next.js doit systématiquement injecter `company_id` dans toutes les requêtes DB
- **NFR-S11**: Tests de sécurité automatisés doivent valider l'isolation multi-tenant avant chaque déploiement production
- **NFR-S12**: Sessions utilisateur doivent expirer après 7 jours d'inactivité (chauffeurs) et 24h (Admin/Gestionnaire)
- **NFR-S13**: Limitation tentatives login : maximum 5 échecs avant blocage temporaire 15 minutes
- **NFR-S14**: 2FA (P1) doit utiliser TOTP standard (RFC 6238) compatible Google Authenticator/Authy
- **NFR-S15**: Tous les accès et modifications de documents doivent être tracés dans audit logs avec horodatage UTC
- **NFR-S16**: Audit logs doivent être conservés 3 ans minimum (conformité URSSAF)
- **NFR-S17**: Aucun utilisateur (même Super Admin) ne doit pouvoir modifier ou supprimer les audit logs

**Scalability (NFR-SC)**

- **NFR-SC1**: Système doit supporter 100 scans simultanés de chauffeurs sans dégradation de performance (< 10% augmentation temps réponse)
- **NFR-SC2**: Dashboard doit rester fluide avec jusqu'à 50 gestionnaires connectés simultanément sur une même entreprise
- **NFR-SC3**: Architecture serverless Vercel doit scaler automatiquement jusqu'à 500 requêtes/seconde
- **NFR-SC4**: Système doit supporter une croissance de 0 à 200 clients actifs sans refactoring architectural majeur
- **NFR-SC5**: Base de données PostgreSQL doit gérer jusqu'à 500 000 documents scannés avec performance constante (indexation optimisée)
- **NFR-SC6**: Object Storage (documents) doit supporter croissance jusqu'à 5 TB sans limite pratique (Scaleway/OVH scaling illimité)
- **NFR-SC7**: Système doit gérer des pics de charge 3x supérieurs à la moyenne (ex: fin de mois pour échéances CT) sans downtime
- **NFR-SC8**: Si charge dépasse 80% capacité serveur, alertes automatiques doivent être envoyées pour scaling proactif

**Reliability (NFR-R)**

- **NFR-R1**: Uptime cible de 99,5% mensuel (≈ 3,6h downtime maximum par mois)
- **NFR-R2**: Maintenance planifiée doit être annoncée 48h à l'avance et limitée à 2h maximum
- **NFR-R3**: En cas de downtime imprévu, restauration du service doit s'effectuer en moins de 4h (RTO - Recovery Time Objective)
- **NFR-R4**: Backup automatique quotidien de la base de données PostgreSQL (snapshot à 3h du matin UTC+1)
- **NFR-R5**: Backups doivent être conservés 30 jours glissants (possibilité de restaurer jusqu'à J-30)
- **NFR-R6**: Documents scannés (Object Storage) doivent avoir versioning activé pour récupération en cas de suppression accidentelle
- **NFR-R7**: RPO (Recovery Point Objective) maximum de 24h : en cas de panne totale, perte de données max = dernières 24h
- **NFR-R8**: Application PWA chauffeur doit fonctionner 100% offline pour scan de documents (stockage local IndexedDB)
- **NFR-R9**: Synchronisation doit gérer les conflits (ex: 2 chauffeurs scannent le même document offline) avec stratégie "last write wins"
- **NFR-R10**: En cas d'échec de sync, système doit réessayer automatiquement toutes les 5 minutes jusqu'à succès (max 10 tentatives)
- **NFR-R11**: Erreurs critiques (paiement échoué, OCR failed, sync impossible) doivent déclencher alertes email admin en temps réel
- **NFR-R12**: Monitoring proactif (Vercel Analytics + Sentry) doit détecter dégradations performance avant impact utilisateur
- **NFR-R13**: Taux d'erreur API doit rester < 1% (99% de requêtes réussies)

**Usability (NFR-U)**

- **NFR-U1**: Installation PWA via QR code doit se compléter en moins de 1 minute
- **NFR-U2**: Interface mobile doit être optimisée pour utilisation une main (boutons accessibles pouce, zones touch 44x44px minimum)
- **NFR-U3**: Premier scan chauffeur doit être guidé par tooltips contextuels (max 3 étapes, validation visuelle immédiate)
- **NFR-U4**: PWA chauffeur doit fonctionner sur iOS 12+ (Safari) et Android 8+ (Chrome)
- **NFR-U5**: Dashboard gestionnaire doit supporter Chrome 90+, Firefox 88+, Safari 14+, Edge 90+
- **NFR-U6**: Application doit être responsive et utilisable sur écrans de 320px (iPhone SE) à 2560px (desktop 27")
- **NFR-U7**: Nouveau chauffeur doit pouvoir scanner son premier document sans formation préalable (guidage intuitif)
- **NFR-U8**: Gestionnaire doit pouvoir importer sa première flotte (CSV) en moins de 15 minutes
- **NFR-U9**: Vidéo tutoriel chauffeur doit durer maximum 90 secondes
- **NFR-U10**: Interface doit être 100% en français pour MVP (langue cible PME/ETI France)
- **NFR-U11**: Dates doivent être affichées au format français (jj/mm/aaaa)
- **NFR-U12**: Montants doivent être affichés en euros (€) avec virgule décimale française (ex: 265,00€)

**Accessibility (NFR-A)**

- **NFR-A1**: Application doit être conforme WCAG 2.1 niveau AA (standard légal EU) pour dashboard gestionnaire
- **NFR-A2**: Contraste texte/fond doit respecter ratio minimum 4.5:1 (texte normal) et 3:1 (texte large)
- **NFR-A3**: Navigation clavier complète : toutes les fonctionnalités doivent être accessibles sans souris (tab, enter, espace)
- **NFR-A4**: Statuts conformité (vert/orange/rouge) doivent avoir labels textuels alternatifs pour lecteurs d'écran
- **NFR-A5**: Formulaires doivent avoir labels explicites et messages d'erreur descriptifs (pas juste "Erreur")
- **NFR-A6**: Images et icônes doivent avoir attributs alt textuels pertinents
- **NFR-A7**: Taille de police minimum 14px sur mobile, 16px sur desktop (lisibilité chauffeurs 50+ ans)
- **NFR-A8**: Boutons d'action principaux (Scanner, Valider) doivent avoir taille minimum 48x48px
- **NFR-A9**: Messages d'erreur doivent être rédigés en langage simple, non technique
- **NFR-A10**: Dashboard conformité doit utiliser couleurs standardisées (vert = ok, orange = attention, rouge = critique)
- **NFR-A11**: Interface doit éviter dépendance exclusive à la couleur pour transmettre information (ajouter icônes/texte)

### Additional Requirements from Architecture

**STARTER TEMPLATE (CRITICAL for Epic 1 Story 1):**

Better Auth Starter (devAaus) from https://github.com/devAaus/better-auth

The architecture specifies using the **Better Auth Starter template** as the foundation. This greenfield project template includes:
- Next.js 15+ App Router
- Better Auth pre-configured with credentials + OAuth Google
- Prisma ORM with PostgreSQL adapter
- shadcn/ui component library
- TailwindCSS 4.x with dark mode support
- TypeScript 5.x with strict configuration
- Vitest testing framework to be added

**Initialization Command:**
```bash
git clone https://github.com/devAaus/better-auth.git flottebox-mvp
cd flottebox-mvp
pnpm install
pnpm update next@16 react@19 react-dom@19 typescript@latest
pnpm update @prisma/client@latest prisma@latest
pnpm update better-auth@latest
pnpm run build
cp .env.example .env
# Configure DATABASE_URL (OVH PostgreSQL)
# Configure BETTER_AUTH_SECRET
# Configure GOOGLE_CLIENT_ID + GOOGLE_CLIENT_SECRET
pnpm prisma generate
pnpm prisma db push
```

**INFRASTRUCTURE REQUIREMENTS:**

- **Hosting Stack**: Vercel (frontend/backend) + OVH (database + object storage)
  - Vercel for Next.js App Router deployment with serverless scaling
  - OVH Managed PostgreSQL for database (France-hosted for GDPR compliance)
  - OVH S3 Object Storage for document storage (presigned URLs, multi-tenant isolation)
- **Cost Constraints**: Infrastructure must cost ~20-30€/month in MVP phase
- **Database**: PostgreSQL via OVH with Prisma ORM, migrations managed via Prisma
- **Deployment**: Vercel auto-deployment from Git push to main branch
- **Environment Requirements**: Node.js 20+ runtime, pnpm package manager, TypeScript strict mode

**INTEGRATION REQUIREMENTS:**

- **Better Auth**: Multi-tenant authentication with email/password + OAuth Google
- **OVH S3 Object Storage**: Document storage with presigned URLs and multi-tenant isolation
- **Mistral OCR API**: Document text extraction in French with 30-second timeout
- **Resend Email API**: Transactional emails for alerts and onboarding
- **OVH SMS API**: SMS onboarding for drivers with rate limiting
- **Sentry**: Error logging and performance monitoring
- **Vercel Cron Jobs**: Daily alert calculation protected by CRON_SECRET

**OTHER TECHNICAL REQUIREMENTS:**

- **Offline-First Architecture**: IndexedDB local storage, Service Workers for sync-on-reconnect
- **Multi-Tenant Security**: Prisma Middleware with automatic orgId injection on all DB queries
- **API Design Standards**: Plural resources, kebab-case for actions, Zod schemas shared between client/server
- **Performance & Caching**: Next.js `unstable_cache` with 60-second revalidation for dashboard metrics
- **GDPR & Compliance**: AES-256 encryption at rest, TLS 1.3 in transit, audit logging, anonymization on deletion
- **Testing Framework**: Vitest + Testing Library (unit), Playwright (E2E)

### Additional Requirements from UX Design

- Mobile-first architecture for chauffeurs (terrain, offline zones)
- Desktop optimized for gestionnaires (dashboard, bulk upload)
- Touch-first mobile with minimum 48×48px tap targets for users 50-60 years old
- High contrast interface testable with 55-year-old driver success criteria
- Minimum 16px font size for readability
- Language simplified for non-technical users (terrain vocabulary)
- Color-coded semantic indicators with shape differentiation for colorblind users
- Bottom navigation tabs instead of hidden hamburger menus
- SMS-based onboarding without email requirements
- Android 7-8 support (critical for older driver phones)
- PWA installation via browser (no app store required)
- 3-click maximum rule for core scanning action
- Scan photo → OCR pre-fills → Validation (2-3 fields max) → Record workflow
- Automatic vehicle association (single vehicle = no selection, multiple = pre-selected last used)
- Drag-and-drop zone for desktop file upload
- Batch upload with multiple file simultaneous processing
- Progressive visual feedback animation for OCR extraction
- Clearly marked "à vérifier" for unreadable documents
- Non-blocking error messages with solutions
- Large primary action buttons positioned centrally for thumb access
- Color-coded status cards (green = OK, orange = watch, red = urgent)
- Bottom tab navigation (3 tabs max: Scanner, Mes Véhicules, Profil)
- Offline-first with discreet "Hors ligne" indicator
- Silent sync with notification "X documents synchronisés ✅" on network return
- Video tutorial (90 seconds) at first login, skippable
- SMS delivery of login credentials and PWA installation link

### FR Coverage Map

**Epic 1: Project Foundation & Authentication**
- FR1: Users can create account with email/password or OAuth Google
- FR2: Companies can require credit card during trial
- FR8: Admins can configure 2FA
- FR55: Multi-tenant data isolation
- FR59: RGPD data privacy enforcement
- FR60: Users can exercise RGPD rights
- FR61: Gestionnaires manage RGPD requests for chauffeurs

**Epic 2: Fleet Management Core**
- FR11: Add vehicles with motor/trailer distinction
- FR12: Import vehicles bulk via CSV
- FR14: Edit and delete vehicles
- FR16: Auto-count motors/trailers for billing

**Epic 3: Driver Management & Onboarding**
- FR3: Create chauffeur accounts with custom identifier
- FR4: Create/modify/delete user accounts
- FR5: Chauffeurs authenticate with identifier + password
- FR6: Chauffeurs read-only access to profile
- FR7: Deactivate/delete chauffeur accounts
- FR13: Assign chauffeurs to vehicles
- FR40: Generate QR code for PWA installation
- FR41: Send onboarding SMS with credentials
- FR42: Install PWA via QR code
- FR43: Send tutorial video via SMS

**Epic 4: Document Scanning & OCR (Mobile PWA)**
- FR17: Upload documents via camera scan (mobile)
- FR18: Scan documents in <30 seconds
- FR19: OCR pre-fill with >90% precision
- FR20: Mandatory human validation
- FR21: Auto-classify document types
- FR22: Mark illegible docs "à vérifier"
- FR23: Offline scan with auto-sync
- FR24: Archive documents (soft delete)

**Epic 5: Document Management (Desktop)**
- FR15: View vehicle info and documents
- FR17: Upload via drag-and-drop (desktop)
- FR25: Import documents bulk via ZIP

**Epic 6: Compliance Dashboard & Real-Time Monitoring**
- FR33: Real-time compliance dashboard
- FR34: Upcoming deadlines calendar
- FR35: Chauffeur adoption metrics

**Epic 7: Alerts & Notifications System**
- FR26: Auto-calculate expiration deadlines
- FR27: Email notifications for expirations
- FR28: PWA push notifications
- FR31: Customize alert frequency/types
- FR32: Remind if chauffeur hasn't scanned in 30 days

**Epic 8: Compliance Exports & Reporting**
- FR36: Export registre conducteurs (PDF)
- FR37: Export registre véhicules (PDF/Excel)
- FR38: Select custom date periods

**Epic 9: Billing & Subscription Management**
- FR47: Calculate monthly billing with usage-based pricing
- FR48: Apply tiered pricing for motors
- FR49: Charge OCR add-on per document
- FR50: Handle TVA automatically
- FR51: Activate/deactivate add-ons
- FR52: Download invoices as PDF
- FR53: Auto-renew with J-7 reminder
- FR54: Cancel subscription self-service

**Epic 10: Driver Adoption & Gamification**
- FR44: Guide first scan with tooltips
- FR45: Activate gamification automatically
- FR46: Award "Chauffeur exemplaire" badge

**Epic 11: Third-Party Access & Multi-Client Management**
- FR9: Grant read-only access to tiers
- FR10: Comptables access multiple clients
- FR39: Tiers export compliance registers

**Epic 12: SMS Alerts Add-On**
- FR29: Activate SMS alerts add-on
- FR30: Chauffeurs receive SMS alerts

**Epic 13: Audit Logs & Compliance Tracking (P1)**
- FR56: Log all access/modifications
- FR57: View audit logs with filters
- FR58: Retain logs 3 years minimum

**Epic 14: Super Admin Analytics & Health Monitoring**
- FR62: View global analytics dashboard
- FR63: Identify at-risk clients
- FR64: View feature adoption metrics
- FR65: Activate demo mode
- FR66: Track funnel and engagement events

## Epic List

### Epic 1: Project Foundation & Authentication
Les gestionnaires peuvent créer un compte entreprise, s'authentifier via email/password ou Google OAuth, et accéder à l'application sécurisée. Le projet est initialisé avec le starter template Better Auth incluant Next.js 15+, Prisma, shadcn/ui, et l'infrastructure multi-tenant.

**FRs covered:** FR1, FR2, FR8, FR55, FR59, FR60, FR61

**Architecture Requirements:** Starter Template Setup (Better Auth from devAaus), OVH PostgreSQL, Vercel deployment, multi-tenant isolation with Prisma middleware

---

### Epic 2: Fleet Management Core
Les gestionnaires peuvent créer leur flotte de véhicules avec distinction moteurs/remorques, importer en masse via CSV, et gérer les modifications. Le système compte automatiquement les véhicules actifs pour la facturation différenciée.

**FRs covered:** FR11, FR12, FR14, FR16

---

### Epic 3: Driver Management & Onboarding
Les gestionnaires peuvent créer des comptes chauffeurs avec identifiants custom (sans email requis), les assigner aux véhicules, et les onboarder automatiquement via SMS contenant les credentials et le lien PWA avec QR code.

**FRs covered:** FR3, FR4, FR5, FR6, FR7, FR13, FR40, FR41, FR42, FR43

**Integration Requirements:** OVH SMS API avec rate limiting anti-abus

---

### Epic 4: Document Scanning & OCR (Mobile PWA)
Les chauffeurs peuvent scanner des documents depuis mobile avec OCR assisté Mistral, validation humaine obligatoire, et fonctionnement offline-first avec sync automatique au retour réseau. Temps cible: <30 secondes end-to-end.

**FRs covered:** FR17, FR18, FR19, FR20, FR21, FR22, FR23, FR24

**Integration Requirements:** Mistral OCR API (French), IndexedDB offline storage, Service Workers

**UX Requirements:** 3-click max, 48x48px buttons, progressive OCR feedback animation, Android 7-8 support

---

### Epic 5: Document Management (Desktop)
Les gestionnaires peuvent uploader des documents via drag-and-drop desktop, importer en masse via ZIP avec OCR matching, et consulter tous les documents de leur flotte. Les chauffeurs consultent les documents de leurs véhicules assignés.

**FRs covered:** FR15, FR17, FR25

**Integration Requirements:** OVH S3 Object Storage avec presigned URLs

---

### Epic 6: Compliance Dashboard & Real-Time Monitoring
Les gestionnaires visualisent l'état de conformité temps réel de leur flotte avec statuts visuels (vert/orange/rouge), calendrier des échéances 30 jours, et métriques d'adoption des chauffeurs. Performance cible: chargement <2 secondes.

**FRs covered:** FR33, FR34, FR35

**Performance Requirements:** NFR-P1 (<2s load), NFR-P2 (<1.5s calendar), Next.js unstable_cache 60s revalidation

**UX Requirements:** Color-coded cards, truck vs trailer icons, cards-based layout

---

### Epic 7: Alerts & Notifications System
Le système calcule automatiquement les échéances (J-60, J-30, J-15, expiration) et envoie des alertes email proactives. Les gestionnaires personnalisent la fréquence et types d'alertes par véhicule. Push notifications PWA pour alertes critiques.

**FRs covered:** FR26, FR27, FR28, FR31, FR32

**Integration Requirements:** Resend Email API, Vercel Cron Jobs (daily calculation), PWA Push Notifications API

---

### Epic 8: Compliance Exports & Reporting
Les gestionnaires exportent des registres de conformité conducteurs (permis, visites médicales, FIMO) et véhicules (cartes grises, assurances, CT) en PDF/Excel avec sélection de périodes custom pour audits URSSAF et comptabilité.

**FRs covered:** FR36, FR37, FR38

---

### Epic 9: Billing & Subscription Management
Le système gère la facturation automatique avec pricing différencié évolutif (moteurs: 4€→3€→2,50€ selon paliers, remorques: 1,50€ fixe), add-ons (OCR 0,10€/doc), TVA automatique, et auto-renouvellement avec reminder J-7.

**FRs covered:** FR47, FR48, FR49, FR50, FR51, FR52, FR53, FR54

**Integration Requirements:** LemonSqueezy billing integration (à configurer)

---

### Epic 10: Driver Adoption & Gamification
Le système guide les chauffeurs lors du premier scan avec tooltips contextuels et active automatiquement la gamification (badges "Chauffeur exemplaire", classements) si entreprise >10 users OU >5 scans/mois.

**FRs covered:** FR44, FR45, FR46

**UX Requirements:** Spectaculaire first-scan animation, video tutorial 90s skippable

---

### Epic 11: Third-Party Access & Multi-Client Management
Les entreprises accordent des accès lecture seule à des tiers externes (comptables, auditeurs) avec permissions granulaires. Les comptables gèrent plusieurs clients avec un seul compte et exportent les registres de conformité.

**FRs covered:** FR9, FR10, FR39

---

### Epic 12: SMS Alerts Add-On
Les entreprises activent l'add-on SMS alertes (0,50€/véhicule/mois) en self-service. Les chauffeurs reçoivent des alertes SMS sur leur numéro enregistré pour les échéances critiques.

**FRs covered:** FR29, FR30

**Integration Requirements:** OVH SMS API (déjà configuré pour onboarding)

---

### Epic 13: Audit Logs & Compliance Tracking (P1)
Le système trace tous les accès et modifications de documents dans des audit logs immuables avec horodatage UTC. Les admins consultent les logs filtrés par utilisateur/date/action. Conservation 3 ans minimum pour conformité URSSAF.

**FRs covered:** FR56, FR57, FR58

**Security Requirements:** NFR-S15, NFR-S16, NFR-S17 (logs immuables, 3 ans retention)

---

### Epic 14: Super Admin Analytics & Health Monitoring
Le Super Admin (Quentin) monitore la santé globale du SaaS avec analytics dashboard (MRR, churn, ARPU, NPS), identifie les clients à risque (health score <40/100), track l'adoption des features, et active le mode démo pour présentations commerciales.

**FRs covered:** FR62, FR63, FR64, FR65, FR66

---

## Epic 1: Project Foundation & Authentication

Les gestionnaires peuvent créer un compte entreprise, s'authentifier via email/password ou Google OAuth, et accéder à l'application sécurisée. Le projet est initialisé avec le starter template Better Auth incluant Next.js 15+, Prisma, shadcn/ui, et l'infrastructure multi-tenant.

### Story 1.1: Initialize Project with Better Auth Starter Template

As a developer,
I want to initialize the project using the Better Auth starter template with all required dependencies,
So that the foundation is set with Next.js 15+, Prisma, Better Auth, and shadcn/ui pre-configured.

**Acceptance Criteria:**

**Given** the Better Auth starter template repository (https://github.com/devAaus/better-auth)
**When** I clone and initialize the project
**Then** the project structure includes:
- Next.js 15+ App Router configured with TypeScript strict mode
- Better Auth library installed and base configuration present
- Prisma ORM with PostgreSQL adapter configured
- shadcn/ui component library installed with TailwindCSS 4.x
- .env.example file with required environment variables template

**And** I can run `pnpm install` successfully without errors

**And** I can run `pnpm run build` and the project compiles without TypeScript errors

**And** .env file is configured with:
- `DATABASE_URL` pointing to OVH PostgreSQL instance
- `BETTER_AUTH_SECRET` generated and set
- `GOOGLE_CLIENT_ID` and `GOOGLE_CLIENT_SECRET` placeholders added

**And** Prisma migrations can be generated with `pnpm prisma generate`

**And** database schema can be pushed with `pnpm prisma db push`

**And** the development server starts successfully on `http://localhost:3000`

---

### Story 1.2: Configure Multi-Tenant Database Schema & Isolation

As a developer,
I want to configure the database schema with multi-tenant isolation using Prisma middleware,
So that each company's data is completely isolated and no cross-tenant data leakage is possible.

**Acceptance Criteria:**

**Given** Prisma is initialized from Story 1.1
**When** I define the base multi-tenant schema
**Then** the Prisma schema includes:
- `Organization` model with fields: id (UUID), name, createdAt, updatedAt
- `User` model with fields: id (UUID), organizationId (FK), email (nullable), username (nullable), role (enum: ADMIN, MANAGER, DRIVER), passwordHash, phone, name, firstName, createdAt, updatedAt
- Unique constraint on `(organizationId, email)` for users with email
- Unique constraint on `(organizationId, username)` for users with username

**And** Prisma middleware is configured in `lib/db.ts` to:
- Automatically inject `organizationId` filter on all READ queries
- Require explicit `organizationId` in all UPDATE/DELETE WHERE clauses
- Block any query attempting to access data without organizationId context

**And** I can run a test query that verifies:
- Query without orgId context throws error
- Query with orgId only returns data for that organization
- Attempting to query another org's data returns empty results

**And** database migrations are generated and applied successfully

**And** Prisma Client is regenerated with the new schema types

---

### Story 1.3: Implement Company Registration with Email/Password

As a gestionnaire,
I want to create a company account using email and password,
So that I can register my company and start using FlotteBox.

**Acceptance Criteria:**

**Given** I am on the registration page `/register`
**When** I fill in the registration form with:
- Company name (required)
- Email address (required, valid format)
- Password (required, min 12 characters, must include uppercase, lowercase, number, special char)
- Password confirmation (must match)
- RGPD consent checkbox (required)
**Then** a new `Organization` record is created in the database

**And** a new `User` record is created with:
- Role: ADMIN
- organizationId: linked to the new organization
- passwordHash: bcrypt hashed with cost factor 12
- email: as provided

**And** I am automatically logged in with a valid session

**And** I am redirected to the dashboard page `/dashboard`

**And** Better Auth session cookie is set with secure flags (httpOnly, secure, sameSite)

**And** if email already exists for another organization, I see error: "Cet email est déjà utilisé"

**And** if password is weak, I see error: "Le mot de passe doit contenir au moins 12 caractères, une majuscule, une minuscule, un chiffre et un caractère spécial"

**And** if RGPD consent is not checked, registration is blocked with message: "Vous devez accepter la politique de confidentialité"

---

### Story 1.4: Implement OAuth Google Authentication

As a gestionnaire,
I want to authenticate using my Google account,
So that I can quickly register/login without managing a password.

**Acceptance Criteria:**

**Given** I am on the login page `/login` or registration page `/register`
**When** I click the "Continuer avec Google" button
**Then** I am redirected to Google OAuth consent screen

**And** after granting permissions, I am redirected back to FlotteBox with OAuth code

**And** if this is my first login (email not in database):
- A new `Organization` is created with name extracted from Google account
- A new `User` is created with role ADMIN, email from Google, and organizationId

**And** if my email already exists in the database:
- I am logged into the existing organization
- No duplicate user is created

**And** I am redirected to `/dashboard` with active session

**And** Better Auth stores OAuth tokens securely

**And** if OAuth fails or is cancelled, I see error: "Authentification Google échouée. Veuillez réessayer."

**And** GOOGLE_CLIENT_ID and GOOGLE_CLIENT_SECRET are read from environment variables

---

### Story 1.5: Implement User Login & Session Management

As a gestionnaire,
I want to login using my email and password,
So that I can access my company's FlotteBox dashboard.

**Acceptance Criteria:**

**Given** I have previously registered with email/password (Story 1.3)
**When** I visit `/login` and enter my email and password
**Then** Better Auth validates credentials against bcrypt hash

**And** if credentials are correct:
- A new session is created with 24h expiration (for ADMIN/MANAGER roles)
- Session cookie is set with secure flags
- I am redirected to `/dashboard`

**And** if credentials are incorrect:
- Login attempt is logged (for rate limiting)
- I see error: "Email ou mot de passe incorrect"
- After 5 failed attempts, account is temporarily locked for 15 minutes (NFR-S13)

**And** if account is locked, I see error: "Compte temporairement bloqué. Réessayez dans 15 minutes."

**And** when I click "Mot de passe oublié?", I am redirected to `/forgot-password`

**And** logout functionality is available via `/api/auth/logout` endpoint that:
- Destroys the session
- Clears the session cookie
- Redirects to `/login`

---

### Story 1.6: Implement RGPD Consent Management & Privacy UI

As a user,
I want to view and manage my RGPD privacy settings,
So that I can exercise my data privacy rights (access, rectification, deletion).

**Acceptance Criteria:**

**Given** I am logged in as any user type
**When** I navigate to `/settings/privacy`
**Then** I see the RGPD privacy dashboard with sections:
- "Politique de confidentialité" (link to full policy)
- "Vos données personnelles" (summary of what data is collected)
- "Vos droits RGPD" with actions:
  - "Accéder à mes données" (download personal data as JSON)
  - "Rectifier mes données" (link to profile edit)
  - "Supprimer mon compte" (delete account with confirmation)

**And** when I click "Accéder à mes données":
- API endpoint `/api/rgpd/export` generates JSON export of all my personal data
- Export includes: user info, organization info (if admin), activity logs
- Download starts automatically

**And** when I click "Supprimer mon compte":
- Confirmation modal appears: "Êtes-vous sûr de vouloir supprimer votre compte? Cette action est irréversible."
- If I confirm, API endpoint `/api/rgpd/delete` is called
- My user record is anonymized (email, phone, name replaced with anonymized values)
- Anonymization completes within 24h (NFR-S4)
- I am logged out and redirected to `/login` with message: "Votre compte a été supprimé"

**And** if I am an ADMIN and try to delete my account while having active drivers:
- Deletion is blocked with error: "Vous devez d'abord supprimer tous les comptes chauffeurs avant de supprimer votre compte entreprise"

**And** during registration (`/register`), RGPD consent checkbox is displayed with text:
"J'accepte la [Politique de confidentialité](#) et les [Conditions Générales d'Utilisation](#)"

**And** consent timestamp is stored in database when user registers

---

### Story 1.7: Implement 2FA Configuration for Admin Users

As an admin user,
I want to configure Two-Factor Authentication (2FA) using TOTP,
So that my account has an additional layer of security.

**Acceptance Criteria:**

**Given** I am logged in as an ADMIN user
**When** I navigate to `/settings/security`
**Then** I see a "Configurer l'authentification à deux facteurs (2FA)" section

**And** when I click "Activer 2FA":
- A QR code is generated using TOTP standard (RFC 6238)
- QR code is displayed with instructions: "Scannez ce code avec Google Authenticator ou Authy"
- A secret key is shown as text backup: "Clé secrète: ABCD-EFGH-IJKL-MNOP"

**And** I am prompted to enter a 6-digit verification code from my authenticator app

**And** when I submit a valid code:
- 2FA is enabled for my account
- Database field `twoFactorEnabled` is set to true
- Recovery codes (10 single-use codes) are generated and displayed
- I see success message: "2FA activé avec succès. Conservez vos codes de récupération en lieu sûr."

**And** when I submit an invalid code:
- I see error: "Code invalide. Veuillez réessayer."
- I can attempt verification again

**And** after 2FA is enabled, subsequent logins require:
- Email + password validation first
- Then 6-digit TOTP code input
- Only after valid TOTP code is session created

**And** I can disable 2FA by:
- Navigating to `/settings/security`
- Clicking "Désactiver 2FA"
- Entering current TOTP code to confirm
- 2FA is disabled and `twoFactorEnabled` is set to false

**And** if I lose access to my authenticator, I can use one of my recovery codes:
- Each recovery code can only be used once
- After use, the code is marked as consumed in database

---

## Epic 2: Fleet Management Core

Les gestionnaires peuvent créer leur flotte de véhicules avec distinction moteurs/remorques, importer en masse via CSV, et gérer les modifications. Le système compte automatiquement les véhicules actifs pour la facturation différenciée.

### Story 2.1: Create Vehicle Entity & Add Single Vehicle

As a gestionnaire,
I want to add individual vehicles to my fleet with motor/trailer distinction,
So that I can start managing vehicle documentation.

**Acceptance Criteria:**

**Given** I am logged in as ADMIN or MANAGER
**When** I navigate to `/vehicles` and click "Ajouter un véhicule"
**Then** a form is displayed with fields:
- Immatriculation (required, unique per organization)
- Type: Motor or Trailer (radio buttons, required)
- If Motor selected: Sub-type dropdown (Camion, VUL, Voiture, Engin)
- If Trailer selected: Sub-type shows "Remorque"
- Make/Brand (optional)
- Model (optional)
- Year (optional, numeric YYYY)
- Status: Active/Inactive (default: Active)

**And** the Prisma schema includes `Vehicle` model with:
- id (UUID), organizationId (FK), registration, type (enum: MOTOR, TRAILER), subType, make, model, year, status, createdAt, updatedAt
- Unique constraint on `(organizationId, registration)`

**And** when I submit the form with valid data:
- A new `Vehicle` record is created with my organizationId
- I see success message: "Véhicule ajouté avec succès"
- I am redirected to vehicle list showing the new vehicle

**And** if registration already exists in my organization:
- Form shows error: "Cette immatriculation existe déjà dans votre flotte"

**And** vehicle type icons are displayed:
- 🚛 for Camion
- 🚐 for VUL
- 🚗 for Voiture
- 🚜 for Engin
- 🚚 for Remorque

---

### Story 2.2: Import Vehicles in Bulk via CSV Upload

As a gestionnaire,
I want to import multiple vehicles at once using a CSV file,
So that I can onboard my entire fleet quickly (<2 hours for 50 vehicles).

**Acceptance Criteria:**

**Given** I am on `/vehicles` page
**When** I click "Importer CSV"
**Then** I see a modal with:
- Download template CSV link (pre-filled with column headers)
- File upload dropzone
- Instructions: "Format: immatriculation, type (MOTOR/TRAILER), subType, make, model, year"

**And** template CSV contains headers: `registration,type,subType,make,model,year`

**And** when I upload a valid CSV file:
- System validates each row
- Duplicates within CSV are detected (same registration)
- Duplicates with existing vehicles are detected
- Invalid types are flagged

**And** I see a preview table showing:
- Valid rows (green checkmark)
- Duplicate rows (orange warning with "Déjà dans la flotte")
- Invalid rows (red X with error message)
- Summary: "X véhicules valides, Y doublons, Z invalides"

**And** when I click "Confirmer l'import":
- Only valid, non-duplicate vehicles are created
- Bulk insert completes in <5 seconds for 50 vehicles
- Success message shows: "X véhicules importés avec succès"

**And** if CSV has no valid rows:
- Import button is disabled
- Message shows: "Aucun véhicule valide à importer"

---

### Story 2.3: Edit and Delete Vehicle Records

As a gestionnaire,
I want to edit or delete vehicle information,
So that I can keep my fleet data up-to-date.

**Acceptance Criteria:**

**Given** I am on the vehicles list page `/vehicles`
**When** I click the edit icon on a vehicle row
**Then** an edit modal opens pre-filled with current vehicle data

**And** I can modify any field except organizationId

**And** when I save changes:
- Vehicle record is updated
- updatedAt timestamp is refreshed
- Success message: "Véhicule modifié avec succès"
- List is refreshed to show updated data

**And** when I click the delete icon (trash):
- Confirmation modal appears: "Êtes-vous sûr de vouloir supprimer ce véhicule ?"
- Modal warns if vehicle has associated documents: "Ce véhicule a X documents associés"

**And** when I confirm deletion:
- Vehicle record is soft-deleted (status = DELETED) if it has documents
- Vehicle record is hard-deleted if it has no documents
- Success message: "Véhicule supprimé"
- List is refreshed

**And** if vehicle is assigned to a driver:
- Deletion is blocked with error: "Ce véhicule est assigné à un chauffeur. Veuillez d'abord supprimer l'assignation."

---

### Story 2.4: Auto-Count Active Motors & Trailers for Billing

As a developer,
I want the system to automatically count active motors and trailers per organization,
So that billing can be calculated accurately based on fleet composition.

**Acceptance Criteria:**

**Given** an organization has multiple vehicles with different types
**When** I query the vehicle count for billing
**Then** an API endpoint `/api/billing/vehicle-counts` returns:
```json
{
  "organizationId": "uuid",
  "activeMotors": 15,
  "activeTrailers": 8,
  "inactiveVehicles": 2,
  "totalVehicles": 25
}
```

**And** the count includes only vehicles with status = ACTIVE

**And** motors count includes types: MOTOR with subTypes (Camion, VUL, Voiture, Engin)

**And** trailers count includes type: TRAILER

**And** the count query is optimized with database index on `(organizationId, type, status)`

**And** when a vehicle status changes from ACTIVE to INACTIVE:
- Count is automatically updated on next billing calculation
- No manual intervention required

**And** count is cached for 60 seconds using Next.js `unstable_cache`

**And** cache is invalidated when:
- New vehicle is added
- Vehicle is deleted
- Vehicle status changes

---

## Epic 3: Driver Management & Onboarding

Les gestionnaires peuvent créer des comptes chauffeurs avec identifiants custom (sans email requis), les assigner aux véhicules, et les onboarder automatiquement via SMS contenant les credentials et le lien PWA avec QR code.

### Story 3.1: Create Driver Accounts with Custom Identifiers

As a gestionnaire,
I want to create driver accounts using custom identifiers without requiring email addresses,
So that I can onboard drivers who may not have email access.

**Acceptance Criteria:**

**Given** I am logged in as ADMIN or MANAGER
**When** I navigate to `/drivers` and click "Ajouter un chauffeur"
**Then** a form is displayed with fields:
- Username/Identifier (required, custom format, unique per organization)
- Password (auto-generated, can be manually edited)
- First name (required)
- Last name (required)
- Phone number (required, French format validation)
- Email (optional)

**And** the `User` model from Story 1.2 is extended to support:
- Nullable email field
- Non-nullable username field for drivers
- Role: DRIVER

**And** when I submit the form:
- A new `User` record is created with role=DRIVER and my organizationId
- Password is hashed with bcrypt cost factor 12
- Unique constraint `(organizationId, username)` is enforced
- Success message: "Chauffeur créé avec succès. Identifiant: [username]"

**And** username validation enforces:
- Min 4 characters, max 20 characters
- Alphanumeric only (no spaces or special chars)
- Case-insensitive uniqueness

**And** if username already exists in my organization:
- Form shows error: "Cet identifiant est déjà utilisé"

**And** phone number validation:
- Must be valid French mobile format (06/07 XX XX XX XX)
- Must be unique per organization

---

### Story 3.2: Assign Drivers to Vehicles

As a gestionnaire,
I want to assign drivers to specific vehicles,
So that drivers can access and scan documents for their assigned vehicles.

**Acceptance Criteria:**

**Given** I have created vehicles (Epic 2) and drivers (Story 3.1)
**When** I navigate to `/drivers` and click "Assigner véhicules" on a driver row
**Then** a modal displays:
- Driver name and identifier
- List of all active vehicles in my organization
- Checkboxes next to each vehicle
- Currently assigned vehicles are pre-checked

**And** Prisma schema includes `VehicleAssignment` model:
- id (UUID), organizationId (FK), userId (FK to driver), vehicleId (FK), assignedAt, createdAt

**And** when I save assignments:
- Previous assignments for this driver are removed
- New assignments are created based on selections
- Success message: "Assignations mises à jour"

**And** a driver can be assigned to multiple vehicles

**And** on driver detail page `/drivers/[id]`, I see:
- List of assigned vehicles with icons
- Quick "Ajouter/Retirer" buttons

**And** when I delete a vehicle assignment:
- Confirmation modal: "Retirer le véhicule [registration] de ce chauffeur?"
- Assignment record is deleted
- Driver can no longer see this vehicle in mobile app

---

### Story 3.3: Driver Authentication with Username + Password

As a chauffeur,
I want to login using my custom identifier and password,
So that I can access the mobile PWA without needing an email address.

**Acceptance Criteria:**

**Given** my gestionnaire has created my driver account (Story 3.1)
**When** I visit the mobile PWA login page `/login`
**Then** I see a simplified login form:
- "Identifiant" field (not "Email")
- "Mot de passe" field
- "Se connecter" button
- No "Continuer avec Google" option (drivers use username only)

**And** when I enter my username and password:
- Better Auth validates against username field (not email)
- Username lookup is case-insensitive
- Session is created with 7-day expiration (NFR-S12 for drivers)

**And** if credentials are correct:
- I am redirected to `/driver/dashboard`
- Session cookie is set

**And** if credentials are incorrect:
- I see error: "Identifiant ou mot de passe incorrect"
- After 5 failed attempts, account is locked for 15 minutes

**And** drivers cannot access:
- `/vehicles` management
- `/drivers` management
- `/settings/security` (no 2FA for drivers)
- Billing pages

**And** drivers can only access:
- Their own profile (read-only per FR6)
- Scanner page
- Their assigned vehicles' documents

---

### Story 3.4: Read-Only Profile Access for Drivers

As a chauffeur,
I want to view my profile information but not modify it,
So that I can see my details without accidentally changing them.

**Acceptance Criteria:**

**Given** I am logged in as a driver
**When** I navigate to `/driver/profile`
**Then** I see my information displayed as read-only text:
- Username/Identifier
- First name, Last name
- Phone number
- Email (if provided)
- Assigned vehicles list

**And** all fields are displayed as text (not input fields)

**And** I see a message: "Pour modifier vos informations, contactez votre gestionnaire"

**And** if I attempt to access `/settings/profile/edit`:
- I am redirected to `/driver/profile`
- Toast message: "Vous ne pouvez pas modifier votre profil. Contactez votre gestionnaire."

**And** API endpoint `/api/driver/profile` has authorization check:
- If role=DRIVER and attempting UPDATE: 403 Forbidden
- If role=DRIVER and requesting GET: 200 OK

---

### Story 3.5: Deactivate and Delete Driver Accounts

As a gestionnaire,
I want to deactivate or delete driver accounts,
So that I can manage driver turnover efficiently.

**Acceptance Criteria:**

**Given** I am on `/drivers` page
**When** I click the toggle switch on a driver row
**Then** the driver account status changes between ACTIVE/INACTIVE

**And** when status is INACTIVE:
- Driver cannot login (credentials rejected with message: "Compte désactivé")
- Driver's vehicle assignments remain in database
- Driver appears in list with "Inactif" badge

**And** when I reactivate (toggle back to ACTIVE):
- Driver can login immediately
- All previous assignments are still valid

**And** when I click delete icon (trash) on a driver:
- Confirmation modal: "Supprimer le chauffeur [name]?"
- Modal shows: "[X] documents scannés par ce chauffeur seront conservés"

**And** when I confirm deletion:
- `User` record is soft-deleted if driver has scanned documents
- Hard-deleted if no documents
- Vehicle assignments are deleted
- Sessions are invalidated (driver is logged out)

**And** deleted drivers no longer appear in list (unless "Afficher supprimés" filter is enabled)

---

### Story 3.6: Generate QR Code for PWA Installation

As a gestionnaire,
I want to generate a QR code for PWA installation,
So that drivers can quickly install the mobile app by scanning.

**Acceptance Criteria:**

**Given** I am on `/drivers` page or dashboard
**When** I click "Installer l'app mobile" button
**Then** a modal displays:
- Large QR code (300x300px minimum)
- PWA installation URL: `https://flottebox.app/install`
- Instructions: "Scannez ce code avec l'appareil photo de votre smartphone"
- "Télécharger le QR code" button (saves as PNG)

**And** the QR code encodes the PWA manifest URL

**And** when a driver scans the QR code:
- Their mobile browser opens the PWA URL
- Browser shows "Add to Home Screen" prompt automatically
- After adding, PWA icon appears on home screen with FlotteBox logo

**And** PWA manifest (`/manifest.json`) includes:
```json
{
  "name": "FlotteBox Chauffeur",
  "short_name": "FlotteBox",
  "start_url": "/login",
  "display": "standalone",
  "theme_color": "#1E40AF",
  "icons": [...]
}
```

**And** PWA is installable on:
- iOS 12+ (Safari)
- Android 8+ (Chrome)

---

### Story 3.7: Send Onboarding SMS with Credentials

As a gestionnaire,
I want to automatically send an SMS to new drivers with their login credentials and PWA link,
So that onboarding takes less than 2 minutes per driver.

**Acceptance Criteria:**

**Given** I have just created a new driver account (Story 3.1)
**When** the driver is successfully created
**Then** I see a checkbox option: "Envoyer SMS d'onboarding" (checked by default)

**And** when I confirm with SMS option checked:
- An SMS is sent via OVH SMS API to the driver's phone number
- SMS content:
```
Bienvenue sur FlotteBox!
Identifiant: [username]
Mot de passe: [password]
Installer l'app: https://flottebox.app/install
```

**And** OVH SMS API integration (`lib/sms.ts`) includes:
- Endpoint: `https://eu.api.ovh.com/1.0/sms/send`
- Authentication: X-Ovh-Application header + signature
- Rate limiting: Max 10 SMS per minute per organization (anti-abuse)

**And** if SMS send fails:
- Driver account is still created
- Warning message: "Le SMS n'a pas pu être envoyé. Vérifiez le numéro de téléphone."
- I can retry SMS from driver detail page

**And** SMS send is logged in database (`SmsLog` table):
- organizationId, userId, phoneNumber, content, sentAt, status (SENT/FAILED), errorMessage

**And** rate limiting logic:
- If organization exceeds 10 SMS/min: HTTP 429 error
- Error message: "Limite d'envoi SMS dépassée. Réessayez dans 1 minute."

---

### Story 3.8: Install PWA via QR Code (Driver Flow)

As a chauffeur,
I want to install the FlotteBox PWA on my smartphone by scanning a QR code,
So that I can access the app without going through app stores.

**Acceptance Criteria:**

**Given** my gestionnaire has shared the QR code (Story 3.6)
**When** I scan the QR code with my phone camera
**Then** my browser opens `https://flottebox.app/install`

**And** the `/install` page displays:
- FlotteBox logo
- "Installer FlotteBox sur votre téléphone" title
- "Ajouter à l'écran d'accueil" button (triggers browser install prompt)
- Visual instructions with screenshots

**And** when I tap "Ajouter à l'écran d'accueil":
- iOS: Browser shows native "Add to Home Screen" sheet
- Android: Browser shows native install banner
- After confirming, PWA is installed

**And** PWA icon appears on my home screen with FlotteBox branding

**And** when I open the installed PWA:
- App opens in standalone mode (no browser UI)
- I am directed to `/login` page
- I can login with my username + password from SMS

**And** installation completes in <1 minute (NFR-U1)

---

### Story 3.9: Send Tutorial Video SMS to New Drivers

As a gestionnaire,
I want the system to automatically send a 90-second tutorial video to new drivers,
So that they understand how to scan documents without training.

**Acceptance Criteria:**

**Given** a new driver has just been created and onboarding SMS sent (Story 3.7)
**When** SMS is successfully delivered
**Then** a follow-up SMS is sent 5 minutes later with tutorial link:
```
📹 Regardez comment scanner un document (90 secondes):
https://flottebox.app/tutorial
```

**And** the tutorial video page `/tutorial` displays:
- Embedded video player (90 seconds max)
- Video content: "Comment scanner votre premier document"
- Steps shown: Ouvrir app → Clic Scanner → Photo document → Valider
- Video is hosted on efficient CDN (not database)

**And** video is optimized for mobile data:
- Max 10MB file size
- 720p resolution
- MP4 format with H.264 codec

**And** video is skippable at any time

**And** when driver watches video:
- View is tracked (`TutorialView` table: userId, viewedAt, completionPercentage)
- Gestionnaire can see adoption metrics: "X/Y chauffeurs ont vu le tutoriel"

**And** driver can replay video anytime from `/driver/help`

---

### Story 3.10: Driver Management Dashboard Summary

As a gestionnaire,
I want to see a summary of all my drivers and their status,
So that I can quickly identify who needs attention.

**Acceptance Criteria:**

**Given** I am on `/drivers` page
**When** the page loads
**Then** I see a summary card at the top:
- Total drivers: X
- Active: Y (green)
- Inactive: Z (gray)
- Unassigned (no vehicles): W (orange warning)

**And** I see a table of all drivers with columns:
- Name (firstName + lastName)
- Username/Identifier
- Phone number
- Status (Active/Inactive toggle)
- Assigned vehicles count (with icon)
- Last login date
- Actions (Edit, Assign vehicles, Send SMS, Delete)

**And** I can filter drivers by:
- Status (All / Active / Inactive)
- Has vehicles (Yes / No / Any)
- Last login (Last 7 days / Last 30 days / Never)

**And** I can search drivers by name or username

**And** table is paginated (20 drivers per page)

**And** when I click on a driver row:
- Driver detail page opens showing:
  - Profile information
  - Assigned vehicles list
  - Recent scans (last 10 documents)
  - Tutorial video status
  - SMS history

---

---

## Epic 4: Document Scanning & OCR (Mobile PWA)

Epic goal: Allow drivers to scan documents using their smartphone camera, automatically extract information via OCR, and enable offline-first operation.

**FRs covered:** FR17, FR18, FR19, FR20, FR21, FR22, FR23, FR24, NFR-P2, NFR-P3, NFR-U1, NFR-U2

---

### Story 4.1: Create Document Entity & OVH S3 Integration

As a developer,
I want to create the Document entity with OVH S3 Object Storage integration,
So that scanned documents are stored securely and accessible via presigned URLs.

**Acceptance Criteria:**

**Given** the Prisma schema needs a `Document` entity
**When** I create the Document model
**Then** it contains the following fields:
- id (String, cuid)
- organizationId (String, for multi-tenancy)
- vehicleId (String, references Vehicle)
- uploadedByUserId (String, references User - driver or gestionnaire)
- documentType (Enum: CARTE_GRISE, ASSURANCE, CONTROLE_TECHNIQUE, VISITE_TECHNIQUE, VGP, ATTESTATION_CONFORMITE, PERMIS, AUTRE)
- originalFileName (String)
- s3Key (String, unique) - Object key in OVH S3
- s3Bucket (String) - "flottebox-documents-prod"
- fileSize (Int, bytes)
- mimeType (String, e.g., "image/jpeg")
- uploadedAt (DateTime)
- expirationDate (DateTime, nullable) - for compliance tracking
- ocrStatus (Enum: PENDING, PROCESSING, COMPLETED, FAILED, ILLEGIBLE, VALIDATED)
- ocrRawData (Json, nullable) - raw Mistral OCR response
- ocrExtractedText (String, nullable)
- validatedAt (DateTime, nullable)
- validatedByUserId (String, nullable, references User)
- isIllegible (Boolean, default false)
- notes (String, nullable)
- createdAt, updatedAt

**And** Prisma middleware injects organizationId for all Document queries

**And** environment variables are configured:
```
OVH_S3_ENDPOINT=https://s3.gra.io.cloud.ovh.net
OVH_S3_REGION=gra
OVH_S3_ACCESS_KEY_ID=<from OVH console>
OVH_S3_SECRET_ACCESS_KEY=<from OVH console>
OVH_S3_BUCKET=flottebox-documents-prod
```

**And** a utility function `generatePresignedUrl(s3Key: string, expiresIn: number)` is created:
- Returns a signed URL valid for specified duration (default 3600s)
- Uses AWS SDK v3 for S3 client
- Handles OVH S3 endpoint configuration

**And** migration is created and applied to PostgreSQL

---

### Story 4.2: Mobile PWA Camera Scan Interface with Large Touch Targets

As a chauffeur,
I want to scan a document using my smartphone camera with large, easy-to-tap buttons,
So that I can quickly capture documents even with low digital literacy.

**Acceptance Criteria:**

**Given** I am logged into the PWA as a driver
**When** I navigate to `/driver/scan`
**Then** I see a full-screen camera interface with:
- Live camera preview (rear camera by default)
- "Capturer" button (48x48px minimum, green, centered at bottom)
- "Annuler" button (48x48px minimum, gray, top-left corner)
- Camera flip icon (48x48px, top-right) to switch front/rear camera
- Grid overlay (3x3 rule of thirds) to help alignment
- Device orientation lock (portrait mode)

**And** camera permissions are requested on first access:
- If denied: Show error message "Accès caméra requis pour scanner"
- If granted: Camera preview starts automatically

**And** when I tap "Capturer":
- Camera captures image at high resolution (min 1920x1080)
- Image is displayed in preview mode
- I see two buttons (48x48px each):
  - "Valider" (green checkmark) - proceeds to document type selection
  - "Reprendre photo" (orange retry icon) - returns to camera

**And** image is compressed before upload:
- JPEG format, 85% quality
- Max 2MB file size
- Maintains EXIF orientation data

**And** when I tap "Valider":
- I am directed to `/driver/scan/select-type` with image data

**And** the interface works on Android 7-8+ devices (NFR-U2)

**And** all buttons follow 48x48px minimum size (NFR-U3)

---

### Story 4.3: Mistral OCR API Integration & French Text Extraction

As a developer,
I want to integrate Mistral OCR API to extract French text from scanned documents,
So that document information is automatically populated.

**Acceptance Criteria:**

**Given** a driver has captured a document image
**When** the image is uploaded to OVH S3
**Then** an API route `/api/ocr/process` is triggered with:
- s3Key of the uploaded image
- documentId
- documentType (selected by driver)

**And** the OCR processing function:
1. Generates presigned URL for the image (valid 5 minutes)
2. Calls Mistral OCR API endpoint with:
   - Image URL
   - Language hint: "fr" (French)
   - Document type hint (e.g., "carte_grise", "assurance")
3. Receives OCR response with extracted text and structured data

**And** Mistral OCR API credentials are configured:
```
MISTRAL_OCR_API_KEY=<from Mistral console>
MISTRAL_OCR_ENDPOINT=https://api.mistral.ai/v1/ocr
```

**And** OCR response is stored in `Document.ocrRawData` (JSON format)

**And** extracted text is stored in `Document.ocrExtractedText`

**And** `Document.ocrStatus` is updated:
- PROCESSING → COMPLETED (if successful)
- PROCESSING → FAILED (if API error)
- PROCESSING → ILLEGIBLE (if confidence < 60%)

**And** error handling:
- If Mistral API timeout (>10s): Retry once, then mark FAILED
- If HTTP 429 (rate limit): Retry with exponential backoff
- If HTTP 500 (server error): Mark FAILED, log error to Sentry

**And** cost tracking:
- Each OCR call is logged in `OcrUsageLog` table:
  - organizationId, documentId, apiCallCost (€0.10), calledAt, status

**And** NFR-P2: OCR processing completes in <10 seconds

---

### Story 4.4: Human Validation Workflow with Pre-filled Form

As a chauffeur,
I want to review and validate OCR-extracted information before saving,
So that I can correct any errors and ensure accuracy.

**Acceptance Criteria:**

**Given** OCR processing has completed (Story 4.3)
**When** I am on `/driver/scan/validate/:documentId`
**Then** I see a form pre-filled with OCR-extracted data:
- Document type (dropdown, pre-selected)
- Vehicle (dropdown, shows my assigned vehicles only)
- Expiration date (date picker, pre-filled if OCR found date)
- Extracted text preview (read-only, scrollable)
- Document thumbnail (shows captured image, 200x150px)

**And** form fields are large and touch-friendly:
- Input height: 48px minimum
- Font size: 16px minimum (no zoom on iOS)
- Date picker opens native mobile keyboard

**And** document type dropdown shows relevant types:
- Carte Grise
- Assurance
- Contrôle Technique
- Visite Technique
- VGP
- Attestation de Conformité
- Permis de Conduire
- Autre

**And** when I tap "Enregistrer" (48x48px button):
- Document status changes: COMPLETED → VALIDATED
- `validatedAt` timestamp is set
- `validatedByUserId` is set to my userId
- If offline: Document is saved to IndexedDB (Story 4.7)
- If online: Document is saved to PostgreSQL immediately
- Success message: "Document enregistré avec succès"
- I am redirected to `/driver/dashboard`

**And** when I tap "Marquer comme illisible":
- Document status changes to ILLEGIBLE
- `isIllegible` flag is set to true
- I am prompted: "Pourquoi est-il illisible?" (optional notes field)
- Gestionnaire is notified via dashboard alert
- I am redirected to `/driver/dashboard`

**And** validation completes in <5 seconds (NFR-P3)

---

### Story 4.5: Automatic Document Type Classification

As a developer,
I want the system to automatically suggest the document type based on OCR text analysis,
So that drivers don't have to manually select the type every time.

**Acceptance Criteria:**

**Given** OCR has extracted text from a document
**When** the OCR response is processed
**Then** a classification function analyzes `ocrExtractedText` for keywords:
- "CERTIFICAT D'IMMATRICULATION" or "CARTE GRISE" → CARTE_GRISE
- "ATTESTATION D'ASSURANCE" or "POLICE D'ASSURANCE" → ASSURANCE
- "PROCÈS-VERBAL DE CONTRÔLE TECHNIQUE" or "CT" → CONTROLE_TECHNIQUE
- "VGP" or "VÉRIFICATION GÉNÉRALE PÉRIODIQUE" → VGP
- "VISITE TECHNIQUE" → VISITE_TECHNIQUE
- "PERMIS DE CONDUIRE" → PERMIS

**And** classification confidence score is calculated (0-100%)

**And** if confidence > 80%:
- Document type is auto-selected in validation form
- User sees badge: "Type détecté automatiquement"
- User can still change it manually

**And** if confidence < 80%:
- Document type defaults to "Autre"
- User must select manually

**And** classification logic is implemented as:
```typescript
function classifyDocumentType(text: string): {
  type: DocumentType;
  confidence: number;
} {
  // Keyword matching logic with confidence scoring
}
```

**And** classification results are logged for future ML model training

---

### Story 4.6: Mark Illegible Documents & Notify Gestionnaire

As a chauffeur,
I want to mark a document as illegible if OCR fails or the scan is poor quality,
So that my gestionnaire knows to request a rescan or manual upload.

**Acceptance Criteria:**

**Given** I am on the validation form (`/driver/scan/validate/:documentId`)
**When** I tap "Marquer comme illisible" button (48x48px, orange)
**Then** a confirmation modal appears:
- Title: "Document illisible ?"
- Message: "Vous pouvez reprendre la photo ou demander de l'aide."
- Optional notes field: "Pourquoi est-il illisible ?" (max 200 chars)
- Buttons: "Reprendre photo" (48x48px), "Confirmer illisible" (48x48px)

**And** when I tap "Reprendre photo":
- I am redirected back to `/driver/scan` camera interface
- Previous document is marked as DISCARDED

**And** when I tap "Confirmer illisible":
- Document status is set to ILLEGIBLE
- `isIllegible` flag is set to true
- Optional notes are saved to `Document.notes`
- A notification is created in gestionnaire's dashboard:
  - "⚠️ Document illisible scanné par [Driver Name] pour [Vehicle Registration]"
  - Links to document detail page
- Success message: "Votre gestionnaire a été informé"
- I am redirected to `/driver/dashboard`

**And** gestionnaire can see illegible documents on `/documents` page:
- Filter: "Illisibles uniquement"
- Badge: "Illisible" (orange) next to document
- Can view driver's notes
- Can mark as "Résolu" after manual handling

**And** illegible documents are excluded from compliance calculations

---

### Story 4.7: Offline-First Document Storage with IndexedDB

As a chauffeur,
I want to scan documents offline and have them automatically sync when I reconnect,
So that I can work in areas with poor network coverage.

**Acceptance Criteria:**

**Given** I am using the PWA in offline mode (no network)
**When** I scan a document and complete validation
**Then** the document data is saved to IndexedDB:
- Database name: "flottebox-offline"
- Store name: "pendingDocuments"
- Indexed fields: id, organizationId, vehicleId, createdAt
- Stored data:
  - Document metadata (type, vehicleId, expirationDate, etc.)
  - Base64-encoded image data
  - Validation status
  - Timestamp of creation

**And** image is compressed for offline storage:
- Max 500KB per image (lower quality for offline)
- JPEG format, 70% quality

**And** I see a banner at the top: "Mode hors ligne - X documents en attente de synchronisation"

**And** when network reconnects (Story 4.8):
- Banner updates: "Synchronisation en cours..."
- IndexedDB pending documents are uploaded to server
- After successful upload, documents are removed from IndexedDB
- Banner updates: "✓ Synchronisation terminée"

**And** I can view pending offline documents:
- Navigate to `/driver/offline-queue`
- See list of pending documents with thumbnails
- Badge shows "En attente de sync"
- Can delete pending documents if needed

**And** Service Worker is configured:
- Caches essential PWA assets for offline use
- Routes `/driver/*` pages for offline-first strategy
- Background sync API registration for document upload

**And** IndexedDB storage quota:
- Request persistent storage on first scan
- If quota exceeded (>50MB): Prompt user to sync or delete old documents

---

### Story 4.8: Automatic Sync on Network Reconnection

As a chauffeur,
I want my offline-scanned documents to automatically upload when I reconnect to the internet,
So that I don't have to manually trigger sync.

**Acceptance Criteria:**

**Given** I have pending documents in IndexedDB (offline queue)
**When** my device reconnects to the internet (online event)
**Then** the Service Worker triggers a background sync task:
- Sync tag: "sync-documents"
- Background Sync API initiates upload process

**And** for each pending document in IndexedDB:
1. Upload image to OVH S3 via `/api/documents/upload` endpoint
2. Create Document record in PostgreSQL
3. Trigger OCR processing (Story 4.3)
4. Remove document from IndexedDB after successful upload
5. Update sync progress counter

**And** upload process is resilient:
- If one document fails: Continue with next document
- Failed documents remain in IndexedDB with error status
- User can manually retry failed uploads

**And** sync progress is shown via notification:
- "Synchronisation: 1/3 documents envoyés..."
- PWA push notification when sync completes: "✓ 3 documents synchronisés"

**And** if sync is interrupted (network drops again):
- Progress is saved
- Sync resumes automatically when reconnected
- No duplicate uploads (check documentId)

**And** after successful sync:
- IndexedDB is cleared of uploaded documents
- User sees updated dashboard with new documents
- Compliance status is recalculated

**And** sync completes within 30 seconds for 5 documents (NFR-P3)

**And** sync respects rate limits:
- Max 5 documents uploaded concurrently
- Exponential backoff if server returns 429

---

## Epic 5: Document Management (Desktop)

Les gestionnaires peuvent uploader des documents via drag-and-drop desktop, importer en masse via ZIP avec OCR matching, et consulter tous les documents de leur flotte. Les chauffeurs consultent les documents de leurs véhicules assignés.

### Story 5.1: Desktop Drag-and-Drop Document Upload

As a gestionnaire,
I want to upload documents using drag-and-drop on desktop,
So that I can quickly add multiple documents without using the mobile scanner.

**Acceptance Criteria:**

**Given** I am logged in as ADMIN or MANAGER
**When** I navigate to `/documents/upload`
**Then** I see a drag-and-drop zone with:
- Large dropzone area (400x300px minimum)
- Icon and text: "Glissez vos documents ici ou cliquez pour sélectionner"
- Supported formats listed: "JPG, PNG, PDF (max 10MB par fichier)"
- "Sélectionner fichiers" button as alternative to drag-and-drop

**And** when I drag files over the dropzone:
- Dropzone highlights with blue border
- Visual feedback: "Déposez vos fichiers ici"

**And** when I drop files or select via button:
- Files are validated:
  - Type: image/jpeg, image/png, application/pdf
  - Size: max 10MB per file
  - Invalid files show error: "Format non supporté" or "Fichier trop volumineux"
- Valid files are added to upload queue

**And** I see a preview list of files to upload:
- Thumbnail preview for images
- PDF icon for PDFs
- File name, size, and status (Pending/Uploading/Done)
- Remove button (X) for each file

**And** for each file, I must select:
- Vehicle (dropdown of my organization's vehicles)
- Document type (dropdown: same types as Story 4.4)
- Expiration date (optional, date picker)

**And** when I click "Télécharger tous" (48x48px button):
- Files are uploaded to OVH S3 in parallel (max 3 concurrent uploads)
- Progress bar shows: "Upload en cours: X/Y fichiers"
- Each file upload:
  1. Uploads to S3 via presigned POST URL
  2. Creates Document record in PostgreSQL
  3. Triggers OCR processing (Story 4.3)
  4. Updates status in preview list

**And** after upload completes:
- Success message: "Y documents téléchargés avec succès"
- OCR processing continues in background
- I can navigate away (processing is async)

**And** if upload fails for a file:
- File shows "Échec" status with error message
- I can retry individual failed uploads
- Other files continue uploading

**And** uploaded documents appear in `/documents` list immediately with status "Traitement OCR..."

---

### Story 5.2: View Vehicle Documents List with Presigned URLs

As a gestionnaire or chauffeur,
I want to view all documents for a vehicle,
So that I can check compliance status and download documents if needed.

**Acceptance Criteria:**

**Given** I am logged in as any role
**When** I navigate to `/vehicles/[vehicleId]/documents`
**Then** I see a list of all documents for this vehicle with columns:
- Thumbnail (50x50px, clickable to view full size)
- Document type (with icon)
- Expiration date (if applicable)
- Upload date
- Status (OCR: Validé, En cours, Illisible)
- Uploaded by (user name)
- Actions (View, Download, Edit, Delete)

**And** documents are sorted by:
- Default: expiration date ascending (soonest first)
- Can toggle: upload date, type, status

**And** when I click thumbnail or "View" button:
- Modal opens with full-size document viewer
- Image documents: displayed at full resolution
- PDF documents: embedded PDF viewer with pagination
- Document loaded via presigned URL from OVH S3 (valid 1 hour)

**And** when I click "Download":
- File downloads with original filename
- Uses presigned S3 URL (valid 5 minutes)
- No server processing required

**And** if I am a DRIVER (role=DRIVER):
- I only see documents for vehicles assigned to me (Story 3.2)
- Edit and Delete actions are hidden
- I see message: "Pour modifier un document, contactez votre gestionnaire"

**And** if I am ADMIN or MANAGER:
- I can click "Edit" to modify:
  - Document type
  - Expiration date
  - Notes field
- I can click "Delete" to soft-delete document (Story 4.1)

**And** if document status is ILLEGIBLE:
- Badge shows: "Illisible" (orange)
- Driver's notes are displayed
- I see "Reprendre document" button to re-scan

**And** if vehicle has no documents:
- Empty state shows: "Aucun document pour ce véhicule"
- CTA button: "Ajouter un document"

**And** list is paginated (20 documents per page)

**And** I can filter documents by:
- Type (all types + "Tous")
- Status (Validé / En cours / Illisible)
- Expiration: "Expire dans 30 jours" toggle

---

### Story 5.3: Bulk ZIP Import with OCR Matching

As a gestionnaire,
I want to import multiple documents at once via ZIP file with automatic vehicle matching,
So that I can onboard my fleet's existing documents quickly (<2 hours for 50 vehicles).

**Acceptance Criteria:**

**Given** I am on `/documents/import` page
**When** I click "Importer ZIP"
**Then** I see a modal with:
- File upload dropzone (accepts .zip only, max 100MB)
- Instructions: "Le ZIP peut contenir jusqu'à 200 documents (JPG, PNG, PDF)"
- Expected naming convention: "[IMMATRICULATION]_[TYPE]_[DATE].jpg"
  - Example: "AB123CD_CT_2024-12-15.jpg" → matches vehicle AB-123-CD, type Contrôle Technique, expires 2024-12-15

**And** when I upload a ZIP file:
- ZIP is unzipped on server (temp directory)
- Each file is processed:
  1. Filename parsed to extract: immatriculation, type, date (optional)
  2. Vehicle lookup by registration (case-insensitive, ignores dashes/spaces)
  3. OCR processing triggered for each file
  4. File uploaded to OVH S3

**And** I see a preview table with parsed results:
- Columns: Filename, Vehicle matched, Type detected, Expiration date, Status
- Status indicators:
  - ✅ "Valide" (green): vehicle found, type valid
  - ⚠️ "Véhicule non trouvé" (orange): registration not in fleet
  - ⚠️ "Type inconnu" (orange): type not recognized
  - ❌ "Format invalide" (red): file not supported

**And** for files with warnings (⚠️), I can:
- Manually select vehicle from dropdown
- Manually select document type
- Skip file (will not be imported)

**And** summary shows:
- "X fichiers valides, Y nécessitent votre attention, Z seront ignorés"

**And** when I click "Confirmer l'import":
- Valid files are uploaded to S3
- Document records created
- OCR processing triggered in background
- Progress bar: "Import en cours: X/Y fichiers"

**And** after import completes:
- Success message: "Y documents importés avec succès"
- Email notification sent: "Import ZIP terminé: Y/X documents importés"
- I am redirected to `/documents` list

**And** if import fails mid-process:
- Partial uploads are retained (not rolled back)
- Error message shows: "Import interrompu: X/Y fichiers importés"
- I can retry remaining files

**And** processing time: 50 documents imported in <5 minutes

**And** naming convention variations are handled:
- "AB-123-CD_CT.jpg" → vehicle AB-123-CD, type CT, no date
- "AB123CD_Assurance_15-12-2024.pdf" → vehicle AB-123-CD, type Assurance, date parsed
- "CT_AB123CD.jpg" → vehicle AB-123-CD, type CT (flexible order)

---

### Story 5.4: Document Archive (Soft Delete) for Legal Retention

As a gestionnaire,
I want deleted documents to be archived instead of permanently deleted,
So that I comply with 10-year legal retention requirements.

**Acceptance Criteria:**

**Given** I am on a vehicle's document list (`/vehicles/[id]/documents`)
**When** I click "Supprimer" on a document
**Then** a confirmation modal appears:
- Title: "Archiver ce document ?"
- Message: "Le document sera archivé (non supprimé) pour conservation légale de 10 ans"
- Buttons: "Annuler" (gray), "Archiver" (red, 48x48px)

**And** when I confirm:
- Document status changes to ARCHIVED
- `archivedAt` timestamp is set
- `archivedByUserId` is recorded
- Document no longer appears in default list

**And** archived documents are excluded from:
- Default `/documents` list
- Compliance calculations
- Dashboard statistics

**And** I can view archived documents by:
- Toggling "Afficher documents archivés" filter
- Archived documents show badge: "Archivé" (gray)
- I can see who archived it and when

**And** if I need to restore an archived document:
- Click "Restaurer" button on archived document
- Document status returns to VALIDATED or previous status
- Document reappears in active list

**And** archived documents are retained for 10 years:
- After 10 years, a cron job permanently deletes from S3
- Database record remains with `permanentlyDeletedAt` timestamp

**And** Super Admin can view retention status:
- Navigate to `/admin/retention-status`
- See documents approaching 10-year deletion
- Can manually trigger permanent deletion if needed

---

## Epic 6: Compliance Dashboard & Real-Time Monitoring

Les gestionnaires visualisent l'état de conformité temps réel de leur flotte avec statuts visuels (vert/orange/rouge), calendrier des échéances 30 jours, et métriques d'adoption des chauffeurs. Performance cible: chargement <2 secondes.

### Story 6.1: Real-Time Compliance Dashboard with Color-Coded Status

As a gestionnaire,
I want to see a real-time compliance dashboard with color-coded vehicle statuses,
So that I can quickly identify which vehicles need attention.

**Acceptance Criteria:**

**Given** I am logged in as ADMIN or MANAGER
**When** I navigate to `/dashboard`
**Then** I see a compliance overview with:
- Summary cards at top:
  - "Conformes" (green): X véhicules
  - "Attention" (orange): Y véhicules (expiration < 30 jours)
  - "Critique" (red): Z véhicules (expiration < 15 jours OR expired)
  - Total vehicles count
- Distinction visuelle moteurs vs remorques:
  - 🚛 icon for motors
  - 🚚 icon for trailers
  - Separate counts: "X moteurs, Y remorques"

**And** below summary cards, I see a grid/list of all vehicles with:
- Vehicle registration (large, bold)
- Vehicle type icon (Camion, VUL, Voiture, Engin, Remorque)
- Overall status badge (Conforme / Attention / Critique)
- Document status breakdown:
  - Carte Grise: ✓ (green) / ⚠️ (orange) / ❌ (red) / - (gray if not uploaded)
  - Assurance: status
  - Contrôle Technique: status
  - Other relevant docs
- "Voir détails" button

**And** status color logic:
- **Green (Conforme)**: All required docs present AND expiration > 30 days
- **Orange (Attention)**: Any doc expires in 15-30 days
- **Red (Critique)**: Any doc expires in < 15 days OR already expired OR doc missing

**And** when I click on a vehicle card:
- I am navigated to `/vehicles/[vehicleId]/documents`
- I see full document list for that vehicle (Story 5.2)

**And** I can filter vehicles by:
- Status: All / Conformes / Attention / Critique
- Type: All / Motors / Trailers
- Search by registration

**And** I can sort by:
- Status (Critical first)
- Registration (alphabetical)
- Next expiration date (soonest first)

**And** dashboard data is cached for 60 seconds using Next.js `unstable_cache`:
- Cache key: `dashboard-${organizationId}`
- Revalidation: 60 seconds
- Cache invalidated when:
  - New document uploaded
  - Document expiration date updated
  - Vehicle added/deleted

**And** dashboard loads in <2 seconds (NFR-P1):
- Uses optimized PostgreSQL query with joins
- Index on: `(organizationId, expirationDate, status)`
- Only loads visible vehicles (pagination: 20 per page)

**And** if fleet has no vehicles:
- Empty state shows: "Ajoutez vos premiers véhicules pour commencer"
- CTA button: "Ajouter véhicule"

---

### Story 6.2: Interactive Calendar of Upcoming Deadlines (30 days)

As a gestionnaire,
I want to see a calendar view of upcoming document expiration deadlines,
So that I can plan renewals and avoid last-minute urgency.

**Acceptance Criteria:**

**Given** I am on the dashboard (`/dashboard`)
**When** I scroll to the "Échéances à venir" section
**Then** I see a calendar widget displaying:
- Current month + next month (2-month view)
- Days with expiring documents are highlighted:
  - Green dot: 1-2 expirations
  - Orange dot: 3-5 expirations
  - Red dot: 6+ expirations
- Today's date is outlined

**And** when I click on a highlighted day:
- A popover opens showing:
  - List of documents expiring that day
  - Format: "[Vehicle Registration] - [Document Type] - Expire le [Date]"
  - Icon indicating document type
  - Link to vehicle detail page
- Popover auto-closes when clicking outside

**And** below the calendar, I see an "Échéances 30 prochains jours" list:
- Sorted by expiration date (soonest first)
- Columns: Days remaining, Vehicle, Document type, Expiration date, Action
- Days remaining color-coded:
  - < 7 days: Red
  - 8-15 days: Orange
  - 16-30 days: Yellow
- Action button: "Voir document" (navigates to document detail)

**And** I can filter the list by:
- Document type (all types + "Tous")
- Vehicle type (Motors / Trailers / All)

**And** if there are no upcoming expirations:
- Calendar shows no dots
- List shows: "Aucune échéance dans les 30 prochains jours ✓"

**And** calendar loads in <1.5 seconds (NFR-P2):
- Query optimized: `WHERE expirationDate BETWEEN today AND today+30 AND status != ARCHIVED`
- Uses cached dashboard data when possible

**And** I can export upcoming deadlines as CSV:
- Button: "Exporter les échéances"
- CSV columns: Registration, Document Type, Expiration Date, Days Remaining, Status
- Filename: "echeances_[organization]_[date].csv"

---

### Story 6.3: Driver Adoption Metrics Dashboard

As a gestionnaire,
I want to see metrics on driver adoption and scanning activity,
So that I can identify drivers who need training or encouragement.

**Acceptance Criteria:**

**Given** I am on `/dashboard/adoption` page
**When** the page loads
**Then** I see adoption overview cards:
- "Taux d'adoption global": X% (drivers who scanned ≥1 doc in last 30 days / total active drivers)
- "Documents scannés ce mois": Y documents
- "Chauffeurs actifs": Z / Total
- "Moyenne scans par chauffeur": W scans/month

**And** I see a table of all drivers with columns:
- Name
- Username
- Assigned vehicles count
- Last scan date
- Total scans (last 30 days)
- Status badge:
  - ✓ "Actif" (green): ≥1 scan in last 30 days
  - ⚠️ "Inactif" (orange): No scan in last 30 days
  - ❌ "Jamais scanné" (red): Never scanned any document
- Action: "Envoyer rappel SMS" button

**And** drivers are sorted by:
- Default: Status (Jamais scanné → Inactif → Actif)
- Can toggle: Last scan date, Total scans

**And** I can filter drivers by:
- Status: All / Actifs / Inactifs / Jamais scanné
- Has vehicles: Yes / No / Any

**And** when I click "Envoyer rappel SMS":
- Confirmation modal: "Envoyer un SMS de rappel à [Driver Name] ?"
- SMS content preview:
```
Bonjour [Name],
N'oubliez pas de scanner les documents de vos véhicules via l'app FlotteBox.
Besoin d'aide ? Regardez le tutoriel : https://flottebox.app/tutorial
```
- If confirmed: SMS sent via OVH SMS API (Story 3.7)
- Rate limiting: Max 1 reminder SMS per driver per 7 days

**And** I see an adoption trend chart:
- Line chart: X-axis = last 6 months, Y-axis = % adoption
- Shows adoption evolution over time
- Hover tooltip shows exact % for each month

**And** I see a "Top Scanners" leaderboard (if gamification is active, Epic 10):
- Top 10 drivers by scan count (last 30 days)
- Badges displayed (if earned)
- Click on driver → navigates to driver detail page

**And** if fleet has no drivers:
- Empty state: "Ajoutez des chauffeurs pour suivre leur adoption"
- CTA: "Ajouter chauffeur"

**And** adoption metrics are calculated daily via Vercel Cron Job:
- Runs at 3:00 AM UTC+1
- Updates cached metrics in database (`OrganizationMetrics` table)
- No real-time calculation (performance optimization)

---

## Epic 7: Alerts & Notifications System

Le système calcule automatiquement les échéances (J-60, J-30, J-15, expiration) et envoie des alertes email proactives. Les gestionnaires personnalisent la fréquence et types d'alertes par véhicule. Push notifications PWA pour alertes critiques.

### Story 7.1: Auto-Calculate Expiration Deadlines & Alert Triggers

As a developer,
I want the system to automatically calculate expiration deadlines and trigger alerts,
So that gestionnaires are notified proactively before documents expire.

**Acceptance Criteria:**

**Given** a document has an expiration date set
**When** the system runs daily deadline calculation (via Vercel Cron Job)
**Then** for each document, the system calculates:
- Days until expiration: `expirationDate - today`
- Alert trigger points:
  - J-60: 60 days before expiration
  - J-30: 30 days before expiration
  - J-15: 15 days before expiration
  - J-0: Expiration day
  - J+1: 1 day after expiration (overdue)

**And** a new database table `Alert` is created with fields:
- id (UUID), organizationId (FK), documentId (FK), vehicleId (FK)
- alertType (enum: J60, J30, J15, EXPIRATION, OVERDUE)
- triggeredAt (DateTime)
- sentAt (DateTime, nullable)
- emailSent (Boolean, default false)
- smsSent (Boolean, default false) - for Epic 12
- pushSent (Boolean, default false)
- status (enum: PENDING, SENT, FAILED)
- createdAt, updatedAt

**And** the cron job logic:
1. Query all documents with `expirationDate <= today + 60 days AND status = VALIDATED`
2. For each document, check if alert should be triggered:
   - If `daysUntilExpiration == 60` AND no J60 alert exists: Create J60 alert
   - If `daysUntilExpiration == 30` AND no J30 alert exists: Create J30 alert
   - If `daysUntilExpiration == 15` AND no J15 alert exists: Create J15 alert
   - If `daysUntilExpiration == 0` AND no EXPIRATION alert exists: Create EXPIRATION alert
   - If `daysUntilExpiration < 0` AND no OVERDUE alert for today: Create OVERDUE alert
3. Update alert status to PENDING

**And** cron job runs daily at 6:00 AM UTC+1 (before business hours)

**And** cron job is secured with `CRON_SECRET` environment variable:
- Request header: `Authorization: Bearer ${CRON_SECRET}`
- If header missing or invalid: Return 401 Unauthorized
- Only Vercel cron can trigger this endpoint

**And** cron job logs execution:
- Table: `CronJobLog` with fields: jobName, startedAt, completedAt, alertsCreated, errors
- Errors logged to Sentry for monitoring

**And** if cron job fails:
- Admin receives email: "Alerte système: Calcul échéances échoué"
- Retry automatically after 1 hour (max 3 retries)

---

### Story 7.2: Email Notifications with Resend API

As a gestionnaire,
I want to receive email notifications for upcoming document expirations,
So that I can renew documents before they expire.

**Acceptance Criteria:**

**Given** alerts have been created by the cron job (Story 7.1)
**When** email sending process runs (triggered after cron job)
**Then** for each PENDING alert, the system:
1. Loads organization's email notification settings (Story 7.4)
2. Checks if email notifications are enabled for this alert type
3. If enabled: Sends email via Resend API
4. Updates alert: `emailSent = true`, `sentAt = now`, `status = SENT`

**And** Resend API integration (`lib/email.ts`):
- Endpoint: `https://api.resend.com/emails`
- Authentication: `Authorization: Bearer ${RESEND_API_KEY}`
- Rate limiting: Max 100 emails/hour per organization (Resend free tier)

**And** email templates are created for each alert type:

**J-60 Email:**
- Subject: "FlotteBox - Échéances dans 60 jours"
- Body:
```
Bonjour,

Les documents suivants expirent dans 60 jours :

- [Vehicle Registration] - [Document Type] - Expire le [Date]
- [Vehicle Registration] - [Document Type] - Expire le [Date]

Connectez-vous à FlotteBox pour renouveler ces documents : https://flottebox.app/dashboard

Cordialement,
L'équipe FlotteBox
```

**J-30 Email:**
- Subject: "⚠️ FlotteBox - Échéances dans 30 jours"
- Body: Similar format, orange icon

**J-15 Email:**
- Subject: "🚨 URGENT - Échéances dans 15 jours"
- Body: Similar format, red icon, "URGENT" in subject

**EXPIRATION Email:**
- Subject: "❌ CRITIQUE - Documents expirés aujourd'hui"
- Body: Similar format, critical tone

**OVERDUE Email:**
- Subject: "❌ RETARD - Documents expirés depuis [X] jours"
- Body: Similar format, includes overdue days count

**And** emails are batched by organization:
- Single email per organization per alert type per day
- Groups all alerts of same type into one email
- Avoids spamming (max 5 emails per day per organization)

**And** email recipients are configured per organization (Story 7.4):
- Primary contact email (required)
- CC emails (optional, up to 3)
- BCC: Always include admin for monitoring

**And** if Resend API fails:
- Alert status set to FAILED
- Error logged to Sentry
- Retry automatically after 1 hour (max 3 retries)
- If all retries fail: Admin notified

**And** email sending respects user preferences:
- Users can unsubscribe via link in email footer
- Unsubscribe stores preference in `EmailPreferences` table
- Unsubscribed users no longer receive emails (except critical OVERDUE alerts)

---

### Story 7.3: PWA Push Notifications for Critical Alerts

As a gestionnaire,
I want to receive push notifications on my device for critical expiration alerts,
So that I'm immediately aware even when not checking email.

**Acceptance Criteria:**

**Given** I have installed the PWA and granted push notification permissions
**When** a critical alert (J-15, EXPIRATION, or OVERDUE) is triggered
**Then** the system sends a push notification to my device via Web Push API

**And** push notification setup:
- User grants permission on first login via browser prompt
- Permission stored in `PushSubscription` table: userId, endpoint, keys (p256dh, auth), createdAt
- Uses standard Web Push Protocol (RFC 8030)

**And** push notification content:
- Title: "🚨 FlotteBox - Document critique"
- Body: "[Vehicle Registration] - [Document Type] expire dans [X] jours"
- Icon: FlotteBox logo (192x192px)
- Badge: FlotteBox badge icon
- Click action: Opens `/vehicles/[vehicleId]/documents` in PWA

**And** push notifications are sent for:
- J-15 alerts (if user opted in)
- EXPIRATION alerts (always sent, cannot be disabled)
- OVERDUE alerts (always sent, cannot be disabled)

**And** push notification library:
- Use `web-push` npm package
- VAPID keys stored in environment variables:
  - `VAPID_PUBLIC_KEY`
  - `VAPID_PRIVATE_KEY`
  - `VAPID_SUBJECT=mailto:contact@flottebox.app`

**And** when sending push notification:
```typescript
await webpush.sendNotification(
  subscription,
  JSON.stringify({
    title: "🚨 FlotteBox - Document critique",
    body: `${vehicle.registration} - ${doc.type} expire dans ${daysLeft} jours`,
    icon: "/icon-192.png",
    data: { vehicleId, documentId, url: `/vehicles/${vehicleId}/documents` }
  })
)
```

**And** if push fails (subscription expired or invalid):
- Remove subscription from database
- Log error (non-blocking, email still sent)

**And** users can manage push preferences:
- Navigate to `/settings/notifications`
- Toggle: "Recevoir notifications push pour alertes critiques"
- Test push notification button: "Envoyer notification test"

**And** push notifications respect device settings:
- If device has Do Not Disturb: notification queued until available
- If user revokes permissions: subscription removed from database

---

### Story 7.4: Customize Alert Frequency & Types per Vehicle

As a gestionnaire,
I want to customize alert frequency and types for each vehicle,
So that I receive only relevant notifications.

**Acceptance Criteria:**

**Given** I am on `/settings/notifications`
**When** I access alert configuration
**Then** I see global notification settings:
- "Activer alertes email" (toggle, default: ON)
- "Activer notifications push" (toggle, default: OFF until permission granted)
- Email recipients:
  - Primary email (required, pre-filled with account email)
  - CC emails (optional, up to 3, comma-separated input)
- Alert frequency:
  - Daily digest (one email per day at 8 AM)
  - Immediate (email sent as soon as alert triggers)
  - Weekly summary (every Monday at 8 AM)

**And** I see alert type preferences (which alerts to receive):
- J-60 alerts: ☐ Email ☐ Push (checkboxes)
- J-30 alerts: ☑ Email ☐ Push
- J-15 alerts: ☑ Email ☑ Push
- EXPIRATION alerts: ☑ Email ☑ Push (always enabled, cannot uncheck)
- OVERDUE alerts: ☑ Email ☑ Push (always enabled, cannot uncheck)

**And** I can configure vehicle-specific settings:
- Button: "Configurer par véhicule"
- Modal opens with list of all vehicles
- For each vehicle, I can:
  - Disable alerts completely (toggle: "Alertes désactivées pour ce véhicule")
  - Override global settings for this vehicle
  - Set custom reminder interval (e.g., J-45 instead of J-60)

**And** when I save settings:
- Settings stored in `NotificationSettings` table:
  - organizationId, userId, alertType, emailEnabled, pushEnabled, frequency, customInterval
- Settings apply immediately to next cron job run
- Success message: "Paramètres de notifications mis à jour"

**And** I can test notification settings:
- Button: "Envoyer notification test"
- Sends test email to configured recipients
- Test email subject: "FlotteBox - Test de notification"
- Test email body: "Ceci est un test. Vos notifications sont correctement configurées."
- If push enabled: Also sends test push notification

**And** if I disable all notifications:
- Warning modal: "Vous ne recevrez aucune alerte. Êtes-vous sûr ?"
- Confirmation required: Type "DESACTIVER" to confirm
- Critical alerts (OVERDUE) still sent regardless (cannot be fully disabled)

**And** vehicle-specific overrides are visually indicated:
- On dashboard, vehicles with custom settings show icon: ⚙️
- Tooltip on hover: "Alertes personnalisées configurées"

---

### Story 7.5: Vercel Cron Job Configuration & Monitoring

As a developer,
I want to configure Vercel Cron Jobs for automated alert calculation,
So that the system runs daily alert processing without manual intervention.

**Acceptance Criteria:**

**Given** the application is deployed on Vercel
**When** I configure the cron job in `vercel.json`
**Then** the configuration includes:
```json
{
  "crons": [
    {
      "path": "/api/cron/calculate-alerts",
      "schedule": "0 5 * * *"
    },
    {
      "path": "/api/cron/send-notifications",
      "schedule": "0 6 * * *"
    }
  ]
}
```

**And** cron job endpoints are created:
- `/api/cron/calculate-alerts`: Runs at 5:00 AM UTC (Story 7.1)
- `/api/cron/send-notifications`: Runs at 6:00 AM UTC (Story 7.2)

**And** both endpoints check `CRON_SECRET`:
```typescript
if (request.headers.get('Authorization') !== `Bearer ${process.env.CRON_SECRET}`) {
  return new Response('Unauthorized', { status: 401 })
}
```

**And** cron job execution is logged:
- Table: `CronJobLog` with fields:
  - id, jobName, startedAt, completedAt, status (SUCCESS/FAILED)
  - alertsCreated (for calculate-alerts job)
  - emailsSent, pushSent (for send-notifications job)
  - errors (JSON, nullable)
- Logs retained for 90 days

**And** monitoring dashboard for Super Admin:
- Navigate to `/admin/cron-status`
- See recent cron job executions (last 30 days)
- Columns: Job name, Run date, Duration, Status, Alerts created, Notifications sent, Errors
- Filter by status (Success / Failed)
- "Retry failed job" button for failed executions

**And** if cron job fails:
- Error logged to Sentry with context:
  - Job name
  - Timestamp
  - Error message and stack trace
  - Affected organizations
- Email sent to Super Admin: "Cron job échoué: [Job Name]"
- Automatic retry after 1 hour (max 3 retries)

**And** cron job timeout:
- Max execution time: 60 seconds (Vercel limit)
- If approaching timeout: Log partial results and exit gracefully
- Next run will process remaining items

**And** performance optimization:
- Process organizations in batches of 50
- Use database connection pooling
- Limit concurrent email sends (max 10/second)

---

## Epic 8: Compliance Exports & Reporting

Les gestionnaires exportent des registres de conformité conducteurs (permis, visites médicales, FIMO) et véhicules (cartes grises, assurances, CT) en PDF/Excel avec sélection de périodes custom pour audits URSSAF et comptabilité.

### Story 8.1: Export Registre Conducteurs (PDF)

As a gestionnaire,
I want to export a compliance register for all drivers as PDF,
So that I can provide it during URSSAF audits or inspections.

**Acceptance Criteria:**

**Given** I am logged in as ADMIN or MANAGER
**When** I navigate to `/exports` and click "Exporter Registre Conducteurs"
**Then** a modal opens with export options:
- Date period selector:
  - "Tous les documents" (default)
  - "Période personnalisée" (date range picker: from / to)
- Document types to include (checkboxes, all selected by default):
  - ☑ Permis de conduire
  - ☑ Visites médicales
  - ☑ FIMO/FCO
  - ☑ Autres documents conducteurs
- Format: PDF (only option for now)
- Button: "Générer le PDF" (48x48px, blue)

**And** when I click "Générer le PDF":
- Backend generates PDF using a library (e.g., `puppeteer` or `pdfkit`)
- PDF contains:
  - Header: Organization name, "Registre de Conformité Conducteurs", export date
  - Table with columns:
    - Nom/Prénom conducteur
    - Identifiant
    - Type de document
    - Date d'émission
    - Date d'expiration
    - Statut (Valide / Expiré)
  - Footer: Page numbers, "Généré par FlotteBox le [Date]"
- Rows sorted by: Driver name (alphabetical), then document type
- Expired documents highlighted in red
- Documents expiring in <30 days highlighted in orange

**And** PDF generation completes in <10 seconds for 100 drivers

**And** after generation:
- PDF downloads automatically: `registre_conducteurs_[organization]_[date].pdf`
- Success message: "Registre conducteurs exporté avec succès"

**And** export is logged in `ExportLog` table:
- organizationId, userId, exportType (REGISTRE_CONDUCTEURS), format (PDF), generatedAt, fileSizeKB

**And** if no driver documents exist:
- Modal shows warning: "Aucun document conducteur trouvé"
- Export button is disabled

**And** I can access export history:
- Navigate to `/exports/history`
- See list of recent exports (last 90 days)
- Can re-download previous exports if still available (cached for 7 days)

---

### Story 8.2: Export Registre Véhicules (PDF/Excel)

As a gestionnaire,
I want to export a compliance register for all vehicles in PDF or Excel format,
So that I can share it with accountants or auditors.

**Acceptance Criteria:**

**Given** I am on `/exports` page
**When** I click "Exporter Registre Véhicules"
**Then** a modal opens with export options:
- Date period selector:
  - "Tous les documents actifs" (default)
  - "Période personnalisée" (date range: from / to)
- Vehicle types to include:
  - ☑ Moteurs (Camions, VUL, Voitures, Engins)
  - ☑ Remorques
- Document types to include (checkboxes):
  - ☑ Cartes grises
  - ☑ Assurances
  - ☑ Contrôles techniques
  - ☑ Visites techniques
  - ☑ VGP
  - ☑ Attestations de conformité
  - ☑ Autres
- Format selector:
  - ◉ PDF (radio button, default)
  - ○ Excel (.xlsx)
- Button: "Générer l'export" (48x48px, blue)

**And** when I select PDF format and click "Générer l'export":
- Backend generates PDF with:
  - Header: Organization name, "Registre de Conformité Véhicules", export date
  - Table columns:
    - Immatriculation
    - Type véhicule
    - Marque/Modèle
    - Type de document
    - Date d'expiration
    - Statut (Conforme / Attention / Critique)
    - Chauffeur assigné
  - Rows sorted by: Vehicle registration (alphabetical), then document type
  - Color-coded status:
    - Green row: All docs valid
    - Orange row: Expiring in <30 days
    - Red row: Expired or missing
- PDF downloads: `registre_vehicules_[organization]_[date].pdf`

**And** when I select Excel format and click "Générer l'export":
- Backend generates Excel file using library (e.g., `exceljs`)
- Excel contains:
  - Sheet 1: "Registre Véhicules" with same columns as PDF
  - Sheet 2: "Résumé" with summary statistics:
    - Total véhicules: X
    - Conformes: Y (green)
    - En attention: Z (orange)
    - Critiques: W (red)
    - Documents expirés: V
  - Conditional formatting:
    - Green cells: Expiration > 30 days
    - Orange cells: Expiration 15-30 days
    - Red cells: Expiration < 15 days or expired
- Excel downloads: `registre_vehicules_[organization]_[date].xlsx`

**And** Excel file is compatible with:
- Microsoft Excel 2016+
- Google Sheets
- LibreOffice Calc

**And** generation completes in <15 seconds for 200 vehicles

**And** if no vehicle documents exist:
- Modal shows warning: "Aucun document véhicule trouvé"
- Export button is disabled

---

### Story 8.3: Custom Date Period Selection & Compliance Report Preview

As a gestionnaire,
I want to preview compliance data before exporting and select custom date periods,
So that I can verify the export includes exactly what I need.

**Acceptance Criteria:**

**Given** I am on the export modal (Story 8.1 or 8.2)
**When** I select "Période personnalisée"
**Then** two date pickers appear:
- "Date de début" (from date, default: 1 year ago)
- "Date de fin" (to date, default: today)
- Date range validation: max 10 years span

**And** when I select a custom date range:
- System calculates how many documents match the criteria
- Preview shows: "X documents trouvés pour cette période"
- Breakdown by type:
  - "Permis: Y documents"
  - "Assurances: Z documents"
  - etc.

**And** I see a "Prévisualiser" button:
- When clicked: Opens preview table (HTML) with first 50 rows
- Columns: same as final export
- Pagination: "Affichage 1-50 sur X documents"
- No download yet, just preview

**And** preview loads in <3 seconds

**And** if date range includes expired documents:
- Warning message: "⚠️ Cette période inclut X documents expirés"
- Checkbox: "Inclure les documents expirés" (checked by default)
- If unchecked: Filter out expired documents from export

**And** if date range has no matching documents:
- Preview shows: "Aucun document trouvé pour cette période"
- Export button is disabled
- Suggestion: "Essayez d'élargir la période ou de modifier les filtres"

**And** when I'm satisfied with preview and click "Générer l'export":
- Export is generated based on preview filters
- Export includes exactly the documents shown in preview

**And** I can save export configurations as presets:
- Button: "Enregistrer comme modèle"
- Modal: "Nom du modèle" (e.g., "Audit URSSAF annuel")
- Saved presets appear in dropdown: "Modèles sauvegardés"
- Can load preset: Automatically fills all filters

**And** common presets are provided by default:
- "Audit URSSAF complet" (all driver docs, all time)
- "Conformité véhicules actifs" (all vehicle docs, last 3 years, active only)
- "Documents expirés" (all docs, expired only, last year)

---

## Epic 9: Billing & Subscription Management

Le système gère la facturation automatique avec pricing différencié évolutif (moteurs: 4€→3€→2,50€ selon paliers, remorques: 1,50€ fixe), add-ons (OCR 0,10€/doc), TVA automatique, et auto-renouvellement avec reminder J-7.

### Story 9.1: LemonSqueezy Integration & Subscription Creation

As a developer,
I want to integrate LemonSqueezy for subscription management,
So that organizations can subscribe and be billed automatically.

**Acceptance Criteria:**

**Given** LemonSqueezy account is configured
**When** I set up the integration
**Then** environment variables are configured:
```
LEMONSQUEEZY_API_KEY=<from LS dashboard>
LEMONSQUEEZY_STORE_ID=<store ID>
LEMONSQUEEZY_WEBHOOK_SECRET=<webhook signing secret>
```

**And** LemonSqueezy products are created:
- Product: "FlotteBox Subscription"
- Variants:
  - "Motors Tier 1" (1-25 vehicles, 4€/motor/month)
  - "Motors Tier 2" (26-100 vehicles, 3€/motor/month)
  - "Motors Tier 3" (101+ vehicles, 2,50€/motor/month)
  - "Trailers" (1,50€/trailer/month, unlimited)
- Add-on products:
  - "OCR Processing" (0,10€/document, usage-based)
  - "SMS Alerts" (0,50€/vehicle/month) - Epic 12

**And** database table `Subscription` is created:
- id (UUID), organizationId (FK)
- lemonSqueezySubscriptionId (String, unique)
- lemonSqueezyCustomerId (String)
- status (enum: TRIAL, ACTIVE, PAUSED, CANCELLED, EXPIRED)
- trialEndsAt (DateTime, nullable)
- currentPeriodStart (DateTime)
- currentPeriodEnd (DateTime)
- cancelAtPeriodEnd (Boolean, default false)
- createdAt, updatedAt

**And** when a new organization registers (Epic 1, Story 1.3 or 1.4):
- A LemonSqueezy customer is created via API
- A 14-day trial subscription is activated
- Subscription status: TRIAL
- `trialEndsAt` set to today + 14 days
- No payment required during trial

**And** LemonSqueezy webhook endpoint is created at `/api/webhooks/lemonsqueezy`:
- Webhook events subscribed:
  - `subscription_created`
  - `subscription_updated`
  - `subscription_cancelled`
  - `subscription_payment_success`
  - `subscription_payment_failed`
- Webhook signature validation using `LEMONSQUEEZY_WEBHOOK_SECRET`
- Webhook updates `Subscription` table based on events

**And** webhook is secured:
- Signature verification: `x-signature` header validated
- If signature invalid: Return 401 Unauthorized
- All webhook events logged in `WebhookLog` table

---

### Story 9.2: Usage-Based Billing Calculation Engine

As a developer,
I want to calculate monthly billing based on active vehicle counts,
So that organizations are charged correctly for their usage.

**Acceptance Criteria:**

**Given** an organization has active vehicles (Epic 2, Story 2.4)
**When** the billing calculation runs (triggered by LemonSqueezy at end of billing cycle)
**Then** the system calculates:
1. Count active motors: `SELECT COUNT(*) FROM Vehicle WHERE organizationId = X AND type = MOTOR AND status = ACTIVE`
2. Count active trailers: `SELECT COUNT(*) FROM Vehicle WHERE organizationId = X AND type = TRAILER AND status = ACTIVE`
3. Determine motor pricing tier based on motor count:
   - If motors <= 25: Tier 1 (4€/motor)
   - If motors 26-100: Tier 2 (3€/motor)
   - If motors >= 101: Tier 3 (2,50€/motor)
4. Calculate base amount:
   - `baseMotorCost = motorCount × tierPrice`
   - `trailerCost = trailerCount × 1.50`
   - `subtotal = baseMotorCost + trailerCost`

**And** database table `BillingPeriod` is created:
- id (UUID), organizationId (FK), subscriptionId (FK)
- periodStart (DateTime), periodEnd (DateTime)
- motorCount (Int), trailerCount (Int)
- motorPriceTier (enum: TIER1, TIER2, TIER3)
- motorUnitPrice (Decimal, e.g., 4.00)
- trailerUnitPrice (Decimal, 1.50)
- subtotalMotors (Decimal)
- subtotalTrailers (Decimal)
- subtotalAddOns (Decimal) - for OCR, SMS
- subtotalBeforeTax (Decimal)
- taxRate (Decimal, e.g., 0.20 for 20% VAT)
- taxAmount (Decimal)
- totalAmount (Decimal)
- invoiceUrl (String, nullable) - from LemonSqueezy
- status (enum: PENDING, PAID, FAILED, REFUNDED)
- createdAt, paidAt

**And** billing calculation is triggered:
- Automatically via LemonSqueezy webhook on subscription renewal
- OR manually via admin dashboard: `/admin/billing/recalculate/[orgId]`

**And** if vehicle count changes mid-cycle:
- Billing uses vehicle count at END of billing period (not averaged)
- Organizations can see vehicle count in real-time on `/billing` page

**And** calculation includes tier transitions:
- Example: Organization has 24 motors (Tier 1: 4€) → Adds 2 motors → Now 26 (Tier 2: 3€)
- Next billing cycle: Charged 26 × 3€ = 78€ (not 24×4€ + 2×4€)
- Tier change applies to ALL motors, not just incremental

**And** billing preview is available:
- Navigate to `/billing/preview`
- See: "Prochain prélèvement estimé: X€ le [Date]"
- Breakdown:
  - "X moteurs × Y€ = Z€"
  - "W remorques × 1,50€ = V€"
  - "Add-ons: U€"
  - "Sous-total HT: S€"
  - "TVA (20%): T€"
  - "Total TTC: TOTAL€"

---

### Story 9.3: OCR Add-On Billing (Pay-As-You-Go)

As a gestionnaire,
I want to be billed for OCR document processing on a pay-as-you-go basis,
So that I only pay for what I use.

**Acceptance Criteria:**

**Given** OCR processing is triggered (Epic 4, Story 4.3)
**When** a document is successfully processed via Mistral OCR API
**Then** the system logs OCR usage:
- Table: `OcrUsageLog` (already exists from Story 4.3)
- Fields: organizationId, documentId, processedAt, cost (0.10€)

**And** at end of billing period:
- System counts: `SELECT COUNT(*) FROM OcrUsageLog WHERE organizationId = X AND processedAt BETWEEN periodStart AND periodEnd`
- OCR add-on cost: `ocrCount × 0.10€`
- Added to `BillingPeriod.subtotalAddOns`

**And** OCR usage is displayed on `/billing` page:
- Section: "Utilisation OCR ce mois"
- Metrics:
  - "Documents traités: X"
  - "Coût OCR: Y€ (X × 0,10€)"
  - Chart: Daily OCR usage (last 30 days)

**And** OCR add-on has no activation required:
- Automatically enabled for all organizations
- Usage-based: Only billed if used
- Line item on invoice: "OCR - X documents × 0,10€ = Y€"

**And** if OCR API call fails:
- No charge applied (Story 4.3 handles retries)
- Only successful OCR calls are billed

**And** organizations can export OCR usage history:
- Navigate to `/billing/ocr-usage`
- See table: Date, Document, Vehicle, Cost
- Export as CSV: "ocr_usage_[period].csv"

**And** OCR pricing is displayed during document upload:
- Tooltip on upload page: "ℹ️ OCR: 0,10€ par document scanné"
- Confirmation modal if >50 docs uploaded at once: "Cela coûtera environ X€ en frais OCR"

---

### Story 9.4: TVA Automatic Calculation & EU Compliance

As a developer,
I want to calculate TVA (VAT) automatically based on organization location,
So that invoices comply with French and EU tax regulations.

**Acceptance Criteria:**

**Given** an organization's billing address is configured
**When** calculating invoice totals
**Then** the system determines TVA rate:
- France (B2B): 20% TVA
- France (B2C): 20% TVA (not applicable for FlotteBox B2B)
- EU country with valid TVA number: 0% (reverse charge mechanism)
- EU country without TVA number: Domestic VAT rate of that country
- Outside EU: 0% (export)

**And** database table `Organization` is extended with fields:
- billingAddress (String)
- billingCity (String)
- billingPostalCode (String)
- billingCountry (String, ISO 3166-1 alpha-2, e.g., "FR")
- vatNumber (String, nullable, format: "FRXX999999999")
- vatValidated (Boolean, default false)
- vatValidatedAt (DateTime, nullable)

**And** when admin enters VAT number on `/settings/billing`:
- System validates via VIES API (EU VAT validation service):
  - Endpoint: `https://ec.europa.eu/taxation_customs/vies/rest-api/check-vat-number`
  - Request: `{countryCode: "FR", vatNumber: "XX999999999"}`
- If valid:
  - `vatValidated = true`
  - `vatValidatedAt = now`
  - Message: "✓ Numéro TVA valide"
- If invalid:
  - `vatValidated = false`
  - Error: "Numéro TVA invalide. Vérifiez votre saisie."

**And** TVA calculation logic:
```typescript
function calculateTVA(subtotal: number, country: string, vatNumber: string | null): {
  taxRate: number;
  taxAmount: number;
  totalAmount: number;
  taxNote: string;
} {
  if (country === 'FR') {
    // France: Always 20% TVA
    return {
      taxRate: 0.20,
      taxAmount: subtotal * 0.20,
      totalAmount: subtotal * 1.20,
      taxNote: 'TVA française 20%'
    }
  }

  if (EU_COUNTRIES.includes(country) && vatNumber && vatValidated) {
    // EU with valid VAT: Reverse charge (0%)
    return {
      taxRate: 0,
      taxAmount: 0,
      totalAmount: subtotal,
      taxNote: 'Autoliquidation (reverse charge)'
    }
  }

  // Outside EU or invalid VAT: 0%
  return {
    taxRate: 0,
    taxAmount: 0,
    totalAmount: subtotal,
    taxNote: 'Hors UE - Export'
  }
}
```

**And** invoice displays tax information:
- If TVA 20%: "TVA (20%): X€"
- If reverse charge: "TVA: 0€ (Autoliquidation - Art. 283-2 du CGI)"
- If export: "TVA: 0€ (Export hors UE)"

**And** LemonSqueezy is configured to handle VAT:
- LemonSqueezy's built-in VAT handling is used
- FlotteBox backend validates and provides tax info
- Invoice generated by LemonSqueezy includes correct VAT

---

### Story 9.5: Self-Service Add-Ons Activation & Billing Portal

As a gestionnaire,
I want to activate or deactivate add-ons myself,
So that I can control my monthly costs without contacting support.

**Acceptance Criteria:**

**Given** I am logged in as ADMIN
**When** I navigate to `/billing/add-ons`
**Then** I see available add-ons with toggle switches:

**OCR Processing:**
- Toggle: Always ON (cannot be disabled, usage-based)
- Current usage: "X documents ce mois (Y€)"
- Pricing: "0,10€ par document"
- Info: "Facturation à l'usage, pas d'abonnement"

**SMS Alerts (Epic 12):**
- Toggle: OFF (can be activated)
- Pricing: "0,50€ par véhicule par mois"
- Estimated cost: "Z véhicules × 0,50€ = W€/mois"
- Button: "Activer SMS Alerts"

**And** when I toggle SMS Alerts ON:
- Confirmation modal: "Activer SMS Alerts ?"
  - "Coût estimé: W€/mois (Z véhicules)"
  - "Facturation mensuelle, ajoutée à votre abonnement"
  - Buttons: "Annuler" / "Activer" (48x48px)
- When confirmed:
  - Database: `OrganizationAddOns` table updated: `smsAlertsEnabled = true`, `smsAlertsActivatedAt = now`
  - LemonSqueezy: Add-on subscription created via API
  - Success message: "SMS Alerts activé. Facturation effective dès le prochain cycle."

**And** when I toggle SMS Alerts OFF:
- Confirmation modal: "Désactiver SMS Alerts ?"
  - "Les chauffeurs ne recevront plus de SMS d'alerte"
  - "Facturation stoppée à la fin du cycle en cours"
  - Buttons: "Annuler" / "Désactiver" (48x48px, red)
- When confirmed:
  - Database: `smsAlertsEnabled = false`, `smsAlertsDeactivatedAt = now`
  - LemonSqueezy: Add-on subscription cancelled at period end
  - Success message: "SMS Alerts désactivé. Facturation stoppée fin de cycle."

**And** I can access LemonSqueezy billing portal:
- Button: "Gérer mon abonnement" (on `/billing` page)
- Redirects to LemonSqueezy customer portal URL
- In portal, I can:
  - Update payment method
  - View invoice history
  - Download invoices as PDF
  - Update billing address
  - Cancel subscription

**And** billing portal URL is generated via LemonSqueezy API:
```typescript
const portalUrl = await lemonSqueezy.customers.getBillingPortalUrl(customerId)
// Returns: https://flottebox.lemonsqueezy.com/billing
```

---

### Story 9.6: Invoice Generation, Auto-Renewal & Cancellation

As a gestionnaire,
I want invoices to be generated automatically and access them easily,
So that I can manage my accounting without manual intervention.

**Acceptance Criteria:**

**Given** a billing period ends
**When** LemonSqueezy processes payment
**Then** LemonSqueezy webhook fires: `subscription_payment_success`

**And** webhook handler:
1. Creates `BillingPeriod` record with status PAID
2. Updates `Subscription.currentPeriodStart` and `currentPeriodEnd`
3. Stores `invoiceUrl` from LemonSqueezy response
4. Sends email notification: "Votre facture FlotteBox est disponible"

**And** I can view invoice history:
- Navigate to `/billing/invoices`
- See table: Invoice date, Period, Amount HT, TVA, Amount TTC, Status, Actions
- Action button: "Télécharger PDF" (opens LemonSqueezy invoice URL)
- Invoices paginated (20 per page)
- Filter by: Year, Status (Paid/Failed)

**And** invoice email contains:
- Subject: "FlotteBox - Facture [Month] [Year]"
- Body:
```
Bonjour,

Votre facture FlotteBox pour la période du [Start] au [End] est disponible.

Montant: X€ TTC
Détails:
- X moteurs × Y€ = Z€
- W remorques × 1,50€ = V€
- Add-ons (OCR, SMS): U€

Téléchargez votre facture : [Download Link]
Gérez votre abonnement : [Billing Portal Link]

Cordialement,
L'équipe FlotteBox
```

**And** auto-renewal reminder (J-7):
- Email sent 7 days before renewal
- Subject: "FlotteBox - Renouvellement dans 7 jours"
- Body:
```
Bonjour,

Votre abonnement FlotteBox sera renouvelé automatiquement le [Date].

Montant estimé: X€ TTC
Moyen de paiement: •••• 1234 (Visa)

Pour modifier ou annuler : [Billing Portal Link]
```

**And** reminder is sent via Vercel Cron Job:
- Runs daily at 8:00 AM
- Query: `SELECT * FROM Subscription WHERE currentPeriodEnd = today + 7 days AND status = ACTIVE`
- Send email to organization admin

**And** I can cancel subscription:
- Navigate to LemonSqueezy billing portal via `/billing` page
- Click "Cancel subscription"
- Choose: "Cancel immediately" OR "Cancel at end of period"
- If "Cancel at end of period":
  - `Subscription.cancelAtPeriodEnd = true`
  - Access continues until `currentPeriodEnd`
  - Status changes to CANCELLED at period end
- If "Cancel immediately":
  - `Subscription.status = CANCELLED`
  - Access revoked immediately
  - Partial refund calculated pro-rata (LemonSqueezy handles)

**And** when subscription is cancelled:
- Email notification: "Votre abonnement FlotteBox a été annulé"
- User can still access dashboard in read-only mode for 30 days
- After 30 days: Data archived, login disabled (RGPD compliant)

**And** if payment fails:
- LemonSqueezy webhook: `subscription_payment_failed`
- `BillingPeriod.status = FAILED`
- Email notification: "⚠️ Paiement échoué - Action requise"
- Email includes: Reason, "Update payment method" link
- Retry automatically after 3 days (LemonSqueezy default)
- After 3 failed attempts: `Subscription.status = PAUSED`

---

## Epic 10: Driver Adoption & Gamification

Le système guide les chauffeurs lors du premier scan avec tooltips contextuels et active automatiquement la gamification (badges "Chauffeur exemplaire", classements) si entreprise >10 users OU >5 scans/mois.

### Story 10.1: Guided First Scan with Contextual Tooltips

As a chauffeur,
I want to be guided through my first document scan with tooltips,
So that I can successfully scan without needing training.

**Acceptance Criteria:**

**Given** I am a driver logging in for the first time
**When** I navigate to `/driver/scan` page
**Then** I see a welcome overlay:
- Title: "Bienvenue sur FlotteBox !"
- Message: "Nous allons vous guider pour scanner votre premier document en 3 étapes simples"
- Button: "Commencer" (48x48px, green)
- Checkbox: "Ne plus afficher ce message"

**And** when I click "Commencer":
- Overlay dismisses
- Tooltip appears on camera preview: "Étape 1/3: Positionnez le document dans le cadre"
- Green frame overlay helps align document
- Tooltip auto-advances when camera is ready

**And** when I tap "Capturer" button:
- Tooltip appears: "Étape 2/3: Vérifiez la photo. Si elle est nette, cliquez Valider."
- Highlights "Valider" and "Reprendre photo" buttons with pulsing animation

**And** when I tap "Valider":
- Tooltip appears on document type dropdown: "Étape 3/3: Sélectionnez le type de document"
- Dropdown opens automatically
- Tooltip disappears after selection

**And** when I complete validation and tap "Enregistrer":
- Success animation plays (confetti or checkmark)
- Modal appears:
  - Title: "🎉 Premier document scanné !"
  - Message: "Vous avez scanné votre premier document avec succès. Continuez comme ça !"
  - Button: "Scanner un autre document" / "Retour au tableau de bord"
- Database: `User.firstScanCompletedAt = now`

**And** on subsequent scans:
- No tooltips or guidance (user is now familiar)
- Can manually trigger tutorial via `/driver/help` page: "Revoir le tutoriel"

**And** tooltip styling:
- Dark background with white text (high contrast)
- Arrow pointing to relevant UI element
- "Suivant" button to manually advance (optional)
- "Passer le tutoriel" link in corner (dismisses all tooltips)

---

### Story 10.2: Auto-Activate Gamification Rules

As a developer,
I want gamification to activate automatically when adoption thresholds are met,
So that engaged teams benefit from motivational features.

**Acceptance Criteria:**

**Given** an organization is using FlotteBox
**When** the system checks gamification eligibility (daily cron job)
**Then** gamification is activated IF:
- Organization has > 10 active drivers OR
- Organization has > 5 scans/month per active driver (average)

**And** gamification status is stored in database:
- Table: `OrganizationSettings`
- Fields: gamificationEnabled (Boolean), gamificationActivatedAt (DateTime)

**And** when gamification is first activated:
- Email sent to admin: "🎮 Gamification activée pour votre équipe !"
- Email body explains:
```
Bonjour,

Félicitations ! Votre équipe a atteint le seuil d'adoption pour activer la gamification FlotteBox.

Vos chauffeurs peuvent maintenant:
- Gagner des badges pour leur activité
- Voir leur classement dans l'équipe
- Être récompensés pour leur régularité

Consultez le classement : [Dashboard Link]
```

**And** gamification features unlocked:
- Badge system (Story 10.3)
- Leaderboard on `/dashboard/adoption` (Story 6.3)
- Driver profile shows earned badges
- Monthly "Top Scanner" email to team

**And** if organization falls below threshold:
- Gamification remains enabled (not automatically disabled)
- Admin can manually disable via `/settings/gamification`

**And** admin can manually enable/disable gamification:
- Navigate to `/settings/gamification`
- Toggle: "Activer la gamification pour mes chauffeurs"
- Preview of badges and leaderboard

**And** gamification is visible only to drivers when enabled:
- Drivers see badges on their `/driver/profile` page
- Drivers see leaderboard on `/driver/team` page
- Gestionnaires see adoption metrics with gamification data

---

### Story 10.3: Badge System & "Chauffeur Exemplaire" Award

As a chauffeur,
I want to earn badges for my scanning activity,
So that I feel recognized and motivated to continue.

**Acceptance Criteria:**

**Given** gamification is enabled for my organization (Story 10.2)
**When** I meet badge criteria
**Then** I earn badges automatically

**And** badge types defined:

**1. Premier Scan (Bronze)**
- Criteria: Complete first document scan
- Auto-awarded after Story 10.1 first scan completion

**2. Chauffeur Actif (Silver)**
- Criteria: Scan ≥ 5 documents in a month
- Recalculated monthly

**3. Chauffeur Exemplaire (Gold)**
- Criteria: Scan ≥ 10 documents in a month AND maintain 100% compliance for assigned vehicles
- Highest recognition badge
- Appears on profile with star icon ⭐

**4. Régularité (Blue)**
- Criteria: Scan at least 1 document every week for 4 consecutive weeks
- Encourages consistency

**5. Top Scanner du Mois (Purple, rare)**
- Criteria: Highest scan count in organization for the month
- Only 1 driver per month can have this
- Previous month's winner loses it when new month starts

**And** database table `DriverBadge` is created:
- id (UUID), userId (FK), badgeType (enum), earnedAt (DateTime), expiresAt (DateTime nullable), isActive (Boolean)

**And** when badge is earned:
- Database: New `DriverBadge` record created
- In-app notification: "🏆 Vous avez gagné le badge [Badge Name] !"
- Push notification (if enabled): "[Badge Name] débloqué !"
- Email digest (weekly): "Cette semaine, vous avez gagné X badges"

**And** badges are displayed on:
- `/driver/profile`: All earned badges with icons
- `/driver/team` leaderboard: Active badges shown next to name
- `/dashboard/adoption` (gestionnaire view): Badge count per driver

**And** badge icons:
- Premier Scan: 📄 Bronze medal
- Chauffeur Actif: 💪 Silver medal
- Chauffeur Exemplaire: ⭐ Gold medal with star
- Régularité: 📅 Blue badge
- Top Scanner: 👑 Purple crown

**And** monthly badge recalculation:
- Runs via Vercel Cron Job on 1st of each month at 2:00 AM
- Recalculates: Chauffeur Actif, Chauffeur Exemplaire, Top Scanner
- Sends monthly summary email: "Vos badges du mois dernier"

**And** leaderboard sorting (on `/driver/team`):
- Sorted by: Total scan count (last 30 days), then badge count
- Shows: Rank, Name, Scans this month, Badges earned, Current streak (weeks)

---

## Epic 11: Third-Party Access & Multi-Client Management

Les entreprises accordent des accès lecture seule à des tiers externes (comptables, auditeurs) avec permissions granulaires. Les comptables gèrent plusieurs clients avec un seul compte et exportent les registres de conformité.

### Story 11.1: Grant Read-Only Access to External Tiers

As a gestionnaire,
I want to grant read-only access to external third parties (accountants, auditors),
So that they can view my compliance documents without modifying anything.

**Acceptance Criteria:**

**Given** I am logged in as ADMIN
**When** I navigate to `/settings/external-access`
**Then** I see a section: "Accès tiers externes"

**And** I can click "Inviter un tiers":
- Modal opens with form fields:
  - Email (required)
  - Name (required)
  - Company/Role (optional, e.g., "Expert-comptable")
  - Access level dropdown:
    - "Lecture seule - Tous documents" (default)
    - "Lecture seule - Véhicules uniquement"
    - "Lecture seule - Conducteurs uniquement"
  - Expiration date (optional, default: no expiration)
  - Button: "Envoyer invitation" (48x48px)

**And** when I send invitation:
- Database: `ExternalAccess` table created with fields:
  - id (UUID), organizationId (FK), email, name, role, accessLevel (enum), expiresAt, invitedBy (FK to User), invitedAt, acceptedAt (nullable), status (enum: PENDING, ACTIVE, REVOKED, EXPIRED)
- Email sent to invited party:
  - Subject: "Invitation FlotteBox - Accès lecture seule"
  - Body:
```
Bonjour,

[Organization Name] vous invite à accéder à leurs documents de conformité FlotteBox en lecture seule.

Pour accepter cette invitation, créez votre compte : [Invitation Link]

Cette invitation expire le [Date] (si applicable).

Cordialement,
L'équipe FlotteBox
```
- Invitation link: `https://flottebox.app/invite/accept/[token]`
- Token valid for 7 days

**And** when invited party clicks invitation link:
- Redirected to registration page pre-filled with email
- Must create password
- Role automatically set to EXTERNAL_TIER
- After registration: Status changes to ACTIVE

**And** external tier user has read-only permissions:
- Can navigate to: `/organizations/[orgId]/dashboard` (read-only)
- Can view: Vehicle list, document list, compliance status
- Can download: Documents, compliance exports (Story 8.1, 8.2)
- Cannot: Create, edit, delete any data
- Cannot: Access billing, settings, or user management

**And** I can manage external access:
- View list of invited tiers on `/settings/external-access`
- Columns: Name, Email, Role, Access level, Status, Invited date, Actions
- Actions: "Révoquer accès" button

**And** when I revoke access:
- Confirmation modal: "Révoquer l'accès de [Name] ?"
- When confirmed:
  - `ExternalAccess.status = REVOKED`
  - User can no longer access organization data
  - Email notification: "Votre accès à [Organization] a été révoqué"

**And** external access auto-expires:
- Daily cron job checks: `WHERE expiresAt < today AND status = ACTIVE`
- Status changes to EXPIRED
- User can no longer access

---

### Story 11.2: Multi-Client Comptable Account Management

As a comptable (accountant),
I want to manage multiple client accounts with a single login,
So that I can easily switch between clients without logging out.

**Acceptance Criteria:**

**Given** I have been invited by multiple organizations (Story 11.1)
**When** I log in to FlotteBox
**Then** I land on `/comptable/clients` page (instead of single dashboard)

**And** I see a list of all my client organizations:
- Client cards with:
  - Organization name
  - Access level badge (e.g., "Lecture seule")
  - Last accessed date
  - Quick stats: "X véhicules, Y documents"
  - Button: "Accéder" (48x48px)

**And** when I click "Accéder" on a client:
- I am navigated to that organization's dashboard: `/organizations/[orgId]/dashboard`
- Top navigation bar shows:
  - Client name (bold)
  - Badge: "Mode lecture seule"
  - "Retour à mes clients" link

**And** while viewing a client organization:
- I have read-only access (Story 11.1 permissions)
- I can export compliance reports (Story 11.3)
- I can download documents
- Top bar reminds me: "👁️ Mode lecture seule"

**And** I can switch clients without logging out:
- Click "Retour à mes clients" in top bar
- Returns to `/comptable/clients` page
- Switch to different client seamlessly

**And** my account is of role EXTERNAL_TIER:
- Database: `User.role = EXTERNAL_TIER`
- `UserOrganizationAccess` junction table links me to multiple organizations:
  - userId (FK), organizationId (FK), accessLevel, addedAt

**And** if I have no active client access:
- Landing page shows: "Aucun client actif"
- CTA: "Demandez à vos clients de vous inviter via FlotteBox"

**And** client list can be filtered:
- Search by client name
- Filter by: Active / Expired / Revoked
- Sort by: Name (alphabetical), Last accessed

---

### Story 11.3: Tiers Export Capabilities

As a comptable (external tier user),
I want to export compliance registers for my client companies,
So that I can include them in financial audits or reporting.

**Acceptance Criteria:**

**Given** I have read-only access to a client organization
**When** I navigate to `/organizations/[orgId]/exports`
**Then** I see the same export page as gestionnaires (Epic 8)

**And** I can export:
- Registre Conducteurs (PDF) - Story 8.1
- Registre Véhicules (PDF/Excel) - Story 8.2
- Custom date period selection - Story 8.3

**And** export permissions are the same as read-only users:
- Can generate and download exports
- Export history shows my exports separately: "Exporté par [My Name] (Comptable)"
- Gestionnaire can see audit log: "[Comptable Name] exported [Export Type] on [Date]"

**And** export is logged for compliance:
- Table: `ExportLog` includes `exportedByUserId`
- If external tier: `isExternalUser = true`
- Audit trail for URSSAF/inspections

**And** I receive email confirmation after export:
- Subject: "Export FlotteBox - [Client Name]"
- Body:
```
Bonjour,

Votre export pour [Client Name] est prêt.

Type: [Export Type]
Période: [Date Range]
Télécharger: [Download Link]

L'export est disponible pendant 7 jours.

Cordialement,
L'équipe FlotteBox
```

**And** if my access is revoked mid-session:
- Export page shows: "Accès révoqué. Contactez [Organization] pour retrouver l'accès."
- Previously generated exports remain accessible for 7 days (grace period)

---

## Epic 12: SMS Alerts Add-On

Les entreprises activent l'add-on SMS alertes (0,50€/véhicule/mois) en self-service. Les chauffeurs reçoivent des alertes SMS sur leur numéro enregistré pour les échéances critiques.

### Story 12.1: Activate SMS Alerts Add-On (Self-Service)

As a gestionnaire,
I want to activate the SMS Alerts add-on for my fleet,
So that drivers receive SMS notifications for critical document expirations.

**Acceptance Criteria:**

**Given** I am on `/billing/add-ons` page (Story 9.5)
**When** I see the "SMS Alerts" add-on card
**Then** it displays:
- Title: "Alertes SMS pour chauffeurs"
- Description: "Vos chauffeurs reçoivent des SMS pour les documents critiques (J-15, expiration, retard)"
- Pricing: "0,50€ par véhicule par mois"
- Estimated cost: "[X] véhicules × 0,50€ = [Y]€/mois"
- Toggle switch: OFF (default)
- Button: "Activer" (48x48px, blue)

**And** when I click "Activer":
- Confirmation modal appears:
  - Title: "Activer les alertes SMS ?"
  - Cost breakdown:
    - "Coût mensuel: [Y]€ ([X] véhicules)"
    - "Facturation ajoutée à votre abonnement"
    - "SMS envoyés uniquement pour alertes critiques (J-15, expiration, retard)"
  - Checkbox: "J'accepte les conditions d'utilisation"
  - Buttons: "Annuler" / "Activer SMS" (48x48px)

**And** when I confirm activation:
- Database: `OrganizationAddOns.smsAlertsEnabled = true`, `smsAlertsActivatedAt = now`
- LemonSqueezy: Add-on subscription created via API (Story 9.5)
- Success message: "✓ Alertes SMS activées. Vos chauffeurs recevront des SMS pour les échéances critiques."
- Email confirmation sent to admin

**And** pricing calculation for billing:
- At end of billing period: `activeVehicleCount × 0.50€`
- Uses same vehicle count as base subscription (Story 9.2)
- Added to `BillingPeriod.subtotalAddOns`

**And** I can deactivate at any time:
- Toggle switch to OFF on `/billing/add-ons`
- Confirmation modal: "Désactiver alertes SMS ?"
  - Warning: "Vos chauffeurs ne recevront plus de SMS d'alerte"
  - "Facturation stoppée à la fin du cycle en cours"
- When confirmed:
  - `smsAlertsEnabled = false`, `smsAlertsDeactivatedAt = now`
  - LemonSqueezy: Add-on cancelled at period end

**And** add-on status is visible on dashboard:
- `/dashboard` shows badge: "SMS Alerts activé" (if enabled)
- `/billing` page shows current month usage: "SMS Alerts: [X] véhicules × 0,50€"

---

### Story 12.2: Send SMS Alerts to Drivers for Critical Deadlines

As a chauffeur,
I want to receive SMS alerts for critical document expirations,
So that I'm immediately aware even without opening the app.

**Acceptance Criteria:**

**Given** my organization has SMS Alerts add-on activated (Story 12.1)
**When** a critical alert is triggered (Epic 7, Story 7.1: J-15, EXPIRATION, or OVERDUE)
**Then** I receive an SMS on my registered phone number

**And** SMS content is concise (max 160 characters):

**J-15 Alert:**
```
FlotteBox: Le [Document Type] de [Vehicle Registration] expire dans 15 jours. Pensez à le renouveler. [Link]
```

**EXPIRATION Alert:**
```
FlotteBox URGENT: Le [Document Type] de [Vehicle Registration] expire AUJOURD'HUI. Renouvelez-le immédiatement. [Link]
```

**OVERDUE Alert:**
```
FlotteBox CRITIQUE: Le [Document Type] de [Vehicle Registration] est EXPIRÉ depuis [X] jours. Action requise! [Link]
```

**And** link shortener is used for SMS:
- Full URL: `https://flottebox.app/vehicles/[vehicleId]/documents`
- Shortened: `https://flbox.app/v/[shortId]` (saves characters)
- Short link redirects to vehicle documents page

**And** SMS sending logic:
- Integrated with OVH SMS API (already configured in Story 3.7)
- Rate limiting: Max 1 SMS per driver per alert type per day (avoid spam)
- If driver has multiple expiring docs: Group into single SMS (list max 3 vehicles)

**And** SMS is sent only if:
- `OrganizationAddOns.smsAlertsEnabled = true`
- Driver has valid phone number in `User.phone`
- Alert type is J-15, EXPIRATION, or OVERDUE (not J-60 or J-30)

**And** SMS sending is logged:
- Table: `SmsLog` (reuse from Story 3.7)
- Fields: organizationId, userId, alertId (FK), phoneNumber, content, sentAt, status (SENT/FAILED), cost (calculated based on OVH pricing)

**And** SMS cost tracking:
- Each SMS logged with cost (e.g., 0.035€ per SMS for OVH)
- Monthly report: "SMS envoyés ce mois: Y SMS (Z€)"
- Displayed on `/billing/add-ons` page

**And** if SMS send fails:
- Error logged to `SmsLog.status = FAILED`
- Fallback: Email alert sent instead
- Admin notified if >10% SMS fail rate

**And** drivers can opt-out of SMS:
- Navigate to `/driver/settings/notifications`
- Toggle: "Recevoir alertes SMS" (default: ON if add-on active)
- If opted out: SMS not sent, email sent instead
- Opt-out stored in `User.smsNotificationsEnabled`

**And** SMS is not sent for:
- J-60 alerts (too early, email sufficient)
- J-30 alerts (email sufficient)
- Non-critical alerts

**And** admin can view SMS history:
- Navigate to `/admin/sms-usage`
- Table: Date, Driver, Alert type, Vehicle, Status, Cost
- Filter by: Status (Sent/Failed), Date range
- Export as CSV: "sms_usage_[period].csv"

---

## Epic 13: Audit Logs & Compliance Tracking (P1)

Le système trace tous les accès et modifications de documents dans des audit logs immuables avec horodatage UTC. Les admins consultent les logs filtrés par utilisateur/date/action. Conservation 3 ans minimum pour conformité URSSAF.

### Story 13.1: Log All Access & Modifications with UTC Timestamps

As a developer,
I want to log all access and modifications to documents automatically,
So that we have a complete audit trail for URSSAF compliance and security.

**Acceptance Criteria:**

**Given** any user accesses or modifies a document
**When** the action occurs
**Then** an audit log entry is created automatically

**And** database table `AuditLog` is created:
- id (UUID)
- organizationId (FK) - for multi-tenant filtering
- userId (FK) - who performed the action
- action (enum: VIEW, DOWNLOAD, UPLOAD, UPDATE, DELETE, ARCHIVE, RESTORE, EXPORT)
- entityType (enum: DOCUMENT, VEHICLE, USER, ORGANIZATION, BILLING)
- entityId (UUID) - ID of affected entity
- timestamp (DateTime, UTC, indexed)
- ipAddress (String)
- userAgent (String, nullable)
- metadata (JSON) - additional context:
  - For DOCUMENT: documentType, vehicleId, fileName
  - For EXPORT: exportType, recordCount
  - For USER: role, changedFields
- createdAt (DateTime, UTC)

**And** middleware automatically logs on every relevant API call:
```typescript
// Example middleware
async function auditMiddleware(req, res, next) {
  const result = await next()

  if (shouldAudit(req.method, req.path)) {
    await prisma.auditLog.create({
      data: {
        organizationId: req.user.organizationId,
        userId: req.user.id,
        action: mapActionFromRequest(req),
        entityType: extractEntityType(req.path),
        entityId: req.params.id || null,
        timestamp: new Date(),
        ipAddress: req.ip,
        userAgent: req.headers['user-agent'],
        metadata: extractMetadata(req, result)
      }
    })
  }

  return result
}
```

**And** actions logged include:

**Document actions:**
- VIEW: User views document details or opens document viewer
- DOWNLOAD: User downloads document file
- UPLOAD: User uploads new document
- UPDATE: User updates document metadata (type, expiration date)
- DELETE: User deletes/archives document
- RESTORE: User restores archived document

**User actions:**
- User created, updated, deleted
- Role changes
- Password resets

**Export actions:**
- Compliance register exports (PDF/Excel)
- OCR usage exports
- Invoice downloads

**And** audit logs are immutable:
- No UPDATE or DELETE operations allowed on `AuditLog` table
- Database enforces: `REVOKE UPDATE, DELETE ON AuditLog FROM ALL USERS`
- Only INSERT and SELECT permitted

**And** all timestamps stored as UTC:
- `timestamp` field always UTC
- Display logic converts to user's timezone for UI
- Ensures consistent audit trail across timezones

**And** audit logging performance:
- Async logging (non-blocking, doesn't slow down API responses)
- Batch inserts for high-volume actions
- Indexed fields: organizationId, userId, timestamp, entityId

---

### Story 13.2: Audit Log Viewer with Filters (User, Date, Action)

As an admin,
I want to view and search audit logs with filters,
So that I can investigate specific events or provide audit trails for inspections.

**Acceptance Criteria:**

**Given** I am logged in as ADMIN
**When** I navigate to `/admin/audit-logs`
**Then** I see an audit log viewer interface

**And** viewer displays table with columns:
- Timestamp (date + time, formatted in local timezone)
- User (name + role badge)
- Action (color-coded badge:
VIEW = blue, DOWNLOAD = green, UPLOAD = purple, UPDATE = orange, DELETE = red, EXPORT = teal)
- Entity Type (icon + label)
- Details (brief description, e.g., "Document: CT_AB123CD.pdf")
- IP Address (tooltip shows full)
- Actions: "View details" (eye icon)

**And** logs are paginated:
- 50 logs per page (default)
- Pagination controls: Previous, Next, Jump to page
- Total count displayed: "Affichage 1-50 sur 15,234 logs"

**And** filters available:

**Date Range Filter:**
- Preset options: Today, Last 7 days, Last 30 days, Last 3 months, Custom range
- Custom range: Date picker (from / to)
- Default: Last 30 days

**User Filter:**
- Dropdown: All users (default)
- Searchable user list (name or username)
- Can select multiple users
- Includes: Gestionnaires, Chauffeurs, External Tiers

**Action Filter:**
- Checkboxes: VIEW, DOWNLOAD, UPLOAD, UPDATE, DELETE, EXPORT, ARCHIVE
- Default: All selected
- Can deselect to narrow results

**Entity Type Filter:**
- Checkboxes: DOCUMENT, VEHICLE, USER, ORGANIZATION, BILLING
- Default: DOCUMENT only (most common)

**And** when I click "View details" on a log:
- Modal opens with full log details:
  - Timestamp (UTC + local)
  - User: Full name, username, email, role
  - Action performed
  - Entity affected: Type, ID, name
  - IP address
  - User agent (browser/device info)
  - Metadata (JSON formatted, expandable)

**And** I can export audit logs:
- Button: "Exporter les logs" (CSV format)
- Exports current filtered results
- CSV columns: Timestamp, User, Action, Entity Type, Entity ID, Details, IP
- Filename: `audit_logs_[organization]_[date_range].csv`

**And** search functionality:
- Search bar: "Rechercher dans les logs..."
- Searches: User name, entity ID, document filename, IP address
- Real-time filtering (debounced 500ms)

**And** if no logs match filters:
- Empty state: "Aucun log trouvé pour ces critères"
- Suggestion: "Essayez d'élargir les filtres ou la période"

**And** external tier users (comptables) logs are marked:
- Badge: "Tiers externe" next to user name
- Allows admins to see external access history

---

### Story 13.3: 3-Year Retention Policy & Immutable Logs

As a developer,
I want audit logs to be retained for 3 years minimum and be immutable,
So that we comply with URSSAF legal requirements (NFR-S16, NFR-S17).

**Acceptance Criteria:**

**Given** audit logs are being generated (Story 13.1)
**When** logs reach 3 years old
**Then** they are archived but not deleted

**And** retention policy enforced:
- Minimum retention: 3 years from creation date
- Maximum retention: 10 years (same as document retention)
- After 10 years: Logs can be purged via admin action

**And** database-level immutability:
- PostgreSQL row-level security:
```sql
CREATE POLICY audit_immutable ON AuditLog
  FOR UPDATE, DELETE
  TO ALL
  USING (false);  -- No one can update or delete
```
- Only service account can INSERT
- Even Super Admin (Quentin) cannot modify audit logs

**And** archival cron job:
- Runs monthly on 1st at 3:00 AM
- Identifies logs older than 10 years:
  `SELECT * FROM AuditLog WHERE createdAt < NOW() - INTERVAL '10 years'`
- Exports to cold storage (S3 Glacier or equivalent)
- Marks as archived in database: `archivedAt = now`
- Does NOT delete from database (kept for reference)

**And** archived logs remain queryable:
- Admins can still search archived logs
- Archived logs show badge: "Archivé" (gray)
- Archived logs load slightly slower (cold storage retrieval)

**And** Super Admin dashboard shows retention stats:
- Navigate to `/admin/audit-retention`
- Metrics:
  - Total audit logs: X
  - Active logs (<3 years): Y
  - Archived logs (3-10 years): Z
  - Eligible for purge (>10 years): W
- Chart: Audit log growth over time (last 3 years)

**And** manual purge functionality (Super Admin only):
- Button: "Purger logs >10 ans" (red, requires confirmation)
- Confirmation modal:
  - Title: "ATTENTION: Suppression définitive"
  - Message: "Vous allez supprimer [W] logs de plus de 10 ans. Cette action est IRREVERSIBLE."
  - Checkbox: "Je comprends que cette action est irréversible"
  - Input: Type "PURGE" to confirm
  - Buttons: "Annuler" / "Supprimer définitivement" (red)
- When confirmed:
  - Logs >10 years permanently deleted from database
  - Action logged in separate admin action log
  - Email sent to Quentin: "Purge de [W] audit logs effectuée"

**And** compliance report generation:
- Navigate to `/admin/compliance-report`
- Button: "Générer rapport de conformité"
- Report includes:
  - Audit log retention status
  - Immutability verification (checks database policies)
  - Recent access by external tiers
  - Document modification history
  - Export as PDF for URSSAF audits

---

## Epic 14: Super Admin Analytics & Health Monitoring

Le Super Admin (Quentin) monitore la santé globale du SaaS avec analytics dashboard (MRR, churn, ARPU, NPS), identifie les clients à risque (health score <40/100), track l'adoption des features, et active le mode démo pour présentations commerciales.

### Story 14.1: Global Analytics Dashboard (MRR, Churn, ARPU, NPS)

As a Super Admin,
I want to view global SaaS metrics on a dashboard,
So that I can monitor business health and growth.

**Acceptance Criteria:**

**Given** I am logged in as Super Admin (Quentin)
**When** I navigate to `/admin/analytics`
**Then** I see a global analytics dashboard with KPI cards:

**MRR (Monthly Recurring Revenue):**
- Current MRR: X€
- MRR growth: +Y% vs last month (green if positive, red if negative)
- Chart: MRR trend (last 12 months)
- Breakdown: Base subscription (motors + trailers), Add-ons (OCR + SMS)

**Churn Rate:**
- Monthly churn: Z% (cancelled subscriptions / active subscriptions)
- Chart: Churn trend (last 12 months)
- Target: <5% monthly churn (color-coded: green if <5%, orange if 5-10%, red if >10%)

**ARPU (Average Revenue Per User):**
- Current ARPU: W€/org/month
- ARPU trend: +/-X% vs last month
- Breakdown by tier: Tier 1 (1-25 motors), Tier 2 (26-100), Tier 3 (101+)

**NPS (Net Promoter Score):**
- Current NPS: Score out of 100
- NPS distribution: Promoters (9-10), Passives (7-8), Detractors (0-6)
- Note: "NPS collected via quarterly surveys (Story 14.5)"

**And** additional metrics cards:

**Active Organizations:**
- Total: X organizations
- Trial: Y (with trial end dates)
- Active: Z (paying)
- Paused: W (payment failed)
- Cancelled: V (churned)

**Total Vehicles Managed:**
- Motors: X
- Trailers: Y
- Total: X + Y
- Average per org: Z vehicles/org

**Document Scans:**
- Total scans this month: X
- OCR usage: Y documents (Z€ revenue)
- Average scans per org: W scans/month

**And** time range selector:
- Dropdown: Last 7 days, Last 30 days, Last 3 months, Last 12 months, All time
- Default: Last 30 days

**And** data refresh:
- Metrics calculated daily via cron job at 3:00 AM
- "Last updated: [timestamp]" displayed
- Manual refresh button: "Actualiser les données"

**And** export functionality:
- Button: "Exporter rapport analytique" (PDF)
- PDF includes: All KPIs, charts, date range, timestamp
- Filename: `analytics_report_[date].pdf`

---

### Story 14.2: Client Health Score Monitoring (<40/100 = At-Risk)

As a Super Admin,
I want to monitor client health scores and identify at-risk organizations,
So that I can proactively intervene before churn.

**Acceptance Criteria:**

**Given** I am on `/admin/clients-health` page
**When** the page loads
**Then** I see a list of all organizations with health scores

**And** health score calculation (0-100):
- **Usage score (40 points):**
  - Active drivers: % of drivers who scanned ≥1 doc in last 30 days (max 15 pts)
  - Scan frequency: Avg scans per vehicle per month (max 15 pts)
  - Feature usage: OCR, alerts, exports usage (max 10 pts)
- **Engagement score (30 points):**
  - Login frequency: Admin logins per week (max 10 pts)
  - Support tickets: 0 recent tickets = +10 pts, >3 tickets = 0 pts
  - Dashboard visits: Daily active users (max 10 pts)
- **Payment health (30 points):**
  - Payment status: On-time = +30 pts, 1 failed = +15 pts, 2+ failed = 0 pts
  - Subscription tenure: >6 months = +10 pts (bonus)
  - Upgrade/downgrade activity: Upgrades = +5 pts, Downgrades = -5 pts

**And** health score color coding:
- **Green (70-100):** Healthy, low churn risk
- **Orange (40-69):** Warning, moderate risk
- **Red (<40):** At-risk, high churn probability

**And** client table columns:
- Organization name
- Health score (color-coded badge + number)
- Active vehicles count
- MRR contribution
- Subscription status (Trial, Active, Paused)
- Last activity date
- Actions: "View details"

**And** sorting options:
- Sort by: Health score (ascending: at-risk first), MRR (descending), Last activity

**And** filters:
- Health status: All / Healthy / Warning / At-Risk
- Subscription status: All / Trial / Active / Paused
- MRR range: <100€, 100-500€, >500€

**And** when I click "View details" on an at-risk client:
- Modal opens with:
  - Health score breakdown (40pt usage, 30pt engagement, 30pt payment)
  - Recent activity timeline (last 30 days)
  - Key indicators:
    - "⚠️ Low driver adoption: 30% active"
    - "⚠️ No logins in 14 days"
    - "✓ Payment on-time"
  - Suggested actions:
    - "Send adoption email"
    - "Schedule check-in call"
    - "Review support tickets"
  - Button: "Marquer comme contacté"

**And** automated alerts:
- Daily email to Super Admin: "X clients at-risk today"
- Email lists: Client name, health score, drop since yesterday
- CTA: "View full report" (links to `/admin/clients-health`)

**And** historical health tracking:
- Each org has `OrganizationHealthScore` table:
  - organizationId, date, score, usageScore, engagementScore, paymentScore, calculatedAt
- Chart on detail page: Health score trend (last 90 days)
- Identifies: When did health score drop? What triggered it?

---

### Story 14.3: Feature Adoption Metrics

As a Super Admin,
I want to track feature adoption across all organizations,
So that I can identify underused features and prioritize development.

**Acceptance Criteria:**

**Given** I am on `/admin/feature-adoption` page
**When** the page loads
**Then** I see feature adoption metrics for all organizations

**And** features tracked:

**Core Features:**
- Document scanning (mobile PWA): % orgs with ≥1 scan
- OCR usage: % orgs using OCR (vs manual entry)
- Alerts configured: % orgs with email alerts enabled
- Driver onboarding: % orgs with ≥1 driver account
- Vehicle import CSV: % orgs used bulk import

**Add-On Features:**
- SMS Alerts: % orgs with SMS add-on active
- Gamification: % orgs with gamification enabled (auto or manual)
- External access: % orgs with ≥1 external tier user
- Compliance exports: % orgs exported ≥1 register

**Advanced Features:**
- 2FA: % admins with 2FA enabled
- Custom alert settings: % orgs customized alert frequency
- Audit logs viewed: % orgs accessed audit logs

**And** feature adoption table:
- Columns: Feature name, Adoption rate (%), Orgs using, Trend (↑↓), Last 30 days change
- Sorted by: Adoption rate (ascending: underused features first)
- Color-coded: >70% green, 40-70% orange, <40% red

**And** feature detail modal (click on feature row):
- Adoption breakdown by org size:
  - Small (<10 vehicles): X% adoption
  - Medium (10-50 vehicles): Y% adoption
  - Large (>50 vehicles): Z% adoption
- Adoption timeline: Chart showing adoption growth (last 12 months)
- Top 5 orgs using this feature most

**And** recommendations:
- For underused features (<40% adoption):
  - Suggestion: "Consider in-app tutorial or email campaign"
  - Button: "Send feature announcement email to non-users"

**And** feature usage heatmap:
- Visual grid: Organizations (rows) × Features (columns)
- Cell color: Green = used, Gray = not used
- Identifies: Which orgs are power users (all green) vs low engagement (mostly gray)

---

### Story 14.4: Demo Mode with Realistic Fake Data

As a Super Admin,
I want to activate demo mode to generate fake but realistic company data,
So that I can use it for sales presentations and product demos.

**Acceptance Criteria:**

**Given** I am on `/admin/demo-mode` page
**When** I click "Créer entreprise démo"
**Then** a form appears with options:

**Demo Configuration:**
- Company name (default: "TransportCo Démo")
- Fleet size: Small (10 vehicles), Medium (50 vehicles), Large (150 vehicles)
- Driver count: Match fleet size (1 driver per 2 vehicles)
- Document completeness: Full (100%), Partial (60%), Low (30%)
- Include alerts: Yes / No (if Yes, generates expiring documents)
- Subscription status: Trial / Active / At-Risk

**And** when I confirm:
- System generates:
  - 1 Organization with demo name
  - X Vehicles (motors + trailers, realistic registrations like "AB-123-CD")
  - Y Drivers (realistic French names, usernames, phone numbers)
  - Z Documents (PDFs with realistic content, varying expiration dates)
  - Alerts configured if requested
  - Billing data (fake subscription, MRR calculated)
  - Activity logs (simulated scans, logins over last 30 days)
- Database: `isDemoOrganization = true` flag set
- Success message: "Entreprise démo créée: [Name]"
- Button: "Se connecter en tant que Admin" (auto-login to demo org)

**And** demo organizations are clearly marked:
- Dashboard top banner: "🎭 MODE DÉMO" (yellow, persistent)
- All pages show: "Ceci est une entreprise de démonstration"
- Demo data cannot be used for real billing

**And** demo login credentials:
- Admin: `demo@flottebox.app` / Password: `Demo2024!`
- Driver: `chauffeur1` / Password: `chauffeur`
- Credentials displayed on `/admin/demo-mode` page

**And** I can reset demo data:
- Button: "Réinitialiser les données démo"
- Confirmation: "Supprimer et recréer les données démo ?"
- When confirmed: All demo org data deleted, fresh data regenerated

**And** I can delete demo organizations:
- Button: "Supprimer entreprise démo"
- Confirmation: "Cette action est irréversible"
- When confirmed: Demo org and all associated data hard-deleted

**And** demo mode restrictions:
- No real payment processing (LemonSqueezy bypassed)
- No real emails sent (logged only)
- No real SMS sent (logged only)
- Banner reminds: "Aucun vrai paiement ou notification ne sera envoyé"

---

### Story 14.5: Funnel & Engagement Event Tracking

As a Super Admin,
I want to track user funnel events and engagement metrics,
So that I can optimize onboarding and reduce drop-off rates.

**Acceptance Criteria:**

**Given** users are interacting with FlotteBox
**When** key events occur
**Then** events are tracked in analytics

**And** funnel events tracked:

**Registration Funnel:**
- Event: `registration_started` (user visits `/register`)
- Event: `registration_completed` (org created successfully)
- Event: `trial_started` (14-day trial begins)
- Event: `first_login` (admin first login)

**Onboarding Funnel:**
- Event: `first_vehicle_added` (org adds first vehicle)
- Event: `first_driver_created` (org creates first driver)
- Event: `first_document_scanned` (first scan via mobile PWA)
- Event: `first_alert_configured` (email alerts enabled)

**Activation Funnel:**
- Event: `trial_to_paid` (trial converts to paid subscription)
- Event: `add_on_activated` (OCR or SMS add-on enabled)
- Event: `external_access_granted` (first external tier invited)

**Engagement Events:**
- Event: `daily_active_user` (admin or driver logs in)
- Event: `weekly_active_user` (user logs in at least once per week)
- Event: `feature_used` (specific feature used, e.g., export, gamification)

**And** events stored in database:
- Table: `AnalyticsEvent`
- Fields: id, organizationId, userId (nullable), eventType, eventData (JSON), timestamp, createdAt

**And** funnel visualization on `/admin/funnels`:
- **Registration Funnel:**
  - Started: X users → Completed: Y users (Z% conversion)
  - Trial started: W users (W/Y% activation)
- **Onboarding Funnel:**
  - First vehicle: X% of orgs
  - First driver: Y% of orgs
  - First scan: Z% of orgs (activation milestone)
- **Activation Funnel:**
  - Trial to paid: W% conversion rate
  - Add-on adoption: V% of paid orgs

**And** drop-off analysis:
- Identifies: Where do users drop off in funnel?
- Example: "60% of orgs add vehicles, but only 30% create drivers"
- Recommendation: "Simplify driver creation or add tutorial"

**And** engagement metrics:
- DAU (Daily Active Users): X users/day
- WAU (Weekly Active Users): Y users/week
- MAU (Monthly Active Users): Z users/month
- Stickiness ratio: DAU/MAU (target: >30%)

**And** cohort analysis:
- Group orgs by signup month (e.g., "January 2024 cohort")
- Track: Retention rate over time (Month 1, 2, 3, 6, 12)
- Chart: Cohort retention heatmap
- Identifies: Which cohorts have best retention?

**And** NPS survey integration:
- Quarterly NPS survey sent to admins (Epic 7 email system)
- Question: "How likely are you to recommend FlotteBox? (0-10)"
- Results tracked in `NpsResponse` table: organizationId, score, comment, createdAt
- NPS calculated: (% Promoters - % Detractors)
- Displayed on Story 14.1 dashboard

**And** export funnel report:
- Button: "Exporter rapport funnel" (CSV)
- CSV includes: Funnel stage, Count, Conversion rate, Drop-off rate
- Filename: `funnel_report_[date].csv`

---

---

## Summary & Completion

**🎉 EPICS COMPLETS - PRÊTS POUR L'IMPLÉMENTATION**

✅ **Epic 1:** Project Foundation & Authentication (7 stories)
✅ **Epic 2:** Fleet Management Core (4 stories)
✅ **Epic 3:** Driver Management & Onboarding (10 stories)
✅ **Epic 4:** Document Scanning & OCR (Mobile PWA) (8 stories)
✅ **Epic 5:** Document Management (Desktop) (4 stories)
✅ **Epic 6:** Compliance Dashboard & Real-Time Monitoring (3 stories)
✅ **Epic 7:** Alerts & Notifications System (5 stories)
✅ **Epic 8:** Compliance Exports & Reporting (3 stories)
✅ **Epic 9:** Billing & Subscription Management (6 stories)
✅ **Epic 10:** Driver Adoption & Gamification (3 stories)
✅ **Epic 11:** Third-Party Access & Multi-Client Management (3 stories)
✅ **Epic 12:** SMS Alerts Add-On (2 stories)
✅ **Epic 13:** Audit Logs & Compliance Tracking (P1) (3 stories)
✅ **Epic 14:** Super Admin Analytics & Health Monitoring (5 stories)

**TOTAL: 14 Epics, 66 User Stories implementation-ready**

---

### Coverage Vérification

**Functional Requirements:** 66 FRs couverts sur 66 ✅
**Non-Functional Requirements:** 72 NFRs documentés et référencés ✅
**Architecture Requirements:** Starter template, infrastructure, intégrations ✅
**UX Requirements:** Mobile-first, offline, accessibility ✅

---

### Next Steps Recommandés

1. **Phase 1 (MVP Core):** Implémenter Epics 1-4 (Foundation, Fleet, Drivers, Scanning)
2. **Phase 2 (Compliance & Alerts):** Implémenter Epics 5-7 (Document Management, Dashboard, Alerts)
3. **Phase 3 (Business Features):** Implémenter Epics 8-10 (Exports, Billing, Gamification)
4. **Phase 4 (Advanced):** Implémenter Epics 11-14 (External Access, SMS, Audit Logs, Admin Analytics)

**Document prêt pour `/bmad:bmm:workflows:dev-story` ! 🚀**

## Summary & Next Steps

### Completed in this Session

**✅ Epics with Complete Stories (Ready for Development):**
- **Epic 1:** Project Foundation & Authentication (7 stories)
- **Epic 2:** Fleet Management Core (4 stories)
- **Epic 3:** Driver Management & Onboarding (10 stories)

**Total: 21 implementation-ready stories covering 21 FRs**

### Remaining Work

**Epics 4-14:** 11 epics with ~45-50 stories identified (summaries provided above)

These epics can be developed in detail by:
1. Re-running this workflow for Epics 4-6 (core MVP features)
2. Prioritizing based on implementation roadmap
3. Developing stories as needed during sprint planning

### Document Status

- ✅ All 66 FRs mapped to epics
- ✅ All 72 NFRs documented
- ✅ Architecture & UX requirements integrated
- ✅ FR Coverage Map complete
- ✅ Epic 1-3 stories ready for `/bmad:bmm:workflows:dev-story`
