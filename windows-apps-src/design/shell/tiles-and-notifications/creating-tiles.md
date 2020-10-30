---
description: Une vignette est la représentation d’une application dans le menu Démarrer. Chaque application dispose d’une vignette. Lorsque vous créez un projet d’application Windows dans Microsoft Visual Studio, il comprend une vignette par défaut qui affiche le nom et le logo de votre application.
title: Vignettes pour applications Windows
ms.assetid: 09C7E1B1-F78D-4659-8086-2E428E797653
label: Tiles
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 79e1ac5188b5c777ba1e99380f0e63beb6e01d37
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93032072"
---
# <a name="tiles-for-windows-apps"></a>Vignettes pour applications Windows

 

Une *vignette* est une représentation d’une application dans le menu Démarrer. Chaque application dispose d’une vignette. Lorsque vous créez un projet d’application Windows dans Microsoft Visual Studio, il comprend une vignette par défaut qui affiche le nom et le logo de votre application. Windows affiche cette vignette lors de la première installation de l’application. Une fois votre application installée, vous pouvez modifier le contenu de votre vignette via des notifications. Par exemple, vous pouvez modifier la vignette pour communiquer de nouvelles informations à l’utilisateur, telles que des titres d’actualités, ou l’objet du dernier message non lu.

## <a name="configure-the-default-tile"></a>Configurer la vignette par défaut


Lorsque vous créez un projet dans Visual Studio, cela a pour effet de créer une vignette par défaut simple qui affiche le nom et le logo de votre application.

Pour modifier votre vignette, double-cliquez sur le fichier **Package. appxmanifest** dans votre projet UWP principal pour ouvrir le concepteur (ou cliquez avec le bouton droit sur le fichier et sélectionnez Afficher le code).

```XML
  <Applications>
    <Application Id="App"
      Executable="$targetnametoken$.exe"
      EntryPoint="ExampleApp.App">
      <uap:VisualElements
        DisplayName="ExampleApp"
        Square150x150Logo="Assets\Square150x150Logo.png"
        Square44x44Logo="Assets\Square44x44Logo.png"
        Description="ExampleApp"
        BackgroundColor="#464646">
        <uap:SplashScreen Image="Assets\SplashScreen.png" />
      </uap:VisualElements>
    </Application>
  </Applications>
```

Vous devez mettre à jour quelques éléments :

-   DisplayName : remplacez cette valeur par le nom à afficher sur votre vignette.
-   ShortName : l’espace disponible pour le nom d’affichage sur les vignettes étant limité, nous vous recommandons de spécifier ce nom court pour éviter que le nom de votre application soit tronqué.
-   Images de logo :

    Vous devez remplacer ces images par vos propres images. Vous avez la possibilité de fournir des images pour différentes échelles visuelles, mais vous n’êtes pas obligé de le faire pour toutes. Pour vérifier que votre application s’affiche correctement sur divers appareils, nous vous recommandons de fournir des versions de chaque image aux échelles 100 %, 200 % et 400 %. Pour en savoir plus sur la génération de ces ressources [, consultez icônes et logos d’application](../../style/app-icons-and-logos.md) .

    Les images mises à l’échelle respectent la Convention d’affectation de noms suivante :
    
    *&lt; nom &gt;* de l’image. *&lt; facteur &gt; d’échelle* . *&lt; &gt;extension de fichier image* 

    Par exemple : SplashScreen.scale-100.png

    Lorsque vous faites référence à l’image, vous y faites référence en tant que *&lt; nom &gt; d’image* . *&lt; &gt;extension de fichier image* (« SplashScreen.png » dans cet exemple). Le système sélectionne automatiquement l’image à l’échelle appropriée pour l’appareil parmi les images que vous avez fournies.

-   Nous vous conseillons vivement de fournir des logos pour les vignettes de grande taille afin que l’utilisateur puisse redimensionner la vignette de votre application si nécessaire. Pour fournir ces images supplémentaires, vous créez un élément **DefaultTile** et utilisez les attributs **Wide310x150Logo** et **Square310x310Logo** pour spécifier les images supplémentaires :
```    XML
  <Applications>
        <Application Id="App"
          Executable="$targetnametoken$.exe"
          EntryPoint="ExampleApp.App">
          <uap:VisualElements
            DisplayName="ExampleApp"
            Square150x150Logo="Assets\Square150x150Logo.png"
            Square44x44Logo="Assets\Square44x44Logo.png"
            Description="ExampleApp"
            BackgroundColor="#464646">
            <uap:DefaultTile
              Wide310x150Logo="Assets\Wide310x150Logo.png"
              Square310x310Logo="Assets\Square310x310Logo.png">
            </uap:DefaultTile>
            <uap:SplashScreen Image="Assets\SplashScreen.png" />
          </uap:VisualElements>
        </Application>
      </Applications>
```

## <a name="use-notifications-to-customize-your-tile"></a>Utiliser les notifications pour personnaliser votre vignette


Une fois votre application installée, vous pouvez utiliser les notifications pour personnaliser votre vignette. Vous pouvez effectuer cette opération la première fois que votre application est lancée ou en réponse à un événement, tel qu’une notification push.

Pour savoir comment envoyer des notifications par vignettes, consultez [Envoyer une notification de vignette locale](sending-a-local-tile-notification.md).
