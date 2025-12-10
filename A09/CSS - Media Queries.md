
# Définition

Les media queries permettent d'appliquer des styles différents selon l'appareil. Elles rendent possible les approches **Mobile first** ou **Desktop first**.
![[media-query.png]]


# Syntaxe

Les media queries utilisent le mot-clé ```@media``` suivi d'une condition précisant quels styles doivent s'appliquer.

Elle se compose de trois éléments
- **Media condition** :  ```and```, ```or```, ```not```, ```only``` --> opérateurs logiques.
- **Media type** : ```screen```, ```print``` ou  ```all``` --> écran ou imprimante.
- **Media feature :```width```,  ```orientation``` ou  ```resolution``` --> format


# Pixel CSS

Le pixel CSS n'est pas égal à un pixel physique, c'est une unité "logique" utilisée par le navigateur afin de garder une apparence cohérente sur tous les écrans.

**Résolution matérielle** : Nombre réel de pixels sur un écran (ex : 1920x1080)
**Résolution logique** : Taille perçue parce le navigateur.

Une image à basse résolution peut donc devenir floue sur un écran haute densité.

**DPR** = Device Pixel Ratio

**Images vectorielles** : Les images SVG restent nettes quelle que soit la résolution, car elles ne dépendent pas des pixels --> Solution idéale pour un bon rendu.









