---
layout: page
title: Bootloader
linkdesc: Le bootloader de la carte SD du GNUVario
linkmsg: Trouver !
linktarget: "/assets/optiboot_atmega328_pro_8MHz.hex"
---

Le GNUVario utilise un bootloader spécial qui charge les firmwares à partir de la carte SD. Cela évite d'ouvrir le variomètre chaque fois que vous souhaitez mettre à jour le code.

Flasher le firmware en utilisant le bootloader
-----------------------------------------

Si le bootloader est déjà installé, il suffit de [compiler]({{ site.baseurl }}{% link code.md %}) l'esquisse que vous voulez flasher. **Renommer** le firmware en **FIRM.HEX** et copier le sur la carte SD.

Allumez ensuite le variomètre avec l’écran face au sol. Et **durant** les trois bips, retournez le variomètre vers le haut. **Faites attention !** Le variomètre met à jour le micrologiciel, alors **ne l'éteignez pas trop tôt !**

<iframe width="560" height="315" src="https://www.youtube.com/embed/o-LqxW8vlXE" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe>

Installation du Bootloader sur une nouvelle carte Arduino :
--------------------------------------------------

Les étapes sont expliquées sur la page officielle [Arduino as ISP](https://www.arduino.cc/en/Tutorial/ArduinoISP). Vous avez besoin d’un Arduino supplémentaire pour graver le Bootloader. J'aime l'**Arduino Nano** car c'est une carte, petite et facile à utiliser.

### 1) Configurer la carte de programmation

Suivez ces étapes avec l'Arduino Nano. La carte va graver le Bootloader à l'intérieur du Pro Mini :
* Lancer l'IDE Arduino
* Dans le menu **Outils** **choisissez la carte Arduino Nano**
* Dans **Fichier** ouvrir **Exemples -> Arduino ISP**
* Téléchargez ce croquis à l'intérieur de la carte Nano

### 2) Connecter la carte Arduino variomètre

Avec une platine d'essai, connecter la carte de programmation à la carte variomètre comme indiqué :

| Arduino Nano   |   Arduino Pro mini      |
| :------------: | :---------------------: |
|      5V        |         RAW             |
|      GND       |         GND             |
|      13        |         13              |
|      12        |         12              |
|      11        |         11              |
|      10        |         RESET           |


Parfois, vous devez ajouter un condensateur entre RESET et GND sur la carte de programmation. Vous pouvez également utiliser des connecteurs à broches pour connecter l'Arduino pro mini sans le souder.

![Programming bootloader]({{ "/assets/tuto_img/IMG_6298.JPG" | absolute_url }})

### 3) Essayez de télécharger le bootloader Arduino par défaut

Suivez ces étapes pour préparer l'EDI :
* Lancer l'IDE Arduino
* **définir la carte de l'Arduino variomètre**. Habituellement, l'Arduino 328 pro mini 3.3V.
* Dans **fichier -> préférences** activer la sortie détaillée pour le téléchargement.
* Dans **Outils** définir le programmeur comme **Arduino as ISP**.
* Connectez le Nano et cliquez sur **Outils -> Graver la séquence d'initialisation**

Si cela fonctionne, vous savez maintenant comment graver un bootloader. Sinon, vérifiez votre câblage ou ajoutez le condensateur entre RESET et GND.

### 4) Graver le Bootloader du GNUVario

Dans le panneau inférieur de l'IDE Arduino, recherchez dans le code de téléchargement une commande permettant de télécharger un fichier **. Hex **.

Regardez le chemin de ce fichier et trouvez-le sur votre ordinateur.

Remplacez le fichier par le [GNUVario's bootloader]( {{"/assets/optiboot_atmega328_pro_8MHz.hex" | absolute_url }} ). **Conservez le même nom de fichier que le Bootloader Arduino d'origine !**

Recommencez maintenant la procédure de gravure du Bootloader. Vous avez terminé.









