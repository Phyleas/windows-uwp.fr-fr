---
title: Configuration de builds automatisées pour votre application UWP
description: Configuration de builds automatisées pour produire des packages de chargement indépendant et/ou de stockage.
ms.date: 07/17/2019
ms.topic: article
keywords: windows 10, uwp
ms.assetid: f9b0d6bd-af12-4237-bc66-0c218859d2fd
ms.localizationpriority: medium
ms.openlocfilehash: 7f0e1d460ca52c659401bbb291deafa5746b7bb6
ms.sourcegitcommit: 53c00939b20d4b0a294936df3d395adb0c13e231
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91933110"
---
# <a name="set-up-automated-builds-for-your-uwp-app"></a>Configuration de builds automatisées pour votre application UWP

Vous pouvez utiliser Azure Pipelines pour créer des builds automatisées pour les projets UWP. Cet article vous présente différentes méthodes d’utilisation. Il explique également comment effectuer ces tâches à l’aide de la ligne de commande afin de procéder à l’intégration avec tout autre système de génération.

## <a name="create-a-new-azure-pipeline"></a>Créer un pipeline Azure

Commencez par [vous inscrire à Azure Pipelines](/azure/devops/pipelines/get-started/pipelines-sign-up) si vous ne l’avez pas déjà fait.

Ensuite, créez un pipeline que vous pouvez utiliser pour générer votre code source. Pour obtenir un tutoriel sur la création d’un pipeline en vue de créer un dépôt GitHub, consultez [Créer votre premier pipeline](/azure/devops/pipelines/get-started-yaml). Azure Pipelines prend en charge les types de dépôts listés [dans cet article](/azure/devops/pipelines/repos).

## <a name="set-up-an-automated-build"></a>Configuration d’une build automatisée

Nous allons commencer par la définition de build UWP par défaut, disponible dans Azure Dev Ops, puis vous expliquer comment configurer le pipeline.

Dans la liste des modèles de définition de build, choisissez le modèle **Plateforme Windows universelle**.

![Sélectionner le modèle UWP](images/select-yaml-template.png)

Ce modèle comprend la configuration de base pour générer votre projet UWP :

```yml
trigger:
- master

pool:
  vmImage: 'windows-latest'

variables:
  solution: '**/*.sln'
  buildPlatform: 'x86|x64|ARM'
  buildConfiguration: 'Release'
  appxPackageDir: '$(build.artifactStagingDirectory)\AppxPackages\\'

steps:
- task: NuGetToolInstaller@0

- task: NuGetCommand@2
  inputs:
    restoreSolution: '$(solution)'

- task: VSBuild@1
  inputs:
    platform: 'x86'
    solution: '$(solution)'
    configuration: '$(buildConfiguration)'
    msbuildArgs: '/p:AppxBundlePlatforms="$(buildPlatform)" /p:AppxPackageDir="$(appxPackageDir)" /p:AppxBundle=Always /p:UapAppxPackageBuildMode=StoreUpload'

```

Le modèle par défaut essaie de signer le package à l'aide du certificat spécifié dans le fichier .csproj. Pour signer votre package lors de sa génération, vous devez pouvoir accéder à la clé privée. À défaut, vous pouvez désactiver la signature en ajoutant le paramètre `/p:AppxPackageSigningEnabled=false` à la section `msbuildArgs` du fichier YAML.

## <a name="add-your-project-certificate-to-the-secure-files-library"></a>Ajouter votre certificat de projet à la bibliothèque de fichiers sécurisés

Vous devez dans la mesure du possible éviter de soumettre des certificats à votre dépôt, et Git les ignore par défaut. Pour gérer de manière sécurisée les fichiers sensibles tels que les certificats, Azure DevOps prend en charge la fonctionnalité [Fichiers sécurisés](/azure/devops/pipelines/library/secure-files?view=azure-devops).

Pour charger un certificat pour votre build automatisée

1. Dans Azure Pipelines, développez **Pipelines** dans le volet de navigation et cliquez sur **Bibliothèque**.
2. Cliquez sur l’onglet **Fichiers sécurisés**, puis sur **+ Fichier sécurisé**.

    ![Capture d’écran d’Azure avec l’option Bibliothèque mise en surbrillance montrant la page Fichiers sécurisés.](images/secure-file1.png)

