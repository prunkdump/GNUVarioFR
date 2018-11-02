---
layout: page
title: Configuration
---

Avant de configurer le variomètre, vous devez savoir comment [compiler le code]({{ site.baseurl }}{% link code.md %}). Assurez-vous de bien connaître l'IDE Arduino.

Toute la configuration du variomètre se fait en éditant le fichier [libraries/VarioSettings/VarioSetting.h](https://github.com/prunkdump/arduino-variometer/blob/master/libraries/VarioSettings/VarioSettings.h) dans votre dossier Arduino. Si vous n'avez pas d'éditeur adapté à l'écriture de code, vous pouvez le télécharger [Notepad++](https://notepad-plus-plus.org/).

Commencez par vérifier le fichier. Il y a deux parties: une partie concerne la configuration **du logiciel** et une partie la configuration du **matériel**. Chaque option est documentée par des commentaires directement à l'intérieur de ce fichier, lisez-les attentivement.

Pour "commenter" une ligne ajouter "//" au début. Cela désactive l'option.


1) Vérification de la configuration matérielle
--------------------------------------

Avant de commencer à installer le logiciel, vous devez **vous assurer que la configuration matérielle correspond à votre configuration**. Vous pouvez trouver quelques conseils dans la page [Schema]({{ site.baseurl }}{% link schematics.md %}).

Si vous utilisez un kit de pré-construction, voici les paramètres que vous devez modifier.

**GNUVario V2**

|         Option        |        Value                 |  
| :-------------------: | :--------------------------: |
| VARIOSCREEN_DC_PIN    |           2                  |
| VARIOSCREEN_CS_PIN    |           3                  |
| VARIOSCREEN_RST_PIN   |           4                  |

**GNUVario V3** (version en cours du PCB)

|         Option              |        Value                 |  
| :-------------------------: | :--------------------------: |
| VARIOSCREEN_DC_PIN          |           6                  |
| VARIOSCREEN_CS_PIN          |           7                  |
| VARIOSCREEN_RST_PIN         |           8                  |
| VARIOMETER_POWER_ON_DELAY   |         3000                 |
| VOLTAGE_DIVISOR_REF_VOLTAGE |          3.0                 |


2) Si nécessaire préparez la carte SD
---------------------------------

Si votre GNUVario possède un lecteur de carte SD, vous pouvez préparer votre carte SD maintenant.

**Si le bootloader du GNUVario est installé sur votre variomètre, vous devez effectuer cette étape** pour pouvoir flasher le micrologiciel avec les fichiers FIRM.HEX créés comme indiqué dans la page [compilation]({{ site.baseurl }}{% link code.md %}). Vous devez également connaître la [procédure de flashing]({{ site.baseurl }}{% link bootloader.md %}).

La carte SD doit être formatée avec une partition FAT16 inférieure à 2Go. Voici comment faire cela.

### Sur windows ou Mac

Télécharger et installer [Etcher](https://etcher.io/). Avec ce programme télécharger cette [image binaire]( {{"/assets/sdcard_fat16.zip" | absolute_url }} ) sur votre carte SD.

Vous **n'avez pas besoin d'extraire le zip**. Donnez l'image directement à Etcher.

### Sur Linux

**Assurez-vous de savoir ce que vous faites!** Je suppose que si vous utilisez Linux, vous avez certaines compétences pour comprendre ces étapes.

Localisez le périphérique correspondant à votre carte SD dans le dossier "/dev". Par exemple, "/dev/sdc". Vous pouvez brancher et retirer la carte SD pour en être sûr.

Avec *fdisk* en tant que root (utilisez *sudo* si nécessaire), créez une partition 1.5Go :

{% highlight shell_session %}
~# fdisk /dev/sdc

Welcome to fdisk (util-linux 2.32.1).
Changes will remain in memory only, until you decide to write them.
Be careful before using the write command.

Command (m for help): d
Selected partition 1
Partition 1 has been deleted.

Command (m for help): n
Partition type
   p   primary (0 primary, 0 extended, 4 free)
   e   extended (container for logical partitions)
Select (default p): p
Partition number (1-4, default 1): 
First sector (2048-30277631, default 2048): 
Last sector, +sectors or +size{K,M,G,T,P} (2048-30277631, default 30277631): +1.5G

Created a new partition 1 of type 'Linux' and of size 1.5 GiB.

Command (m for help): t
Selected partition 1
Hex code (type L to list all codes): 6
Changed type of partition 'Linux' to 'FAT16'.

Command (m for help): p
Disk /dev/sdc: 14.4 GiB, 15502147584 bytes, 30277632 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: dos
Disk identifier: 0x000ee832

Device     Boot Start     End Sectors  Size Id Type
/dev/sdc1        2048 3123199 3121152  1.5G  6 FAT16

Command (m for help): w
The partition table has been altered.
Syncing disks.
{% endhighlight %}

Et formater la partition avec *mkfs.vfat*
{% highlight shell_session %}
~# mkfs.vfat -F16 /dev/sdc1
{% endhighlight %}

3) Enregistrez vos informations personnelles
----------------------------------

