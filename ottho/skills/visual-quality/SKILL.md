---
name: visual-quality
description: Principes de qualité visuelle (typographie, espacement, hiérarchie, accessibilité, patterns de sections) à appliquer systématiquement lors de la génération HTML/CSS. Évite le rendu « générique IA » et produit des pages web qui ressemblent à du travail de designer. Invoquée par /ottho:build et /ottho:corrige.
---

# Skill : visual-quality

Cette skill **élève le niveau visuel** des pages HTML/CSS générées par le plugin. Sans elle, le rendu serait « correct mais générique ». Avec elle, le rendu est **professionnel et distinctif**.

## 🔗 Intégration avec `ottho-design` (si installé)

Si l'utilisateur a aussi installé le plugin `ottho-design` (marketplace officiel Ottho), **utilise ses skills en priorité** :

- `ottho-design:frontend-design` — règles distinctives pour interfaces web
- `ottho-design:design-review` — critique visuelle automatique
- `ottho-design:mockup` — génération de maquettes Pencil.dev
- `ottho-design:components` — bibliothèque de composants pré-faits

Détecte la présence du plugin via la liste des commandes disponibles (`/ottho-design:*`). Si présent, invoque ces skills AVANT de générer le HTML, pour piocher dans les patterns pros.

**Si `ottho-design` n'est pas installé**, applique les principes ci-dessous qui couvrent déjà 80 % du travail.

---

## 1. Typographie — la fondation

### Échelle typographique (système 1.25 — Major Third)

Base : 16px. Chaque taille = 1.25× la précédente.

```
12px  → mentions, footer, labels
14px  → texte petit, légendes
16px  → texte de base (≠ changer)
20px  → citations, intro de section
25px  → sous-titres (h3)
31px  → titres de section (h2)
39px  → grands titres (h1 sur mobile)
49px  → titre hero (h1 sur desktop)
61px  → display (titre marketing)
```

### Line-height par contexte

- **Titres** : 1.1 à 1.2 (compact)
- **Sous-titres** : 1.3
- **Corps** : 1.6 à 1.75
- **Boutons** : 1 (strict)

### Poids

Utiliser **4 poids maximum** dans tout le site :

- `font-weight: 300` — texte allégé (rare, élégance)
- `font-weight: 400` — corps par défaut
- `font-weight: 600` — sous-titres, boutons
- `font-weight: 800` — titres forts

### Tracking (letter-spacing)

- **Titres grands** (> 40px) : `letter-spacing: -0.02em` (resserré, moderne)
- **Corps** : `letter-spacing: 0` (défaut)
- **Labels / caps** : `letter-spacing: 0.05em` à `0.1em` (aéré)

### Pairings typographiques par ambiance

- **Moderne / tech** : Inter (tout)
- **Luxe / éditorial** : Playfair Display (titres) + Inter (corps)
- **Chaleureux / humain** : Cormorant Garamond (titres) + Inter (corps)
- **Minimal / épuré** : Söhne ou Suisse Int'l (tout)
- **Jeune / dynamique** : Outfit ou Sora (tout)

---

## 2. Grille d'espacement (base 8)

**Règle** : tous les espacements sont des multiples de 8. Résultat : alignement visuel parfait.

### Échelle

```
4px   (0.5× — exceptions : gap entre icône et label)
8px   (1× — espace entre petits éléments)
16px  (2× — espace entre éléments liés)
24px  (3× — padding interne des cards)
32px  (4× — gap entre éléments indépendants)
48px  (6× — espace entre blocs)
64px  (8× — padding vertical des sections)
96px  (12× — padding vertical entre sections majeures)
128px (16× — respiration desktop large)
```

### Règle de respiration

- **Mobile** : padding vertical de section = 64px
- **Tablette** : 80px
- **Desktop** : 96-128px

Les sections ne sont JAMAIS serrées — le blanc crée l'élégance.

### Grille horizontale

- **Container max-width** : 1200px pour contenu texte, 1440px pour hero visuels
- **Padding horizontal** : 24px mobile, 40px tablette, 80px desktop
- **Colonnes** : 12 colonnes classique, gap 24px

---

## 3. Hiérarchie visuelle — les 3 leviers

En ordre de puissance décroissante :

### Levier 1 — Taille (le plus puissant)

Un `<h1>` à 48px attire infiniment plus qu'un texte à 16px. Utilisez la taille pour hiérarchiser les messages.

### Levier 2 — Contraste

- **Fort** : couleur principale foncée sur fond clair
- **Moyen** : gris 70% sur fond blanc
- **Faible** : gris 40% sur fond blanc (pour mentions, métadonnées)

### Levier 3 — Espace blanc

Un élément **entouré d'espace** devient important. Le vide est un outil, pas un manque.

### Anti-patterns

- ❌ Tout en gras = rien en gras
- ❌ 5 couleurs d'accentuation = aucune hiérarchie
- ❌ Texte qui touche les bords = mal dégage

---

## 4. Couleurs — la règle 60/30/10

Pour chaque section, respecter :

- **60 %** de la couleur **neutre** (fond, texte secondaire)
- **30 %** de la couleur **secondaire** (éléments de support)
- **10 %** de la couleur **principale** (accents, CTA, highlights)

Jamais l'inverse. La couleur principale est **rare** et **impactante**.

