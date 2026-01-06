## 1. Présentation du projet

### 1.1 Contexte

**Cible principale :** PME et ETI de tous secteurs possédant une flotte de 10 à 200 véhicules, qui n'ont pas encore investi dans une solution de gestion de flotte complète (trop complexe, trop cher, ou inadaptée à leurs besoins).

**Secteurs concernés :**
- **Transport & Logistique** : transporteurs routiers, commissionnaires, affréteurs, coursiers
- **BTP & Construction** : entreprises de gros œuvre, travaux publics, démolition (camions, VUL, engins de chantier)
- **Artisans** : plombiers, électriciens, menuisiers, paysagistes avec flottes de camionnettes
- **Services** : entreprises de nettoyage, maintenance, sécurité, livraison à domicile
- **Distribution** : grossistes, distributeurs avec flottes de livraison
- **Collectivités locales** : mairies, communautés de communes, services techniques municipaux
- **Location de véhicules** : loueurs courte/moyenne durée, LLD professionnelle
- **Agriculture** : coopératives, exploitations avec tracteurs et engins agricoles
- **Déménagement** : entreprises de déménagement et garde-meubles

**Exemple type :** Une entreprise BTP de 50 employés avec 15-20 véhicules (camions bennes, fourgons, utilitaires, engins) qui gère actuellement ses documents dans des classeurs au bureau et n'a aucune visibilité sur les échéances à venir.

