
**PDO** (PHP Data Objects) est une API orientée objet permettant à PHP d'interagir avec une base de données via une couche d'abstraction.
Elle permet d'exécuter les opérations **CRUD** sans gérer les détails bas niveau du SGBD.

## PDO vs MySQLi

**MySQLi

- Spécifique à MySQL
- Procédural ou orienté objet

**PDO**

- Orienté objet uniquement
- Compatible avec plusieurs SGBD
- Gestion des erreurs via exceptions
- Support natif des requêtes préparées


## Connexion à la base de données


Création d'une instance `PDO` avec : 

- un **DSN** (type de base, hôte, base)
- un **username**
- un **password**

```
PDO::ATTR_ERRMODE => PDO::ERRMODE_EXCEPTION
```

La connexion doit idéalement être centralisée (fichier ou classe dédiée).


## Requêtes préparées

Les requêtes préparées séparent :

- la **structure SQL**
- les **données**

#### Avantages

- Protection contre les injections SQL
- Meilleures performances pour les requêtes préparées
- Code plus lisible et maintenable

#### Fonctionnement

1. Préparation de la requête `prepare`
2. Liaison des valeurs `bindParam` ou `bindValue`
3. Exécution `execute`

#### Placeholders

- `?` -> anonyme
- `:name` -> nommé (recommandé)

#### bindParam vs bindValue

**bindParam**

- Lie une variable par référence
- Valeur lue au moment du `execute`

**bindValue**

- Lie directement la valeur
- Plus simple et plus sûr


## Récupération des données

Fetch()

- Récupère une ligne à la fois
- Utilisé dans une boucle

fetchAll()

- Récupère toutes les lignes d'un coup
- Plus simple, mais consomme plus de mémoire

**fetchColumn()

- Récupère une seule colonne par ligne
- Utile pour des listes simples

#### Modes de récupération

`PDO::FETCH_ASSOC` -> tableau associatif
`PDO::FETCH_NUM` -> tableau indexé
`PDO::FETCH_BOTH` -> les deux


### Récupération d'un ID auto-incrémenté

`lastInsertID()` permet d'obtenir l'ID généré lors du dernier `INSERT`

Indispensable pour :

- créer une relation avec une autre table
- enchaîner plusieurs insertions liées

### Transactions

Les transactions garantissent l'atomicité

Soit tout réussi, soit rien n'est appliqué

#### Principe

- `beginTransaction()`
- `commit()` si tout est correct
- `rollBack()` en cas d'erreur

#### Utilité

- opérations multiples dépendantes
- éviter les insertions partielles
- préserver l'intégrité des données