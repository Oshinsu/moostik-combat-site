# Moostik Combat — plateforme web

Site vitrine + jeu jouable + pages légales, **dédié à Moostik Combat** (≠ moostik.vercel.app qui est le studio de prod).
Édité par **BYSS GROUP** (SASU, Fort-de-France).

## Structure
```
index.html              landing page (DA Bloodwings) → bouton JOUER vers /play
play/                   le JEU (export web Godot : PWA installable, multijoueur)
privacy.html            politique de confidentialité (obligatoire Play Store)
mentions-legales.html   mentions légales
.well-known/
  assetlinks.json       lien app↔site pour le TWA Play Store (⚠️ à compléter, voir ci-dessous)
img/                    visuels du site (icône, héros, persos)
vercel.json             MIME wasm + content-type assetlinks + cache
```

## Déployer (Vercel — recommandé)
1. Pousser ce dossier sur un repo GitHub.
2. vercel.com → **New Project** → importer le repo → **Deploy** (preset « Other », pas de build).
3. **Domaine custom** (recommandé, ~10 €/an) : Settings ▸ Domains → ajouter `moostikcombat.com` (ou autre). Un domaine propre = **origin propre pour l'assetlinks** (TWA plein écran) + marque.
   - Alternative gratuite : utiliser l'URL `*.vercel.app` fournie.

> GitHub Pages marche aussi, mais le `.well-known/assetlinks.json` doit être à la **racine du domaine** → un domaine custom (ou un repo `user.github.io`) est nécessaire pour le TWA. Vercel + domaine = le plus simple.

## ✅ À COMPLÉTER avant le Play Store
1. **E-mail de contact** : remplacer `contact@byss-group.com` dans `privacy.html` + `mentions-legales.html` par une adresse qui existe (ou créer la boîte).
2. **assetlinks.json** : remplacer `REMPLACER_PAR_LE_SHA256_DE_TA_CLE_DE_SIGNATURE` par l'empreinte SHA-256 de ta clé de signature Android — fournie par **PWABuilder** ou `bubblewrap` au moment de générer l'app. Le `package_name` (`org.bloodwings.moostikcombat`) doit correspondre à celui de l'app.
3. **TWA** : avec PWABuilder, emballe l'URL `https://TON-DOMAINE/play/` (et NON la racine) pour que le TWA ouvre directement le jeu en plein écran.

## Mettre à jour le jeu
Ré-exporter le jeu (`Godot --export-release "Web" builds/web/index.html`) puis recopier `builds/web/*` (sauf `.import`) dans `play/`.

## (Option) Activer les threads WASM (meilleures perfs)
Ré-exporter avec `variant/thread_support=true`, puis ajouter à `vercel.json` les headers
`Cross-Origin-Opener-Policy: same-origin` + `Cross-Origin-Embedder-Policy: require-corp`
sur `/play/(.*)`. (Impossible sur GitHub Pages — d'où l'intérêt de Vercel.)
