---
name: build
description: Génère les fichiers HTML/CSS/JS du site à partir de sections.md (landing) ou arborescence.md (site multi-pages). Applique la charte graphique, intègre les images disponibles, produit un header + footer commun avec navigation (par ancres pour une landing, par dropdowns thématiques pour un site). Responsive et SEO-ready.
---

# /build — Générer les pages HTML du site

Tu vas transformer le plan d'architecture en **vrai code HTML/CSS/JS**. Après cette commande, l'utilisateur a un site fonctionnel en local, prêt à être connecté au CTA (via `/ottho:cta-setup`) puis déployé.

## Étape 0 — Détecter le contexte

Dans le dossier courant, cherche :

- `./sections.md` → site de type **landing page** → génère `index.html`
- `./arborescence.md` → site de type **site multi-pages** → génère toute l'arborescence de pages

Si aucun des deux n'existe :

> « Il me manque le plan d'architecture. Lance d'abord `/ottho:architecture` pour produire `sections.md` ou `arborescence.md`, puis reviens ici. »

Si les deux existent : demande à l'utilisateur lequel utiliser (rare, mais possible).

## Étape 1 — Charger le contexte graphique et de contenu

Lis les fichiers disponibles pour nourrir la génération :

1. `brief.md` → pour le ton de voix, l'audience, le copywriting AIDA/PAS, la charte graphique (si elle est dedans)
2. `charte.md` (si existant) → palette, typographies, ton visuel
3. `./assets/` → liste des images déjà disponibles (hero, Open Graph, illustrations de sections, avatars…)

Si **aucune charte graphique** n'est disponible, arrête-toi et propose :

> « Aucune charte graphique trouvée dans `brief.md` ou `charte.md`. Deux options :
> 1. Lance `/ottho:brief` pour compléter ton brief avec une charte.
> 2. Tape `Propose-moi une charte graphique à partir de [description du projet]` pour en générer une.
>
> Sans charte, je génèrerais un site neutre qui ne reflète pas ton identité. On attend. »

## Étape 2 — Confirmer le stack

Affiche rapidement :

```
Je vais générer le site en :
- HTML/CSS/JS pur (pas de framework)
- Responsive mobile-first
- Compatible Vercel (déploiement direct)
- SEO de base intégré (5 balises par page)
- Formulaires en PLACEHOLDER (à brancher au CTA avec /ottho:cta-setup)

OK ? (oui / ajuster le stack)
```

Si l'utilisateur veut un framework (Next.js, Astro…), arrête-toi et rappelle que ce cours reste en HTML/CSS/JS pur. Oriente vers une documentation externe.

---

## Parcours A — Si LANDING (sections.md)

### A1. Générer `index.html`

Un seul fichier avec la structure suivante :

```html
<!DOCTYPE html>
<html lang="fr">
<head>
  <!-- SEO de base (5 balises) -->
  <title>[title de la landing]</title>
  <meta name="description" content="[description incitative]">
  <link rel="canonical" href="[URL canonique]">
  <!-- Open Graph -->
  <meta property="og:title" content="…">
  <meta property="og:description" content="…">
  <meta property="og:image" content="./assets/og-image.webp">
  <meta property="og:type" content="website">
  <!-- Viewport mobile -->
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <!-- CSS inline ou fichier séparé -->
  <link rel="stylesheet" href="./style.css">
</head>
<body>
  <!-- HEADER STICKY — navigation par ancres -->
  <header>
    <nav>
      <a href="#hero" class="logo">[Nom]</a>
      <ul>
        <li><a href="#probleme">Problème</a></li>
        <li><a href="#solution">Solution</a></li>
        <li><a href="#benefices">Bénéfices</a></li>
        <li><a href="#tarifs">Tarifs</a></li>
        <li><a href="#faq">FAQ</a></li>
      </ul>
      <a href="#cta" class="cta-mini">[CTA court]</a>
    </nav>
  </header>

  <!-- SECTIONS empilées, chacune avec son id -->
  <section id="hero">…</section>
  <section id="probleme">…</section>
  <section id="solution">…</section>
  <section id="benefices">…</section>
  <section id="social">…</section>
  <section id="tarifs">…</section>
  <section id="faq">…</section>
  <section id="cta">
    <!-- Formulaire placeholder — sera branché par /ottho:cta-setup -->
    <form id="ottho-subscribe-form" data-placeholder="true">
      <input type="email" name="email" placeholder="votre@email.fr" required>
      <input type="text" name="prenom" placeholder="Votre prénom">
      <button type="submit">[CTA]</button>
    </form>
  </section>

  <!-- FOOTER léger -->
  <footer>
    <p>© [année] [Nom]. Tous droits réservés.</p>
    <ul>
      <li><a href="/mentions-legales.html">Mentions légales</a></li>
      <li><a href="/confidentialite.html">Confidentialité</a></li>
    </ul>
    <!-- Réseaux sociaux si pertinent -->
  </footer>
</body>
</html>
```

