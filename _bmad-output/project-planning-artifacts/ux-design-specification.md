---
stepsCompleted: [1, 2, 3, 4, 5]
inputDocuments:
  - _bmad-output/prd.md
workflowType: 'ux-design'
lastStep: 5
project_name: 'FlotteBox'
user_name: 'Quentin'
date: '2026-01-07'
---

# UX Design Specification FlotteBox

**Author:** Quentin
**Date:** 2026-01-07

---

## Executive Summary

### Project Vision

FlotteBox est une application SaaS B2B mobile-first + web qui transforme la gestion documentaire des flottes de véhicules (10-200 véhicules) d'un processus manuel chronophage en une expérience automatisée ultra-simple. La promesse centrale : scan photo → OCR pré-remplit le formulaire → validation humaine → document classé + alertes configurées. Le tout en moins de 30 secondes depuis un mobile.

### Target Users

**Persona 1 : Marie - Assistante administrative (40-55 ans)**
- Gère la conformité administrative, perd actuellement 2-3h/semaine sur la gestion manuelle
- Tech-savvy : Moyen (utilise Excel, emails confortablement)
- Device principal : Desktop
- Besoins UX : Vision centralisée, gains de temps immédiat, interface claire et efficace

**Persona 2 : Karim - Chauffeur (50-60 ans)** 🎯 **PERSONA CRITIQUE POUR ADOPTION**
- Peu tech-savvy, utilise smartphone Android 7-8 (courant dans ce segment)
- Device : Mobile uniquement (terrain, souvent en zone blanche)
- Besoins UX : Simplicité extrême (< 3 clics), pas de formation complexe, interface évidente
- Success criteria : >60% des chauffeurs utilisent l'app régulièrement

**Persona 3 : Philippe - Dirigeant PME (45-60 ans)**
- Veut éviter amendes (135€ à 3 750€), avoir vision conformité temps réel, ROI immédiat
- Tech-savvy : Moyen
- Device : Desktop/tablet
- Besoins UX : Dashboard synthétique, rapports pour audits, sérénité réglementaire

### Key Design Challenges

🚨 **Challenge #1 : Adoption chauffeurs (critique pour succès produit)**
- Si <60% des chauffeurs utilisent l'app mobile → gestionnaires scannent tout eux-mêmes → FlotteBox devient "juste un autre Excel" → échec produit
- Population 50-60 ans, peu tech-savvy, smartphones anciens (Android 7-8)
- Solutions UX obligatoires :
  - Onboarding in-app avec tutoriel vidéo 90 sec à la première connexion (rejouable)
  - QR code installation PWA ultra-simple depuis dashboard gestionnaire
  - Premier scan guidé avec tooltips interactifs et validation étape par étape
  - Gamification contextuelle (badge "Chauffeur exemplaire", classement) si flotte >10 utilisateurs
  - Support technique Android 7-8 et optimisation performances

⚡ **Challenge #2 : OCR assisté fiable mais humain**
- Workflow : scan → pré-remplissage automatique → validation humaine obligatoire → enregistrement
- Précision requise >90% sur champs critiques (immatriculations, dates)
- Gestion erreurs : documents illisibles marqués "à vérifier" + notification gestionnaire
- UX doit montrer clairement que c'est une "assistance" pas une "automatisation magique"

📱 **Challenge #3 : Mode offline mobile essentiel**
- Chauffeurs terrain souvent en zones blanches (chantiers, campagne)
- Scan sans connexion + sync automatique au retour de réseau
- Feedback visuel clair sur statut "offline" et "en cours de sync"

🎯 **Challenge #4 : Distinction moteurs vs remorques**
- Pricing différencié (4€ moteurs, 1,50€ remorques)
- UX doit clairement distinguer les deux types (visuellement, fonctionnellement)
- Remorques ont moins de documents (pas de CT annuel, FIMO, etc.)

### Design Opportunities

✨ **Opportunité #1 : Le moment "aha!" - Premier scan OCR**
- Expérience du premier scan qui pré-remplit tout automatiquement = révélation de la valeur
- Doit être spectaculaire, fluide, gratifiant
- Feedback immédiat : "Vous venez d'économiser 8 minutes de saisie manuelle !"

