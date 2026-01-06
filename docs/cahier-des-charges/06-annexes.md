## 3. Périmètre fonctionnel

### 3.1 Utilisateurs cibles

**Persona 1 : Gestionnaire de flotte / Responsable administratif**
- Âge : 30-55 ans
- Secteurs : transport, BTP, services, artisans
- Taille flotte : 10-200 véhicules
- Besoins : vision globale, alertes proactives, export comptable, preuve de conformité pour audits
- Douleurs : "Je perds 1 journée par mois à chercher des documents et vérifier les dates", "J'ai peur d'oublier un CT et de prendre une amende"
- Usage : desktop principalement, mobile occasionnel (scan rapide)

**Persona 2 : Chauffeur / Conducteur / Ouvrier**
- Âge : 25-60 ans
- Secteurs : tous
- Besoins : consulter ses véhicules, scanner un document rapidement (nouveau CT, constat), pas de complexité
- Douleurs : "On me demande toujours de ramener des papiers au bureau", "Je sais pas si mon camion est à jour"
- Usage : 100% mobile (sur chantier, en livraison, sur la route)

**Persona 3 : Dirigeant PME**
- Âge : 40-65 ans
- Secteurs : BTP, artisans, services
- Besoins : sérénité (conformité légale assurée), pas de surprise (amendes, immobilisations), vision macro sur les coûts
- Douleurs : "J'ai déjà pris 2 amendes pour CT périmé cette année", "Je veux dormir tranquille"
- Usage : dashboard synthétique, rapports mensuels

**Persona 4 : Assistante administrative / Comptable**
- Âge : 25-50 ans
- Secteurs : tous
- Besoins : saisie documents, suivi échéances quotidien, archivage numérique pour compta
- Douleurs : "Je passe des heures à scanner et classer", "Les conducteurs perdent les papiers"
- Usage : desktop + mobile (scan documents)

### 3.2 Types de véhicules supportés

**Véhicules routiers :**
- Poids lourds (porteurs, tracteurs, semi-remorques)
- Camionnettes et VUL (véhicules utilitaires légers)
- Fourgons et utilitaires
- Véhicules frigorifiques
- Remorques et semi-remorques
- Voitures de société

**Engins et matériel BTP :**
- Engins de chantier (pelleteuses, bulldozers, chargeuses)
- Nacelles et plateformes élévatrices
- Grues mobiles et grues de chantier
- Compacteurs et rouleaux
- Bétonnières portées
- Camions-bennes et dumpers

**Matériel agricole :**
- Tracteurs agricoles
- Moissonneuses-batteuses
- Pulvérisateurs et épandeurs
- Matériel de récolte

**Véhicules spécialisés :**
- Véhicules sanitaires (ambulances)
- Véhicules de sécurité/intervention
- Camions citernes
- Bennes à ordures
- Balayeuses de voirie

**Note :** Pour chaque type, les documents obligatoires peuvent varier (ex: certificat ADR pour transport de matières dangereuses, certificat de conformité CE pour engins de chantier). L'application permet d'ajouter des types de documents personnalisés selon les besoins métier.

### 3.3 Types de documents gérés

**Documents véhicule :**
- Carte grise (certificat d'immatriculation)
- Assurance véhicule
- Contrôle technique
- Certificat de conformité
- Contrat de location/leasing (si applicable)
- Factures d'entretien

**Documents conducteur :**
- Permis de conduire (B, C, CE, etc.)
- FIMO / FCO
- Carte de qualification conducteur
- Visite médicale

**Autres documents :**
- Documents libres (factures, PV, constats)

---


## 12. Risques & Mitigation

| Risque | Probabilité | Impact | Mitigation |
|--------|-------------|--------|------------|
| Difficulté technique PWA caméra iOS | Moyenne | Élevé | Tests très tôt, fallback upload fichier |
| Supabase Storage lent | Faible | Moyen | Compression images, CDN, migration S3 si besoin |
| Clients veulent features complexes | Élevée | Moyen | Roadmap claire, priorisation MVP strict |
| Concurrent arrive sur le marché | Moyenne | Élevé | Time-to-market, focus client ancre |
| Changement pricing | Moyenne | Faible | A/B testing, feedback continu |

---

## 13. Prochaines étapes (Post-MVP)

### Phase 2 features (Mois 3-6)

**P1 - Priorité haute :**
- Multi-utilisateurs avec rôles granulaires
- Export Excel/PDF personnalisable
- Historique modifications (audit log)
- **Import intelligent de documents en masse avec OCR** (killer feature)
- Intégration comptabilité (Pennylane, QuickBooks)
- Notifications push natives (PWA)

**P2 - Priorité moyenne :**
- OCR automatique sur upload unitaire (extraction données carte grise)
- API publique (pour partenaires)
- Tableau de bord analytique avancé
- Import masse de documents sans OCR (ZIP + CSV métadonnées)
- Templates d'alertes personnalisables

**P3 - Nice to have :**
- App React Native (si vraiment demandé)
- Mode hors ligne complet (édition)
- Intégrations GPS (Webfleet, Masternaut)
- QR codes sur véhicules (scan rapide)
- Signature électronique documents

---

## 14. Critères de succès MVP

**Critères techniques :**
- ✅ Application déployée et accessible 24/7
- ✅ Temps de réponse < 2s sur toutes les pages
- ✅ 0 bug critique
- ✅ PWA installable sur iOS + Android

**Critères business :**
- ✅ Client ancre (100 véhicules) en production
- ✅ 3-5 autres clients testeurs
- ✅ Taux de satisfaction > 8/10
- ✅ Temps d'onboarding < 30 minutes

**Critères produit :**
- ✅ Un gestionnaire peut ajouter un véhicule en < 1 minute
- ✅ Upload document depuis mobile en < 30 secondes
- ✅ Alertes envoyées quotidiennement sans intervention

---

## 15. Contact & Suivi

**Product Owner :** Quentin  
**Email :** [ton-email]  
**Repository :** [GitHub URL]  
**Suivi :** Notion board (ou Linear, Jira)

**Fréquence revue :** Hebdomadaire (chaque vendredi)

---

**Fin du cahier des charges**

*Dernière mise à jour : 1er décembre 2024*
