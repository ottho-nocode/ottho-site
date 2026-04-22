---
name: site-structure
description: Méthode de structure d'un site web. Pour une landing, sections types + document pivot sections.md. Pour un site multi-pages, arborescence par thèmes + templates par dossier. Enseigné au chapitre Architecture du cours Claude + Site web.
---

# Skill : site-structure

Cette skill contient la méthode de choix entre landing page et site multi-pages, le catalogue des sections types d'une landing, et les règles d'arborescence et de templates pour un site multi-pages.

## Arbre de décision — landing ou site multi-pages ?

### Question 1 — Combien d'objectifs mesurables ?

- **Un seul** (capter un email, vendre un produit unique, inscrire à un événement) → **landing**
- **Plusieurs** (contact + crédibilité + recrutement + plusieurs offres) → **site multi-pages**

### Question 2 — D'où vient le trafic ?

- **Payant / social** (Meta Ads, Google Ads, campagnes email) → **landing**
- **Organique / SEO** (recherche Google, bouche-à-oreille) → **site multi-pages**

### Question 3 — Besoin de démontrer l'expertise par le contenu ?

- **Non** → **landing**
- **Oui** (conseil, avocat, médecin, architecte, consultant) → **site multi-pages**

### Règle simple

- **2+ signaux landing** → landing page
- **2+ signaux site** → site multi-pages
- **Partagé** → commencer par une landing, étendre plus tard

---

## Parcours A — LANDING PAGE

Une landing = **un seul fichier HTML qui empile des sections**. Pas de menu, pas de navigation — un long document vertical avec un seul objectif.

### Qu'est-ce qu'une « section » ?

Une section est **un bloc autonome de page**, avec :

- Un **contenu textuel** (titre, sous-titre, corps, liste, CTA)
- Un **contenu visuel** (image, illustration, icônes, layout)

Chaque section se pense, s'écrit, se génère, se modifie **indépendamment des autres**.

### Catalogue des sections types

Une landing typique utilise **5 à 8 sections**. Piochez dans :

#### Hero (obligatoire)
- Textuel : titre fort, sous-titre explicatif, bouton CTA primaire
- Visuel : image ou illustration pleine largeur

#### Problème
- Textuel : 3-4 phrases qui nomment le problème et ses conséquences
- Visuel : icônes ou illustration sobre

#### Solution
- Textuel : 3-4 phrases sur la promesse de transformation
- Visuel : photo ou illustration qui humanise

#### Bénéfices
- Textuel : 3 à 5 bénéfices, chacun avec titre court + description courte
- Visuel : icônes ou pictogrammes en grille

#### Preuve sociale
- Textuel : 3 à 5 témoignages (nom, rôle, citation), logos clients, chiffres
- Visuel : photos des témoins, logos, graphiques simples

#### Tarifs (si pertinent)
- Textuel : 1 à 3 cartes (nom, prix, inclus, CTA)
- Visuel : cartes en grille, option « recommandée » en avant

#### FAQ
- Textuel : 5 à 8 questions/réponses (accordéon possible)
- Visuel : texte sobre

#### CTA final
- Textuel : appel direct + bouton
- Visuel : bandeau contrasté

