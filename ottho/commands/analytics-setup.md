---
name: analytics-setup
description: Active Google Analytics 4 (défaut) avec bandeau de consentement RGPD, ou Plausible (alternative souveraine européenne) sans cookies. Configure les événements personnalisés.
arguments:
  - name: tool
    description: "ga4 (défaut) ou plausible"
    required: false
---

# /analytics-setup — Activation des analytics

Tu vas activer les analytics sur le site, avec la configuration adaptée à la conformité RGPD. Méthode du chapitre Analytics du cours.

## Étape 0 — Demander l'outil

Si l'utilisateur n'a pas spécifié `--plausible`, utilise **Google Analytics 4** par défaut (standard du marché, gratuit, fonctionnalités avancées).

### Si GA4

```
Pour activer Google Analytics 4, j'ai besoin de :

1. Ton ID de mesure GA4 (format G-XXXXXXXXXX)
   Tu le trouves dans analytics.google.com → Admin → Flux de données.
   Si tu n'as pas encore de propriété GA4, crée-la (2 minutes).

2. Une confirmation que tu acceptes qu'un bandeau de consentement RGPD soit
   ajouté au site (GA4 utilise des cookies — c'est obligatoire en Europe).
   Je génère un bandeau conforme : accepter / refuser / personnaliser,
   rejet par défaut.

Tu as ton ID de mesure ?
```

### Si Plausible

```
Pour activer Plausible (alternative souveraine européenne, sans cookies) :

1. Ton domaine enregistré dans Plausible (ex. monsite.fr)
   Tu le trouves dans plausible.io → Sites.

Rappel : Plausible est payant dès le début (9 $/mois), mais pas de bandeau
de consentement nécessaire.

Tu as ton domaine Plausible ?
```

## Étape 1 — Installer la balise

### Pour GA4

Injecte dans le `<head>` de toutes les pages HTML du site :

1. Le script Google Tag officiel (avec l'ID utilisateur).
2. Un wrapper de **consent mode v2** qui rejette tout par défaut, n'active GA4 qu'après consentement explicite.

Exemple à générer (adapté au projet) :

```html
<!-- Google tag (gtag.js) — bloqué par consent mode jusqu'à acceptation -->
<script async src="https://www.googletagmanager.com/gtag/js?id=G-XXXXXXXXXX"></script>
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}

  gtag('consent', 'default', {
    'analytics_storage': 'denied',
    'ad_storage': 'denied',
    'ad_user_data': 'denied',
    'ad_personalization': 'denied'
  });

  gtag('js', new Date());
  gtag('config', 'G-XXXXXXXXXX', { anonymize_ip: true });
</script>
```

### Pour Plausible

1 ligne :

```html
<script defer data-domain="monsite.fr" src="https://plausible.io/js/script.js"></script>
```

## Étape 2 — Bandeau de consentement (GA4 uniquement)

Génère un bandeau cohérent avec la charte graphique du site.

### Fonctionnalités minimum

- **Affichage à la première visite** (stocké en localStorage).
- **3 boutons** : « Tout accepter », « Tout refuser », « Personnaliser ».
- **Personnaliser** ouvre une modale avec toggles par catégorie (Essentiel / Analytics / Marketing).
- **Stockage** du choix en localStorage (expire après 13 mois).
- **Re-déclenchement** de `gtag('consent', 'update', ...)` avec les choix.
- **Bouton « Paramètres cookies »** accessible en footer en permanence.

Code à générer : `./consent-banner.js` + `./consent-banner.css` (inclus dans tous les HTML), ou inline selon la convention du projet.

Le bandeau doit être **sobre, non intrusif, rapide à comprendre**. Pas de texte juridique lourd.

## Étape 3 — Événements personnalisés

Propose à l'utilisateur :

```
Je peux configurer ces événements personnalisés (utiles pour mesurer les
conversions fines) :

- `form_submit` : soumission du formulaire
- `cta_click` : clic sur le bouton CTA principal
- `scroll_depth` : aux seuils 25 %, 50 %, 75 %, 100 %

Tu veux tous les activer ? (oui / non / sélectionner)
```

Ajoute le code JS correspondant. Adapte les sélecteurs au vrai HTML du site (pas de `document.querySelector('form')` en aveugle — identifie le bon élément).

### Pour GA4

```javascript
document.querySelector('form#[id-réel]').addEventListener('submit', () => {
  gtag('event', 'form_submit', { form_name: '[nom-descriptif]' });
});
```

### Pour Plausible

```javascript
document.querySelector('form#[id-réel]').addEventListener('submit', () => {
  window.plausible && plausible('form_submit');
});
```

## Étape 4 — Redéployer

Lance un déploiement preview via le MCP Vercel pour tester la balise + le bandeau (si GA4).

Propose un protocole de test :

```
Teste ton installation :

1. Ouvre l'URL preview en navigation privée (pour simuler une première visite).
2. GA4 uniquement : vérifie que le bandeau de consentement apparaît.
3. Accepte les cookies — ouvre l'inspecteur (F12) → Network → filtre
   "google" ou "plausible". Les requêtes partent ?
4. Attends 30-60 minutes puis vérifie dans ton dashboard :
   - GA4 → Rapports → Temps réel (tu dois voir ta visite)
   - Plausible → Dashboard (idem)
5. Teste la soumission du formulaire — l'événement form_submit doit
   remonter dans le dashboard.
```

## Clôture

> « Analytics actif. Tu vas commencer à voir des données dans 24-48h.
>
> Si tu veux voir les conversions fines (combien de clics sur un bouton sans soumission, parcours entre pages…), explore les rapports « Événements » et « Funnels » de GA4 ou le dashboard Plausible.
>
> Prochaine étape (si pas encore fait) : `/deploy --prod` pour passer en production. »
