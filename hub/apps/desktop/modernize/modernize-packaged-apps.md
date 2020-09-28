---
description: Découvrez comment ajouter des expériences modernes pour les utilisateurs de Windows 10 dans une application de bureau intégrée à un package d’application Windows.
title: Modernisation des applications de bureau packagées
ms.date: 04/22/2019
ms.topic: article
keywords: windows 10, uwp
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: adcf1e26ba5ebd2d4fb3b901e27e49da4b6d89dd
ms.sourcegitcommit: 5d7168ebc9f43aa13051446aff45a46600e6aafe
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/18/2020
ms.locfileid: "90783041"
---
# <a name="features-that-require-package-identity"></a>Fonctionnalités nécessitant l’identité du package

Si vous souhaitez mettre à jour votre application de bureau en intégrant des [expériences Windows 10 modernes](index.md), de nombreuses fonctionnalités sont disponibles uniquement dans les applications de bureau qui disposent de [l’identité du package](/uwp/schemas/appxpackage/uapmanifestschema/element-identity). Il existe plusieurs façons d’accorder l’identité du package à une application de bureau :

* Empaquetez-la dans un package [MSIX](/windows/msix/desktop/desktop-to-uwp-root). MSIX est un format de package d’application moderne qui permet de créer des packages universels pour toutes les applications Windows, WPF, Windows Forms et Win32. Il offre une expérience d’installation et de mise à jour fiable, un modèle de sécurité managé avec un système de capacité flexible, une prise en charge du Microsoft Store, une gestion d’entreprise et de nombreux modèles de distribution personnalisés. Pour plus d’informations, consultez [Empaqueter des applications de bureau](/windows/msix/desktop/desktop-to-uwp-root) dans la documentation MSIX.
* Si vous ne parvenez pas à adopter l’empaquetage MSIX pour le déploiement de votre application de bureau, à compter de Windows 10 version 2004, vous pouvez accorder une identité de package en créant un *package MSIX partiel* contenant uniquement un manifeste de package. Pour plus d’informations, consultez [Accorder une identité à des applications de bureau non empaquetées](grant-identity-to-nonpackaged-apps.md).

Si votre application de bureau dispose de l’identité du package, vous pouvez utiliser les fonctionnalités suivantes dans votre application.

## <a name="use-windows-runtime-apis-that-require-package-identity"></a>Utilisation des API Windows Runtime pour lesquelles l’identité du package est nécessaire

Les API Windows Runtime de [cette liste](desktop-to-uwp-supported-api.md#list-of-apis) imposent l’utilisation de l’identité du package dans une application de bureau.

## <a name="integrate-with-package-extensions"></a>Intégrer avec des extensions de package

Si votre application a besoin de s’intégrer au système (par exemple, pour établir des règles de pare-feu), décrivez ces éléments dans le manifeste du package de votre application et le système s’occupera du reste. Pour la plupart de ces tâches, vous n’avez pas à écrire de code. Avec un peu de XML dans le manifeste, vous pouvez faire des choses comme démarrer un processus quand l’utilisateur ouvre une session, intégrer votre application dans l’Explorateur de fichiers et ajouter à votre application la liste des cibles d’impression qui s’affichent dans d’autres applications.

Pour plus d’informations, consultez [Intégration d’une application de bureau à des extensions de package](desktop-to-uwp-extensions.md).

## <a name="get-activation-info-for-packaged-apps"></a>Obtenir des informations d’activation pour les applications empaquetées

À compter de la version 1809 de Windows 10, les applications de bureau empaquetées peuvent récupérer certains types d’informations d’activation lors du démarrage. Par exemple, vous pouvez obtenir des informations relatives à l’activation des applications concernant l’ouverture d’un fichier, un clic sur un toast interactif ou l’utilisation d’un protocole.

Pour plus d’informations, consultez [Obtenir des informations sur l’activation pour les applications empaquetées](get-activation-info-for-packaged-apps.md).

## <a name="extend-with-uwp-components"></a>Étendre à l’aide de composants UWP

Certaines expériences Windows 10 (par exemple, une page d’interface utilisateur tactile) doivent s’exécuter à l’intérieur d’un conteneur d’application moderne. En règle générale, vous devez d’abord déterminer si vous pouvez ajouter votre expérience en [améliorant](desktop-to-uwp-enhance.md) votre application de bureau avec des API Windows Runtime. Si vous devez utiliser un composant UWP pour réaliser l’expérience, vous pouvez ajouter un projet UWP à votre solution et utiliser des services d’application pour la communication entre votre application de bureau et les composants UWP.

Pour plus d’informations, consultez [Extension d’une application de bureau avec des composants UWP](desktop-to-uwp-extend.md).

## <a name="distribute"></a>Distribuer

Si vous intégrez votre application à un package MSIX, vous pouvez la distribuer en la publiant dans le Microsoft Store ou par chargement indépendant sur d’autres systèmes.

Consultez [Distribution d’une application de bureau packagée](desktop-to-uwp-distribute.md).