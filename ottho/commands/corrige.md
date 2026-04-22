---
name: corrige
description: Applique une correction ciblée à une section d'une landing (sections.md) ou à une section d'un template d'un site multi-pages (arborescence.md), en cascadant automatiquement sur tous les fichiers HTML concernés. Préserve le reste du code intact.
---

# /corrige — Correction ciblée d'une section ou d'un template

Tu vas appliquer une correction **chirurgicale** à un site existant. Deux scénarios selon le type de projet :

- **Landing page** → correction d'une section du `sections.md` → répercussion sur `index.html`
- **Site multi-pages** → correction d'une section d'un template → répercussion sur toutes les pages du thème concerné

L'objectif : **ne toucher qu'à ce qu'il faut**, préserver le reste intact.

## Étape 0 — Détecter le contexte

Dans le dossier courant, cherche :

- `./sections.md` → site de type **landing page**
- `./arborescence.md` → site de type **site multi-pages**

Si aucun des deux n'existe, arrête-toi et dis :

> « Je ne trouve ni `sections.md` ni `arborescence.md` dans ce dossier. Pour pouvoir corriger proprement, il me faut un de ces documents de référence. Lance `/ottho:architecture` si tu n'en as pas encore. »

Si les deux existent : demande à l'utilisateur lequel concerne cette correction.

---

## Parcours A — Correction d'une LANDING (sections.md)

### A1. Identifier la section à corriger

Affiche la liste des sections actuellement dans `sections.md` :

```
Sections actuelles de ta landing :
  1. Hero
  2. Problème
  3. Solution
  4. Bénéfices
  5. Preuve sociale
  6. FAQ
  7. CTA final

Laquelle veux-tu corriger ? (numéro ou nom)
```

### A2. Afficher le contenu actuel

