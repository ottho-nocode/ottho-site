---
name: seo-checklist
description: Les 10 points SEO à vérifier sur toute page HTML. À utiliser pour auditer, corriger et maintenir un site optimisé pour Google. Enseigné au chapitre 4 du cours Claude + Site web.
---

# Skill : seo-checklist

Cette skill contient les 10 points de contrôle SEO à appliquer systématiquement sur tout site HTML. Utilisée par `/seo-audit`.

## Les 10 points

### Point 1 — `<title>`

**Critères :**
- Présent dans le `<head>` de chaque page
- **Unique** (différent sur chaque page)
- **55 à 60 caractères** (au-delà, Google tronque)
- Commence par la **promesse principale**, finit par la **marque**
- Un seul `<title>` par page

**Exemples :**
- ✅ « [Promesse] — [précision / audience] | [marque] » (57-60 car.)
- ✅ « [Nom du site] — [spécialité ou métier] à [ville] » (60 car.)
- ❌ « Accueil »
- ❌ « Bienvenue sur notre site ! »

### Point 2 — `<meta name="description">`

**Critères :**
- Présente dans le `<head>`
- Unique par page
- **140 à 160 caractères**
- Incitative (donne envie de cliquer)
- Inclut le mot-clé principal naturellement

**Exemples :**
- ✅ « Recevez chaque semaine un message clair pour oser changer de cap. Un guide offert à l'inscription : 5 signes qu'il est temps de partir. » (147 car.)
- ❌ « Bienvenue sur notre site »
- ❌ (absente) — Google invente souvent une mauvaise description

### Point 3 — `<h1>`

**Critères :**
- **UN SEUL** par page
- Contient le **sujet principal** de la page
- Contient le **mot-clé principal** (si possible naturellement)
- Différent du `<title>` (plus humain, moins « résultats Google »)

**Exemples :**
- ✅ `<h1>Un conseil juridique clair pour votre PME</h1>`
- ✅ `<h1>Avocats en droit du travail pour entreprises</h1>`
- ❌ Deux `<h1>` sur la même page
- ❌ `<h1>Logo [marque]</h1>` (c'est le logo, pas le sujet)

### Point 4 — Structure `<h2>` / `<h3>`

**Critères :**
- Hiérarchie cohérente (`<h2>` pour les sections principales, `<h3>` pour les sous-sections)
- Au moins **2-3 `<h2>`** par page de contenu
- Les titres de section doivent **décrire ce que contient la section** (pas « Partie 1 »)

### Point 5 — Balises Open Graph

**Les 4 obligatoires :**

```html
<meta property="og:title" content="[titre, même logique que <title>]">
<meta property="og:description" content="[description]">
<meta property="og:image" content="[URL absolue vers image 1200×630]">
<meta property="og:type" content="website">
```

**Pour l'image :**
- Format **1200 × 630 pixels** (paysage)
- Moins de **1 Mo**
- Lisible en miniature
- Cohérente avec la charte graphique

### Point 6 — Balise canonique

**Critère :**
- Présente dans le `<head>` de chaque page
- Pointe vers l'URL « officielle » de la page (souvent https://www.domaine.fr/chemin)

**Exemple :**
```html
<link rel="canonical" href="https://www.durand-avocats.fr/expertises/droit-du-travail" />
```

**Pourquoi :** un même site peut être accessible par plusieurs URLs (avec/sans www, avec/sans /, etc.). Sans canonique, la puissance SEO se dilue.

### Point 7 — Attributs `alt` sur les images

**Critères :**
- Toutes les `<img>` ont un `alt`
- L'`alt` **décrit factuellement** ce que montre l'image
- 8 à 15 mots
- Pas de « image », « photo » dans l'alt (redondant avec la nature de l'élément)

**Exemples :**
- ✅ `alt="L'équipe de [marque], [N] personnes souriantes devant les bureaux"`
- ❌ `alt="image1"`
- ❌ `alt="photo"`
- ❌ (absent)

**Cas particulier :** les images purement décoratives peuvent avoir `alt=""` (lu comme « rien » par les lecteurs d'écran).

### Point 8 — Paragraphes courts

**Critères :**
- **Pas plus de 4 lignes** par paragraphe
- Idées découpées, pas de murs de texte
- Listes à puces dès que c'est possible

**Action automatique :** identifier les `<p>` de plus de 300 caractères et les proposer à découper.

### Point 9 — `sitemap.xml`

**Critères :**
- Présent à la racine du site (ou dans `public/`)
- Liste **toutes les pages HTML** publiques
- Format XML valide
- Chaque `<url>` a :
  - `<loc>` : URL complète absolue (https://www.domaine.fr/chemin)
  - `<lastmod>` : date ISO
  - `<priority>` : 1.0 pour accueil, 0.7-0.9 pour autres pages

**Template :**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">
  <url>
    <loc>https://www.domaine.fr/</loc>
    <lastmod>2026-04-20</lastmod>
    <priority>1.0</priority>
  </url>
  <url>
    <loc>https://www.domaine.fr/a-propos</loc>
    <lastmod>2026-04-20</lastmod>
    <priority>0.8</priority>
  </url>
  <!-- ... -->
</urlset>
```

### Point 10 — `robots.txt`

**Critères :**
- Présent à la racine du site (ou dans `public/`)
- Référence le sitemap
- Autorise par défaut tous les robots

**Template minimal :**

```txt
User-agent: *
Allow: /
Sitemap: https://www.domaine.fr/sitemap.xml
```

**Cas particuliers :** ajouter des `Disallow:` pour les pages admin, de remerciement, ou en test.

---

## Protocole d'audit

Pour chaque page HTML :

1. Parser le `<head>` et le `<body>`.
2. Vérifier chacun des 10 points.
3. Verdict : **OK** / **À corriger** / **Absent** + 1-2 lignes de justification.
4. Produire un rapport sous forme de tableau markdown.

## Protocole de correction

1. Générer les balises manquantes (avec contenu adapté au brief / au contexte de la page).
2. Corriger les balises mal formées (trop longues, dupliquées, vides).
3. Générer `sitemap.xml` et `robots.txt` si absents.
4. Proposer une image Open Graph si absente.
5. Refaire l'audit et produire le rapport APRÈS.

## Usage dans les commandes

Cette skill est invoquée par :

- `/seo-audit` — principal usage
- `/site-web` — via `/seo-audit` dans l'orchestration
