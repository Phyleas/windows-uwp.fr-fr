---
title: Déployer des profils de scanneur de codes-barres avec MDM
description: Découvrez comment déployer des profils de scanneur de codes-barres avec un serveur de gestion des appareils mobiles (MDM) à l’aide du fournisseur de services de configuration (CSP) EnterpriseExtFileSystem.
ms.assetid: 99ED3BD8-022C-40C2-9C65-F599186548FE
ms.date: 09/26/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: de3a43386e37c9bb997340c35c8f16977871916d
ms.sourcegitcommit: 5481bb34def681bc60fbfa42d9779053febec468
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89304721"
---
# <a name="deploy-barcode-scanner-profiles-with-mdm"></a>Déployer des profils de scanneur de codes-barres avec MDM

**Remarque**    Cette fonctionnalité nécessite Windows 10 mobile ou une version ultérieure.

Les profils de scanneur de codes-barres peuvent être déployés avec un serveur MDM. Pour déployer les profils, utilisez *OemProfile* dans le [fournisseur de services de chiffrement EnterpriseExtFileSystem](/windows/client-management/mdm/enterpriseextfilessystem-csp) pour les placer dans le \\ dossier de \\ \\ profil public OEM Data SharedData \\ \\ . Ces profils de scanneur peuvent ensuite être utilisés par les fabricants de pilotes pour configurer des paramètres qui ne sont pas exposés via la surface de l’API.

Microsoft ne définit pas les spécificités d’un profil de scanneur ni comment les implémenter.

## <a name="related-topics"></a>Rubriques connexes
- [Fournisseur CSP EnterpriseExtFileSystem](/windows/client-management/mdm/enterpriseextfilessystem-csp)
- [Prise en charge des périphériques du scanneur de codes-barres](./pos-device-support.md#barcode-scanner)