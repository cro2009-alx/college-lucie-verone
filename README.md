# Collège Lucie Vérone — Site vitrine (Vue 3 + Vite)

Site vitrine one-page pour le Collège Lucie Vérone. Toute la navigation
(Accueil, L'établissement, Nos formations, Vie scolaire, Galerie, Actualités,
Contact) pointe vers des sections de la même page (ancres `#id`) : pas de
changement de page.

## 1. Tester en local (optionnel mais recommandé)

Node.js requis (version 18+) : https://nodejs.org

```bash
npm install
npm run dev
```

Le site s'ouvre sur `http://localhost:5173`.

## 2. Déployer sur Vercel

### Option A — via GitHub (recommandée, mises à jour automatiques)
1. Créez un dépôt GitHub et poussez-y ce dossier (sans `node_modules`, déjà
   exclu par `.gitignore`).
2. Sur vercel.com : **Add New → Project**, importez ce dépôt.
3. Vercel détecte automatiquement Vite grâce à `vercel.json` — laissez les
   réglages par défaut (`npm run build`, dossier de sortie `dist`).
4. Cliquez sur **Deploy**. Le site est en ligne en moins d'une minute, avec
   une URL `xxxx.vercel.app`.

### Option B — via la CLI Vercel (sans GitHub)
```bash
npm install -g vercel
cd college-lucie-verone
vercel
```
Suivez les instructions à l'écran (connexion, nom du projet). Pour mettre
en production :
```bash
vercel --prod
```

## 3. Relier votre nom de domaine

Dans le tableau de bord Vercel du projet : **Settings → Domains**, ajoutez
votre nom de domaine acheté chez votre registrar, puis suivez les
instructions DNS affichées par Vercel (en général un enregistrement A ou
CNAME à ajouter chez votre registrar).

## Modifier les informations du site

Les coordonnées réelles sont centralisées en haut du fichier
`src/App.vue`, dans la balise `<script setup>` :

```js
const PHONE_DISPLAY = '01 97 11 58 69'
const PHONE_TEL = '+2290197115869'
const PHONE_WHATSAPP = 'https://wa.me/2290197115869'
const ADDRESS = "Quartier Akpakpa Ayélawadjè 1, 06 BP 0964, Cotonou, République du Bénin"
```

### Ce qui a été rempli
Les textes de présentation (histoire, mission, vision, atouts, vie scolaire)
sont des textes génériques rédigés à partir des éléments réellement visibles
sur l'établissement (devise « Discipline · Travail · Excellence », valeurs
École/Action/Réflexion/Rigueur/Savoir/Réussite/Culture/Joie/Communication,
niveaux 6ème à Terminale). Ils sont prêts à l'emploi mais gagnent à être
relus et validés par l'administration.

### Ce qui reste à renseigner (`[À RENSEIGNER]`)
Ces champs sont des données propres à l'établissement qu'il n'était pas
possible de deviner sans risquer de donner une information fausse :
- Adresse e-mail
- Liens Facebook / Instagram / TikTok / YouTube
- Horaires précis et calendrier scolaire
- Pièces à fournir pour l'inscription
- Coordonnées GPS exactes pour la carte
- Actualités réelles (sorties, TD, événements)

Recherchez `[À RENSEIGNER]` dans `src/App.vue` pour les localiser
rapidement et les remplacer au fur et à mesure.

Les photos réelles se trouvent dans `src/assets/gallery/`.
