# ottho-site

> Marketplace Claude Code pour les plugins du cours [academy.ottho.co/claude-site-web](https://academy.ottho.co/claude-site-web).

## Plugins disponibles

### `ottho` — Compagnon du cours « Claude + Site web »

Produit, déploie et mesure une landing page ou un site web multi-pages :

- `/ottho:brief` — boussole de démarrage (objectif, audience, copywriting, ton)
- `/ottho:architecture` — structure (sections.md pour landing / arborescence.md pour site multi-pages avec templates par thème)
- `/ottho:generate-image` — images via Nano Banana (Google Gemini sur fal.ai)
- `/ottho:cta-setup` — formulaire opérationnel (Resend + Airtable + Vercel Function)
- `/ottho:seo-audit` — 10 points SEO corrigés + sitemap + robots
- `/ottho:deploy` — déploiement Vercel (preview ou production)
- `/ottho:analytics-setup` — GA4 (défaut, avec bandeau RGPD) ou Plausible
- `/ottho:corrige` — correction chirurgicale d'une section ou d'un template
- `/ottho:site-web` — commande MASTER qui enchaîne tout

Détail complet : [ottho/README.md](./ottho/README.md)

## Installation

Dans Claude Code :

```
/plugin marketplace add ottho-nocode/ottho-site
/plugin install ottho@ottho-site
```

## Pré-requis MCP

Les plugins Ottho utilisent les MCPs suivants (installation couverte dans le cours) :

- **Vercel MCP** (hébergement, domaines, déploiement)
- **Resend MCP** (emails transactionnels)
- **Airtable MCP** (stockage des soumissions de formulaire)
- **fal.ai MCP** (génération d'images — optionnel)
- **Google Analytics MCP** (lecture des données GA4 — optionnel, niveau avancé)

Tous s'installent par simple dialogue dans Claude Code. Procédure complète dans le cours.

## Version

**v2.0.0** — Refonte complète pour la v2 du cours (7 chapitres, arborescence par thèmes, intégration fal.ai/Nano Banana, zéro persona fixe).

## Licence

MIT — voir [LICENSE](./LICENSE).

## Support

- Cours : [academy.ottho.co/claude-site-web](https://academy.ottho.co/claude-site-web)
- Contact : tools@ottho.co
- Ottho : [ottho.co](https://ottho.co)