3. Recherchez le fichier de certificat et cliquez sur **OK**.
4. Une fois le certificat chargé, sélectionnez-le pour afficher ses propriétés. Sous **Autorisations des pipelines**, activez l’option **Autoriser l’utilisation dans tous les pipelines**.

    ![Capture d’écran de la section Autorisations des pipelines, avec l’option Autoriser l’utilisation dans tous les pipelines sélectionnée.](images/secure-file2.png)

5. Si la clé privée dans le certificat a un mot de passe, nous vous recommandons de stocker votre mot de passe dans [Azure Key Vault](/azure/key-vault/about-keys-secrets-and-certificates) puis de le lier à un [groupe de variables](/azure/devops/pipelines/library/variable-groups). Vous pouvez utiliser la variable pour accéder au mot de passe à partir du pipeline. Notez qu’un mot de passe est pris en charge uniquement pour la clé privée. L’utilisation d’un fichier de certificat qui est lui-même protégé par mot de passe n’est pas prise en charge actuellement.

> [!NOTE]
> À partir de Visual Studio 2019, un certificat temporaire n’est plus généré dans les projets UWP. Pour créer ou exporter des certificats, utilisez les applets de commande PowerShell décrites dans [cet article](/windows/msix/package/create-certificate-package-signing).

## <a name="configure-the-build-solution-build-task"></a>Configuration de la tâche Générer la solution

Cette tâche compile toutes les solutions se trouvant dans le dossier de travail en fichiers binaires et produit un fichier de package d'application de sorte. Cette tâche utilise des arguments MSbuild. Vous devez spécifier la valeur de ces arguments. Inspirez-vous du tableau suivant.

|**Argument MSBuild**|**Valeur**|**Description**|
|--------------------|---------|---------------|
| AppxPackageDir | $(Build.ArtifactStagingDirectory)\AppxPackages | Définit le dossier de stockage des artefacts générés. |
| AppxBundlePlatforms | $(Build.BuildPlatform) | Vous permet de définir les plateformes à inclure dans le bundle. |
| AppxBundle | Toujours | Crée un fichier .msixbundle/.appxbundle avec les fichiers .msix/.appx pour la plateforme spécifiée. |
| UapAppxPackageBuildMode | StoreUpload | Génère le fichier .msixupload/.appxupload et le dossier **_Test** pour le chargement indépendant. |
| UapAppxPackageBuildMode | CI | Génère uniquement le fichier .msixupload/.appxupload. |
| UapAppxPackageBuildMode | SideloadOnly | Génère le dossier **_Test** pour le chargement indépendant uniquement. |
| AppxPackageSigningEnabled | true | Active la signature du package. |
| PackageCertificateThumbprint | Empreinte de certificat | Cette valeur **doit** correspondre à l’empreinte numérique du certificat de signature, ou être une chaîne vide. |
| PackageCertificateKeyFile | Path | Chemin du certificat à utiliser. Il est extrait des métadonnées de fichier sécurisé. |
| PackageCertificatePassword | Password | Mot de passe de la clé privée dans le certificat. Nous vous recommandons de stocker votre mot de passe dans [Azure Key Vault](/azure/key-vault/about-keys-secrets-and-certificates) et de le lier à un [groupe de variables](/azure/devops/pipelines/library/variable-groups). Vous pouvez passer la variable à cet argument. |

### <a name="configure-the-build"></a>Configurer la build

Si vous souhaitez générer votre solution à l’aide de la ligne de commande ou à l’aide d’un autre système de génération, exécutez MSBuild avec ces arguments.

```powershell
/p:AppxPackageDir="$(Build.ArtifactStagingDirectory)\AppxPackages\\"
/p:UapAppxPackageBuildMode=StoreUpload
/p:AppxBundlePlatforms="$(Build.BuildPlatform)"
/p:AppxBundle=Always
```

### <a name="configure-package-signing"></a>Configurer la signature du package

Pour signer le package MSIX (ou .appx), le pipeline doit récupérer le certificat de signature. Pour ce faire, ajoutez une tâche DownloadSecureFile avant la tâche VSBuild.
Vous aurez alors accès au certificat de signature par le biais de ```signingCert```.

```yml
- task: DownloadSecureFile@1
  name: signingCert
  displayName: 'Download CA certificate'
  inputs:
    secureFile: '[Your_Pfx].pfx'
```

Ensuite, mettez à jour la tâche VSBuild pour qu’elle référence le certificat de signature :

