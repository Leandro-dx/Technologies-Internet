
# Définition 

Une base de données relationnelle stocke des informations dans des **tables**  organisées et liées entre elles grâce à des clés.
Elle permet de conserver de grandes quantités de données structurées et d'y accéder rapidement.

Un DMBS (Database Management System)  comme MySQL, MariaDB, Oracle etc. est le logiciel qui permet de créer, administrer et interroger ces bases

Le **modèle relationnel** organise les données en **tables**.

- Une **table** représente une entité.
- Un **enregistrement** (tuple) est une ligne représentant un élément unique.
- Un **attribut** (colonne) décrit une caractéristique de cette entité.

![[Pasted image 20251211200630.png]]


# Clés

Les **clés** assurent l'identification unique de données et les relations entre tables.

- **Clé primaire** : Identifie chaque ligne ; unique et non nulle.
- **Clé étrangère : Pointe vers une clé primaire d'une autre table ; garantit l'intégrité référentielle.
- **Clé candidate** : colonnes(s) pouvant servir de clé primaire ; unique et non nulle.
- **Clé composite** : Clé primaire composée de plusieurs colonnes.
- **Clé unique** : Impose l'unicité mais autorise les valeurs NULL.
- **Clé artificielle** : Identifiant généré sans signification métier.
- **Clé naturelle** : Identifiant réel (ex. numéro AVS) ; compréhensible mais parfois long, changeant ou non unique. 


# Contraintes d'intégrité des données

Elles assurent la cohérence et la validité des données.

**Contrainte d'intégrité d'entité** : chaque table doit avoir une clé primaire unique et non nulle.

**Contraintes de domaine** : définissent les valeurs autorisées, empêchant les données incorrectes.
- type (`INT`, `VARCHAR`, `DATE`, ...)
- longueur (`VARCHAR(255)`)
- format (`CHECK`, regex SQL)
- ensembles restreints (`ENUM`)

**Contrainte d'intégrité référentielle** : garantit la cohérence entre tables via les clés étrangères (ex. impossibilité de référencer un client inexistant)
- Ex. insérer dans la colonne `order.customer_id` une valeur n'existant pas dans `customer_id`
- Souvent associée à des actions :
  - `ON DELETE CASCADE`
  - `ON DELETE RESTRICT`
  - `ON UPDATE CASCADE`


# Normalisation

La normalisation organise les données d'une base relationnelle pour :

- Réduire les redondances
- Eviter les anomalies de mise à jour
- Garantir l'intégrité
- Rendre le modèle plus cohérent.

Elle s'appuie sur plusieurs niveaux appelés **formes normales (NF)**.

## 1NF

Aucune cellule ne doit contenir plusieurs valeurs

Une table est 1NF si :

- Chaque colonne contient une **valeur atomique** (simple, indivisible)
- Aucune liste dans une case
- Chaque ligne est identifiable (souvent via une clé)

Pour respecte la 1NF, on introduit un `ID Commande` qui permet d'identifier les commandes indépendamment du client.

Exemple conforme ✅ : 

| ID Commande | ID Client | Nom Client | Produit |
| ----------- | --------- | ---------- | ------- |
| 1           | 101       | Alice      | Pommes  |
| 1           | 101       | Alice      | Bananes |
| 2           | 102       | Bob        | Lait    |
| 2           | 102       | Bob        | Pain    |
Une seule valeur par colonne.


Exemple non conforme  ❌ : 

| ID Client | Nom Client | Produit         |
| --------- | ---------- | --------------- |
| 101       | Alice      | Pommes, Bananes |
| 102       | Bob        | Lait, Pain      |
Car il y a plusieurs valeurs dans la colonne `Produit`.

## 2NF

Pour qu'une table soit 2NF, elle doit déjà être en 1NF, et toutes les colonnes doivent dépendre entièrement de leur clé primaire.

Si l'on reprend l'exemple du 1NF, connaître seulement l'`ID Commande` permet de déterminer le client, sans avoir besoin du produit.

Hors ça ne doit pas être le cas en 2NF, car les colonnes non clé doivent **entièrement** dépendre de la clé primaire composite. Il ne doit pas y avoir de dépendance partielle.

Pour contourner cela, on scinde la table en deux tables distinctes :

La table `Commande`
- Clé primaire : `ID Commande`

| ID Commande | ID Client | Nom Client |
| ----------- | --------- | ---------- |
| 1           | 101       | Alice      |
| 2           | 102       | Bob        |
| 3           | 101       | Alice      |
Et la table `Détail`
- Clé primaire composite : (`ID Commande`, `Produit`)

| ID Commande | Produit |
| ----------- | ------- |
| 1           | Pommes  |
| 1           | Bananes |
| 2           | Lait    |
| 2           | Pain    |
| 3           | Pommes  |
| 3           | Bananes |

**Résulat**

- Plus aucune dépendance partielle
- Toutes les colonnes non-clé dépendent entièrement de leur clé primaire

## 3NF

Une table est en 3NF si :
- Elle est déjà en 2NF
- Il n'existe aucune dépendance transitive

Cela signifie qu'une colonne non-clé ne doit pas dépendre d'une autre colonne non-clé.
Toutes les colonnes non-clé doivent alors dépendre uniquement de la clé primaire, et directement.

Exemple de dépendance transitive :

Dans la table `Commande` issue de la 2NF :

| ID Commande | ID Client | Nom client |
| ----------- | --------- | ---------- |
|             |           |            |
`Nom client` dépend de `ID Commande` **via** `ID Client` : C'est une dépendance transitive

Pour respecter la 3NF, il faut séparer les données du client dans une table indépendante.

1. **Table** `Commande` : Contient uniquement les infos propres à la commande.
	- Clé primaire : `ID Commande`

| ID Commande | ID Client |
| ----------- | --------- |
| 1           | 101       |
| 2           | 102       |
| 3           | 101       |


2. **Table** `Client` : Contient les infos relatives au client
	- Clé primaire : `ID Client`
	
| ID Client | Nom Client |
| --------- | ---------- |
| 101       | Alice      |
| 102       | Bob        |


3. **Table** `Détail` : Liste les produits associés à chaque commande
	- Clé primaire composite : (`ID Commande`, `Produit`)
	
| ID Commande | Produit |
| ----------- | ------- |
| 1           | Pommes  |
| 1           | Bananes |
| 2           | Lait    |
| 2           | Pain    |
| 3           | Pommes  |
| 3           | Bananes |