💎 **Opportunité #2 : 10× plus simple que la concurrence**
- Face aux TMS lourds (plusieurs jours de formation) → prise en main <30 min
- Interface pensée pour non-techniciens dès la conception
- Chaque écran doit répondre à : "Un chauffeur de 55 ans peut-il l'utiliser sans aide ?"

🎮 **Opportunité #3 : Gamification intelligente pour adoption**
- Badge "Chauffeur exemplaire" après premier scan
- Classement par taux d'adoption (si flotte >10 utilisateurs)
- Dashboard gestionnaire : vue temps réel de l'adoption chauffeurs
- Rappels automatiques non-intrusifs si chauffeur inactif >30 jours

📊 **Opportunité #4 : Exports pensés pour experts-comptables**
- Format "registre de conformité" (tous véhicules, tous docs, toutes dates)
- Accès en lecture seule pour tiers (comptables, auditeurs)
- Traçabilité complète pour audits URSSAF
- UX professionnelle qui rassure sur la conformité légale

## Core User Experience

### Defining Experience

**L'action centrale de FlotteBox : Scanner et valider un document en <30 secondes**

Cette action peut être réalisée par **deux types d'utilisateurs** selon le modèle d'adoption de l'entreprise :

1. **Chauffeur (mobile terrain)** : Scan photo → OCR pré-remplit → Validation → Document classé
2. **Gestionnaire (mobile ou desktop)** :
   - Mobile : Scan photo direct (identique chauffeur)
   - Desktop : Upload fichier PC → OCR pré-remplit → Validation → Document classé

**Réalité terrain acceptée :** Certaines entreprises préféreront que le gestionnaire scanne tout lui-même plutôt que former les chauffeurs. FlotteBox doit soutenir les deux modèles d'adoption :
- **Adoption optimale** : Chauffeurs scannent (>60% cible) → gestionnaire supervise
- **Adoption pragmatique** : Gestionnaire scanne tout → gain de temps sur saisie manuelle vs Excel

**L'action secondaire critique : Consultation dashboard conformité**
- Moins fréquente une fois la confiance établie (les alertes automatiques prennent le relais)
- Essentielle pour la vue d'ensemble temps réel et les audits URSSAF

### Platform Strategy

**Architecture PWA (Progressive Web App) :**
- **Pas d'app store** (iOS/Android) → Installation directe via navigateur
- **Responsive multi-device** :
  - Mobile-first pour chauffeurs (terrain, zones blanches)
  - Desktop optimisé pour gestionnaires (dashboard, bulk upload)
  - Tablet supporté pour dirigeants (consultation nomade)

**Capacités platform requises :**
- **Caméra native** : Scan documents haute qualité via API Web
- **Mode offline-first** : Service Workers + IndexedDB pour scan sans connexion
- **Notifications push** : API Push Notifications pour alertes critiques
- **Installation PWA** : Manifest + Add to Home Screen prompt
- **Performance Android 7-8** : Support smartphones anciens (optimisation critique)

**Touch-first sur mobile, efficace sur desktop :**
- Grandes zones tactiles (min 48×48px) pour chauffeurs
- Keyboard shortcuts et bulk actions pour gestionnaires desktop

### Effortless Interactions

**Ce qui doit être complètement sans friction :**

**1. Onboarding chauffeur sans email (30 sec top chrono)**
- Gestionnaire crée compte → Identifiant custom + password générés
- SMS automatique avec : "Votre identifiant : KARIM2024, mot de passe : ********, lien PWA : https://..."
- Chauffeur : Clic lien → Install PWA → Login (identifiant + password) → Tutoriel vidéo 90 sec → Premier scan guidé

**2. Scan document ultra-rapide (<30 sec)**
- Ouvrir app → Bouton "Scanner un document" (évident, central)
- Photo automatique → OCR pré-remplit instantanément
- Validation 2-3 champs max → Enregistré ✅
- Feedback gratifiant : "Document enregistré ! Vous avez économisé 8 min de saisie"

