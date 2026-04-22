---
name: cta-templates
description: Templates de Vercel Function + schéma Airtable + script JS client pour connecter un formulaire HTML à Resend + Airtable, sans exposer de clé API. Enseigné au chapitre 5 du cours Claude + Site web.
---

# Skill : cta-templates

Cette skill contient les templates techniques pour rendre un formulaire HTML opérationnel via Resend (email) + Airtable (stockage) + Vercel Function (pont serveur).

Utilisée par `/cta-setup` et `/site-web`.

## Architecture cible

```
┌─────────────────┐       ┌──────────────────┐       ┌─────────────────┐
│  Formulaire     │  →    │  Vercel Function │   →   │     Resend      │
│  HTML           │       │  /api/subscribe  │       │  (emails)       │
└─────────────────┘       └──────────────────┘       └─────────────────┘
                                   │
                                   ▼
                          ┌──────────────────┐
                          │    Airtable      │
                          │  (stockage)      │
                          └──────────────────┘
```

Les clés API ne quittent **jamais** Vercel — elles vivent en variables d'environnement, lues par la Vercel Function côté serveur.

## Schéma Airtable

Base type « Inscrits — [nom du projet] » avec la table suivante :

| Colonne | Type | Obligatoire | Note |
|---|---|---|---|
| `email` | Email | ✅ | Identifiant principal |
| `prenom` | Texte | ⚠️ | Utilisé pour personnaliser l'email |
| `date_inscription` | Date | ✅ | Auto : `NOW()` |
| `source` | Texte | ❌ | UTM source si disponible |
| `ip` | Texte | ❌ | Anti-spam (optionnel) |

Pour un formulaire de contact (site multi-pages), ajouter :

| Colonne | Type | Note |
|---|---|---|
| `nom` | Texte | |
| `entreprise` | Texte | |
| `telephone` | Texte | |
| `objet` | Texte | |
| `message` | Texte long | |

## Variables d'environnement Vercel

À pousser via MCP Vercel :

```
RESEND_API_KEY       = re_xxxxxxxxxxxxxxxxxx
AIRTABLE_TOKEN       = pat_xxxxxxxxxxxxxxxxxx
AIRTABLE_BASE_ID     = appXXXXXXXXXXXXXX
AIRTABLE_TABLE_NAME  = Inscrits
RESEND_FROM_EMAIL    = hello@[domaine]
NOTIFY_EMAIL         = [email professionnel]
```

## Template Vercel Function — `/api/subscribe.js`

Node.js, simple, lisible. Pas de dépendances externes autres que `fetch` natif.

```javascript
export default async function handler(req, res) {
  if (req.method !== 'POST') {
    return res.status(405).json({ ok: false, error: 'Method not allowed' });
  }

  const { email, prenom, source } = req.body || {};

  if (!email || !/^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(email)) {
    return res.status(400).json({ ok: false, error: 'Email invalide' });
  }

  const {
    RESEND_API_KEY, AIRTABLE_TOKEN, AIRTABLE_BASE_ID,
    AIRTABLE_TABLE_NAME, RESEND_FROM_EMAIL, NOTIFY_EMAIL
  } = process.env;

  // 1. Enregistrement Airtable
  const airtableRes = await fetch(
    `https://api.airtable.com/v0/${AIRTABLE_BASE_ID}/${encodeURIComponent(AIRTABLE_TABLE_NAME)}`,
    {
      method: 'POST',
      headers: {
        'Authorization': `Bearer ${AIRTABLE_TOKEN}`,
        'Content-Type': 'application/json'
      },
      body: JSON.stringify({
        fields: {
          email,
          prenom: prenom || '',
          date_inscription: new Date().toISOString(),
          source: source || 'direct',
          ip: req.headers['x-forwarded-for'] || ''
        }
      })
    }
  );

  if (!airtableRes.ok) {
    return res.status(500).json({ ok: false, error: 'Erreur Airtable' });
  }

  // 2. Email de bienvenue (au visiteur)
  await fetch('https://api.resend.com/emails', {
    method: 'POST',
    headers: {
      'Authorization': `Bearer ${RESEND_API_KEY}`,
      'Content-Type': 'application/json'
    },
    body: JSON.stringify({
      from: RESEND_FROM_EMAIL,
      to: email,
      subject: 'Bienvenue !',
      html: `<p>Bonjour ${prenom || ''},</p><p>Merci pour votre inscription. À très vite.</p>`
    })
  });

  // 3. Notification (à soi)
  await fetch('https://api.resend.com/emails', {
    method: 'POST',
    headers: {
      'Authorization': `Bearer ${RESEND_API_KEY}`,
      'Content-Type': 'application/json'
    },
    body: JSON.stringify({
      from: RESEND_FROM_EMAIL,
      to: NOTIFY_EMAIL,
      subject: `Nouvelle inscription : ${email}`,
      html: `<p>Nouveau : ${prenom || ''} <${email}> — source : ${source || 'direct'}</p>`
    })
  });

  return res.status(200).json({ ok: true });
}
```

**Points d'attention :**

- Le contenu de l'email de bienvenue doit être **personnalisé** avec les valeurs fournies par l'utilisateur dans `/cta-setup` (ne pas laisser le "Bienvenue !" générique).
- Pour envoyer un PDF en pièce jointe, utiliser le champ `attachments` de l'API Resend (base64 ou URL).
- Pour un formulaire de contact (pas juste newsletter), adapter les champs et l'email de réponse.

## Template — Formulaire HTML

Minimum viable pour une capture email :

```html
<form id="ottho-subscribe-form">
  <input type="email" name="email" placeholder="votre@email.fr" required />
  <input type="text" name="prenom" placeholder="Votre prénom" />
  <button type="submit">Recevoir le guide</button>
  <div id="ottho-subscribe-message"></div>
</form>
```

Adapter les classes CSS à la charte graphique du site (couleurs, typographie, espacements).

## Template — Script JS client

À ajouter au site (inline ou fichier séparé) :

```javascript
document.getElementById('ottho-subscribe-form')?.addEventListener('submit', async (e) => {
  e.preventDefault();

  const form = e.target;
  const data = Object.fromEntries(new FormData(form).entries());
  const messageEl = document.getElementById('ottho-subscribe-message');

  messageEl.textContent = 'Envoi en cours…';

  try {
    const res = await fetch('/api/subscribe', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify(data)
    });

    const result = await res.json();

    if (result.ok) {
      messageEl.textContent = 'Merci ! Vérifiez votre boîte mail.';
      form.reset();
    } else {
      messageEl.textContent = result.error || 'Une erreur est survenue.';
    }
  } catch (err) {
    messageEl.textContent = 'Erreur réseau. Réessayez dans un instant.';
  }
});
```

## Checklist sécurité

- [ ] Clés API **jamais** dans le HTML ou le JS côté client
- [ ] Clés API **uniquement** en variables d'environnement Vercel
- [ ] Validation de l'email côté serveur (pas seulement côté client)
- [ ] CORS géré par Vercel (par défaut, la Function n'est accessible que depuis le même domaine)
- [ ] Rate limiting minimum (optionnel pour démarrer, recommandé si trafic élevé)
- [ ] Pas de logs des emails en clair dans les logs Vercel (seulement les métriques)

## Usage

Invoquée par :

- `/cta-setup` — génération de la Function + mise à jour HTML
- `/site-web` — via `/cta-setup` dans l'orchestration
