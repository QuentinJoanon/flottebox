---
stepsCompleted: [1, 2, 3, 4, 5, 6, 7]
inputDocuments: []
documentCounts:
  briefs: 0
  research: 0
  brainstorming: 0
  projectDocs: 0
workflowType: 'prd'
lastStep: 7
project_name: 'laboiteagants_cahier des charges'
user_name: 'Quentin'
date: '2026-01-06'
---

# Product Requirements Document - laboiteagants_cahier des charges

**Author:** Quentin
**Date:** 2026-01-06

## Executive Summary

### Vision du Produit

FlotteBox est une application SaaS B2B qui révolutionne la gestion documentaire pour les flottes de véhicules des PME et ETI. En transformant un processus manuel chronophage en une expérience automatisée et mobile-first, FlotteBox permet aux gestionnaires de flottes de 10 à 200 véhicules de gagner du temps, d'éviter des amendes coûteuses, et d'avoir une visibilité temps réel sur la conformité légale de leur flotte.

### Le Problème

Les PME/ETI gérant des flottes de véhicules (Transport, BTP, Artisans, Services, Collectivités) perdent actuellement **20-30 minutes par document traité** dans une gestion manuelle via classeurs physiques ou Excel. Cette approche entraîne :

- **Risques financiers** : Oublis d'échéances → amendes de 135€ à 3 750€ + immobilisations de véhicules
- **Temps administratif perdu** : Recherche de documents, classement, photocopies, envois par email
- **Absence de vision centralisée** : Aucune vue d'ensemble sur l'état de conformité de la flotte
- **Risques de sécurité** : Documents sensibles (cartes grises, permis) accessibles sans contrôle, risques de vol/perte
- **Non-conformité RGPD** : Données personnelles des conducteurs mal protégées

**Impact quantifié** : Pour une flotte de 50 véhicules, cela représente 100-150h/an de temps perdu (2 000-3 000€/an), sans compter les amendes évitables.

### La Solution

FlotteBox propose une **application web/mobile (PWA)** permettant de :

1. **Centraliser tous les documents** administratifs des véhicules dans un espace sécurisé et conforme RGPD
2. **Scanner et classifier intelligemment** via OCR assisté : un document scanné est pré-rempli automatiquement avec validation humaine obligatoire pour éviter les erreurs
3. **Recevoir des alertes proactives** avant les échéances (60j, 30j, 15j, expiration) via email, SMS (add-on), et notifications push
4. **Avoir une vision temps réel** de la conformité de la flotte via dashboard
5. **Accéder depuis mobile** pour les chauffeurs (scan terrain en < 30 secondes) et desktop pour les gestionnaires

### Ce qui rend FlotteBox différent

**Face à Excel/Classeurs manuels :**
- **OCR assisté intelligent** : Un scan photo → formulaire pré-rempli + validation obligatoire → document classé au bon véhicule + alertes configurées (vs 5-10 min de saisie manuelle à risque d'erreur)
- **Zéro oubli** : Alertes automatiques multi-canal (email, SMS optionnel, push) vs vérifications manuelles mensuelles
- **Mobile-first ultra-simple** : Chauffeurs scannent en < 30 secondes depuis le terrain, même sans formation technique, avec onboarding vidéo intégré

**Face aux TMS légers et logiciels de gestion de flotte :**
- **10× plus simple** : Interface pensée pour non-techniciens, prise en main < 30 min (vs plusieurs jours de formation)
- **5-10× moins cher** : 2,50-4€/véhicule/mois (100-300€/mois pour flottes moyennes) vs 500-2000€/mois pour TMS complets
- **Focus laser** : Uniquement la gestion documentaire (pas de suivi GPS, planification, etc.) = simplicité maximale
- **Pricing juste et transparent** : Tarif différencié véhicules moteurs vs remorques (les remorques ont moins de documents)

**ROI immédiat** : 1 amende CT évitée (135€) = 3 mois d'abonnement pour 10 véhicules. Le gain de temps administratif seul justifie l'investissement.

### Ce qui rend FlotteBox spécial

**OCR assisté, pas automatique**
FlotteBox utilise l'OCR pour **accélérer la saisie**, pas la remplacer. Workflow : scan → pré-remplissage automatique → **validation humaine obligatoire** → enregistrement. Cela évite les erreurs critiques (mauvaise lecture de date d'expiration) tout en divisant par 5 le temps de saisie.

**UX mobile pensée pour des non-techniciens (50-60 ans)**
L'interface chauffeur est conçue pour être utilisable par des profils peu tech-savvy :
- **Scan document : 3 clics maximum** (Ouvrir app → Photo → Envoyer)
- **Onboarding vidéo intégré** : Tutoriel 90 secondes envoyé par SMS à chaque nouveau chauffeur
- **Installation ultra-simple** : QR code sur dashboard gestionnaire → scan par chauffeur → installation directe PWA
- **Association automatique** au véhicule habituel du chauffeur
- **Mode offline** : scan sans connexion, sync automatique au retour de réseau
- **Notifications multi-canal** : SMS (add-on) + push + email pour s'adapter aux préférences
- **Support smartphones anciens** : Version optimisée pour Android 7-8 (encore courants chez les chauffeurs)
- **Gamification adoption** : Badge "Chauffeur exemplaire", classement par taux d'adoption

**Onboarding rapide (< 2 heures pour 50 véhicules)**
- Import CSV en masse (véhicules + conducteurs + assignations)
- Template pré-rempli fourni
- Validation interactive avec détection de doublons
- Pas besoin de scanner tous les documents existants : ajout progressif au fil des renouvellements

**Conçu pour les experts-comptables et auditeurs**
- Exports standardisés type "registre de conformité" (tous véhicules, tous docs, toutes dates)
- Accès en lecture seule pour tiers (comptables, auditeurs, inspecteurs)
- Traçabilité complète (qui a consulté/modifié quoi, quand)
- Conservation légale 10 ans automatique
- Facturation transparente des add-ons (ligne séparée sur facture mensuelle)

### Validation Terrain - Retours Personas

**✅ Ce qui résonne fortement (validé par stakeholders) :**
- **Dirigeant PME** : "405€ d'amendes évitées l'année dernière, mon pricing actuel est rentable"
- **Assistante administrative** : "Je perds 2-3h/semaine à chercher des docs et mettre à jour Excel"
- **Chauffeur** : "Si c'est 30 secondes pour scanner, je le ferai. Si c'est compliqué, non."
- **Expert-comptable** : "Mes clients ont besoin de traçabilité pour les audits URSSAF"

**⚠️ Inquiétudes adressées dans la conception :**
- **Fiabilité OCR** : Validation humaine obligatoire (pas de création automatique sans vérification)
- **Simplicité mobile** : Onboarding vidéo + QR code + wireframes testés avec chauffeurs 50-60 ans, < 3 clics
- **Notifications efficaces** : Email en standard, SMS et push en add-on payant
- **Documents illisibles** : Workflow de gestion d'erreur (document marqué "à vérifier" + notification gestionnaire)

**🚨 Insight pricing terrain CRITIQUE :**
Un directeur d'entreprise de transport (100 immatriculations : 60 moteurs + 40 remorques) a donné un feedback essentiel :
- "500€/mois c'est trop cher pour ce service"
- "Pas logique de payer le même prix pour les moteurs et les remorques" (remorques = moins de documents)

**→ Décision prise** : Pricing différencié véhicules moteurs vs remorques (voir section Modèle Économique)

### Marché Adressable

**France - 150 000 à 200 000 structures** avec flottes de 10+ véhicules :

- **Transport & Logistique** : ~37 000 entreprises
- **BTP & Construction** : ~50 000 entreprises (>10 salariés)
- **Artisans multi-équipés** : ~30 000 avec flottes 10+ véhicules
- **Services & Distribution** : ~200 000 entreprises
- **Collectivités locales** : ~35 000 communes

**Segmentation par taille** :
- 40% ont 10-25 véhicules → **cœur de cible initial** (40-100€/mois)
- 35% ont 25-50 véhicules → **sweet spot rentabilité** (100-200€/mois)
- 20% ont 50-200 véhicules → **high-value accounts** (150-400€/mois)
- 5% ont 200+ véhicules → **grands comptes** (sur-mesure)

### Modèle Économique

**Pricing différencié Véhicules moteurs vs Remorques** (basé sur feedback terrain)

**Véhicules moteurs** (camions, VUL, voitures, engins) :
- 1-25 véhicules : **4€/véhicule/mois**
- 26-100 véhicules : **3€/véhicule/mois**
- 101+ véhicules : **2,50€/véhicule/mois**

**Remorques** : **1,50€/remorque/mois** (prix fixe, pas de dégressivité)
- Moins de documents à gérer (pas de CT annuel, uniquement assurance + carte grise)

**Exemples concrets de pricing :**

| Type client | Moteurs | Remorques | Calcul mensuel | Prix/mois | Prix annuel (-15%) |
|-------------|---------|-----------|----------------|-----------|---------------------|
| Artisan | 10 camionnettes | 2 remorques | (10×4€) + (2×1,50€) | **43€** | **439€/an** (37€/mois) |
| PME BTP | 30 camions | 15 remorques | (25×4€ + 5×3€) + (15×1,50€) | **138€** | **1 409€/an** (117€/mois) |
| Transporteur | 60 moteurs | 40 remorques | (25×4€ + 35×3€) + (40×1,50€) | **265€** | **2 706€/an** (226€/mois) |
| Grande flotte | 120 moteurs | 80 remorques | (25×4€ + 75×3€ + 20×2,50€) + (80×1,50€) | **495€** | **5 051€/an** (421€/mois) |