**3. Mode offline transparent**
- Indicateur discret mais clair : pastille "Hors ligne" en haut
- Scan fonctionne normalement → badge "En attente de sync" sur le document
- Retour réseau → sync silencieuse + notification "3 documents synchronisés ✅"

**4. Association véhicule automatique**
- Chauffeur habituel d'un véhicule → ses scans s'associent auto au bon véhicule
- Détection intelligente : Si chauffeur assigné à 1 seul véhicule → pas de sélection manuelle
- Si multiple véhicules → dropdown pré-sélectionné sur dernier utilisé

**5. Distinction moteur/remorque visuelle évidente**
- **Icônes différentes** : 🚛 Moteur vs 🚚 Remorque
- **Codes couleur** : Bleu pour moteurs, Gris pour remorques
- **Badge différencié** : "Moteur - 4€/mois" vs "Remorque - 1,50€/mois"
- Liste documents adaptée : Remorques cachent automatiquement CT, FIMO, permis

**6. Upload desktop gestionnaire fluide**
- Drag & drop zone évidente
- Upload multiple (10 fichiers simultanés)
- OCR batch → validation en file d'attente
- Progress bar temps réel

**7. Configuration des alertes par email flexible**
- Admin/Gestionnaire configure 3 emails distincts :
  - **Email alertes opérationnelles** : Toutes alertes échéances (J-60, J-30, J-15, expiration), documents à vérifier, adoption chauffeurs
  - **Email facturation** : Factures, renouvellements, modifications plan
  - **Email rapports** : Synthèses hebdo/mensuelles, rapports conformité
- Configurable dès l'onboarding (étape 2 après création compte)
- Email de test envoyé après configuration : "Test d'alerte - Configuration réussie ✅"
- Par défaut = email du compte admin si non configuré

### Critical Success Moments

**Les moments make-or-break de l'expérience :**

**1. Premier scan OCR réussi (chauffeur ou gestionnaire)** ⭐ **LE moment "aha!"**
- Scan photo → OCR pré-remplit TOUT instantanément (immat, date, type doc)
- Feedback spectaculaire : Animation de remplissage + "✨ Vous venez d'économiser 8 min de saisie !"
- Badge débloqué immédiatement (gamification instant gratification)

**2. Première connexion chauffeur réussie (sans email)**
- SMS reçu → Clic lien → Install PWA → Login identifiant custom → **Ça marche !**
- Tutoriel vidéo 90 sec clair et rassurant
- Premier scan guidé avec tooltips : succès garanti

**3. Import CSV initial 50 véhicules (<2h)**
- Template fourni, pré-rempli avec exemples
- Détection doublons intelligente
- Validation interactive : "3 doublons trouvés, voulez-vous les fusionner ?"
- Succès = "50 véhicules importés ✅ Prêt à scanner vos documents"

**4. Premier coup d'œil dashboard conformité**
- Vision instantanée : "32 véhicules OK ✅, 12 à surveiller ⚠️, 6 critiques 🚨"
- Prochaines échéances visibles en 1 coup d'œil
- Sentiment : "Enfin je vois tout d'un coup !"

**5. Première alerte échéance évitée**
- Email/push 30j avant expiration CT : "Le CT du Renault Master AA-123-BB expire le 15 février"
- Dirigeant prend RDV garage → amende évitée
- ROI tangible : "FlotteBox vient de vous faire économiser 135€ d'amende"

**6. Premier document illisible géré intelligemment**
- OCR échoue sur document flou → système marque "à vérifier" automatiquement
- Notification gestionnaire : "1 document nécessite votre attention"
- Validation manuelle → confiance dans le système : "L'OCR ne laisse rien passer, je peux lui faire confiance"

**7. Configuration alertes email sans spam dirigeant**
- Philippe (dirigeant) crée le compte avec sa CB
- Configure email alertes vers Marie (gestionnaire) → "marie@entreprise.fr recevra toutes les alertes"
- Email de test reçu par Marie → "Je suis bien configurée !"
- Philippe reçoit uniquement facturation → pas de spam quotidien

### Experience Principles