Certains paramètres particuliers doivent être stockés dans la mémoire Arduino. Ces paramètres sont vos informations personnelles. Vous devez faire cette étape **juste une fois**. Ces réglages ne peuvent pas être supprimés simplement en faisant une mise à jour du variomètre. Vous devez utiliser un code spécial.

Ainsi, dans *VarioSettings.h*, définissez votre **Nom du pilote** et **Nom de la voile**.

Ouvrez l'esquisse *SetVarioParameters*. Compilez-la et téléchargez-la dans votre variomètre.

Lorsque vous allumez le variomètre, attendez trois bips hauts. Ce signal indique que vos paramètres personnels sont stockés dans la mémoire Arduino. Vous avez terminé pour cette étape.

4) Si nécessaire, calibrez l'accéléromètre
----------------------------------------

Si vous intégrez un accéléromètre, vous devez le calibrer.

D'abord sous Windows ou Mac, installez [Python version 2] (https://www.python.org/). Sous Windows, assurez-vous de cocher l'option **add to PATH variable**.

Ensuite, compilez et téléchargez le schéma **calibration_recorder**. Assurez-vous que votre carte SD est à l'intérieur du variomètre. Et suivez cette procédure:

<iframe width="560" height="315" src="https://www.youtube.com/embed/6yxoZcxxzVY" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe>

Cela créera un fichier "RECORD**.IGC" sur la carte SD. Copiez ce fichier dans le dossier "best-fit-calibration" et renommez-le si nécessaire en tant que "RECORD00.IGC".

Sous Windows ou Mac, lancez l'**Idle** Python et ouvrez le programme "best-fit-calibrage/calibrer.py". Appuyez sur "F5" pour exécuter.

Sous Linux, lancez simplement :

{% highlight shell_session %}
~$ cd Arduino/best-fit-calibration
~/best-fit-calibration$ python2 calibrate.py
{% endhighlight %}

Cela montrera les paramètres que vous devez remplacer dans votre fichier *VarioSettings.h*.

5) Liste des paramètres personnalisables
-----------------------------

Disponible dans le fichier *VarioSettings.h*, il est possible de personnaliser le fonctionnement du GnuVario

Voici les paramètres modifiables les plus importants et leurs descriptions

[VARIOMETER_BEEP_VOLUME]
Volume des Bips, valeur entre 0 et 10, 10 étant le volume maximum

[VARIOMETER_TIME_ZONE] 
Definit la zone horaire UTC (+2) en été (+1) en hivers 

[HAVE_SCREEN_JPG63]
Si cette option permet de choisir entre l'affichage par défaut ou l'affichage alternatif (JPG63).
En activant cette option, vous aurez toutes les fonctionnalitées de la branche 63 

[VARIOMETER_SINKING_THRESHOLD] 
Seuil de descente

[VARIOMETER_CLIMBING_THRESHOLD] 
Seuil de zérotage

[VARIOMETER_NEAR_CLIMBING_SENSITIVITY]
Seuil de montée

[VARIOMETER_ENABLE_NEAR_CLIMBING_ALARM]
Alarme de montée proche: signale que vous entrez ou sortez de la zone de montée proche

