---
name: deploy
description: Déploie le site courant sur Vercel via le MCP Vercel. Preview par défaut. Utilise --prod pour un déploiement en production. Gère aussi l'ajout d'un domaine personnalisé.
arguments:
  - name: environment
    description: "preview (défaut) ou prod"
    required: false
  - name: domain
    description: "Domaine personnalisé à brancher (optionnel, ex. monsite.fr)"
    required: false
---

# /deploy — Mise en ligne sur Vercel

Tu vas déployer le site courant sur Vercel, soit en **preview** (URL de test) soit en **production** (URL définitive). Méthode du chapitre 5.

## Étape 0 — Vérifications

Avant de déployer :

1. Le **MCP Vercel** est-il connecté ? Si non, arrête-toi et indique la procédure.
2. Les fichiers essentiels sont-ils présents ? (au minimum : un `index.html`, `sitemap.xml`, `robots.txt`)
3. Si le dossier contient un `package.json`, vérifie qu'il n'y a pas de build pending (propose de relancer `npm run build` si configuration détectée).
4. Parse les arguments de la commande :
   - `--prod` ou `prod` → déploiement production
   - `--domain monsite.fr` → brancher ce domaine après déploiement

## Étape 1 — Pré-check avant production

**Si déploiement production** (`--prod`) : affiche la checklist et demande confirmation.

```
⚠️ Tu es sur le point de déployer en PRODUCTION. Cette version sera
visible par tout le monde sur ton URL officielle.

Checklist avant production :
  [ ] Le site fonctionne en local (testé)
  [ ] Le preview Vercel est testé (mobile + desktop)
  [ ] SEO audité (/seo-audit lancé)
  [ ] Formulaire fonctionne (si /cta-setup déjà exécuté)
  [ ] Aucune faute d'orthographe visible
  [ ] Tous les liens internes fonctionnent

Tu confirmes le déploiement en production ? (oui / non)
```

Si l'utilisateur dit "non", bascule sur un déploiement preview à la place.

## Étape 2 — Déployer

Via le MCP Vercel :

### Déploiement preview (défaut)

- Crée ou réutilise un projet Vercel pour ce dossier (nom suggéré : dérivé du nom du dossier, ou du `brief.md` si disponible).
- Lance un déploiement preview.
- Suis le statut en streaming (affiche les étapes : upload → build → deploy → ready).
- Récupère l'URL preview (`*.vercel.app`).

### Déploiement production (`--prod`)

- Lance un déploiement avec le flag `--prod`.
- Suis le statut.
- Récupère l'URL production.

## Étape 3 — En cas d'erreur

Si le déploiement échoue :

1. Récupère les **logs de build** via le MCP Vercel.
2. Analyse l'erreur (99% du temps : fichier manquant, typo HTML, chemin d'image invalide).
3. Propose une correction et demande confirmation à l'utilisateur.
4. Applique le fix.
5. Relance le déploiement.

## Étape 4 — Brancher un domaine personnalisé (si `--domain` fourni)

Via le MCP Vercel :

- Ajoute le domaine au projet.
- Récupère les **enregistrements DNS** à configurer (A + CNAME).
- Affiche-les à l'utilisateur sous forme claire :

```
Pour que ton domaine [monsite.fr] pointe vers Vercel, ajoute
ces enregistrements DNS chez ton registraire (OVH, Namecheap, etc.) :

Enregistrement A
  Nom : @
  Valeur : 76.76.21.21

Enregistrement CNAME
  Nom : www
  Valeur : cname.vercel-dns.com

Une fois ajoutés, la propagation prend entre 5 minutes et 24 heures.
Lance `/deploy --check-domain [monsite.fr]` pour vérifier le statut.
```

## Étape 5 — Clôture

Affiche un résumé clair :

```
✅ Déploiement [preview / production] terminé.

URL : [URL]

Prochaines étapes suggérées :
- Teste sur mobile (mode device du navigateur).
- Vérifie que tous les liens internes fonctionnent.
- Si preview OK → `/deploy --prod` quand tu es prêt.
- Si production → partage ton URL !
```

## Cas particulier — `--check-domain`

Si l'utilisateur passe `--check-domain [domaine]`, vérifie via le MCP Vercel :

- Les DNS sont-ils correctement propagés ?
- Vercel valide-t-il le domaine ?
- Le HTTPS est-il actif ?

Affiche le statut en clair (et propose de relancer dans 1h si en attente).