Ces entreprises gèrent manuellement les documents administratifs de leurs flottes (cartes grises, assurances, contrôles techniques, permis) via des classeurs physiques, Excel ou des dossiers partagés non sécurisés. Cette gestion manuelle entraîne :
- Risques d'oubli d'échéances (CT, assurances) → amendes (135€ à 3 750€) et immobilisations de véhicules
- Perte de temps administratif significative (recherche de documents, classement, photocopies, envois par email)
- Absence de vision centralisée sur l'état de conformité de la flotte
- Risques de sécurité : documents sensibles accessibles sans contrôle, risques de vol/détournement/perte
- Non-conformité RGPD potentielle (données personnelles des conducteurs mal protégées)
- Stress lors des contrôles ou audits (inspections du travail, URSSAF, clients donneurs d'ordres)

### 1.2 Solution proposée

Une application web/mobile SaaS permettant de :
- Centraliser tous les documents administratifs des véhicules
- Recevoir des alertes automatiques avant les échéances
- Scanner et uploader des documents depuis mobile (caméra)
- Avoir une vision temps réel de la conformité de la flotte
- Générer des exports pour la comptabilité

### 1.3 Valeur ajoutée

**Gains opérationnels :**

**Gain de temps** : 20-30 minutes économisées par document traité (workflow complet : réception papier du chauffeur, scan, classement, saisie dans Excel, vérification mensuelle, recherche à la demande)

**Calcul par taille de flotte** (base : 6 documents/véhicule/an en moyenne) :

| Flotte | Documents/an | Temps économisé | Valeur* | Coût annuel** | ROI |
|--------|--------------|-----------------|---------|---------------|-----|
| 10 véh. | 60 docs | 20-30h/an | **400-600€/an** | 510€/an | 78-118% |
| 25 véh. | 150 docs | 50-75h/an | **1 000-1 500€/an** | 1 122€/an | 89-134% |
| 50 véh. | 300 docs | 100-150h/an | **2 000-3 000€/an** | 2 142€/an | 93-140% |
| 100 véh. | 600 docs | 200-300h/an | **4 000-6 000€/an** | 3 672€/an | 109-163% |

*Valorisé au coût horaire chargé d'une assistante administrative : **20€/h** (salaire brut 2 000€/mois + charges 45%)
**Avec engagement annuel (-15%)

**Note importante** : Ce calcul ne prend en compte QUE le temps de l'assistante administrative. S'ajoutent :
- Temps du dirigeant lors de problèmes (recherche doc, gestion amende) : **80-150€/h**
- Temps des chauffeurs (retours au bureau pour déposer papiers, recherche de docs) : **30-40€/h**

**Réduction des risques légaux** :
- **1 amende CT périmé évitée** (135€) = 3 mois d'abonnement pour 10 véhicules
- **1 amende assurance non valide évitée** (3 750€) = 10 mois d'abonnement pour 100 véhicules
- **1 immobilisation de véhicule évitée** (500-2 000€ de manque à gagner selon durée)
- **1 refus de chantier pour véhicule non conforme** (perte de CA)

**Exemple concret - Entreprise BTP 50 véhicules** :
- Coût annuel : 2 142€ (tarif dégressif avec engagement annuel)
- Gain temps secrétaire : 2 000-3 000€/an
- Évite en moyenne 1 amende/an : +135€
- Évite 1 immobilisation : +500€
- **ROI total : 118-170%** (l'outil se paie tout seul)

**Simplicité** :
- Solution **10× plus simple** et accessible qu'un TMS complet
- **5× moins cher** qu'un logiciel de gestion de flotte traditionnel
- Prise en main en **moins de 30 minutes**
- Interface pensée pour des non-techniciens

**Sécurité et conformité :**
- **Protection des données sensibles** : 
  - Stockage sécurisé et chiffré des documents (cartes grises, permis, données personnelles conducteurs)
  - Accès contrôlé par utilisateur avec traçabilité complète (qui a consulté/modifié quoi et quand)
  - Prévention des fuites : plus de documents sensibles dans des dossiers partagés non sécurisés
  - Protection contre le vol/détournement : impossible de copier en masse ou exfiltrer les données
  
- **Conformité RGPD** :
  - Isolation stricte des données par entreprise (multi-tenant sécurisé)
  - Gestion des consentements et droits des personnes (accès, rectification, suppression)
  - Traçabilité des accès aux données personnelles
  - Hébergement données en Europe (Supabase EU)
  - Documentation des mesures de sécurité pour audits

**Sérénité managériale :**
- Vision temps réel de la conformité légale de la flotte
- Prêt pour tout contrôle ou audit (URSSAF, inspection du travail, DGCCRF)
- Preuve documentaire horodatée et inaltérable

---

## 2. Objectifs du projet

### 2.1 Objectifs business

**Phase Bêta (Mois 0-2)** :
- Recruter **10 entreprises bêta-testeuses** (profils variés : BTP, transport, artisans)
- Accès **gratuit** pendant 2 mois (quelle que soit la taille de la flotte)
- En échange : feedback régulier, remontée de bugs, participation à des appels de suivi hebdomadaires
- Objectif : valider l'UX, identifier les bugs critiques, affiner le product-market fit
- Condition : engagement de tester activement (minimum 5 véhicules + 10 documents uploadés/mois)
- Durée bêta : **2 mois** → puis passage en offre payante avec réduction de lancement (-50% pendant 12 mois)

**Court terme (6 mois)** :
- Convertir les 10 bêta-testeurs en clients payants
- Recruter 5-10 nouveaux clients payants
- Générer 2000-5000€ MRR
- Valider le pricing et le product-market fit

**Moyen terme (12 mois)** :
- Atteindre 30-50 clients
- 10 000-15 000€ MRR
- Structurer les process de support et onboarding

**Long terme (18-24 mois)** :
- 100+ clients
- 30 000€+ MRR
- Équipe de 2-3 personnes

### 2.2 Programme Bêta-Test

#### 2.2.1 Critères de sélection des bêta-testeurs

**Profils recherchés :**
- **3 entreprises BTP** (15-50 véhicules) : camions, VUL, engins de chantier
- **3 entreprises Transport/Logistique** (20-100 véhicules) : poids lourds, remorques
- **2 artisans multi-équipés** (10-20 véhicules) : plombiers, électriciens, paysagistes
- **1 collectivité locale** (25-50 véhicules) : services techniques municipaux
- **1 entreprise de services** (15-30 véhicules) : nettoyage, maintenance, livraison

**Critères de sélection :**
- Gestion actuelle manuelle (Excel, classeurs papier)
- Motivé pour tester une solution digitale
- Disponible pour des points hebdomadaires (30 min/semaine)
- Équipe de 1-2 personnes identifiées pour utiliser l'outil
- Engagement écrit (contrat bêta-test signé)

#### 2.2.2 Engagement des bêta-testeurs

**Contreparties offertes :**
- Accès gratuit pendant 2 mois (valeur : 50-300€/mois selon taille flotte)
- Réduction -50% pendant 12 mois après la bêta (valeur : 300-1 800€)
- Statut de "Client Fondateur" (badge + remerciements publics si accord)
- Accès prioritaire aux nouvelles fonctionnalités
- Ligne directe avec le fondateur (Quentin)

**Engagements demandés :**
- Ajouter minimum **2 véhicules** dans les 7 premiers jours
- Uploader minimum **5 documents** par mois
- Participer à un **call hebdomadaire de 30 min** (retour d'expérience)
- Remonter les bugs via le widget feedback in-app (réponse sous 24h garantie)
- Répondre à un **questionnaire de satisfaction** à J+15, J+30, J+60
- Accepter d'être éventuellement cité comme client référence (si accord)

**Contrat bêta-test (points clés) :**
- Durée : 2 mois à partir de la date d'activation du compte
- Confidentialité : NDA mutuel (les testeurs voient une version non finalisée)
- Données : les données sont réelles et protégées (RGPD appliqué)
- Résiliation : possible à tout moment par les 2 parties sans pénalité
- Conversion : à l'issue, proposition de passage en payant (-50% pendant 12 mois)

#### 2.2.3 Suivi et métriques du programme bêta

**KPIs à suivre pendant la bêta :**
- **Taux d'activation** : % de testeurs ayant ajouté leur 1er véhicule (objectif : 100%)
- **Taux d'adoption** : % ayant uploadé au moins 5 documents (objectif : 90%)
- **Engagement** : nombre moyen de connexions/semaine (objectif : 3+)
- **NPS (Net Promoter Score)** : à J+30 et J+60 (objectif : >40)
- **Bugs critiques** : nombre de bugs bloquants remontés (objectif : <5)
- **Taux de conversion** : % de testeurs passant en payant après la bêta (objectif : 80%+)

**Format des appels de suivi :**
- Call individuel hebdomadaire (30 min par testeur)
- Questions clés :
  - Qu'avez-vous testé cette semaine ?
  - Quelles difficultés avez-vous rencontrées ?
  - Quelles fonctionnalités manquent selon vous ?
  - Sur une échelle de 1 à 10, recommanderiez-vous FlotteBox ?
  - Seriez-vous prêt à payer [prix] pour cet outil ?

**Planning bêta :**
- **Semaine 0** : Recrutement des 10 testeurs (LinkedIn, réseau, cold outreach)
- **Semaine 1** : Onboarding individualisé (visio 1h par testeur)
- **Semaines 2-8** : Période de test active (calls hebdomadaires)
- **Semaine 9** : Bilan final + questionnaire de satisfaction + pitch conversion
- **Semaine 10** : Conversion en clients payants (objectif : 8/10 convertis)

### 2.3 Marché adressable (TAM)

**France - Flottes professionnelles tous secteurs :**

**Transport & Logistique :**
- ~37 000 entreprises de transport
- ~600 000 poids lourds + ~450 000 VUL

**BTP & Construction :**
- ~580 000 entreprises du BTP (dont ~50 000 avec >10 salariés)
- ~1,2 million de véhicules professionnels (VUL + engins)
- Parc engins : ~500 000 unités

**Artisans multi-équipés :**
- ~1,3 million d'entreprises artisanales
- ~30 000 avec flottes de 10+ véhicules

**Services & Distribution :**
- ~200 000 entreprises de services avec flottes
- ~500 000 véhicules

**Collectivités locales :**
- ~35 000 communes
- ~180 000 véhicules municipaux

**Agriculture :**
- ~400 000 exploitations
- Cible : moyennes/grandes exploitations (~50 000)

**TOTAL MARCHÉ ADRESSABLE :**
- **150 000 à 200 000 structures** avec flottes de 10+ véhicules
- **4-5 millions de véhicules** professionnels à gérer

**Segmentation par taille de flotte :**
- 40% ont 10-25 véhicules → **cœur de cible initial** (plan Starter/Business)
- 35% ont 25-50 véhicules → **sweet spot rentabilité** (Business/Pro)
- 20% ont 50-200 véhicules → **high-value accounts** (Pro/Enterprise)
- 5% ont 200+ véhicules → **grands comptes** (Enterprise sur mesure)

**Stratégie de pénétration par phase :**
1. **Phase 1 (0-12 mois)** : Focus Transport & BTP (secteurs les plus réglementés, douleur immédiate)
2. **Phase 2 (12-24 mois)** : Artisans & Services (via partenariats chambres des métiers, assureurs)
3. **Phase 3 (24-36 mois)** : Collectivités & Agriculture (cycles de vente plus longs)

### 2.4 Modèle économique et pricing

#### 2.4.1 Structure tarifaire

**Tarification unique et simple : au véhicule avec dégressivité**

### Pricing par véhicule (engagement mensuel)

| Nombre de véhicules | Prix par véhicule | Exemple de prix total mensuel |
|---------------------|-------------------|-------------------------------|
| 1-10 véhicules | **5€/véhicule/mois** | 10 véh. = **50€/mois** |
| 11-50 véhicules | **4€/véhicule/mois** | 25 véh. = **110€/mois** |
| 51+ véhicules | **3€/véhicule/mois** | 100 véh. = **360€/mois** |

**Fonctionnalités incluses (tous les clients) :**
- ✓ Gestion complète des documents (cartes grises, assurances, contrôles techniques, permis)
- ✓ Alertes automatiques avant les échéances par email
- ✓ Upload documents depuis mobile (scan photo)
- ✓ 1 compte principal (gestion complète) + comptes chauffeurs (consultation + scan documents)
- ✓ Stockage sécurisé et chiffré
- ✓ Export Excel/PDF
- ✓ Support par email
- ✓ Conformité RGPD

**Exemples de tarification mensuelle :**

| Flotte | Calcul | Prix mensuel | Prix annuel (-15%) |
|--------|--------|--------------|---------------------|
| 5 véh. | 5 × 5€ | **25€/mois** | **255€/an** (21€/mois) |
| 10 véh. | 10 × 5€ | **50€/mois** | **510€/an** (43€/mois) |
| 25 véh. | 10×5€ + 15×4€ | **110€/mois** | **1 122€/an** (94€/mois) |
| 50 véh. | 10×5€ + 40×4€ | **210€/mois** | **2 142€/an** (179€/mois) |
| 100 véh. | 10×5€ + 40×4€ + 50×3€ | **360€/mois** | **3 672€/an** (306€/mois) |
| 200 véh. | 10×5€ + 40×4€ + 150×3€ | **660€/mois** | **6 732€/an** (561€/mois) |

#### 2.4.2 Tarification annuelle

**Engagement 12 mois : -15% sur le prix total**

Le calcul reste identique avec les paliers dégressifs, puis application de -15% sur le montant annuel.

#### 2.4.3 Options complémentaires

**Add-ons prioritaires (MVP) :**
- **OCR automatique** : 0,10€/document scanné
  - Permet la création de véhicules depuis scan de carte grise
  - Permet l'import intelligent de documents en masse
  - Facturation à l'usage (pay-as-you-go)
  - Disponible pour tous les clients

- **Verbalisation (API ANTAI)** : 1€/véhicule/mois
  - Gestion automatisée des PV et contraventions
  - Alertes de paiement avant majoration
  - Suivi des contestations
  - Synchronisation avec le système ANTAI
  - Exemples : 10 véh. = 10€/mois | 25 véh. = 25€/mois | 100 véh. = 100€/mois

**Add-ons post-MVP :**
- **API access** : 49€/mois (pour intégrations tierces)
- **Export comptable avancé** : 29€/mois (formats personnalisés)
- **Support téléphone prioritaire** : 99€/mois
- **Onboarding personnalisé** : 299€ one-time (formation 2h sur site/visio)

#### 2.4.4 Stratégie de conversion

**Essai gratuit avec auto-renouvellement :**
- **14 jours d'essai gratuit** sur tous les plans
- **Carte bancaire obligatoire** à l'inscription (mais pas de débit immédiat)
- Message clair affiché : "✅ Essai gratuit 14 jours, puis [Prix]€/mois. ❌ Annulez à tout moment."
- Accès complet aux fonctionnalités du plan choisi pendant la période d'essai
- À J-3 : email de rappel "Vous serez débité le [date] si vous ne résiliez pas"
- À J-1 : email de rappel final + notification in-app
- À J-14 : **débit automatique** du premier mois → passage en statut "active"
- Si annulation avant J-14 : pas de débit, compte désactivé, données conservées 30 jours
- Pas de plan gratuit permanent (stratégie 100% payante)

**Pourquoi carte bancaire obligatoire :**
- **Meilleur taux de conversion** : 60-80% au lieu de 20-30% (sans CB)
- **Qualification forte** : les utilisateurs qui donnent leur CB sont engagés
- **Automatisation** : pas de friction à la fin de l'essai (conversion automatique)
- **Réduction du churn** : friction au début = clients mieux qualifiés qui restent
- **Standard SaaS B2B** : modèle utilisé par Notion, Slack, HubSpot, etc.

**Pourquoi pas de plan gratuit :**
- Évite les utilisateurs non qualifiés (freelances avec 1-2 véhicules)
- Force la qualification : seuls les vrais besoins (10+ véhicules) convertissent
- Simplifie la grille tarifaire
- Meilleur LTV : tous les clients génèrent du revenu
- Focus sur le support des clients payants

**Upsell naturel :**
- Client démarre avec 10 véhicules (50€/mois)
- Atteint 11 véhicules → passage automatique au palier dégressif (4€/véh)
- Notification : "Vous bénéficiez désormais du tarif dégressif !"
- Ajout de véhicules illimité depuis le dashboard

**Incentives engagement annuel :**
- -15% sur annuel (affiché)
- 2 mois offerts si présentation annuelle
- Paiement flexible : 3× ou 4× sans frais (via LemonSqueezy)

#### 2.4.5 Positionnement concurrentiel

**Comparaison avec alternatives :**

| Solution | Prix indicatif | Complexité | Cible |
|----------|----------------|------------|-------|
| **Excel + classeurs** | 0€ (temps perdu) | Faible | Tous |
| **FlotteBox** | 25-660€/mois (3-5€/véh.) | Très faible | PME 5-200 véh. |
| **TMS léger** (ex: Dashdoc) | 100-300€/mois | Moyenne | Transport 20-100 véh. |
| **TMS complet** (ex: Shiptify) | 500-2000€/mois | Élevée | Transport 50+ véh. |
| **ERP avec module flotte** | 1000-5000€/mois | Très élevée | Grands groupes 200+ |

**Avantages FlotteBox :**
- **5-10× moins cher** qu'un TMS complet
- **20× plus simple** qu'un ERP
- **Pricing transparent et prédictible** : pas de plans complexes
- **ROI immédiat** : 1 amende évitée = plusieurs mois d'abonnement payés

#### 2.4.6 Projections de revenus

**Hypothèses de conversion :**
- **Taux de conversion trial → payant** : 60-70% (grâce à la CB obligatoire)
- **Taux de churn mensuel** : 3-5%
- **ARPU moyen** : 120€/mois (basé sur flotte moyenne de 25-30 véhicules)

**Hypothèses conservatrices (taux conversion 60%) :**

| Mois | Inscriptions | Conversions | Clients actifs | MRR | ARR |
|------|--------------|-------------|----------------|-----|-----|
| M3 (fin MVP) | 10 | 6 | 5 | 750€ | 9 000€ |
| M6 | 30 | 18 | 15 | 2 250€ | 27 000€ |
| M12 | 80 | 48 | 42 | 6 300€ | 75 600€ |
| M18 | 150 | 90 | 78 | 11 700€ | 140 400€ |
| M24 | 250 | 150 | 130 | 19 500€ | 234 000€ |

**Hypothèses optimistes (taux conversion 70% + croissance rapide) :**

| Mois | Inscriptions | Conversions | Clients actifs | MRR | ARR |
|------|--------------|-------------|----------------|-----|-----|
| M3 | 15 | 11 | 10 | 1 500€ | 18 000€ |
| M6 | 50 | 35 | 32 | 4 800€ | 57 600€ |
| M12 | 150 | 105 | 95 | 14 250€ | 171 000€ |
| M18 | 300 | 210 | 185 | 27 750€ | 333 000€ |
| M24 | 500 | 350 | 310 | 46 500€ | 558 000€ |

**Note :** Les "Clients actifs" sont inférieurs aux "Conversions cumulées" à cause du churn (3-5%/mois).

**Mix clients type (M12 - scénario conservateur) :**
- 15 clients avec 5-10 véh. (moyenne 40€/mois) : **600€/mois**
- 15 clients avec 15-25 véh. (moyenne 85€/mois) : **1 275€/mois**
- 8 clients avec 30-50 véh. (moyenne 170€/mois) : **1 360€/mois**
- 3 clients avec 75-100 véh. (moyenne 280€/mois) : **840€/mois**
- 1 client avec 150+ véh. (~500€/mois) : **500€/mois**
- **Total : 42 clients = 4 575€ MRR** (54 900€ ARR)
- **ARPU réel : 109€/mois**

**Impact du modèle avec CB obligatoire :**
- **Avant (sans CB)** : 20-30% conversion → 80 inscriptions = 20 clients payants
- **Après (avec CB)** : 60-70% conversion → 80 inscriptions = 48 clients payants
- **Gain : +140% de clients payants** pour le même nombre d'inscriptions

#### 2.4.7 Churn et lifetime value

**Objectifs :**
- **Churn mensuel** : < 5% (objectif < 3% après M12)
- **LTV (Lifetime Value) par segment** :
  - Petites flottes (5-10 véh., 40€/mois) : 40€ × 18 mois = **720€**
  - Moyennes flottes (15-30 véh., 100€/mois) : 100€ × 24 mois = **2 400€**
  - Grandes flottes (50-100 véh., 250€/mois) : 250€ × 30 mois = **7 500€**
  - Très grandes flottes (100+ véh., 500€/mois) : 500€ × 36 mois = **18 000€**

**CAC (Customer Acquisition Cost) cible :**
- Petites flottes : < 120€ (LTV:CAC = **6,0:1** ✅)
- Moyennes flottes : < 400€ (LTV:CAC = **6,0:1** ✅)
- Grandes flottes : < 1 250€ (LTV:CAC = **6,0:1** ✅)
- Très grandes flottes : < 3 000€ (LTV:CAC = **6,0:1** ✅)

**Canaux d'acquisition :**
1. **Inbound** (SEO, content marketing) : CAC ~50-100€
2. **Partenariats** (assureurs, experts-comptables, chambres des métiers) : CAC ~100-200€
3. **Outbound** (LinkedIn, cold email) : CAC ~200-400€
4. **Référencement** (parrainage : 1 mois offert) : CAC ~0€

### 2.2 Objectifs techniques

- Time-to-market : **MVP en 6 semaines**
- Performance : temps de chargement < 2s
- Disponibilité : 99.5% uptime
- Sécurité : conformité RGPD, données isolées par entreprise
- Scalabilité : architecture supportant 500+ clients

---

