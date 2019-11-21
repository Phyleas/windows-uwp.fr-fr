---
ms.assetid: 88e16ec8-deff-4a60-bda6-97c5dabc30b8
description: This topic presents a case study of porting a functioning peer-to-peer quiz game WinRT 8.1 sample app to a Windows 10 Universal Windows Platform (UWP) app.
title: 'Étude de cas de portage d’application Windows Runtime 8.x vers UWP : exemple d’application d’homologue à homologue QuizGame'
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: b03e13352717c5e414dda60fe413b00edc9ba827
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74260117"
---
# <a name="windows-runtime-8x-to-uwp-case-study-quizgame-sample-app"></a>Étude de cas de portage d’application Windows Runtime 8.x vers UWP : exemple d’application QuizGame




This topic presents a case study of porting a functioning peer-to-peer quiz game WinRT 8.1 sample app to a Windows 10 Universal Windows Platform (UWP) app.

A Universal 8.1 app is one that builds two versions of the same app: one app package for Windows 8.1, and a different app package for Windows Phone 8.1. La version WinRT 8.1 de l’application QuizGame utilise une disposition de projet d’application Windows universelle, mais adopte une approche différente et génère une application fonctionnellement distincte pour les deux plates-formes. The Windows 8.1 app package serves as the host for a quiz game session, while the Windows Phone 8.1 app package plays the role of the client to the host. Les deux composantes de la session de jeu-questionnaire communiquent via un réseau homologue à homologue.

Une adaptation personnalisée de ces deux composantes pour un PC et un téléphone (respectivement) semble appropriée. Toutefois, ne serait-il pas préférable de pouvoir exécuter le client et l’hôte sur n’importe quel appareil ? In this case study, we'll port both apps to Windows 10 where they will each build into a single app package that users can install onto a wide range of devices.

L’application utilise des modèles qui exploitent des affichages et des modèles d’affichage. Grâce à cette séparation nette, le processus de portage de cette application est très direct, comme vous allez le constater.

**Note**  This sample assumes your network is configured to send and receive custom UDP group multicast packets (most home networks are, although your work network may not be). Cet exemple envoie et reçoit également des paquets TCP.

 

**Note**   When opening QuizGame10 in Visual Studio, if you see the message "Visual Studio update required", then follow the steps in [TargetPlatformVersion](w8x-to-uwp-troubleshooting.md).

 

## <a name="downloads"></a>Téléchargements

