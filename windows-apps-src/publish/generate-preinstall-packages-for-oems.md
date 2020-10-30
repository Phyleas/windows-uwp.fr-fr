---
description: Si les autorisations appropriées sont accordées à votre compte de développeur, vous pouvez générer et télécharger des packages préinstallés afin qu’un OEM puisse inclure votre application dans son image de système d’exploitation.
title: Génération des packages de préinstallation pour les fabricants OEM
ms.assetid: AC3A45E8-7BBD-44E9-B2D3-B74B7C9B2BC9
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: e6b2aa9688b141318a9e96a26e5d0dc643306459
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93034782"
---
# <a name="generate-preinstall-packages-for-oems"></a>Génération des packages de préinstallation pour les fabricants OEM

Si les autorisations appropriées sont accordées à votre compte de développeur, vous pouvez générer et télécharger des packages préinstallés afin qu’un OEM puisse inclure votre application dans son image de système d’exploitation. Les autorisations de préinstallation sont uniquement activées sur les comptes de développeur qui sont sponsorisés par des OEM.


## <a name="important-preinstall-policy--limitations"></a>Politique et limitations importantes en matière de préinstallation

Pour pouvoir se connecter au Store et recevoir des mises à jour d’application, les applications préinstallées doivent être certifiées par le biais de l' [espace partenaires](https://partner.microsoft.com/dashboard) .

Toute application préinstallée doit être et rester libre sur tous les marchés.


## <a name="generating-preinstall-packages"></a>Génération de packages de préinstallation

Une fois qu'un compte a été activé avec des autorisations de préinstallation, suivez la procédure ci-après :

1.  Dans l’espace partenaires, accédez à l’application qui doit être préinstallée.
2.  Dans le menu de navigation de gauche, développez **gestion d’applications** , puis sélectionnez **packages actuels** .
3.  Dans la section **demander des packages pour la préinstallation du système d’exploitation** , sélectionnez **activer les packages téléchargeables** .
4.  Dans la boîte de dialogue de confirmation, sélectionnez **activer** .
5.  Recherchez le package que vous souhaitez télécharger, puis sélectionnez le lien **générer le package** approprié.

    > [!NOTE]
    > La durée de génération des packages préinstallés varie en fonction de la taille du package que vous avez sélectionné. Vous pouvez conserver cette page et revenir ultérieurement, ou vous pouvez conserver la page ouverte pendant la génération de votre package.

6.  Une fois le package généré, un lien vers le **package de téléchargement** s’affiche. Sélectionnez ce lien pour télécharger le fichier. zip.

Vous pouvez ensuite fournir le fichier. zip au fabricant OEM pour l’inclure dans son image du système d’exploitation.


## <a name="support"></a>Support

Si vous avez d’autres questions sur la génération de packages de préinstallation, envoyez un e-mail à <partnerops@microsoft.com> .

 

 