### A2. Générer `style.css`

Applique **strictement** la charte graphique (palette, typographies, ton visuel).

Points techniques obligatoires :

```css
html {
  scroll-behavior: smooth;
}

header {
  position: sticky;
  top: 0;
  z-index: 100;
  background: [couleur-fond];
  /* + styles menu */
}

section {
  scroll-margin-top: [hauteur-du-header]; /* pour compenser le sticky au clic ancre */
  padding: [généreux] 0;
}

/* Responsive mobile-first */
@media (max-width: 768px) {
  /* Menu burger, sections empilées sans colonnes, boutons 44px min */
}
```

### A3. Intégrer les images de `./assets/`

Pour chaque image présente :

- Hero : `<img src="./assets/hero.webp" alt="[description]" loading="eager" width="..." height="...">`
- Illustrations sections : `loading="lazy"`
- Open Graph : dans le `<head>` → `<meta property="og:image">`
- `alt` descriptif (8-15 mots) généré depuis le contexte de la section

Si une image manque : ajoute un placeholder propre et indique à l'utilisateur quelles images générer via `/ottho:generate-image`.

### A4. Lancer un serveur local pour preview

Propose à l'utilisateur de démarrer un serveur local simple :

```bash
# Option 1 — Python (déjà installé sur Mac/Linux)
python3 -m http.server 3000

# Option 2 — Node
npx serve .
```

Ouvre `http://localhost:3000` dans le navigateur.

---

## Parcours B — Si SITE MULTI-PAGES (arborescence.md)

### B1. Créer la structure de dossiers

À partir de `arborescence.md`, crée :

```
/mon-site
├── accueil.html              (top-level)
├── contact.html              (top-level)
├── equipe.html               (top-level)
├── mentions-legales.html     (top-level, légal)
├── confidentialite.html      (top-level, RGPD)
├── /[theme-1]/               (dossier thématique)
│   ├── page-1.html
│   └── page-2.html
├── /[theme-2]/
│   └── page-1.html
├── /assets/                  (images communes)
└── /css/
    └── style.css             (CSS commun à toutes les pages)
```

### B2. Générer `header.html` et `footer.html` (templates communs)

Pour un site multi-pages, **le header et le footer doivent être identiques sur toutes les pages**. Deux approches :

#### Approche A — Inclusion côté client (JS léger)

Crée `./components/header.html` et `./components/footer.html`, puis injecte-les dans chaque page via un petit script JS :

```html
<!-- Dans chaque page HTML -->
<body>
  <div id="header-mount"></div>
  <main>…contenu spécifique de la page…</main>
  <div id="footer-mount"></div>

  <script>
    fetch('/components/header.html').then(r => r.text()).then(h => {
      document.getElementById('header-mount').innerHTML = h;
    });
    fetch('/components/footer.html').then(r => r.text()).then(h => {
      document.getElementById('footer-mount').innerHTML = h;
    });
  </script>
</body>
```

**Avantage :** modification du header/footer = 1 fichier à modifier.
**Inconvénient :** SEO dégradé (Google voit le header/footer vides au premier chargement).

#### Approche B — Duplication (recommandée pour SEO)

Header et footer **dupliqués dans chaque page HTML**. C'est plus lourd à maintenir, mais :
- SEO parfait (tout est dans chaque page dès le premier chargement)
- Pas de JS requis
- Modification = `/ottho:corrige` répercute sur toutes les pages

**Choix par défaut : Approche B (duplication).** SEO > confort de maintenance, et `/ottho:corrige` gère la cascade.

### B3. Contenu du header (commun à toutes les pages)

```html
<header>
  <nav>
    <a href="/" class="logo">[Nom du site]</a>
    <ul class="menu-principal">
      <li><a href="/accueil.html">Accueil</a></li>
      <li><a href="/equipe.html">Équipe</a></li>

      <!-- Dropdown par thème -->
      <li class="menu-dropdown">
        <a href="#">[Thème 1] ▾</a>
        <ul class="dropdown">
          <li><a href="/theme-1/page-1.html">[Sous-page 1]</a></li>
          <li><a href="/theme-1/page-2.html">[Sous-page 2]</a></li>
        </ul>
      </li>

      <li class="menu-dropdown">
        <a href="#">[Thème 2] ▾</a>
        <ul class="dropdown">
          <li><a href="/theme-2/page-1.html">[Sous-page 1]</a></li>
        </ul>
      </li>
    </ul>
    <a href="/contact.html" class="cta-principal">Contact</a>

    <!-- Burger mobile -->
    <button class="burger" aria-label="Menu">☰</button>
  </nav>
</header>
```

