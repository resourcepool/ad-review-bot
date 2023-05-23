# ad-review-bot

Ce projet a pour objectif de créer un batch avec RoR permettant de review un ensemble de petites-annonces avec des règles spécifiques.

`Temps de réalisation : Entre 1h et 3h`


## Briefing

L'agence Unicorn Inc édite GlitterParadise, un site de petites annonces vendant exclusivement des produits à paillettes.
Seulement voilà : certains vendeurs mettent en vente des faux produits. Ils récupèrent des photos d'articles connus et à la mode et les vendent à des prix très en dessous de la côte afin d'attirer
rapidement l'attention d'acheteurs.ses potentiel.les. Craignant de passer à côté de ces bonnes
affaires, des internautes versent parfois la totalité du paiement sans même demander plus de détails.


## Organisation
Créez un projet contenant un script que l'on peut exécuter depuis la ligne de commande qui implémente des règles en dur de détection d'arnaques sur des annonces.
Organisez votre projet de façon à ce qu'il soit facilement évolutif (sans overdesigner non plus), mettez en place tous les outils qui vous semblent pertinents et que vous aimeriez avoir dans un projet sur lequel vous travaillez (s'ils ne sont pas disponibles en local, listez-les et présentez leur usage en quelques mots).

Il n'est pas nécessaire de mettre en place une gestion des arguments de la ligne de commande, on pourra charger l'annonce directement dans les sources.  
Le modèle d'annonce est disponible dans le fichier `ad-sample.json`.

Pour simplifier le développement, on part du principe que **tous les champs sont systématiquement renseignés**.

 * **contacts** : informations du vendeur (nom, prénom, email, n° de téléphone)
 * **creationDate** : date de création de l'annonce
 * **price** : prix de vente de l'objet en euros
 * **publicationOptions** : liste des options auxquelles le vendeur a souscrit pour son annonce
 * **reference** : id unique
 * **item** : données de l'article (marque, modèle, version, catégorie et code EAN)

## Services tiers
Tous vos services fonctionneront **de manière asynchrone**.
Vous mockerez des appels à des services externes **fonctionnant en asynchrone** en
implémentant un sleep de 50ms, puis retournant la valeur spécifiée.

| Nom du service | Description | Paramètre(s) d'entrée | Retour |
| ---      |  ------  |  ------  |---------:|
| QuotationClient  | Service permettant de calculer la côte d'une annonce  | l'article (item)  | 35000  |
| BlacklistClient  | Service permettant de blacklister un code EAN  | le code EAN  | true si le code EAN vaut "9-782940-199617" false sinon  |

## Règles

| Nom de la règle | Description |
| ----- |  ------  |
| rule::firstname::length | le prénom doit faire strictement plus de 2 caractères  |
| rule::lastname::length | le nom doit faire strictement plus de 2 caractères  |
| rule:email:alpha_rate | la proportion de caractères alphanumériques (chiffres et lettres) par rapport au nombre total de caractères de la partie avant le '@' de l'email doit être strictement supérieure à 70%  |
| rule:email:number_rate | la proportion de caractères numériques par rapport au nombre total de caractères de la partie avant le '@' de l'email doit être strictement inférieure à 30%  |
| rule::price::quotation_rate | le prix de l'annonce doit être dans une fourchette de 20% autour de la côte calculée  |
| rule::reference::blacklist | la référence de l'annonce ne doit pas être blacklistée  |


## Retour
Votre script devra sortir un objet json dans le SIGOUT de ce format :
```
{
"reference": string,
"scam": boolean,
"rules": string[]
}
```

Afin d'aider le débogage et de pouvoir faire des statistiques sur les différentes règles, on souhaite pour une annonce savoir quelles règles ont déterminé qu'elle était une arnaque. 
Si au moins une de ces règles détermine que l'annonce est une arnaque renvoyer le champ scam à true et le nom des règles ayant déterminée que c'était une arnaque.

## Procédure
Clonez ou téléchargez le repo (pas de Fork ni de PR), faites votre implem, et pushez sur un repo public de votre handle Github.
