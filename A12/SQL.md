
# Définition

Le language SQL (Structured Query Language) est un langage de programmation conçu pour gérer et manipuler des bases de données.


# Convention de nommage


Les noms des bases, tables et attributs s'écrivent 

- En anglais
- En minuscules
- En snake_case
- 
Les noms des tables s'écrivent au singulier
Les attributs doivent utiliser des noms courts, mais explicites
Attention à ne pas employer des mots clés réservés SQL
- https://dev.mysql.com/doc/mysqld-version-reference/en/keywords-8-4.html
Les clés primaires portent généralement le nom de `id`

MySQL propose plusieurs familles de types de données :

- Entiers, décimaux
- Chaînes de caractères
- Date / heure
- JSON
- Binaires
- Spatiaux

La valeur **NULL** représente l'absence de valeur : 

- Ce n'est ni 0,
- ni une chaîne vide
- mais le fait qu'**aucune donnée** n'a été définie


# Création de tables


La commande `CREATE TABLE` sert à créer une table dans une base de données.

Elle permet de définir :

- Le **nom** de la table
- Les **colonnes**
- Le **type de données** de chaque colonne
- Les **contraintes** (clé primaire, étrangère, composite, etc.)

Exemple : 
```
create table employee
(
    id int primary key,
    first_name  varchar(50),
    last_name   varchar(50),
    department  varchar(50)
);

create table project
(
    id   varchar(10) primary key,
    name varchar(50)
);

create table skill
(
    id   int primary key,
    name varchar(50)
);

create table employee_skill
(
    employee_id int,
    skill_id    int,
    primary key (employee_id, skill_id)
);

create table employee_project
(
    employee_id int,
    project_id  varchar(10),
    primary key (employee_id, project_id)
);
```

Cela donne :

![[Pasted image 20251215225605.png]]


# Création de clés étrangères


La commande `ALTER TABLE` permet de **modifier une table existante**, notamment pour rajouter des **contraintes**, comme des **clés étrangères**.

Ici, elle sert à créer **les relations entre tables** et $ garantir **l'intégrité référentielle**.


```
alter table employee_project
    add constraint employee_project_employee_id_fk
        foreign key (employee_id) references employee (id),
    add constraint employee_project_project_id_fk
        foreign key (project_id) references project (id);

alter table employee_skill
    add constraint employee_skill_employee_id_fk
        foreign key (employee_id) references employee (id),
    add constraint employee_skill_skill_id_fk
        foreign key (skill_id) references skill (id);
```

#### Table `employee_project`

Ce que cela fait :

- `employee_id` doit exister dans `employee.id`
- `projet_id` doit exister dans `projet.id`

Il est impossible d'associer un employé inexistant à un projet, et inversement.

Cette table représente une relation N:N entre employés et projets.

#### Table `employee_skill`

Ce que cela fait : 

- `employee_id` doit exister dans `employee.id`
- `skill_id` doit exister dans `skill.id`

Un employé ne peut avoir que des **compétences existantes**
Une compétence ne peut être liée qu'à des **employés existants**


![[Pasted image 20251215231222.png]]

Cela sert à :

- Garantir la cohérence des données
- Empêcher les références invalides
- Rendre le modèle relationnel **solide et fiable**
- Matérialiser les **relations N:N** en SQL

## Restrictions sur les contraintes d'intégrité référentielle

Les **restrictions** définissent ce que MySQL doit faire lorsqu'un enregistrement **parent** est **modifié ou supprimé**, afin de préserver la cohérence des données.

### CASCADE

Tout modification ou suppression dans la table parent est répercutée automatiquement dans la table enfant.
Exemple : supprimer un projet supprime automatiquement ses lignes dans `employee_project`.

### SET NULL

Lors d'une suppression ou modification du parent, la clé étrangère dans la table enfant est mise à **NULL**..
L'enregistrement enfant est conservé, mais la relation disparaît.

### SET DEFAULT

Similaire à **SET NULL**, mais la clé étrangère prend une **valeur par défaut**.
Fonctionne seulement si un **DEFAULT** est défini sur la colonne.

### NO ACTION / RESTRICT

**Interdit** la suppression ou modification du parent tant qu'il est référencé.
**MySQL** bloque immédiatement l'opération.

### Comportement par défaut (MySQL)

Si rien n'est précisé :

- `ON UPDATE RESTRICT`
- `ON DELETE RESTRICT`

### Bonnes pratiques

- `ON DELETE RESTRICT` --> le plus **courant**, évite les suppressions accidentelles
- `ON DELETE CASCADE` --> pratique pour les tables de liaison (N:N)
- `ON UPDATE CASCADE` --> rare (une clé primaire ne devrait presque jamais changer)


# ID auto-incrémentés

Les ID auto-incrémentés sont des entiers générés automatiquement par MySQL à chaque insertion. Ils sont très souvent utilisés comme **clé primaire**.

Avantages :

- Unicité garantie : chaque ligne reçoit un identifiant unique.
- Simplicité : pas besoin de fournir ou gérer l'ID manuellement.
- Performance : très efficaces pour les index et les recherches

Exemple : 
```
create table user (
    id int auto_increment primary key,
    name varchar(50),
    email varchar(254)
);
```

Ici, `id` identifie chaque utilisateur, MySQL attribue automatiquement la valeur suivante lors d'une insertion, et celle-ci fonctionne sans même préciser l'ID.
