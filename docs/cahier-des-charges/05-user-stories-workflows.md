## 6. User Stories & Cas d'usage

### US1 - Inscription et première utilisation
**En tant que** gestionnaire de flotte
**Je veux** créer un compte et ajouter mes premiers véhicules
**Afin de** centraliser mes documents administratifs

**Critères d'acceptation :**
- Je peux m'inscrire en < 2 minutes
- Je peux ajouter mon premier véhicule immédiatement après inscription
- **OU** je peux importer toute ma flotte (10-50 véhicules) via CSV en < 5 minutes
- Je reçois un email de bienvenue avec guide de démarrage

**Scénario onboarding avec CSV :**
1. Inscription → validation email
2. Premier accès : modal "Comment voulez-vous commencer ?"
   - Option A : "Ajouter un véhicule manuellement" (pour tester)
   - Option B : **"Importer ma flotte existante (CSV)"** ← recommandé si 5+ véhicules
3. Si option B :
   - Téléchargement du template CSV
   - Instructions claires : "Remplissez le fichier avec vos données, puis importez-le"
   - Upload → preview → validation → import
   - "Félicitations ! 15 véhicules ont été importés. Prochaine étape : ajouter vos documents."

---

### US2 - Upload d'un document depuis mobile
**En tant que** chauffeur  
**Je veux** scanner la nouvelle assurance de mon camion avec mon téléphone  
**Afin que** le gestionnaire soit informé sans que j'aie à le faire depuis un ordinateur

**Critères d'acceptation :**
- J'ouvre l'app PWA depuis mon téléphone
- Je clique sur "Scanner document"
- La caméra s'ouvre, je prends la photo
- Je peux recadrer/retake si besoin
- Je sélectionne le type de document et la date d'expiration
- Le document est uploadé et visible immédiatement pour le gestionnaire

---

### US3 - Réception d'alerte critique
**En tant que** gestionnaire  
**Je veux** recevoir un email quand un CT expire dans 15 jours  
**Afin de** prendre RDV au garage avant l'expiration

**Critères d'acceptation :**
- Je reçois un email à 8h du matin
- L'email contient : véhicule concerné, type de document, jours restants, lien direct
- Je peux cliquer sur "Marquer comme traité" directement depuis l'email
- L'alerte disparaît du dashboard après traitement

---

### US4 - Vision globale de la conformité
**En tant que** dirigeant  
**Je veux** voir d'un coup d'œil quels véhicules sont en règle  
**Afin de** préparer un contrôle ou audit sans stress

**Critères d'acceptation :**
- Le dashboard affiche un compteur : X véhicules OK, Y en alerte, Z critiques
- Je peux filtrer par statut de conformité
- Je peux exporter un rapport PDF "Conformité flotte au {date}"

---

### US5 - Ajout rapide d'un nouveau véhicule
**En tant que** gestionnaire
**Je veux** ajouter un nouveau camion en 30 secondes
**Afin de** ne pas perdre de temps sur la saisie

**Critères d'acceptation :**
- Formulaire simple avec seulement champs essentiels
- Autocomplete sur marque/modèle si possible
- Je peux uploader la carte grise en même temps (optionnel)
- Le véhicule apparaît immédiatement dans ma liste

---

### US6 - Migration depuis Excel/classeur papier
**En tant que** gestionnaire de flotte de 30 véhicules
**Je veux** importer toute ma flotte depuis mon fichier Excel existant
**Afin de** ne pas avoir à ressaisir manuellement 30 véhicules (1h de travail économisée)

**Contexte :**
J'utilise actuellement un fichier Excel avec mes 30 camions (colonnes : immat, marque, modèle, année, conducteur). Je veux passer à FlotteBox sans tout ressaisir.

**Critères d'acceptation :**
1. Je télécharge le template CSV depuis FlotteBox
2. Je copie-colle mes données Excel dans le template CSV
3. Je sauvegarde en UTF-8 (warning affiché si mauvais encodage)
4. J'uploade le fichier
5. Je vois un tableau de preview avec mes 30 véhicules
6. Le système détecte 2 erreurs :
   - Ligne 12 : format d'immatriculation invalide
   - Ligne 25 : année manquante
