---
step: 2
description: Réglage de la configuration de l'alimentation
---

Le GNUVario propose deux modes d'alimentation **STANDARD** et **EXPERT**.

En mode **STANDARD**, toutes les cartes de composants peuvent être soudées directement. Ils sont alimentés avec la tension de 4.2v provenant de la batterie LiPo. Chaque carte étant équipée d’un régulateur 3.3v, cette source d’énergie peut être utilisée directement.

En mode **EXPERT**, les cartes des composants sont alimentées par la sortie du régulateur de l'Arduino. Et ce n'est **pas** bien d'alimenter un régulateur 3.3v avec une source 3.3v car le régulateur a une tension de chute. Donc, dans ce mode, vous devez désactiver les régulateurs de toutes les cartes. Et vous pouvez également changer le régulateur Arduino avec un régulateur de bonne qualité. De cette manière, vous pouvez considérablement améliorer la consommation électrique du variomètre.

Alors préparez un fil pour le mode d'alimentation que vous voulez :
{% include tutoimg.md name="IMG_6319.JPG" %}
{% include tutoimg.md name="IMG_6321.JPG" %}

Coupez les extrémités du fil très près du circuit imprimé. Garder en juste assez pour toucher les pointes avec le fer à souder.
{% include tutoimg.md name="IMG_6322.JPG" %}

Ensuite, soudez un bout et vérifiez le placement du fil avant de souder l’autre :
{% include tutoimg.md name="IMG_6323.JPG" %}
{% include tutoimg.md name="IMG_6324.JPG" %}
{% include tutoimg.md name="IMG_6326.JPG" %}


