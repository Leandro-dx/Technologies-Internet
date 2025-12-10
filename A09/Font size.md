
# Définition

La taille de texte par défaut des navigateur est d'environ 16px. Comme l'utilisateur peut changer cette valeur pour améliorer la lisibilité, il est conseillé d'utiliser des unités relatives (em, rem) plutôt que des pixels afin de rendre le texte plus accessible et adaptable.


# L'unité rem

L'unité rem est la référence en RWD car elle est toujours basée sur la taille du texte de l'élément racine (html).

Par défaut, 1rem = 16px.
Si l'on change la taille du ```<html>```, toutes les tailles en rem s'ajustent proportionnellement.
Garantit une mise en page cohérente et évite les accumulations

Pour que 1rem soit égal à 10px, il faut régler la taille de police à 62.5%.
```html { font-size: 62.5%; }```