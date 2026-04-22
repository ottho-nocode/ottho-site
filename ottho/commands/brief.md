---
name: brief
description: Dialogue guidé pour produire le brief d'un projet de site web (objectif mesurable, audience, proposition de valeur, ton de voix, framework AIDA ou PAS). Génère brief.md dans le dossier courant.
---

# /brief — Rédaction du brief de projet

Tu vas accompagner l'utilisateur dans la rédaction du brief de son site web. Le cours ne contient plus de chapitre Préparation dédié — cette commande incarne donc à elle seule la méthode de cadrage.

## Protocole

Pose les **7 questions** suivantes **UNE PAR UNE**, en attendant la réponse avant de passer à la suivante. Ne jamais poser plusieurs questions d'un coup.

### Question 1 — Type de site

> « On commence par l'orientation : est-ce une **landing page** (un seul objectif unique : capter un email, inscrire à un événement, vendre un produit unique) ou un **site multi-pages** (plusieurs objectifs, plusieurs thématiques, SEO organique) ?
>
> Si tu hésites, décris-moi en 2 phrases ce que tu veux accomplir et je te propose l'orientation. »

### Question 2 — Objectif mesurable

> « Quel est ton **objectif principal mesurable** ? Donne-moi un chiffre (ex. "capter 50 emails par mois", "recevoir 5 demandes de RDV par semaine", "vendre 10 formations par trimestre").
>
> Un objectif flou rend toutes les décisions floues. Sois précis. »

### Question 3 — Audience cible

> « Décris-moi ton **audience cible** en 4 dimensions :
>
> 1. **Qui** (âge, métier, contexte professionnel ou personnel)
> 2. **Problème réel** (la douleur concrète qui la pousse à te chercher)
> 3. **Motivation profonde** (ce qu'elle veut vraiment, au-delà du problème apparent)
> 4. **Objections principales** (ce qui la freine avant d'agir)
>
> Donne-moi ces 4 éléments, même en quelques mots pour chaque. »

### Question 4 — Proposition de valeur

> « En **une seule phrase**, quelle est ta proposition de valeur ? Qu'est-ce que tu apportes à ton audience que personne d'autre n'apporte aussi bien ? »

### Question 5 — Ton de voix

> « Quels sont **les 3 adjectifs** qui décrivent ton ton de voix ? (ex. « chaleureux, inspirant, sans jargon » ou « sobre, direct, expert »)
>
> Test de l'opposé : un bon adjectif a un opposé crédible pour un autre acteur du même secteur. Évite « professionnel » ou « qualitatif » (trop génériques). »

### Question 6 — Framework copywriting

> « Pour l'accroche principale, tu veux utiliser **AIDA** (Attention → Intérêt → Désir → Action, classique universel) ou **PAS** (Problème → Agitation → Solution, punchy et émotionnel) ?
>
> AIDA par défaut si tu hésites. PAS fonctionne mieux quand ton audience ressent une douleur concrète et identifiable. »

### Question 7 — Contraintes particulières

> « Dernière question : y a-t-il des **contraintes particulières** ?
>
> - Mentions légales spécifiques (RGPD, professions réglementées)
> - Plusieurs langues
> - Compliance sectorielle (santé, juridique, finance)
> - Délais contraints
>
> Dis-moi tout ce qui est pertinent, ou "rien de particulier". »

## Génération du brief.md

Une fois les 7 réponses collectées, **utilise la skill `copywriting-framework`** (elle contient les templates AIDA et PAS) et **génère** un fichier `brief.md` dans le dossier courant avec la structure suivante :

```markdown
# Brief — [nom du projet]

## Type de site
[Landing page / Site multi-pages]
Justification : [critères de l'arbre de décision]

## Objectif mesurable
[chiffré, vérifiable à la fin du mois]

## Audience cible
- **Qui :** [âge, métier, contexte]
- **Problème :** [la douleur concrète]
- **Motivation profonde :** [ce qu'elle veut vraiment]
- **Objections :** [freins principaux]

## Proposition de valeur
[une seule phrase]

## Ton de voix
Trois adjectifs : [adj1], [adj2], [adj3]
[Notes de style : tutoiement/vouvoiement, longueur des phrases, mots à éviter, mots à privilégier]

## Copywriting
Framework choisi : [AIDA / PAS]

[Draft de 3-4 paragraphes rédigés selon le framework choisi, cohérent avec les réponses]

## Contraintes particulières
[mentions légales, langues, compliance, délais]

---

Brief généré le [date] par le plugin ottho.
```

## Vérifications finales

Avant de sauvegarder :

- [ ] L'objectif est **chiffré**.
- [ ] L'audience a les **4 dimensions** renseignées.
- [ ] La proposition de valeur tient en **une phrase**.
- [ ] Les 3 adjectifs de ton **passent le test de l'opposé** (non génériques).
- [ ] Le draft copywriting **nomme explicitement** chaque section du framework choisi (A/I/D/A ou P/A/S).
- [ ] Le choix landing/site est **justifié** par l'objectif.

Si un critère n'est pas rempli, demande à l'utilisateur de préciser avant d'écrire le fichier.

## Clôture

Une fois le fichier écrit :

> « Ton brief est prêt : `./brief.md`. C'est ta boussole pour tout le reste du projet.
>
> Prochaine étape : `/architecture` pour transformer ce brief en structure de site — `sections.md` pour une landing, ou `arborescence.md` pour un site multi-pages. »
