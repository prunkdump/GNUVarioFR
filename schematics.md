---
layout: page
title: Schémas
linkdesc: Le schéma de câblage du GNUVario
linkmsg: Trouver !
linktarget: "/assets/schematic.pdf"
---

Cette page explique comment assembler les composants GNUVario par vous-même. Si vous utilisez le circuit imprimé de GNUVario ou les kits préconfigurés, vous pouvez aller directement à la page [Construction]({{ site.baseurl }}{% link hardware.md %}).

De quoi avez-vous besoin :
--------------

Presque tous les composants sont optionnels sauf les cartes Arduino et ms5611. Vous pouvez vous adapter à vos besoins. Vous devez simplement éditer le fichier libraries / VarioSettings / VarioSettings.h et désactiver le composant que vous n'avez pas.

Par exemple, si vous n'avez que le haut-parleur, l'écran et le diviseur de tension, modifiez le fichier comme suit:

{% highlight c %}
/* Commenter ou décoummenter selon  */
/* ce que vous intégrez dans le variomètre  */ 
#define HAVE_SPEAKER
//#define HAVE_ACCELEROMETER
#define HAVE_SCREEN
//#define HAVE_GPS
//#define HAVE_SDCARD
//#define HAVE_BLUETOOTH
#define HAVE_VOLTAGE_DIVISOR
{% endhighlight %}


