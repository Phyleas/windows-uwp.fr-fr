---
title: Données de Registre pour les contrôleurs de jeu
description: En savoir plus sur les données que vous pouvez ajouter au registre du PC pour permettre l’utilisation de votre contrôleur dans les jeux UWP.
ms.assetid: 2DD0B384-8776-4599-9E52-4FC0AA682735
ms.date: 04/08/2019
ms.topic: article
keywords: Windows 10, UWP, jeux, entrée, registre, personnalisé
ms.localizationpriority: medium
ms.openlocfilehash: ac2ca98a067fb88dfcdc86c4e4ee4047b82206bc
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89159273"
---
# <a name="registry-data-for-game-controllers"></a>Données de Registre pour les contrôleurs de jeu

> [!NOTE]
> Cette rubrique est destinée aux fabricants de contrôleurs de jeu compatibles Windows 10 et ne s’applique pas à la majorité des développeurs.

L' [espace de noms Windows. Gaming. Input](/uwp/api/windows.gaming.input) permet aux fournisseurs de matériel indépendants d’ajouter des données au registre du PC, en permettant à leurs appareils d’apparaître sous forme de [boîtiers](/uwp/api/windows.gaming.input.gamepad), [RacingWheels](/uwp/api/windows.gaming.input.racingwheel), [ArcadeSticks](/uwp/api/windows.gaming.input.arcadestick), [FlightSticks](/uwp/api/windows.gaming.input.flightstick)et [UINavigationControllers](/uwp/api/windows.gaming.input.uinavigationcontroller) , le cas échéant. Tous les IHV doivent ajouter ces données pour leurs contrôleurs compatibles. En procédant ainsi, tous les jeux UWP (et tous les jeux de bureau qui utilisent l’API WinRT) seront en mesure de prendre en charge votre contrôleur de jeu.

## <a name="mapping-scheme"></a>Schéma de mappage

Les mappages d’un appareil avec l’ID de fournisseur (VID) **vvvv**, l’ID de produit (PID) **pppp**, la page d’utilisation **uuuu**et l’ID d’utilisation **xxxx**sont lus à partir de cet emplacement dans le registre :

`HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\VVVVPPPPUUUUXXXX`

Le tableau ci-dessous décrit les valeurs attendues sous l’emplacement racine de l’appareil :

<table>
    <tr>
        <th>Nom</th>
        <th>Type</th>
        <th>Requis ?</th>
        <th>Info</th>
    </tr>
    <tr>
        <td>Désactivé</td>
        <td>DWORD</td>
        <td>Non</td>
        <td>
            <p>Indique que cet appareil particulier doit être désactivé.</p>
            <ul>
                <li><b>0</b>: l’appareil n’est pas désactivé.</li>
                <li><b>1</b>: l’appareil est désactivé.</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td>Description</td>
        <td>REG_SZ <td>Non</td>
        <td>Brève description de l’appareil.</td>
    </tr>
</table>