**Principes directeurs pour toutes les décisions UX :**

**1. "Un chauffeur de 55 ans doit réussir seul"**
- Chaque écran, chaque interaction testée avec cette question
- Grandes cibles tactiles, texte lisible (min 16px), contraste élevé
- Pas de jargon technique, langage terrain ("Scanner" pas "Uploader")

**2. "L'OCR accélère, l'humain valide"**
- Jamais de création automatique sans validation humaine
- UI montre clairement : "Pré-rempli par OCR → Vérifiez SVP"
- Confiance par la transparence, pas par la magie noire

**3. "Le mobile ne doit jamais demander si le réseau est là"**
- Offline-first par défaut
- UI indique l'état mais ne bloque jamais l'action
- Sync silencieuse et intelligente

**4. "Moins de clics = plus d'adoption"**
- Règle des 3 clics maximum pour scanner un document
- Associations automatiques (véhicule, type doc) dès que possible
- Shortcuts intelligents basés sur l'usage

**5. "Flexibilité d'adoption : chauffeurs OU gestionnaires"**
- Pas de modèle d'adoption imposé
- Workflow identique mobile chauffeur vs mobile gestionnaire
- Desktop gestionnaire optimisé pour bulk operations

**6. "Gratification immédiate pour renforcer les bons comportements"**
- Feedback instantané après chaque action réussie
- Gamification contextuelle (pas spam de badges inutiles)
- Métriques d'adoption visibles pour gestionnaires

**7. "Notifications intelligentes, pas spam"**
- Séparation stricte : alertes opérationnelles vs facturation vs rapports
- Configuration flexible dès l'onboarding
- Email de test systématique pour valider la configuration

## Desired Emotional Response

### Primary Emotional Goals

**Par persona, les émotions primaires recherchées :**

**Marie (Gestionnaire administrative) :**
- **Soulagement** : "Enfin je ne suis plus submergée par la paperasse"
- **Contrôle** : "Je maîtrise la situation, je vois tout d'un coup d'œil"
- **Efficacité** : "Je gagne 2-3h par semaine, je peux me concentrer sur des tâches à plus forte valeur"

**Karim (Chauffeur) :**
- **Confiance** : "J'y arrive, ce n'est pas compliqué"
- **Fierté** : "Je contribue à l'entreprise, je suis un chauffeur exemplaire"
- **Autonomie** : "Je ne dépends pas du bureau pour scanner mes documents"

**Philippe (Dirigeant) :**
- **Sérénité** : "Je dors tranquille, plus de risque d'amendes oubliées"
- **Efficacité organisationnelle** : "Mon équipe est plus productive"
- **ROI tangible** : "J'ai évité des amendes, l'investissement est rentabilisé"

**Émotion transversale recherchée :**
**"C'est tellement simple que même mes chauffeurs l'utilisent !"** → Sentiment de **surprise positive** + **accomplissement** ("J'ai réussi là où d'autres solutions ont échoué")

### Emotional Journey Mapping

**Phase 1 : Découverte initiale (site web / démo)**
- Émotion cible : **Espoir** ("Enfin une solution qui comprend mes problèmes réels")
- Éviter : Scepticisme ("Encore un logiciel compliqué qui promet la lune...")

**Phase 2 : Première connexion / Onboarding**
- Émotion cible : **Confiance** ("L'interface est claire, je comprends où aller")
- Éviter : Anxiété ("Je ne sais pas par où commencer, c'est trop compliqué")

**Phase 3 : Premier scan OCR (moment "aha!")**
- Émotion cible : **Émerveillement** + **Soulagement** ("Wow, ça pré-remplit tout ! Je viens de gagner 8 minutes !")
- Éviter : Frustration ("L'OCR s'est trompé et a créé une erreur dans mes données")

**Phase 4 : Utilisation quotidienne**
- Émotion cible : **Fluidité** + **Efficacité** ("C'est devenu un réflexe, je ne réfléchis plus")
- Éviter : Friction ("Encore des clics inutiles, ça prend autant de temps qu'Excel")