**Justification pricing différencié :**
- ✅ **Juste** : Les remorques génèrent 3× moins de documents (pas de CT, FIMO, visite médicale, permis)
- ✅ **Compétitif** : Le transporteur avec 100 immatriculations paie 265€/mois au lieu de 500€ (ancien pricing à 5€/véh)
- ✅ **Répond à l'objection terrain** : "Pas logique même prix moteurs/remorques"

**Stratégie de conversion** :
- **14 jours d'essai gratuit** avec carte bancaire obligatoire (taux de conversion 60-70% vs 20-30% sans CB)
- Auto-renouvellement à J-14 (débit automatique)
- Pas de plan gratuit permanent (stratégie 100% payante)

**Add-ons** (revenus complémentaires) :

1. **OCR automatique** : 0,10€/document scanné
   - Pay-as-you-go, facturé mensuellement
   - Ligne séparée sur facture (ex: "50 documents OCR × 0,10€ = 5€")

2. **Alertes SMS** : 0,50€/véhicule/mois
   - Inclut 4 SMS/mois par véhicule (alertes critiques)
   - Coût réel : 0,035€/SMS (OVH SMS) × 4 = 0,14€/véhicule → marge 0,36€/véhicule
   - SMS supplémentaires : 0,15€/SMS facturé

3. **API Verbalisation ANTAI** : 1€/véhicule/mois (post-MVP P1)
   - Gestion automatisée des PV et contraventions

4. **Add-ons post-MVP** (P2) :
   - API access : 49€/mois (pour intégrations tierces)
   - Export comptable avancé : 29€/mois (formats personnalisés)
   - Support téléphone prioritaire : 99€/mois
   - Onboarding personnalisé : 299€ one-time (formation 2h sur site/visio)

**Projections conservatrices (M12)** :
- 42 clients actifs
- Mix clients : 15 petites flottes (40€/mois) + 15 moyennes (110€/mois) + 8 grandes (200€/mois) + 4 très grandes (400€/mois)
- **MRR : 4 200€** (vs 4 575€ projeté initialement - ajusté avec nouveau pricing)
- **ARR : 50 400€**
- **ARPU moyen : 100€/mois**

**Impact du modèle avec CB obligatoire :**
- Avant (sans CB) : 20-30% conversion → 80 inscriptions = 20 clients payants
- Après (avec CB) : 60-70% conversion → 80 inscriptions = 48 clients payants
- **Gain : +140% de clients payants** pour le même nombre d'inscriptions

## Project Classification

**Technical Type:** SaaS B2B (Web App + Progressive Web App)

**Domain:** Général (gestion documentaire multi-secteurs)

**Complexity:** Moyenne
- Besoin business clairement défini et validé terrain
- Architecture technique à valider/optimiser
- Intégrations externes stratégiques à prioriser
- Conformité RGPD standard + exigences de sécurité renforcées

**Project Context:** Greenfield - nouveau projet

### Contexte Technique Initial

**Stack proposée (à valider/challenger)** :
- Frontend : Next.js 16 + React 19 + TypeScript
- Backend : **Hébergement France dès le début** (Scaleway ou OVH Cloud + PostgreSQL)
- Storage : Scaleway Object Storage ou OVH Object Storage (S3-compatible)
- Auth : NextAuth.js ou custom avec JWT
- Paiements : LemonSqueezy (avec TVA automatique)
- OCR : Mistral OCR 3 via API ($2/1000 pages)
- Emails : Resend
- SMS : OVH SMS (0,035€/SMS - 55% moins cher que Twilio)
- Hosting : Vercel (frontend) + Scaleway/OVH (backend)

**⚠️ Décision architecture critique : Hébergement France**

**Pourquoi hébergement France dès le MVP :**
- ✅ Réponse aux exigences collectivités locales (obligation hébergement données France)
- ✅ Argument commercial pour grands comptes sensibles à la souveraineté données
- ✅ Préparation future certification ISO 27001 (hébergement France = prérequis)
- ✅ Pas de migration douloureuse plus tard

**Compromis acceptés :**
- ❌ Setup initial plus long (2-3 semaines vs 1 semaine avec Supabase)
- ❌ Coûts légèrement plus élevés au démarrage (50-80€/mois vs gratuit Supabase)
- ✅ Mais scalabilité maîtrisée et conformité garantie

**Architecture découplée (anti-lock-in) :**
- Utilisation d'un ORM (Prisma ou Drizzle) pour abstraction DB
- Couche de services métier indépendante de l'infrastructure
- Si besoin de migrer vers autre hébergeur → possible sans refonte

**Seuils de migration à surveiller :**
- > 100 clients actifs → évaluer passage à infrastructure dédiée
- Temps de réponse API > 2 secondes → optimisation urgente
- Coût infrastructure > 300€/mois → réévaluation architecture

### Objectifs MVP (6 semaines)

**Phase Bêta (Mois 0-2)** :
- Recruter **10 entreprises bêta-testeuses** (BTP, Transport, Artisans, Collectivités)
- Accès gratuit 2 mois en échange de feedback régulier
- Objectif : Valider product-market fit, affiner UX, identifier bugs critiques
- **Questions clés à poser aux bêta-testeurs** :
  - "Auriez-vous besoin d'ISO 27001 / SOC2 pour signer ?"
  - "L'hébergement des données en France est-il obligatoire pour vous ?"
  - → Si >50% répondent oui → investir dans certifications à M3
- Taux de conversion cible : **80%+ des bêta-testeurs → clients payants**

**Fonctionnalités P0 (MVP critique - validées par stakeholders)** :

1. **Authentification**
   - Email/password + OAuth Google
   - Essai gratuit 14 jours avec CB obligatoire
   - Multi-utilisateurs avec rôles (Admin, Gestionnaire, Chauffeur)

2. **Gestion véhicules**
   - CRUD véhicules avec distinction **type : moteur vs remorque** (impact pricing)
   - Import CSV en masse avec template fourni
   - Validation interactive avec détection doublons
   - Association chauffeur → véhicule habituel

3. **Upload documents**
   - Drag & drop desktop + scan caméra mobile via PWA
   - **OCR assisté avec validation humaine obligatoire**
   - Gestion documents illisibles (marquage "à vérifier" + notification)
   - Mode offline mobile (scan sans connexion, sync auto au retour réseau)

4. **Onboarding chauffeurs** ⭐ **CRITIQUE POUR ADOPTION**
   - **Vidéo tutoriel 90 secondes** "Comment scanner un document" (envoyée par SMS à chaque chauffeur)
   - **QR code installation PWA** sur dashboard gestionnaire → scan par chauffeur → installation directe
   - **Premier scan guidé** (tooltips, flèches, validation étape par étape)
   - **Gamification** : Badge "Chauffeur exemplaire", classement par taux d'adoption
   - **Dashboard gestionnaire** : vue sur adoption chauffeurs (% ayant scanné au moins 1 doc)
   - **Rappels automatiques** : Email gestionnaire si chauffeur n'a pas scanné depuis 30j
   - **Support smartphones anciens** : Test et optimisation sur Android 7-8

5. **Alertes automatiques**
   - Calcul temps réel (60j, 30j, 15j, expiration)
   - Emails quotidiens/hebdomadaires (inclus)
   - Notifications push PWA (incluses)
   - ~~SMS~~ → **Add-on payant** (0,50€/véh/mois avec OVH SMS)

6. **Dashboard conformité**
   - Vue d'ensemble statut flotte (véhicules OK, à surveiller, critiques)
   - Distinction visuelle moteurs vs remorques
   - Prochaines échéances calendrier
   - Activité récente

7. **Facturation LemonSqueezy**
   - Abonnement avec gestion TVA automatique
   - Pricing différencié moteurs (4€) vs remorques (1,50€)
   - Facturation add-ons sur ligne séparée (OCR, SMS)
   - Historique factures téléchargeable

**Fonctionnalités P1 (post-MVP prioritaires - remontées terrain)** :

- **Import intelligent de documents en masse** (ZIP + OCR + matching automatique avec validation)
- **Exports personnalisables** pour comptables (format "registre de conformité")
- **Accès en lecture seule** pour tiers (comptables, auditeurs)
- **API Verbalisation ANTAI** (add-on 1€/véhicule/mois)
- **Module Super Admin** (analytics, gestion clients, mode démo)
- **Notifications SMS avancées** (personnalisation fréquence, templates)

**Fonctionnalités P2 (Nice-to-have - opportunités futures)** :