[Téléchargez l’application 8.1 universelle QuizGame](https://codeload.github.com/MicrosoftDocs/windows-topic-specific-samples/zip/QuizGame). Il s’agit de l’état initial de l’application avant le portage. 

[Download the QuizGame10 Windows 10 app](https://codeload.github.com/MicrosoftDocs/windows-topic-specific-samples/zip/QuizGame10). Il s’agit de l’état de l’application juste après le portage. 

[Voir la dernière version de cet exemple sur GitHub](https://github.com/microsoft/Windows-appsample-networkhelper).

## <a name="the-winrt-81-solution"></a>Solution WinRT 8.1


Voici à quoi ressemble QuizGame, l’application que nous allons porter.

![Application QuizGame hôte s’exécutant sur Windows](images/w8x-to-uwp-case-studies/c04-01-win81-how-the-host-app-looks.png)

Application QuizGame hôte s’exécutant sur Windows

 

![Application QuizGame cliente s’exécutant sur Windows Phone](images/w8x-to-uwp-case-studies/c04-02-wp81-how-the-client-app-looks.png)

Application QuizGame cliente s’exécutant sur Windows Phone

## <a name="a-walkthrough-of-quizgame-in-use"></a>Procédure pas à pas de l’application QuizGame en cours d’utilisation

Il s’agit d’un compte-rendu hypothétique de l’application en cours d’utilisation, qui fournit cependant des informations utiles si vous souhaitez tester l’application vous-même sur votre réseau sans fil.

Un jeu-questionnaire amusant est diffusé dans un bar. Un immense écran de télévision est installé ; tous les clients peuvent le voir. L’animateur dispose d’un PC, dont la sortie est affichée sur l’écran de télévision. Sur ce PC s’exécute « l’application hôte ». Toute personne qui souhaite participer à ce questionnaire doit simplement installer « l’application cliente » sur son téléphone ou sur sa tablette Surface.

L’application hôte est en mode d’introduction ; l’écran de télévision indique qu’elle est prête et attend la connexion des applications clientes. Joanna lance l’application cliente sur son appareil mobile. Elle tape son nom dans la zone de texte **Nom du joueur**, puis appuie sur **Rejoindre le jeu**. L’application hôte reconnaît la connexion de Joanna en affichant son nom. Quant à l’application cliente de Joanna, elle indique qu’elle attend le début de la partie. Ensuite, Maxwell effectue la même procédure sur son appareil mobile.

L’animateur clique sur **Démarrer le jeu** et l’application hôte affiche une question, ainsi que les différentes réponses possibles (elle affiche également une liste des joueurs ayant rejoint la partie, en utilisant une police normale grise). Simultanément, les réponses s’affichent sur les boutons des appareils clients connectés. Joanna appuie sur le bouton indiquant la réponse « 1975 ». À ce moment, tous les boutons sont désactivés. Sur l’application hôte, le nom de Joanna s’affiche en vert (et en gras), ce qui indique que sa réponse a bien été reçue. Les réponses de Maxwell s’affichent de la même manière. L’animateur remarque que tous les noms de joueurs sont affichés en vert. Il clique alors sur **Question suivante**.

Le jeu se poursuit. Une question est posée et reçoit une réponse ; l’animateur pose la suivante, et ainsi de suite. Une fois la dernière question affichée sur l’application hôte, le bouton indique **Afficher les résultats**, et non plus **Question suivante**. Lorsque l’animateur clique sur **Afficher les résultats**, les résultats apparaissent. Un clic sur **Revenir à la page d’introduction** vous ramène au début du cycle du jeu, sauf que les joueurs restent connectés. Toutefois, le retour à la page d’introduction permet à de nouveaux joueurs de participer et aux joueurs déjà connectés, de quitter la partie (même s’ils peuvent la quitter à tout moment en appuyant sur **Quitter la partie**).

## <a name="local-test-mode"></a>Mode test local

Pour tester l’application et ses interactions sur un seul PC, et non sur des appareils distribués, vous pouvez générer l’application hôte en mode test local. Ce mode ne tient pas compte de l’utilisation du réseau. Au lieu de cela, l’interface utilisateur de l’application hôte affiche la partie hôte à gauche de la fenêtre et, à droite, deux copies de l’interface utilisateur d’application cliente empilées verticalement (dans cette version, l’interface utilisateur de mode test local est fixe pour un affichage PC ; il ne s’adapte pas aux appareils de petite taille). Dans la même application, ces segments de l’interface utilisateur communiquent entre eux par le biais d’une fonction Communicator de client fictive, qui simule des interactions survenant sur le réseau.

Pour activer le mode test local, définissez l’élément **LOCALTESTMODEON** (dans les propriétés du projet) en tant que symbole de compilation conditionnelle, puis relancez la génération.

## <a name="porting-to-a-windows10-project"></a>Porting to a Windows 10 project

L’application QuizGame comporte les éléments suivants :

-   P2PHelper. Il s’agit d’une bibliothèque de classes portable, qui contient la logique du réseau homologue à homologue.
-   QuizGame.Windows. This is the project that builds the app package for the host app, which targets Windows 8.1.
-   QuizGame.WindowsPhone. Il s’agit du projet qui génère le package d’application pour l’application client, qui cible Windows Phone 8.1.
-   QuizGame.Shared. Il s’agit du projet qui contient le code source, les fichiers de balisage et d’autres actifs et ressources qui sont utilisés par les deux autres projets.

Pour cette étude de cas, nous disposons des options habituelles décrites dans la section [Si vous disposez d’une application 8.1 universelle](w8x-to-uwp-root.md), relative aux appareils à prendre en charge.

Based on those options, we'll port QuizGame.Windows to a new Windows 10 project called QuizGameHost. And, we'll port QuizGame.WindowsPhone to a new Windows 10 project called QuizGameClient. Ces projets ciblent la famille d’appareils universels ; ainsi, ils peuvent s’exécuter sur n’importe quel appareil. Nous allons laisser les fichiers sources de l’élément QuizGame.Shared, entre autres, dans leur dossier, et lier les fichiers partagés dans les deux nouveaux projets. Comme auparavant, nous allons conserver tous les éléments en une seule solution, que nous appellerons QuizGame10.

**The QuizGame10 solution**

-   Create a new solution (**New Project** &gt; **Other Project Types** &gt; **Visual Studio Solutions**) and name it QuizGame10.

**P2PHelper**

-   In the solution, create a new Windows 10 class library project (**New Project** &gt; **Windows Universal** &gt; **Class Library (Windows Universal)** ) and name it P2PHelper.
-   Dans le nouveau projet, supprimez le fichier Class1.cs.
-   Copiez les fichiers P2PSession.cs, P2PSessionClient.cs et P2PSessionHost.cs dans le dossier du nouveau projet, puis insérez les fichiers copiés dans le nouveau projet.
-   Le projet est généré, aucune autre modification n’était nécessaire.

**Shared files**

-   Copy the folders Common, Model, View, and ViewModel from \\QuizGame.Shared\\ to \\QuizGame10\\.
-   Lorsque nous évoquons les dossiers partagés sur le disque, c’est à ces dossiers (Common, Model, View et ViewModel) que nous faisons allusion.

**QuizGameHost**

-   Create a new Windows 10 app project (**Add** &gt; **New Project** &gt; **Windows Universal** &gt; **Blank Application (Windows Universal)** ) and name it QuizGameHost.
-   Add a reference to P2PHelper (**Add Reference** &gt; **Projects** &gt; **Solution** &gt; **P2PHelper**).
-   Dans l’**Explorateur de solutions**, créez un dossier pour chacun des dossiers partagés sur le disque. In turn, right-click each folder you just created and click **Add** &gt; **Existing Item** and navigate up a folder. Ouvrez le dossier partagé approprié, sélectionnez tous les fichiers, puis cliquez sur **Ajouter en tant que lien**.
-   Copy MainPage.xaml from \\QuizGame.Windows\\ to \\QuizGameHost\\ and change the namespace to QuizGameHost.
-   Copy App.xaml from \\QuizGame.Shared\\ to \\QuizGameHost\\ and change the namespace to QuizGameHost.
-   Au lieu de remplacer le fichier app.xaml.cs, nous allons conserver sa version dans le nouveau projet en lui apportant une seule modification ciblée afin d’assurer la prise en charge du mode test local. Dans le fichier app.xaml.cs, remplacez cette ligne de code :

```CSharp
rootFrame.Navigate(typeof(MainPage), e.Arguments);
```

par :

```CSharp
#if LOCALTESTMODEON
    rootFrame.Navigate(typeof(TestView), e.Arguments);
#else
    rootFrame.Navigate(typeof(MainPage), e.Arguments);
#endif
```

-   In **Properties** &gt; **Build** &gt; **conditional compilation symbols**, add LOCALTESTMODEON.
-   Vous pourrez maintenant revenir au code que vous avez ajouté dans le fichier app.xaml.cs et résoudre le type TestView.
-   Dans le fichier package.appxmanifest, remplacez le nom de la fonctionnalité « internetClient » par « internetClientServer ».

**QuizGameClient**

-   Create a new Windows 10 app project (**Add** &gt; **New Project** &gt; **Windows Universal** &gt; **Blank Application (Windows Universal)** ) and name it QuizGameClient.
-   Add a reference to P2PHelper (**Add Reference** &gt; **Projects** &gt; **Solution** &gt; **P2PHelper**).
-   Dans l’**Explorateur de solutions**, créez un dossier pour chacun des dossiers partagés sur le disque. In turn, right-click each folder you just created and click **Add** &gt; **Existing Item** and navigate up a folder. Ouvrez le dossier partagé approprié, sélectionnez tous les fichiers, puis cliquez sur **Ajouter en tant que lien**.
-   Copy MainPage.xaml from \\QuizGame.WindowsPhone\\ to \\QuizGameClient\\ and change the namespace to QuizGameClient.
-   Copy App.xaml from \\QuizGame.Shared\\ to \\QuizGameClient\\ and change the namespace to QuizGameClient.
-   Dans le fichier package.appxmanifest, remplacez le nom de la fonctionnalité « internetClient » par « internetClientServer ».

Vous pourrez maintenant générer l’application et l’exécuter.

## <a name="adaptive-ui"></a>Interface utilisateur adaptative

The QuizGameHost Windows 10 app looks fine when the app is running in a wide window (which is only possible on a device with a large screen). Par contre, lorsque la fenêtre d’application est étroite (comme sur un appareil de petite taille, voire sur certains appareils plus grands), l’interface utilisateur est tellement écrasée qu’elle en devient illisible.

Nous pouvons utiliser la fonction adaptative de gestionnaire d’état visuel pour remédier au problème, comme nous l’avons expliqué dans la section [Étude de cas : Bookstore2](w8x-to-uwp-case-study-bookstore2.md). Tout d’abord, définissez les propriétés sur les éléments visuels afin que, par défaut, l’interface utilisateur soit affichée selon une disposition étroite. All of these changes take place in \\View\\HostView.xaml.

-   Dans l’élément **Grid** principal, modifiez le paramètre **Height** du premier **RowDefinition** en remplaçant « 140 » par « Auto ».
-   Sur l’élément **Grid** qui contient le **TextBlock** nommé `pageTitle`, définissez `x:Name="pageTitleGrid"` et `Height="60"`. Ces deux premières étapes sont organisées de telle sorte que nous puissions contrôler efficacement la hauteur de ce paramètre **RowDefinition** par le biais d’une méthode setter dans un état visuel.
-   Sur `pageTitle`, définissez `Margin="-30,0,0,0"`.
-   Sur l’élément **Grid** signalé par le commentaire `<!-- Content -->`, définissez `x:Name="contentGrid"` et `Margin="-18,12,0,0"`.
-   Sur l’élément **TextBlock** situé juste au-dessus du commentaire `<!-- Options -->`, définissez `Margin="0,0,0,24"`.
-   Dans le style **TextBlock** par défaut (première ressource du fichier), remplacez la valeur de la méthode setter **FontSize** par « 15 ».
-   Dans `OptionContentControlStyle`, remplacez la valeur de la méthode setter **FontSize** par « 20 ». Cette étape et l’étape précédente nous permettent d’obtenir une rampe d’un type correct, qui fonctionnera efficacement sur tous les appareils. These are much more flexible sizes than the "30" we were using for the Windows 8.1 app.
-   Enfin, ajoutez le balisage du Gestionnaire d’état visuel approprié à l’élément **Grid** racine.

```xml
<VisualStateManager.VisualStateGroups>
    <VisualStateGroup>
        <VisualState x:Name="WideState">
            <VisualState.StateTriggers>
                <AdaptiveTrigger MinWindowWidth="548"/>
            </VisualState.StateTriggers>
            <VisualState.Setters>
                <Setter Target="pageTitleGrid.Height" Value="140"/>
                <Setter Target="pageTitle.Margin" Value="0,0,30,40"/>
                <Setter Target="contentGrid.Margin" Value="40,40,0,0"/>
            </VisualState.Setters>
        </VisualState>
    </VisualStateGroup>
</VisualStateManager.VisualStateGroups>
```

## <a name="universal-styling"></a>Stylisation universelle


You'll notice that in Windows 10, the buttons don't have the same touch-target padding in their template. Deux petites modifications devraient résoudre le problème. Tout d’abord, ajoutez ce balisage dans le fichier app.xaml des projets QuizGameHost et QuizGameClient.

```xml
<Style TargetType="Button">
    <Setter Property="Margin" Value="12"/>
</Style>
```

And second, add this setter to `OptionButtonStyle` in \\View\\ClientView.xaml.

```xml
<Setter Property="Margin" Value="6"/>
```

Grâce à ce dernier ajustement, l’application se comportera comme auparavant et aura le même aspect qu’avant le portage, à une exception près : elle pourra s’exécuter sur tous les types d’appareils.

## <a name="conclusion"></a>Conclusion

L’application que nous avons portée dans le cadre de cette étude de cas était relativement complexe, car elle impliquait plusieurs projets, une bibliothèque de classes, une interface utilisateur assez volumineuse et une grande quantité de code. Pourtant, son portage s’est révélé très simple. Some of the ease of porting is directly attributable to the similarity between the Windows 10 developer platform and the Windows 8.1 and Windows Phone 8.1 platforms. Le mode de conception de l’application d’origine, qui séparait les modèles, les modèles d’affichage et les affichages, contribue également à simplifier cette opération.