**Phase 5 : Retour après plusieurs semaines**
- Émotion cible : **Gratitude** ("FlotteBox m'a alerté 30j avant, j'ai évité une amende de 135€")
- Éviter : Indifférence ("Je ne vois plus la valeur, je pourrais revenir à Excel")

**Phase 6 : En cas d'erreur / problème**
- Émotion cible : **Confiance maintenue** ("Le système me prévient clairement, je comprends ce qui s'est passé et comment corriger")
- Éviter : Panique ("J'ai perdu des données ! Je ne comprends pas ce qui s'est passé !")

### Micro-Emotions

**1. Confiance vs. Scepticisme** ⭐ **CRITIQUE**
- **Enjeu** : OCR assisté + validation humaine obligatoire → les utilisateurs doivent faire confiance au système sans craindre les erreurs silencieuses
- **Leviers UX** :
  - Transparence totale ("Pré-rempli par OCR, vérifiez SVP")
  - Gestion erreurs visible (document marqué "à vérifier")
  - Premier document illisible géré intelligemment → renforce confiance

**2. Accomplissement vs. Frustration** ⭐ **CRITIQUE**
- **Enjeu** : Premier scan chauffeur = moment make-or-break pour adoption
- **Leviers UX** :
  - Feedback immédiat gratifiant ("Vous avez économisé 8 min !")
  - Badge "Chauffeur exemplaire" instantané
  - Gamification contextuelle pour renforcement positif

**3. Maîtrise vs. Dépassement** (chauffeurs 50-60 ans)
- **Enjeu** : Si adoption chauffeurs <60% → échec produit
- **Leviers UX** :
  - Interface évidente, grandes cibles tactiles (48×48px min)
  - Langage simple, terrain ("Scanner" pas "Uploader")
  - Tutoriel vidéo 90 sec rassurant
  - Premier scan guidé avec tooltips → succès garanti

**4. Sérénité vs. Anxiété** (dirigeants)
- **Enjeu** : ROI émotionnel = "Je dors tranquille sans craindre les amendes"
- **Leviers UX** :
  - Dashboard statut clair avec codes couleur (vert/orange/rouge)
  - Alertes proactives multi-canal (email, push, SMS optionnel)
  - Aucune surprise : tout est anticipé et notifié à temps

**5. Efficacité vs. Perte de temps**
- **Enjeu** : Gain de temps = promesse centrale (2-3h/semaine économisées)
- **Leviers UX** :
  - Scan <30 secondes end-to-end
  - Associations automatiques (véhicule, type document)
  - Mode offline-first : jamais bloqué par le réseau
  - Bulk operations desktop pour gestionnaires

### Design Implications

**Pour créer la confiance :**
- Validation humaine obligatoire après OCR (pas de magie noire)
- Messages clairs sur l'état du système ("Pré-rempli par OCR → Vérifiez")
- Gestion erreurs proactive (document illisible marqué automatiquement)
- Email de test après configuration alertes ("Tout fonctionne ✅")

**Pour créer l'accomplissement :**
- Feedback immédiat et spectaculaire après premier scan
- Quantification du gain ("Vous avez économisé 8 min de saisie !")
- Gamification contextuelle (badges, classement si flotte >10 users)
- Dashboard adoption visible pour gestionnaires

**Pour créer la maîtrise (chauffeurs) :**
- Règle des 3 clics maximum pour action principale
- Tutoriel vidéo 90 sec à la première connexion
- Premier scan guidé avec tooltips interactifs
- Langage terrain, pas jargon technique
- Support Android 7-8 avec performances optimisées

**Pour créer la sérénité (dirigeants) :**
- Dashboard conformité temps réel avec statuts visuels évidents
- Alertes multi-niveaux (J-60, J-30, J-15, expiration)
- Notifications intelligentes sans spam (emails séparés)
- Exports "registre de conformité" pour audits URSSAF

**Pour créer l'efficacité :**
- Mode offline-first : scan sans connexion, sync silencieuse
- Associations automatiques intelligentes (véhicule habituel, type doc)
- Bulk upload desktop avec OCR batch
- Keyboard shortcuts pour power users

### Emotional Design Principles

