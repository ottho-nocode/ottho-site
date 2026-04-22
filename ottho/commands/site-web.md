---
name: site-web
description: Commande MASTER — enchaîne les 7 étapes du cours (brief → architecture → images → génération du site → CTA → SEO → déploiement → analytics) avec un point de validation à chaque étape. Reprenable via .ottho/state.json.
---

# /site-web — Orchestrateur complet

Tu vas orchestrer la création **complète** d'un site, de A à Z, en enchaînant les phases dans l'ordre, avec un **checkpoint** à chaque étape. L'utilisateur peut dire STOP à tout moment et reprendre plus tard.

## Étape 0 — Gestion de l'état

### Si `.ottho/state.json` existe

Lis le fichier. Il contient la progression (phase courante, choix faits, fichiers générés). Propose à l'utilisateur :

```
Je vois que tu as déjà avancé sur ce projet.
Progression actuelle : phase [X] sur 7.
Dernière étape complétée : [nom].

Tu veux :
1. Reprendre là où tu en étais (phase [X+1])
2. Recommencer depuis le début
3. Sauter à une phase spécifique (précise laquelle)
```

### Si le fichier n'existe pas

Crée `.ottho/state.json` :

```json
{
  "project_name": "[nom du projet, demandé à l'utilisateur]",
  "created_at": "[date ISO]",
  "current_phase": 0,
  "completed_phases": [],
  "site_type": null,
  "choices": {}
}
```

## Étape 1 — Annonce du parcours

Affiche :

```
🚀 /site-web — Création de site, de A à Z

Tu vas enchaîner 7 phases, avec une validation à chaque étape.
Tu peux dire STOP à tout moment, je garde ta progression.

Phase 1 — Brief (10 min)               [objectif, audience, ton, copy]
Phase 2 — Architecture (10 min)        [sections.md OU arborescence.md]
Phase 3 — Images (10-20 min)           [génération via fal.ai + Nano Banana]
Phase 4 — Génération du site (20 min)  [HTML/CSS/JS + test local]
Phase 5 — CTA opérationnel (10 min)    [Resend + Airtable + Vercel Function]
Phase 6 — SEO + Déploiement (15 min)   [audit + preview + prod]
Phase 7 — Analytics (10 min)           [GA4 ou Plausible + bandeau RGPD]

Temps total estimé : ~1h30 (si tu as déjà ta charte graphique prête).

On y va ? (oui / non)
```

## Phase 1 — Brief

**Si phase non complétée :**

1. Vérifie si `./brief.md` existe. Si oui : proposer de le relire et le valider, ou de le refaire.
2. Si non : lance la logique de `/brief` (dialogue 7 questions + génération `brief.md`).
3. **Checkpoint :** « Le brief est-il validé ? On passe à l'architecture ? (oui / stop) »
4. Marque Phase 1 complétée dans le state, note `site_type` (landing ou multi-pages).

## Phase 2 — Architecture

1. Lance la logique de `/architecture`.
2. Si landing → génère `sections.md`.
3. Si site multi-pages → génère `arborescence.md` avec templates par thème.
4. **Checkpoint :** « L'architecture est-elle validée ? On passe aux images ? (oui / stop / sauter les images) »

## Phase 3 — Images (optionnelle)

Si l'utilisateur ne veut pas de génération d'images automatique (il a déjà ses photos, ou pas de budget fal.ai), propose de **sauter cette phase**.

Sinon :

1. Vérifie que le MCP fal.ai est connecté. Si non, propose de l'installer.
2. Lance la logique de `/generate-image` pour chaque image critique identifiée dans `sections.md` / `arborescence.md` :
   - **Landing** : image hero, image Open Graph, éventuelles illustrations de sections
   - **Site** : image hero de l'accueil, image Open Graph, illustrations des pages thématiques, avatars éventuels
