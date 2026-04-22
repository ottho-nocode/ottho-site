---
name: copywriting-framework
description: Templates AIDA et PAS + règles de tone of voice. À utiliser pour rédiger toute accroche, hero, description, CTA d'un site web. Invoqué par /brief, /architecture, /site-web.
---

# Skill : copywriting-framework

Cette skill contient les deux frameworks de copywriting enseignés dans le cours *Claude + Site web* et les règles de tone of voice à appliquer.

## Framework 1 — AIDA

**Attention → Intérêt → Désir → Action**

Classique universel. À utiliser par défaut pour une landing page ou une page d'accueil, surtout si l'offre est positive et désirable (newsletter, ebook gratuit, formation, produit).

### Les 4 sections

#### A — Attention

**Le titre.** Une phrase qui accroche en 3 secondes. Évite le « Bienvenue sur… ». Préfère une **promesse** ou une **question provocante**.

- Bon : « Et si cette année, vous osiez enfin changer de cap ? »
- Mauvais : « Bienvenue sur notre blog ! »

#### I — Intérêt

**Le sous-titre + l'accroche.** Expliquer pourquoi ça le concerne. Nommer le problème qu'il ressent.

- « Vous sentez qu'il est temps de quitter votre job. Mais vous avez peur de faire le mauvais choix, de tout lâcher pour rien, de le regretter. »

#### D — Désir

**Section bénéfices + preuve sociale.** Faire visualiser la transformation. Donner des preuves (témoignages, chiffres, logos clients).

- « Recevez chaque semaine un message clair, sans jargon, pour avancer pas à pas vers une vie pro qui vous ressemble. »
- Témoignage : « [Citation courte et authentique d'un client réel] » — [Prénom], [rôle ou contexte].

#### A — Action

**Le CTA final.** Bouton, formulaire, ou formulation claire. Avec une promesse de ce qui va se passer.

- « Recevoir mon guide gratuit » (formulaire email + bouton)

## Framework 2 — PAS

**Problème → Agitation → Solution**

Plus direct, plus émotionnel. À utiliser quand l'audience cible ressent une douleur concrète (contentieux, urgence, stress, frustration).

### Les 3 sections

#### P — Problème

Nommer le problème précis. Pas d'ambiguïté.

- « Un litige, une négociation, un contrat à sécuriser ? Votre entreprise mérite un conseil juridique clair. »

#### A — Agitation

Appuyer là où ça fait mal. Décrire les conséquences. Rendre l'inaction coûteuse.

- « Chaque jour de retard dans un dossier vous coûte. Un avocat qui tarde à répondre, qui vous noie dans le jargon, ou qui vous surfacture, c'est trois problèmes, pas un. »

#### S — Solution

Se présenter comme la solution. Rassurer. Donner la prochaine étape.

- « Notre cabinet rassemble des experts [domaine], avec disponibilité sous 48h, forfait clair, et interlocuteur dédié. [CTA : Demander un rendez-vous] »

## Quand choisir lequel ?

| Situation | Framework |
|---|---|
| Produit/service positif, désirable | AIDA |
| Audience qui ressent une douleur ou un blocage | PAS |
| Newsletter, lead magnet, ebook, formation | AIDA |
| Cabinet de conseil, avocat, dentiste, urgence | PAS |
| Hésitation | **AIDA par défaut** |

## Règles de tone of voice

### Les 4 axes à décider pour chaque projet

1. **Formel ↔ Familier** : « Nous vous recommandons » vs « On vous conseille »
2. **Direct ↔ Nuancé** : « Faites ça » vs « Vous pourriez envisager »
3. **Chaleureux ↔ Distant** : « On sait que c'est dur » vs « Le processus peut s'avérer complexe »
4. **Sérieux ↔ Ludique** : « Un conseil juridique fiable » vs « On démêle le jargon les yeux fermés »

### Méthode des 3 adjectifs

Pour tout projet, choisir **3 adjectifs** qui décrivent le ton. Vérifier le **test de l'opposé** : un adjectif valide est un adjectif dont l'opposé est crédible pour un autre contexte.

- « Professionnel » : l'opposé (« amateur ») n'est pas crédible → **trop générique**, à remplacer.
- « Direct » : l'opposé (« prolixe ») est crédible ailleurs → **bon adjectif**.
- « Chaleureux » : l'opposé (« distant ») est crédible ailleurs → **bon adjectif**.

## La règle numéro 1 du copywriting

**Parler du lecteur, pas de soi.**

Dans chaque paragraphe, compter les occurrences de « je / nous / notre » vs « vous ». Le « vous » doit gagner.

Mauvais :
> « Nous sommes un cabinet qui accompagne les PME depuis 15 ans. Notre équipe est composée de 3 avocats expérimentés. »

Bon :
> « Votre entreprise mérite un conseil juridique clair. Vous travaillerez avec un interlocuteur dédié, disponible sous 48h. »

## Usage dans les commandes du plugin

Cette skill est invoquée par :

- `/brief` — pour générer le draft copywriting final
- `/architecture` — pour écrire le contenu des blocs (landing) ou pages (site)
- `/site-web` — via l'enchaînement des deux précédentes
