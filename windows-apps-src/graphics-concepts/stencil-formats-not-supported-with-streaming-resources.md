---
title: Formats de stencil non pris en charge avec des ressources de streaming
description: Les formats contenant un gabarit ne sont pas pris en charge par les ressources de diffusion en continu.
ms.assetid: 90A572A4-3C76-4795-BAE9-FCC72B5F07AD
keywords:
- Formats de gabarit non pris en charge par les ressources de diffusion en continu
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: db6476afe265ea4a2556a5f787a14daa2e212e61
ms.sourcegitcommit: ae9c1646398bb5a4a888437628eca09ae06e6076
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/03/2019
ms.locfileid: "74735134"
---
# <a name="stencil-formats-not-supported-with-streaming-resources"></a>Formats de gabarit non pris en charge par les ressources de diffusion en continu


Les formats contenant un gabarit ne sont pas pris en charge par les ressources de diffusion en continu.

Les formats qui contiennent stencil incluent DXGI\_FORMAT\_D24\_UNORM\_S8\_UINT (et les formats associés de la famille R24G8) et DXGI\_FORMAT\_d32\_FLOAT\_S8X24\_UINT (et les formats associés de la famille R32G8X24).

Certaines implémentations stockent les informations de profondeur et de gabarit dans des emplacements distincts tandis que d'autres les stockent ensemble. Pour chacun des deux cas, les vignettes devront être gérées différemment et aucune API unique ne pourra extraire ou rationaliser les différences. Pour les prochains matériels, nous vous conseillons de privilégier une prise en charge de surfaces de gabarit et de profondeur indépendantes, celles-ci étant restituées différemment sous forme de vignettes.

Pour une profondeur de 32 bits, vous disposeriez de vignettes 128 x 128 , tandis qu'avec un gabarit de 8 bits, elles seraient de 256 x 256. Ainsi, en raison de l'hétérogénéité des caractéristiques de profondeur et de gabarit, vos applications présenteraient des formes de vignette incohérentes. Ce problème existe déjà avec des formats différents de surfaces de cible de rendu.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Partage de ressources en continu et partage d’appareils](streaming-resource-cross-process-and-device-sharing.md)

 

 




