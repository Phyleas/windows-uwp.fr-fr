---
title: Publiez votre application de bureau empaquetée sur le Microsoft Store ou chargez sa version test sur un ou plusieurs appareils.
description: Découvrez comment utiliser le Pont du bureau pour distribuer une application de bureau packagée sur le Microsoft Store ou la charger de façon indépendante sur un ou plusieurs appareils.
ms.date: 05/18/2018
ms.topic: article
keywords: windows 10, uwp
ms.assetid: edff3787-cecb-4054-9a2d-1fbefa79efc4
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.openlocfilehash: 469ed71fcb42894b2dfd179ce21f44da3702705e
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89170643"
---
# <a name="distribute-your-packaged-desktop-app"></a>Distribuer votre application de bureau empaquetée

Si vous décidez d’[empaqueter votre application de bureau dans un package MSIX](/windows/msix/desktop/desktop-to-uwp-root), vous pouvez publier votre application empaquetée sur le Microsoft Store ou charger sa version de test sur un ou plusieurs appareils.

> [!NOTE]
> Avez-vous un plan pour la transition des utilisateurs vers votre application empaquetée ? Avant de distribuer votre application, consultez la section [Transition des utilisateurs vers votre application empaquetée](#transition-users) de ce guide où vous trouverez quelques idées.

## <a name="distribute-your-application-by-publishing-it-to-the-microsoft-store"></a>Distribuer votre application en la publiant sur le Microsoft Store

Le [Microsoft Store](https://www.microsoft.com/store/apps) est un moyen pratique de rendre votre application accessible aux clients.

Publiez votre application sur le Microsoft Store pour atteindre l’audience la plus large. En outre, les organisations clientes peuvent acquérir votre application pour la distribuer en interne au sein de leur organisation par le biais du [Microsoft Store pour Entreprises](https://businessstore.microsoft.com/store).

Si vous envisagez de publier sur le Microsoft Store, vous êtes invité à répondre à quelques questions supplémentaires dans le cadre du processus de soumission. En effet, le manifeste de votre package déclare une fonctionnalité restreinte nommée **runFullTrust**, et nous devons approuver l’utilisation de cette fonctionnalité par votre application. Pour en savoir plus sur cette exigence, voir ici : [Fonctionnalités restreintes](/windows/uwp/packaging/app-capability-declarations#restricted-capabilities).

Vous n’êtes pas obligé de signer votre application avant de la soumettre au Store.

>[!IMPORTANT]
> Si vous prévoyez de publier votre application sur le Microsoft Store, assurez-vous qu’elle fonctionne correctement sur les appareils qui exécutent Windows 10 S. Il s’agit d’une exigence imposée par le Store. Consultez [Tester votre application Windows pour Windows 10 S](/windows/msix/desktop/desktop-to-uwp-test-windows-s).

<a id="side-load"></a>

## <a name="distribute-your-application-without-placing-it-onto-the-microsoft-store"></a>Distribuer votre application sans la mettre sur le Microsoft Store

Si vous préférez distribuer votre application sans passer par le Store, sachez qu’il est possible de distribuer manuellement des applications pour un ou plusieurs appareils.

Cela peut être utile si vous souhaitez contrôler davantage l’expérience de distribution ou si vous ne voulez pas vous impliquer dans le processus de certification du Microsoft Store.

Pour distribuer votre application à d’autres appareils sans la placer sur le Store, vous devez obtenir un certificat, signer votre application à l’aide de celui-ci, puis charger votre application en version test sur ces appareils.

Vous pouvez [créer un certificat](/windows/msix/package/create-certificate-package-signing) ou en obtenir un auprès d’un fournisseur réputé tel que [Verisign](https://www.verisign.com/).

Si vous envisagez de distribuer votre application sur des appareils exécutant Windows 10 S, votre application doit être signée par le Microsoft Store. Vous devez donc au préalable passer par le processus de soumission de ce dernier.

Si vous créez un certificat, vous devez l’installer dans le magasin de certificats **Racine de confiance** ou **Personnes autorisées** sur chaque appareil exécutant votre application. Si vous obtenez un certificat auprès d’un fournisseur populaire, vous n’avez rien à installer sur des systèmes autres que votre application.  

> [!IMPORTANT]
> Vérifiez que le nom de l’éditeur mentionné sur votre certificat correspond à celui de l’éditeur de votre application.

Pour signer votre application à l’aide d’un certificat, consultez [Signer un package d’application à l’aide de SignTool](/windows/msix/package/sign-app-package-using-signtool).

Pour charger votre application sur d’autres appareils, consultez [Chargement de la version test d’applications métier dans Windows 10](/windows/application-management/sideload-apps-in-windows-10).

<a id="transition-users"></a>

## <a name="transition-users-to-your-packaged-app"></a>Transition des utilisateurs vers votre application empaquetée

Avant de distribuer votre application, envisagez d’ajouter quelques extensions au manifeste de votre package pour aider les utilisateurs à s’habituer à utiliser votre application empaquetée. Voici certaines choses que vous pouvez faire.

* Pointer les vignettes existantes de l’écran de démarrage et les boutons de barre des tâches vers votre application empaquetée.
* Associer votre application empaquetée à un ensemble de types de fichiers.
* Autoriser votre application empaquetée à ouvrir certains types de fichiers par défaut.

Pour obtenir la liste complète des extensions et des conseils pour leur utilisation, consultez [Transition des utilisateurs vers votre application](desktop-to-uwp-extensions.md#transition-users-to-your-app).

Envisagez également d’ajouter du code à votre application empaquetée accomplissant les tâches suivantes :

* migrer les données utilisateur associées à votre application de bureau vers les dossiers appropriés de votre application empaquetée ;
* permettre aux utilisateurs de désinstaller la version de bureau de votre application.

Examinons chacune de ces tâches. Nous allons commencer par la migration des données utilisateur.

### <a name="migrate-user-data"></a>Migrer les données utilisateur

Si vous comptez ajouter du code qui effectuera la migration des données utilisateur, il est préférable de n’exécuter ce code que lors du démarrage initial de l’application. Avant de migrer les données de l’utilisateur, affichez une boîte de dialogue lui expliquant ce qui est en train de se produire, les raisons pour lesquelles cette action est recommandée, et ce qui va advenir des données existantes.

Voici un exemple montrant comment vous pourriez procéder dans une application empaquetée .NET.

```csharp
private void MigrateUserData()
{
    String sourceDir = Environment.GetFolderPath
        (Environment.SpecialFolder.ApplicationData) + "\\AppName";

    if (sourceDir != null)
    {
        DialogResult migrateResult = MessageBox.Show
            ("Would you like to migrate your data from the previous version of this app?",
             "Data Migration", MessageBoxButtons.YesNo);

        if (migrateResult.Equals(DialogResult.Yes))
        {
            String destinationDir =
                Windows.Storage.ApplicationData.Current.LocalFolder.Path + "\\AppName";

            Process process = new Process();
            process.StartInfo.FileName = "robocopy.exe";
            process.StartInfo.Arguments = "%LOCALAPPDATA%\\AppName " + destinationDir + " /move";
            process.StartInfo.CreateNoWindow = true;
            process.Start();
            process.WaitForExit();

            if (process.ExitCode > 1)
            {
                //Migration was unsuccessful -- you can choose to block/retry/other action
            }
        }
    }
}
```

### <a name="uninstall-the-desktop-version-of-your-app"></a>Désinstaller la version bureau de votre application

Il est préférable de ne pas désinstaller l’application de bureau des utilisateurs sans leur autorisation. Affichez une boîte de dialogue leur demandant cette autorisation. Les utilisateurs peuvent décider de conserver la version bureau de votre application. Dans ce cas, vous devrez décider si vous souhaitez bloquer l’utilisation de l’application de bureau ou prendre en charge la cohabitation des deux applications.

Voici un exemple montrant comment procéder dans une application empaquetée .NET.

Pour voir le contexte complet de cet extrait de code, examinez le fichier **MainWindow.cs** de l’exemple [Visionneuse d’images WPF avec transition/migration/désinstallation](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/DesktopAppTransition).

```csharp
private void RemoveDesktopApp()
{
    //Typically, you can find your uninstall string at this location.
    String uninstallString = (String)Microsoft.Win32.Registry.GetValue
        (@"HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\Microsoft\Windows\CurrentVersion" +
         @"\Uninstall\{7AD02FB8-B85E-44BC-8998-F4803BA5A0E3}\", "UninstallString", null);

    //Detect if the previous version of the Desktop application is installed.
    if (uninstallString != null)
    {
        DialogResult uninstallResult = MessageBox.Show
            ("To have the best experience, consider uninstalling the "
              + " previous version of this app. Would you like to do that now?",
              "Uninstall the previous version", MessageBoxButtons.YesNo);

        if (uninstallResult.Equals(DialogResult.Yes))
        {
                    string[] uninstallArgs = uninstallString.Split(' ');

            Process process = new Process();
            process.StartInfo.FileName = uninstallArgs[0];
            process.StartInfo.Arguments = uninstallArgs[1];
            process.StartInfo.CreateNoWindow = true;

            process.Start();
            process.WaitForExit();

            if (process.ExitCode != 0)
            {
                //Uninstallation was unsuccessful - You can choose to block the application here.
            }
        }
    }

}
```

## <a name="next-steps"></a>Étapes suivantes

Des questions ? Contactez-nous sur Stack Overflow. Notre équipe supervise ces [étiquettes](https://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge). Vous pouvez également nous poser vos questions [ici](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D).

Si vous rencontrez des problèmes lors de la publication de votre application sur le Store, ce [billet de blog](/archive/blogs/appconsult/preparing-a-desktop-bridge-application-for-the-store-submission) contient des conseils utiles.