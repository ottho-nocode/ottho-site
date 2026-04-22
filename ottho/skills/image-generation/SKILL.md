---
name: image-generation
description: Règles de prompting Nano Banana (Google Gemini) via le MCP fal.ai + règles de cohérence visuelle pour les images d'un site. À utiliser pour générer des hero, OG, illustrations, avatars cohérents avec la charte graphique. Enseigné au chapitre Design du cours Claude + Site web.
---

# Skill : image-generation

Cette skill contient la méthode de prompting pour Nano Banana (modèle d'image Google Gemini, accessible via fal.ai) et les règles de cohérence visuelle à respecter pour qu'un site ait l'air **professionnel et homogène**.

## Qu'est-ce que Nano Banana ?

**Nano Banana** est le nom de code (devenu officiel) du modèle d'image **Google Gemini**. C'est l'un des modèles les plus performants en 2026 pour :

- Générer des illustrations cohérentes avec une charte
- Transformer une photo existante (changer fond, lumière, style)
- Fusionner plusieurs images
- Suivre un style artistique précis
- Générer des bannières Open Graph aux bonnes dimensions

**Accès** : via le MCP `fal-ai` connecté à Claude Code. Clé API sur [fal.ai/dashboard/keys](https://fal.ai/dashboard).

---

## Structure d'un bon prompt

Un prompt efficace pour Nano Banana contient **7 éléments** :

1. **Sujet** — ce qui est à l'image (précis, concret)
2. **Style** — photographique / illustration / dessin à la plume / 3D render / vectoriel…
3. **Palette de couleurs** — tirée de la charte graphique du projet
4. **Ambiance lumineuse** — naturelle chaude / studio / crépuscule / high-key…
5. **Ton visuel** — les 3 adjectifs de la charte (ex. « chaleureux, épuré, lumineux »)
6. **Cadrage / composition** — plan large / gros plan / centré / rule of thirds…
7. **Contraintes** — pas de texte dans l'image (sauf demande explicite), pas de logo tiers, fond spécifique

### Modèle générique

```
[Sujet], [style], palette [couleurs], lumière [ambiance],
ton [adjectifs de la charte], cadrage [composition],
[contraintes spécifiques].
```

### Exemple de prompt Hero

```
Une personne en train de prendre des notes dans un carnet, assise à une
grande table en bois clair, photographie éditoriale moderne, palette
terracotta (#C44D2F) et crème (#F5EDE0), lumière naturelle chaude venant
de la gauche, ton chaleureux et inspirant, cadrage trois-quarts avec
espace négatif à droite pour le texte, pas de texte dans l'image,
arrière-plan flou.
```

### Exemple de prompt Open Graph (1200×630)

```
Composition graphique paysage 1200×630, fond uni [couleur principale] avec
un dégradé subtil, grand titre en [police titres charte] centré en blanc
sur le fond foncé, petite pastille décorative [couleur secondaire] en bas
à droite, style éditorial minimaliste et professionnel, pas d'image photo.
```

### Exemple de prompt Illustration de section

```
Icône illustrée représentant [concept : rapidité, sécurité, accompagnement…],
style ligne fine et aplats colorés, palette [couleurs charte], fond crème
clair, cadrage carré 800×800, centré, ambiance moderne et douce, pas de
texte, composition équilibrée.
```

---

## Les règles de cohérence visuelle d'un site

**Un site web ne doit pas ressembler à une galerie d'images sans lien entre elles.** Pour une cohérence professionnelle :

### 1. Même style partout

Choisis **un style dominant** (photographique OU illustration OU dessin) et tiens-toi à ce choix pour toutes les images du site. Mixer les styles = site amateur.

### 2. Même palette partout

Réutilise **la même palette** à toutes les images. Si ta couleur principale est `#0B2544` (bleu marine), elle doit revenir dans chaque image générée, même subtilement.

### 3. Même ambiance lumineuse

Si le hero est en lumière chaude de fin d'après-midi, les autres images du site doivent avoir une ambiance compatible (pas de lumière de studio froide puis de photo nocturne sombre).

### 4. Même traitement

Si tu utilises un filtre granuleux, des contours doux, un léger flou d'arrière-plan — **applique-le partout**.

### 5. Prompt de base commun

Crée un **prompt de base** qui contient le style + la palette + l'ambiance, et change uniquement le sujet pour chaque image.

---

## Dimensions par usage

| Usage | Dimensions |
|---|---|
| Hero landing ou accueil | 1600 × 900 ou 1920 × 1080 |
| Image Open Graph (partages sociaux) | **1200 × 630** (strict) |
| Illustration de section (carrée) | 800 × 800 |
| Avatar / portrait d'équipe | 600 × 600 |
| Bannière horizontale large | 1920 × 600 |
| Logo / pastille | 512 × 512 (format PNG, fond transparent si possible) |

---

## Post-traitement obligatoire

Après génération par Nano Banana :

1. **Conversion en WebP** (plus léger que JPG/PNG).
2. **Redimensionnement** au format exact demandé (si Nano Banana a généré plus grand).
3. **Compression** sous 200 Ko (idéalement 100 Ko).
4. **Nom de fichier descriptif** (`hero-principal.webp`, `og-image.webp`, `service-coaching.webp`).
5. **Sauvegarde** dans `./assets/`.

## Intégration HTML

Dans le fichier HTML, pour chaque image :

- `alt` descriptif (8-15 mots, factuel) — généré depuis le prompt
- `loading="lazy"` pour les images **en dessous** de la ligne de flottaison
- `loading="eager"` (ou par défaut) pour l'image hero **au-dessus** de la ligne de flottaison
- Attributs `width` et `height` pour éviter les sauts de layout

Exemple :

```html
<img src="./assets/hero-principal.webp"
     alt="[description factuelle de l'image en 8-15 mots]"
     width="1600"
     height="900"
     loading="eager">
```

---

## Règles légales à respecter

- Les images générées par Nano Banana sont **libres de droits** pour usage commercial (conditions fal.ai / Google).
- **Ne jamais demander** à reproduire :
  - Un visage de personne célèbre
  - Un logo de marque existant
  - Une œuvre copyrightée
- Pour les **portraits réels** (vous ou vos clients) : utilisez les photos originales et demandez à Nano Banana de les retoucher (fond, lumière) — ne générez pas de faux visages présentés comme réels.

---

## Alternative si pas de fal.ai

Si l'utilisateur n'a pas installé le MCP fal.ai (pour raison de budget ou de simplicité) :

1. Il peut générer les images sur Midjourney, DALL·E 3 (ChatGPT), Imagen 3 (Google AI Studio).
2. Sauvegarde des fichiers dans `./assets/`.
3. Claude Code s'occupe du post-traitement (conversion WebP, compression, intégration HTML).

Moins fluide, mais totalement valide.

---

## Usage dans les commandes du plugin

Cette skill est invoquée par :

- `/generate-image` — principal usage
- `/site-web` — via `/generate-image` dans l'orchestration (Phase 3)
