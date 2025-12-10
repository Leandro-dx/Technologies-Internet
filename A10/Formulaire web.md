
# Définition

Les formulaires web servent à collecter des informations précises saisies par un utilisateur. Ils permettent notamment l'inscription, la publication de contenu, la recherche et d'effectuer des sondages.

Types de formulaires :
- **Formulaire d'inscription** : Permet à un utilisateur de créer un compte avec ses infos personnelles
- **Formulaire de connexion** : Permet d'accéder à un espace privé grâce à un identifiant et un mot de passe.
- **Formulaire de recherche** : Permet de rechercher une ressource spécifique sur un site.
- **Formulaire de contact** : Permet d'envoyer un message, principalement au support d'un site web.
- **Formulaire de sondage** : Permet de répondre à un sondage.
- **Formulaire de réservation** : Permet de choisir une date et heure précise pour un rendez-vous, une réservation.


# Eléments de formulaire

Les éléments HTML d'un formulaire servent à collecter des informations auprès de l'utilisateur.

- ```<form>``` : contient tous les champs et définit l'envoi des données.
- ```<label>``` : associe un texte descriptif à un champ.
- ```<input>``` : champ polyvalent.
- ```<textarea>``` : champ de texte sur plusieurs lignes.
- ```<select>``` / ```option>```  : liste déroulante à choix multiples.
- ```<button>``` : bouton pour envoyer, réinitialiser ou exécuter une action.


# Fonctionnement

## Frontend (client)

- Le formulaire est construit en HTML
- L'utilisateur remplit les champs
- Les données sont envoyées au serveur (via POST ou GET)

## Backend (serveur)

- Le serveur reçoit les données
- Il les traite (validation, stockage, email, etc.)
- Il envoie une réponse au client (confirmation, erreur, redirection, etc.)

## Envoie des données

Les données d'un formulaire sont envoyées via la balise ```<form>``` qui indique **l'action** (adresse du serveur) et la méthode (POST ou GET)

### Méthode GET

- Données visibles dans l'URL
- Limitées en taille
- Utilisable pour recherche, filtres
- Sûr seulement pour données non sensibles
### Méthode POST

- Données dans le body de la requête
- Pas de limite importante
- Adaptées aux informations sensibles
- Utilisée pour créer ou modifier des données


# Identification des données

L'attribut **name** est essentiel dans un formulaire : il sert à identifier les données envoyées au serveur. Lors de la soumission, chaque champ est transmis sous forme de clé = valeur :

- la **clé** = name
- la **valeur** = ce que l'utilisateur a saisi

Il permet
- D'identifier chaque donnée.
- D'envoyer les informations correctement au backend.
- De regrouper certains champs (ex : les boutons **radio** dans le même **name**)
![[form-name-value.png]]
## Tableaux auto-indexés

Les noms des champs peuvent être utilisés comme tableaux grâce à la notation ```[]```. Chaque valeur envoyée crée un nouvel élément, avec un index généré automatiquement côté serveur.

Exemple :
```
<form action="./form-ctrl.php" method="post">
  <label>First name</label>
  <input name="user-data[]">

  <label>Last name</label>
  <input name="user-data[]">

  <label>Email</label>
  <input name="user-data[]">

  <button>Send</button>
</form>
```

## Tableaux à clés indexées

On peut également utiliser des **clés** dans les crochets pour obtenir un tableau associatif. Chaque clé à l'intérieur des crochets devient une clé du tableau côté serveur.

Exemple :
```
<form action="./form-ctrl.php" method="post">
  <label>First name</label>
  <input name="user-data[first-name]">

  <label>Last name</label>
  <input name="user-data[last-name]">

  <label>Email</label>
  <input name="user-data[email]">

  <button>Send</button>
</form>
```


# Identification des éléments

Chaque champ d'un formulaire doit être clairement identifié pour offrir une bonne accessibilité et une meilleure compréhension.

On associe un **label** à un **input** avec :
- ```for``` dans ```label```
- ```id``` dans l'input

Les deux doivent avoir la même valeur.
![[form-label-for-id.png]]
Exemple : 
```
<label for="username">Nom d'utilisateur</label>
<input id="username">
```

## Alternatives aux labels

Quand un label visible n'est pas possible, on peut utiliser des alternatives, même si cela reste à éviter, l'attribut ```<label>``` reste la meilleure option.

#### ```aria-label```
- Décrit un champ pour les lecteurs d'écran
- Invisible pour l'utilisateur

