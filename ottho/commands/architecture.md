---
name: architecture
description: À partir de brief.md, produit le plan d'architecture adapté au type de site. Landing → sections.md (liste ordonnée des sections avec contenu textuel + visuel). Site multi-pages → arborescence.md (arbre par thèmes + templates par dossier).
---

# /architecture — Structure du site

Tu vas transformer le `brief.md` en un plan d'architecture concret, en suivant la méthode du chapitre 1 du cours.

Deux parcours possibles selon le type de site : **sections.md** pour une landing, **arborescence.md** pour un site multi-pages.

## Étape 0 — Vérifier le brief

Lis `./brief.md`. S'il n'existe pas, arrête-toi et dis :

> « Je ne trouve pas de `brief.md` dans ce dossier. Lance d'abord `/brief` pour le créer, puis reviens à `/architecture`. »

## Étape 1 — Confirmer le type de site

Vérifie le type défini dans `brief.md`. Si c'est ambigu, pose les 3 questions de l'arbre de décision (via la skill `site-structure`) :

1. Combien d'objectifs mesurables ? Un seul → landing. Plusieurs → site.
2. Trafic payant/social ou organique/SEO ? Payant → landing. Organique → site.
3. Besoin de démontrer l'expertise par le contenu ? Non → landing. Oui → site.

Si au moins 2 signaux convergent, confirme. Sinon, présente les deux options et laisse l'utilisateur trancher.

---

## Parcours A — Si LANDING PAGE

Utilise la skill `site-structure` (catalogue des sections types).

### A1. Sélection des sections

Propose à l'utilisateur **une landing type de 5 à 8 sections**. Sélectionne parmi :

- **Hero** (obligatoire) — titre, sous-titre, CTA, visuel
- **Problème** — 3-4 phrases qui nomment la douleur
- **Solution** — la transformation apportée
- **Bénéfices** — 3 à 5 bénéfices concrets
- **Preuve sociale** — témoignages, logos, chiffres
- **Tarifs** (si pertinent) — 1 à 3 cartes
- **FAQ** — 5 à 8 questions/réponses
- **CTA final** — dernière chance de conversion
- Sections secondaires : Comparatif, Équipe, Étapes, Ressources/presse

### A2. Pour chaque section, récupère

- **Contenu textuel** (titre, sous-titre, corps, CTA)
- **Contenu visuel** (image, illustration, icônes, layout)

**Pas de « rôle »** — la section se définit par ce qu'elle contient, pas par une justification pédagogique.

### A3. Génère `sections.md`

```markdown
# Sections — [nom du projet]

## Ordre des sections
1. Hero (id #hero)
2. Problème (id #probleme)
3. Solution (id #solution)
4. Bénéfices (id #benefices)
5. Preuve sociale (id #social)
6. FAQ (id #faq)
7. CTA final (id #cta)

## Header (sticky)
- Logo à gauche (lien vers #hero)
- Menu : liens par ancres vers chaque section (#probleme, #solution, #benefices, #tarifs, #faq)
- CTA mini à droite (optionnel, pointe vers #cta)

## Section 1 — Hero (id: #hero)

- **Contenu textuel :**
  - Titre principal : « … »
  - Sous-titre : « … »
  - CTA : « [verbe d'action + promesse] »
- **Contenu visuel :**
  - Image hero : [dimensions, prompt Nano Banana si à générer via /ottho:generate-image]
  - Layout : [titre à gauche / image à droite, centré…]

## Section 2 — Problème (id: #probleme)

- **Contenu textuel :**
  - …
- **Contenu visuel :**
  - …

[Et ainsi de suite — chaque section reçoit un id unique correspondant à un lien du header]

## Footer
- Mentions légales (obligatoire)
- Politique de confidentialité (RGPD)
- Liens réseaux sociaux
- Copyright

Architecture générée le [date] par le plugin ottho.
```

### A3.bis — Règles importantes pour la génération HTML

Quand tu génères le `index.html` à partir de ce plan :

- **Chaque section reçoit un `id` HTML** (`<section id="probleme">`) pour que les liens du header fonctionnent.
- **Header sticky** : `position: sticky; top: 0; z-index: 10;` en CSS.
- **Menu uniquement avec ancres** (`href="#probleme"`) — jamais de lien vers d'autres pages.
- **Scroll fluide** : ajoute `scroll-behavior: smooth;` au `html` ou `body` en CSS.
- **Décalage d'ancre** : pense au `scroll-margin-top` égal à la hauteur du header sticky, sinon les sections sont masquées derrière le header au clic.

### A4. Arborescence de fichiers proposée

```
/ma-landing
├── index.html
├── sections.md
├── /assets (images, og-image.webp)
└── /api (subscribe.js — placeholder, branché au chapitre Hébergement)
```