### États des boutons (toujours définir)

- Normal
- Hover (léger assombrissement ~8 %)
- Active (plus marqué ~15 %)
- Disabled (opacité 50 %)
- Focus (ring visible pour clavier)

### Contraste WCAG AA (obligatoire)

- **Texte normal** : ratio ≥ 4.5:1 (lisibilité)
- **Texte large** : ratio ≥ 3:1
- **Liens** : ratio ≥ 3:1 avec le texte environnant

Vérification : outil navigateur (Lighthouse) ou [webaim.org/resources/contrastchecker](https://webaim.org/resources/contrastchecker/).

---

## 5. Accessibilité visuelle

### Touch targets (mobile)

- **Minimum 44 × 44 pixels** pour tout élément cliquable.
- Espace entre boutons : 8px minimum.

### Focus visible

**Jamais** supprimer les focus states (`outline: none`) sans les remplacer par un ring visible.

```css
:focus-visible {
  outline: 2px solid [couleur-accent];
  outline-offset: 2px;
}
```

### Reduced motion

Respecter la préférence utilisateur :

```css
@media (prefers-reduced-motion: reduce) {
  * { animation-duration: 0.01ms !important; transition-duration: 0.01ms !important; }
}
```

### Zoom 200 %

Le site doit rester utilisable si l'utilisateur zoome à 200 % (utile pour les malvoyants).

---

## 6. Patterns de sections qui fonctionnent

### Hero — 3 layouts éprouvés

**A — Split (titre gauche / visuel droite)**
```
┌─────────────────────────┬──────────────────┐
│ Titre fort              │                  │
│ Sous-titre              │   Image 60%      │
│ [CTA primaire]          │   (personne,     │
│ Trust indicator         │    produit,      │
│                         │    illustration) │
└─────────────────────────┴──────────────────┘
```

**B — Centered (titre + image en dessous)**
```
┌───────────────────────────────────────────┐
│            Titre fort (centré)            │
│            Sous-titre                     │
│            [CTA]                          │
│                                           │
│      Image large, 16:9 ou 4:3             │
└───────────────────────────────────────────┘
```

**C — Asymmetric (éditorial)**
```
┌──────────────┬────────────────────────────┐
│              │                            │
│ Titre XXL    │      [espace vide,         │
│ à 60% gauche │       composition          │
│              │       off-grid]            │
└──────────────┴────────────────────────────┘
```

### Cards de bénéfices / services

- **3 colonnes desktop / 1 colonne mobile**
- Icône 48px, titre 20-24px, description 16px
- Card background légèrement différent du fond (ex. blanc sur gris clair)
- `border-radius: 12px` à 20px (jamais rigide)
- `transition: transform 0.2s` au hover : `translateY(-4px)` subtil

### Section témoignages

- 1 témoignage grand format OU 3 en grille
- **Jamais** un carrousel auto (agressif, inutilisable accessibilité)
- Photo 80-120px cercle, nom, rôle, citation 60-120 mots

### Formulaires

- Labels **au-dessus** des champs (pas à côté)
- Messages d'erreur **en rouge sous le champ**, pas en popup
- Bouton de soumission aligné à gauche ou pleine largeur mobile
- États hover/focus/error distincts

---

## 7. Micro-interactions (sobres)

**Autoriser** (en moderation) :

- Hover sur cards et boutons : translation 2-4px + ombre subtile
- Fade-in au scroll sur les sections (150-300ms)
- Transition douce sur les couleurs (150ms)
- Smooth scroll entre ancres

**Interdire** :

- Animations répétitives en continu (pulse, bounce, shake)
- Carrousels auto-play
- Popup modales non sollicitées
- Parallax fort (cassure d'expérience)
- Son / vidéo auto-play

---

## 8. Check-list visuelle avant validation

Avant de déclarer une page « finie », vérifier :

- [ ] Grille d'espacement 8px respectée partout
- [ ] 4 poids typographiques max
- [ ] Hiérarchie claire en 3 secondes (test du plissement des yeux)
- [ ] Règle 60/30/10 couleurs
- [ ] Tous les éléments cliquables ≥ 44×44 mobile
- [ ] Focus states visibles
- [ ] Contrastes WCAG AA vérifiés
- [ ] Pas de mur de texte (paragraphes < 4 lignes)
- [ ] Images chargent correctement, alt descriptifs
- [ ] Cohérence entre sections (même style, même palette)
- [ ] Test à 200 % de zoom : lisible
- [ ] Test mobile (375px), tablette (768px), desktop (1440px)

---

## Usage dans les commandes du plugin

Cette skill est invoquée automatiquement par :

- `/ottho:build` — au moment de générer les pages HTML/CSS
- `/ottho:corrige` — quand une modification touche le visuel
- `/ottho:site-web` — via `/build` dans l'orchestration

**Ordre d'invocation** :

1. Si `ottho-design` est installé → invoquer ses skills d'abord
2. Sinon (ou en complément) → appliquer les principes de cette skill
3. Valider avec la check-list

## Principe guide

> **Le design générique vient d'une absence de choix.** Chaque décision (taille, couleur, espace, poids) doit être consciente et justifiable. Si tu hésites sur un paramètre, reviens aux principes ci-dessus — ils sont là pour trancher.