7. Je corrige mon fichier Excel, re-exporte en CSV, re-uploade
8. Preview OK : "30 véhicules prêts à être importés, 15 conducteurs seront créés"
9. Je clique "Importer"
10. 5 secondes plus tard : "Succès ! 30 véhicules et 15 conducteurs importés."
11. Les 15 conducteurs reçoivent un email d'invitation automatique
12. Je peux immédiatement commencer à uploader des documents

**Valeur ajoutée :**
- **Temps économisé** : 1h de saisie manuelle → 5 minutes d'import
- **Pas de friction** : migration fluide depuis Excel
- **Onboarding rapide** : prêt à l'emploi en 10 minutes

---

### US7 - Mise à jour en masse des assurances (Post-MVP)
**En tant que** gestionnaire de flotte de 50 camions
**Je veux** mettre à jour toutes les assurances de ma flotte en une seule opération
**Afin de** ne pas passer 45 minutes à uploader 50 documents un par un

**Contexte :**
C'est le 1er janvier, mon assureur m'envoie 50 nouvelles attestations d'assurance (renouvellement annuel) dans un fichier ZIP. Tous mes véhicules ont déjà leurs anciennes assurances dans FlotteBox.

**Workflow actuel (sans import intelligent) :**
1. Je dézippe les 50 PDF
2. J'ouvre le premier PDF : "Assurance_AB123CD.pdf"
3. Je cherche le véhicule AB-123-CD dans FlotteBox
4. J'ouvre sa fiche
5. Je clique "Ajouter document"
6. J'uploade le PDF
7. Je sélectionne "Assurance"
8. Je remplis les dates
9. Je valide
10. Je répète 50 fois...
→ **Temps : 45 minutes**
→ **Erreurs possibles** : mauvais document sur mauvais véhicule, dates inversées

**Workflow avec import intelligent OCR :**
1. Je drag & drop le fichier ZIP (50 PDF) sur FlotteBox
2. Je clique "Analyser avec OCR" (coût : 5€)
3. 20 secondes plus tard, preview :
   ```
   ✅ 50 documents détectés
   ✅ Type : Assurance (détecté automatiquement)
   ✅ 50 véhicules trouvés en base de données

   Modifications prévues :
   - AB-123-CD : remplacement assurance (expire 31/12/2024 → 31/12/2025)
   - CD-456-EF : remplacement assurance (expire 31/12/2024 → 31/12/2025)
   - GH-789-IJ : remplacement assurance (expire 31/12/2024 → 31/12/2025)
   ... (47 autres)

   ⚠️ Attention : les anciennes assurances seront archivées (historique conservé)
   ```
4. Je vérifie visuellement (scroll rapide)
5. Je clique "Valider l'import"
6. 5 secondes plus tard :
   ```
   ✅ Succès ! 50 assurances mises à jour
   📧 Email de confirmation envoyé
   ```
→ **Temps : 2 minutes**
→ **Coût : 5€**
→ **0 erreur** (matching automatique)

**Critères d'acceptation :**
1. L'OCR détecte automatiquement :
   - Type de document (assurance)
   - Immatriculation sur le document
   - Dates d'émission et d'expiration
2. Le système trouve le véhicule correspondant en BDD
3. Le système compare avec l'assurance existante
4. Si la nouvelle est plus récente → propose le remplacement
5. Mode preview obligatoire avant validation
6. Les anciennes assurances sont archivées (pas supprimées)
7. Les alertes sont recalculées automatiquement
8. Email de confirmation avec résumé envoyé

