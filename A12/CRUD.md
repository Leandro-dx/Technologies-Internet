
# Définition

CRUD est un acronyme qui désigne les quatre opérations fondamentales que l'on peut effectuer sur des données stockées dans une base de données relationnelle.

- Create : créer / insérer des données
- Read : lire / sélectionner des données
- Update : modifier des données
- Delete : supprimer des données

![[Pasted image 20251215234648.png]]


# Create

Insertion de données dans la table `user`

```
create table user (
    id int auto_increment primary key,
    name varchar(50),
    email varchar(254)
);
```

Cela crée une table :

| id  | name | email |
| --- | ---- | ----- |
|     |      |       |

**INSERT** sert à ajouter une ou plusieurs lignes dans une table SQL :

```
INSERT INTO table_name (col1, col2, ...)
VALUES (val1, val2, ...);
```

Cas de la table `user` :

- `id` est auto-incrémenté, donc pas besoin de le fournir lors de l'insertion
- Les colonnes listées après le nom de la table sont les seules à renseigner.

**Insertion simple** :

- Ajoute une ligne
- MySQL génère automatiquement l'`id`

```
insert into user (name, email)
values ('Alice Dupont', 'alice.dupont@example.com');
```


**Insertion multiple** : 

- Ajoute plusieurs lignes en une seule requête
- Chaque ligne reçoit l'`id` auto-incrémenté distinct

```
insert into user (name, email)
values ('Bob Martin', 'bob.martin@example.com'),
       ('Claire Lune', 'claire.lune@example.com');
```

**Insertion partielle** :

- Les colonnes non fournies prennent :
	- `NULL` si autorisé,
	-  ou leur valeur `DEFAULT`
- Ici, l'email devient `NULL` car non renseigné

```
insert into user (name)
values ('Daniel Morand');
```


# Read

La commande `SELECT` permet de récupérer des données depuis une table SQL.
Elle correspond à l'opération `READ` du CRUD

```
SELECT colonne1, colonne2, ...
FROM table_name
WHERE condition;
```

#### Sélection de données

- `SELECT` : récupère toutes les colonnes de la table
- `SELECT col1, col2` : récupère uniquement les commandes spécifiées (Ici col 1 et 2)

```
select name, email
from user;
```
#### Filtrage avec WHERE

- `WHERE` permet de sélectionner uniquement certaines lignes
- Les conditions sont évaluées lignes par lignes
- Exemple : filtrer sur une valeur précise (nom, id, etc.)

```
select *
from user
where name = 'Alice Dupont';
```
#### Gestion des valeurs NULL

- NULL = absence de valeur
- On ne compare jamais NULL avec `=` ou `!=`
- Utiliser `IS NULL` et `IS NOT NULL` permet d'inclure ou d'exclure des données incomplètes.

```
select *
from user
where email is not null;
```
#### Tri des résultats

- `ORDERED BY` permet de trier les résultats
- Par défaut : **ASC** (croissant)
- **DESC** = ordre décroissant
- Le tri s'applique après la sélection et le filtrage

```
select *
from user
order by name desc;
```


# Update

La commande `UPDATE` permet de modifier des données existantes dans une table SQL.
Elle agit sur une ou plusieurs lignes, selon la condition définie.

```
UPDATE table_name
SET col1 = value1,
    col2 = value2
WHERE condition;
```

#### Principe

- `SET` définit les nouvelles valeurs des colonnes
- `WHERE` indique quelles lignes doivent être modifiées
- Sans `WHERE`, toutes les lignes de la table sont mises à jour

#### Mise à jour d'un utilisateur spécifique

```
update user
set email = 'bob.newemail@example.com'
where name = 'Bob Martin';
```

#### Mise à jour de plusieurs colonnes

```
update user
set name = 'Claire Dupond',
    email = 'claire.dupond@example.com'
where id = 3;
```

#### Mise à jour conditionnelle

```
update user
set email = 'default@example.com'
where email is null;
```

#### Mise à jour sans WHERE

Modifie toutes les lignes de la table

```
update user
set email = 'updated@example.com';
```


# Delete

La commande `DELETE` est utilisée pour supprimer des enregistrements d'une table.
Une fois les données supprimées, elles ne peuvent pas être récupérées.

#### Supprimer un enregistrement spécifique

```
delete
from user
where name = 'Bob Martin';
```

#### Supprimer un enregistrement selon une condition

```
delete
from user
where email is null;
```

#### Suppression sans WHERE 🚨

```
delete
from user;
```

Cela supprime l'entièreté de la table !


# Jointure

Les **JOIN** permettent de combiner des lignes de plusieurs tables en se basant sur une relation, souvent entre une clé primaire et une clé étrangère.

Une jointure relie deux tables via une condition : 

`ON table1.pk = table2.fk`

Le résultat dépend du type de **JOIN** utilisé

#### INNER JOIN

- Ne retourne que les lignes ayant une correspondance dans les deux tables.
- Les lignes sans lien sont exclues

#### LEFT JOIN (LEFT OUTER JOIN)

- Retourne toutes les lignes de la table de gauche
- Si aucune correspondance n'existe dans la table de droite -> valeurs **NULL*

#### RIGHT JOIN (RIGHT OUTER JOIN)

- Retourne toutes les lignes de la table de droite
- Equivalent à **LEF JOIN** en inversant l'ordre des tables
- Peu utilisé en pratique

#### LEFT JOIN avec exclusion (anti-join)

- Combine un `LEFT JOIN` avec `WHERE ... IS NULL`.
- Permet de trouver les lignes de la table de gauche sans correspondance dans la table de droite.

![[Pasted image 20251216175111.png]]