---

## Parcours B — Si SITE MULTI-PAGES

### B1. Définir l'arborescence par thèmes

Demande à l'utilisateur :

1. Quelles sont les **pages top-level** ? (accueil, contact, équipe, mentions légales) — à la racine.
2. Quels sont les **thèmes** de contenu ? (produits, offres, méthode, services…) — chacun = un dossier.
3. Pour chaque thème, combien de pages y a-t-il ? (produit1, produit2, etc.)

Construis mentalement l'arbre.

**Exclus le blog** pour l'instant (il fera l'objet d'un chapitre dédié du cours).

### B2. Pour chaque template de thème, définis

- La **liste des sections** qui composent le template (hero, contenu spécifique, CTA…)
- Les **variantes autorisées** (ex. ajouter Tarifs si la page décrit un produit payant)

Rappel : **par défaut, toutes les pages d'un thème partagent le même template.** Une page peut dévier si nécessaire — c'est accepté.

### B3. Génère `arborescence.md` ET un `template.md` par dossier thématique

**Principe clé** : l'architecture d'un site multi-pages repose sur **trois niveaux de documents** :

1. **`arborescence.md`** à la racine → **plan global** du site (structure, liste des pages, navigation)
2. **`section-[nom-de-page].md`** à la racine → **détail des sections** de chaque page top-level (ex. `section-accueil.md`, `section-contact.md`, `section-equipe.md`)
3. **`/[theme]/template.md`** dans chaque dossier thématique → **détail du template** appliqué aux pages de ce dossier

Cette séparation permet la **cascade maîtrisée** :
- Modifier `section-accueil.md` = mise à jour de `accueil.html` uniquement.
- Modifier `/offres/template.md` = mise à jour de toutes les pages de `/offres/`.
- Autres documents intacts.

### B3.1 — Générer `arborescence.md` (plan global)

```markdown
# Arborescence — [nom du site]

## Arbre du site

/mon-site
├── accueil.html
├── contact.html
├── equipe.html
├── arborescence.md                  ← plan global
├── section-accueil.md               ← sections de accueil.html
├── section-contact.md               ← sections de contact.html
├── section-equipe.md                ← sections de equipe.html
├── /[theme-1]
│   ├── template.md                  ← template du dossier
│   ├── page-1.html
│   └── page-2.html
└── /[theme-2]
    ├── template.md                  ← template du dossier
    ├── page-1.html
    └── page-2.html

## Pages top-level

Chaque page top-level a son propre fichier `section-[nom].md` à la racine.
Pour modifier une page top-level : éditer le fichier correspondant, puis lancer `/ottho:corrige`.

- `section-accueil.md` — sections de `accueil.html`
- `section-contact.md` — sections de `contact.html`
- `section-equipe.md` — sections de `equipe.html`

## Thèmes (dossiers)

Chaque thème a son propre `template.md` qui définit la structure des pages de son dossier.

- `/[theme-1]/template.md` — appliqué à toutes les pages de `/[theme-1]/`
- `/[theme-2]/template.md` — appliqué à toutes les pages de `/[theme-2]/`

Pour modifier la structure d'un thème, éditer le `template.md` correspondant puis lancer `/ottho:corrige` pour répercuter.

## Header (commun à toutes les pages, sticky)

- Logo à gauche (renvoie vers accueil.html)
- Menu principal :
  - Pages top-level (Accueil, Équipe, Contact, etc.)
  - Thèmes (chaque dossier /theme/ = une entrée)
- Dropdown sous chaque thème qui liste les sous-pages du dossier :
  - « [Thème 1] » → [page-1], [page-2], …
  - « [Thème 2] » → [page-1], [page-2], …
- CTA fixe en haut à droite (« Contact » / « Demander un devis »…)
- Version mobile : menu burger, thèmes en accordéon

## Footer (commun à toutes les pages)

- Colonnes par thème (chaque thème avec ses sous-pages en liens)
- Colonne « Entreprise » : À propos, Équipe, Contact, Recrutement
- Colonne « Légal » : Mentions légales, Politique de confidentialité, CGV, Cookies
- Colonne « Social » : LinkedIn, Instagram, YouTube, Email pro
- Copyright

## CTA principal du site
[verbe d'action + promesse concrète]

Architecture générée le [date] par le plugin ottho.
```

### B3.2 — Générer un `section-[nom].md` pour chaque page top-level

Pour chaque page top-level (`accueil.html`, `contact.html`, `equipe.html`, `mentions-legales.html`…), crée un fichier `section-[nom-de-page].md` à la racine :