Donc vous avez besoin ou vous pouvez obtenir :
* Une carte Arduino (de préférence 3.3v, voir ci-dessous)
* Une carte ms5611 ( peut être combinée avec l'accéléromètre MPU9250 )
* Une carte MPU9250 ( peut être combinée avec le baromètre ms5611 )
* Un Haut-Parleur ou un buzzer ( avec une résistance de 120 Ohms si vous n'avez pas la puce L9110 ) 
* Un amplificateur L9110
* Un écran Nokia 5110
* Un module GPS
* Un module lecteur de carte SD
* Un module Bluetooth
* Une résistance 270k et 1M pour le diviseur de tension


Type d'alimentation :
-------------

Choisir un type d’alimentation appropriée pour votre GNUVario est une partie importante du travail. Comme il existe deux types de cartes Arduino (5v et 3.3v) et de nombreux types de batteries, il existe de nombreuses combinaisons possibles. Chaque configuration à une influence sur la consommation de courant globale et sur la stabilité.

La principale chose que vous devez comprendre est que tous les régulateurs de tension ont une tension de chute. Donc, si vous achetez une carte Arduino 5v, vous avez besoin de plus de 5v pour votre source d’alimentation. Si votre module GPS dispose d’un régulateur 3.3v, vous devez l’alimenter avec plus de 3,3v.

Comme presque tous les composants électroniques sont maintenant basés sur la tension 3.3v, **je vous conseille d'utiliser une carte Arduino 3.3v**. Cela réduira la consommation d'énergie et, étant donné qu'une batterie LiPo à une seule cellule peut fournir jusqu'à 4,2 v, vous pouvez les utiliser directement.

À l'exception de l'écran ms5611 qui nécessite une source 3,3v régulée, **toutes les cartes ont besoin d'une source d'alimentation supérieure à 3,3v**. Cette source d'alimentation porte la mention **RAW_V** sur les schémas.


### Si vous choisissez un arduino 5v

Les batteries simples fournissent une tension maximale de 4,2 v. Donc, vous avez trois possibilités :
* Vous achetez un régulateur de tension élévateur 5v pour votre batterie et vous connectez la sortie à la broche 5v de l'Arduino.
* Vous achetez un régulateur de tension de plus de 5 V et connectez la sortie à la broche RAW de l’Arduino.
* Vous mettez deux batteries LiPo en parallèle. Mais vous avez besoin d'un chargeur spécial LiPo.

Donc, pour chaque configuration, vous avez maintenant une source d'alimentation régulée 5v sur la broche 5V de l'Arduino. Vous pouvez directement utiliser cette source d'alimentation en tant que source d'alimentation **RAW_V** pour alimenter toutes les cartes de composants.

**!! Attention !!** Si vous alimentez la carte Arduino avec la broche RAW, la puissance fournie par la broche 5v passe par le régulateur de l'Arduino. Ce régulateur ne peut pas alimenter directement le buzzer, car il tire trop de courant. Donc, **ne connectez pas le buzzer directement à la broche 5v**. Vous devez ajouter une résistance de 120 ohms ou alimenter le signal sonore avec la batterie à l'aide de l'amplificateur L9110 (voir ci-dessous).

De plus, l'écran ms5611 nécessite une source d'alimentation 3,3v. Alors connectez-le à la broche 3.3v.

### Si vous choisissez un arduino 3.3v

Vous pouvez connecter directement la batterie LiPo à la broche RAW car elle fournit jusqu'à 4,2 volts. Mais vous ne pouvez pas alimenter les cartes composant avec la sortie régulée 3.3v car chaque carte possède déjà un régulateur 3.3v nécessitant une source d'alimentation supérieure à 3,3v.

Donc, vous devez alimenter toutes les cartes sauf si l'écran ms5611 directement avec la sortie de la batterie. Ceci est votre source d'alimentation **RAW_V**. Connectez toutes les cartes des composants à la broche **RAW**.


Câblage du variomètre :
---------------------

Vous avez maintenant votre source d'alimentation **RAW_V** qui fournit plus de 3,3 v et une source d'alimentation régulée **3,3 v**. Vous pouvez maintenant connecter tous les composants suivant ce [schéma] ({{"/assets/schematic.pdf" | absolute_url}}) ou le tableau ci-dessous. **Attention** les numéros de broches ne sont valables que pour les Arduino basés sur la puce Atmega328 (P).

**The ms5611 and MPU9250 board**

|    ms5611 board  |     Arduino    |  
| :--------------: | :------------: |
|       SDA        |     SDA (A4)   |
|       SCL        |     SCL (A5)   |
|       VCC        |       RAW_V    |
|       GND        |       GND      |

**haut parleur ou buzzer** (sans amplificateur)

|     Buzzer           |     Arduino    |  
| :------------------: | :------------: |
|  + -> résistance 120k|       D9       |
|       -              |       D10      |

**Haut parleur ou buzzer** (Avec un amplificateur L9110, voir [datasheet](https://www.elecrow.com/download/datasheet-l9110.pdf) )

|      Buzzer      |      L9110     |  
| :--------------: | :------------: |
|        +         |       OA       |
|        -         |       OB       |

**Amplificateur L9110** (voir [datasheet](https://www.elecrow.com/download/datasheet-l9110.pdf) )

|      L9110       |      Arduino   |  
| :--------------: | :------------: |
|        IA        |       D9       |
|        IB        |       D10      |
|        VCC       |       RAW_V    |
|        GND       |       GND      |


**Ecran Nokia 5110**

|    Ecran 5110    |     Arduino                              |  
| :--------------: | :--------------------------------------: |
|       SCK        |    SCK (D13)                             |
|     DIN/MOSI     |    MOSI (D11)                            |
|       DC         | Situé dans VarioSettings.h ( defaut D4 ) |
|       CS         | Situé dans VarioSettings.h ( defaut D3 ) |
|       RST        | Situé dans VarioSettings.h ( defaut D2 ) |
|       VCC        |     3.3v régulé                          |
|       GND        |      GND                                 |

**Lecteur carte SD**

|    SD card         |     Arduino                                  |  
| :----------------: | :------------------------------------------: |
|       CS           |  Situé dans VarioSettings.h ( defaut A0 )    |
|       MOSI         |      MOSI (D11)                              |
|       MISO         |      MISO (D12)                              |
|       SCLK         |      SCK (D13)                               |
|     5v or 3.3v     |    RAW_V ou 3.3v régulé                      |
|       GND          |         GND                                  |

**Carte GPS**

|    carte GPS     |     Arduino                          |  
| :--------------: | :----------------------------------: |
|       TX         |        RX                            |
|       RX         |    Tout (pas utilisé actuellement)   |
|       VCC        |       RAW_V                          |
|       GND        |       GND                            |

**Bluetooth module**

|    Bluetooth     |     Arduino                                     |  
| :--------------: | :---------------------------------------------: |
|       RX         |       TX                                        |
|       TX         |     Toute broche avec des interruptions         |
|                  |     (non utilisée actuellement)                 | 
|       VCC        |       RAW_V                                     |
|       GND        |       GND                                       |

**Diviseur de tension** (270k et 1M résistances en ligne)

|    diviseur de tension       |     Arduino                              |  
| :--------------------------: | :--------------------------------------: |
|       Côté 270k              |       Batterie + (RAW_V)                 |
|       Entre les résistances  | Situé dans VarioSettings.h ( defaut A2 ) |
|       Côté 1M                |       GND                                |
                      

















 