```yml
- task: VSBuild@1
  inputs:
    platform: 'x86'
    solution: '$(solution)'
    configuration: '$(buildConfiguration)'
    msbuildArgs: '/p:AppxBundlePlatforms="$(buildPlatform)" 
                  /p:AppxPackageDir="$(appxPackageDir)" 
                  /p:AppxBundle=Always 
                  /p:UapAppxPackageBuildMode=StoreUpload 
                  /p:AppxPackageSigningEnabled=true
                  /p:PackageCertificateThumbprint="" 
                  /p:PackageCertificateKeyFile="$(signingCert.secureFilePath)"'
```

> [!NOTE]
> Une chaîne vide est affectée intentionnellement à l’argument PackageCertificateThumbprint en guise de précaution. Si l’empreinte numérique est définie dans le projet, mais qu’elle ne correspond pas au certificat de signature, la génération échoue avec l’erreur : `Certificate does not match supplied signing thumbprint`.

### <a name="review-parameters"></a>Passer en revue les paramètres

Les paramètres définis avec la syntaxe `$()` sont des variables définies dans la définition de build. Ils sont différents dans d’autres systèmes de génération.

![variables par défaut](images/building-screen5.png)

Pour afficher toutes les variables prédéfinies, consultez [Variables de build prédéfinies](/azure/devops/pipelines/build/variables).

## <a name="configure-the-publish-build-artifacts-task"></a>Configurer la tâche Publier des artefacts de build

Le pipeline UWP par défaut n’enregistre pas les artefacts générés. Pour ajouter les fonctionnalités de publication à votre définition YAML, ajoutez les tâches suivantes.

```yml
- task: CopyFiles@2
  displayName: 'Copy Files to: $(build.artifactstagingdirectory)'
  inputs:
    SourceFolder: '$(system.defaultworkingdirectory)'
    Contents: '**\bin\$(BuildConfiguration)\**'
    TargetFolder: '$(build.artifactstagingdirectory)'

- task: PublishBuildArtifacts@1
  displayName: 'Publish Artifact: drop'
  inputs:
    PathtoPublish: '$(build.artifactstagingdirectory)'
```

Vous pouvez voir les artefacts générés dans l’option **Artefacts** de la page de résultats de la build.

![artefacts](images/building-screen6.png)

Étant donné que nous avons attribué à l'argument `UapAppxPackageBuildMode` la valeur `StoreUpload`, le dossier d’artefacts inclut le package pour la soumission au Store (.msixupload/.appxupload). Vous pouvez également soumettre un package d'application standard (.msix/.appx) ou un ensemble d’applications (.msixbundle/.appxbundle/) dans le Store. Dans le cadre de cet article, nous allons utiliser le fichier .appxupload.

## <a name="address-bundle-errors"></a>Résoudre les erreurs d'offre groupée

Si vous ajoutez plusieurs projets UWP à votre solution, puis tentez de créer une offre groupée, il se peut que vous receviez une erreur comme celle-ci.

  `MakeAppx(0,0): Error : Error info: error 80080204: The package with file name "AppOne.UnitTests_0.1.2595.0_x86.appx" and package full name "8ef641d1-4557-4e33-957f-6895b122f1e6_0.1.2595.0_x86__scrj5wvaadcy6" is not valid in the bundle because it has a different package family name than other packages in the bundle`

Cette erreur s’affiche car l’application qui doit apparaître dans l’offre groupée n’est pas clairement définie au niveau de la solution. Pour résoudre ce problème, ouvrez chaque fichier de projet et ajoutez les propriétés suivantes à la fin du premier élément `<PropertyGroup>`.

|**Projet**|**Propriétés**|
|-------|----------|
|Application|`<AppxBundle>Always</AppxBundle>`|
|UnitTests|`<AppxBundle>Never</AppxBundle>`|

Ensuite, supprimez l’argument MSBuild `AppxBundle` de l’étape de génération.

## <a name="related-topics"></a>Rubriques connexes

- [Créer votre application .NET pour Windows](/vsts/build-release/get-started/dot-net)
- [Création de packages d’application UWP](/windows/msix/package/packaging-uwp-apps)
- [Chargement indépendant d’applications métier dans Windows 10](/windows/deploy/sideload-apps-in-windows-10)
- [Créer un certificat de signature de package](/windows/msix/package/create-certificate-package-signing)