#### ```title```
- Affiche une infobulle
- Peu fiable pour l'accessibilité
- Ne doit pas être utilisé seul

#### ```placeholder```
- Donne un exemple dans le champ
- Ne doit **jamais** remplacer un label car
  - Disparaît au remplissage
  - N'est pas toujours lu
  - Pose des problèmes de contraste

Exemple :
```
<form action="./form-ctrl.php" method="post">
  <input
    aria-label="First name"
    name="first-name"
    placeholder="John"
    title="First name"
  >
  <input
    aria-label="Last name"
    name="last-name"
    placeholder="Doe"
    title="Last name"
  >
  <input
    aria-label="Email"
    name="email"
    placeholder="john.doe@example.com"
    title="Email"
  >
  <button>Send</button>
</form>
```

Ces alternatives sont à utiliser uniquement si l'attribut `<label>` ne peut pas fonctionner pour quelconque raison.


# Validation des données

La validation des données assure que les informations envoyées via un formulaire sont correctes, complètes et sécurisées.
Elle doit toujours être effectué côté **client** et côté **serveur**. 

## Validation côté client

Effectuée dans le navigateur avant l'envoi.
Permet de donner un retour immédiat à l'utilisateur, et ainsi éviter l'envoie des données incomplètes.

Elle utilise les attributs HTML : `required`, `type`, `min`, `max`, `pattern`, etc. et éventuellement du JavaScript pour des règles plus avancées

Elle peut être contournée, c'est pour cela qu'une validation côté serveur est nécessaire en plus de celle côté client, afin d'assurer la sécurité.

## Validation côté serveur

Indispensable en plus de la validation client.
Elle permet plus de sécurité, en évitant par exemple les injections ou les données falsifiées, et afin de garantir l'intégrité et la cohérence des données.

C'est le serveur (PHP, Python, Node, etc.) qui vérifie et traite définitivement les valeurs

## L'attribut `type` sur `<input>`

Il détermine le type de champ à afficher, ainsi que la validation native que le navigateur doit appliquer.

**`text`** : Champ de texte générique (valeur par défaut).

`<input type="text">`


**`radio`** : Boutons radio (une seule option sélectionnable parmi plusieurs).

`<input type="radio" value="1">`


**`checkbox`** : Case à cocher (option activable/désactivable).

`<input type="checkbox" value="1">`


**`email`** : Champ pour une adresse email, avec validation intégrée du format.

`<input type="email">`

![[form-input-email.png]]

**`number`** : Saisie numérique, avec flèches d'incrémentation.

`<input type="number">`

![[form-input-number.png]]

**`date`** : Sélection d'une date via un widget natif du navigateur.

`<input type="date">`

![[form-input-date.png]]
## L'attribut `pattern`et regex

`pattern` permet d'imposer un format précis via une **expression régulière**.
- Utilisé surtout pour les champs texte / email personnalisés.
- Fonctionne seulement côté client --> doit aussi être vérifié côté serveur

Exemple : regex pour un numéro mobile suisse.
```
((00|\+)41|0)7[35-9]\d{7}
```

## Validation CSS

Les pseudos-classes permettent d'afficher visuellement l'état des champs :

Elles s'appliquent sur les attributs `<input>` `<select>` et `<textarea>

Exemple de pseudo-classes :

- `:valid` → champ valide
- `:invalid` → champ invalide
- `:required` / `:optional`
- `:in-range` / `:out-of-range`
- `:read-only` / `:read-write`

Elles permettent d'améliorer l'expérience utilisateur grâce à un feedback immédiat

Exemple d'utilisation :
```
<form action="./form-ctrl.php" method="post">
  <label>First name</label>
  <input name="first-name">

  <label>Last name</label>
  <input name="last-name" required>

  <label>Email</label>
  <input type="email">

  <label>Comment</label>
  <textarea></textarea>

  <button>Send</button>
</form>
```

# Soumission du formulaire

La méthode recommandée est la suivante :

`<button type="submit">`

- Soumet le formulaire lors du clic
- Peut contenir : icônes, images, HTML…
- Mieux pour l’accessibilité
- Plus flexible pour le styling et le RWD
- Peut omettre `type="submit"` car c’est la valeur par défaut dans un `<form>`

Il y a 3 types de boutons que l'on peut utiliser, pas forcément pour soumettre le formulaire mais :

- `submit` : Soumission --> Envoie le formulaire
- `reset` : Réinitialisation --> Vide les champs
- `button` : Bouton neutre --> Ne fait rien par défaut, utile avec JS