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

### B3. Génère `arborescence.md`

```markdown
# Arborescence — [nom du site]

## Arbre du site

/mon-site
├── accueil.html
├── contact.html
├── equipe.html
├── /[theme-1]
│   ├── page-1.html
│   └── page-2.html
└── /[theme-2]
    ├── page-1.html
    └── page-2.html

## Pages top-level

### accueil.html
- Rôle : orienter le visiteur vers le bon thème
- Sections : [liste]
- CTA principal : …

### contact.html
- Rôle : convertir l'intérêt en prise de contact
- Sections : Formulaire, Coordonnées, Horaires, FAQ

### equipe.html
- Rôle : humaniser le site
- Sections : [liste]

## Templates par thème

### Template [Thème 1] (appliqué à /[theme-1]/*.html)
- Sections : [liste ordonnée]
- Variantes autorisées : [si applicable]

### Template [Thème 2] (appliqué à /[theme-2]/*.html)
- Sections : [liste ordonnée]
- Variantes autorisées : [si applicable]

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
├── arborescence.md
├── /[theme-1]
│   └── page-1.html
├── /[theme-2]
│   └── page-1.html
├── /assets
└── /api (contact.js — placeholder, branché au chapitre Hébergement)
```

---

## Clôture

Affiche le chemin du fichier généré et dis :

> « Ton architecture est prête : `./[sections.md / arborescence.md]`. C'est ton plan détaillé.
>
> Prochaines étapes suggérées :
> 1. Demande à Claude Code de **générer le HTML/CSS** à partir de ce plan (« Génère index.html en suivant sections.md »)
> 2. `/generate-image` pour les images de ton site (hero, Open Graph, illustrations)
> 3. `/cta-setup` quand le formulaire HTML est en place et que tu es au chapitre Hébergement
> 4. `/seo-audit` pour vérifier les balises
> 5. `/deploy` pour mettre en ligne »
