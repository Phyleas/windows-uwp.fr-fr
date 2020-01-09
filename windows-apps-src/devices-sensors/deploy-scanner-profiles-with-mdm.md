---
title: Déployer des profils de scanneurs de codes-barres avec GPM
description: Les profils de scanneurs de codes-barres peuvent être déployés avec un serveur GPM.
ms.assetid: 99ED3BD8-022C-40C2-9C65-F599186548FE
ms.date: 09/26/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: f537833385582678b215804cac9a16002618c7e4
ms.sourcegitcommit: 26bb75084b9d2d2b4a76d4aa131066e8da716679
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/06/2020
ms.locfileid: "75684827"
---
# <a name="deploy-barcode-scanner-profiles-with-mdm"></a>Déployer des profils de scanneurs de codes-barres avec GPM

**Notez**  cette fonctionnalité requiert Windows 10 mobile ou version ultérieure.

Les profils de scanneurs de codes-barres peuvent être déployés avec un serveur GPM. Pour déployer les profils, utilisez *OemProfile* dans le [fournisseur de services de chiffrement EnterpriseExtFileSystem](https://docs.microsoft.com/windows/client-management/mdm/enterpriseextfilessystem-csp) pour les placer dans le dossier \\Data\\SharedData\\OEM\\public\\Profile. Ces profils de scanneurs peuvent ensuite être utilisés par les fabricants de pilotes pour configurer les paramètres qui ne sont pas exposés par le biais de la surface d’API.

Microsoft ne définit pas les spécificités d’un profil de scanneur et n’indique pas comment les implémenter.

## <a name="related-topics"></a>Rubriques connexes
- [CSP EnterpriseExtFileSystem](https://docs.microsoft.com/windows/client-management/mdm/enterpriseextfilessystem-csp)
- [Prise en charge des périphériques du scanneur de codes-barres](https://docs.microsoft.com/windows/uwp/devices-sensors/pos-device-support#barcode-scanner)