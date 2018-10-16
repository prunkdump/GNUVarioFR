---
layout: page
title: Code
linkdesc: Le code source
linkmsg: Trouver !
linktarget: "https://github.com/prunkdump/arduino-variometer"
---

Pour compiler le code source GNUVario, vous avez besoin de l’IDE ​​Arduino pouvant être téléchargé [ici](https://www.arduino.cc/en/Main/Software). Commencez par l'installer et assurez-vous que vous pouvez lancer l'éditeur.

Ensuite, vous pouvez récupérer le code source GNUVario.

La méthode simple : télécharger le fichier zip
----------------------------------

Le code source peut être téléchargé directement sous forme de fichier zip sur [GitHub](https://github.com/prunkdump/arduino-variometer). Il suffit de cliquer sur **Clone or download** and **Download zip**.

![GitHub download zip]({{ "/assets/code/code1.jpg" | absolute_url }})

Extrayez le zip à l'emplacement de votre choix. Cela va créer un repertoire **arduino-variometer-master**.

![GitHub téléchargement zip]({{ "/assets/code/code2.jpg" | absolute_url }})

L’installation de l’EDI Arduino a normalement créé un répertoire **Arduino** dans votre dossier personnel. Assurez-vous que ce dossier est vide et copiez le **contenu** du dossier **arduino-variometer-master** dans le dossier **Arduino**.

![GitHub téléchargement zip]({{ "/assets/code/code3.jpg" | absolute_url }})
![GitHub téléchargement zip]({{ "/assets/code/code4.jpg" | absolute_url }})

La méthode avancée: utiliser Git
-----------------------------

Vous pouvez également obtenir le code source avec [Git](https://git-scm.com/). C'est une meilleure approche car avec Git, vous pouvez mettre à jour le code tout en gardant vos paramètres préférés.

Installez donc Git et assurez-vous que le dossier **Arduino** de votre répertoire personnel est vide. Avec le bash, allez dans le répertoire **Arduino**, clonez le référentiel et créez une branche pour vous.

{% highlight shell_session %}
~$ cd Arduino
~/Arduino$ git clone https://github.com/prunkdump/arduino-variometer.git .
~/Arduino$ git checkout -b myversion 
{% endhighlight %}

Ainsi, chaque fois que vous souhaitez mettre à jour le code, tapez les commandes suivantes à partir du répertoire **Arduino**. Les deux premières lignes enregistrent vos modifications. Les deux lignes suivantes téléchargent les modifications depuis la branche principale de GitHub. La dernière applique la modification à votre version.

{% highlight shell_session %}
~/Arduino$ git add -A
~/Arduino$ git commit -m 'change'
~/Arduino$ git checkout master
~/Arduino$ git pull
~/Arduino$ git checkout myversion
~/Arduino$ git merge master
{% endhighlight %}

Compiler le code
-----------------

Lancez maintenant l'IDE Arduino et ouvrez l'esquisse que vous souhaitez compiler. Par exemple, le croquis **variometer.ino**.

![GitHub download zip]({{ "/assets/code/code5.jpg" | absolute_url }})

Dans le menu **Outils**, **assurez-vous de choisir la bonne carte**. La plus classique de ce projet est l'**Arduino pro mini** avec le microcontrôleur **ATmega328P 3.3V**.

![GitHub download zip]({{ "/assets/code/code6.jpg" | absolute_url }})

Si vous utilisez le bootloader classique Arduino, il vous suffit de cliquer sur le bouton **téléverser**.

![GitHub download zip]({{ "/assets/code/code7.jpg" | absolute_url }})

Si vous avez installé le logiciel spécial GNUVario [bootloader]({{ site.baseurl }}{% link bootloader.md %}) vous devez créer un fichier de firmware. Dans le menu **Croquis**, choisissez donc **Exporter les binaires compilés**.

![GitHub download zip]({{ "/assets/code/code8.jpg" | absolute_url }})

Naviguez maintenant dans le dossier de l'esquisse et **renommez** le firmware **without bootloader** en **FIRM.HEX**.

![GitHub download zip]({{ "/assets/code/code9.jpg" | absolute_url }})
![GitHub download zip]({{ "/assets/code/code10.jpg" | absolute_url }})

Vous pouvez ensuite copier ce firmware sur la carte SD et effectuer la [procédure de flashing]({{ site.baseurl }}{% link bootloader.md %}).