- **Intégration email assureurs** (import automatique attestations par parsing email)
- **API publique** pour écosystème tiers
- **Support engins de chantier spéciaux** (pelleteuses, grues, nacelles) avec docs spécifiques
- **Intégration garagistes** (rappels entretien, import factures)
- **Certification ISO 27001** (si >50% bêta-testeurs l'exigent)

### Intégrations Stratégiques à Explorer

**Partenariats prioritaires pour accélération go-to-market :**

1. **Assureurs flottes (AXA, Allianz, MMA, Groupama)**
   - Import automatique attestations par API ou parsing email
   - Co-marketing : assureur recommande FlotteBox à ses clients flottes
   - Réduction de la sinistralité → intérêt mutuel

2. **Experts-comptables et cabinets d'expertise**
   - Accès multi-clients (1 compte comptable = vue sur 15 clients PME)
   - Programme partenaire : commission récurrente sur clients apportés
   - Formation gratuite + certification "FlotteBox Partner"

3. **Chambres des métiers et fédérations professionnelles**
   - CAPEB (BTP), FNTR (Transport), CMA France (Artisans)
   - Référencement sur leurs sites + webinaires de formation
   - Tarif négocié pour adhérents (-20% sur abonnement)

4. **Garagistes et réseaux d'entretien (Midas, Norauto, Speedy)**
   - Export automatique de la liste véhicules avec échéances CT
   - Rappels entretien intégrés dans FlotteBox
   - Lead gen pour garagistes (notifications "CT dans 30j, prenez RDV")

### Risques Identifiés & Plans de Mitigation (Pre-mortem)

**🔴 RISQUE CRITIQUE #1 : Certifications ISO 27001 / SOC2 bloquent ventes grands comptes**

**Scénario d'échec :** Les structures moyennes/grandes (50-200 véhicules, ARPU 200-500€/mois) demandent systématiquement ISO 27001. Sans certification → impossible de vendre au segment rentable.

**Mitigation :**
- ✅ **Approche hybride** : MVP sans certification pour valider le marché
- ✅ **Questions clés pendant bêta** : Interroger les 10 testeurs sur leurs exigences conformité
- ✅ **Seuil de décision à M3** : Si >50% des bêta-testeurs exigent ISO → investir (15-25k€, 6-12 mois)
- ✅ **Segmentation réaliste** : Accepter qu'en M0-M6, seules les petites structures (10-30 véh) peuvent signer

**🔴 RISQUE CRITIQUE #2 : Adoption chauffeurs < 50% → produit inutile**

**Scénario d'échec :** 70% des chauffeurs ne téléchargent/utilisent jamais l'app mobile → gestionnaires scannent tout eux-mêmes → FlotteBox = "juste un autre Excel" → taux de conversion bêta 40% au lieu de 80%.

**Mitigation :**
- ✅ **Onboarding vidéo 90 sec** (P0 MVP) : Envoyée par SMS à chaque chauffeur
- ✅ **QR code installation PWA** (P0 MVP) : Depuis dashboard gestionnaire
- ✅ **Premier scan guidé** (P0 MVP) : Tooltips, validation étape par étape
- ✅ **Gamification** (P0 MVP) : Badge "Chauffeur exemplaire", classement adoption
- ✅ **Support smartphones anciens** (P0 MVP) : Test Android 7-8, version optimisée
- ✅ **Dashboard gestionnaire** (P0 MVP) : Vue sur adoption chauffeurs, rappels automatiques

**🟠 RISQUE MAJEUR #3 : Coûts SMS explosent et tuent la marge**

**Scénario d'échec :** SMS inclus gratuit dans plan de base → 0,035€/SMS × 4 alertes/mois = 0,14€/véhicule → sur pricing 4€/véh = 3,5% du revenu qui part en SMS → marge réduite.

**Mitigation :**
- ✅ **SMS en add-on payant** : 0,50€/véhicule/mois (coût 0,14€, marge 0,36€)
- ✅ **OVH SMS** au lieu de Twilio : 0,035€/SMS vs 0,08€ (55% moins cher)
- ✅ **Email + Push inclus** : Canaux gratuits en standard, SMS optionnel

**🟠 RISQUE MAJEUR #4 : Pricing perçu comme "pas sérieux" par grands comptes**

**Scénario d'échec :** Pricing trop bas (3-5€/véh) signale "produit gadget" → grands comptes refusent → bloqué sur segment petites structures à faible ARPU.

**Mitigation :**
- ✅ **Pricing différencié moteurs/remorques** : Plus sophistiqué, perçu comme "professionnel"
- ✅ **Communication ROI** au lieu de "prix bas" : Focus sur "1 amende évitée = 3 mois payés"
- ✅ **Plan Enterprise sur-devis** (P2) : Pour clients 200+ véh avec certifications ISO, pricing premium

**🟢 RISQUE FAIBLE #5 : Scaleway/OVH ne tiennent pas la charge au-delà de 100 clients**

**Scénario d'échec :** Infrastructure France atteint ses limites → lenteurs, downtime → clients annulent.

**Mitigation :**
- ✅ **Architecture découplée** (ORM Prisma/Drizzle) : Migration facile vers autre hébergeur
- ✅ **Monitoring proactif** : Alertes si temps réponse > 2s ou charge > 70%
- ✅ **Plan de migration** documenté : Passage à infra dédiée si >100 clients actifs

### Questions Ouvertes & À Investiguer

**Conformité & Sécurité** :
- ISO 27001 nécessaire dès M6 ou peut attendre M12-M18 ?
- SOC 2 Type II exigé par quels types de clients ?
- Assurance cyber-risques : obligatoire pour commercialisation B2B ?
- Conservation légale 10 ans : implications coûts stockage Scaleway/OVH ?

**Scalabilité & Architecture** :
- Scaleway vs OVH Cloud : lequel choisir pour rapport performance/coût ?
- PostgreSQL géré (Scaleway Database) ou auto-hébergé sur VPS ?
- Redis cache nécessaire dès MVP ou seulement à >50 clients ?
- CDN Cloudflare devant Vercel : pertinent pour performances France ?

**Positionnement & Go-to-Market** :
- Canaux d'acquisition prioritaires : SEO (guides conformité CT/assurance), partenariats (comptables, assureurs), outbound (LinkedIn) ?
- Stratégie de contenu : calculateur ROI, guides gratuits "Gestion conformité flotte", webinaires ?
- Programme de parrainage : 1 mois offert par client apporté ?
- Pitch deck investisseurs : lever dès M6 (50-100k€ pour certifications + croissance) ou bootstrapper jusqu'à rentabilité ?

**Fonctionnalités critiques à clarifier** :
- Mistral OCR 3 à $2/1000 pages vs alternatives (Google Vision $1,50, AWS Textract $1,50, Tesseract open-source gratuit) ?
- LemonSqueezy : peut-il gérer le pricing différencié moteurs/remorques nativement ou développement custom nécessaire ?
- Mode offline mobile : complexité technique vs impact adoption - faisable en 6 semaines MVP ?
- Support smartphones anciens (Android 7-8) : performance PWA acceptable ou faut-il native app ?

## Success Criteria

### User Success

**Le moment "aha!" :**
L'utilisateur réalise la valeur de FlotteBox lors du **premier scan OCR** qui pré-remplit automatiquement toutes les informations du véhicule et du document. Ce moment tangible et immédiat démontre : "C'est simple et je gagne du temps !".

**Critères de succès utilisateur mesurables :**

**Pour les gestionnaires de flotte (Marie, assistante administrative) :**
- ✅ **Gain de temps réel : 2-3h/semaine économisées** sur la gestion documentaire
- ✅ **Zéro oubli d'échéance** : 100% des alertes déclenchées minimum 30 jours avant expiration
- ✅ **Vision temps réel** : Dashboard conformité flotte chargé en < 2 secondes
- ✅ **Import onboarding rapide** : 50 véhicules importés depuis CSV en < 2 heures
- ✅ **Soulagement administratif** : Plus de stress lié aux échéances documentaires

**Pour les chauffeurs (Karim) :**
- ✅ **Scan ultra-rapide** : Document scanné et envoyé en < 30 secondes
- ✅ **Adoption spontanée** : 60%+ des chauffeurs scannent au moins 1 document/mois sans formation complexe
- ✅ **Zéro friction** : Installation PWA en < 1 minute via QR code
- ✅ **Ça marche partout** : Mode offline fonctionnel (scan sans réseau, sync auto)

**Pour les dirigeants (Philippe) :**
- ✅ **ROI immédiat** : Zéro amende pour documents périmés après adoption FlotteBox
- ✅ **Sérénité contrôles** : Tous documents accessibles en 1 clic lors des audits URSSAF ou inspections
- ✅ **Conformité temps réel** : Vision instantanée du statut conformité de toute la flotte
- ✅ **Efficacité opérationnelle** : Équipe administrative gagne 100-150h/an

**Critère de complétion utilisateur :**
Un utilisateur a "réussi" avec FlotteBox quand **sa flotte est 100% à jour** :
- Tous les véhicules ajoutés dans le système
- Tous les documents obligatoires (carte grise, assurance, CT) uploadés
- Alertes configurées et fonctionnelles
- Au moins 1 cycle de renouvellement traité avec succès (document scanné → alerte reçue → nouveau document uploadé)

**Outcome émotionnel recherché :**
- **Soulagement** : Plus de stress administratif ni de peur d'oublier une échéance
- **Efficacité** : Temps gagné réalloué sur des tâches à plus forte valeur ajoutée

### Business Success

**Objectif stratégique à M12 :** En vivre et potentiellement embaucher une première personne.

**Critères financiers :**

**Phase Bêta (M0-M2) :**
- ✅ **10 entreprises bêta-testeuses** recrutées (BTP, Transport, Artisans, Collectivités)
- ✅ **Taux de conversion bêta → payant : 80%+** (8/10 minimum convertissent à la fin de la période gratuite)
- ✅ **Validation questions clés** : >50% des bêta-testeurs indiquent besoin ISO 27001 ou non

**Court terme (M6) :**
- ✅ **MRR : 3 000-5 000€** (métrique primaire de succès)
- ✅ **20-30 clients payants** (focus qualité > quantité)
- ✅ **ARPU moyen : 100-150€/mois** (privilégier moyennes flottes vs petites)
- ✅ **Bouche-à-oreille : 20%+ des nouveaux clients** viennent par recommandation
- ✅ **Churn mensuel : < 5%** (signe de product-market fit)

**Moyen terme (M12) :**
- ✅ **MRR : 5 000-6 000€** (vivre de FlotteBox - objectif minimum)
- ✅ **MRR stretch goal : 8 500-9 500€** (embaucher une personne)
- ✅ **40-50 clients actifs** (objectif minimum)
- ✅ **60-80 clients actifs** (stretch goal pour embauche)
- ✅ **ARPU moyen maintenu : 100-150€/mois**
- ✅ **Churn mensuel : < 3%** (rétention forte)
- ✅ **Taux de recommandation : 30%+ NPS > 50**

**Long terme (M24) :**
- ✅ **MRR : 15 000-20 000€**
- ✅ **100+ clients actifs**
- ✅ **Équipe : 2-3 personnes** (développeur + commercial/support)

**Seuils de décision critiques :**

| Métrique | Seuil | Action déclenchée |
|----------|-------|-------------------|
| **MRR à M6** | > 5 000€ | 🚀 Lever fonds (50-100k€) pour certifications ISO + croissance |
| **MRR à M12** | > 8 500€ | 👥 Embaucher première personne (dev ou commercial) |
| **Churn mensuel** | > 5% | 🔄 Pivoter stratégie produit/onboarding |
| **Conversion bêta** | < 60% | 🔧 Refonte complète onboarding + UX |
| **Adoption chauffeurs** | < 50% | 🎯 Retravailler app mobile + gamification |

**Critère de réussite business principal :**
**"Je peux en vivre à M12"** = 5 000-6 000€ MRR stable avec churn < 3%.

### Technical Success

**🔴 Critères CRITIQUES (tuent le produit si non atteints) :**

1. **OCR : précision > 90% sur champs clés**
   - Immatriculation : 95%+ de précision
   - Dates (expiration, émission) : 90%+ de précision
   - Type de document (CT, assurance, carte grise) : 85%+ de précision
   - **Validation humaine obligatoire** : 100% des documents pré-remplis nécessitent confirmation utilisateur
   - **Gestion erreurs illisibles** : Documents marqués "à vérifier" + notification gestionnaire

2. **Adoption chauffeurs : > 60% utilisent l'app mobile**
   - 60%+ des chauffeurs scannent au moins 1 document/mois (dans flottes >10 véhicules)
   - Onboarding vidéo visionnée par 80%+ des chauffeurs
   - Installation PWA via QR code : taux de succès > 90%
   - Temps moyen premier scan : < 2 minutes après installation
   - **Note** : Gamification activée automatiquement si entreprise >10 utilisateurs OU >5 scans/mois (pertinent pour transporteurs, pas pour artisans avec usage sporadique)

**🟠 Critères MAJEURS (dégradent l'expérience) :**

3. **Performance : temps de chargement < 2 secondes**
   - Dashboard homepage : < 2s
   - Liste véhicules (100 véhicules) : < 2s
   - Upload document : < 5s pour PDF 2 MB
   - Scan mobile + OCR : < 10s total

4. **Disponibilité : uptime > 99%**
   - Downtime mensuel : < 7h/mois
   - Temps de récupération après incident : < 1h
   - Alertes monitoring : notification si temps réponse > 3s

5. **Scalabilité : support 100+ clients simultanés**
   - Architecture Scaleway/OVH tient la charge jusqu'à 100 clients
   - Plan de migration documenté si >100 clients actifs
   - Monitoring proactif : alertes à 70% charge

**🟢 Critères IMPORTANTS (nice-to-have) :**

6. **Mode offline mobile** : scan sans connexion + sync automatique
7. **Support smartphones anciens** : PWA fonctionnelle sur Android 7-8
8. **Sécurité** : conformité RGPD + hébergement France dès MVP

**Critère de succès technique principal :**
**"L'OCR fonctionne assez bien (>90%) ET les chauffeurs l'utilisent vraiment (>60%)"** = FlotteBox délivre sa promesse de gain de temps.

### Measurable Outcomes

**Timeline de validation du succès :**

**M2 (Fin bêta) :**
- [ ] 10 bêta-testeurs recrutés
- [ ] 8/10 convertissent en clients payants (80%)
- [ ] Adoption chauffeurs > 60% constatée chez les bêta-testeurs
- [ ] OCR précision mesurée : >90% sur immatriculations
- [ ] NPS bêta-testeurs : > 40

**M6 :**
- [ ] MRR : 3 000-5 000€
- [ ] 20-30 clients payants
- [ ] Churn < 5%
- [ ] 20%+ nouveaux clients par recommandation
- [ ] Décision : lever fonds ou bootstrapper ?

**M12 :**
- [ ] MRR : 5 000-6 000€ (minimum pour en vivre)
- [ ] MRR stretch : 8 500-9 500€ (embaucher)
- [ ] 40-50 clients actifs
- [ ] Churn < 3%
- [ ] NPS > 50
- [ ] Décision : embaucher première personne ?

**Indicateurs d'alerte (red flags) :**
- Churn > 5% pendant 2 mois consécutifs → problème produit
- Adoption chauffeurs < 50% → revoir UX mobile
- Conversion bêta < 60% → revoir onboarding/pricing
- Croissance MRR < 500€/mois → revoir stratégie acquisition

## Product Scope

### MVP - Minimum Viable Product (M0-M2)

**Objectif MVP :** Valider que FlotteBox résout le problème de gestion documentaire ET que les utilisateurs sont prêts à payer pour la solution.

**Périmètre fonctionnel P0 (6 semaines de développement) :**

**1. Authentification & Gestion utilisateurs**
- Email/password + OAuth Google
- Essai gratuit 14 jours avec CB obligatoire
- Multi-utilisateurs avec rôles (Admin, Gestionnaire, Chauffeur)

**2. Gestion véhicules**
- CRUD véhicules avec distinction type : **moteur vs remorque** (impact pricing)
- Import CSV en masse avec template fourni
- Validation interactive avec détection doublons
- Association chauffeur → véhicule habituel

**3. Upload documents + OCR assisté** ⭐ **KILLER FEATURE**
- Drag & drop desktop + scan caméra mobile via PWA
- **OCR assisté avec validation humaine obligatoire** (précision >90%)
- Gestion documents illisibles (marquage "à vérifier" + notification)
- Mode offline mobile (scan sans connexion, sync auto au retour réseau)

**4. Onboarding chauffeurs** ⭐ **CRITIQUE ADOPTION**
- Vidéo tutoriel 90 secondes "Comment scanner un document" (SMS à chaque chauffeur)
- QR code installation PWA sur dashboard gestionnaire
- Premier scan guidé (tooltips, flèches, validation étape par étape)
- **Gamification contextuelle** : Badge "Chauffeur exemplaire" + classement activés automatiquement si entreprise >10 utilisateurs OU >5 scans/mois (pertinent pour transporteurs, inutile pour artisan menuisier)
- Dashboard gestionnaire : vue adoption chauffeurs (% ayant scanné ≥1 doc)
- Rappels automatiques : Email gestionnaire si chauffeur n'a pas scanné depuis 30j
- Support smartphones anciens : Test et optimisation sur Android 7-8

**5. Alertes automatiques**
- Calcul temps réel (60j, 30j, 15j, expiration)
- Emails quotidiens/hebdomadaires (inclus)
- Notifications push PWA (incluses)
- SMS → Add-on payant (0,50€/véh/mois avec OVH SMS)

**6. Dashboard conformité**
- Vue d'ensemble statut flotte (véhicules OK, à surveiller, critiques)
- Distinction visuelle moteurs vs remorques
- Prochaines échéances calendrier
- Activité récente

**7. Facturation LemonSqueezy**
- Abonnement avec gestion TVA automatique
- Pricing différencié moteurs (4€) vs remorques (1,50€)
- Facturation add-ons sur ligne séparée (OCR, SMS)
- Historique factures téléchargeable

**Critère de succès MVP :**
- 8/10 bêta-testeurs convertissent en clients payants
- Adoption chauffeurs > 60% chez les bêta-testeurs (flottes >10 véh)
- OCR précision > 90% sur immatriculations
- Temps de scan mobile : < 30 secondes

### Growth Features (Post-MVP) - M3-M12

**Fonctionnalités P1 (prioritaires après validation MVP) :**

**1. Import intelligent de documents en masse**
- ZIP + OCR + matching automatique avec validation
- Mise à jour en masse (50 attestations assurance → associées automatiquement aux 50 véhicules)
- Workflow validation intermédiaire avant création/remplacement

**2. Exports personnalisables pour comptables**
- Format "registre de conformité" (tous véhicules, tous docs, toutes dates)
- Exports Excel/PDF avec colonnes configurables
- Templates pré-définis pour experts-comptables

**3. Accès en lecture seule pour tiers**
- Comptables, auditeurs, inspecteurs
- Permissions granulaires par utilisateur externe
- Traçabilité accès (qui a consulté quoi, quand)

**4. API Verbalisation ANTAI**
- Add-on 1€/véhicule/mois
- Gestion automatisée des PV et contraventions
- Alertes paiement avant majoration
- Suivi des contestations

**5. Module Super Admin**
- Dashboard analytics global (MRR, churn, ARPU, NPS)
- Gestion clients avec filtres avancés (statut, plan, engagement)
- Vue 360° client (métriques engagement, usage, santé compte)
- Mode démo pour présentations commerciales
- Event tracking (funnel activation, feature adoption)

**6. Notifications SMS avancées**
- Personnalisation fréquence par type d'alerte
- Templates SMS personnalisables
- Gestion quotas SMS inclus vs payants

**Critère de succès Growth Phase :**
- MRR > 5 000€ à M6
- Churn < 5%
- 20%+ clients viennent par recommandation

### Vision (Future) - M12-M24

**Fonctionnalités P2 (opportunités futures) :**

**1. Intégrations stratégiques**
- **Assureurs flottes** : Import automatique attestations par API ou parsing email
- **Garagistes** : Rappels entretien, import factures maintenance
- **ERP clients** : Export automatisé vers systèmes comptables
- **API publique** : Écosystème tiers pour intégrations custom

**2. Certification & Conformité Enterprise**
- **ISO 27001** (si >50% bêta-testeurs l'exigent)
- **SOC 2 Type II** (pour grands comptes exigeants)
- Audits sécurité annuels
- SLA 99.9% uptime

**3. Fonctionnalités avancées**
- Support engins de chantier spéciaux (pelleteuses, grues, nacelles) avec documents spécifiques
- Multi-devises et multi-pays (expansion européenne)
- Mobile app native (si PWA montre ses limites sur vieux smartphones)
- Intelligence prédictive : "Votre CT expire dans 45j, voici 3 garages disponibles près de vous"

**4. Positionnement premium**
- **Plan Enterprise sur-devis** pour flottes 200+ véhicules
- Pricing premium avec certifications ISO incluses
- Onboarding dédié + Account Manager
- Support téléphone prioritaire

**Critère de succès Vision :**
- MRR > 15 000€ à M24
- 100+ clients actifs
- Équipe 2-3 personnes
- Reconnaissance marché : référence SaaS gestion documentaire flottes France

## User Journeys

### Journey 1: Marie Dubois - Gestionnaire de flotte libérée du chaos administratif

Marie Dubois est assistante administrative dans une PME de transport routier de 45 véhicules (35 camions + 10 remorques). Elle jongle entre Excel, classeurs physiques et emails pour gérer les documents de la flotte. Chaque lundi matin, elle passe 1h30 à vérifier manuellement les échéances sur son tableau Excel et à envoyer des rappels par email aux chauffeurs. La semaine dernière, elle a raté l'échéance d'un contrôle technique : 135€ d'amende + immobilisation du camion pendant 2 jours. Son patron Philippe lui met une pression constante : "Marie, on ne peut pas se permettre ces oublis !"

Un matin, Philippe lui montre FlotteBox après l'avoir découvert via un article LinkedIn. "On va tester ça 14 jours, si ça marche on garde." Marie est sceptique - encore un nouvel outil compliqué ? - mais elle décide de tenter. L'onboarding est surprenant : elle télécharge le fichier CSV template, copie-colle ses données depuis Excel (immatriculations, modèles, chauffeurs), et importe le tout en 15 minutes. FlotteBox détecte 3 doublons et les signale. Elle corrige, valide, et voilà : ses 45 véhicules sont dans le système.

Le vrai "aha moment" arrive le lendemain. Karim, l'un des chauffeurs, vient de recevoir la nouvelle attestation d'assurance pour son camion. Marie ouvre l'app mobile via le QR code du dashboard, scanne le document en 20 secondes. **L'OCR pré-remplit automatiquement** : numéro de police, date d'expiration (12/04/2027), immatriculation du véhicule (AB-123-CD). Elle vérifie rapidement les champs, clique sur "Valider", et c'est terminé. Le système lui dit : "Alerte configurée pour le 12/03/2027 (30 jours avant expiration)". Elle réalise : "Je viens de gagner 8 minutes de saisie manuelle ET je n'aurai plus jamais à me rappeler de cette date."

Deux mois plus tard, Marie a scanné 127 documents. Le dashboard lui montre que sa flotte est à 94% conforme (3 véhicules manquent encore des docs). Chaque matin, elle reçoit un email récapitulatif des échéances des 30 prochains jours. Plus de stress, plus d'oublis. Elle a récupéré 2h30/semaine qu'elle consacre maintenant à des tâches à plus forte valeur ajoutée. Quand Philippe lui demande "Et FlotteBox ?", elle répond : "Je ne pourrais plus m'en passer. C'est devenu mon réflexe."

**Impact mesurable** : 2h30/semaine économisées, 0 amende depuis adoption, 100% des alertes reçues 30j avant expiration.

---

### Journey 2: Karim Benali - Chauffeur routier qui scanne sans y penser

Karim Benali, 52 ans, conduit des poids lourds depuis 28 ans. Il n'est pas à l'aise avec les smartphones - il utilise le sien uniquement pour téléphoner et WhatsApp avec sa famille. Quand Marie lui envoie un SMS lui demandant de "scanner des documents avec une app", il soupire. "Encore un truc compliqué qui va me prendre du temps."

Le SMS contient un lien vers une vidéo de 90 secondes. Karim la regarde pendant sa pause café. La vidéo montre un chauffeur comme lui qui scanne un document en 3 étapes simples : 1) Cliquer sur le QR code, 2) Installer l'app (1 clic), 3) Prendre une photo du document. Ça a l'air gérable. Il clique sur le QR code dans le SMS, l'app s'installe en 30 secondes, et il voit apparaître une interface ultra-simple avec un gros bouton "Scanner un document".

Le lendemain, Karim reçoit son attestation d'assurance renouvelée. Il se souvient de la vidéo. Il ouvre l'app, appuie sur "Scanner", prend une photo de l'attestation, et l'app lui montre un écran de confirmation : "Document envoyé à Marie - Merci Karim !". Temps total : **22 secondes**. Il se dit : "C'est même pas compliqué en fait."

Trois mois plus tard, Karim a scanné 8 documents (permis poids lourd, visite médicale, attestations). L'app lui a même donné un badge "Chauffeur exemplaire" avec une petite étoile dorée. Il ne l'avouerait jamais à ses collègues, mais il en est un peu fier. FlotteBox est devenu un réflexe : document reçu → photo → envoi. Il ne pense même plus à l'administratif - Marie s'occupe de tout le reste. Et surtout, **il n'a plus besoin de ramener les documents physiques au bureau** ou de les envoyer par email.

**Impact mesurable** : Installation en 30 secondes, scan moyen en 22 secondes, 8 documents scannés en 3 mois sans formation supplémentaire.

---

### Journey 3: Philippe Moreau - Dirigeant qui dort mieux la nuit

Philippe Moreau dirige une entreprise de transport de 35 camions et 10 remorques depuis 15 ans. L'année dernière, il a payé 405€ d'amendes pour documents périmés (2 CT oubliés, 1 assurance expirée). Lors d'un contrôle routier, l'un de ses chauffeurs s'est fait immobiliser 48h pour un CT périmé de 12 jours. Perte sèche : 1 200€ (chauffeur payé à ne rien faire + livraison retardée = client mécontent).

Philippe sait qu'il a un problème de gestion administrative, mais les logiciels TMS qu'il a évalués coûtent 800-1 500€/mois et nécessitent plusieurs jours de formation. Trop cher, trop compliqué pour son besoin simple : juste ne plus oublier les échéances.

Un soir, en scrollant LinkedIn, il tombe sur un post d'un autre dirigeant de transport qui partage son expérience avec FlotteBox : "Fini les oublis de CT, 138€/mois pour ma flotte de 30 véhicules, ROI immédiat." Philippe fait le calcul : **265€/mois pour ses 100 immatriculations** (60 moteurs + 40 remorques avec le pricing différencié). C'est 3 fois moins cher qu'un TMS complet. Il s'inscrit pour l'essai gratuit 14 jours.

Le **"aha moment"** de Philippe arrive 10 jours après le lancement. Il ouvre le dashboard FlotteBox et voit une interface claire avec 3 voyants :
- 🟢 **32 véhicules conformes** (tous documents à jour)
- 🟠 **8 véhicules à surveiller** (échéance dans 30-60j)
- 🔴 **5 véhicules critiques** (échéance dans -15j)

Il clique sur les véhicules critiques et voit immédiatement lesquels ont besoin d'action. Pour la première fois depuis des années, **il a une vision temps réel de la conformité de sa flotte**. Il calcule mentalement : 1 seule amende évitée (135€) = la moitié de son abonnement mensuel remboursée. Et le temps que Marie gagne (2h30/semaine × 4 semaines = 10h/mois) vaut au moins 150€. ROI évident.

Six mois plus tard, Philippe n'a plus eu **aucune amende** pour documents périmés. Lors du dernier audit URSSAF, l'inspecteur lui a demandé les registres des conducteurs (permis, visites médicales, FIMO). Marie a exporté un PDF "Registre de conformité conducteurs" depuis FlotteBox en 2 clics. L'inspecteur a hoché la tête : "Impeccable, vous êtes bien organisé." Philippe a souri - avant FlotteBox, cet audit aurait été un cauchemar de 3 jours à chercher des documents dans des classeurs poussiéreux.

**Impact mesurable** : 0 amende depuis adoption (vs 405€/an avant), audit URSSAF passé en 2h au lieu de 3 jours, ROI positif dès le premier mois.

---

### Journey 4: Julien Marchand - Expert-comptable qui gère 12 clients flottes

Julien Marchand est expert-comptable dans un cabinet de 8 personnes. Il a 12 clients PME qui gèrent des flottes de véhicules (artisans, transporteurs, entreprises BTP). Chaque année, lors des audits URSSAF ou des contrôles fiscaux, ces clients l'appellent en panique : "Julien, ils me demandent les registres des conducteurs, les assurances des 3 dernières années, tu peux m'aider ?"

Le problème : ses clients stockent leurs documents n'importe comment (classeurs physiques, Dropbox personnel, emails éparpillés). Julien passe 4-6h par client à reconstituer des registres de conformité avant chaque audit. C'est chronophage, mal facturé, et ses clients sont stressés.

Un de ses clients PME (Philippe, transporteur) adopte FlotteBox et en parle à Julien lors de leur rendez-vous trimestriel. "Regarde, je peux te donner un accès en lecture seule à tous mes documents de flotte." Philippe crée un compte "Tiers - Expert-comptable" pour Julien avec permissions granulaires : lecture seule, pas de modification. Julien se connecte et découvre une interface claire avec tous les véhicules, tous les documents classés, toutes les dates d'expiration.

Le **"aha moment"** de Julien arrive 2 mois plus tard. L'URSSAF audite Philippe. Au lieu de passer 6h à chercher et reconstituer les registres, Julien se connecte à FlotteBox, clique sur "Exporter > Registre de conformité conducteurs (URSSAF)", choisit la période (3 dernières années), et télécharge un PDF de 42 pages parfaitement structuré : tous les conducteurs, tous les permis, toutes les visites médicales, toutes les FIMO, avec dates d'expiration et statuts. **Temps total : 2 minutes au lieu de 6 heures.**

Julien réalise le potentiel. Il contacte FlotteBox pour proposer un partenariat : "Je veux recommander FlotteBox à mes 12 clients flottes. En échange, je souhaite un accès multi-clients (1 compte Julien = vue sur 12 entreprises) et une commission récurrente sur les clients que j'apporte." FlotteBox accepte et crée un programme partenaire avec 15% de commission récurrente.

Un an plus tard, Julien a converti 9 de ses 12 clients à FlotteBox. Il économise **40-50h/an** de temps administratif sur les audits, qu'il peut facturer à d'autres clients. Il touche 180€/mois de commissions passives. Et surtout, ses clients le voient comme un conseiller moderne qui leur fait gagner du temps ET de l'argent.

**Impact mesurable** : 40-50h/an économisées, 180€/mois de revenus passifs, 9/12 clients convertis en 1 an.

---

### Journey 5: Inspecteur URSSAF - Audit en 2 minutes au lieu de 2 heures

Inspecteur URSSAF depuis 12 ans, je contrôle la conformité des entreprises employant des conducteurs routiers. Mon travail : vérifier que tous les permis, visites médicales et formations (FIMO/FCO) sont à jour. La loi est stricte : un conducteur sans visite médicale valide = infraction grave.

Le problème récurrent : 80% des entreprises que j'audite ne sont **pas du tout organisées**. Classeurs physiques avec des photocopies illisibles, documents manquants ("je crois qu'il est dans le camion, je vais appeler le chauffeur"), dates d'expiration notées à la main sur des Post-it. Je passe **2 à 3 heures par entreprise** à reconstituer manuellement les registres et à vérifier chaque date une par une. C'est pénible pour tout le monde.

Ce matin, j'arrive chez Philippe Moreau (entreprise de transport, 60 conducteurs). Je m'attends à la routine habituelle : classeurs poussiéreux et Excel approximatif. Surprise : Marie, l'assistante administrative, m'accueille avec un sourire détendu. "Bonjour, tous nos documents sont dans FlotteBox, je peux vous donner un accès temporaire ou exporter ce dont vous avez besoin."

Je demande le registre de conformité conducteurs des 3 dernières années. Marie clique sur "Exporter > Registre URSSAF", sélectionne la période, et télécharge un PDF structuré :
- Tableau récapitulatif : 60 conducteurs, statut conformité (vert/orange/rouge)
- Pour chaque conducteur : permis (type, date expiration), visite médicale (date dernière visite, prochaine échéance), FIMO/FCO (date formation, validité)
- Documents scannés joints en annexe (permis, certificats médicaux, attestations formation)

**Je vérifie l'ensemble en 2 minutes**. Tout est à jour, tout est traçable, tout est conforme. Je note : "Entreprise exemplaire - gestion documentaire irréprochable". Marie me raconte que FlotteBox leur envoie des alertes 30j avant chaque expiration, donc ils sont toujours en avance. Elle ajoute : "Avant, on était stressés pendant des jours avant un audit. Maintenant, on est sereins."

Je repars 15 minutes après mon arrivée (vs 2-3h habituellement). Dans mon rapport, je note FlotteBox comme "bonne pratique à recommander aux autres entreprises du secteur". Si toutes les entreprises étaient aussi bien organisées, mon travail serait 10 fois plus efficace.

**Impact mesurable** : Audit passé de 2-3h à 15 minutes, conformité 100% vérifiable en temps réel, entreprise notée "exemplaire".

---

### Journey 6: Quentin - Super Admin qui fait grandir FlotteBox

Quentin, fondateur de FlotteBox, a lancé son MVP il y a 6 mois. Aujourd'hui, il a 28 clients payants et un MRR de 3 200€. Mais il sait qu'il doit optimiser son produit pour atteindre son objectif de 5 000€ MRR à M12. Le problème : **il n'a pas assez de visibilité sur ce qui fonctionne et ce qui ne fonctionne pas**.

Quentin se connecte au **Module Super Admin** (P1, développé à M4) et accède à son dashboard analytics :

**Vue d'ensemble (Dashboard principal)** :
- 📊 **MRR actuel : 3 200€** (+12% vs mois dernier)
- 📈 **ARPU moyen : 114€/mois** (objectif : 100-150€ ✅)
- 🔄 **Churn mensuel : 3,2%** (objectif : <5% ✅)
- ⭐ **NPS : 58** (objectif : >50 ✅)
- 👥 **28 clients actifs** (objectif M6 : 20-30 ✅)

**Insights critiques (Analytics détaillées)** :
- ⚠️ **Taux de conversion bêta → payant : 72%** (objectif 80%) - 2 clients sur 10 n'ont pas converti
- ✅ **Adoption chauffeurs : 67%** (objectif >60%) - La gamification fonctionne !
- 🔍 **Feature adoption** : OCR utilisé par 89% des clients, SMS add-on adopté par seulement 12%

Quentin clique sur "Clients à risque" et voit 3 entreprises avec un score de santé < 40/100 :
- **Artisan Menuisier Dupont** : 0 scan depuis 45 jours, 2 véhicules, engagement faible → risque de churn élevé
- **PME BTP Martin** : 15 véhicules, mais adoption chauffeurs à 30% seulement → problème d'onboarding
- **Transporteur Petit** : MRR 265€, mais 0 utilisation du dashboard depuis 15j → possiblement en difficulté

Il décide d'agir immédiatement :
1. Appeler l'Artisan Dupont pour comprendre pourquoi il n'utilise pas FlotteBox (usage sporadique ? Problème technique ?)
2. Envoyer un email à la PME Martin avec un lien vers la vidéo d'onboarding chauffeurs
3. Proposer un RDV démo à Transporteur Petit pour l'aider à tirer pleinement parti du dashboard

**Le "aha moment"** de Quentin arrive quand il utilise le **Mode Démo** pour faire une présentation commerciale. Il active le mode démo, qui génère instantanément une entreprise fictive (Transports Démo SAS, 40 véhicules, 12 chauffeurs) avec des données réalistes : documents scannés, alertes configurées, dashboard conformité rempli. Il peut faire des démos commerciales sans exposer les données réelles de ses clients. Il convertit 2 prospects en clients la semaine suivante grâce à des démos ultra-fluides.

Six mois plus tard (M12), Quentin atteint **5 400€ MRR** avec 42 clients actifs. Il analyse les données du Super Admin et décide d'embaucher une première personne : un développeur full-stack pour l'aider à scaler le produit. FlotteBox n'est plus un side-project - c'est devenu son activité principale.

**Impact mesurable** : 5 400€ MRR à M12 (objectif atteint), churn maintenu à 2,8%, 3 clients à risque sauvés grâce aux alertes proactives.

---

### Journey Requirements Summary

Ces 6 journeys révèlent les capacités fonctionnelles nécessaires pour que FlotteBox délivre sa promesse :

**Capacités Gestion de flotte (Journeys Marie, Philippe)** :
- Import CSV en masse (véhicules + chauffeurs + assignations) avec détection doublons
- CRUD véhicules avec distinction moteurs vs remorques
- Dashboard conformité temps réel avec statuts visuels (vert/orange/rouge)
- Vue prochaines échéances calendrier
- Export "Registre de conformité" multi-formats (PDF, Excel)

**Capacités OCR et Documents (Journeys Marie, Karim)** :
- Scan mobile ultra-simple (< 3 clics)
- OCR assisté avec pré-remplissage automatique et validation humaine obligatoire
- Mode offline (scan sans réseau, sync auto)
- Gestion documents illisibles (marquage "à vérifier" + notification)
- Classification automatique par type de document

**Capacités Onboarding et Adoption Chauffeurs (Journey Karim)** :
- Vidéo tutoriel 90 secondes envoyée par SMS
- QR code installation PWA instantanée
- Premier scan guidé (tooltips, validation étapes)
- Gamification contextuelle (badges, classement)
- Support smartphones anciens (Android 7-8)
- Dashboard adoption chauffeurs pour gestionnaires
- Rappels automatiques si inactivité > 30j

**Capacités Alertes et Notifications (Journeys Marie, Philippe)** :
- Calcul temps réel des échéances (60j, 30j, 15j, expiration)
- Emails quotidiens/hebdomadaires récapitulatifs
- Notifications push PWA
- SMS add-on payant (0,50€/véh/mois)
- Personnalisation fréquence et types d'alertes

**Capacités Multi-utilisateurs et Permissions (Journey Julien)** :
- Rôles : Admin, Gestionnaire, Chauffeur, Tiers (lecture seule)
- Accès multi-clients pour comptables (1 compte = vue sur N entreprises)
- Permissions granulaires par utilisateur externe
- Traçabilité accès (qui a consulté/modifié quoi, quand)

**Capacités Conformité et Audit (Journey Inspecteur URSSAF)** :
- Export "Registre de conformité conducteurs" (permis, visites médicales, FIMO)
- Export "Registre de conformité véhicules" (cartes grises, assurances, CT)
- Période sélectionnable (3 dernières années, date custom)
- Documents scannés joints en annexe
- Format PDF structuré et professionnel

**Capacités Super Admin et Analytics (Journey Quentin)** :
- Dashboard analytics global (MRR, churn, ARPU, NPS)
- Métriques d'engagement par client (score de santé, usage features)
- Alertes clients à risque (churn prediction)
- Funnel activation et feature adoption
- Mode démo pour présentations commerciales (données fictives réalistes)
- Gestion clients avec filtres avancés (statut, plan, engagement)
- Event tracking détaillé

**Capacités Facturation (Journeys Marie, Philippe)** :
- Pricing différencié moteurs vs remorques
- Facturation add-ons sur ligne séparée (OCR, SMS)
- Gestion TVA automatique (LemonSqueezy)
- Historique factures téléchargeable
- Essai gratuit 14 jours avec CB obligatoire

## SaaS B2B Specific Requirements

### Multi-Tenant Architecture

**Isolation des données par entreprise cliente** :
- Architecture multi-tenant stricte : chaque entreprise (tenant) ne voit QUE ses propres données
- Isolation au niveau base de données : Colonne `company_id` sur toutes les entités (véhicules, documents, utilisateurs, alertes)
- Requêtes systématiquement filtrées par `company_id` pour garantir zéro fuite de données entre clients
- Exemple concret :
  - Entreprise Transport Dupont (company_id=1) → voit uniquement ses 50 véhicules
  - Entreprise BTP Martin (company_id=2) → voit uniquement ses 30 véhicules
  - Aucun croisement possible entre tenants

**Facturation usage-based (pas de quotas fixes)** :
- Pas de limite arbitraire de véhicules par entreprise (illimité)
- Facturation mensuelle basée sur l'usage réel : nombre exact de véhicules moteurs + remorques déclarés
- LemonSqueezy Usage-Based Billing : compteur en temps réel du nombre de véhicules actifs par tenant
- Calcul automatique fin de mois : (X moteurs × tarif tier correspondant) + (Y remorques × 1,50€) + add-ons activés
- Ajout/suppression véhicule → recalcul automatique de la facture suivante (pas de prorata immédiat pour MVP)

### Permission Model (RBAC - Role-Based Access Control)

**5 rôles système définis** :

**1. Super Admin (Quentin - hors tenant)** :
- Accès global cross-tenant pour gestion plateforme
- Dashboard analytics (MRR, churn, ARPU, NPS, clients à risque)
- Gestion multi-clients avec filtres avancés
- Mode démo pour présentations commerciales
- Event tracking et funnel activation
- Pas de compte "Super Admin" accessible aux clients (rôle système réservé)

**2. Admin (niveau entreprise)** :
- Full access sur son entreprise uniquement (tenant isolé)
- Gérer utilisateurs (créer/modifier/supprimer comptes Admin, Gestionnaire, Chauffeur, Tiers)
- Gérer abonnement et paiement (LemonSqueezy billing portal)
- Modifier paramètres entreprise (nom, adresse, logo, configuration alertes globales)
- Accès complet à tous les véhicules et documents de l'entreprise
- Dashboard conformité global de la flotte

**3. Gestionnaire (niveau opérationnel)** :
- CRUD véhicules et documents
- Import CSV en masse
- Export registres de conformité
- Vue dashboard conformité flotte
- Paramétrer alertes pour véhicules spécifiques
- **NE PEUT PAS** : Modifier abonnement, créer/supprimer utilisateurs, accéder aux paramètres entreprise

**4. Chauffeur (niveau mobile)** :
- Scanner documents (mobile PWA)
- **Voir uniquement les véhicules qui lui sont assignés** (pas toute la flotte pour éviter surcharge UX)
- Consultation documents et infos du véhicule assigné (carte grise, assurance, CT, permis, visite médicale)
- Recevoir notifications push pour alertes échéances sur ses véhicules
- **NE PEUT PAS** : Voir les autres véhicules, modifier configuration, accéder dashboard gestionnaire

**5. Tiers - Comptable/Auditeur (lecture seule externe)** :
- Accès en lecture seule cross-client pour comptables (1 compte Julien = vue sur N entreprises clientes)
- Export registres conformité (PDF, Excel)
- Consultation documents et véhicules (pas de modification)
- Traçabilité accès : audit log enregistre qui a consulté quoi, quand
- Permissions granulaires configurables par Admin (ex: accès uniquement docs conducteurs, pas véhicules)

**Matrice de permissions RBAC** :

| Fonctionnalité | Super Admin | Admin | Gestionnaire | Chauffeur | Tiers |
|----------------|-------------|-------|--------------|-----------|-------|
| Dashboard analytics global | ✅ | ❌ | ❌ | ❌ | ❌ |
| Gestion multi-clients | ✅ | ❌ | ❌ | ❌ | ✅ (lecture) |
| Gérer utilisateurs | ✅ | ✅ | ❌ | ❌ | ❌ |
| Gérer abonnement | ✅ | ✅ | ❌ | ❌ | ❌ |
| CRUD véhicules | ✅ | ✅ | ✅ | ❌ | ❌ |
| Scanner documents | ✅ | ✅ | ✅ | ✅ | ❌ |
| Export registres | ✅ | ✅ | ✅ | ❌ | ✅ |
| Vue tous véhicules flotte | ✅ | ✅ | ✅ | ❌ | ✅ (selon permissions) |
| Vue véhicules assignés | ✅ | ✅ | ✅ | ✅ | ❌ |
| Paramètres entreprise | ✅ | ✅ | ❌ | ❌ | ❌ |

### Subscription Tiers & Billing Implementation

**Modèle de facturation usage-based avec LemonSqueezy** :

**Tarification dynamique implémentée via LemonSqueezy Usage-Based Billing** :
- Utilisation du système de compteurs LemonSqueezy pour tracking en temps réel
- 2 compteurs séparés par tenant :
  - `vehicle_motors_count` : Nombre de véhicules moteurs actifs (camions, VUL, voitures, engins)
  - `vehicle_trailers_count` : Nombre de remorques actives
- Calcul pricing tier automatique selon le nombre de moteurs :
  - 1-25 moteurs : 4€/moteur/mois
  - 26-100 moteurs : 3€/moteur/mois
  - 101+ moteurs : 2,50€/moteur/mois
- Remorques : prix fixe 1,50€/remorque/mois (pas de dégressivité)
- Facture mensuelle calculée automatiquement par LemonSqueezy : `(Σ moteurs × tarif tier) + (Σ remorques × 1,50€) + add-ons`

**Gestion des add-ons** :
- **OCR automatique** : 0,10€/document scanné (pay-as-you-go)
  - Compteur LemonSqueezy `ocr_scans_count` incrémenté à chaque scan validé
  - Facturé mensuellement, ligne séparée sur facture
- **Alertes SMS** : 0,50€/véhicule/mois activé (forfait 4 SMS inclus)
  - Activation/désactivation self-service par Admin depuis dashboard paramètres
  - Custom développement : Toggle "Activer SMS pour cette flotte" → appel API LemonSqueezy pour ajouter/retirer add-on
- **API Verbalisation ANTAI** : 1€/véhicule/mois (P1 - post-MVP)
  - Activation self-service identique à SMS

**Gestion TVA automatique** :
- LemonSqueezy gère automatiquement la TVA française (20%) et européenne (reverse charge B2B)
- Facturation conforme pour clients professionnels (numéro SIRET requis à l'inscription)

**Essai gratuit 14 jours avec CB obligatoire** :
- Carte bancaire requise à l'inscription (améliore conversion bêta → payant de 30% à 70%)
- Aucun débit pendant 14 jours
- Auto-renouvellement à J-14 avec email de rappel à J-7
- Politique d'annulation : annulation self-service jusqu'à la fin de la période d'essai

**Facturation récurrente mensuelle** :
- Calcul fin de mois : comptage véhicules actifs au dernier jour du mois
- Ajout/suppression véhicule en cours de mois → pris en compte facture suivante (pas de prorata pour MVP)
- Historique factures téléchargeable en PDF depuis dashboard Admin (requis pour comptabilité)

### Integration Strategy

**Pour MVP (P0)** :
- ✅ **Pas d'intégrations tierces prioritaires** (focus sur core product)
- ✅ LemonSqueezy (paiement) et OVH SMS (alertes) sont les seules dépendances externes

**Post-MVP (P1)** :
- API ANTAI (verbalisations automatiques)
- Webhooks sortants pour événements FlotteBox (document expiré, alerte déclenchée)
- Export API JSON/XML pour intégration ERP clients

**Vision (P2)** :
- API publique REST documentée (OpenAPI/Swagger)
- OAuth2 pour authentification tiers
- Intégrations assureurs (import automatique attestations)
- Intégrations garagistes (rappels entretien, import factures)
- SDK JavaScript/Python pour développeurs tiers

### Compliance & Security Requirements

**RGPD & Hébergement France (P0 - MVP)** :
- ✅ Hébergement base de données PostgreSQL en France (Scaleway ou OVH Cloud)
- ✅ Hébergement fichiers (documents scannés) en France (Scaleway Object Storage ou OVH Object Storage)
- ✅ Conformité RGPD dès le MVP (obligation légale)
- ✅ Politique de confidentialité et CGU affichées avant inscription
- ✅ Consentement explicite pour traitement données personnelles (chauffeurs, gestionnaires)
- ✅ Droit d'accès, rectification, suppression (RGPD Article 15-17) : interface self-service Admin

**Audit Logs & Traçabilité (P1 - post-MVP)** :
- Table `audit_logs` pour traçabilité complète :
  - Qui a consulté quel document, quand (requis pour audits URSSAF)
  - Qui a modifié quel véhicule/document, quand
  - Qui a ajouté/supprimé des utilisateurs
  - Qui a exporté des registres de conformité
- Retention audit logs : 3 ans minimum (conformité URSSAF)
- Interface de consultation audit logs pour Admin (filtres par utilisateur, date, action)

**2FA - Two-Factor Authentication (P1 - post-MVP)** :
- **Obligatoire pour comptes Admin** (protection abonnement et données sensibles)
- **Optionnel pour Gestionnaire** (recommandé mais pas forcé)
- **Non disponible pour Chauffeur** (UX mobile simplifiée, pas critique sécurité)
- Méthode 2FA : TOTP (Time-based One-Time Password) via app authenticator (Google Authenticator, Authy)
- Gestion "appareils de confiance" : option "Se souvenir de cet appareil pendant 30 jours" pour éviter 2FA à chaque connexion
- Recovery codes générés à l'activation 2FA (10 codes à usage unique en cas de perte téléphone)

**Conservation légale 10 ans (P0 - MVP)** :
- Documents réglementaires (cartes grises, assurances, CT, permis) : conservation obligatoire 10 ans
- Soft delete : documents marqués `deleted_at` mais jamais supprimés physiquement de la BDD/storage
- Interface Admin : "Archiver document" au lieu de "Supprimer" (masqué dans l'interface, conservé en base)
- Coût storage Scaleway/OVH : ~0,01€/GB/mois → impact marginal (10 000 documents scannés ≈ 5GB ≈ 0,05€/mois)

**Certifications futures (évaluation en bêta)** :
- **ISO 27001** : Si >50% bêta-testeurs l'exigent → investir 15-25k€ à M3-M6
- **SOC 2 Type II** : Pour grands comptes exigeants (M12-M24)
- Audits sécurité annuels (P2)

**SSO - Single Sign-On (P2 - Vision)** :
- SAML 2.0 / OAuth2 pour grands comptes (>100 véhicules)
- Intégration Azure AD, Google Workspace, Okta
- Plan Enterprise sur-devis avec SSO inclus

### Technical Architecture Considerations

**Stack technique validée** :
- Frontend : Next.js 16 + React 19 + TypeScript
- Backend : Next.js API Routes (serverless) ou Node.js/Express si besoin serveur dédié
- Base de données : PostgreSQL géré (Scaleway Database ou OVH Cloud Databases)
- ORM : Prisma ou Drizzle (abstraction DB pour migration facile si besoin)
- Storage : Scaleway Object Storage ou OVH Object Storage (S3-compatible)
- Auth : NextAuth.js avec support multi-tenant (session stockée avec `company_id`)
- Paiements : LemonSqueezy (usage-based billing, TVA automatique)
- OCR : Mistral OCR 3 API ($2/1000 pages)
- Emails : Resend
- SMS : OVH SMS (0,035€/SMS)
- Hosting : Vercel (frontend Next.js) + Scaleway/OVH (backend API si besoin)

**Multi-tenancy PostgreSQL implementation** :
- Approche Row-Level Security (RLS) avec colonne `company_id` sur toutes les tables
- Index composite sur `(company_id, id)` pour performances
- Middleware Next.js : extraction `company_id` depuis session utilisateur → injection automatique dans requêtes
- Prévention fuite données : toutes les requêtes Prisma/Drizzle incluent `WHERE company_id = ?`

**Scalabilité cible MVP** :
- Support 100 clients actifs simultanés (objectif M12 : 40-50 clients)
- Architecture serverless Vercel : scaling automatique selon charge
- PostgreSQL géré : vertical scaling jusqu'à 100 clients (puis migration cluster si >100)
- Monitoring proactif : alertes si temps réponse API > 2s ou charge DB > 70%

## Functional Requirements

### 1. User Management & Authentication

- **FR1**: Users can create an account with email/password or OAuth Google
- **FR2**: Companies can require credit card during 14-day free trial registration
- **FR3**: Admins can create, modify, and delete user accounts (Admin, Gestionnaire, Chauffeur, Tiers roles)
- **FR4**: Admins can assign roles and permissions to users within their company
- **FR5**: Admins can configure 2FA (Two-Factor Authentication) for their account with trusted device management
- **FR6**: Companies can grant read-only access to external third parties (comptables, auditeurs) with granular permissions
- **FR7**: Comptables can access multiple client companies with a single account

### 2. Fleet & Vehicle Management

- **FR8**: Gestionnaires can add vehicles with distinction between motors (camions, VUL, voitures, engins) and trailers (remorques)
- **FR9**: Gestionnaires can import vehicles in bulk via CSV file with duplicate detection
- **FR10**: Gestionnaires can assign chauffeurs to specific vehicles
- **FR11**: Gestionnaires can edit and delete vehicles
- **FR12**: Chauffeurs can view vehicle information and documents for vehicles assigned to them
- **FR13**: System can automatically count active motors and trailers per company for billing

### 3. Document Management & OCR

- **FR14**: Gestionnaires can upload documents via drag-and-drop (desktop) or camera scan (mobile PWA)
- **FR15**: Chauffeurs can scan documents using mobile PWA in less than 30 seconds
- **FR16**: System can perform OCR-assisted extraction to pre-fill document fields (immatriculation, dates, type) with >90% precision
- **FR17**: Users must validate OCR-extracted data before document creation (mandatory human validation)
- **FR18**: System can classify document types automatically (carte grise, assurance, CT, permis, visite médicale, FIMO)
- **FR19**: System can mark illegible documents as "à vérifier" and notify gestionnaire
- **FR20**: Chauffeurs can scan documents offline and system syncs automatically when network returns
- **FR21**: System can archive documents (soft delete) instead of permanent deletion for 10-year legal retention
- **FR22**: Gestionnaires can import documents in bulk via ZIP with OCR matching to vehicles

### 4. Alerts & Notifications

- **FR23**: System can calculate expiration deadlines automatically and trigger alerts at 60 days, 30 days, 15 days, and expiration
- **FR24**: Users can receive email notifications (daily/weekly summaries) for upcoming expirations
- **FR25**: Users can receive push notifications via PWA for critical alerts
- **FR26**: Companies can activate SMS alerts add-on (0,50€/vehicle/month) with self-service toggle
- **FR27**: Gestionnaires can customize alert frequency and types per vehicle
- **FR28**: System can send automatic reminders to gestionnaires if chauffeur hasn't scanned in 30 days

### 5. Dashboard & Reporting

- **FR29**: Gestionnaires can view real-time fleet compliance dashboard with visual status (vert/orange/rouge)
- **FR30**: Dirigeants can view upcoming deadlines calendar for next 30 days
- **FR31**: Gestionnaires can view chauffeur adoption metrics (% having scanned ≥1 document/month)
- **FR32**: Gestionnaires can export "Registre de conformité conducteurs" (permis, visites médicales, FIMO) as PDF for URSSAF audits
- **FR33**: Gestionnaires can export "Registre de conformité véhicules" (cartes grises, assurances, CT) as PDF/Excel
- **FR34**: Users can select custom date periods for compliance exports (e.g., last 3 years)
- **FR35**: Tiers (comptables) can export compliance registers for their client companies

### 6. Onboarding & Driver Adoption

- **FR36**: Gestionnaires can generate QR code for PWA installation to share with chauffeurs
- **FR37**: Chauffeurs can install PWA in under 1 minute by scanning QR code
- **FR38**: System can send 90-second tutorial video via SMS to new chauffeurs
- **FR39**: System can guide chauffeurs through first scan with tooltips and step-by-step validation
- **FR40**: System can activate gamification (badges, leaderboard) automatically if company >10 users OR >5 scans/month
- **FR41**: Chauffeurs can earn "Chauffeur exemplaire" badge for consistent document scanning

### 7. Billing & Subscription Management

- **FR42**: System can calculate monthly billing using usage-based pricing (motors × tier rate + trailers × 1,50€)
- **FR43**: System can apply tiered pricing for motors (1-25: 4€, 26-100: 3€, 101+: 2,50€)
- **FR44**: System can charge OCR add-on at 0,10€/document scanned (pay-as-you-go)
- **FR45**: System can handle TVA automatically for French and European B2B invoicing
- **FR46**: Admins can activate/deactivate add-ons (SMS, API ANTAI) via self-service dashboard
- **FR47**: Admins can download monthly invoices as PDF from billing portal
- **FR48**: System can auto-renew subscriptions with email reminder at J-7 before charge
- **FR49**: Admins can cancel subscription self-service before end of free trial

### 8. Compliance & Security

- **FR50**: System can isolate data by company (multi-tenant) with no cross-tenant data leakage
- **FR51**: System can log all access and modifications to documents for audit trail (P1)
- **FR52**: Admins can view audit logs filtered by user, date, and action (P1)
- **FR53**: System can retain audit logs for minimum 3 years for URSSAF compliance (P1)
- **FR54**: System can enforce RGPD data privacy with user consent management
- **FR55**: Users can exercise RGPD rights (access, rectification, deletion) via self-service interface

### 9. Super Admin & Analytics

- **FR56**: Super Admin (Quentin) can view global analytics dashboard (MRR, churn, ARPU, NPS)
- **FR57**: Super Admin can identify at-risk clients with health score <40/100
- **FR58**: Super Admin can view feature adoption metrics (OCR usage, SMS add-on, etc.)
- **FR59**: Super Admin can activate demo mode to generate realistic fake company data for sales presentations
- **FR60**: Super Admin can track funnel activation and user engagement events
