
En PHP, deux types de problèmes peuvent survenir à l'éxecution : **les erreurs** et les **exceptions**.


# Erreurs

Les erreurs proviennent généralement du code ou de l'environnement d'exécution.
Elles sont la **responsabilité** du développeur et ne sont pas destinées à être gérées comme une logique applicative.


### Types d'erreurs

**Erreur de sytaxe (Parse error)

- Code invalide, le script ne s'éxecute pas du tout.

**Erreur fatale**

- Arrête immédiatement le script.

**Warning**

- Erreur non fatale, le script continue (génère un avertissement)

**Deprecated** 

- Fonctionnalité obsolète, encore utilisable mais vouées à disparaître.


# Exceptions

Une exception représente une situation anromale prévue par le développeur
C'est le mécanisme recommandé pour gérer les **erreurs applicatives**.

#### Principes

- Déclenchées avec `throw`
- Encadrées avec `try`
- Traitées avec `catch`
- Principalement utilisées en **programmation orientée objet**

#### Avantages

- Gestion structurée et contrôlée
- Le programme peut continuer proprement
- Permet un traitement différent selon le type de problème


## Gestion et journalisation

- Les exceptions doivent être gérées et souvent journalisées
- PHP fournit `error_log()` pour écrire dans un fichier de log.
- Les logs permettent de diagnostiquer les problèmes sans les afficher à l'utilisateur