```markdown
# Sections — [nom de la page]

> Ce document définit les sections de `[nom-de-page].html`.
> Pour modifier : éditez ce fichier, puis lancez `/ottho:corrige` pour mettre à jour le HTML.

## Ordre des sections
1. Hero [nom-de-page]
2. [Section spécifique 1]
3. [Section spécifique 2]
4. CTA

## Détail par section

### Section 1 — Hero [nom-de-page]

- **Contenu textuel :**
  - Titre : « … »
  - Sous-titre : « … »
  - CTA : « … »
- **Contenu visuel :**
  - Image : [dimensions, prompt Nano Banana si à générer]

### Section 2 — [Nom]

- **Contenu textuel :** …
- **Contenu visuel :** …

[Et ainsi de suite pour chaque section]

## SEO spécifique à cette page

- `<title>` : « [title unique pour cette page] »
- `<meta description>` : « … »
- `<h1>` : « … »
- URL canonique : `https://[domaine]/[nom-de-page].html`

Sections générées le [date] par le plugin ottho.
```

### B3.3 — Générer un `template.md` dans chaque dossier thématique

Pour chaque dossier `/[theme]/`, crée un `template.md` dédié :

```markdown
# Template — [nom du thème]

> Ce template s'applique à **toutes les pages HTML du dossier `/[theme]/`** (par défaut).
> Pour modifier la structure : éditez ce fichier, puis lancez `/ottho:corrige` pour répercuter sur toutes les pages.

## Sections (dans l'ordre)

1. Hero [thème]
2. [Section spécifique 1]
3. [Section spécifique 2]
4. Tarif (si produit payant)
5. CTA « [Action] »

## Détail par section

### Section 1 — Hero [thème]

- **Contenu textuel :**
  - Titre : « [nom de la page] »
  - Sous-titre : « … »
  - CTA : « [verbe d'action] »
- **Contenu visuel :**
  - Image : [dimensions, prompt Nano Banana si à générer]

### Section 2 — [Nom]

- **Contenu textuel :** …
- **Contenu visuel :** …

[Et ainsi de suite pour chaque section]

## Variantes autorisées

Une page peut dévier du template si nécessaire. Variantes déjà enregistrées :

- [page-1.html] : ajoute une section « Témoignages » après « Tarif ».
- [page-2.html] : suit le template strict.

## Règles techniques

- Header et footer communs (repris depuis arborescence.md).
- SEO : chaque page a son propre title / meta description / h1 spécifiques, mais structure HTML identique.
- Responsive mobile-first.

Template généré le [date] par le plugin ottho.
```

### B3.bis — Règles importantes pour la génération HTML

Quand tu génères les pages HTML à partir de ce plan :

- **Header et footer sont des composants communs** à toutes les pages : génère-les **une seule fois** en HTML, puis inclus-les dans chaque page (via copier-coller identique, ou via un script de build léger si disponible).
- **Menu dropdown** : utilise HTML/CSS pur (pas de JS nécessaire pour un dropdown au hover en desktop).
- **Version mobile** : JS minimaliste pour le menu burger et les accordéons par thème.
- **Toujours vérifier la cohérence** : si une page est renommée, mettre à jour le lien dans header ET footer sur toutes les pages.

### B4. Arborescence de fichiers proposée

```
/mon-site
├── accueil.html
├── contact.html
├── equipe.html
├── arborescence.md              ← plan global
├── section-accueil.md           ← sections de accueil.html
├── section-contact.md           ← sections de contact.html
├── section-equipe.md            ← sections de equipe.html
├── /[theme-1]
│   ├── template.md              ← template du dossier
│   └── page-1.html
├── /[theme-2]
│   ├── template.md
│   └── page-1.html
├── /assets
└── /api (contact.js — placeholder, branché au chapitre Hébergement)
```

**Règle cascade :**
- Modifier `section-[page].md` = mise à jour de la page top-level correspondante (`/ottho:corrige`).
- Modifier `/[theme]/template.md` = mise à jour de toutes les pages du dossier (`/ottho:corrige`).
- `arborescence.md` reste stable, sauf si on ajoute/supprime/renomme une page ou un thème.

---

## Clôture

Affiche les chemins des fichiers générés et dis :

> « Ton architecture est prête :
> - **Landing** : `./sections.md` (plan complet)
> - **Site** : `./arborescence.md` (plan global) + un `./[theme]/template.md` dans chaque dossier thématique
>
> Prochaines étapes suggérées :
> 1. `/ottho:build` pour **générer les fichiers HTML/CSS/JS** à partir du plan
> 2. `/ottho:generate-image` pour les images (hero, Open Graph, illustrations)
> 3. `/ottho:cta-setup` au chapitre Hébergement, pour brancher le formulaire
> 4. `/ottho:seo-audit` pour vérifier les balises
> 5. `/ottho:deploy` pour mettre en ligne »