CSS : dropdowns au `hover` en desktop, au `tap` en mobile (via `<details>` ou JS léger).

### B4. Contenu du footer (commun à toutes les pages)

```html
<footer>
  <div class="footer-cols">
    <div>
      <h4>[Thème 1]</h4>
      <ul>
        <li><a href="/theme-1/page-1.html">[Sous-page 1]</a></li>
        <li><a href="/theme-1/page-2.html">[Sous-page 2]</a></li>
      </ul>
    </div>
    <div>
      <h4>[Thème 2]</h4>
      <ul>…</ul>
    </div>
    <div>
      <h4>Entreprise</h4>
      <ul>
        <li><a href="/equipe.html">Équipe</a></li>
        <li><a href="/contact.html">Contact</a></li>
      </ul>
    </div>
    <div>
      <h4>Légal</h4>
      <ul>
        <li><a href="/mentions-legales.html">Mentions légales</a></li>
        <li><a href="/confidentialite.html">Confidentialité</a></li>
        <li><a href="/cgv.html">CGV</a></li>
      </ul>
    </div>
    <div>
      <h4>Social</h4>
      <ul>
        <li><a href="…">LinkedIn</a></li>
        <li><a href="mailto:contact@…">Email</a></li>
      </ul>
    </div>
  </div>
  <p class="copyright">© [année] [Nom]. Tous droits réservés.</p>
</footer>
```

### B5. Générer chaque page

Pour chaque fichier dans `arborescence.md` :

#### Pages top-level (accueil, contact, équipe…)

Chacune a ses **sections propres** définies dans `arborescence.md`. Génère le HTML avec :
- Header commun (copié dans le `<body>`)
- Sections spécifiques à la page
- Footer commun
- SEO propre (title, description, H1, Open Graph spécifiques à la page)

#### Pages thématiques (dans les dossiers)

Chaque page applique le **template du dossier** (défini dans `arborescence.md`). Sections identiques pour toutes les pages du dossier, contenu spécifique par page.

Si une page a une **variante** (déviation du template), respecte-la et note dans `arborescence.md` section Variantes.

### B6. Générer `style.css` commun

Un seul fichier CSS pour tout le site. Applique :
- Charte graphique (palette, typos)
- Header sticky, dropdown hover
- Footer multi-colonnes
- Responsive : menu burger mobile + accordéon pour les thèmes
- Scroll fluide pour les ancres internes (si présentes dans certaines pages)

### B7. Générer les fichiers SEO

- `sitemap.xml` à la racine : liste toutes les pages générées
- `robots.txt` à la racine avec référence au sitemap
- Ces fichiers seront audités par `/ottho:seo-audit`

### B8. Lancer le serveur local

Même logique que Parcours A. Navigue sur chaque page générée pour validation visuelle.

---

## Étape finale (commune A et B)

Affiche un résumé :

```
✅ Site généré.

Fichiers créés :
  Pour une landing :
    - index.html
    - style.css
    - assets/ (images référencées)

  Pour un site multi-pages :
    - [N] pages HTML ([X] top-level + [Y] thématiques)
    - css/style.css
    - sitemap.xml
    - robots.txt
    - assets/

Preview local : http://localhost:3000

À vérifier visuellement :
  ☐ Desktop
  ☐ Mobile (mode device du navigateur : iPhone + iPad)
  ☐ Navigation (menu, dropdowns, ancres si landing)
  ☐ Formulaires (placeholder — seront branchés avec /ottho:cta-setup)
  ☐ Images en place (sinon lancer /ottho:generate-image)

Prochaines étapes :
  1. `/ottho:cta-setup` pour brancher le formulaire (Resend + Airtable + Vercel Function)
  2. `/ottho:seo-audit` pour vérifier les 10 points SEO
  3. `/ottho:deploy` pour mettre en preview sur Vercel
  4. `/ottho:analytics-setup` pour activer GA4 ou Plausible
```

## Règles importantes

- **Respecte strictement la charte graphique.** Une seule couleur hors palette = cohérence cassée.
- **Header + footer identiques** sur toutes les pages (site multi-pages).
- **Sticky header** avec `scroll-margin-top` pour les ancres (landing).
- **Accessibilité minimale** : `alt` sur les images, `aria-label` sur les boutons sans texte, contraste suffisant.
- **Mobile-first** : le menu burger doit marcher avant que le menu desktop soit joli.
- **Pas de CDN externe non nécessaire** : Google Fonts OK, mais évitez jQuery, Bootstrap, Tailwind CDN… le code doit rester pur.

## En cas de pépin

Si la génération produit quelque chose de cassé :

- Lance `/ottho:corrige` pour réparer une section ou un template sans tout refaire.
- Ou corrige à la main : Claude Code peut éditer ligne par ligne chaque fichier HTML.
