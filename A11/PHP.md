
# Définition

Langage de script open source créé en 1994 par Rasmus Lerdorf. Il était au départ une simple boîte d'outils en C pour un site personnel, puis est devenu l'n des langages côté serveur les plus utilisés pour la création de sites web dynamiques.

Un site dynamique adapte son contenu selon l'utilisateur et les données.

L'objectif principal d'un script PHP est de :
- Générer du HTML automatiquement
- S'exécuter côté serveur
- Une intégration simple dabs une page HTML

Utilité de PHP :
- Création de sites interactifs
- Connexion facile aux bases de données (MySQL)
- Gestion des sessions, cookies, fichiers, formulaires
- Enorme écosystème (WordPress, Laravel, Symfony...)
- Fonctionne sur tous les systèmes et serveurs (Apache, Nginx...)

PHP est un langage interprété, il s'exécute à la volée, sans compilaton.
- Avantages : Développement rapide, portable
- Inconvénients : Moins performant, erreurs détectées uniquement à l'exécution, vulnérable si mal sécurisé.

Il est principalement un langage impératif, complété depuis PHP 5 par un solide support de la programmation orientée objet, et dont la conception s'inspire fortement de C pour la syntaxe, de Perl pour les sigils et expressions régulières, ainsi que de Java/Python pour ses concepts orientés objets.


# Fonctionnement

Le fonctionnement d'un serveur nginx avec PHP-FPM suit quatre étapes :

1.  Nginx reçoit la requête :
	- S'il s'agit d'un fichier statique, il le renvoie directement
	- Sinon, il transmet la requête à PHP-FPM
2. Communication FastCGI :
	- Nginx envoie la requête à PHP-FPM via un socket ou un port TCP
	- PHP-FPM exécute le script PHP
3. Retour de PHP-FPM à nginx :
	- PHP génère du HTML (ou JSON, XML) et l'envoie à nginx
4. Nginx renvoie la réponse au client
	- Le navigateur reçoit le HTML et affiche la page

![[nginx-php.png]]

Exemple :
```
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <title>Demo</title>
  </head>
  <body>
    <?php
    $text = 'Hello world!';
    echo $text;
    ?>
  </body>
</html>
```


# Syntaxe

## Fichiers et balises PHP

- Un fichier PHP est un fichier texte avec l'extension **.php**.
- Le code PHP se place entre `<?php ... ?>`
- Si un fichier contient _uniquement_ du PHP, on **ne met pas la balise fermante** `?>` afin d'éviter des erreurs de sortie.
- Tout de qui se trouve hors balises PHP est traité comme HTML

## Syntaxe de base

#### Commentaires

- `//` pour une ligne
- `/* ... */` pour plusieurs lignes

```
<?php
// Commentaire sur une seule ligne

/*
 * Commentaire sur plusieurs lignes,
 * utile pour documenter un bloc de code.
 */
```

#### Instructions

- Chaque instruction se termine par un point-virgule `;`

```
<?php
echo 'Hello';
$name = 'Alice';
$count = 3;
```

#### Variables

- Commencent par `$`
- Typage dynamique (le type dépend de la valeur)
- Types principaux : ** booléen, entier, float, string, array, object, resource, null
- Portée : variables globales, locales, superglobales (`$_GET`, `$_POST`, `$_SESSION`)

```
<?php
$name = 'Joe';
$Name = 'Bob';
echo "$name, $Name";    // output: Joe, Bob

$4site = 'not yet';     // invalid: starts with a number
$_4site = 'not yet';    // valid: starts with an underscore
```

## Chaînes de caractères

- `'texte'` --> **littéral**, variables non interprétées
```
	<?php
	$text = 'World';
	echo 'Hello $text';  // output: Hello $text
```
- `"texte"` --> variables et séquences interprétées (`\n`, `\t`)
```
	<?php
	$text = 'World';
	echo "Hello $text";  // output: Hello World
```
 - Concaténation avec `.` 
 ```
	<?php
	$text = 'The answer is ';
	$number = 42;
	echo $text . $number;  // output: The answer is 42
```
 - Echappement avec `\"` ou `\'`
 ```
	<?php
	echo 'Escape \' single quote';  // output: Escape ' single quote
```
 - Sytaxe `{}` pour éviter des ambiguïtés dans les chaînes :
	 `"Hello {$name}"`

## Types de données structurés

#### Booléens

- false si : `0`, `'0'`, `''`, `[]`, `null`.
- Tout le reste = true.

#### Entiers

- Plusieurs notations : **décimale**, `0x` (hex), `0o` (octal), `0b` (binaire).

#### Tableaux

- Deux syntaxes : `array()` ou `[]`.
- Types :
	- Indexés : (`[0 => 'apple']`) --> on utilise `[0]` comme étant la 1ère valeur tu tableau.
	- Associatifs (`['a' => 'apple']`) --> `'a'` est associé manuellement à `'apple'`.
	- Multidimensionnels (tableaux dans des tableaux).
- Parcours des tableaux avec `foreach`.
```
	<?php
	$array = [2, 40];
	$sum = 0;
	
	foreach ($array as $value) {
	    $sum += $value;
	}
	
	echo $sum; // 42
```

#### null

- Représente l'absence de valeur.
- `is_null()` pour tester si une variable est nulle.
- Attention : accéder à une variable non définie --> warning, il est recommandé d'utilisé `isset()` avant d'accéder à une variable incertaine.

## Structures de contrôle

#### Conditionnelles

- `if`, `else`, `elseif`, `switch`.

#### Boucles

- `while`, `do … while`, `for`, `foreach`.

#### Autres

- `break` pour quitter une boucle
- `continue` pour passer à l’itération suivante

#### Syntaxe alternative (utile dans les fichiers HTML/PHP)

```
<body>
  <?php
  $condition = true;
  if ($condition): ?>
    <p>La condition est vraie.</p>
  <?php else: ?>
    <p>La condition est fausse.</p>
  <?php endif; ?>
</body>
```
ou
```
<body>
  <?php $fruits = ['apple', 'orange', 'banana']; ?>
  <ul>
    <?php foreach ($fruits as $fruit): ?>
      <li><?= $fruit ?></li>
    <?php endforeach; ?>
  </ul>
</body>
```

## Fonctions et inclusion de fichiers

#### Fonctions

- Définies avec `function nom() { ... }`
- Acceptent des arguments (+ valeurs par défaut)
- `return` renvoie une valeur
- Support des **fonctions anonymes**

#### Inclusion de fichiers

- `include` : warning si fichier absent, le script continue
- `require` : erreur fatale si fichier absent, le script s'arrête immédiatement
- Version `_once` pour éviter plusieurs inclusions du même fichier

## Résumé global en une phrase

PHP utilise des fichiers `.php` contenant du code entre balises, une syntaxe impérative simple (variables, types dynamiques, chaînes, tableaux), des structures de contrôle flexibles, un système de fonctions complet, et permet d’organiser une application via l’inclusion de fichiers.