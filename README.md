# Site Expertise Comptable — v64 (architecture multi-fichiers)

## ⚠ Changement important par rapport aux versions précédentes

Le site n'est **plus un fichier unique**. Il se compose désormais de :

```
v64/
├── index.html          ← le site (1,2 Mo, contre 2,2 Mo en v62)
├── data/               ← 15 fichiers JSON (699 Ko) : missions, normes,
│                          institutions, outils, actus, DEC, glossaire…
└── modeles/            ← 21 modèles Word (.docx) téléchargeables (256 Ko)
```

**Conséquence** : il faut ouvrir le site via un serveur HTTP, pas en
double-cliquant sur index.html (le chargement `fetch()` des JSON est
bloqué en `file://` par les navigateurs).

## Comment tester en local

1. Ouvrir le dossier `v64/` dans VS Code
2. Clic droit sur `index.html` → **Open with Live Server**

Si le site est ouvert en `file://` par erreur, un bandeau rouge
d'avertissement s'affiche en bas de page avec la marche à suivre.

## Déploiement Netlify

Glisser-déposer le dossier `v64/` entier dans Netlify (ou le connecter
à un dépôt GitHub contenant ces trois éléments à la racine).

## Ce qui a changé techniquement (v63 → v64)

- **15 structures de données** (677 Ko de JS) externalisées en JSON,
  chargées en parallèle et de façon non bloquante au démarrage.
  Un événement `data:ready` est émis quand tout est chargé.
- **21 modèles Word** extraits du base64 (285 Ko) vers `modeles/`,
  avec attribut `download` systématique. Les 7 modèles de rapport EC
  (NP 2300 → 4410) qui n'avaient pas de nom de fichier en ont reçu un.
- Le bloc « Quoi de neuf » de la page d'accueil attend `data:ready`
  avant de se peindre (aucun flash de contenu vide).
- Aucune modification de contenu : textes, seuils et fiches identiques
  à la v63 (vérifié par comparaison JSON aller-retour).

## Pour modifier le contenu désormais

- Une fiche mission / norme / institution / actu → éditer le fichier
  `data/*.json` correspondant (attention : format JSON strict, guillemets
  doubles ; les apostrophes françaises ne posent plus aucun problème ✓)
- Un modèle Word → remplacer le fichier dans `modeles/` (même nom)
- La structure des pages, le CSS, la logique JS → `index.html`
