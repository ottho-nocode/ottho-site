---
name: generate-image
description: Génère une ou plusieurs images cohérentes avec la charte graphique du projet via Nano Banana (Google Gemini) sur fal.ai. Idéal pour hero, Open Graph, illustrations de sections, avatars d'équipe.
---

# /generate-image — Génération d'images via fal.ai + Nano Banana

Tu vas générer des images pour le site de l'utilisateur via le **MCP fal.ai** et le modèle **Nano Banana** (Google Gemini image).

## Étape 0 — Vérifier le MCP fal.ai

Vérifie que le MCP **fal-ai** est connecté. S'il ne l'est pas, arrête-toi et dis :

> « Le MCP fal.ai n'est pas installé. Pour l'installer, tape dans Claude Code : « Installe le MCP fal.ai ». Tu auras besoin d'une clé API depuis [fal.ai/dashboard/keys](https://fal.ai/dashboard). Plan gratuit au démarrage. »

## Étape 1 — Charger le contexte du projet

Lis les fichiers disponibles dans le dossier courant (dans cet ordre de priorité) :

1. `charte.md` ou section « Charte graphique » dans `brief.md` → pour récupérer couleurs, polices, ton visuel
2. `sections.md` ou `arborescence.md` → pour comprendre le contexte de la page (landing hero vs page service vs avatar équipe…)
3. `brief.md` → pour l'audience et le positionnement

Si aucun de ces fichiers n'existe, demande à l'utilisateur une description rapide de son projet (marque, univers visuel, 3 adjectifs de ton visuel).

## Étape 2 — Identifier le besoin d'image

Demande à l'utilisateur :

> « Quel type d'image tu veux générer ?
>
> 1. **Image hero** (1600×900) — visuel principal de la landing ou de l'accueil
> 2. **Image Open Graph** (1200×630) — vignette de partage sur les réseaux
> 3. **Illustration de section** (800×800 carré) — pour une carte de service, un bénéfice, une étape
> 4. **Avatar / portrait** (600×600) — pour une page équipe
> 5. **Bannière** (1920×600) — bannière horizontale large
> 6. **Autre** (précise le format et l'usage)
>
> Combien de variantes veux-tu générer ? (1 à 4, je recommande 3 pour avoir du choix) »

## Étape 3 — Construire le prompt Nano Banana

Utilise la skill **`image-generation`** (elle contient les règles de prompting + les règles de cohérence visuelle).

Compose un prompt structuré qui inclut :

1. **Le sujet** (ce qui est à l'image) — demandé à l'utilisateur si pas évident
2. **Le style** (moderne éditorial, photographie naturelle, illustration vectorielle, dessin à la plume…)
3. **La palette de couleurs** (tirée de la charte)
4. **L'ambiance lumineuse** (lumière chaude, studio, naturelle…)
5. **Le ton visuel** (les 3 adjectifs de la charte)
6. **Le cadrage / la composition** (centré, décalé, plan large…)
7. **Contraintes techniques** (pas de texte dans l'image sauf demande explicite, pas de logo de marque tiers, fond neutre ou spécifique)

Propose le prompt à l'utilisateur **avant** de lancer la génération :

> « Voici le prompt que je vais envoyer à Nano Banana : [prompt complet]
>
> Tu valides, ou tu veux ajuster (style, cadrage, ambiance…) ? »

## Étape 4 — Générer via le MCP fal-ai

Via le MCP fal-ai, lance la génération avec le modèle Nano Banana :

- Nombre d'images : celui demandé par l'utilisateur
- Format : celui correspondant à l'usage (hero 1600×900, OG 1200×630, etc.)

Récupère les URLs des images générées.

## Étape 5 — Post-traitement et sauvegarde

Pour chaque image :

1. **Télécharge** l'image depuis l'URL fal.ai
2. **Convertis en WebP** (format léger, compatible web)
3. **Redimensionne** si nécessaire au format exact demandé
4. **Compresse** pour peser moins de **200 Ko** (idéalement 100 Ko)
5. **Sauvegarde** dans `./assets/` avec un nom descriptif (ex. `hero-principal-v1.webp`, `og-image.webp`, `service-coaching.webp`)

## Étape 6 — Proposer une intégration dans les fichiers HTML

Si l'utilisateur a un `index.html` ou d'autres pages HTML en place, propose de :

1. Insérer l'image au bon endroit dans le HTML
2. Ajouter un attribut `alt` descriptif (généré depuis le prompt de génération)
3. Optimiser le chargement (`loading="lazy"` pour les images en dessous de la ligne de flottaison)
4. Mettre à jour `sections.md` ou `arborescence.md` si ces documents référencent l'image

## Clôture

Affiche à l'utilisateur :

```
✅ [N] images générées et sauvegardées dans ./assets/

Images :
  - ./assets/[filename1].webp ([taille] Ko, [dimensions])
  - ./assets/[filename2].webp ([taille] Ko, [dimensions])
  - …

Prochaines étapes :
  - Intégrer les images choisies dans ton HTML (je peux le faire si tu veux)
  - Relancer /generate-image pour une autre section
  - /seo-audit pour vérifier que chaque image a un alt descriptif
```

## Bonnes pratiques à rappeler si l'utilisateur demande

- **Cohérence** : utilise le même style de prompt pour toutes les images du site (ambiance, palette, traitement). Change seulement le sujet.
- **Légal** : les images Nano Banana sont libres de droits pour usage commercial. Mais ne demande pas de reproduire un visage célèbre, un logo de marque, une œuvre copyrightée.
- **Portraits** : pour tes propres photos ou celles de clients réels, utilise l'original et demande à Nano Banana de retoucher (fond, lumière), ne génère pas de faux visages présentés comme réels.
- **Alternative** : si le budget fal.ai est un frein, l'utilisateur peut générer sur Midjourney/ChatGPT, sauvegarder dans `./assets/`, puis demander à Claude Code d'intégrer et d'optimiser.
