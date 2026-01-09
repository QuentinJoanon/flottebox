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
