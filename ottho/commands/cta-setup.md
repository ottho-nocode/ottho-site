---
name: cta-setup
description: Connecte le formulaire HTML du site à Resend (email) et Airtable (stockage) via une Vercel Function. Utilise les MCPs Resend, Airtable, Vercel en coulisses. Zéro clé API au clavier.
---

# /cta-setup — Formulaire opérationnel

Tu vas transformer le formulaire HTML du site (posé comme placeholder) en un formulaire qui envoie vraiment des emails et stocke les soumissions. Méthode du chapitre Hébergement du cours.

## Étape 0 — Pré-requis

Vérifie que les 3 MCPs suivants sont connectés :

- **Vercel MCP** (pour push env vars + redéploiement)
- **Resend MCP** (pour domaine d'envoi et clé API)
- **Airtable MCP** (pour création de base + token)

Si l'un manque, arrête-toi et dis :

> « Il me manque le MCP [nom]. Pour l'installer, tape « Installe le MCP [nom] » dans Claude Code. La procédure est couverte dans le chapitre Hébergement du cours. »

## Étape 1 — Collecter les infos manquantes

Avant de configurer, demande à l'utilisateur :

1. **Le nom de la base Airtable** (ex. « Inscrits newsletter », « Demandes de contact » — propose une valeur dérivée du type de site).
2. **L'email professionnel** qui recevra les notifications de soumission.
3. **Le contenu de l'email de bienvenue** (ou les lignes directrices : simple texte, pièce jointe PDF, lien vers ressource…).
4. **Le slug de la Vercel Function** (défaut : `/api/subscribe` pour une landing, `/api/contact` pour un site).

## Étape 2 — Créer la base Airtable

Via le MCP Airtable, crée la base avec les colonnes :

### Pour une landing (newsletter / lead magnet)
- `email` (texte)
- `prenom` (texte)
- `date_inscription` (date, auto = maintenant)
- `source` (texte, UTM si disponible)
- `ip` (texte, optionnel — anti-spam)

### Pour un site multi-pages (formulaire de contact)
- `nom` (texte)
- `email` (texte)
- `entreprise` (texte, optionnel)
- `telephone` (texte, optionnel)
- `objet` (texte)
- `message` (texte long)
- `date_demande` (date)
- `source` (texte)

Récupère l'**ID de la base** et l'**ID de la table**.

## Étape 3 — Préparer Resend

Via le MCP Resend :

- Vérifie qu'une clé API active existe (ou crée-en une nouvelle nommée « ottho »).
- Si l'utilisateur a un domaine Vercel personnalisé, **propose de vérifier ce domaine dans Resend** (DKIM + SPF) pour envoyer depuis une adresse pro (`hello@[domaine]`). Sinon, garder `onboarding@resend.dev` par défaut.

## Étape 4 — Pousser les variables d'environnement Vercel

Via le MCP Vercel, ajoute au projet courant, pour `production`, `preview`, `development` :

- `RESEND_API_KEY`
- `AIRTABLE_TOKEN`
- `AIRTABLE_BASE_ID`
- `AIRTABLE_TABLE_NAME`
- `RESEND_FROM_EMAIL` (l'adresse d'envoi)
- `NOTIFY_EMAIL` (l'email pro de l'utilisateur)

Confirme à l'utilisateur que les variables sont bien stockées côté serveur.

## Étape 5 — Générer la Vercel Function

Utilise la skill `cta-templates` et génère le fichier (`./api/subscribe.js` ou `./api/contact.js`). La Function :

1. Accepte uniquement **POST**.
2. Parse le body JSON.
3. **Valide** le format email. Rejette si invalide.
4. **Enregistre** la ligne dans Airtable (API REST).
5. **Envoie** deux emails via Resend :
   - Email de bienvenue au visiteur
   - Notification à l'email pro de l'utilisateur
6. Renvoie `{ ok: true }` en succès, `{ ok: false, error: "..." }` en échec.

Code clean, lisible, ~30 lignes maximum.

## Étape 6 — Mettre à jour le HTML

Identifie le fichier qui contient le `<form>` (landing : `index.html` ; site multi-pages : typiquement `contact.html`).

Modifie le `<form>` :
- `action="/api/[subscribe|contact]"`
- `method="POST"`

Ajoute un petit script JS côté client (inline ou fichier séparé selon la convention du projet) qui :

1. Intercepte la soumission (`e.preventDefault()`).
2. Sérialise le formulaire en JSON.
3. Fait un `fetch()` vers la Function.
4. Affiche un message de succès ou d'erreur visible au visiteur.
5. Vide le formulaire en cas de succès.

Style du message cohérent avec la charte graphique du site.

## Étape 7 — Redéployer et tester

- Lance un redéploiement preview via le MCP Vercel.
- Récupère l'URL preview.
- Propose un protocole de test :

```
Teste toi-même ton formulaire :
1. Ouvre [URL preview]
2. Soumets avec ton vrai email
3. Vérifie le message de succès
4. Vérifie ta boîte mail — l'email de bienvenue arrive en 2-10 secondes
5. Ouvre Airtable — la ligne doit apparaître
6. Vérifie ton email pro — tu dois recevoir la notification
```

## Si un test échoue

Propose :

> « Décris-moi précisément ce qui ne marche pas. Je récupère les logs Vercel via MCP et je débogue. »

Récupère les logs de la Function, analyse, corrige, redéploie. 99 % des problèmes : variable d'environnement mal nommée, faute de frappe dans le nom de table Airtable, email mal formé dans la config Resend.

## Clôture

Une fois tous les tests OK :

> « Ton formulaire est opérationnel. Chaque soumission :
> - 1 ligne ajoutée dans Airtable
> - 1 email de bienvenue au visiteur
> - 1 notification à toi
>
> Les clés API restent dans le coffre-fort Vercel — jamais dans ton HTML.
>
> Prochaine étape : `/seo-audit` avant la mise en production, puis `/deploy --prod`. »
