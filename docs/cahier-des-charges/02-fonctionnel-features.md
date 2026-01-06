## 4. Spécifications fonctionnelles

### 4.1 Module Authentification & Gestion utilisateurs

#### F1.1 - Inscription entreprise + Essai gratuit
**Priorité : P0 (MVP)**

**Flux d'inscription avec CB obligatoire :**
1. L'utilisateur arrive sur la page d'inscription
2. **Méthode d'authentification (choix) :**
   - **Option A : "Continuer avec Google"** (bouton OAuth Google)
     - Popup Google → Autorisation
     - Email, nom et prénom récupérés automatiquement depuis le compte Google
     - Pas besoin de validation email (déjà vérifié par Google)
   - **Option B : "S'inscrire avec email"** (formulaire classique)
     - Email professionnel
     - Mot de passe (min 8 caractères, 1 majuscule, 1 chiffre)
     - Validation email obligatoire (lien de confirmation)
   - Séparateur visuel : "OU"
3. **Choix du plan** (STARTER, PRO, BUSINESS, TEAM)
   - Affichage clair du prix mensuel
   - Badge "Essai gratuit 14 jours" sur chaque plan
4. **Formulaire informations entreprise :**
   - Nom entreprise
   - SIRET (optionnel à l'inscription)
   - Acceptation CGU/CGV
5. **Page de paiement (LemonSqueezy Checkout) :**
   - Message clair affiché :
     ```
     ┌─────────────────────────────────────────────────┐
     │ ✅ Essai gratuit pendant 14 jours                │
     │ 💳 Carte bancaire requise (pas de débit immédiat)│
     │ 💰 Premier débit le [date]                      │
     │ ❌ Annulez à tout moment avant cette date       │
     └─────────────────────────────────────────────────┘
     ```
   - Saisie CB (via LemonSqueezy)
   - Validation 3D Secure
   - **Création de la subscription LemonSqueezy** avec période d'essai de 14 jours
6. **Validation email** (uniquement si inscription par email, pas nécessaire avec Google OAuth)
7. Création automatique du compte administrateur
8. Génération d'un espace isolé (tenant)
9. **Activation automatique de l'essai gratuit 14 jours**
   - Subscription LemonSqueezy en mode "trial"
   - Accès complet aux fonctionnalités du plan choisi
   - Date de fin d'essai affichée clairement dans le dashboard

**Gestion de l'essai gratuit avec auto-renew :**
- Champs dans la table `companies` :
  - `trial_end_date` : date de fin d'essai
  - `lemonsqueezy_subscription_id` : ID de la subscription LemonSqueezy
  - `subscription_status` : "trial" puis "active" après J-14
- Badge "Essai gratuit - X jours restants" visible dans la sidebar
- À J-7 : notification in-app "Plus que 7 jours d'essai"
- À J-3 : **email critique** "Vous serez débité le [date] si vous ne résiliez pas" + lien "Annuler mon essai"
- À J-1 : **email + notification urgent** "Dernier jour avant débit automatique"
- À J-14 :
  - **LemonSqueezy débite automatiquement** le premier mois (avec TVA gérée automatiquement)
  - Statut passe de "trial" à "active"
  - Email de confirmation "Bienvenue chez FlotteBox ! Votre abonnement est actif."
  - Facture envoyée par email (via LemonSqueezy avec TVA incluse)
- **Si annulation avant J-14 :**
  - L'utilisateur clique "Annuler mon essai" dans les paramètres
  - Annulation de la subscription LemonSqueezy (via API)
  - Compte désactivé (mode lecture seule)
  - Pas de débit effectué
  - Données conservées 30 jours pour réactivation
- Après 30 jours sans réactivation : suppression des données (conformité RGPD)

**Critères d'acceptation :**
- **OAuth Google :**
  - Bouton "Continuer avec Google" visible et fonctionnel
  - Popup Google s'ouvre correctement (OAuth 2.0)
  - Email, nom et prénom récupérés automatiquement
  - Pas de validation email nécessaire (déjà vérifié par Google)
  - Fonctionne sur tous navigateurs (Chrome, Firefox, Safari, Edge)
- **Inscription classique :**
  - Email de validation envoyé en < 5 secondes
  - Impossible de créer 2 comptes avec le même email (que ce soit via Google ou email)
  - SIRET validé format 14 chiffres (si fourni)
- **Paiement LemonSqueezy :**
  - Page de paiement LemonSqueezy sécurisée (HTTPS + 3D Secure)
  - Message "Essai gratuit 14 jours" clairement affiché avant saisie CB
  - Subscription LemonSqueezy créée avec période d'essai de 14 jours
  - Pas de débit immédiat (vérifiable dans LemonSqueezy Dashboard)
  - TVA calculée et gérée automatiquement selon la localisation du client
- **Essai gratuit :**
  - Essai de 14 jours activé automatiquement
  - Emails envoyés aux bons moments (J-7, J-3, J-1)
  - Débit automatique à J-14 (conversion automatique via LemonSqueezy)
  - Bouton "Annuler mon essai" fonctionnel et accessible dans les paramètres

#### F1.2 - Connexion
**Priorité : P0 (MVP)**

**Méthodes de connexion :**
- **Option A : "Se connecter avec Google"** (bouton OAuth Google)
  - Popup Google → Autorisation → Connexion automatique
  - Redirection vers dashboard
- **Option B : Login classique** (email + mot de passe)
  - Email
  - Mot de passe
  - Option "Se souvenir de moi" (7 jours)
  - Lien "Mot de passe oublié"
- Séparateur visuel : "OU"
- Redirection vers dashboard après connexion réussie

**Critères d'acceptation :**
- Connexion en < 2 secondes (quelle que soit la méthode)
- OAuth Google fonctionne sur tous navigateurs
- Compte Google et compte email avec même adresse sont considérés comme identiques
- 3 tentatives échouées (email/mdp) = compte temporairement bloqué (15 min)
- Session expiration après 24h d'inactivité

#### F1.3 - Gestion des utilisateurs (multi-comptes)
**Priorité : P1 (post-MVP)**

- L'admin peut inviter des utilisateurs (email)
- Rôles : Admin, Gestionnaire, Chauffeur (lecture seule)
- Gestion des permissions par rôle

**Critères d'acceptation :**
- Invitation par email avec lien d'activation
- Un chauffeur ne voit que ses véhicules assignés
- Un gestionnaire peut tout voir mais pas supprimer

---

### 4.2 Module Véhicules

#### F2.1 - Ajouter un véhicule
**Priorité : P0 (MVP)**

**Deux modes de création disponibles :**

**Mode 1 : Création manuelle (MVP)**

**Champs obligatoires :**
- Type (camion, VUL, remorque)
- Immatriculation
- Marque
- Modèle
- Année de mise en circulation

**Champs optionnels :**
- VIN (numéro de série)
- Kilométrage actuel
- Date d'achat
- Photo du véhicule
- Conducteur assigné
- Notes libres

**Mode 2 : Création depuis scan de carte grise (P1 - Add-on OCR payant)**

**💰 Tarification :** Add-on OCR automatique à 0,10€/document scanné (voir section 2.4.3)

**Fonctionnalités :**
- Bouton "Créer depuis une carte grise" 🔒 (badge "OCR requis" si add-on non activé)
- Upload ou scan caméra (mobile) de la carte grise
- **OCR automatique** pour extraire les informations :
  - Immatriculation (champ D.1)
  - Marque (champ D.1)
  - Modèle (champ D.2)
  - VIN (champ E)
  - Date de 1ère mise en circulation (champ B)
  - Type de véhicule (champ J.1)
  - Poids (champ F.1, F.2, F.3)
- **Pré-remplissage automatique** du formulaire avec les données extraites
- L'utilisateur **DOIT vérifier/corriger** les champs avant validation (pas de création automatique sans validation)
- La carte grise scannée est automatiquement enregistrée comme document du véhicule

**Workflow création depuis scan (unitaire) :**
1. Clic sur "Créer depuis une carte grise"
2. Upload/scan de la carte grise
3. Processing OCR (2-5 secondes) - Coût : 0,10€
4. Formulaire pré-rempli affiché avec données extraites
5. **Utilisateur VÉRIFIE et corrige si nécessaire** (étape obligatoire)
6. Validation → Véhicule créé + Carte grise attachée

**Import multiple de cartes grises (avec validation) :**
- Upload d'un ZIP de cartes grises (max 50 à la fois pour éviter surcharge)
- Processing OCR sur toutes les cartes (coût affiché : X cartes × 0,10€ = Y€)
- **Écran de validation intermédiaire** : tableau avec toutes les données extraites
  - Colonnes : Immatriculation | Marque | Modèle | VIN | Date circulation | Actions
  - Utilisateur peut corriger les données directement dans le tableau
  - Cases à cocher pour sélectionner les véhicules à créer (tous cochés par défaut)
  - Lignes en rouge si erreur détectée (immatriculation illisible, doublon, etc.)
- Bouton "Créer X véhicules" après validation
- Rapport de création : X véhicules créés, Y erreurs corrigées manuellement

**Gestion des erreurs et limites :**
- **Limite par import** : 50 cartes grises maximum par ZIP (évite surcharge serveur)
- **Détection de doublons** : si immatriculation existe déjà → ligne en orange, création bloquée
- **Validation obligatoire** : pas de création automatique sans confirmation humaine
- **Timeout** : si OCR prend > 30s → erreur, utilisateur peut réessayer
- **Coût transparent** : affichage du coût total avant lancement de l'OCR

**Critères d'acceptation :**
- Validation du format d'immatriculation (AA-123-BB)
- Impossible d'ajouter 2 véhicules avec même immatriculation
- Création manuelle en < 1 seconde
- OCR carte grise avec précision > 95% (immatriculation, marque, modèle)
- Création depuis scan en < 5 secondes (traitement OCR inclus)
- Import multiple : 10 cartes grises → 10 véhicules créés en < 30 secondes

#### F2.2 - Liste des véhicules
**Priorité : P0 (MVP)**

**Affichage :**
- Vue en grille (cartes) ou liste (tableau)
- Informations visibles : immatriculation, type, statut de conformité
- Indicateurs visuels :
  - ✅ Vert : tous docs à jour
  - ⚠️ Orange : doc expire dans < 30 jours
  - ❌ Rouge : doc expiré ou manquant

**Fonctionnalités :**
- Recherche par immatriculation, marque, modèle
- Filtres : type de véhicule, statut conformité, conducteur
- Tri : par immatriculation, date d'ajout, statut

**Critères d'acceptation :**
- Affichage de 100 véhicules en < 2 secondes
- Recherche en temps réel (< 500ms)

#### F2.3 - Fiche véhicule détaillée
**Priorité : P0 (MVP)**

**Sections :**
1. Informations générales (modifiable)
2. Documents liés (liste avec statut)
3. Historique des alertes
4. Actions rapides (modifier, archiver, supprimer)

**Critères d'acceptation :**
- Chargement de la fiche en < 1 seconde
- Modification en temps réel (save automatique ou bouton explicite)

#### F2.4 - Import en masse (CSV)
**Priorité : P0 (MVP) - Critical pour l'onboarding**

**Objectif :** Permettre l'import de plusieurs véhicules et conducteurs via fichier CSV pour faciliter la migration depuis Excel ou autre outil.

**Fonctionnalités :**
- Bouton "Importer des véhicules" en haut de la liste véhicules
- Upload fichier CSV (max 5 MB, encodage UTF-8)
- Template CSV téléchargeable avec colonnes pré-remplies
- Preview des données avant import (tableau avec 10 premières lignes)
- Validation automatique :
  - Format immatriculation (AA-123-BB)
  - Types véhicules valides (liste prédéfinie)
  - Dates au bon format (YYYY-MM-DD)
  - Champs obligatoires remplis
- Rapport d'import :
  - X véhicules importés avec succès
  - Y erreurs détectées (ligne par ligne avec message explicite)
  - Option "Corriger le fichier et réimporter"

**Format CSV attendu :**
```csv
type,immatriculation,marque,modele,annee,vin,kilometrage,conducteur_email,conducteur_nom,conducteur_prenom,notes
camion,AB-123-CD,Renault,Master,2020,VF1MA000123456789,45000,jean.dupont@example.com,Dupont,Jean,Camion principal
van,CD-456-EF,Peugeot,Boxer,2019,VF3YCYHZF12345678,32000,marie.martin@example.com,Martin,Marie,
```

**Colonnes véhicule :**
- `type` (obligatoire) : truck, van, trailer, construction
- `immatriculation` (obligatoire)
- `marque` (obligatoire)
- `modele` (obligatoire)
- `annee` (obligatoire)
- `vin` (optionnel)
- `kilometrage` (optionnel)
- `notes` (optionnel)

**Colonnes conducteur (optionnelles) :**
- `conducteur_email` : si fourni, crée ou assigne le conducteur
- `conducteur_nom`
- `conducteur_prenom`
- `conducteur_telephone` (optionnel)

**Logique :**
1. Si email conducteur existe déjà → assignation du véhicule
2. Si email conducteur nouveau → création du compte conducteur (rôle "driver") + email d'invitation automatique
3. Si pas de conducteur → véhicule non assigné

**Gestion des doublons :**
- Si immatriculation existe déjà : mise à jour du véhicule (pas de création)
- Option "Écraser les données existantes" ou "Ignorer les doublons"

**Critères d'acceptation :**
- Import de 100 véhicules en < 10 secondes
- Validation temps réel pendant l'upload (avant sauvegarde)
- Rapport d'erreurs téléchargeable en CSV
- Rollback automatique si erreur critique (transaction atomique)
- Logs d'import dans la table `activity_logs`

**Template CSV fourni :**
- Lien "Télécharger le template" sur la page d'import
- Template pré-rempli avec 3 exemples fictifs
- Instructions dans un fichier README.txt joint

---

#### F2.5 - Import intelligent de documents en masse (Post-MVP P1)
**Priorité : P1 (post-MVP) - Killer feature pour la mise à jour en masse**

Cette fonctionnalité permet de **mettre à jour automatiquement tous les documents de la flotte** en une seule opération, ou de **créer plusieurs véhicules** à partir de leurs cartes grises.

**Cas d'usage typique :**

**Pour mise à jour de documents existants :**
- Fin d'année : renouvellement de toutes les assurances → 50 nouveaux PDF reçus par email
- Retour du garage : 10 contrôles techniques effectués en même temps
- Changement d'assureur : nouvelles attestations pour toute la flotte

**Pour création en masse de véhicules :**
- Nouvelle entreprise : 20 cartes grises → création de 20 véhicules après validation
- Achat de plusieurs véhicules d'occasion : création rapide de la flotte
- Import initial lors de l'onboarding

**💰 Tarification :** Add-on OCR automatique à 0,10€/document scanné

**Fonctionnement :**
1. **Upload en masse** : drag & drop d'un dossier ZIP contenant 20-100 PDF (max 100)
2. **Affichage du coût** : "X documents détectés × 0,10€ = Y€ - Confirmer ?"
3. **OCR automatique** (après confirmation) : le système lit chaque document et extrait :
   - Type de document (carte grise, CT, assurance, etc.)
   - Immatriculation du véhicule
   - Dates d'émission et d'expiration
   - Numéro de document
4. **Matching intelligent** :
   - Pour chaque document, recherche du véhicule correspondant en BDD via l'immatriculation
   - **Si véhicule trouvé** → vérification des documents existants du même type + comparaison des dates
   - **Si véhicule non trouvé ET type = carte grise** → ajout à la liste des véhicules à créer
5. **Écran de validation intermédiaire** (si cartes grises détectées sans véhicule) :
   - Tableau avec tous les véhicules à créer
   - Utilisateur peut **corriger les données** avant création
   - Cases à cocher pour sélectionner les véhicules à créer
   - Détection de doublons (immatriculations déjà existantes)
6. **Traitement selon le cas** :
   - **Mise à jour de document** : Si le nouveau document est plus récent → remplacement automatique, ancien archivé
   - **Création de véhicule depuis carte grise** : Après validation utilisateur → création du véhicule + attachement de la carte grise
   - Alertes recalculées automatiquement
7. **Rapport de traitement** :
   - ✅ 45 documents importés et associés avec succès
   - 🆕 5 véhicules créés depuis cartes grises (après validation)
   - ⚠️ 3 documents non reconnus (immatriculation illisible)
   - ℹ️ 2 documents ignorés (plus anciens que ceux en BDD)
   - 💰 Coût total : 53 × 0,10€ = 5,30€

**Avantages :**
- **Gain de temps massif** : 50 documents → 2 minutes au lieu de 30-45 minutes
- **Pas d'erreur humaine** : pas de risque d'associer le mauvais document au mauvais véhicule
- **Pas de doublon** : le système gère automatiquement les versions (garde la plus récente)
- **Traçabilité** : historique complet des remplacements

**Logique de gestion des versions :**
```
SI nouveau_doc.type == ancien_doc.type ET nouveau_doc.vehicule_id == ancien_doc.vehicule_id ALORS
  SI nouveau_doc.date_expiration > ancien_doc.date_expiration ALORS
    → Archiver ancien_doc (statut: "remplacé")
    → Sauvegarder nouveau_doc (statut: "actif")
    → Log: "Assurance de AB-123-CD mise à jour (expire le 15/12/2025 au lieu du 10/01/2025)"
  SINON
    → Ignorer nouveau_doc
    → Log: "Document ignoré : version plus ancienne détectée"
  FIN SI
FIN SI
```

**Options avancées (interface) :**
- **Mode "Preview"** : voir tous les changements avant de valider
- **Mode "Force"** : écraser même si plus ancien (cas particulier)
- **Sélection manuelle** : si OCR incertain, demander confirmation utilisateur
- **Notification par email** : résumé envoyé après traitement

**Exemple concret :**
```
Entreprise BTP avec 50 camions.
L'assureur envoie 50 nouvelles attestations d'assurance par email (1 ZIP).

Workflow actuel (sans cette feature) :
- Télécharger le ZIP
- Ouvrir chaque PDF manuellement
- Trouver le véhicule correspondant dans FlotteBox
- Uploader le document
- Remplir les métadonnées (type, dates)
- Répéter 50 fois
→ Temps : 30-45 minutes

Workflow avec import intelligent :
1. Drag & drop du ZIP sur FlotteBox
2. Clic "Analyser avec OCR"
3. Preview : "50 assurances détectées, 50 véhicules trouvés"
4. Clic "Valider l'import"
→ Temps : 2 minutes
→ Coût : 50 × 0,10€ = 5€
```

**Tarification :**
- Intégré dans l'add-on **OCR automatique** : 0,10€/document scanné
- Exemple : import de 50 assurances → 5€

**Alternative sans OCR (post-MVP P2) :**
- Import ZIP + CSV de métadonnées (mapping manuel immatriculation → nom_fichier)
- Format CSV :
```csv
immatriculation,nom_fichier,type_document,date_emission,date_expiration
AB-123-CD,assurance_AB123CD.pdf,assurance,2024-01-01,2025-12-31
CD-456-EF,assurance_CD456EF.pdf,assurance,2024-01-01,2025-12-31
```
- Plus fastidieux mais **sans coût OCR** (gratuit)

**Critères d'acceptation :**
- Import de 100 documents en < 30 secondes (hors OCR)
- OCR avec précision > 95% sur immatriculations
- Gestion des doublons : toujours garder le plus récent
- Historique : ancien document archivé, pas supprimé
- Rapport téléchargeable en CSV
- Mode preview obligatoire avant validation

**Impact business :**
- **Réduction du churn** : plus besoin de quitter FlotteBox pour gérer les mises à jour en masse
- **Argument de vente** : "Mettez à jour 50 assurances en 2 minutes"
- **Revenus OCR** : source de revenus récurrente (renouvellements annuels)

---

#### F2.6 - Archiver/Supprimer un véhicule
**Priorité : P1 (post-MVP)**

- Archiver : véhicule masqué par défaut mais conservé
- Supprimer : confirmation obligatoire, suppression cascade des documents

**Critères d'acceptation :**
- Popup de confirmation avec texte explicite
- Documents associés supprimés ou déplacés vers archive

---

### 4.3 Module Documents

#### F3.1 - Uploader un document
**Priorité : P0 (MVP)**

**Méthodes d'upload :**
1. Drag & drop sur desktop
2. Sélection fichier (input file)
3. **Scan caméra depuis mobile (PWA)** ← priorité
4. Futur : OCR automatique pour extraction données

**Champs à renseigner :**
- Type de document (dropdown)
- Date d'émission
- Date d'expiration (si applicable)
- Numéro de document (optionnel)
- Notes (optionnel)

**Formats acceptés :**
- PDF (max 10 MB)
- Images : JPG, PNG, WEBP (max 5 MB)

**Critères d'acceptation :**
- Upload en < 5 secondes pour un PDF de 2 MB
- Preview du document après upload
- Compression automatique des images > 2 MB
- Message d'erreur clair si format non supporté

#### F3.2 - Consulter un document
**Priorité : P0 (MVP)**

- Visualisation inline (PDF.js pour PDF, image pour JPG/PNG)
- Téléchargement direct
- Informations : date upload, uploadé par qui, taille fichier

**Critères d'acceptation :**
- Preview en < 2 secondes
- Téléchargement avec nom de fichier explicite : `{immatriculation}_{type_doc}_{date}.pdf`

#### F3.3 - Modifier/Supprimer un document
**Priorité : P0 (MVP)**

- Modifier les métadonnées (dates, type, notes)
- Remplacer le fichier (nouvel upload)
- Supprimer avec confirmation

**Critères d'acceptation :**
- Historique des modifications (qui, quand)
- Impossible de supprimer un document si c'est le seul du type pour le véhicule (avertissement)

#### F3.4 - Scan via caméra mobile (PWA)
**Priorité : P0 (MVP)**

**Fonctionnement :**
- Bouton "Scanner un document" accessible depuis fiche véhicule ou page documents
- Ouverture de la caméra du téléphone
- Capture photo → preview → confirmation → upload
- Option "Retake" si photo floue/mal cadrée

**Optimisations :**
- Détection de contours (crop automatique si possible)
- Compression avant upload (max 1 MB)
- Mode portrait et paysage supportés

**Critères d'acceptation :**
- Caméra s'ouvre en < 1 seconde
- Photo uploadée en < 5 secondes
- Fonctionne sur iOS Safari + Android Chrome

---

### 4.4 Module Alertes & Notifications

#### F4.1 - Calcul automatique des alertes
**Priorité : P0 (MVP)**

**Logique :**
- Chaque document avec date d'expiration génère une alerte
- Seuils d'alerte :
  - 60 jours avant : notification "Info" (bleue)
  - 30 jours avant : notification "Attention" (orange)
  - 15 jours avant : notification "Urgent" (rouge)
  - 0 jour (expiré) : notification "Critique" (rouge foncé)

**Critères d'acceptation :**
- Calcul des alertes en temps réel à l'ouverture du dashboard
- Job quotidien (3h du matin) pour envoi emails

#### F4.2 - Dashboard des alertes
**Priorité : P0 (MVP)**

**Affichage :**
- Widget sur homepage : "X alertes urgentes"
- Page dédiée avec liste complète
- Groupement par urgence et par véhicule
- Action : "Marquer comme traité" / "Snooze 7 jours"

**Critères d'acceptation :**
- Chargement en < 1 seconde
- Compteurs à jour en temps réel

#### F4.3 - Notifications par email
**Priorité : P0 (MVP)**

**Fréquence :**
- Email quotidien (8h du matin) si alertes critiques/urgentes
- Email hebdomadaire (lundi 8h) résumé de toutes les alertes

**Contenu email :**
- Liste des véhicules concernés
- Type de document à renouveler
- Jours restants
- Lien direct vers la fiche véhicule

**Critères d'acceptation :**
- Email délivré en < 5 minutes après génération
- Lien de désabonnement dans le footer
- Template responsive (mobile-friendly)

#### F4.4 - Notifications push (PWA)
**Priorité : P2 (post-MVP)**

- Notifications push natives via PWA
- Activables/désactivables par utilisateur
- Même logique que les emails

---

### 4.5 Module Dashboard & Reporting

#### F5.1 - Dashboard homepage
**Priorité : P0 (MVP)**

**Widgets :**
1. **Statistiques globales** :
   - Nombre total de véhicules
   - Documents à jour vs expirés
   - Alertes urgentes
   
2. **Véhicules critiques** :
   - Liste des 5 véhicules avec le plus d'alertes
   
3. **Timeline** :
   - Prochaines échéances sur 30 jours (calendrier)

4. **Activité récente** :
   - Derniers documents uploadés
   - Dernières modifications

**Critères d'acceptation :**
- Chargement complet en < 2 secondes
- Graphiques avec Recharts (simple et performant)
- Responsive (mobile + desktop)

#### F5.2 - Exports comptables
**Priorité : P1 (post-MVP)**

**Formats :**
- Export Excel : liste véhicules + documents + dates d'expiration
- Export PDF : rapport de conformité de la flotte

**Filtres :**
- Période (date range)
- Type de documents
- Statut (à jour, expiré, bientôt expiré)

**Critères d'acceptation :**
- Export généré en < 10 secondes pour 100 véhicules
- Nom de fichier : `export_flotte_{date}.xlsx`

---

### 4.6 Module PWA (Progressive Web App)

#### F6.1 - Installation PWA
**Priorité : P0 (MVP)**

**Fonctionnalités :**
- Bouton "Installer l'app" visible sur mobile
- Icône sur home screen après installation
- Splash screen personnalisé
- Fonctionne offline (mode lecture seule des données en cache)

**Critères d'acceptation :**
- Manifeste PWA valide (Lighthouse score > 90)
- Service worker enregistré
- Installable sur iOS Safari + Android Chrome

#### F6.2 - Interface mobile optimisée
**Priorité : P0 (MVP)**

**Spécificités mobile :**
- Navigation bottom tab bar (Véhicules, Scanner, Alertes, Profil)
- Cartes véhicules en mode liste (pas de grille)
- Accès rapide à la caméra depuis n'importe quelle page
- Swipe gestures (retour arrière, refresh)

**Critères d'acceptation :**
- Interface fluide (60 FPS)
- Boutons tactiles de taille suffisante (min 44×44px)
- Pas de zoom involontaire

#### F6.3 - Mode offline
**Priorité : P2 (post-MVP)**

- Cache des données consultées récemment
- Mode lecture seule offline
- Synchronisation automatique au retour de connexion

---

### 4.7 Module Administration

#### F7.1 - Paramètres compte
**Priorité : P0 (MVP)**

- Modifier nom entreprise, logo
- Modifier email de contact
- Changer mot de passe
- Supprimer compte (avec confirmation)

#### F7.2 - Facturation & Abonnement
**Priorité : P0 (MVP)**

**Intégration LemonSqueezy :**
- Affichage du plan actuel (Starter, Business, Pro)
- Nombre de véhicules utilisés / limite du plan
- Bouton "Changer de plan"
- Historique des factures (téléchargement PDF avec TVA incluse)
- Affichage automatique de la TVA selon la localisation

**Critères d'acceptation :**
- Paiement sécurisé via LemonSqueezy Checkout
- Mise à jour du plan en temps réel après paiement
- Email de confirmation après chaque paiement
- TVA calculée et affichée automatiquement

#### F7.3 - Bouton d'aide et support
**Priorité : P0 (MVP)**

**Emplacement :**
- Bouton fixe dans la sidebar (icône "?" ou "Aide")
- Visible sur toutes les pages du dashboard
- Version mobile : accessible depuis le menu profil

**Fonctionnalités :**
- **Tooltip au survol** : "Besoin d'aide ?"
- **Click** : ouvre un panneau latéral (slide-in) avec :
  1. **Guide de démarrage rapide** (liens vers docs)
  2. **FAQ** (questions fréquentes contextuelles)
  3. **Contacter le support** : formulaire rapide
     - Type : Bug, Question, Suggestion, Autre
     - Message (textarea)
     - Capture d'écran optionnelle (upload ou paste)
     - Bouton "Envoyer" → email envoyé via Resend à support@flottebox.fr
  4. **Ressources utiles** :
     - Tutoriel vidéo "Scanner un document" (YouTube embed)
     - Guide PDF "Prise en main FlotteBox"
     - Lien vers changelog (nouvelles fonctionnalités)

**Confirmation d'envoi :**
- Toast notification : "Votre message a été envoyé ! Nous vous répondons sous 24h."
- Email de confirmation automatique à l'utilisateur
- Création automatique d'une entrée dans la table `feedback` (pour le super admin)

**Critères d'acceptation :**
- Panneau s'ouvre en < 500ms
- Formulaire validé (Zod) avant envoi
- Email envoyé via Resend avec template HTML
- Historique des demandes consultable dans les paramètres utilisateur
- Badge de notification si réponse reçue

#### F7.4 - Logs d'activité
**Priorité : P2 (post-MVP)**

- Historique de toutes les actions :
  - Qui a uploadé quel document
  - Qui a modifié quel véhicule
  - Connexions/déconnexions
- Filtrable par utilisateur, action, date

---

### 4.8 Module Super Admin (Quentin uniquement)

**Priorité : P1 (post-MVP mais important pour piloter le business)**

Ce module est accessible uniquement par le compte super admin (Quentin) et permet de piloter l'ensemble du SaaS.

#### F8.1 - Dashboard Analytics Global

**Métriques business :**
- **MRR (Monthly Recurring Revenue)** : revenu mensuel récurrent
- **ARR** : revenu annuel projeté
- **Nombre de clients** : actifs, trial, churned
- **Churn rate** : taux de désabonnement mensuel
- **ARPU (Average Revenue Per User)** : revenu moyen par client
- **LTV (Lifetime Value)** : valeur vie client moyenne
- **CAC payback** : temps pour récupérer le coût d'acquisition

**Métriques produit :**
- **Utilisateurs actifs** : DAU, WAU, MAU
- **Taux d'adoption** : % clients ayant uploadé au moins 1 document
- **Taux d'activation** : % nouveaux inscrits ayant ajouté leur 1er véhicule
- **Feature usage** : % utilisation scan mobile, alertes, export, etc.
- **Time to value** : temps moyen entre inscription et 1ère action utile
- **Stickiness** : DAU/MAU ratio (engagement)

**Graphiques :**
- Évolution MRR sur 12 mois (courbe)
- Distribution clients par plan (pie chart)
- Croissance nette (new - churn) par mois
- Funnel d'activation (inscription → 1er véhicule → 1er doc → alerte activée)
- Heatmap usage par jour de la semaine

**Critères d'acceptation :**
- Dashboard temps réel (rafraîchi toutes les heures)
- Export Excel de toutes les données
- Comparaison vs mois précédent (%, flèches)

#### F8.2 - Liste clients avec filtres avancés

**Informations affichées par client :**
- Nom entreprise, SIRET
- Plan actuel, MRR
- Date inscription, statut (trial, actif, churned, en retard de paiement)
- Nombre véhicules, nombre documents uploadés
- Nombre users, dernier login
- Taux d'utilisation (véhicules utilisés / limite plan)
- Health score (vert/orange/rouge selon engagement)

**Filtres disponibles :**
- Par plan (Starter, Pro, Business, Team, Enterprise)
- Par statut (trial, actif, churned, payment_failed)
- Par date inscription (7j, 30j, 3 mois, custom)
- Par engagement :
  - Actifs (login < 7 jours)
  - À risque (pas de login depuis 14+ jours)
  - Inactifs (pas de login depuis 30+ jours)
- Par usage :
  - Power users (>80% quota utilisé)
  - Sous-utilisateurs (<30% quota)
- Par MRR (< 100€, 100-500€, 500€+)
- Par taux d'adoption features (scan mobile oui/non, alertes activées, export utilisé)

**Actions rapides :**
- Bouton "Contacter" → ouvre email pré-rempli avec contexte
- Bouton "Impersonate" → se connecter en tant que ce client (mode démo/debug)
- Bouton "Notes" → ajouter note privée (ex: "client VIP", "demande fonctionnalité X")
- Bouton "Historique" → voir toute l'activité du client

**Exports :**
- Export CSV de la liste filtrée
- Export avec segmentation (ex: "tous les clients à risque de churn")

**Critères d'acceptation :**
- Recherche instantanée (< 500ms)
- Pagination (50 clients par page)
- Tri par colonne (MRR, date inscription, dernier login)

#### F8.3 - Détail client (vue 360°)

**Page dédiée pour analyser un client en profondeur :**

**Section 1 : Informations générales**
- Entreprise, contact principal, plan, MRR
- Date inscription, date dernière activité
- LemonSqueezy customer ID (lien direct vers LemonSqueezy)
- Notes internes (éditables)
- Tags (ex: "Lead chaud", "Churned", "VIP", "Beta tester")

**Section 2 : Métriques d'engagement**
- Nombre de connexions (graphique 30 derniers jours)
- Véhicules ajoutés (courbe cumulative)
- Documents uploadés (courbe)
- Alertes générées et traitées
- Features utilisées (checkboxes : scan mobile, export, API, etc.)

**Section 3 : Usage détaillé**
- Quota véhicules : X / Y utilisés (barre de progression)
- Quota users : X / Y utilisés
- **Quota OCR mensuel** :
  - Si quota gratuit : "45 / 100 docs OCR utilisés ce mois" (barre de progression)
  - Si pay-as-you-go : "23 docs OCR ce mois (2,30€)" + graphique évolution mensuelle
  - Coût OCR total depuis inscription : X€
- Dernier login de chaque utilisateur
- Documents par type (camembert : CT, assurance, carte grise, etc.)
- Top 5 actions récentes

**Section 4 : Santé du compte**
- **Health score** : 0-100 (algorithme basé sur engagement)
  - 80-100 : Vert (healthy, très engagé)
  - 50-79 : Orange (à surveiller)
  - 0-49 : Rouge (risque churn)
- Facteurs positifs : "Login régulier", "Utilise toutes les features"
- Facteurs négatifs : "Pas de login depuis 15j", "Seulement 30% quota utilisé"
- **Prédiction churn** : Probabilité de désabonnement (ML post-MVP)

**Section 5 : Historique facturation**
- Liste des paiements (date, montant, statut)
- Upgrades/downgrades de plan
- Paiements échoués (avec relance)

**Section 5.5 : Gestion du plan et quotas personnalisés**

**Modification du plan :**
- Dropdown pour changer le plan actuel (STARTER, PRO, BUSINESS, TEAM, ENTERPRISE)
- Bouton "Appliquer" avec confirmation
- Log de l'action dans l'historique client

**Quotas personnalisés (overrides) :**
Interface pour définir des quotas custom qui **écrasent** les limites du plan standard :

```
┌─────────────────────────────────────────────────────┐
│ Quotas personnalisés                                │
├─────────────────────────────────────────────────────┤
│ 🚗 Véhicules                                         │
│    Plan : 30 véhicules                              │
│    Custom : [___50___] véhicules  ☑ Activer override│
│                                                     │
│ 👥 Utilisateurs                                      │
│    Plan : 5 utilisateurs                            │
│    Custom : [___10___] users  ☑ Activer override    │
│                                                     │
│ 📄 OCR mensuel                                       │
│    Plan : 0€/doc (pay-as-you-go)                   │
│    Custom : [___100___] docs/mois gratuits          │
│           ☑ Activer (forfait inclus)               │
│                                                     │
│ 🔌 API Access                                        │
│    Plan : Non inclus (49€/mois)                    │
│    Custom : ☑ Offert gratuitement                  │
│                                                     │
│ 📞 Support téléphone                                 │
│    Plan : Non inclus (99€/mois)                    │
│    Custom : ☑ Offert gratuitement                  │
│                                                     │
│ 💰 Tarif mensuel custom                             │
│    Plan : 99€/mois                                  │
│    Custom : [___79___]€/mois  ☑ Tarif négocié      │
│                                                     │
│ 📝 Notes internes                                    │
│    [Client VIP - tarif négocié suite appel 15/01]  │
│                                                     │
│           [Enregistrer les modifications]           │
└─────────────────────────────────────────────────────┘
```

**Cas d'usage :**
- **Client VIP** : Plan PRO avec 100 docs OCR/mois offerts au lieu de 0
- **Deal commercial** : Plan BUSINESS à 149€/mois au lieu de 199€
- **Test bêta** : Plan STARTER avec API access offert temporairement
- **Partenaire** : Plan TEAM avec support téléphone inclus gratuitement
- **Grande flotte custom** : Plan BUSINESS avec quota 100 véhicules au lieu de 50

**Fonctionnalités techniques :**
- Les quotas custom sont stockés dans une table `company_overrides`
- Si override actif → utiliser quota custom, sinon utiliser quota du plan
- Historique des modifications (qui a changé quoi, quand)
- Badge "Custom" visible dans le dashboard client si overrides actifs
- Les overrides restent actifs même si le client change de plan (sauf si super admin les désactive)

**Synchronisation avec LemonSqueezy :**

**Architecture de séparation :**

FlotteBox est la **source de vérité** pour :
- ✅ Quotas véhicules, utilisateurs, OCR (gérés dans `company_overrides`)
- ✅ Accès aux features (API, support téléphone, export avancé)
- ✅ Limite des fonctionnalités selon le plan + overrides
- ✅ Facturation OCR pay-as-you-go (tracking dans `ocr_usage`)

LemonSqueezy gère uniquement :
- 💳 **Paiements récurrents mensuels/annuels** (abonnement de base)
- 💳 **TVA automatique** selon localisation
- 💳 **Webhooks** pour changements de subscription (upgrade, downgrade, annulation)
- 💳 **Coupons de réduction** sur l'abonnement de base

**Workflows de synchronisation :**

**1. Changement de plan par le client (via LemonSqueezy) :**
```
Client clique "Upgrade to PRO" dans FlotteBox
→ Redirection vers LemonSqueezy Checkout
→ Client paie
→ Webhook LemonSqueezy : subscription_updated
→ FlotteBox met à jour companies.subscription_plan = "pro"
→ Les overrides restent actifs (ne sont pas écrasés)
```

**2. Changement de plan par le super admin (manuel) :**
```
Super admin change le plan PRO → BUSINESS dans FlotteBox
→ FlotteBox met à jour companies.subscription_plan = "business"
→ ❌ AUCUNE synchronisation vers LemonSqueezy
→ Le client continue de payer le prix PRO (99€/mois)
→ Note interne obligatoire : "Deal commercial - paie PRO, a accès BUSINESS"
```

**3. Tarif custom par le super admin :**
```
Super admin définit custom_monthly_price = 149€ au lieu de 199€
→ FlotteBox stocke dans company_overrides.custom_monthly_price
→ ❌ AUCUNE synchronisation vers LemonSqueezy
→ Facturation manuelle ou via LemonSqueezy coupon créé manuellement
→ Note : "Coupon LS appliqué : 25% off BUSINESS"
```

**4. Facturation OCR pay-as-you-go :**
```
Client utilise 50 docs OCR dans le mois
→ FlotteBox enregistre dans ocr_usage (50 × 0.10€ = 5€)
→ Fin du mois : Edge Function calcule total OCR
→ ❌ Pas de facturation automatique via LemonSqueezy
→ Options :
   - Créer manuellement une facture LemonSqueezy (post-MVP)
   - Intégration API LemonSqueezy pour créer "one-time charges" (post-MVP)
   - MVP : facturation manuelle, email avec facture PDF
```

**5. Quota OCR gratuit (via override) :**
```
Super admin définit custom_ocr_monthly_quota = 100 docs/mois
→ Client utilise 45 docs ce mois
→ FlotteBox vérifie : 45 < 100 → was_free = true
→ Pas de facturation
→ Client utilise 120 docs le mois suivant
→ 100 premiers gratuits, 20 × 0.10€ = 2€ facturés
```

**Recommandations d'implémentation :**

**MVP (Phase 1) :**
- Abonnement de base géré 100% via LemonSqueezy
- Overrides gérés dans FlotteBox uniquement (pas de sync)
- Facturation OCR : email mensuel avec montant dû (pas de facturation automatique)

**Post-MVP (Phase 2) :**
- Intégration API LemonSqueezy pour créer des "one-time charges" (frais OCR)
- Synchronisation du tarif custom via LemonSqueezy coupons API
- Facturation OCR automatique en fin de mois

**Avantages de cette approche :**
- ✅ Flexibilité totale côté FlotteBox sans dépendances externes
- ✅ LemonSqueezy reste simple (uniquement abonnements récurrents)
- ✅ Pas de risque de désynchronisation pour les overrides
- ✅ TVA gérée automatiquement par LemonSqueezy sur l'abonnement de base
- ✅ Facturation OCR séparée = transparent pour le client

**Section 6 : Actions rapides**
- Envoyer email de feedback
- Programmer call de check-in
- Offrir upgrade trial
- Appliquer réduction (coupon LemonSqueezy)
- Passer en mode démo (voir F8.4)

**Critères d'acceptation :**
- Chargement en < 2 secondes
- Bouton "Refresh" pour actualiser les données
- Timeline des événements (inscription, 1er véhicule, upgrade, etc.)

#### F8.4 - Mode Démo

**Fonctionnalité pour les démos commerciales :**

**Activation du mode démo :**
- Depuis le super admin, bouton "Passer en mode démo"
- Sélection d'un compte client (ou création d'un compte démo type)
- Le compte se transforme en environnement de démo avec :
  - Données fictives mais réalistes (véhicules, documents, alertes)
  - Bannière discrète "Mode Démo" visible uniquement pour l'admin
  - Toutes les fonctionnalités actives

