# ottho

> Plugin Claude Code compagnon des cours Ottho. Commence par [academy.ottho.co/claude-site-web](https://academy.ottho.co/claude-site-web).
> Crée, déploie et mesure une landing page ou un site web multi-pages — avec arborescence par thèmes, templates, et génération d'images par IA — sans jamais copier-coller une clé API.

## À qui ça s'adresse

Ce plugin est le **raccourci final** du cours *Claude + Site web*. Il condense en 10 commandes tout ce qui a été enseigné au fil des 7 chapitres du cours :

- Rédiger un brief clair (objectif, audience, copywriting)
- Structurer le site : **`sections.md`** pour une landing, **`arborescence.md`** (pages + templates par thème) pour un site multi-pages
- Générer des images uniques via **Nano Banana** (Google Gemini) sur fal.ai
- **Construire les fichiers HTML/CSS/JS** à partir du plan d'architecture
- Configurer un formulaire opérationnel (Resend + Airtable + Vercel Function)
- Auditer et corriger le SEO
- Déployer sur Vercel
- Activer les analytics (GA4 ou Plausible)
- Corriger des sections ou des templates **après coup**, avec cascade propre
- Enchaîner tout ça via une commande master

**Pré-requis :** avoir suivi le cours ou maîtriser la méthode. Ce plugin n'est pas un substitut à la compréhension — c'est un accélérateur.

## Installation

Dans Claude Code :

```
/plugin install ottho@ottho-local
```

(Le nom exact du marketplace dépend de comment le plugin a été publié — voir le `marketplace.json` à la racine du dossier `plugin/`.)

## Pré-requis MCP

Avant utilisation, les MCPs suivants doivent être connectés (procédures couvertes dans le cours) :

- **Vercel MCP** (obligatoire) — hébergement, domaines, env vars, déploiement
- **Resend MCP** (obligatoire) — emails transactionnels
- **Airtable MCP** (obligatoire) — stockage des soumissions
- **fal.ai MCP** (optionnel) — génération d'images via Nano Banana

Tous s'installent par simple dialogue dans Claude Code (« Installe le MCP [nom] »). Zéro commande bash.

## Les 10 commandes

Toutes les commandes sont namespacées sous `/ottho:` (convention Claude Code pour éviter les conflits entre plugins).

### `/ottho:brief`

Dialogue guidé : objectif mesurable, audience cible, proposition de valeur, ton de voix, framework AIDA/PAS. Génère `brief.md` dans le dossier courant.

### `/ottho:architecture`

Produit votre plan d'architecture, adapté au type de site :

- **Landing page** → génère `sections.md` (liste ordonnée des sections avec contenu textuel + visuel)
- **Site multi-pages** → génère `arborescence.md` (arbre par thèmes + templates par dossier)

### `/ottho:generate-image`

Génère une ou plusieurs images cohérentes avec votre charte via **Nano Banana** (Google Gemini, sur fal.ai). Idéal pour hero, Open Graph, illustrations de services, avatars.

### `/ottho:build`

Génère les fichiers HTML/CSS/JS du site à partir de `sections.md` (landing) ou `arborescence.md` (site multi-pages). Applique la charte graphique, intègre les images, produit un **header + footer commun** avec navigation (ancres pour landing, dropdowns thématiques pour site multi-pages). Responsive, SEO-ready, formulaires en placeholder.

### `/ottho:cta-setup`

Connecte votre formulaire HTML à Resend + Airtable via une Vercel Function. Gère les env vars automatiquement. Zéro clé API au clavier.

### `/ottho:seo-audit`

Audite les 10 points SEO de votre site, propose des corrections, les applique sur accord. Génère aussi `sitemap.xml` et `robots.txt` si absents.

### `/ottho:deploy`

Déploie sur Vercel. Preview par défaut. Ajoutez `--prod` pour la production. Gère aussi l'ajout d'un domaine personnalisé.

### `/ottho:analytics-setup`

Active **Google Analytics 4** (par défaut, avec bandeau RGPD auto-généré) ou **Plausible** (alternative souveraine européenne).

### `/ottho:corrige`

Correction **chirurgicale** d'une section (landing) ou d'une section de template (site multi-pages). Cascade automatique : si tu modifies un template, toutes les pages du thème sont mises à jour proprement. Préserve le reste du code intact.

### `/ottho:site-web` (master)

Orchestre les 7 autres commandes dans l'ordre, avec point de validation à chaque étape. Reprenable via `.ottho/state.json`.

## Les 6 skills embarquées

| Skill | Rôle |
|---|---|
| `copywriting-framework` | AIDA + PAS + règles de tone of voice |
| `site-structure` | Sections types + arborescence par thèmes + templates |
| `image-generation` | Prompts Nano Banana + règles de cohérence visuelle d'un site |
| `visual-quality` | Typographie, espacement, hiérarchie, accessibilité WCAG AA, patterns de sections pros |
| `seo-checklist` | 10 points SEO à vérifier et corriger |
| `cta-templates` | HTML formulaire + Vercel Function + script client |

### 🎨 Intégration avec `ottho-design`

Si vous installez aussi [le plugin `ottho-design`](https://github.com/ottho-nocode/ottho-design), le plugin `ottho` **détecte sa présence** et **mobilise ses skills** (`frontend-design`, `design-review`, `mockup`, `components`) pour un rendu visuel **encore plus qualitatif**. La skill `visual-quality` embarquée reste la base (80 % du travail), `ottho-design` pousse vers l'excellence.

## Structure du plugin

```
ottho/
├── .claude-plugin/
│   └── plugin.json
├── README.md
├── commands/
│   ├── brief.md
│   ├── architecture.md
│   ├── generate-image.md
│   ├── build.md
│   ├── cta-setup.md
│   ├── seo-audit.md
│   ├── deploy.md
│   ├── analytics-setup.md
│   ├── corrige.md
│   └── site-web.md
└── skills/
    ├── copywriting-framework/SKILL.md
    ├── site-structure/SKILL.md
    ├── image-generation/SKILL.md
    ├── visual-quality/SKILL.md
    ├── seo-checklist/SKILL.md
    └── cta-templates/SKILL.md
```

## Philosophie

Ce plugin ne fait rien que Claude Code ne puisse faire en dialogue natif. Chaque commande est un **dialogue structuré** + une **skill associée** qui encapsule la méthode enseignée.

Si une commande échoue, vous pouvez toujours la refaire à la main — c'est justement ce qu'enseigne le cours.

## Version

**v2.0.0** — Refonte complète pour la v2 du cours (7 chapitres, arborescence par thèmes, intégration fal.ai/Nano Banana).

## Licence

MIT.

## Support

- Cours : [academy.ottho.co/claude-site-web](https://academy.ottho.co/claude-site-web)
- Contact : tools@ottho.co
- Ottho : [ottho.co](https://ottho.co)