Une fois la section choisie, affiche son contenu actuel (tel qu'il est dans `sections.md`) :

```
Section [X] — [Nom]

Contenu textuel actuel :
  - Titre : « … »
  - …

Contenu visuel actuel :
  - …
```

### A3. Recueillir la correction

Demande à l'utilisateur :

```
Qu'est-ce que tu veux changer ?

Options :
  1. Réécrire complètement la section (je te pose les questions pour le
     contenu textuel et visuel)
  2. Modifier uniquement un élément spécifique (précise lequel : titre,
     sous-titre, CTA, visuel, layout…)
  3. Supprimer la section (attention : décale l'ordre des autres)
  4. Ajouter une nouvelle section avant ou après celle-ci
```

### A4. Appliquer la correction

1. **Mets à jour `sections.md`** — uniquement la section concernée, le reste intact.
2. **Mets à jour `index.html`** — régénère uniquement le bloc HTML/CSS de cette section, sans toucher aux autres sections.
3. Respecte la charte graphique existante (palette, typographie, espaces).
4. Préserve tous les attributs techniques (IDs, classes, `loading`, `alt`…).

### A5. Clôture

Affiche un diff résumé :

```
✅ Section [X] — [Nom] mise à jour.

Fichiers modifiés :
  - ./sections.md (section [X] uniquement)
  - ./index.html (bloc correspondant régénéré)

Changements clés :
  - [bullet 1]
  - [bullet 2]

Teste la landing en local pour valider visuellement.
```

---

## Parcours B — Correction d'un SITE MULTI-PAGES (arborescence.md)

### B1. Identifier le périmètre

Demande à l'utilisateur :

```
Que veux-tu corriger ?

  1. Une section d'un TEMPLATE (appliquée à toutes les pages d'un thème)
     → Exemple : ajouter une section « Témoignages » à toutes les pages
     du dossier /offres.

  2. Une section d'une PAGE SPÉCIFIQUE (déviation du template autorisée)
     → Exemple : ajouter une section unique à offre-premium.html sans
     toucher aux autres offres.

  3. Une PAGE TOP-LEVEL (accueil, contact, équipe…)
     → Exemple : refondre la section Hero de accueil.html.

  4. La STRUCTURE de l'arborescence (ajouter/supprimer/renommer un dossier
     ou une page).
```

### B2. Récupérer l'existant

Selon le choix :

- **Template** → ouvre le `template.md` du dossier thématique concerné (`/[theme]/template.md`) et affiche la liste ordonnée de ses sections
- **Page spécifique** (déviation) → ouvre le fichier `.html` concerné et liste ses sections
- **Page top-level** → ouvre le `section-[nom-de-page].md` correspondant à la racine (ex. `section-accueil.md`)

### B3. Recueillir la correction

Idem à A3, adapté :

```
Qu'est-ce que tu veux changer dans [template / page] ?

  1. Modifier une section existante (précise laquelle)
  2. Ajouter une nouvelle section (et à quelle position)
  3. Supprimer une section
  4. Réordonner les sections
```

### B4. Appliquer la correction

#### Cas 1 — Correction d'un TEMPLATE

1. **Mets à jour le `template.md`** du dossier thématique concerné (ex. `/offres/template.md`).
2. **Liste les fichiers HTML du dossier** (toutes les pages de `/offres/*.html`).
3. **Montre à l'utilisateur cette liste avant d'appliquer** :

   ```
   Le template /offres/template.md change. Ça va affecter :
     - /offres/offre1.html
     - /offres/offre2.html
     - /offres/offre3.html

   Je répercute la correction sur ces 3 pages ?
   Oui / Non / Exclure certaines pages (précise)
   ```

4. **Applique la modification** sur chaque page concernée :
   - Conserve le contenu spécifique de chaque page (hero titre, prix, cas d'usage…)
   - Ne modifie que la structure ajoutée/changée

**Cascade propre** : le `template.md` est la source de vérité du dossier. Toutes les pages qui ne dévient pas doivent le refléter strictement après la correction.

#### Cas 2 — Correction d'une PAGE SPÉCIFIQUE (déviation)

1. **Ne touche pas au `template.md`** (la page dévie intentionnellement).
2. **Mets à jour uniquement le fichier HTML concerné.**
3. **Note la déviation** dans la section « Variantes autorisées » du `template.md` du dossier :

   ```markdown
   ## Variantes autorisées

   - [page-spécifique.html] : ajoute une section « Témoignages » après « Tarif ».
   ```

Les autres pages du dossier restent fidèles au template standard.

#### Cas 3 — Correction d'une PAGE TOP-LEVEL

1. **Mets à jour le `section-[nom-de-page].md`** correspondant (ex. `section-accueil.md`).
2. **Régénère le fichier HTML** (`accueil.html`) en respectant le nouveau plan de sections.
3. Header et footer communs intacts (sauf si la modification les concerne — auquel cas voir cas 4).

Les autres pages top-level ne sont **pas affectées**.

#### Cas 4 — Modification de la STRUCTURE

1. **Mets à jour `arborescence.md`** : l'arbre du site.
2. **Crée, renomme, ou supprime** les dossiers et fichiers correspondants.
3. Si tu **crées une nouvelle page top-level** : génère aussi son `section-[nom].md` à la racine.
4. Si tu **crées un nouveau dossier thématique** : génère aussi son `template.md` (le définir avec l'utilisateur).
5. Si tu **renommes** une page : renomme aussi son `section-[nom].md` ou son `template.md`, et vérifie les liens internes (menu, footer, teasers).
6. Si tu **supprimes** une page : supprime son `section-[nom].md` correspondant, et vérifie les liens internes.
7. Si tu **supprimes un dossier thématique** : supprime aussi son `template.md` et retire le thème du header/footer commun.

### B5. Clôture

Affiche un diff résumé :

```
✅ Correction appliquée.

Fichiers modifiés :
  - ./arborescence.md (section [X])
  - ./[theme]/page-1.html
  - ./[theme]/page-2.html
  - ...

Changements clés :
  - [bullet 1]
  - [bullet 2]

Teste le site en local et vérifie :
  - La cohérence visuelle sur toutes les pages modifiées
  - Les liens internes si la structure a changé
  - Le responsive mobile
```

---

## Règles transverses

1. **Toujours afficher** ce qui va changer **avant** d'appliquer.
2. **Toujours montrer** la liste des fichiers affectés dans le cas d'une cascade.
3. **Jamais toucher** à ce qui n'est pas dans le périmètre de la correction.
4. **Préserver** la charte graphique et les balises SEO existantes (`<title>`, meta, Open Graph, alt, etc.) sauf demande explicite.
5. **Si le site est déjà déployé** : proposer en fin de correction de lancer `/ottho:deploy` (preview d'abord, prod ensuite).

## Dialogue type

```
Utilisateur : /ottho:corrige

Claude : Je vois que c'est un site multi-pages (arborescence.md trouvé).
         Que veux-tu corriger ?
         1. Une section d'un template
         2. Une page spécifique
         3. Une page top-level
         4. La structure

Utilisateur : 1 — je veux ajouter une section Témoignages dans toutes
              les pages de /offres.

Claude : OK. Le template Offres actuellement contient :
         1. Hero offre
         2. Ce qui est inclus
         3. Pour qui
         4. Tarif
         5. CTA

         Tu veux ajouter « Témoignages » à quelle position ?

Utilisateur : Après Pour qui, avant Tarif.

Claude : Compris. Le template devient :
         1. Hero offre
         2. Ce qui est inclus
         3. Pour qui
         4. Témoignages  ← nouveau
         5. Tarif
         6. CTA

         Pages affectées :
         - /offres/offre1.html
         - /offres/offre2.html

         Pour la section Témoignages, que veux-tu comme contenu ?
         (Je peux générer un placeholder, ou tu me donnes 2-3
         témoignages types à placer.)

Utilisateur : Placeholder pour l'instant, je les remplirai plus tard.

Claude : OK, je génère.
         [Met à jour arborescence.md]
         [Met à jour offre1.html — section Témoignages ajoutée avec
          3 emplacements placeholder, charte respectée]
         [Met à jour offre2.html — idem]

         ✅ Template Offres mis à jour avec la section Témoignages.
         Teste les 2 pages en local.
```

## Clôture

Après la correction, propose :

> « Correction appliquée. Si tu veux voir le résultat en ligne, lance `/ottho:deploy` (preview). Une fois validé : `/ottho:deploy --prod`. »
