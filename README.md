# pharma-perimes

Fichiers base de données des médicaments : https://base-donnees-publique.medicaments.gouv.fr/telechargement

Fichiers actuellement utilisés :
- Fichier des spécialités (Date de mise à jour : 28/04/2026, 3091 Ko)
- Fichier des présentations (Date de mise à jour : 22/05/2026, 4054 Ko)

# 📦 Gestion des Périmés Pharma & Parapharmacie

<p align="center">

![Version](https://img.shields.io/badge/version-1.0-blue)
![Google Apps Script](https://img.shields.io/badge/Google%20Apps%20Script-Backend-green)
![Google Sheets](https://img.shields.io/badge/Google%20Sheets-Database-success)
![Mobile Friendly](https://img.shields.io/badge/mobile-friendly-brightgreen)
![PWA Ready](https://img.shields.io/badge/PWA-ready-orange)

</p>

Application web de gestion des produits périmés destinée aux pharmacies et parapharmacies.

L'objectif est de permettre à un utilisateur de scanner rapidement les produits à retirer du stock, d'identifier automatiquement les médicaments grâce à la BDPM, de gérer les produits de parapharmacie et de centraliser l'ensemble des données dans Google Sheets sans infrastructure complexe.
Particulièrement utile si vous disposez d'une équipe de PDA et qu'une partie des médicaments est stockée à part (étagères, bacs nominatifs, etc.)

---

# 🚀 Fonctionnalités

## 📷 Scan intelligent

### DataMatrix GS1

Extraction automatique :

- GTIN
- CIP13
- Date de péremption (AI 17)

Exemple :

```text
(01)03400931234567
(17)280930
```

➡ Produit identifié automatiquement

➡ Péremption détectée :

```text
09-2028
```

---

### EAN13

Compatible avec :

- Produits de parapharmacie
- Cosmétique
- Dispositifs médicaux
- Compléments alimentaires

---

## 💊 Identification des médicaments

Recherche locale ultra-rapide dans les fichiers officiels BDPM :

```text
CIS_bdpm.txt
CIS_CIP_bdpm.txt
```

Correspondances :

```text
CIP13
 ↓
CIS
 ↓
Dénomination
```

Exemple :

```text
3400931234567
 ↓
CETIRIZINE BIOGARAN 10 mg
```

---

## 🧴 Gestion de la parapharmacie

Système de recherche hiérarchique :

```text
BDPM
 ↓
_CACHE_PRODUITS
 ↓
Recherche externe
 ↓
Saisie manuelle
```

Tout produit saisi manuellement est mémorisé automatiquement.

---

## 📅 Gestion automatique des péremptions

Création dynamique des feuilles :

```text
2028
01-2028
02-2028
03-2028

2029
01-2029
02-2029
...
```

Les produits sont automatiquement rangés dans leur mois de péremption.

---

## 🗂 Cache intelligent

Feuille dédiée :

```text
_CACHE_PRODUITS
```

Structure :

| CIP13 | CIS | Produit | DateMaj |
|---------|---------|---------|---------|

Permet :

- apprentissage progressif
- suppression des ressaisies
- amélioration continue du système

---

# 📸 Captures d'écran

## Interface mobile

```md
![Interface principale](docs/screenshots/home.png)
```

## Scan produit

```md
![Scan produit](docs/screenshots/scan.png)
```

## Gestion des dates

```md
![Dates de péremption](docs/screenshots/dates.png)
```

## Google Sheets

```md
![Google Sheets](docs/screenshots/sheets.png)
```

---

# 🎥 Démonstration

Ajouter un GIF ou une vidéo :

```md
![Démo](docs/demo/demo.gif)
```

ou :

```md
https://github.com/user-attachments/assets/demo.mp4
```

---

# 🏗 Architecture

```text
┌─────────────────────────┐
│ Smartphone / Navigateur │
└─────────────┬───────────┘
              │
              ▼
┌─────────────────────────┐
│ Interface Web HTML/JS   │
│ BarcodeDetector API     │
└─────────────┬───────────┘
              │
              ▼
┌─────────────────────────┐
│ Google Apps Script      │
│ Web App REST            │
└─────────────┬───────────┘
              │
      ┌───────┼──────────────┐
      │       │              │
      ▼       ▼              ▼

┌────────┐ ┌─────────┐ ┌────────────┐
│ BDPM   │ │ Cache   │ │ Recherche  │
│ Locale │ │ Produits│ │ Externe    │
└────────┘ └─────────┘ └────────────┘

              │
              ▼

┌─────────────────────────┐
│ Google Sheets           │
│ Gestion des périmés     │
└─────────────────────────┘
```

---

# 🔄 Flux de traitement

## Médicament

```text
Scan
 ↓
Lecture DataMatrix
 ↓
Recherche BDPM
 ↓
Produit trouvé
 ↓
Création fiche péremption
 ↓
Enregistrement
```

---

## Produit de parapharmacie

```text
Scan EAN13
 ↓
Recherche cache
 ↓
Produit trouvé ?
 ↓
Oui → Enregistrement

Non
 ↓
Saisie manuelle
 ↓
Enregistrement
 ↓
Ajout au cache
```

---

# 📂 Structure du projet

```text
/
├── index.html
├── app.js
├── style.css
│
├── data/
│   ├── CIS_bdpm.txt
│   └── CIS_CIP_bdpm.txt
│
├── docs/
│   ├── screenshots/
│   └── demo/
│
└── apps-script/
    └── Code.gs
```

---

# ⚙️ Installation

## 1. Cloner le dépôt

```bash
git clone https://github.com/votre-compte/gestion-perimes.git
```

---

## 2. Déployer Apps Script

Créer un projet Google Apps Script puis :

- copier `Code.gs`
- déployer en Web App

---

## 3. Configurer l'URL

Dans le front :

```javascript
const BASE_URL =
  "https://script.google.com/macros/s/XXXXXXXXXXXX/exec";
```

---

## 4. Héberger le front

### GitHub Pages

```text
Settings
 → Pages
 → Deploy from branch
```

ou tout autre serveur web.

---

# 📈 Évolutions prévues

## Version 1.1

- Recherche automatique parapharmacie
- Complétion des produits inconnus
- Historique des scans

## Version 1.2

- Export PDF
- Impression des listes

## Version 2.0

- Multi-utilisateurs
- Gestion multi-sites
- Synchronisation cloud

---

# 🔒 Sécurité & Confidentialité

- Aucune donnée patient
- Aucun stockage externe obligatoire
- Données conservées dans Google Sheets
- Hébergement maîtrisé

---

# 🤝 Contribution

Les contributions sont les bienvenues :

1. Fork du projet
2. Création d'une branche
3. Pull Request

---

<p align="center">
Développé pour simplifier la gestion des périmés en pharmacie et parapharmacie 💊
</p>