**1. "Rassurez en montrant, pas en cachant"**
- Transparence sur ce que fait l'OCR
- Erreurs clairement expliquées avec solutions
- État du système toujours visible (offline, sync en cours)

**2. "Chaque succès doit être célébré immédiatement"**
- Feedback instantané après chaque action réussie
- Gamification pour renforcer comportements positifs
- Métriques visibles (temps économisé, documents scannés)

**3. "Ne jamais bloquer, toujours guider"**
- Mode offline fonctionnel (scan sans réseau)
- Tooltips et guidage contextuel au lieu d'erreurs bloquantes
- Suggestions intelligentes plutôt qu'obligations

**4. "La simplicité crée la confiance"**
- Interface évidente pour non-techniciens
- Langage simple et terrain
- Chaque écran répond à : "Un chauffeur de 55 ans peut-il réussir seul ?"

**5. "L'utilisateur doit toujours savoir où il en est"**
- Indicateurs d'état clairs (offline, sync, validation requise)
- Progress bars pour opérations longues (import CSV, upload batch)
- Confirmations visuelles après chaque action

**6. "La gratification doit être proportionnelle à l'effort"**
- Premier scan = grosse célébration (animation, badge, feedback détaillé)
- Scans suivants = confirmation simple et rapide
- Gamification activée uniquement si pertinent (>10 users)

## UX Pattern Analysis & Inspiration

### Inspiring Products Analysis

**1. WhatsApp - Référence pour simplicité & adoption universelle**

