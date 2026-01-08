---
stepsCompleted: [1, 2]
inputDocuments:
  - _bmad-output/prd.md
workflowType: 'ux-design'
lastStep: 2
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
