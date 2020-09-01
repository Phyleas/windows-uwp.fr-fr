---
title: Prise en charge du matériel de point de service
description: Cet article contient des informations sur la prise en charge matérielle de chacune des classes d’appareils de point de service
ms.date: 06/13/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: bf5d6a2413ba6aeb2e3fd86122e865e34b8729fa
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89172203"
---
# <a name="supported-point-of-service-peripherals"></a>Périphériques de point de service pris en charge

## <a name="barcode-scanner"></a>Scanneur de codes-barres
| Connectivité | Support |
| -------------|-------------|
| USB          | <p>Windows contient un pilote de classe intégré pour les scanneurs de codes-barres connectés USB qui est basé sur la spécification de la table d’utilisation du scanneur HID POS (8c) définie par [USB.org](https://www.usb.org/hid). Pour obtenir la liste des périphériques compatibles connus, consultez le tableau ci-dessous.  Consultez le manuel de votre scanneur de codes-barres ou contactez le fabricant pour déterminer comment configurer votre scanneur en **USB. HID. Mode du scanneur du PDV** . </p><p>Windows prend également en charge l’implémentation de pilotes spécifiques au fournisseur pour prendre en charge des scanneurs de codes-barres supplémentaires qui ne prennent pas en charge le port USB. HID. Standard du scanneur du PDV. Vérifiez auprès du fabricant de votre scanneur de codes-barres la disponibilité du pilote spécifique au fournisseur.</p><p>Fabricants de scanneurs de codes-barres consultez le [Guide de conception du pilote](/windows-hardware/drivers/ddi/_pos/index) de code-barres pour plus d’informations sur la création d’un pilote de scanneur de codes-barres personnalisé</p> |
| Bluetooth    | <p>Windows prend en charge les scanneurs de codes-barres Bluetooth basés sur le protocole de port série (SPP-SSI). Pour obtenir la liste des périphériques compatibles connus, consultez le tableau ci-dessous. Consultez le manuel de votre scanneur de codes-barres ou contactez le fabricant pour déterminer comment configurer votre scanneur en mode **spp-SSI** .</p> |
| Webcam       | <p>À compter de Windows 10, version 1803, vous pouvez lire les codes-barres à l’aide d’une lentille de caméra standard à partir d’une application Windows universelle. Nous vous recommandons d’utiliser une caméra qui prend en charge la focalisation automatique et une résolution minimale de 1920 x 1440.  Certains appareils photo de résolution inférieure peuvent lire des codes-barres standard si le code-barres est suffisamment grand.  Les codes-barres avec des éléments plus fins peuvent nécessiter des caméras plus haute résolution.</p>| 
|


| Fabricant  | Modèle                          | Fonctionnalité | Connexion    | Type         | Mode                      |
|---------------|--------------------------------|------------|--------------|--------------|---------------------------|
| Code          | Lecteur™ 950                    | 2D         | USB          | Poche     | Scanneur de PDV HID           |
| Code          | Lecteur™ 1021                   | 2D         | USB          | Poche     | Scanneur de PDV HID           |
| Code          | Lecteur™ 1421                   | 2D         | USB          | Poche     | Scanneur de PDV HID           |
| Code          | Lecteur™ 5000                   | 2D         | USB          | Présentation | Scanneur de PDV HID           |
| Honeywell     | Genesis 7580g                  | 2D         | USB          | Présentation | Scanneur de PDV HID           |
| Honeywell     | Granit 198Xi                   | 2D         | USB          | Poche     | Scanneur de PDV HID           |
| Honeywell     | Granit 191Xi                   | 2D         | USB          | Poche     | Scanneur de PDV HID           |
| Honeywell     | N5680                          | 2D         | Interne     | Composant    | Scanneur de PDV HID           |
| Honeywell     | N3680                          | 2D         | Interne     | Composant    | Scanneur de PDV HID           |
| Honeywell     | Orbite 7190g                    | 2D         | USB          | Présentation | Scanneur de PDV HID           |
| Honeywell     | Stratos 2700                   | 2D         | USB          | Dans le compteur   | Scanneur de PDV HID           |
| Honeywell     | Voyager 1200g                  | 1D         | USB          | Poche     | Scanneur de PDV HID           |
| Honeywell     | Voyager 1202g                  | 1D         | USB          | Poche     | Scanneur de PDV HID           |
| Honeywell     | Voyager 1202-BF                | 1D         | USB          | Poche     | Scanneur de PDV HID           |
| Honeywell     | Voyager 145Xg                  | 1D/2D<sup>1</sup>   | USB          | Poche     | Scanneur de PDV HID           |
| Honeywell     | Voyager 1602g                  | 2D         | USB          | Poche     | Scanneur de PDV HID           |
| Honeywell     | 1900g du xénon                    | 2D         | USB          | Poche     | Scanneur de PDV HID           |
| Honeywell     | 1902g du xénon                    | 2D         | USB          | Poche     | Scanneur de PDV HID           |
| Honeywell     | 1902g du xénon-BF                 | 2D         | USB          | Poche     | Scanneur de PDV HID           |
| Honeywell     | 1900h du xénon                    | 2D         | USB          | Poche     | Scanneur de PDV HID           |
| Honeywell     | 1902h du xénon                    | 2D         | USB          | Poche     | Scanneur de PDV HID           |
| HP            | Scanner de codes-barres de valeur (HR2150) | 2D         | USB          | Poche     | Scanneur de PDV HID           |
| Intermec      | SG20                           | 2D         | USB          | Poche     | Scanneur de PDV HID           |
| Mobile Socket | CHS 7Ci                        | 1D         | Bluetooth    | Poche     | Profil de port série (SPP) |
| Mobile Socket | CHS 7Di                        | 1D         | Bluetooth    | Poche     | Profil de port série (SPP) |
| Mobile Socket | CHS 7Mi                        | 1D         | Bluetooth    | Poche     | Profil de port série (SPP) |
| Mobile Socket | CHS 7Pi                        | 1D         | Bluetooth    | Poche     | Profil de port série (SPP) |
| Mobile Socket | CHS 8Ci                        | 1D         | Bluetooth    | Poche     | Profil de port série (SPP) |
| Mobile Socket | DuraScan D700                  | 1D         | Bluetooth    | Poche     | Profil de port série (SPP) |
| Mobile Socket | DuraScan D730                  | 1D         | Bluetooth    | Poche     | Profil de port série (SPP) |
| Mobile Socket | DuraScan D740                  | 2D         | Bluetooth    | Poche     | Profil de port série (SPP) |
| Mobile Socket | SocketScan S700                | 1D         | Bluetooth    | Poche     | Profil de port série (SPP) |
| Mobile Socket | SocketScan S730                | 1D         | Bluetooth    | Poche     | Profil de port série (SPP) |
| Mobile Socket | SocketScan S740                | 2D         | Bluetooth    | Poche     | Profil de port série (SPP) |
| Mobile Socket | SocketScan S800                | 1D         | Bluetooth    | Poche     | Profil de port série (SPP) |
| Mobile Socket | SocketScan S850                | 2D         | Bluetooth    | Poche     | Profil de port série (SPP) |
| Rayures         | DS2208<sup>2</sup>                        | 2D         | USB          | Poche     | Scanneur de PDV HID           |
| Rayures         | DS2278                         | 2D         | USB          | Poche     | Scanneur de PDV HID           |
| Rayures         | DS8108<sup>3</sup>                        | 2D         | USB          | Poche     | Scanneur de PDV HID           |
| Rayures         | DS8178<sup>4</sup>                         | 2D         | USB          | Poche     | Scanneur de PDV HID           | 


<sup>1</sup> mise à niveau pour prendre en charge les codes-barres 2D via Honeywell <br/>
<sup>2</sup> minimum firmware 009 (2018.07.09) requis. Mise à niveau à l’aide de rayures [123Scan](http://www.zebra.com/123scan).<br/>
<sup>3</sup> microprogramme (2018.01.18) minimum requis. Mise à niveau à l’aide de rayures [123Scan](http://www.zebra.com/123scan).<br/> 
<sup>4</sup> microprogramme minimum (2019.03.11) minimum requis. Mise à niveau à l’aide de rayures [123Scan](http://www.zebra.com/123scan).<br/>

<hr>

### <a name="windows-devices-with-built-in-barcode-scanner"></a>Appareils Windows avec un scanneur de codes-barres intégré
| Fabricant   | Modèle | Système d'exploitation |
|----------------|-------|------------------|
| Innowi         | ChecOut-M | Windows 10   |

### <a name="windows-mobile-devices-with-built-in-barcode-scanner"></a>Appareils Windows Mobile avec un scanneur de codes-barres intégré
| Fabricant   | Modèle | Système d'exploitation |
|----------------|-------|------------------|
| Bluebird       | EF400 | Windows Mobile   |
| Bluebird       | EF500 | Windows Mobile   |
| Bluebird       | EF500R | Windows Mobile   |
| Honeywell      | CT50   | Windows Mobile   |
| Honeywell      | D75e | Windows Mobile   |
| Janam          | XT2      | Windows Mobile   |
| Panasonic      | FZ-E1 | Windows Mobile   |
| Panasonic      | FZ-F1 |Windows Mobile   |
| PointMobile    | PM80 | Windows Mobile   |
| Rayures          | TC700j | Windows Mobile   |
| HP             | Enveloppe Elite x3 | Windows Mobile   |




## <a name="cash-drawer"></a>Tiroir-caisse
| Connectivité | Support |
| -------------|-------------|
| Réseau/Bluetooth | <p> La connexion directe au tiroir-caisse peut être établie sur le réseau ou via Bluetooth, en fonction des capacités de l’unité de tiroir-caisse. </p><p>APG Caisse : NetPRO, BluePRO</p> |
| Port DK | <p> Les tiroirs de trésorerie qui ne disposent pas de fonctionnalités réseau ou Bluetooth peuvent être connectés par le biais du port DK sur une imprimante d’accusés de réception pris en charge ou à l’aide des microcartes réseau en étoile de la DK-caisse. </p>
| OPOS    | <p> Prend en charge tous les tiroirs de trésorerie compatibles OPOS via les objets de service OPOS fournis par le fabricant. Installez les pilotes OPOS conformément aux instructions d’installation du fabricant de l’appareil. </p> |


## <a name="customer-display-linedisplay"></a>Affichage client (LineDisplay)
Prend en charge les affichages de lignes compatibles OPOS via les objets de service OPOS fournis par le fabricant. Installez les pilotes OPOS conformément aux instructions d’installation du fabricant de l’appareil.

## <a name="magnetic-stripe-reader"></a>Lecteur de bande magnétique
Windows assure la prise en charge des lecteurs de bandes magnétiques suivants à partir de MagTek et IDTech en fonction de leur ID de fournisseur et de l’ID de produit (VID/PID).

| Fabricant |    Modèle (s) |  Numéro de référence |
|--------------|-----------|--------------|
| IDTech | SecureMag (VID : 0ACD PID : 2010) | IDRE-3x5xxxx |
| | MiniMag (VID : 0ACD PID : 0500) |   IDMB-3x5xxxx |
| Magtek | MagneSafe (VID : ID de l’PID : 0011) |  210730xx |
| | Dynamag (VID : ID de l’PID : 0002) |   210401xx |

 Windows prend en charge l’implémentation de pilotes supplémentaires spécifiques au fournisseur pour prendre en charge des lecteurs de bandes magnétiques supplémentaires. Vérifiez la disponibilité du fabricant de votre lecteur de bande magnétique. Fabricants de lecteurs de bandes magnétiques consultez le [Guide de conception du pilote de bande](/windows-hardware/drivers/ddi/_pos/index) magnétique pour plus d’informations sur la création d’un pilote de lecteur de bande magnétique personnalisé.

## <a name="receipt-printer-posprinter"></a>Imprimante d’accusés de réception (POSPrinter)
| Connectivité | Support |
| -------------|-------------|
| Réseau et Bluetooth | <p>Windows prend en charge les imprimantes de réception connectées au réseau et à Bluetooth à l’aide du langage de contrôle d’imprimante EPSON ESC/POS.  Les imprimantes répertoriées ci-dessous sont découvertes automatiquement à l’aide des API POSPrinter. Des imprimantes de réception supplémentaires qui fournissent une émulation ESC/POS peuvent également fonctionner, mais doivent être associées à l’aide d’un processus de [couplage hors bande](./point-of-service.md#out-of-band-pairing) .</p><p>Remarque : les stations de la station Slip et du journal ne sont pas prises en charge par cette méthode.</p> |
| OPOS    | <p> Prend en charge toutes les imprimantes de réception compatibles OPOS via les objets de service OPOS. Installez les pilotes OPOS conformément aux instructions d’installation du fabricant de l’appareil. </p> |

### <a name="stationary-receipt-printers-networkbluetooth"></a>Imprimantes à réception stationnaire (réseau/Bluetooth)
| Fabricant |    Modèle (s) |
|--------------|-----------|
| Imprimantes |   TM-T88V, TM-T70, TM-T20, TM-U220 |

### <a name="mobile-receipt-printers-bluetooth"></a>Imprimantes à réception mobile (Bluetooth)
| Fabricant |    Modèle (s) |
|--------------|-----------|
| Imprimantes |   MobiLink P20 (TM-P20), MobiLink P60 (TM-P60), MobiLink P80 (TM-P80) |