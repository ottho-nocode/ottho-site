---
name: seo-audit
description: Audite les 10 points SEO d'un site HTML, propose les corrections, les applique sur accord. Génère aussi sitemap.xml et robots.txt s'ils sont absents.
---

# /seo-audit — Audit et correction SEO

Tu vas auditer le SEO du site courant selon la méthode du chapitre 4, appliquer les corrections, et produire un rapport avant/après.

## Étape 0 — Localiser les fichiers

Liste tous les fichiers HTML du projet (hors dossiers `node_modules`, `.vercel`, etc.). Vérifie la présence des fichiers `sitemap.xml` et `robots.txt` à la racine du site (ou dans `public/` selon la convention du projet).

## Étape 1 — Audit AVANT

Utilise la skill `seo-checklist` (10 points de contrôle). Pour chaque page HTML trouvée, produis un verdict pour chaque point :

### Les 10 points à auditer

| # | Point | Critère |
|---|---|---|
| 1 | `<title>` | Présent, unique par page, 55-60 caractères |
| 2 | Meta description | Présente, unique, 140-160 caractères |
| 3 | `<h1>` | Un seul par page, contient le mot-clé principal |
| 4 | Structure `<h2>` / `<h3>` | Hiérarchisée, au moins 2 h2 par page de contenu |
| 5 | Balises Open Graph | og:title, og:description, og:image, og:type présentes |
| 6 | Balise canonique | `<link rel="canonical">` présent |
| 7 | Attributs `alt` | Tous les `<img>` ont un `alt` descriptif |
| 8 | Paragraphes courts | Pas de pavé au-delà de ~4 lignes |
| 9 | `sitemap.xml` | Présent, URLs valides, dates récentes |
| 10 | `robots.txt` | Présent, référence le sitemap |

Pour chaque point : **OK** / **À corriger** / **Absent** + 1-2 lignes de justification.

Présente le rapport sous forme de **tableau markdown** pour chaque page, ou sous forme consolidée si plus de 3 pages.

## Étape 2 — Proposer les corrections

Liste les **corrections** à appliquer, dans l'ordre de priorité :

1. Corrections qui cassent le SEO si absentes (titre, h1, description)
2. Corrections qui améliorent significativement (Open Graph, canonique)
3. Corrections de forme (alt, paragraphes)
4. Fichiers manquants (sitemap, robots)

Demande à l'utilisateur : **« Veux-tu que j'applique toutes les corrections automatiquement, ou seulement certaines ? »**

## Étape 3 — Appliquer les corrections

### Pour les balises de contenu

Modifie directement les fichiers HTML :

- Génère les `<title>` uniques (55-60 caractères, promesse + marque) en utilisant le contexte de la page.
- Génère les meta descriptions (140-160 caractères, incitatives) idem.
- Corrige les `<h1>` en double (gardes-en un, passe les autres en `<h2>`).
- Ajoute ou améliore les balises Open Graph (utilise le titre et la description pour `og:title` et `og:description`).
- Ajoute les balises `<link rel="canonical">` pointant vers l'URL de production (demande à l'utilisateur le domaine canonique si pas trouvé dans le projet).
- Génère des `alt` descriptifs pour toutes les images (8-15 mots, factuel).
- Découpe les paragraphes > 4 lignes.

### Pour l'image Open Graph

Si aucune image OG n'existe (ou si elle est mal dimensionnée), propose à l'utilisateur :

> « Je peux générer une image Open Graph (1200×630) avec ton titre principal, ta charte graphique et ton logo. Ou tu préfères utiliser une image existante ? »

Si l'utilisateur accepte : génère l'image (via outil de génération si disponible dans la session, ou propose un SVG texte propre en fallback). Sauvegarde dans `./og-image.png` (ou `./public/og-image.png`).

### Pour sitemap.xml

Génère un `sitemap.xml` à la racine (ou dans `public/`) qui liste toutes les pages HTML trouvées, avec :

- `<loc>` : URL complète (demande le domaine canonique)
- `<lastmod>` : date du jour
- `<priority>` : 1.0 pour l'accueil, 0.8 pour les pages clés, 0.7 pour les pages secondaires

### Pour robots.txt

Génère un `robots.txt` minimal à la racine :

```
User-agent: *
Allow: /
Sitemap: https://[domaine-canonique]/sitemap.xml
```

Si l'utilisateur a des pages à exclure (admin, merci, test), ajoute les `Disallow:` correspondants.

## Étape 4 — Rapport APRÈS

Produis un nouveau tableau, identique au rapport AVANT, mais avec tous les points en **OK** (si ce n'est pas le cas, explique pourquoi un point ne peut pas être corrigé automatiquement).

Compare AVANT / APRÈS et montre les améliorations.

## Étape 5 — Rappels pour l'utilisateur

Dis en clôture :

> « L'audit SEO est terminé. Rappels importants :
>
> 1. **Soumets ton sitemap à Google Search Console** après ton déploiement en production (chapitre 4 du cours).
> 2. Google met **4 à 8 semaines** à indexer complètement un nouveau site. Sois patient.
> 3. Tape `site:[ton-domaine]` dans Google dans quelques semaines pour voir ce qui est indexé.
> 4. Pour aller plus loin : ajouter du contenu régulièrement (blog, cas clients) est ce qui fait vraiment grimper dans Google sur la durée.
>
> Prochaine étape recommandée : `/deploy` (preview) puis `/deploy --prod` quand tu es satisfait. »

## Cas particulier — site non encore déployé

Si le domaine de production n'est pas encore défini, utilise `https://[mon-domaine].fr/` comme placeholder dans le sitemap et la balise canonique. Signale à l'utilisateur qu'il faudra relancer `/seo-audit` une fois le domaine branché (au chapitre 5) pour mettre à jour les URLs.