#### Sections secondaires
- Comparatif (vous vs concurrents, ou vs « ne rien faire »)
- Équipe (humaniser un service)
- Étapes / roadmap (clarifier le parcours d'achat)
- Ressources / presse (articles, podcasts, mentions)

### Header et footer d'une landing

Une landing a **un header en haut** et **un footer en bas**, comme tout site web.

**Header :**
- Logo à gauche (lien vers #hero, tout en haut)
- Menu par **ancres** vers les sections (#probleme, #solution, #benefices, #tarifs, #faq)
- CTA mini à droite (optionnel, pointe vers #cta)
- **Sticky** (reste fixe au scroll)

**Footer :**
- Mentions légales (obligatoire)
- Politique de confidentialité (RGPD)
- Liens réseaux sociaux
- Copyright

Pas de menu de navigation dans le footer — la landing n'a qu'une seule page.

### Navigation par ancres — règles techniques

- Chaque section a un **id HTML unique** : `<section id="probleme">`
- Les liens du header pointent vers ces id : `<a href="#probleme">`
- CSS : `scroll-behavior: smooth;` pour un défilement doux
- CSS : `scroll-margin-top: [hauteur du header];` sur chaque section pour compenser le sticky header

### Document `sections.md` — structure type

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
- Menu : ancres vers chaque section
- CTA mini à droite (optionnel)

## Section 1 — Hero (id: #hero)

- **Contenu textuel :**
  - Titre principal : « … »
  - Sous-titre : « … »
  - CTA : « [verbe d'action + promesse] »
- **Contenu visuel :**
  - Image hero : [dimensions, prompt Nano Banana si à générer]
  - Layout : [titre à gauche / image à droite, centré…]

[Sections suivantes dans le même format, chacune avec son id]

## Footer
- Mentions légales
- Politique de confidentialité
- Réseaux sociaux
- Copyright
```

### Arborescence d'une landing

```
/ma-landing
├── index.html              ← un seul fichier (header + sections + footer)
├── sections.md             ← la boussole
├── /assets                 ← images
└── /api                    ← Vercel Function (chapitre Hébergement)
```

### Règles d'or de la landing

- **Une section = une idée unique.**
- **Ordre logique** : hero → problème → solution → preuves → action.
- **Maximum 8 sections.**
- **Header + footer présents**, menu du header par **ancres** uniquement (pas de lien vers d'autres pages).
- **Zéro lien vers d'autres pages** (contrairement à un site multi-pages).
- **Deux CTA max, identiques** (hero + fin).
- **Mobile-first.**

---

## Parcours B — SITE MULTI-PAGES

Un site multi-pages se pense en **deux concepts clés** :

1. **Arborescence** — l'arbre des dossiers et pages, organisé par thèmes.
2. **Templates** — structures de page qui se répètent par thématique.

### L'arborescence — par thèmes

**Règle d'or :** le site s'organise **par thèmes de contenu**, pas par type de fichier.

- Chaque thème = **un dossier**
- Chaque page du thème = **un fichier HTML dans le dossier**
- Les pages transverses (accueil, contact, équipe, mentions légales) = **à la racine**

### Arborescence type

```
/mon-site
├── accueil.html              ← top-level
├── contact.html              ← top-level
├── equipe.html               ← top-level
├── arborescence.md           ← boussole du site
├── /[theme-1]
│   ├── page-1.html
│   └── page-2.html
├── /[theme-2]
│   ├── page-1.html
│   └── page-2.html
├── /assets
└── /api
```

### Règles d'arborescence

1. **Pages top-level** → racine.
2. **Pages thématiques** → dossier nommé d'après le thème.
3. **Plusieurs pages d'un même thème** → fichiers frères dans le même dossier.
4. **Pas de profondeur inutile** (évitez les sous-dossiers au-delà d'un niveau).
5. **Le blog suit des règles spécifiques** (dates, catégories, pagination) — fait l'objet d'un chapitre dédié.

### Les templates — pages qui se ressemblent

Dans un thème, les pages ont souvent la **même structure** (mêmes sections dans le même ordre). Cette structure commune = **un template**.

#### La règle

- **Par défaut, toutes les pages d'un même thème partagent le même template.**
- **Une page peut dévier** du template si nécessaire (ex. ajouter une section unique). C'est accepté.

#### Pas de fichier de template séparé

Pas de `template.html` à écrire. Le template **vit dans `arborescence.md`** — Claude Code le comprend et l'applique.

#### Modification en cascade

Si l'utilisateur modifie le template d'un thème :

```
J'ai mis à jour le template [Thème] dans arborescence.md
(j'ai ajouté une section Témoignages après la section Tarif).
Mets à jour toutes les pages de /[theme] pour suivre le nouveau template.
Conserve le contenu spécifique de chaque page ; ne touche qu'à la structure.
```

Claude Code répercute sur toutes les pages du dossier.

### Document `arborescence.md` — structure type

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
    └── page-1.html

## Pages top-level

### accueil.html
- Rôle : orienter vers le bon thème
- Sections : Hero, Bénéfices, Teasers vers les thèmes, Preuve sociale, CTA

### contact.html
- Rôle : convertir l'intérêt en prise de contact
- Sections : Formulaire, Coordonnées, Horaires, FAQ

### equipe.html
- Rôle : humaniser
- Sections : Hero équipe, Portraits, Valeurs, CTA

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
  - Pages top-level (Accueil, Équipe, Contact…)
  - Thèmes (chaque dossier /theme/ = une entrée)
- Dropdown sous chaque thème listant les sous-pages du dossier
- CTA fixe en haut à droite (« Contact » / « Devis »…)
- Version mobile : menu burger, thèmes en accordéon

## Footer (commun à toutes les pages)

- Colonnes par thème (chaque thème avec ses sous-pages)
- Colonne « Entreprise » : À propos, Équipe, Contact, Recrutement
- Colonne « Légal » : Mentions légales, Politique de confidentialité, CGV, Cookies
- Colonne « Social » : LinkedIn, Instagram, YouTube, Email pro
- Copyright

## CTA principal du site
[verbe d'action + promesse concrète]
```

### Sections dans les templates

Les templates utilisent les **mêmes sections que pour une landing** (hero, bénéfices, preuve sociale, tarifs, CTA, etc.), mais appliquées à l'échelle d'une page thématique. Voir le catalogue dans la partie Landing plus haut.

### Header et footer — communs à toutes les pages

**Header (sticky, identique partout) :**
- Logo à gauche → accueil.html
- Menu principal avec pages top-level + thèmes
- Dropdown sous chaque thème listant les sous-pages du dossier correspondant
- CTA fixe en haut à droite

**Footer (identique partout) :**
- Plan de site secondaire par colonnes (un thème par colonne + colonnes Entreprise/Légal/Social)
- Plus riche qu'un footer de landing

**Implémentation HTML :** header et footer sont répétés dans chaque page. Lors d'une modification (ex. renommage d'une page, ajout d'une nouvelle page à un thème), le menu dans header + footer doit être mis à jour **sur toutes les pages** en même temps (cascade gérée par `/ottho:corrige` ou un dialogue avec Claude Code).

### Règles de navigation pour un site

- Header identique partout, sticky au scroll
- Menu principal : pages top-level + thèmes avec dropdowns
- Footer identique partout
- CTA fixe en haut à droite : « Contact » ou « Devis »
- Version mobile : menu burger avec thèmes en accordéon
- Depuis n'importe où, chaque autre page atteignable en 1 ou 2 clics

### Pièges à éviter

- Trop de pages (> 10). Chaque page demande du contenu, du SEO, de l'entretien.
- Pages creuses. Une page avec 3 items = autant pas de page.
- Profondeur excessive (sous-dossiers de sous-dossiers).
- Templates qui dérivent tout seuls — redéfinir si chaque page du thème devient unique.
- Menu différent d'une page à l'autre.

---

## Usage dans les commandes du plugin

Cette skill est invoquée par :

- `/architecture` — principal usage (sections.md pour landing, arborescence.md pour site)
- `/site-web` — via `/architecture` dans l'orchestration