**Valeur ajoutée :**
- **Gain de temps : 45 minutes → 2 minutes** (95% de temps économisé)
- **Fiabilité : 0 erreur** (pas de risque d'interversion)
- **Traçabilité : historique complet** (ancien + nouveau document conservés)
- **ROI immédiat** : 5€ pour économiser 45 min de travail (valorisé à 15-20€)

**Impact business :**
- **Argument de vente massif** : "Mettez à jour 50 documents en 2 minutes"
- **Réduction du churn** : évite que les clients quittent FlotteBox pour des outils plus puissants
- **Revenus récurrents** : chaque renouvellement annuel = 5€ supplémentaires

---

## 7. Wireframes & Maquettes

### 7.1 Desktop - Dashboard Homepage

```
┌────────────────────────────────────────────────────────────┐
│ [Logo] FlotteBox         Véhicules  Documents  Alertes     │
│                                                   [Avatar ▼]│
├────────────────────────────────────────────────────────────┤
│                                                            │
│  Tableau de bord                                          │
│                                                            │
│  ┌──────────┐  ┌──────────┐  ┌──────────┐  ┌──────────┐ │
│  │  Total   │  │ Conformes│  │ Alertes  │  │ Critiques│ │
│  │    45    │  │    38    │  │    5     │  │    2     │ │
│  │ véhicules│  │    ✅    │  │    ⚠️    │  │    ❌    │ │
│  └──────────┘  └──────────┘  └──────────┘  └──────────┘ │
│                                                            │
│  Véhicules nécessitant une attention                      │
│  ┌────────────────────────────────────────────────────┐  │
│  │ 🚚 AB-123-CD  Renault Master                       │  │
│  │    ❌ CT expiré depuis 3 jours                     │  │
│  │    ⚠️ Assurance expire dans 12 jours              │  │
│  │    [Voir détails]                                  │  │
│  ├────────────────────────────────────────────────────┤  │
│  │ 🚛 CD-456-EF  Mercedes Actros                      │  │
│  │    ⚠️ CT expire dans 28 jours                     │  │
│  │    [Voir détails]                                  │  │
│  └────────────────────────────────────────────────────┘  │
│                                                            │
│  Prochaines échéances (30 jours)                          │
│  ┌────────────────────────────────────────────────────┐  │
│  │ Calendrier mini avec points sur les dates clés     │  │
│  └────────────────────────────────────────────────────┘  │
│                                                            │
└────────────────────────────────────────────────────────────┘
```

### 7.2 Desktop - Liste véhicules

```
┌────────────────────────────────────────────────────────────┐
│ Véhicules (45)                                             │
│                                                            │
│ [🔍 Rechercher...]  [Type ▼] [Statut ▼]  [+ Nouveau]     │
│                                                            │
│ ┌─────────┬─────────────────┬──────────┬─────────────┐   │
│ │ Photo   │ Véhicule        │ Type     │ Statut      │   │
│ ├─────────┼─────────────────┼──────────┼─────────────┤   │
│ │ [🚚]    │ AB-123-CD       │ Camion   │ ✅ Conforme │   │
│ │         │ Renault Master  │          │             │   │
│ ├─────────┼─────────────────┼──────────┼─────────────┤   │
│ │ [🚛]    │ CD-456-EF       │ PL       │ ⚠️ 2 alertes│   │
│ │         │ Mercedes Actros │          │             │   │
│ ├─────────┼─────────────────┼──────────┼─────────────┤   │
│ │ [🚐]    │ GH-789-IJ       │ Remorque │ ❌ Critique │   │
│ │         │ Schmitz         │          │             │   │
│ └─────────┴─────────────────┴──────────┴─────────────┘   │
│                                                            │
│ < 1 2 3 ... 5 >                                           │
└────────────────────────────────────────────────────────────┘
```

### 7.3 Mobile PWA - Homepage

```
┌─────────────────────┐
│ FlotteBox      [👤] │
├─────────────────────┤
│                     │
│  Mes véhicules      │
│                     │
│  ┌───────────────┐  │
│  │ 🚚 AB-123-CD  │  │
│  │ Renault Master│  │
│  │               │  │
│  │ ✅ Conforme   │  │
│  └───────────────┘  │
│                     │
│  ┌───────────────┐  │
│  │ 🚛 CD-456-EF  │  │
│  │ Mercedes      │  │
│  │               │  │
│  │ ⚠️ 2 alertes  │  │
│  └───────────────┘  │
│                     │
│  [+ Ajouter]        │
│                     │
│                     │
├─────────────────────┤
│ 🚚   📷   🔔   👤  │
│ Home Scan Alert Me │
└─────────────────────┘
```

### 7.4 Mobile PWA - Scan document

```
┌─────────────────────┐
│ Scanner             │
│         [✕]         │
├─────────────────────┤
│                     │
│  ┌───────────────┐  │
│  │               │  │
│  │   [CAMERA]    │  │
│  │               │  │
│  │  Vue caméra   │  │
│  │  en temps réel│  │
│  │               │  │
│  │               │  │
│  └───────────────┘  │
│                     │
│  Placez le document │
│  dans le cadre      │
│                     │
│     [📸 Capturer]   │
│                     │
│  [Galerie]  [Flash] │
│                     │
└─────────────────────┘
```

---

