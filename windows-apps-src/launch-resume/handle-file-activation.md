---
title: Gérer l’activation des fichiers
description: Une application peut s’inscrire afin de devenir le gestionnaire par défaut pour un certain type de fichier.
ms.assetid: A0F914C5-62BC-4FF7-9236-E34C5277C363
ms.date: 07/05/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
dev_langs:
- csharp
- vb
- cppwinrt
- cpp
ms.openlocfilehash: 079746d3c1619fe940ba243410f0247b7b850ed9
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74259456"
---
# <a name="handle-file-activation"></a>Gérer l’activation des fichiers

**API importantes**

-   [**Windows. ApplicationModel. activation. FileActivatedEventArgs**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Activation.FileActivatedEventArgs)
-   [**Windows. UI. Xaml. application. OnFileActivated**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.onfileactivated)

Votre application peut s’inscrire pour devenir le gestionnaire par défaut d’un certain type de fichier. Tant les applications de plateforme Windows classique (CWP) que les applications de plateforme Windows universelle (UWP) peuvent s’inscrire pour devenir gestionnaire de fichiers par défaut. Si l’utilisateur choisit votre application en tant que gestionnaire par défaut pour un certain type de fichier, celle-ci sera activée lors du lancement de ce type de fichier.

Nous vous recommandons de vous inscrire uniquement pour un type de fichier si vous pensez gérer tous les lancements de fichiers pour ce type. Si votre application nécessite uniquement d’utiliser ce type de fichier en interne, vous ne devez pas vous inscrire pour devenir le gestionnaire par défaut. Si vous choisissez de vous inscrire pour un type de fichier, vous devez fournir à l’utilisateur final la fonctionnalité attendue lorsque votre application est activée pour ce type de fichier. Par exemple, une visionneuse d’images doit s’inscrire pour afficher un fichier .jpg. Pour plus d’informations sur les associations de fichiers, voir [Recommandations en matière de types de fichier et d’URI](https://docs.microsoft.com/windows/uwp/files/index).

Ces étapes montrent comment s’inscrire pour un type de fichier personnalisé, alsdk, et comment activer votre application quand l’utilisateur lance un fichier alsdk.

> **Notez**  dans les applications UWP, certains URI et extensions de fichier sont réservés à une utilisation par les applications intégrées et le système d’exploitation. Toute tentative d’inscription de votre application avec une extension de fichier ou un URI réservés sera ignorée. Pour plus d’informations, voir [Noms de schéma d’URI et de fichier réservé](reserved-uri-scheme-names.md).

## <a name="step-1-specify-the-extension-point-in-the-package-manifest"></a>Étape 1 : spécifier le point d’extension dans le manifeste du package

L’application reçoit des événements d’activation uniquement pour les extensions de fichiers répertoriées dans le manifeste du package. Procédez comme suit pour indiquer que votre application gère les fichiers portant l’extension `.alsdk`.

1.  Dans l’**Explorateur de solutions**, double-cliquez sur package.appxmanifest pour ouvrir le concepteur de manifeste. Sélectionnez l’onglet **Déclarations**. Dans la liste déroulante **Déclarations disponibles**, sélectionnez **Associations de types de fichier**, puis cliquez sur **Ajouter**. Pour plus d’informations sur les identificateurs utilisés par les associations de fichiers, voir [Identificateurs programmatiques](https://docs.microsoft.com/windows/desktop/shell/fa-progids).

    Voici une brève description de chacun des champs que vous pouvez remplir dans le concepteur de manifeste :

| Champ | Description |
|------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Nom complet** | Spécifiez le nom complet d’un groupe de types de fichiers. Le nom d’affichage sert à identifier le type de fichier dans l’option [Définir les programmes par défaut](https://docs.microsoft.com/windows/desktop/shell/default-programs) du **Panneau de configuration**. |
| **Logo** | Spécifiez le logo utilisé pour identifier le type de fichier sur le Bureau et dans l’option [Définir les programmes par défaut](https://docs.microsoft.com/windows/desktop/shell/default-programs) du **Panneau de configuration**. Si aucun logo n’est spécifié, le petit logo de l’application est utilisé. |
| **Info-bulle** | Spécifiez l’[info-bulle](https://docs.microsoft.com/windows/desktop/shell/fa-progids) d’un groupe de types de fichier. Cette info-bulle s’affiche quand l’utilisateur pointe sur l’icône d’un fichier de ce type avec la souris. |
| **Nom** | Choisissez un nom pour un groupe de types de fichiers partageant les mêmes nom complet, logo, info-bulle et indicateurs de modification. Choisissez un nom de groupe pouvant rester le même sur toutes les applications à mettre à jour. **Remarque**  Le nom doit être indiqué entièrement en minuscules. |
| **Type de contenu** | Spécifiez le type de contenu MIME, par exemple **image/jpeg**, pour un type de fichier particulier. **Remarque importante sur les types de contenu autorisés :** voici la liste alphabétique des types de contenu MIME que vous ne pouvez pas inclure dans le manifeste du package, car ils sont réservés ou interdits : **application/force-download**, **application/octet-stream**, **application/unknown**, **application/x-msdownload**. |
| **Type de fichier** | Indiquez le type de fichier à inscrire, précédé d’un point (par exemple, « .jpeg »). **Types de fichier réservés et interdits :** pour obtenir la liste alphabétique des types de fichier des applications intégrées que vous ne pouvez pas inscrire dans vos applications UWP parce qu’ils sont réservés ou interdits, consultez [Noms de schéma d’URI et de fichier réservés](reserved-uri-scheme-names.md). |

2.  Entrez `alsdk` dans **Nom**.
3.  Entrez `.alsdk` comme **Type de fichier**.
4.  Entrez « images\\Icon. png » comme logo.
5.  Appuyez sur Ctrl+S pour enregistrer la modification dans package.appxmanifest.

Les étapes ci-dessus ajoutent un élément [**Extension**](https://docs.microsoft.com/uwp/schemas/appxpackage/appxmanifestschema/element-1-extension) tel que celui-ci dans le manifeste du package. La catégorie **windows.fileTypeAssociation** indique que l’application gère les fichiers portant l’extension `.alsdk`.

```xml
      <Extensions>
        <uap:Extension Category="windows.fileTypeAssociation">
          <uap:FileTypeAssociation Name="alsdk">
            <uap:Logo>images\icon.png</uap:Logo>
            <uap:SupportedFileTypes>
              <uap:FileType>.alsdk</uap:FileType>
            </uap:SupportedFileTypes>
          </uap:FileTypeAssociation>
        </uap:Extension>
      </Extensions>
```

## <a name="step-2-add-the-proper-icons"></a>Étape 2 : Ajouter les icônes appropriées

Les applications qui deviennent la valeur par défaut d’un type de fichier ont leurs icônes affichées à différents emplacements dans l’ensemble du système. Par exemple, ces icônes s’affichent dans :

-   la vue d’éléments de l’Explorateur Windows, les menus contextuels et le Ruban ;
-   l’applet Programmes par défaut du Panneau de configuration ;
-   le sélecteur de fichiers ;
-   les résultats de recherche sur l’écran d’accueil.

Incluez une icône 44 x 44 de votre projet, afin que votre logo puisse apparaître dans ces emplacements. Reproduisez l’apparence du logo de la vignette de l’application et utilisez la couleur d’arrière-plan de celle-ci au lieu de rendre l’icône transparente. Faites en sorte que le logo s’étende jusqu’au bord sans remplissage. Testez vos icônes sur des arrière-plans blancs. Voir [Recommandations en matière de ressources de vignette et d’icône](https://docs.microsoft.com/windows/uwp/design/shell/tiles-and-notifications/app-assets) pour plus d’informations sur les icônes.

## <a name="step-3-handle-the-activated-event"></a>Étape 3 : Gérer l’événement activé

Le gestionnaire d’événements [**OnFileActivated**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.onfileactivated) reçoit tous les événements d’activation des fichiers.

```csharp
protected override void OnFileActivated(FileActivatedEventArgs args)
{
       // TODO: Handle file activation
       // The number of files received is args.Files.Size
       // The name of the first file is args.Files[0].Name
}
```

```vb
Protected Overrides Sub OnFileActivated(ByVal args As Windows.ApplicationModel.Activation.FileActivatedEventArgs)
      ' TODO: Handle file activation
      ' The number of files received is args.Files.Size
      ' The name of the first file is args.Files(0).Name
End Sub
```

```cppwinrt
void App::OnFileActivated(Windows::ApplicationModel::Activation::FileActivatedEventArgs const& args)
{
    // TODO: Handle file activation.
    auto numberOfFilesReceived{ args.Files().Size() };
    auto nameOfTheFirstFile{ args.Files().GetAt(0).Name() };
}
```

```cpp
void App::OnFileActivated(Windows::ApplicationModel::Activation::FileActivatedEventArgs^ args)
{
    // TODO: Handle file activation
    // The number of files received is args->Files->Size
    // The name of the first file is args->Files->GetAt(0)->Name
}
```

> [!NOTE]
> Lorsqu’il est lancé via un contrat de fichier, veillez à ce que le bouton précédent ramène l’utilisateur à l’écran qui a lancé l’application et non au contenu précédent de l’application.

Nous vous recommandons de créer un nouveau **Frame** XAML pour chaque événement d’activation qui ouvre une nouvelle page. De cette façon, la pile de navigation pour le nouveau Frame XAML ne contient pas de contenu précédent que l’application peut avoir dans la fenêtre active lorsqu’elle est suspendue. Si vous décidez d’utiliser un seul **Frame** XAML pour le lancement et pour les contrats de fichier, vous devez effacer les pages dans le journal de navigation du **Frame**avant de naviguer vers une nouvelle page.

Lorsque votre application est lancée par le biais de l’activation de fichier, vous devez envisager d’inclure une interface utilisateur qui permet à l’utilisateur de revenir à la page supérieure de l’application.

## <a name="remarks"></a>Notes

Les fichiers que vous recevez peuvent provenir d’une source non approuvée. Nous vous recommandons de vérifier le contenu d’un fichier avant d’entreprendre une quelconque action sur ce fichier.

## <a name="related-topics"></a>Rubriques connexes

### <a name="complete-example"></a>Exemple complet

* [Exemple de lancement d’association](https://code.msdn.microsoft.com/windowsapps/Association-Launching-535d2cec)

### <a name="concepts"></a>Concepts

* [Programmes par défaut](https://docs.microsoft.com/windows/desktop/shell/default-programs)
* [Type de fichier et modèle d’association de protocole](https://docs.microsoft.com/windows/desktop/w8cookbook/file-type-and-protocol-associations-model)

### <a name="tasks"></a>Tâches

* [Lancer l’application par défaut pour un fichier](launch-the-default-app-for-a-file.md)
* [Gérer l’activation d’un URI](handle-uri-activation.md)

### <a name="guidelines"></a>Recommandations

* [Instructions pour les types de fichiers et les URI](https://docs.microsoft.com/windows/uwp/files/index)

### <a name="reference"></a>Référence
* [Windows. ApplicationModel. activation. FileActivatedEventArgs](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Activation.FileActivatedEventArgs)
* [Windows. UI. Xaml. application. OnFileActivated](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.onfileactivated)