[VARIOMETER_ENABLE_NEAR_CLIMBING_BEEP]
Bip de zérotage: bip lorsque vous êtes dans une zone comprise entre VARIOMETER_CLIMBING_THRESHOLD et VARIOMETER_NEAR_CLIMBING_SENSITIVITY

[VARIOMETER_BASE_PAGE_DURATION] 
Temps d'affichage de la page principale (en millisecondes)

[VARIOMETER_ALTERNATE_PAGE_DURATION] 
Temps d'affichage de la page secondaire (en millisecondes)

[FLIGHT_START_VARIO_LOW_THRESHOLD]
[FLIGHT_START_VARIO_HIGH_THRESHOLD]
Vitesse verticale minimale en m / s (seuil bas / haut) permettant le déclenchement de l'enregistrement

[FLIGHT_START_MIN_SPEED]
Vitesse au sol minimale en km/h déclenchant l'enregistrement du vol

[VARIOMETER_RECORD_WHEN_FLIGHT_START]
Activée, cette option indique que l'enregistrement débutera quand le vol débutera (vitesse horizontale supérieure à FLIGHT_START_MIN_SPEED et vitesse horizontale inférieure à FLIGHT_START_VARIO_LOW_THRESHOLD ou supérieure à FLIGHT_START_VARIO_HIGH_THRESHOLD  

[ALARM_SDCARD}
Alarme de non présence de la carte SD

[ALARM_GPSFIX]
Bip indiquant que le GPS est opérationnel

[ALARM_FLYBEGIN]
Bip indiquant le début de l'enregistrement du vol

[HAVE_MUTE]
Active l'option MUTE, qui permet de couper et remettre le son en tapant sur le boitier

[VARIOMETER_DISPLAY_INTEGRATED_CLIMB_RATE]
Affichage du taux de chute moyen sur une période ou du taux de chute instantané si l'option n'est pas active

[VARIOMETER_INTEGRATION_TIME]
durée de l'intégration pour le calcul du taux de chute

[VARIOMETER_INTEGRATION_DISPLAY_FREQ]
fréquence d'affichage du taux de chute

[RATIO_CLIMB_RATE]
		1: Affichage de la finesse
		2: Affichage du taux de chute moyen
		3: Affichage des 2 informations en alternance dans la zone à droite de l'affiche du vario

5) Configuration taux de chute moyen

Si vous souhaitez utiliser l'affichage du taux de chute moyen, suivez cette procèdure

Avec le taux de chute "moyenné" sur une periode, vous pouvez garder les bips très réactifs mais afficher à l'écran votre bilan sur 5 secondes au plus.

Voici la procédure :

    a) Il vous faut la période de votre GPS ( même si c'est sûrement 1000 ms ). Chargez le sketch "gps-time-analysis" puis attendez le fix. Lorsque le GPS s'est bien stabilisé lisez le chiffre en deuxième position à l'écran. C'est la période du GPS.

    b) Dans VarioSettings.h entrez votre periode du GPS :

      	#define GPS_PERIOD 996 

	c) Si vous voulez un affichage du taux de chute intégré décommentez :

		#define VARIOMETER_DISPLAY_INTEGRATED_CLIMB_RATE

	d) Vous pouvez alors régler la durée de l'intégration ( ici 5s ) et la fréquence d'affichage ( ici 2 affichage par secondes )

		#define VARIOMETER_INTEGRATION_TIME 5000
		#define VARIOMETER_INTEGRATION_DISPLAY_FREQ 2.0

	Ces paramètres sont aussi utilisé pour la finesse.

	e) Si vous voulez afficher le taux de chute en alternance avec la finesse

		#define RATIO_CLIMB_RATE 2

		1: Affichage de la finesse
		2: Affichage du taux de chute moyen
		3: Affichage des 2 informations en alternance dans la zone à droite de l'affiche du vario


6) Télécharger le code du variomètre
-----------------------------

Vous pouvez maintenant définir tous les paramètres souhaités dans *VarioSettings.h*. Pour appliquer votre configuration, il vous suffit de compiler et de télécharger le schéma *Variometer*.

**Il n'est pas nécessaire d'exécuter à nouveau l'esquisse SetVarioParameters!**

Cette esquisse n’est nécessaire que si vous modifiez le **nom du pilote** ou le **nom de la voile**.

Vous avez terminé !