Votre programme d’installation de l’appareil doit ajouter ces données au registre (via le programme d’installation ou un [fichier INF](https://docs.microsoft.com/windows-hardware/drivers/install/inf-files)).

Les sous-clés sous l’emplacement racine de l’appareil sont détaillées dans les sections suivantes.

### <a name="gamepad"></a>Boîtier de commande

Le tableau ci-dessous répertorie les sous-clés obligatoires et facultatives sous la sous-clé du **boîtier** :

<table>
    <tr>
        <th>Sous-clé</th>
        <th>Requis ?</th>
        <th>Info</th>
    </tr>
    <tr>
        <td>Menu</td>
        <td>Oui</td>
        <td rowspan="18" style="vertical-align: middle;">Voir <a href="#button-mapping">mappage de bouton</a></td>
    </tr>
    <tr>
        <td>Affichage</td>
        <td>Oui</td>
    </tr>
    <tr>
        <td>Un</td>
        <td>Oui</td>
    </tr>
    <tr>
        <td>B</td>
        <td>Oui</td>
    </tr>
    <tr>
        <td>X</td>
        <td>Oui</td>
    </tr>
    <tr>
        <td>O</td>
        <td>Oui</td>
    </tr>
    <tr>
        <td>LeftShoulder</td>
        <td>Oui</td>
    </tr>
    <tr>
        <td>RightShoulder</td>
        <td>Oui</td>
    </tr>
    <tr>
        <td>LeftThumbstickButton</td>
        <td>Oui</td>
    </tr>
    <tr>
        <td>RightThumbstickButton</td>
        <td>Oui</td>
    </tr>
    <tr>
        <td>DPadUp</td>
        <td>Oui</td>
    </tr>
    <tr>
        <td>DPadDown</td>
        <td>Oui</td>
    </tr>
    <tr>
        <td>DPadLeft</td>
        <td>Oui</td>
    </tr>
    <tr>
        <td>DPadRight</td>
        <td>Oui</td>
    </tr>
    <tr>
        <td>Paddle1</td>
        <td>Non</td>
    </tr>
    <tr>
        <td>Paddle2</td>
        <td>Non</td>
    </tr>
    <tr>
        <td>Paddle3</td>
        <td>Non</td>
    </tr>
    <tr>
        <td>Paddle4</td>
        <td>Non</td>
    </tr>
    <tr>
        <td>LeftTrigger</td>
        <td>Oui</td>
        <td rowspan="6" style="vertical-align: middle;">Voir <a href="#axis-mapping">mappage d’axe</a></td>
    </tr>
    <tr>
        <td>RightTrigger</td>
        <td>Oui</td>
    </tr>
    <tr>
        <td>LeftThumbstickX</td>
        <td>Oui</td>
    </tr>
    <tr>
        <td>LeftThumbstickY</td>
        <td>Oui</td>
    </tr>
    <tr>
        <td>RightThumbstickX</td>
        <td>Oui</td>
    </tr>
    <tr>
        <td>RightThumbstickY</td>
        <td>Oui</td>
    </tr>
</table>

> [!NOTE]
> Si vous ajoutez votre contrôleur de jeu en tant que **boîtier**de commande pris en charge, nous vous recommandons vivement de l’ajouter également en tant que **UINavigationController**pris en charge.

### <a name="racingwheel"></a>RacingWheel

Le tableau ci-dessous répertorie les sous-clés obligatoires et facultatives sous la sous-clé **RacingWheel** :

<table>
    <tr>
        <th>Sous-clé</th>
        <th>Requis ?</th>
        <th>Info</th>
    </tr>
    <tr>
        <td>PreviousGear</td>
        <td>Oui</td>
        <td rowspan="30" style="vertical-align: middle;">Voir <a href="#button-mapping">mappage de bouton</a></td>
    </tr>
    <tr>
        <td>NextGear</td>
        <td>Oui</td>
    </tr>
    <tr>
        <td>DPadUp</td>
        <td>Non</td>
    </tr>
    <tr>
        <td>DPadDown</td>
        <td>Non</td>
    </tr>
    <tr>
        <td>DPadLeft</td>
        <td>Non</td>
    </tr>
    <tr>
        <td>DPadRight</td>
        <td>Non</td>
    </tr>
    <tr>
        <td>Button1</td>
        <td>Non</td>
    </tr>
    <tr>
        <td>Button2</td>
        <td>Non</td>
    </tr>
    <tr>
        <td>Button3</td>
        <td>Non</td>
    </tr>
    <tr>
        <td>Button4</td>
        <td>Non</td>
    </tr>
    <tr>
        <td>Button5</td>
        <td>Non</td>
    </tr>
    <tr>
        <td>Button6</td>
        <td>Non</td>
    </tr>
    <tr>
        <td>Button7</td>
        <td>Non</td>
    </tr>
    <tr>
        <td>Button8</td>
        <td>Non</td>
    </tr>
    <tr>
        <td>Button9</td>
        <td>Non</td>
    </tr>
    <tr>
        <td>Button10</td>
        <td>Non</td>
    </tr>
    <tr>
        <td>Button11</td>
        <td>Non</td>
    </tr>
    <tr>
        <td>Button12</td>
        <td>Non</td>
    </tr>
    <tr>
        <td>Button13</td>
        <td>Non</td>
    </tr>
    <tr>
        <td>Button14</td>
        <td>Non</td>
    </tr>
    <tr>
        <td>Button15</td>
        <td>Non</td>
    </tr>
    <tr>
        <td>Button16</td>
        <td>Non</td>
    </tr>
    <tr>
        <td>FirstGear</td>
        <td>Non</td>
    </tr>
    <tr>
        <td>SecondGear</td>
        <td>Non</td>
    </tr>
    <tr>
        <td>ThirdGear</td>
        <td>Non</td>
    </tr>
    <tr>
        <td>FourthGear</td>
        <td>Non</td>
    </tr>
    <tr>
        <td>FifthGear</td>
        <td>Non</td>
    </tr>
    <tr>
        <td>SixthGear</td>
        <td>Non</td>
    </tr>
    <tr>
        <td>SeventhGear</td>
        <td>Non</td>
    </tr>
    <tr>
        <td>ReverseGear</td>
        <td>Non</td>
    </tr>
    <tr>
        <td>Roulette</td>
        <td>Oui</td>
        <td rowspan="5" style="vertical-align: middle;">Voir <a href="#axis-mapping">mappage d’axe</a></td>
    </tr>
    <tr>
        <td>Limitation</td>
        <td>Oui</td>
    </tr>
    <tr>
        <td>Garniture</td>
        <td>Oui</td>
    </tr>
    <tr>
        <td>Unidirectionnel</td>
        <td>Non</td>
    </tr>
    <tr>
        <td>Handbrake</td>
        <td>Non</td>
    </tr>
    <tr>
        <td>MaxWheelAngle</td>
        <td>Oui</td>
        <td>Voir <a href="#properties-mapping">mappage des propriétés</a></td>
    </tr>
</table>

### <a name="arcadestick"></a>ArcadeStick

Le tableau ci-dessous répertorie les sous-clés obligatoires et facultatives sous la sous-clé **ArcadeStick** :

<table>
    <tr>
        <th>Sous-clé</th>
        <th>Requis ?</th>
        <th>Info</th>
    </tr>
    <tr>
        <td>Action 1</td>
        <td>Oui</td>
        <td rowspan="12" style="vertical-align: middle;">Voir <a href="#button-mapping">mappage de bouton</a></td>
    </tr>
    <tr>
        <td>Action2</td>
        <td>Oui</td>
    </tr>
    <tr>
        <td>Action3</td>
        <td>Oui</td>
    </tr>
    <tr>
        <td>Action4</td>
        <td>Oui</td>
    </tr>
    <tr>
        <td>Action5</td>
        <td>Oui</td>
    </tr>
    <tr>
        <td>Action6</td>
        <td>Oui</td>
    </tr>
    <tr>
        <td>Special1</td>
        <td>Oui</td>
    </tr>
    <tr>
        <td>Special2</td>
        <td>Oui</td>
    </tr>
    <tr>
        <td>StickUp</td>
        <td>Oui</td>
    </tr>
    <tr>
        <td>StickDown</td>
        <td>Oui</td>
    </tr>
    <tr>
        <td>StickLeft</td>
        <td>Oui</td>
    </tr>
    <tr>
        <td>StickRight</td>
        <td>Oui</td>
    </tr>
</table>

### <a name="flightstick"></a>FlightStick

Le tableau ci-dessous répertorie les sous-clés obligatoires et facultatives sous la sous-clé **Flightstick** :

<table>
    <tr>
        <th>Sous-clé</th>
        <th>Requis ?</th>
        <th>Info</th>
    </tr>
    <tr>
        <td>FirePrimary</td>
        <td>Oui</td>
        <td rowspan="2" style="vertical-align: middle;">Voir <a href="#button-mapping">mappage de bouton</a></td>
    </tr>
    <tr>
        <td>FireSecondary</td>
        <td>Oui</td>
    </tr>
    <tr>
        <td>Group</td>
        <td>Oui</td>
        <td rowspan="4" style="vertical-align: middle;">Voir <a href="#axis-mapping">mappage d’axe</a></td>
    </tr>
    <tr>
        <td>Inclinaison</td>
        <td>Oui</td>
    </tr>
    <tr>
        <td>Lacet</td>
        <td>Oui</td>
    </tr>
    <tr>
        <td>Limitation</td>
        <td>Oui</td>
    </tr>
    <tr>
        <td>HatSwitch</td>
        <td>Oui</td>
        <td>Voir <a href="#switch-mapping">mappage de commutateur</a></td>
    </tr>
</table>

### <a name="uinavigation"></a>UINavigation

Le tableau ci-dessous répertorie les sous-clés obligatoires et facultatives sous la sous-clé **UINavigation** :

<table>
    <tr>
        <th>Sous-clé</th>
        <th>Requis ?</th>
        <th>Info</th>
    </tr>
    <tr>
        <td>Menu</td>
        <td>Oui</td>
        <td rowspan="24" style="vertical-align: middle;">Voir <a href="#button-mapping">mappage de bouton</a></td>
    </tr>
    <tr>
        <td>Affichage</td>
        <td>Oui</td>
    </tr>
    <tr>
        <td>Acceptation</td>
        <td>Oui</td>
    </tr>
    <tr>
        <td>Annuler</td>
        <td>Oui</td>
    </tr>
    <tr>
        <td>PrimaryUp</td>
        <td>Oui</td>
    </tr>
    <tr>
        <td>PrimaryDown</td>
        <td>Oui</td>
    </tr>
    <tr>
        <td>PrimaryLeft</td>
        <td>Oui</td>
    </tr>
    <tr>
        <td>PrimaryRight</td>
        <td>Oui</td>
    </tr>
    <tr>
        <td>Context1 (Contexte 1)</td>
        <td>Non</td>
    </tr>
    <tr>
        <td>Context2 (Contexte 2)</td>
        <td>Non</td>
    </tr>
    <tr>
        <td>Context3 (Contexte 3)</td>
        <td>Non</td>
    </tr>
    <tr>
        <td>Context4 (Contexte 4)</td>
        <td>Non</td>
    </tr>
    <tr>
        <td>Pg préc</td>
        <td>Non</td>
    </tr>
    <tr>
        <td>Pg suiv</td>
        <td>Non</td>
    </tr>
    <tr>
        <td>PageLeft</td>
        <td>Non</td>
    </tr>
    <tr>
        <td>PageRight</td>
        <td>Non</td>
    </tr>
    <tr>
        <td>ScrollUp (Faire défiler vers le haut)</td>
        <td>Non</td>
    </tr>
    <tr>
        <td>ScrollDown (Faire défiler vers le bas)</td>
        <td>Non</td>
    </tr>
    <tr>
        <td>ScrollLeft (Faire défiler vers la gauche)</td>
        <td>Non</td>
    </tr>
    <tr>
        <td>ScrollRight (Faire défiler vers la droite)</td>
        <td>Non</td>
    </tr>
    <tr>
        <td>SecondaryUp</td>
        <td>Non</td>
    </tr>
    <tr>
        <td>SecondaryDown</td>
        <td>Non</td>
    </tr>
    <tr>
        <td>SecondaryLeft</td>
        <td>Non</td>
    </tr>
    <tr>
        <td>SecondaryRight</td>
        <td>Non</td>
    </tr>
</table>

Pour plus d’informations sur les contrôleurs de navigation de l’interface utilisateur et les commandes ci-dessus, consultez [contrôleur de navigation d’interface utilisateur](https://docs.microsoft.com/windows/uwp/gaming/ui-navigation-controller).

## <a name="keys"></a>Keys

Les sections suivantes expliquent le contenu de chacune des sous-clés sous les clés de **boîtier**de commande, **RacingWheel**, **ArcadeStick**, **Flightstick**et **UINavigation** .

### <a name="button-mapping"></a>Mappage de bouton

Le tableau ci-dessous répertorie les valeurs nécessaires pour mapper un bouton. Par exemple, si vous appuyez sur **DPadUp** sur le contrôleur de jeu, le mappage de **DPadUp** doit contenir la valeur **buttonIndex** (le **bouton****source** est). Si **DPadUp** doit être mappé à partir d’une position de commutateur, le mappage **DPadUp** doit contenir les valeurs **SwitchIndex** et **SwitchPosition** (la**source** est **switch**).

<table>
    <tr>
        <th>Source</th>
        <th>Nom de la valeur</th>
        <th>Type de valeur</th>
        <th>Requis ?</th>
        <th>Informations sur la valeur</th>
    </tr>
    <tr>
        <td>Bouton</td>
        <td>ButtonIndex</td>
        <td>DWORD</td>
        <td>Oui</td>
        <td>Index dans le tableau de boutons <b>RawGameController</b> .</td>
    </tr>
    <tr>
        <td rowspan="4" style="vertical-align: middle;">Axe</td>
        <td>AxisIndex</td>
        <td>DWORD</td>
        <td>Oui</td>
        <td>Index dans le tableau de l’axe <b>RawGameController</b> .</td>
    </tr>
    <tr>
        <td>Inverser :</td>
        <td>DWORD</td>
        <td>Non</td>
        <td>Indique que la valeur de l’axe doit être inversée avant que les facteurs de <b>pourcentage de seuil</b> et de <b>DebouncePercent</b> soient appliqués.</td>
    </tr>
    <tr>
        <td>ThresholdPercent</td>
        <td>DWORD</td>
        <td>Oui</td>
        <td>Indique la position de l’axe à laquelle la valeur du bouton mappé passe de l’état enfoncé à l’État relâché. La plage de valeurs valide est comprise entre 0 et 100. Le bouton est considéré comme appuyé si la valeur de l’axe est supérieure ou égale à cette valeur.</td>
    </tr>
    <tr>
        <td>DebouncePercent</td>
        <td>DWORD</td>
        <td>Oui</td>
        <td>
            <p>Définit la taille d’une fenêtre autour de la valeur <b>ThresholdPercent</b> , qui est utilisée pour détourner l’état du bouton signalé. La plage de valeurs valide est comprise entre 0 et 100. Les transitions d’État du bouton ne peuvent se produire que lorsque la valeur de l’axe dépasse les limites supérieure ou inférieure de la fenêtre de débond. Par exemple, un <b>ThresholdPercent</b> de 50 et de <b>DebouncePercent</b> de 10 donne des limites de débond à 45% et 55% des valeurs de l’axe des étendues entières. Le bouton ne peut pas passer à l’état enfoncé tant que la valeur de l’axe n’atteint pas 55% ou plus, et il ne peut pas passer à l’État libéré tant que la valeur de l’axe n’atteint pas 45% ou en dessous.</p>
            <p>Les limites de fenêtre de débond calculées sont ancrées entre 0% et 100%. Par exemple, un seuil de 5% et une fenêtre de débond de 20% entraînent la chute des limites de la fenêtre de débond à 0% et 15%. L’état du bouton pour les valeurs d’axe 0% et 100% est toujours signalé comme relâché et appuyé, respectivement, quels que soient le seuil et les valeurs de débond.</p>
        </td>
    </tr>
    <tr>
        <td rowspan="3" style="vertical-align: middle;">Basculer</td>
        <td>SwitchIndex</td>
        <td>DWORD</td>
        <td>Oui</td>
        <td>Index dans le tableau du commutateur <b>RawGameController</b> .</td>
    </tr>
    <tr>
        <td>SwitchPosition</td>
        <td>REG_SZ</td>
        <td>Oui</td>
        <td>
            <p>Indique la position du commutateur qui fera en sorte que le bouton mappé signale qu’il est enfoncé. Les valeurs de position peuvent être l’une des chaînes suivantes :</p>
            <ul>
                <li>Haut</li>
                <li>UpRight</li>
                <li>Right</li>
                <li>DownRight</li>
                <li>Descendre</li>
                <li>DownLeft</li>
                <li>Gauche</li>
                <li>UpLeft</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td>IncludeAdjacent</td>
        <td>DWORD</td>
        <td>Non</td>
        <td>Indique que les positions de commutateur adjacentes entraîneront également le bouton mappé pour signaler qu’il est enfoncé.</td>
    </tr>
</table>

### <a name="axis-mapping"></a>Mappage d’axe

Le tableau ci-dessous répertorie les valeurs nécessaires pour mapper un axe :

<table>
    <tr>
        <th>Source</th>
        <th>Nom de la valeur</th>
        <th>Type de valeur</th>
        <th>Requis ?</th>
        <th>Informations sur la valeur</th>
    </tr>
    <tr>
        <td rowspan="2" style="vertical-align: middle;">Bouton</td>
        <td>MaxValueButtonIndex</td>
        <td>DWORD</td>
        <td>Oui</td>
        <td>
            <p>Index dans le tableau de boutons <b>RawGameController</b> qui est traduit en valeur d’axe unidirectionnelle mappée.</p>
            <table>
                <tr>
                    <th>MaxButton</th>
                    <th>AxisValue</th>
                </tr>
                <tr>
                    <td>FALSE</td>
                    <td>0,0</td>
                </tr>
                <tr>
                    <td>TRUE</td>
                    <td>1.0</td>
                </tr>
            </table>
        </td>
    </tr>
    <tr>
        <td>MinValueButtonIndex</td>
        <td>DWORD</td>
        <td>Non</td>
        <td>
            <p>Indique que l’axe mappé est bidirectionnel. Les valeurs de <b>MaxButton</b> et <b>MinButton</b> sont combinées en un seul axe bidirectionnel, comme indiqué ci-dessous.</p>
            <table>
                <tr>
                    <th>MinButton</th>
                    <th>MaxButton</th>
                    <th>AxisValue</th>
                </tr>
                <tr>
                    <td>FALSE</td>
                    <td>FALSE</td>
                    <td>0.5</td>
                </tr>
                <tr>
                    <td>false</td>
                    <td>true</td>
                    <td>1.0</td>
                </tr>
                <tr>
                    <td>TRUE</td>
                    <td>FALSE</td>
                    <td>0,0</td>
                </tr>
                <tr>
                    <td>TRUE</td>
                    <td>TRUE</td>
                    <td>0.5</td>
                </tr>
            </table>
        </td>
    </tr>
    <tr>
        <td rowspan="2" style="vertical-align: middle;">Axe</td>
        <td>AxisIndex</td>
        <td>DWORD</td>
        <td>Oui</td>
        <td>Index dans le tableau de l’axe <b>RawGameController</b> .</td>
    </tr>
    <tr>
        <td>Inverser :</td>
        <td>DWORD</td>
        <td>Non</td>
        <td>Indique que la valeur de l’axe mappé doit être inversée avant d’être retournée.</td>
    </tr>
    <tr>
        <td rowspan="3" style="vertical-align: middle;">Basculer</td>
        <td>SwitchIndex</td>
        <td>DWORD</td>
        <td>Oui</td>
        <td>Index dans le tableau du commutateur <b>RawGameController</b> .
    </tr>
    <tr>
        <td>MaxValueSwitchPosition</td>
        <td>REG_SZ</td>
        <td>Oui</td>
        <td>
            <p>Une des chaînes suivantes :</p>
            <ul>
                <li>Haut</li>
                <li>UpRight</li>
                <li>Right</li>
                <li>DownRight</li>
                <li>Descendre</li>
                <li>DownLeft</li>
                <li>Gauche</li>
                <li>UpLeft</li>
            </ul>
            <p>Elle indique la position du commutateur qui provoque le signalement de la valeur de l’axe mappé comme 1,0. La direction opposée de <b>MaxValueSwitchPosition</b> est traitée comme 0,0. Par exemple, si <b>MaxValueSwitchPosition</b> est <b>haut</b>, la traduction de la valeur de l’axe est indiquée ci-dessous :</p>
            <table>
                <tr>
                    <th>Position du commutateur</th>
                    <th>AxisValue</th>
                </tr>
                <tr>
                    <td>Haut</td>
                    <td>1.0</td>
                </tr>
                <tr>
                    <td>Center</td>
                    <td>0.5</td>
                </tr>
                <tr>
                    <td>Descendre</td>
                    <td>0,0</td>
                </tr>
            </table>
        </td>
    </tr>
    <tr>
        <td>IncludeAdjacent</td>
        <td>DWORD</td>
        <td>Non</td>
        <td>
            <p>Indique que les positions de commutateur adjacentes entraînent également l’état de l’axe mappé sur 1,0. Dans l’exemple ci-dessus, si <b>IncludeAdjacent</b> est défini, la traduction de l’axe est effectuée comme suit :</p>
            <table>
                <tr>
                    <th>Position du commutateur</th>
                    <th>AxisValue</th>
                </tr>
                <tr>
                    <td>Haut</td>
                    <td>1.0</td>
                </tr>
                <tr>
                    <td>UpRight</td>
                    <td>1.0</td>
                </tr>
                <tr>
                    <td>UpLeft</td>
                    <td>1.0</td>
                </tr>
                <tr>
                    <td>Center</td>
                    <td>0.5</td>
                </tr>
                <tr>
                    <td>Descendre</td>
                    <td>0,0</td>
                </tr>
                <tr>
                    <td>DownRight</td>
                    <td>0,0</td>
                </tr>
                <tr>
                    <td>DownLeft</td>
                    <td>0,0</td>
                </tr>
            </table>
        </td>
    </tr>
</table>

### <a name="switch-mapping"></a>Mappage de commutateur

Les positions de commutateur peuvent être mappées à partir d’un ensemble de boutons dans le tableau de boutons du **RawGameController** ou à partir d’un index dans le tableau de commutateurs. Les positions de commutateur ne peuvent pas être mappées à partir des axes.

<table>
    <tr>
        <th>Source</th>
        <th>Nom de la valeur</th>
        <th>Type de valeur</th>
        <th>Informations sur la valeur</th>
    </tr>
    <tr>
        <td rowspan="10" style="vertical-align: middle;">Bouton</td>
        <td>ButtonCount</td>
        <td>DWORD</td>
        <td>2, 4 ou 8</td>
    </tr>
    <tr>
        <td>SwitchKind</td>
        <td>REG_SZ</td>
        <td><b>TwoWay</b>, <b>FourWay</b>ou <b>EightWay</b>
    </tr>
    <tr>
        <td>UpButtonIndex</td>
        <td>DWORD</td>
        <td rowspan="8" style="vertical-align: middle;">Voir <a href="#buttonindex-values">* valeurs buttonIndex</a></td>
    </tr>
    <tr>
        <td>DownButtonIndex</td>
        <td>DWORD</td>
    </tr>
    <tr>
        <td>LeftButtonIndex</td>
        <td>DWORD</td>
    </tr>
    <tr>
        <td>RightButtonIndex</td>
        <td>DWORD</td>
    </tr>
    <tr>
        <td>UpRightButtonIndex</td>
        <td>DWORD</td>
    </tr>
    <tr>
        <td>DownRightButtonIndex</td>
        <td>DWORD</td>
    </tr>
    <tr>
        <td>DownLeftButtonIndex</td>
        <td>DWORD</td>
    </tr>
    <tr>
        <td>UpLeftButtonIndex</td>
        <td>DWORD</td>
    </tr>
    <tr>
        <td rowspan="9" style="vertical-align: middle;">Axe</td>
        <td>SwitchKind</td>
        <td>REG_SZ</td>
        <td><b>TwoWay</b>, <b>FourWay</b>ou <b>EightWay</b></td>
    </tr>
    <tr>
        <td>XAxisIndex</td>
        <td>DWORD</td>
        <td rowspan="2" style="vertical-align: middle;"><b>YAxisIndex</b> est toujours présent. <b>XAxisIndex</b> est présent uniquement lorsque <b>SwitchKind</b> est <b>FourWay</b> ou <b>EightWay</b>.</td>
    </tr>
    <tr>
        <td>YAxisIndex</td>
        <td>DWORD</td>
    </tr>
    <tr>
        <td>XDeadZonePercent</td>
        <td>DWORD</td>
        <td rowspan="2" style="vertical-align: middle;">Indiquez la taille de la zone morte autour de la position centrale des axes.</td>
    </tr>
    <tr>
        <td>YDeadZonePercent</td>
        <td>DWORD</td>
    </tr>
    <tr>
        <td>XDebouncePercent</td>
        <td>DWORD</td>
        <td rowspan="2" style="vertical-align: middle;">Définissez la taille des fenêtres autour des limites de zone morte supérieure et inférieure, qui sont utilisées pour défaire passer l’état de commutateur signalé.</td>
    </tr>
    <tr>
        <td>YDebouncePercent</td>
        <td>DWORD</td>
    </tr>
    <tr>
        <td>XInvert</td>
        <td>DWORD</td>
        <td rowspan="2" style="vertical-align: middle;">Indique que les valeurs de l’axe correspondant doivent être inversées avant que la zone morte et les calculs de la fenêtre de débond soient appliqués.</td>
    </tr>
    <tr>
        <td>YInvert</td>
        <td>DWORD</td>
    </tr>
    <tr>
        <td rowspan="3" style="vertical-align: middle;">Basculer</td>
        <td>SwitchIndex</td>
        <td>DWORD</td>
        <td>Index dans le tableau du commutateur <b>RawGameController</b> .
    </tr>
    <tr>
        <td>Inverser :</td>
        <td>DWORD</td>
        <td>Indique que le commutateur signale ses positions dans un sens inverse des aiguilles d’une montre, plutôt que dans le sens des aiguilles d’une montre.</td>
    </tr>
    <tr>
        <td>PositionBias</td>
        <td>DWORD</td>
        <td>
            <p>Décale le point de départ de la façon dont les positions sont signalées par la quantité spécifiée. <b>PositionBias</b> est toujours compté dans le sens des aiguilles d’une montre à partir du point de départ d’origine et est appliqué avant l’inversion de l’ordre des valeurs.</p>
            <p>Par exemple, un commutateur qui signale des positions commençant par <b>carré</b> dans le sens inverse des aiguilles d’une montre peut être normalisé en définissant l’indicateur d' <b>inversion</b> et en spécifiant un <b>PositionBias</b> de 5 :</p>
            <table>
                <tr>
                    <th>Position</th>
                    <th>Valeur signalée</th>
                    <th>Après PositionBias et inverser les indicateurs</th>
                </tr>
                <tr>
                    <td>DownRight</td>
                    <td>0</td>
                    <td>3</td>
                </tr>
                <tr>
                    <td>Right</td>
                    <td>1</td>
                    <td>2</td>
                </tr>
                <tr>
                    <td>UpRight</td>
                    <td>2</td>
                    <td>1</td>
                </tr>
                <tr>
                    <td>Haut</td>
                    <td>3</td>
                    <td>0</td>
                </tr>
                <tr>
                    <td>UpLeft</td>
                    <td>4</td>
                    <td>7</td>
                </tr>
                <tr>
                    <td>Gauche</td>
                    <td>5</td>
                    <td>6</td>
                </tr>
                <tr>
                    <td>DownLeft</td>
                    <td>6</td>
                    <td>5</td>
                </tr>
                <tr>
                    <td>Descendre</td>
                    <td>7</td>
                    <td>4</td>
                </tr>
            </table>
    </tr>
</table>

#### <a name="buttonindex-values"></a>* Valeurs ButtonIndex

\*Index des valeurs ButtonIndex dans le tableau de boutons du **RawGameController**:

<table>
    <tr>
        <th>ButtonCount</th>
        <th>SwitchKind</th>
        <th>RequiredMappings</th>
    </tr>
    <tr>
        <td>2</td>
        <td>Bidirectionnel</td>
        <td>
            <ul>
                <li>UpButtonIndex</li>
                <li>DownButtonIndex</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td>4</td>
        <td>FourWay</td>
        <td>
            <ul>
                <li>UpButtonIndex</li>
                <li>DownButtonIndex</li>
                <li>LeftButtonIndex</li>
                <li>RightButtonIndex</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td>4</td>
        <td>EightWay</td>
        <td>
            <ul>
                <li>UpButtonIndex</li>
                <li>DownButtonIndex</li>
                <li>LeftButtonIndex</li>
                <li>RightButtonIndex</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td>8</td>
        <td>EightWay</td>
        <td>
            <ul>
                <li>UpButtonIndex</li>
                <li>DownButtonIndex</li>
                <li>LeftButtonIndex</li>
                <li>RightButtonIndex</li>
                <li>UpRightButtonIndex</li>
                <li>DownRightButtonIndex</li>
                <li>DownLeftButtonIndex</li>
                <li>UpLeftButtonIndex</li>
            </ul>
        </td>
    </tr>
</table>

### <a name="properties-mapping"></a>Mappage des propriétés

Il s’agit de valeurs de mappage statiques pour différents types de mappage.

<table>
    <tr>
        <th>Mappage</th>
        <th>Nom de la valeur</th>
        <th>Type de valeur</th>
        <th>Informations sur la valeur</th>
    </tr>
    <tr>
        <td>RacingWheel</td>
        <td>MaxWheelAngle</td>
        <td>DWORD</td>
        <td>Indique l’angle de roue physique maximal pris en charge par la roue en une seule direction. Par exemple, une roue avec une rotation possible de-90 degrés à 90 degrés spécifie 90.</td>
    </tr>
</table>

## <a name="labels"></a>Étiquettes

Les étiquettes doivent être présentes sous la clé des **étiquettes** sous la racine de l’appareil. Les **étiquettes** peuvent avoir 3 sous-clés : **boutons**, **axes**et **commutateurs**.

### <a name="button-labels"></a>Libellé des boutons

La touche **boutons** mappe chaque position de bouton du tableau de boutons de **RawGameController**sur une chaîne. Chaque chaîne est mappée en interne à la valeur d’énumération [GameControllerButtonLabel](https://docs.microsoft.com/uwp/api/windows.gaming.input.gamecontrollerbuttonlabel) correspondante. Par exemple, si un boîtier de commande comporte dix boutons et que l’ordre dans lequel le **RawGameController** analyse les boutons et les affiche dans le rapport boutons, cela se présente comme suit :

```cpp
Menu,               // Index 0
View,               // Index 1
LeftStickButton,    // Index 2
RightStickButton,   // Index 3
LetterA,            // Index 4
LetterB,            // Index 5
LetterX,            // Index 6
LetterY,            // Index 7
LeftBumper,         // Index 8
RightBumper         // Index 9
```

Les étiquettes doivent apparaître dans cet ordre sous la clé **Buttons** :

<table>
    <tr>
        <th>Nom</th>
        <th>Valeur (type : REG_SZ)</th>
    </tr>
    <tr>
        <td>Button0</td>
        <td>Menu</td>
    </tr>
    <tr>
        <td>Button1</td>
        <td>Affichage</td>
    </tr>
    <tr>
        <td>Button2</td>
        <td>LeftStickButton</td>
    </tr>
    <tr>
        <td>Button3</td>
        <td>RightStickButton</td>
    </tr>
    <tr>
        <td>Button4</td>
        <td>Lettre</td>
    </tr>
    <tr>
        <td>Button5</td>
        <td>LetterB</td>
    </tr>
    <tr>
        <td>Button6</td>
        <td>LetterX</td>
    </tr>
    <tr>
        <td>Button7</td>
        <td>Lettre</td>
    </tr>
    <tr>
        <td>Button8</td>
        <td>LeftBumper</td>
    </tr>
    <tr>
        <td>Button9</td>
        <td>RightBumper</td>
    </tr>
</table>

### <a name="axis-labels"></a>Étiquettes de l’axe

La touche **axes** mappe chaque position d’axe dans le tableau d’axes de **RawGameController**à l’une des étiquettes listées dans l' [énumération GameControllerButtonLabel](https://docs.microsoft.com/uwp/api/windows.gaming.input.gamecontrollerbuttonlabel) , comme les étiquettes de bouton. Consultez l’exemple dans [étiquettes de bouton](#button-labels).

### <a name="switch-labels"></a>Changer les étiquettes

La touche **commutateurs** mappe les positions aux étiquettes. Les valeurs respectent la Convention d’affectation de noms suivante : pour étiqueter la position d’un commutateur, dont l’index est *x* dans le tableau de commutateurs de **RawGameController**, ajoutez ces valeurs sous la sous-clé **switches** :

* SwitchxUp
* SwitchxUpRight
* SwitchxRight
* SwitchxDownRight
* SwitchxDown
* SwitchxDownLeft
* SwitchxUpLeft
* SwitchxLeft

Le tableau suivant présente un exemple de jeu d’étiquettes pour les positions de commutateur d’un commutateur à 4 voies qui apparaît à l’index 0 dans le **RawGameController**:

<table>
    <tr>
        <th>Nom</th>
        <th>Valeur (type : REG_SZ)</th>
    </tr>
    <tr>
        <td>Switch0Up</td>
        <td>XboxUp</td>
    </tr>
    <tr>
        <td>Switch0Right</td>
        <td>XboxRight</td>
    </tr>
    <tr>
        <td>Switch0Down</td>
        <td>XboxDown</td>
    </tr>
    <tr>
        <td>Switch0Left</td>
        <td>XboxLeft</td>
    </tr>
</table>

<!--### Label strings

* XboxBack
* XboxStart
* XboxMenu
* XboxView
* XboxUp
* XboxDown
* XboxLeft
* XboxRight
* XboxA
* XboxB
* XboxX
* XboxY
* XboxLeftBumper
* XboxLeftTrigger
* XboxLeftStickButton
* XboxRightBumper
* XboxRightTrigger
* XboxRightStickButton
* XboxPaddle1
* XboxPaddle2
* XboxPaddle3
* XboxPaddle4
* Mode
* Select
* Menu
* View
* Back
* Start
* Options
* Share
* Up
* Down
* Left
* Right
* LetterA
* LetterB
* LetterC
* LetterL
* LetterR
* LetterX
* LetterY
* LetterZ
* Cross
* Circle
* Square
* Triangle
* LeftBumper
* LeftTrigger
* LeftStickButton
* Left1
* Left2
* Left3
* RightBumper
* RightTrigger
* RightStickButton
* Right1
* Right2
* Right3
* Paddle1
* Paddle2
* Paddle3
* Paddle4
* Plus
* Minus
* DownLeftArrow
* DialLeft
* DialRight
* Suspension-->

## <a name="example-registry-file"></a>Exemple de fichier de Registre

Pour illustrer la façon dont tous ces mappages et valeurs sont réunis, voici un exemple de fichier de Registre pour un **RacingWheel**générique :

```text
Windows Registry Editor Version 5.00

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004]
"Description" = "Example Wheel Device"

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\Labels\Buttons]
"Button0" = "LetterA"
"Button1" = "LetterB"
"Button2" = "LetterX"
"Button3" = "LetterY"
"Button6" = "Menu"
"Button7" = "View"
"Button8" = "RightStickButton"
"Button9" = "LeftStickButton"

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\Labels\Switches]
"Switch0Down" = "Down"
"Switch0Left" = "Left"
"Switch0Right" = "Right"
"Switch0Up" = "Up"

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel]
"MaxWheelAngle" = dword:000001c2

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\Brake]
"AxisIndex" = dword:00000002
"Invert" = dword:00000001

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\Button1]
"ButtonIndex" = dword:00000000

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\Button2]
"ButtonIndex" = dword:00000001

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\Button3]
"ButtonIndex" = dword:00000002

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\Button4]
"ButtonIndex" = dword:00000003

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\Button5]
"ButtonIndex" = dword:00000009

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\Button6]
"ButtonIndex" = dword:00000008

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\Button7]
"ButtonIndex" = dword:00000007

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\Button8]
"ButtonIndex" = dword:00000006

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\Clutch]
"AxisIndex" = dword:00000003
"Invert" = dword:00000001

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\DPadDown]
"IncludeAdjacent" = dword:00000001
"SwitchIndex" = dword:00000000
"SwitchPosition" = "Down"

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\DPadLeft]
"IncludeAdjacent" = dword:00000001
"SwitchIndex" = dword:00000000
"SwitchPosition" = "Left"

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\DPadRight]
"IncludeAdjacent" = dword:00000001
"SwitchIndex" = dword:00000000
"SwitchPosition" = "Right"

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\DPadUp]
"IncludeAdjacent" = dword:00000001
"SwitchIndex" = dword:00000000
"SwitchPosition" = "Up"

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\FifthGear]
"ButtonIndex" = dword:00000010

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\FirstGear]
"ButtonIndex" = dword:0000000c

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\FourthGear]
"ButtonIndex" = dword:0000000f

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\NextGear]
"ButtonIndex" = dword:00000004

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\PreviousGear]
"ButtonIndex" = dword:00000005

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\ReverseGear]
"ButtonIndex" = dword:0000000b

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\SecondGear]
"ButtonIndex" = dword:0000000d

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\SixthGear]
"ButtonIndex" = dword:00000011

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\ThirdGear]
"ButtonIndex" = dword:0000000e

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\Throttle]
"AxisIndex" = dword:00000001
"Invert" = dword:00000001

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\Wheel]
"AxisIndex" = dword:00000000
"Invert" = dword:00000000
```

## <a name="see-also"></a>Voir aussi

* [Espace de noms Windows. Gaming. Input](/uwp/api/windows.gaming.input)
* [Espace de noms Windows. Gaming. Input. Custom](/uwp/api/windows.gaming.input.custom)
* [Fichiers INF](/windows-hardware/drivers/install/inf-files)