**Ce qu'ils font brillamment :**
- Onboarding invisible : Installation → numéro de téléphone → ça marche (pas d'email, pas de compte complexe)
- Envoi photo ultra-rapide : Ouvrir app → caméra → photo → envoi (3 taps exactement)
- Interface évidente : Grandes icônes, contraste élevé, langage simple
- Feedback visuel clair : Envoyé (✓), Reçu (✓✓), Lu (✓✓ bleu)
- Mode offline transparent : Messages en attente, sync automatique au retour réseau
- Adopté par tous âges : Même grand-parents l'utilisent

**Pertinence pour FlotteBox :**
- Même challenge : faire adopter par utilisateurs 50-60 ans peu tech-savvy
- Même workflow : scan photo → envoi rapide
- Même besoin : offline-first pour zones blanches

**2. Google Maps - Référence pour clarté visuelle & statuts**

**Ce qu'ils font brillamment :**
- Statuts visuels évidents : Codes couleur route (vert OK, orange ralentissement, rouge bloqué)
- Gros boutons tactiles : "Démarrer", "Itinéraire" → impossibles à rater
- Mode offline : Cartes téléchargées, fonctionne sans réseau
- Indicateurs d'état clairs : "Hors ligne", "Recherche GPS", "Calcul itinéraire"
- Navigation simplifiée : Minimal, focus sur l'action principale

**Pertinence pour FlotteBox :**
- Dashboard conformité = même besoin de statuts visuels évidents (vert/orange/rouge)
- Offline-first : même contrainte terrain
- Interface mobile épurée : même cible utilisateur

**3. Banking Apps (Boursorama, Crédit Agricole) - Référence pour alertes & sécurité**

**Ce qu'ils font brillamment :**
- Dashboard synthétique : Vue d'ensemble immédiate (soldes, mouvements récents)
- Alertes intelligentes : Seuils configurables, pas de spam
- Notifications multi-canal : Push + email + SMS (optionnel)
- Sécurité visible : Chaque action critique confirmée, traçabilité
- Confiance par transparence : État des opérations toujours visible

**Pertinence pour FlotteBox :**
- Dashboard dirigeant = même besoin de vue synthétique
- Alertes échéances = même logique de notifications intelligentes
- Confiance critique : documents sensibles (comme argent) → même exigence transparence

**4. Doctolib - Référence pour onboarding grand public**

**Ce qu'ils font brillamment :**
- Onboarding progressif : Pas tout demander d'un coup, juste le nécessaire
- Rappels RDV bien dosés : J-7, J-1, sans spam
- Interface accessible : Utilisée par tous âges (20-80 ans)
- SMS intelligents : Confirmation RDV, lien direct, rappels
- Tutoriels contextuels : Aide au bon moment, pas envahissante

**Pertinence pour FlotteBox :**
- Population similaire : 40-60 ans, pas forcément tech-savvy
- Alertes RDV = alertes échéances FlotteBox
- SMS onboarding : même stratégie pour chauffeurs

### Transferable UX Patterns

**Navigation Patterns :**

**Pattern "Bottom Navigation" (WhatsApp, Google Maps mobile)**
- Pour FlotteBox mobile chauffeur : 3 tabs max
  - 📸 Scanner (action principale, central)
  - 🚛 Mes Véhicules
  - 👤 Profil
- Justification : Accessible au pouce, évident, pas de menu hamburger caché

**Pattern "Dashboard avec Statuts Visuels" (Google Maps, Banking apps)**
- Pour FlotteBox desktop gestionnaire : Cards avec codes couleur
  - Vert ✅ : Véhicules conformes
  - Orange ⚠️ : À surveiller (échéance <30j)
  - Rouge 🚨 : Critique (expiré ou <15j)
- Justification : Compréhension immédiate, pas de lecture nécessaire

**Interaction Patterns :**

**Pattern "Envoi photo rapide" (WhatsApp)**
- Pour FlotteBox scan document :
  - Bouton caméra central, gros, impossible à rater
  - Capture auto avec retour haptique
  - Preview immédiat avec "Utiliser" ou "Refaire"
- Justification : Workflow éprouvé, adopté par millions d'utilisateurs

**Pattern "Feedback double coché" (WhatsApp)**
- Pour FlotteBox statut document :
  - Document scanné ✓ (gris)
  - OCR extrait ✓✓ (orange)
  - Validé humain ✓✓✓ (vert)
- Justification : Progression visible, rassurante

**Pattern "Mode offline transparent" (Google Maps, WhatsApp)**
- Pour FlotteBox :
  - Pastille discrète "Hors ligne" en haut
  - Actions fonctionnent normalement
  - Badge "En attente de sync" sur éléments
  - Notification "X documents synchronisés ✅" au retour réseau
- Justification : Ne jamais bloquer l'utilisateur

**Visual Patterns :**

**Pattern "Codes couleur sémantiques" (Google Maps, Banking apps)**
- Pour FlotteBox :
  - Vert = OK, conforme
  - Orange = Attention, surveiller
  - Rouge = Urgent, action requise
  - Bleu = Informationnel
- Justification : Compréhension universelle, pas de barrière linguistique

**Pattern "Grandes cibles tactiles" (WhatsApp, Google Maps)**
- Pour FlotteBox mobile :
  - Boutons min 48×48px (recommandation Apple/Google)
  - Espacement min 8px entre éléments tactiles
  - Zones de tap étendues (pas juste l'icône visible)
- Justification : Utilisateurs 50-60 ans, doigts moins précis

**Pattern "Animation de progression" (Banking apps transfers)**
- Pour FlotteBox OCR :
  - Photo → Animation extraction données → Pré-remplissage progressif
  - Feedback : "Analyse en cours... Immatriculation détectée ✓ Date détectée ✓"
- Justification : Patience utilisateur, spectaculaire, crée émerveillement

### Anti-Patterns to Avoid

**❌ Anti-Pattern #1 : "Menu hamburger caché" (apps complexes)**
- Problème : Utilisateurs 50-60 ans ne trouvent pas le menu caché derrière ☰
- Pour FlotteBox : Navigation visible, bottom tabs, pas de menu caché
- Source : Études montrent que les utilisateurs seniors ignorent le hamburger menu

**❌ Anti-Pattern #2 : "Onboarding multi-pages intimidant" (apps bancaires anciennes)**
- Problème : 10 écrans de setup → abandon 70%
- Pour FlotteBox chauffeur : Onboarding minimal, progressif, juste le nécessaire
- Source : Taux d'abandon élevé sur onboarding >3 écrans

**❌ Anti-Pattern #3 : "Notifications spam" (apps e-commerce agressives)**
- Problème : 10 notifications/jour → désactivation totale → perte de valeur
- Pour FlotteBox : Alertes intelligentes, configurables, emails séparés (opérationnel vs facturation)
- Source : 60% des utilisateurs désactivent les notifications si trop fréquentes

**❌ Anti-Pattern #4 : "Jargon technique non expliqué" (apps professionnelles complexes)**
- Problème : "Upload", "Sync", "Import CSV" → chauffeurs ne comprennent pas
- Pour FlotteBox : Langage terrain ("Scanner", "Envoyer", "Mes documents")
- Source : Tests utilisateurs montrent incompréhension du vocabulaire tech

**❌ Anti-Pattern #5 : "Erreurs bloquantes cryptiques" (apps anciennes)**
- Problème : "Error 500 - Internal Server Error" → panique utilisateur
- Pour FlotteBox : Messages clairs avec solutions ("Document flou, essayez avec plus de lumière")
- Source : Erreurs techniques augmentent churn de 40%

**❌ Anti-Pattern #6 : "Validation en masse obligatoire" (apps admin lourdes)**
- Problème : Forcer à remplir 20 champs avant d'enregistrer → friction
- Pour FlotteBox : Validation progressive, OCR pré-remplit, user corrige juste ce qui est faux
- Source : Formulaires longs augmentent abandon de 30% par champ supplémentaire

### Design Inspiration Strategy

**Ce que nous allons ADOPTER directement :**

**1. Workflow "Envoi photo WhatsApp" pour scan document**
- Bottom tab central "📸 Scanner" → Caméra → Photo → Validation OCR → Enregistré
- Max 3 taps, feedback immédiat à chaque étape
- Justification : Workflow éprouvé, adopté par millions, même utilisateurs 50-60 ans

**2. Codes couleur sémantiques "Google Maps" pour statuts**
- Vert/Orange/Rouge pour conformité véhicules
- Compréhension immédiate, pas de lecture nécessaire
- Justification : Universellement compris, fonctionne même pour daltoniens (formes différentes)

**3. Alertes multi-niveaux "Banking apps" pour échéances**
- Email standard + Push PWA + SMS optionnel
- Configuration granulaire (emails séparés opérationnel/facturation)
- Justification : Notifications intelligentes réduisent désactivation

**Ce que nous allons ADAPTER à notre contexte :**

**1. Navigation mobile "WhatsApp" → Simplifiée pour FlotteBox**
- Adapter : 3 tabs au lieu de 5 (Scanner, Véhicules, Profil)
- Pourquoi : Chauffeurs ont workflow plus simple que chat (pas de groupes, pas de statuts)

**2. Onboarding progressif "Doctolib" → Ultra-minimaliste FlotteBox**
- Adapter : Pas d'onboarding multi-écrans, juste tutoriel vidéo 90 sec skippable + premier scan guidé
- Pourquoi : Chauffeurs veulent "juste scanner", pas apprendre 10 fonctionnalités

**3. Dashboard "Banking app" → Focus conformité FlotteBox**
- Adapter : Cards véhicules avec statuts, pas de graphiques complexes
- Pourquoi : Dirigeants veulent vision immédiate, pas d'analytics détaillés (ça c'est pour Super Admin)

**Ce que nous allons ÉVITER absolument :**

**1. Menu hamburger caché** (apps complexes)
- Conflit avec : Principe "Un chauffeur de 55 ans doit réussir seul"
- Alternative : Bottom tabs visibles

**2. Onboarding multi-pages obligatoire** (apps bancaires anciennes)
- Conflit avec : Principe "Moins de clics = plus d'adoption"
- Alternative : Tutoriel vidéo skippable + guidage contextuel

**3. Notifications spam agressives** (e-commerce)
- Conflit avec : Principe "Notifications intelligentes, pas spam"
- Alternative : Configuration emails séparés + fréquence intelligente

**4. Jargon technique** (apps professionnelles lourdes)
- Conflit avec : Principe "Langage terrain, pas jargon"
- Alternative : "Scanner" pas "Uploader", "Mes véhicules" pas "Asset management"
