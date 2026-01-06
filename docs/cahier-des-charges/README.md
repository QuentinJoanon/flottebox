# Cahier des Charges - FlotteBox
## Boîte à Gants Numérique

**Version :** 1.0
**Date :** 1er janvier 2026
**Auteur :** Quentin Joanon
**Statut :** MVP - Phase de validation marché

---

## Table des matières

Ce cahier des charges a été organisé en plusieurs documents thématiques pour faciliter la navigation et la consultation :

### Documents principaux

1. **[Business & Produit](./01-business-produit.md)**
   - Présentation du projet (contexte, cible, solution)
   - Objectifs business et programme bêta-test
   - Marché adressable et modèle économique
   - Stratégie de pricing et projections financières

2. **[Fonctionnel & Features](./02-fonctionnel-features.md)**
   - Spécifications fonctionnelles détaillées
   - Modules : Authentification, Véhicules, Documents, Alertes
   - Fonctionnalités P0 (MVP) et P1/P2 (post-MVP)
   - Workflows et cas d'usage détaillés

3. **[Technique & Architecture](./03-technique-architecture.md)**
   - Stack technique (Next.js, Supabase, etc.)
   - Architecture système et base de données
   - Schémas et diagrammes
   - Exemples de code et intégrations

4. **[Gestion de Projet](./04-gestion-projet.md)**
   - Planning de développement (6 semaines)
   - Méthodologie et sprints
   - Tests & Qualité
   - Livrables, budget et ressources

5. **[User Stories & Workflows](./05-user-stories-workflows.md)**
   - User stories détaillées par persona
   - Cas d'usage concrets
   - Wireframes et maquettes
   - Parcours utilisateurs

6. **[Annexes](./06-annexes.md)**
   - Périmètre fonctionnel
   - Risques et mitigation
   - Prochaines étapes post-MVP
   - Critères de succès
   - Contact et suivi

---

## Navigation rapide

### Pour comprendre le projet
Commencez par **[Business & Produit](./01-business-produit.md)** pour saisir la vision, le marché et le modèle économique.

### Pour développer le MVP
Consultez **[Fonctionnel & Features](./02-fonctionnel-features.md)** et **[Technique & Architecture](./03-technique-architecture.md)** pour les spécifications détaillées.

### Pour planifier et organiser
Référez-vous à **[Gestion de Projet](./04-gestion-projet.md)** pour le planning, les sprints et les livrables.

### Pour designer l'UX
Explorez **[User Stories & Workflows](./05-user-stories-workflows.md)** pour comprendre les parcours utilisateurs.

---

## Résumé exécutif

**FlotteBox** est une application SaaS de gestion documentaire pour flottes de véhicules, ciblant les PME/ETI de 10 à 200 véhicules.

**Problème résolu :** Gestion manuelle chronophage des documents administratifs (cartes grises, CT, assurances) avec risques d'amendes et d'oublis.

**Solution :** Application web/mobile permettant de centraliser tous les documents, recevoir des alertes automatiques avant les échéances, et scanner des documents depuis mobile.

**Marché :** 150 000 à 200 000 structures en France (BTP, Transport, Artisans, Services, Collectivités).

**Business model :** SaaS avec abonnement mensuel de 49€ à 349€ selon la taille de flotte.

**Objectif MVP :** Lancement en 6 semaines avec 10 bêta-testeurs, conversion en clients payants sous 2 mois.

---

## Contacts

**Porteur du projet :** Quentin
**Email :** contact@objectively.fr
**Entité :** Objectively.fr
