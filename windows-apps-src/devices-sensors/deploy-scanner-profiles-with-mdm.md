---
title: Déployer des profils de scanneur de codes-barres avec MDM
description: Les profils de scanneur de codes-barres peuvent être déployés avec un serveur MDM.
ms.assetid: 99ED3BD8-022C-40C2-9C65-F599186548FE
ms.date: 09/26/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 1bac497ec52dd0897af8c6c606bcdc041007c579
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89173273"
---
# <a name="deploy-barcode-scanner-profiles-with-mdm"></a>Déployer des profils de scanneur de codes-barres avec MDM

**Remarque**    Cette fonctionnalité nécessite Windows 10 mobile ou une version ultérieure.

Les profils de scanneur de codes-barres peuvent être déployés avec un serveur MDM. Pour déployer les profils, utilisez *OemProfile* dans le [fournisseur de services de chiffrement EnterpriseExtFileSystem](/windows/client-management/mdm/enterpriseextfilessystem-csp) pour les placer dans le \\ dossier de \\ \\ profil public OEM Data SharedData \\ \\ . Ces profils de scanneur peuvent ensuite être utilisés par les fabricants de pilotes pour configurer des paramètres qui ne sont pas exposés via la surface de l’API.

Microsoft ne définit pas les spécificités d’un profil de scanneur ni comment les implémenter.

## <a name="related-topics"></a>Rubriques connexes
- [Fournisseur CSP EnterpriseExtFileSystem](/windows/client-management/mdm/enterpriseextfilessystem-csp)
- [Prise en charge des périphériques du scanneur de codes-barres](./pos-device-support.md#barcode-scanner)