**Scénarios de démo pré-configurés :**

**Démo 1 : Entreprise BTP (50 véhicules)**
- 50 véhicules fictifs (camions, VUL, engins)
- 300 documents pré-uploadés
- 12 alertes (5 urgentes, 7 info)
- 5 utilisateurs (admin, 2 managers, 2 chauffeurs)
- Dashboard avec graphiques réalistes

**Démo 2 : Transporteur (25 véhicules)**
- 25 camions + remorques
- Focus sur la conformité (tous docs à jour)
- Alertes désactivées (tout est OK)
- Export comptable généré

**Démo 3 : Artisan (10 véhicules)**
- 10 camionnettes
- Scénario "découverte" : 2 véhicules avec docs, 8 vides
- Workflow d'ajout de 1er véhicule

**Fonctionnalités en mode démo :**
- Actions fictives : upload simule instantanément
- Reset rapide : bouton "Réinitialiser démo" pour recommencer
- Annotations : possibilité d'ajouter des post-its explicatifs sur l'interface
- Mode présentation : masquer les éléments de UI non essentiels

**Désactivation :**
- Bouton "Quitter mode démo" ramène au compte normal
- Expiration auto après 24h si oubli

**Critères d'acceptation :**
- Passage en mode démo en < 5 secondes
- Données fictives mais 100% cohérentes (pas d'incohérence dates, etc.)
- Aucun email envoyé depuis mode démo
- Aucune charge LemonSqueezy en mode démo

#### F8.5 - Gestion des événements (Event Tracking)

**Système de tracking pour comprendre l'usage :**

**Events trackés automatiquement :**

**Onboarding :**
- `user_signed_up` : inscription
- `user_email_verified` : validation email
- `company_created` : création entreprise
- `first_vehicle_added` : 1er véhicule ajouté
- `first_document_uploaded` : 1er document uploadé
- `first_alert_created` : 1ère alerte générée

**Usage quotidien :**
- `user_logged_in` : connexion
- `vehicle_added` : ajout véhicule
- `document_uploaded` : upload document (avec type)
- `document_viewed` : consultation document
- `alert_dismissed` : alerte marquée comme traitée
- `export_generated` : export Excel/PDF
- `scan_mobile_used` : scan caméra utilisé
- `plan_upgraded` : changement de plan (upgrade)
- `plan_downgraded` : changement de plan (downgrade)

**Friction/Erreurs :**
- `upload_failed` : échec upload (avec raison : taille, format, etc.)
- `payment_failed` : échec paiement LemonSqueezy
- `quota_limit_reached` : limite véhicules/users atteinte
- `page_error` : erreur 500/404 côté client

**Stockage :**
```sql
CREATE TABLE events (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  company_id UUID REFERENCES companies(id),
  user_id UUID REFERENCES users(id),
  event_name TEXT NOT NULL,
  event_data JSONB, -- Metadata spécifique (ex: {document_type: "CT", file_size: 2048})
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);

CREATE INDEX idx_events_company ON events(company_id);
CREATE INDEX idx_events_name ON events(event_name);
CREATE INDEX idx_events_created ON events(created_at);
```

**Dashboard events :**
- Vue "Funnel" : combien passent de l'inscription au 1er doc uploadé
- Vue "Feature adoption" : % clients ayant utilisé X feature
- Vue "Erreurs" : top 10 erreurs les plus fréquentes

**Critères d'acceptation :**
- Events trackés en temps réel (< 1s de latence)
- Pas de PII (Personally Identifiable Information) dans les events
- Agrégations par jour/semaine/mois

#### F8.6 - Segments clients & Campagnes

**Créer des segments pour actions ciblées :**

**Segments pré-définis :**
- **Churned récents** : désabonnés dans les 30 derniers jours → campagne win-back
- **Trial expiring** : trial expire dans 3 jours → relance conversion
- **Power users** : >80% quota + login quotidien → demande témoignage/référence
- **Sous-utilisateurs** : <30% quota après 60j → campagne d'activation
- **À risque** : pas de login depuis 21j → check-in call
- **Upgrade opportunity** : 90%+ quota utilisé → proposer plan supérieur

**Création de segment custom :**
- Filtres combinables (AND/OR)
- Sauvegarde du segment
- Export liste emails

**Actions sur segment :**
- Envoyer email groupé (via Resend)
- Appliquer coupon LemonSqueezy (ex: -20% pendant 3 mois)
- Ajouter tag à tous les clients du segment
- Planifier séquence d'emails (drip campaign - post-MVP)

**Critères d'acceptation :**
- Segment recalculé quotidiennement
- Export CSV avec emails + contexte
- Preview email avant envoi massif

#### F8.7 - Feedback & Support

**Centraliser les retours clients :**

**Widget feedback in-app :**
- Bouton discret "Feedback" dans l'app client
- Formulaire : type (bug, feature request, question), message, screenshot optionnel
- Envoyé directement au super admin

**Dashboard feedbacks :**
- Liste des feedbacks (non lu, en cours, résolu)
- Filtres : par type, par plan client, par date
- Possibilité de répondre directement
- Lien vers détail client pour contexte

**Feature requests voting :**
- Board public (type Canny ou intégré)
- Clients votent pour les features qu'ils veulent
- Quentin voit les demandes les plus populaires

**Critères d'acceptation :**
- Notification email à Quentin pour chaque nouveau feedback
- Temps de réponse moyen tracké
- Feedbacks exportables en CSV

---

