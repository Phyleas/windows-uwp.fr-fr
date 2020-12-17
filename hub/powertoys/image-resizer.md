---
title: Utilitaire de redimensionnement d’image PowerToys pour Windows 10
description: Extension du shell Windows pour le redimensionnement d’image en bloc
ms.date: 12/02/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 486eb90b8515ec8422e8a475c9f03ced070dafa1
ms.sourcegitcommit: 46a7e9db64e17a645ee6e888f62a9b04632c56af
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/17/2020
ms.locfileid: "97618581"
---
# <a name="image-resizer-utility"></a>Utilitaire de redimensionnement d’image

Le redimensionnement d’image est une extension de shell Windows pour le redimensionnement d’image en bloc. Après l’installation des PowerToys, cliquez avec le bouton droit sur un ou plusieurs fichiers image sélectionnés dans l’Explorateur de fichiers, puis sélectionnez **redimensionner les images** dans le menu.

![Démonstration de redimensionnement d’image](../images/powertoys-resize-images.gif)

## <a name="drag-and-drop"></a>Glisser-déposer

Le redimensionnement d’image vous permet également de redimensionner des images en faisant glisser et en déplaçant les fichiers sélectionnés **avec le bouton droit de la souris**. Cela vous permet d’enregistrer rapidement les images redimensionnées dans un autre dossier.

![Démonstration de glissement-déplacement d’image](../images/powertoys-resize-drag-drop.gif)

## <a name="settings"></a>Paramètres

Dans l’onglet options de redimensionnement d’image PowerToys, vous pouvez configurer les paramètres suivants.

![Menu des paramètres de redimensionnement d’image PowerToys](../images/powertoys-imageresize-settings.png)

### <a name="sizes"></a>Tailles

Ajoutez de nouvelles tailles prédéfinies. Chaque taille peut être configurée en tant que remplissage, ajustement ou étirement. La dimension à utiliser pour le redimensionnement peut également être configurée en tant que centimètres, pouces, pourcentage et pixels.

#### <a name="fill-vs-fit-vs-stretch"></a>Remplir vs avec Stretch

- **Remplissage :** Remplit l’intégralité de la taille spécifiée avec l’image. Met à l’échelle l’image proportionnellement. Rogne l’image selon les besoins.
- **Ajuster :** Ajuste l’intégralité de l’image à la taille spécifiée. Met à l’échelle l’image proportionnellement. Ne rogne pas l’image.
- **Stretch :** Remplit l’intégralité de la taille spécifiée avec l’image. Étire l’image de façon disproportionnée en fonction des besoins. Ne rogne pas l’image

La largeur et la hauteur de la taille spécifiée peuvent être permutées pour correspondre à l’orientation (portrait/paysage) de l’image actuelle. Pour toujours utiliser la largeur et la hauteur spécifiées, désactivez : **ignorez l’orientation des images**.

![Paramètres de redimensionnement de l’image](../images/powertoys-resize-settings.gif)

### <a name="fallback-encoding"></a>Encodage de secours

L’encodeur de secours est utilisé lorsque le fichier ne peut pas être enregistré dans son format d’origine. Par exemple, le format d’image de métafichier Windows (. wmf) a un décodeur pour lire l’image, mais pas d’encodeur pour écrire une nouvelle image. Dans ce cas, l’image ne peut pas être enregistrée au format d’origine. Le redimensionnement d’image vous permet de spécifier le format utilisé par l’encodeur de secours : les paramètres PNG, JPEG, TIFF, BMP, GIF ou WMPhoto. *Il ne s’agit pas d’un outil de conversion de type de fichier, mais il fonctionne uniquement comme un secours pour les formats de fichiers non pris en charge.*

### <a name="file"></a>Fichier

Le nom de fichier de l’image redimensionnée peut être modifié avec les paramètres suivants :

- `%1`: Nom de fichier d’origine
- `%2`: Nom de taille (tel que configuré dans les paramètres de redimensionnement d’image PowerToys)
- `%3`: Largeur sélectionnée
- `%4`: Hauteur sélectionnée
- `%5`: Hauteur réelle
- `%6`: Largeur réelle

Par exemple, si vous définissez le format de nom de fichier sur : `%1 (%2)` sur le fichier `example.png` et en sélectionnant le `Small` paramètre de taille de fichier, le nom de fichier est obtenu `example (Small).png` .

En définissant le format `%1_%4` sur le fichier `example.jpg` et en sélectionnant le paramètre taille, vous `Medium 1366 x 768px` obtenez le nom de fichier : `example_768.jpg` .

Vous pouvez également choisir de conserver la date de *Dernière modification* d’origine sur l’image redimensionnée.

### <a name="auto-widthheight"></a>Largeur/hauteur automatiques

Vous pouvez conserver la hauteur ou la largeur vide. Cela permet d’honorer la dimension spécifiée et de « verrouiller » l’autre dimension à une valeur proportionnelle aux proportions d’origine de l’image.

### <a name="sub-directories"></a>Sous-répertoires

Vous pouvez spécifier un répertoire au format de nom de fichier pour regrouper les images redimensionnées dans des sous-répertoires. Par exemple, une valeur de `%2\%1` enregistre l’image redimensionnée sur `Small\Sample.jpg`
