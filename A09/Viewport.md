
# Définition

Le viewport est la partie visible d'une page web sur l'écran de l'utilisateur, sans scroll. Sa taille dépend de l'appareil utilisé, il s'agit d'un concept important pour le RWD.


# Balise meta viewport

La balise ```<meta>``` permet d'adapter une page web à la taille de l'écran. Elle se met dans le code html, dans le ```<head>``` et indique au navigateur comment afficher la page par défaut selon l'appareil utilisé.

Exemple :
```<meta content="width=device-width, initial-scale=1 name="viewport">```

- ```width``` définit la largeur du viewport
- ```initiale scale ``` définit le niveau de zoom initiale de la page, selon la valeur qu'on lui attribue (1 étant la valeur par défaut, pas de zoom)

Les unités de base basées sur la taille du viewport sont :
- vw : %de la largeur
- vh : % de la hauteur
- vmin : % de la plus petite dimension
- vmax : % de la plus grande dimension