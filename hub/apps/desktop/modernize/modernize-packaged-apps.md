---
Description: Découvrez comment ajouter des expériences modernes pour les utilisateurs de Windows 10 dans une application de bureau que vous avez empaquetée dans un package d’application Windows.
title: Moderniser les applications de bureau packagées
ms.date: 04/22/2019
ms.topic: article
keywords: windows 10, uwp
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: ec1c56f205b270262f618ffb46db16b38276975d
ms.sourcegitcommit: d7eccdb27c22bccac65bd014e62b6572a6b44602
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/30/2019
ms.locfileid: "73142504"
---
# <a name="features-that-require-package-identity"></a>Fonctionnalités nécessitant l’identité du package

Si vous souhaitez mettre à jour votre application de bureau avec des [expériences Windows 10 modernes](index.md), de nombreuses fonctionnalités sont disponibles uniquement dans les applications de bureau qui ont une [identité de package](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-identity). Il existe plusieurs façons d’accorder l’identité d’un package à une application de bureau :

* Empaquetez-le dans un [package MSIX](/windows/msix/desktop/desktop-to-uwp-root). MSIX est un format de package d’application moderne qui fournit une expérience d’empaquetage universel pour toutes les applications Windows, WPF, Windows Forms et Win32. Il offre une expérience d’installation et de mise à jour robustes, un modèle de sécurité managé avec un système de capacité flexible, la prise en charge de la Microsoft Store, la gestion d’entreprise et de nombreux modèles de distribution personnalisés. Pour plus d’informations, consultez [Empaqueter des applications de bureau](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-root) dans la documentation MSIX.
* Si vous ne pouvez pas adopter le Packaging MSIX pour le déploiement de votre application de bureau, à partir de Windows 10 Insider Preview Build 10.0.19000.0, vous pouvez accorder l’identité de package en créant un *package MSIX épars* qui contient uniquement un manifeste de package. Pour plus d’informations, consultez [accorder une identité à des applications de bureau non packagées](grant-identity-to-nonpackaged-apps.md).

Si votre application de bureau a une identité de package, vous pouvez utiliser les fonctionnalités suivantes dans votre application.

## <a name="use-uwp-apis-that-require-package-identity"></a>Utiliser les API UWP qui requièrent l’identité du package

La liste suivante d’API UWP requiert l’utilisation d’une identité de package dans une application de bureau : [liste des API](desktop-to-uwp-supported-api.md#list-of-apis).

## <a name="integrate-with-package-extensions"></a>Intégrer avec des extensions de package

Si votre application doit s’intégrer au système (par exemple : établir des règles de pare-feu), décrivez les éléments du manifeste de package de votre application et le système fera le reste. Pour la plupart de ces tâches, vous n’avez pas à écrire du code. Avec un peu de XML dans le manifeste, vous pouvez effectuer des opérations telles que démarrer un processus lorsque l’utilisateur ouvre une session, intégrer votre application dans l’Explorateur de fichiers et ajouter votre application à la liste des cibles d’impression qui s’affichent dans d’autres applications.

Pour plus d’informations, consultez [intégrer votre application de bureau avec les extensions de package](desktop-to-uwp-extensions.md).

## <a name="extend-with-uwp-components"></a>Étendre à l’aide de composants UWP

Certaines expériences Windows 10 (par exemple: une page d'interface utilisateur tactile) doivent s'exécuter à l'intérieur d'un conteneur d'application moderne. En général, vous devez d’abord déterminer si vous pouvez ajouter votre expérience en [améliorant](desktop-to-uwp-enhance.md) votre application de bureau existante avec les API UWP. Si vous devez utiliser un composant UWP, vous pouvez ajouter un projet UWP à votre solution et utiliser app services pour communiquer entre votre application de bureau et le composant UWP.

Pour plus d’informations, consultez [étendre votre application de bureau avec les composants UWP](desktop-to-uwp-extend.md).

## <a name="distribute"></a>Distribuer

Si vous empaquetez votre application dans un package MSIX, vous pouvez la distribuer en la publiant Microsoft Store ou en la chargement sur d’autres systèmes.

Consultez [distribuer votre application de bureau empaquetée](desktop-to-uwp-distribute.md).
