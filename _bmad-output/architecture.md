---
stepsCompleted: [1, 2]
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