3. Sauvegarde toutes les images dans `./assets/`.
4. **Checkpoint :** « Les images sont validées ? On passe à la génération du site ? (oui / en générer d'autres / stop) »

## Phase 4 — Génération du site (HTML/CSS/JS)

Lance la logique de `/build` :

- Détecte le type de site (sections.md → landing, arborescence.md → site multi-pages).
- Charge la charte graphique (depuis brief.md ou charte.md).
- Intègre les images disponibles dans `./assets/`.
- **Landing** : un seul `index.html` avec header sticky (nav par ancres) + sections + footer léger.
- **Site multi-pages** : toutes les pages HTML, avec header + footer commun (dropdowns thématiques, menu burger mobile), sitemap.xml, robots.txt.
- SEO de base intégré (5 balises par page, structure H1/H2/H3).
- Responsive mobile-first.
- Formulaire HTML en placeholder (sera branché en Phase 5).

**Après génération :**
- Lance un serveur local pour prévisualisation (`python3 -m http.server 3000` ou équivalent).
- **Checkpoint :** « Le site est-il validé (desktop + mobile) ? On passe au CTA ? (oui / ajustements / stop) »
- Si ajustements : itère via `/corrige` ou dialogue direct, re-checkpoint.

## Phase 5 — CTA opérationnel

Lance la logique de `/cta-setup` :

- Vérifie les MCPs (Vercel, Resend, Airtable).
- Crée la base Airtable.
- Configure les env vars Vercel.
- Génère `/api/subscribe.js` (ou `/api/contact.js` selon le cas).
- Met à jour le formulaire HTML pour pointer vers la Function.

**Checkpoint :** « Le CTA est-il testé et opérationnel (soumission → Airtable + email reçu) ? On passe au SEO et au déploiement ? (oui / stop) »

## Phase 6 — SEO + Déploiement

1. Lance la logique de `/seo-audit` :
   - Audit AVANT → corrections proposées → application → audit APRÈS.
2. Lance la logique de `/deploy` en **preview**.
3. Propose un test de preview (URL + checklist).
4. **Sous-checkpoint :** « Preview validée ? On déploie en production ? (oui / ajustements / stop) »
5. Si oui : `/deploy --prod`.
6. Propose de brancher le domaine perso si l'utilisateur en a un.

**Checkpoint final :** « Le site est en production. On passe aux analytics ? (oui / stop) »

## Phase 7 — Analytics

Lance la logique de `/analytics-setup` :

- Demande GA4 (par défaut) ou Plausible.
- Installe la balise + bandeau RGPD (si GA4).
- Configure les événements personnalisés.
- Redéploie.

**Checkpoint de clôture.**

## Clôture — félicitations

Affiche :

```
🎉 Ton site est complet, en ligne, mesuré, opérationnel.

URL production : [URL]
Dashboard Analytics : [URL GA4 ou Plausible]
Base Airtable : [nom de la base]

Ce que tu viens d'accomplir :
  ✓ Brief clair et audience définie
  ✓ Architecture pensée (sections.md ou arborescence.md)
  ✓ Images générées et optimisées (si fal.ai utilisé)
  ✓ Site responsive en HTML/CSS/JS
  ✓ Formulaire opérationnel (Resend + Airtable + Vercel Function)
  ✓ SEO optimisé (5 balises + sitemap + robots)
  ✓ Hébergé sur Vercel en production
  ✓ Analytics actif

Prochaines étapes suggérées :
  1. Partage ton URL.
  2. Pour maintenir : une modification du template d'un thème
     répercutée sur toutes les pages du dossier en 2 minutes.
  3. Prochains projets : `/site-web` t'amène au même résultat
     en ~1h30 à chaque fois.
```

Archive le `state.json` en `.ottho/state-[date].json` (historique des projets).

## Gestion des interruptions

Si l'utilisateur dit "stop" à n'importe quel checkpoint :

- Enregistre l'état courant dans `.ottho/state.json`.
- Affiche : « OK, ta progression est sauvegardée. Relance `/site-web` quand tu veux reprendre. »
- Termine la commande proprement.

## Erreurs et reprises

Si une phase échoue (MCP déconnecté, clé expirée, etc.) :

- Affiche l'erreur clairement.
- Propose une solution (installer le MCP manquant, régénérer une clé, corriger la config).
- N'avance pas au checkpoint suivant tant que la phase n'est pas validée.
