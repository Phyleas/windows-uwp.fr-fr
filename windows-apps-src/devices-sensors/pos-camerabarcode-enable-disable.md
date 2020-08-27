---
title: Configuration du scanneur de codes-barres de l’appareil
description: Découvrez comment définir une clé de Registre système dans Windows 10 pour activer ou désactiver le décodeur logiciel pour le scanneur de codes-barres de l’appareil photo.
ms.date: 04/08/2019
ms.topic: article
keywords: Windows 10, UWP, point de service, pos
ms.localizationpriority: medium
ms.openlocfilehash: fefe15dd36cbc08fcae3b5bc0199eafad400cf1f
ms.sourcegitcommit: 8e0e4cac79554e86dc7f035c4b32cb1f229142b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/26/2020
ms.locfileid: "88943059"
---
# <a name="enable-or-disable-the-software-decoder-that-ships-with-windows"></a>Activer ou désactiver le décodeur logiciel fourni avec Windows

Dans Windows 10, version 1803, le décodeur logiciel est installé et activé par défaut.  Vous pouvez désactiver le décodeur logiciel fourni avec Windows si vous ne souhaitez pas utiliser le scanneur de codes-barres de l’appareil photo ou si vous avez acquis un décodeur tiers compatible avec les API Windows. Devices. PointOfService. BarcodeScanner et que vous ne souhaitez pas utiliser les deux.

## <a name="enable-or-disable-using-the-system-registry"></a>Activer ou désactiver l’utilisation du Registre système

Le décodeur logiciel fourni avec Windows peut être activé ou désactivé via le registre système en ajoutant la clé de Registre *InboxDecoder* sous *HKLM\Software\Microsoft\PointOfService\BarcodeScanner* et en définissant la valeur *activer* comme décrit ci-dessous.

| Nom de la valeur  | Type valeur | Valeur | État |
| ----------- | --------- | -------|--------|
| Activer      | DWORD     | 1 (par défaut)<br/>0 |  Active le décodeur logiciel fourni avec Windows <br/> Désactive le décodeur logiciel fourni avec Windows |

Voici un exemple de fichier de registre que vous pouvez utiliser pour **Désactiver** le décodeur logiciel fourni avec Windows :

```text
Windows Registry Editor Version 5.00

[HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\PointOfService\BarcodeScanner\InboxDecoder]
"Enable"=dword:0000000
```  

Utilisez cet exemple de fichier de Registre pour **activer** le décodeur logiciel fourni avec Windows :

```text
Windows Registry Editor Version 5.00

[HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\PointOfService\BarcodeScanner\InboxDecoder]
"Enable"=dword:0000001
```  

> [!Warning]
> En effet, toute erreur de modification peut être à l’origine de problèmes graves.  Pour plus de protection, sauvegardez le registre avant de le modifier.  Vous pourrez ainsi restaurer le Registre en cas de problème.  Pour plus d’informations sur la sauvegarde et la restauration du Registre, cliquez sur le numéro d’article suivant pour afficher l’article de la base de connaissances Microsoft : <br/><br/> [322756](https://support.microsoft.com/help/322756/how-to-back-up-and-restore-the-registry-in-windows) comment sauvegarder et restaurer le registre dans Windows.

> [!NOTE]
> Le décodeur logiciel intégré à Windows 10 est fourni à l’aimable de la  [**société Digimarc**](https://www.digimarc.com/).

## <a name="see-also"></a>Voir aussi

### <a name="samples"></a>exemples

- [Exemple de scanneur de codes-barres](https://github.com/microsoft/Windows-universal-samples/tree/master/Samples/BarcodeScanner)
