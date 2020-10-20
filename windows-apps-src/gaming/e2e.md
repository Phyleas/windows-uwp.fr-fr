---
title: Guide de développement de jeux Windows 10
description: Guide complet sur les ressources et les informations nécessaires au développement de jeux de plateforme Windows universelle (UWP).
ms.assetid: 6061F498-96A8-44EF-9711-68AE5A1218C9
ms.date: 04/16/2018
ms.topic: article
keywords: Windows 10, UWP, jeux, développement de jeux
ms.localizationpriority: medium
ms.openlocfilehash: c6cae9e2416eb992815f098649d6b02ee472da14
ms.sourcegitcommit: e39b569626804d2ce4246353ac2c03a916dc9737
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/19/2020
ms.locfileid: "92192969"
---
# <a name="windows-10-game-development-guide"></a>Guide de développement de jeux Windows 10

Bienvenue dans le guide de développement de jeux Windows 10 !

Ce guide regroupe les ressources et les informations dont vous avez besoin pour développer un jeu UWP. Une version anglaise (US) de ce guide est disponible au format [PDF](https://download.microsoft.com/download/9/C/9/9C9D344F-611F-412E-BB01-259E5C76B17F/Windev_Game_Dev_Guide_Oct_2017.pdf) .

## <a name="introduction-to-game-development-for-the-universal-windows-platform-uwp"></a>Présentation du développement de jeux pour la plateforme Windows universelle (UWP)

Lorsque vous créez un jeu Windows 10, vous avez la possibilité d’atteindre des millions de joueurs du monde entier sur téléphone, PC et Xbox One. Avec Xbox sur Windows, Xbox Live, le mode multijoueur multiappareil, une communauté de joueurs incroyable et de nouvelles fonctionnalités performantes comme la plateforme universelle Windows (UWP) et DirectX 12, les jeux Windows 10 vont ravir les joueurs de tous âges et de toutes sortes. La nouvelle plateforme Windows universelle (UWP) permet à votre jeu d’être compatible avec tous les appareils Windows 10 en offrant une API commune pour téléphone, PC et Xbox One, ainsi que des outils et des options pour adapter votre jeu à chaque appareil.

Ce guide fournit une collection complète des informations et des ressources qui vous aideront lors du développement de votre jeu. Les sections sont organisées en fonction des étapes de développement du jeu. Vous savez donc où rechercher les informations lorsque vous en avez besoin.

Si vous ne connaissez pas le développement de jeux sur Windows ou Xbox, le Guide de [prise en main](getting-started.md) peut être là où vous souhaitez commencer. La section des [ressources de développement de jeux](#game-development-resources) fournit également une enquête de haut niveau sur la documentation, les programmes et d’autres ressources qui sont utiles lors de la création d’un jeu. Si vous souhaitez commencer par examiner un code UWP à la place, consultez [exemples de jeux](#game-samples).

Ce guide sera mis à jour lorsque des ressources et des documents relatifs au développement de jeux Windows 10 seront disponibles.

## <a name="game-development-resources"></a>Ressources de développement de jeux

De la documentation aux programmes de développement, en passant par les forums, les blogs et les exemples, de nombreuses ressources sont disponibles pour vous aider à développer des jeux. Voici un résumé des ressources à connaître lorsque vous commencez à développer votre jeu Windows 10.

> [!Note]
> Certaines fonctionnalités sont gérées par le biais de différents programmes. Comme ce guide couvre une large gamme de ressources, vous pouvez donc constater que certaines ressources ne sont pas accessibles selon le programme que vous utilisez ou votre rôle de développement. Les exemples sont les liens developer.xboxlive.com, forums.xboxlive.com, xdi.xboxlive.com ou réseau GDN (Game Developer Network). Pour plus d’informations sur le partenariat avec Microsoft, voir [Programmes pour développeurs](#developer-programs).

### <a name="game-development-documentation"></a>Documentation sur le développement de jeux

Tout au long de ce guide, vous trouverez des liens ciblés vers la documentation appropriée, organisés par tâche, technologie et étape du développement du jeu. Pour vous donner une vue d’ensemble de ce qui est disponible, voici les principaux portails de documentation destinés au développement de jeux Windows 10.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Portail principal du Centre de développement Windows</td>
        <td><a href="https://developer.microsoft.com/windows">Centre de développement Windows</a></td>
    </tr>
    <tr>
        <td>Développement des applications Windows</td>
        <td><a href="https://developer.microsoft.com/windows/apps/develop">Développer des applications Windows</a></td>
    </tr>
    <tr>
        <td>Développement d’une application de plateforme universelle Windows</td>
        <td><a href="https://developer.microsoft.com/windows/apps">Guides de procédure pour les applications Windows 10</a></td>
    </tr>
    <tr>
        <td>Guides de procédure pour les jeux UWP</td>
        <td><a href="index.md">Jeux et DirectX</a> </td>
    </tr>
    <tr>
        <td>Présentations et informations de référence sur DirectX</td>
        <td><a href="/windows/desktop/directx">Graphiques et jeux DirectX</a></td>
    </tr>
    <tr>
        <td>Azure pour les jeux</td>
        <td><a href="https://azure.microsoft.com/solutions/gaming/">Utiliser Azure pour développer et faire évoluer vos jeux</a></td>
    </tr>
    <tr>
        <td>PlayFab</td>
        <td><a href="https://api.playfab.com/">Solution backend complète pour les jeux en direct</a></td>
    </tr>
    <tr>
        <td>UWP sur Xbox One</td>
        <td><a href="/windows/uwp/xbox-apps/index">Création d’applications UWP sur Xbox One</a></td>
    </tr>
    <tr>
        <td>UWP sur HoloLens</td>
        <td><a href="https://developer.microsoft.com/windows/mixed-reality/development_overview">Création d’applications UWP sur HoloLens</a></td>
    </tr>
    <tr>
        <td>Documentation Xbox Live</td>
        <td><a href="/gaming/xbox-live/">Guide du développeur Xbox Live</a></td>
    </tr>
    <tr>
        <td>Documentation sur le développement Xbox One (XGD)</td>
        <td><a href="https://developer.microsoft.com/games/xbox/partner/development-home">Développement Xbox One</a></td>
    </tr>
    <tr>
        <td>Livres blancs sur le développement Xbox One (XGD)</td>
        <td><a href="https://developer.microsoft.com/games/xbox/partner/development-education-whitepapers">Livres blancs</a></td>
    </tr>
    <tr>
        <td>Documentation interactive de mixer</td>
        <td><a href="https://dev.mixer.com/reference/interactive/index.html">Ajouter de l’interactivité à votre jeu</a></td>
    </tr>
</table>

### <a name="partner-center"></a>Espace partenaires

L' [inscription d’un compte de développeur dans l’espace partenaires](https://developer.microsoft.com/store/register) est la première étape de la publication de votre jeu Windows. Un compte de développeur vous permet de réserver le nom de votre jeu et de soumettre des jeux gratuits ou payants au Microsoft Store pour tous les appareils Windows. Utilisez votre compte de développeur pour gérer votre jeu et les produits intégrés au jeu, obtenir des analyses détaillées et activer des services qui créent des expériences exceptionnelles pour vos joueurs dans le monde entier.

Microsoft propose également plusieurs programmes de développement pour vous aider à développer et à publier des jeux Windows. Nous vous recommandons de vérifier si l’un de vos droits est correct avant de vous inscrire à un compte de l’espace partenaires. Pour plus d’informations, accédez à [programmes de développement](#developer-programs)

### <a name="developer-programs"></a>Programmes pour développeurs

Microsoft propose plusieurs programmes pour développeurs pour vous aider à développer et à publier des jeux Windows. Envisagez de rejoindre un programme de développement si vous souhaitez développer des jeux pour Xbox One et intégrer des fonctionnalités Xbox Live dans votre jeu. Pour publier un jeu dans le Microsoft Store, vous devez également créer un compte de développeur dans l' [espace partenaires](https://partner.microsoft.com/dashboard) .

#### <a name="xbox-live-creators-program"></a>Programme Xbox Live Creators

Le programme de créateurs de Xbox Live permet à toute personne d’intégrer Xbox Live dans son titre et de la publier sur Xbox One et Windows 10. Il existe un processus de certification simplifié et aucune approbation de concept n’est requise en dehors des [stratégies de Microsoft Store](/legal/windows/agreements/store-policies)standard.

Vous pouvez déployer, concevoir et publier votre jeu dans le programme Creators sans kit de développement dédié, en utilisant uniquement du matériel de vente au détail. Pour commencer, téléchargez l' [application d’activation en mode dev](../xbox-apps/devkit-activation.md) sur votre Xbox.

Si vous souhaitez accéder à d’autres fonctionnalités Xbox Live, à une prise en charge du marketing et du développement dédié, et à la possibilité de figurer dans le magasin Xbox principal, appliquez le [ID@Xbox](https://www.xbox.com/Developers/id) programme.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Programme Créateurs Xbox Live</td>
        <td><a href="https://developer.microsoft.com/games/xbox/xboxlive/creator">En savoir plus sur le programme Xbox Live Creators</a></td>
    </tr>
</table>

#### <a name="idxbox"></a>ID@Xbox

Le ID@Xbox programme aide les développeurs de jeux qualifiés à publier eux-mêmes sur Windows et Xbox One. Si vous souhaitez développer pour Xbox One, ou ajouter des fonctionnalités Xbox Live comme joueur, réalisations et leaderboards à votre jeu Windows 10, inscrivez-vous avec ID@Xbox . Devenez ID@Xbox développeur pour bénéficier des outils et du support dont vous avez besoin pour libérer votre créativité et maximiser votre réussite. Nous vous recommandons d’appliquer ID@Xbox avant d’inscrire un compte de développeur dans l’espace partenaires.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>ID@Xbox programme de développement</td>
        <td><a href="https://www.xbox.com/Developers/id">Programme de développement indépendant pour Xbox One</a></td>
    </tr>
    <tr>
        <td>ID@Xbox site grand public</td>
        <td><a href="https://www.idatxbox.com/">ID@Xbox</a></td>
    </tr>
</table>

#### <a name="xbox-tools-and-middleware"></a>Outils et intergiciels (middleware) Xbox

Les outils Xbox et le programme intergiciel cèdent sous licence des kits de développement Xbox aux développeurs professionnels d’outils de jeux et d’intergiciels. Les développeurs acceptés dans le programme peuvent partager et distribuer leurs technologies XDK Xbox à d’autres développeurs Xbox sous licence.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Contacter le programme d’outils et d’intergiciels</td>
        <td><xboxtlsm@microsoft.com></td>
    </tr>
</table>

### <a name="game-samples"></a>Exemples de jeux

De nombreux exemples de jeu et d’application Windows 10 sont disponibles pour vous aider à comprendre les fonctionnalités de jeux de Windows 10 et à démarrer rapidement le développement de jeux. D’autres exemples sont développés et publiés régulièrement. En conséquence, n’oubliez pas de consulter de temps en temps les portails des exemples pour en voir les nouveautés. Vous pouvez également [consulter](https://help.github.com/en/articles/watching-and-unwatching-repositories) les référentiels GitHub pour être averti des modifications et des ajouts.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Exemples d’applications de plateforme universelle Windows</td>
        <td><a href="https://github.com/Microsoft/Windows-universal-samples">Windows-universal-samples</a></td>
    </tr>
    <tr>
        <td>Exemples de graphiques Direct3D 12</td>
        <td><a href="https://github.com/Microsoft/DirectX-Graphics-Samples">DirectX-Graphics-Samples</a></td>
    </tr>
    <tr>
        <td>Exemples de graphiques Direct3D 11</td>
        <td><a href="https://github.com/walbourn/directx-sdk-samples">directx-sdk-samples</a></td>
    </tr>
    <tr>
        <td>Exemple de jeu à la première personne Direct3D 11</td>
        <td><a href="tutorial--create-your-first-uwp-directx-game.md">Créez un jeu UWP simple avec DirectX</a></td>
    </tr>
    <tr>
        <td>Exemple d’effets d’image personnalisés de Direct2D</td>
        <td><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/D2DCustomEffects">D2DCustomEffects</a></td>
    </tr>
    <tr>
        <td>Exemple de maillage dégradé Direct2D</td>
        <td><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/D2DGradientMesh">D2DGradientMesh</a></td>
    </tr>
    <tr>
        <td>Exemple d’ajustement de photo Direct2D</td>
        <td><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/D2DPhotoAdjustment">D2DPhotoAdjustment</a></td>
    </tr>
    <tr>
        <td>Exemples publics Xbox Advanced Technology Group</td>
        <td><a href="https://github.com/Microsoft/Xbox-ATG-Samples">Xbox-ATG-Samples</a></td>
    </tr>
    <tr>
        <td>Exemples Xbox Live</td>
        <td><a href="https://github.com/Microsoft/xbox-live-samples">Xbox-Live-exemples</a></td>
    </tr>
    <tr>
        <td>Exemples de jeux Xbox One (XGD)</td>
        <td><a href="https://developer.microsoft.com/games/xbox/partner/development-education-samples">Exemples</a></td>
    </tr>
    <tr>
        <td>Exemples de jeux Windows (Galerie de code MSDN)</td>
        <td><a href="/samples/browse/?term=games">Exemples de jeux Microsoft Store</a></td>
    </tr>
    <tr>
        <td>Exemple de jeu JavaScript 2D</td>
        <td><a href="../get-started/get-started-tutorial-game-js2d.md">Créer un jeu UWP en JavaScript</a></td>
    </tr>
    <tr>
        <td>Exemple de jeu JavaScript 3D</td>
        <td><a href="../get-started/get-started-tutorial-game-js3d.md">Création d’un jeu JavaScript 3D à l’aide de three.js</a></td>
    </tr>
    <tr>
        <td>Exemple de jeu UWP multigame 2D</td>
        <td><a href="../get-started/get-started-tutorial-game-mg2d.md">Créer un jeu UWP dans MonoGame 2D</a></td>
    </tr>
</table>

### <a name="developer-forums"></a>Forums des développeurs

Les forums des développeurs sont l’endroit idéal pour poser des questions sur le développement de jeux et y répondre, et pour se connecter à la communauté de développement de jeux. Les forums peuvent également être des ressources fantastiques pour trouver des réponses à des problèmes difficiles que les développeurs ont rencontrés et résolus dans le passé.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Publication d’applications et de jeux forums pour développeurs</td>
        <td><a href="https://social.msdn.microsoft.com/Forums/en/home?forum=windowsstore%2Cwpsubmit%2Caiaads%2Caiasdk%2Caiamgr">Publication et ADS dans les applications</a></td>
    </tr>
    <tr>
        <td>Forum des développeurs d’applications UWP</td>
        <td><a href="/answers/topics/uwp.html">Développement d’applications de la plateforme Windows universelles</a></td>
    </tr>
    <tr>
        <td>Forums de développeurs d’applications de bureau</td>
        <td><a href="https://social.msdn.microsoft.com/Forums/en/home?forum=windowsgeneraldevelopmentissues">Forum dédié aux applications de bureau Windows</a></td>
    </tr>
    <tr>
        <td>Jeux DirectX Microsoft Store (publications de Forum archivés)</td>
        <td><a href="https://social.msdn.microsoft.com/Forums/en/home?forum=wingameswithdirectx">Création de jeux Microsoft Store avec DirectX (archivés)</a></td>
    </tr>
    <tr>
        <td>Forums de développeurs partenaires gérés Windows 10</td>
        <td><a href="https://forums.xboxlive.com/users/login.html">Forum des développeurs Xbox : Windows 10</a></td>
    </tr>
    <tr>
        <td>Forum Xbox Live</td>
        <td><a href="https://social.msdn.microsoft.com/Forums/en/home?forum=xboxlivedev">Forum de développement Xbox Live</a></td>
    </tr>
    <tr>
        <td>Forums PlayFab</td>
        <td><a href="https://community.playfab.com/index.html">Forums PlayFab</a></td>
    </tr>
</table>

### <a name="developer-blogs"></a>Blogs de développement

Les blogs de développement sont également une excellente ressource pour obtenir les dernières informations sur le développement de jeux. Vous trouverez des billets sur les nouvelles fonctionnalités, les détails de l’implémentation, les recommandations, l’arrière-plan de l’architecture, etc.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Blog de création d’applications pour Windows</td>
        <td><a href="https://blogs.windows.com/buildingapps/">Création d’applications pour Windows</a></td>
    </tr>
    <tr>
        <td>Windows 10 (billets de blog)</td>
        <td><a href="https://blogs.windows.com/blog/tag/windows-10/">Publications dans Windows 10</a></td>
    </tr>
    <tr>
        <td>Blog de l’équipe d’ingénierie de Visual Studio</td>
        <td><a href="https://devblogs.microsoft.com/visualstudio/">Le blog Visual Studio</a></td>
    </tr>
    <tr>
        <td>Blogs des outils de développement de Visual Studio</td>
        <td><a href="https://devblogs.microsoft.com/visualstudio/">Blogs Outils de développement</a></td>
    </tr>
    <tr>
        <td>Blog des outils de développement de Somasegar</td>
        <td><a href="https://devblogs.microsoft.com/somasegar/">Blog de Somasegar</a></td>
    </tr>
    <tr>
        <td>Blog DirectX pour les développeurs</td>
        <td><a href="https://devblogs.microsoft.com/directx/">Blog du développeur DirectX</a></td>
    </tr>
    <tr>
        <td>Présentation de DirectX 12 (billet de blog)</td>
        <td><a href="https://devblogs.microsoft.com/directx/directx-12/">DirectX 12</a></td>
    </tr>
    <tr>
        <td>Blog de l’équipe Visual C++</td>
        <td><a href="https://devblogs.microsoft.com/cppblog/">Blog de l’équipe Visual C++</a></td>
    </tr>
    <tr>
        <td>Blog de l’équipe PIX</td>
        <td><a href="https://devblogs.microsoft.com/pix/">Réglage des performances et débogage pour les jeux DirectX 12 sur Windows et Xbox</a></td>
    </tr>
    <tr>
        <td>Blog de l’équipe de déploiement d’applications Windows universelles</td>
        <td><a href="/windows/msix/">Créer et déployer un blog de l’équipe des applications UWP</a></td>
    </tr>
</table>

## <a name="concept-and-planning"></a>Concept et planification

Lors de l’étape de concept et de planification, vous décidez de l’apparence de votre jeu, et vous choisissez les technologies et les outils que vous allez utiliser pour lui donner vie.

### <a name="overview-of-game-development-technologies"></a>Vue d’ensemble des technologies de développement de jeux

Lorsque vous commencez à développer un jeu pour la plateforme UWP, plusieurs options sont à votre disposition pour les graphismes, les entrées, l’audio, le réseau, les utilitaires et les bibliothèques.

Si vous avez déjà choisi toutes les technologies que vous utiliserez dans votre jeu, félicitations ! Si tel n’est pas le cas, le guide [Technologies de jeu des applications pour la plateforme Windows universelle (UWP)](game-development-platform-guide.md) est un excellent aperçu de la plupart des technologies disponibles. Sa lecture est vivement conseillée pour vous aider à comprendre les options et leur articulation.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Recensement des technologies de jeu UWP</td>
        <td><a href="game-development-platform-guide.md">Technologies de jeu des applications UWP</a></td>
    </tr>
</table>

Ces trois vidéos du GDC 2015 constituent une bonne vue d’ensemble du développement de jeux Windows 10 et de l’expérience de jeu Windows 10.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Vue d’ensemble du développement de jeux Windows 10 (vidéo)</td>
        <td><a href="https://channel9.msdn.com/Events/GDC/GDC-2015/Developing-Games-for-Windows-10">Développement de jeux pour Windows 10</a></td>
    </tr>
    <tr>
        <td>Expérience de jeu Windows 10 (vidéo)</td>
        <td><a href="https://channel9.msdn.com/Events/GDC/GDC-2015/Gaming-Consumer-Experience-on-Windows-10">Expérience des consommateurs de jeux sur Windows 10</a></td>
    </tr>
    <tr>
        <td>Les jeux à travers l’écosystème Microsoft (vidéo)</td>
        <td><a href="https://channel9.msdn.com/Events/GDC/GDC-2015/The-Future-of-Gaming-Across-the-Microsoft-Ecosystem">L’avenir des jeux à travers l’écosystème Microsoft</a></td>
    </tr>
</table>

### <a name="game-planning"></a>Planification de jeux

Voici quelques concepts et questions d’ordre général à prendre en compte lors de la planification de votre jeu.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Rendre votre jeu accessible</td>
        <td><a href="/windows/uwp/gaming/accessibility-for-games">Accessibilité des jeux</a></td>
    </tr>
    <tr>
        <td>Créer des jeux à l’aide de Cloud</td>
        <td><a href="/windows/uwp/gaming/cloud-for-games">Cloud pour les jeux</a></td>
    </tr>
    <tr>
        <td>Monétisez votre jeu</td>
        <td><a href="/windows/uwp/gaming/monetization-for-games">Monétisation pour les jeux</a></td>
    </tr>
</table>

### <a name="choosing-your-graphics-technology-and-programming-language"></a>Choix de la technologie graphique et du langage de programmation

Plusieurs langages de programmation et technologies graphiques peuvent être utilisés dans les jeux Windows 10. Le chemin d’accès dépend du type de jeu que vous développez, de l’expérience et des préférences de votre studio de développement, ainsi que des exigences spécifiques de votre jeu. Allez-vous utiliser C#, C++ ou JavaScript ? DirectX, XAML ou HTML5 ?

#### <a name="directx"></a>DirectX

Microsoft DirectX représente le choix à faire pour obtenir des graphismes et des éléments multimédias 2D et 3D haute performances.

DirectX 12 est plus rapide et plus efficace que les versions précédentes. Direct3D 12 offre des scènes plus riches, des objets plus complexes, des effets plus complexes et une utilisation complète du matériel GPU moderne sur les PC Windows 10 et Xbox One.

Si vous souhaitez utiliser le pipeline graphique familier de Direct3D 11, vous bénéficierez toujours des nouvelles fonctionnalités d’optimisation et de rendu ajoutées à Direct3D 11,3. Et, si vous êtes un développeur d’API Windows de bureau qui a fait ses propres essais avec des racines dans Win32, vous avez toujours cette option dans Windows 10.

Les fonctionnalités complètes et la solide intégration à la plateforme de DirectX fournissent la puissance et les performances nécessaires aux jeux les plus exigeants.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>DirectX pour le développement UWP</td>
        <td><a href="directx-programming.md">Programmation DirectX</a></td>
    </tr>
    <tr>
        <td>Didacticiel : création d’un jeu DirectX UWP</td>
        <td><a href="tutorial--create-your-first-uwp-directx-game.md">Créez un jeu UWP simple avec DirectX</a></td>
    </tr>
    <tr>
        <td>Présentations et informations de référence sur DirectX</td>
        <td><a href="/windows/desktop/directx">Graphiques et jeux DirectX</a></td>
    </tr>
    <tr>
        <td>Direct3D 12 : Guide de programmation et informations de référence</td>
        <td><a href="/windows/desktop/direct3d12/direct3d-12-graphics">Graphiques Direct3D 12</a></td>
    </tr>
    <tr>
        <td>Vidéos de développement Graphics et DirectX 12 (YouTube)</td>
        <td><a href="https://www.youtube.com/channel/UCiaX2B8XiXR70jaN7NK-FpA">Microsoft DirectX 12 et Graphics Education</a></td>
    </tr>
</table>

#### <a name="xaml"></a>XAML

XAML est un langage d’interface utilisateur déclaratif convivial doté de fonctionnalités pratiques comme les animations, les tables de montage séquentiel, la liaison de données, le format SVG (Scalable Vector Graphics), le redimensionnement dynamique et les graphes de scène. XAML fonctionne parfaitement pour l’interface utilisateur, les menus, les sprites et les graphiques 2D des jeux. Pour simplifier la disposition de l’interface utilisateur, le langage XAML est compatible avec les outils de conception et de développement que sont Expression Blend et Microsoft Visual Studio. XAML est couramment utilisé avec C#, mais C++ est également un bon choix si c’est votre langage préféré ou si votre jeu a des exigences élevées en matière d’UC.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Vue d’ensemble de la plateforme XAML</td>
        <td><a href="/windows/uwp/xaml-platform/index">Plateforme XAML</a></td>
    </tr>
    <tr>
        <td>Interface utilisateur et contrôles XAML</td>
        <td><a href="/windows/uwp/design/basics/">Contrôles, dispositions et texte</a></td>
    </tr>
</table>

#### <a name="html-5"></a>HTML 5

Le langage HTML (HyperText Markup Language) est un langage de balisage d’interface utilisateur couramment utilisé pour les pages web, les applications et les clients enrichis. Les jeux Windows peuvent utiliser le langage HTML5 comme couche présentation complète avec les fonctionnalités habituelles du HTML, l’accès à la plateforme Universal Windows Platform (UWP) et la prise en charge de fonctionnalités web modernes comme AppCache, les traitements web, le canevas, le glisser-déplacer, la programmation asynchrone et le format SVG. En arrière-plan, le rendu HTML tire parti de la puissance de l’accélération matérielle de DirectX. Vous bénéficiez donc toujours de l’avantage des performances de DirectX sans écrire de code supplémentaire. HTML5 convient bien si vous maîtrisez le développement web, le portage d’un jeu web ou si vous souhaitez utiliser des couches de langage et de graphiques dont l’approche est plus simple que les autres choix. Le langage HTML5 est utilisé avec JavaScript, mais il peut être également appelé dans les composants créés en C# ou C++/CX.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Informations sur HTML5 et DOM</td>
        <td><a href="https://developer.mozilla.org/docs/Web">Informations de référence HTML et DOM</a></td>
    </tr>
    <tr>
        <td>Recommandation du W3C sur HTML5</td>
        <td><a href="https://www.w3.org/TR/html5/">HTML5</a></td>
    </tr>
</table>

#### <a name="combining-presentation-technologies"></a>Combinaison des technologies de présentation

L’infrastructure DXGI (DirectX Graphics Infrastructure) de Microsoft fournit interopérabilité et compatibilité entre plusieurs technologies graphiques. Pour des graphismes haute performance, vous pouvez allier XAML et DirectX, en utilisant XAML pour les menus et les autres éléments simples de l’interface utilisateur, et DirectX pour le rendu des scènes 2D et 3D complexes DXGI assure également la compatibilité entre Direct2D, Direct3D, DirectWrite, DirectCompute et Microsoft Media Foundation.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Guide de programmation et informations de référence sur DXGI</td>
        <td><a href="/windows/desktop/direct3ddxgi/dx-graphics-dxgi">DXGI</a></td>
    </tr>
    <tr>
        <td>Combinaison de DirectX et XAML</td>
        <td><a href="directx-and-xaml-interop.md">Technologie interop DirectX et XAML</a></td>
    </tr>
</table>

#### <a name="c"></a>C++

C++/CX est un langage haute performance à faible traitement, qui fournit une puissante combinaison de vitesse, compatibilité et accès aux plateformes. C++/CX facilite l’utilisation de l’ensemble des fonctionnalités de jeux remarquables de Windows 10, notamment DirectX et Xbox Live. Vous pouvez également réutiliser le code et les bibliothèques C++ existants. C++/CX crée du code natif rapide qui n’entraîne pas la surcharge de garbage collection. votre jeu peut donc avoir de bonnes performances et une faible consommation d’énergie, ce qui augmente la durée de vie de la batterie. Utilisez C++/CX avec DirectX ou XAML, ou bien créez un jeu utilisant une combinaison des deux.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Présentations et informations de référence sur C++/CX</td>
        <td><a href="/cpp/cppcx/visual-c-language-reference-c-cx">Informations de référence sur le langage Visual C++ (C++/CX)</a></td>
    </tr>
    <tr>
        <td>Visual C++ : Guide de programmation et informations de référence</td>
        <td><a href="/cpp/visual-cpp-in-visual-studio">Visual C++ dans Visual Studio 2019</a></td>
    </tr>
</table>

#### <a name="c"></a>C#

C# (prononcez « C sharp ») est un langage moderne et innovant, qui est simple, puissant, de type sécurisé et orienté objet. C# permet un développement rapide tout en conservant la familiarité et l’expressivité des langages du style C. Même s’il est facile à utiliser, C# possède de nombreuses fonctionnalités de langage avancées comme le polymorphisme, les délégués, les expressions lambda, les fermetures, la méthode Iterator, la covariance et les expressions LINQ (Language-Integrated Query). C# convient parfaitement si vous ciblez XAML, souhaitez commencer à développer rapidement votre jeu ou bénéficiez déjà d’une expérience en C#. C# est utilisé essentiellement avec XAML. Si vous voulez utiliser DirectX, choisissez plutôt C++ ou écrivez une partie de votre jeu en tant que composant C++ qui interagit avec DirectX. Pensez également à [Win2D](https://github.com/Microsoft/Win2D), une bibliothèque de graphismes Direct2D en mode immédiat pour C# et C++.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>C# : Guide de programmation et informations de référence</td>
        <td><a href="/dotnet/articles/csharp/csharp">Référence du langage C#</a></td>
    </tr>
</table>

#### <a name="javascript"></a>JavaScript

JavaScript est un langage de script dynamique largement utilisé pour les applications web modernes et les applications clientes enrichies.

Les applications Windows app en JavaScript peuvent accéder aux puissantes fonctionnalités de la plateforme Universal Windows Platform (UWP) d’une façon simple et intuitive, comme les méthodes et les propriétés des classes JavaScript orientées objet. JavaScript est un bon choix pour votre jeu si vous utilisez un environnement de développement Web, si vous êtes déjà familiarisé avec JavaScript, ou si vous souhaitez utiliser des bibliothèques HTML5, CSS, WinJS ou JavaScript. Si vous ciblez DirectX ou XAML, choisissez C# ou C++/CX à la place.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Informations de référence sur JavaScript et Windows Runtime</td>
        <td><a href="/scripting/javascript/javascript-language-reference">Référence JavaScript</a></td>
    </tr>
</table>

#### <a name="use-windows-runtime-components-to-combine-languages"></a>Utiliser des composants de Windows Runtime pour combiner des langues

Avec la plateforme Windows universelle, il est facile de combiner des composants écrits dans des langages différents. Créez Windows Runtime composants en C++, C# ou Visual Basic, puis appelez-les à partir de JavaScript, C#, C++ ou Visual Basic. C’est là une méthode remarquable pour programmer des parties de votre jeu dans le langage de votre choix. Les composants vous permettent également de consommer des bibliothèques externes qui ne sont disponibles que dans un langage particulier, ainsi que d’utiliser du code hérité que vous avez déjà écrit.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Comment créer des composants Windows Runtime</td>
        <td><a href="/windows/uwp/winrt-components/creating-windows-runtime-components-in-cpp">Composants Windows Runtime avec C++/CX</a></td>
    </tr>
</table>

### <a name="which-version-of-directx-should-your-game-use"></a>Quelle version de DirectX utiliser dans votre jeu ?

Si vous choisissez DirectX pour votre jeu, vous devez déterminer la version à utiliser : Microsoft Direct3D 12 ou Microsoft Direct3D 11.

DirectX 12 est plus rapide et plus efficace que les versions précédentes. Direct3D 12 offre des scènes plus riches, des objets plus complexes, des effets plus complexes et une utilisation complète du matériel GPU moderne sur les PC Windows 10 et Xbox One. Étant donné que Direct3D 12 fonctionne à un niveau très faible, il donne aux équipes de développement de graphiques expertes, ou aux équipes de développement de DirectX 11 expérimentées, les moyens de maximiser l’optimisation des graphiques.

Direct3D 11.3 est une API graphique de niveau faible, qui utilise le modèle de programmation Direct3D familier, et prend plus facilement en charge le processus complexe de rendu GPU. Elle est également prise en charge dans Windows 10 et Xbox One. Si vous disposez d’un moteur existant écrit en Direct3D 11 et que vous n’êtes pas encore prêt à effectuer la transition vers Direct3D 12, vous pouvez utiliser Direct3D 11 sur 12 pour obtenir certaines améliorations des performances. Les versions 11.3 et ultérieures contiennent également les nouvelles fonctionnalités de rendu et d’optimisation présentes dans Direct3D 12.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Choisir Direct3D 12 ou Direct3D 11</td>
        <td><a href="/windows/desktop/direct3d12/what-is-directx-12-">Qu’est-ce que Direct3D 12?</a></td>
    </tr>
    <tr>
        <td>Vue d’ensemble de Direct3D 11</td>
        <td><a href="/windows/desktop/direct3d11/atoc-dx-graphics-direct3d-11">Graphismes Direct3D 11</a></td>
    </tr>
    <tr>
        <td>Vue d’ensemble de Direct3D 11 sur 12</td>
        <td><a href="/windows/desktop/direct3d12/direct3d-11-on-12">Direct3D 11 sur 12</a></td>
    </tr>
</table>

### <a name="bridges-game-engines-and-middleware"></a>Ponts, moteurs de jeu et intergiciels

En fonction des besoins de votre jeu, l’utilisation de ponts, de moteurs de jeu ou d’intergiciels peut économiser du temps et des ressources de développement et de test. Voici une vue d’ensemble et des ressources pour les ponts, les moteurs de jeu et l’intergiciel (middleware).

#### <a name="universal-windows-platform-bridges"></a>Ponts de plateforme Windows universelle

Les ponts de plateforme Windows universelle sont des technologies qui amènent votre application ou votre jeu existant à la plateforme UWP. Ils sont un excellent moyen de démarrer rapidement le développement des jeux UWP.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Ponts UWP</td>
        <td><a href="https://developer.microsoft.com/windows/bridges">Importer votre code dans Windows</a></td>
    </tr>
    <tr>
        <td>Pont Windows pour iOS</td>
        <td><a href="https://developer.microsoft.com/windows/bridges/ios">Porter vos applications iOS vers Windows</a></td>
    </tr>
    <tr>
        <td>Pont Windows pour les applications de bureau (.NET et Win32)</td>
        <td><a href="https://developer.microsoft.com/windows/bridges/desktop">Convertir votre application de bureau en application UWP</a></td>
    </tr>
</table>

#### <a name="playfab"></a>PlayFab

PlayFab, qui fait désormais partie de la famille Microsoft, est une plateforme principale complète pour les jeux en direct et offre aux studios indépendants un moyen puissant pour se lancer. Stimulez les revenus, l’engagement et la durée de rétention, tout en réduisant les coûts, grâce aux services de jeu, l'analyse en temps réel et LiveOps.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>PlayFab</td>
        <td><a href="https://playfab.com/">Vue d’ensemble des outils et des services</a></td>
    </tr>
    <tr>
        <td>Mise en route</td>
        <td><a href="https://api.playfab.com/docs/general-getting-started">Guide de mise en route général</a></td>
    </tr>
    <tr>
        <td>Série de didacticiels vidéo</td>
        <td><a href="https://www.youtube.com/watch?v=fGNpiqVi5xU&list=PLHCfyL7JpoPbLpA_oh_T5PKrfzPgCpPT5">Série de vidéos de démonstration sur les systèmes de base de PlayFab</a></td>
    </tr>
    <tr>
        <td>Recettes</td>
        <td><a href="https://api.playfab.com/docs/tutorials/recipes-index">Exemples de mécanismes de conception et de modèle de conception populaires</a></td>
    </tr>
    <tr>
        <td>Plateformes</td>
        <td><a href="https://api.playfab.com/platforms">Documentation spécifique pour les différentes plateformes et les moteurs de jeux</a></td>
    </tr>
    <tr>
        <td>Référentiel GitHub</td>
        <td><a href="https://github.com/PlayFab">Obtenir des scripts et des kits de développement logiciel (SDK) pour différentes plateformes, notamment Android, iOS, Windows, Unity et inreal.</a></td>
    </tr>
    <tr>
        <td>Documentation de l’API</td>
        <td><a href="https://api.playfab.com/documentation/">Accéder au service PlayFab directement via des API Web REST</a></td>
    </tr>
    <tr>
        <td>Forums</td>
        <td><a href="https://community.playfab.com/index.html">Forums PlayFab</a></td>
    </tr>
</table>

#### <a name="unity"></a>Unity

Unity offre une plateforme permettant de créer des applications et des jeux 2D, 3D, VR et AR. Elle vous permet de réaliser rapidement votre vision créative et de fournir votre contenu à pratiquement n’importe quel support ou appareil.

À partir de Unity 5,4, Unity prend en charge le développement Direct3D 12.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Moteur de jeu Unity</td>
        <td><a href="https://unity.com/">Unity : Moteur de jeu</a></td>
    </tr>
    <tr>
        <td>Obtenir Unity</td>
        <td><a href="https://store.unity.com/">Obtenir Unity</a></td>
    </tr>
    <tr>
        <td>Documentation Unity pour Windows</td>
        <td><a href="https://docs.unity3d.com/Manual/Windows.html">Manuel Unity/Windows</a></td>
    </tr>
    <tr>
        <td>Ajouter LiveOps à l’aide de PlayFab</td>
        <td><a href="https://api.playfab.com/docs/getting-started/unity-getting-started">Prise en main : effectuez votre premier appel d’API PlayFab à partir de votre jeu Unity</a></td>
    </tr>
    <tr>
        <td>Comment ajouter une interactivité à votre jeu à l’aide de la fenêtre interactive mixer</td>
        <td><a href="https://github.com/mixer/interactive-unity-plugin/wiki/Getting-started">Guide de mise en route</a></td>
    </tr>
    <tr>
        <td>Kit de développement logiciel (SDK) mixer pour Unity</td>
        <td><a href="https://www.assetstore.unity3d.com/en/#!/content/88585">Plug-in Unity mixer</a></td>
    </tr>
    <tr>
        <td>Documentation de référence du kit de développement logiciel de mixage pour Unity</td>
        <td><a href="https://dev.mixer.com/reference/interactive/csharp/index.html">Informations de référence sur l’API pour le plug-in Unity mixer</a></td>
    </tr>
    <tr>
        <td>Publiez votre jeu Unity sur Microsoft Store</td>
        <td><a href="https://unity3d.com/partners/microsoft/porting-guides">Guide sur le portage</a></td>
    </tr>
    <tr>
        <td>Dépannage des références d’assembly manquantes relatives aux API .NET</td>
        <td><a href="/windows/uwp/gaming/missing-dot-net-apis-in-unity-and-uwp">API .NET manquantes dans Unity et UWP</a></td>
    </tr>
    <tr>
        <td>Publier votre jeu Unity en tant qu’application Windows universelle (vidéo)</td>
        <td><a href="https://channel9.msdn.com/Blogs/One-Dev-Minute/How-to-publish-your-Unity-game-as-a-UWP-app">Comment publier votre jeu Unity en tant qu’application UWP</a></td>
    </tr>
    <tr>
        <td>Utiliser Unity pour créer des jeux et applications Windows (vidéo)</td>
        <td><a href="https://channel9.msdn.com/Blogs/One-Dev-Minute/Making-games-and-apps-with-Unity">Création de jeux et applications Windows avec Unity</a></td>
    </tr>
    <tr>
        <td>Développement de jeux Unity à l’aide de Visual Studio (série de vidéos)</td>
        <td><a href="https://www.youtube.com/playlist?list=PLReL099Y5nRfseAg0k1SJOlpqdcsDs8Em">Utilisation d’Unity avec Visual Studio 2015</a></td>
    </tr>
</table>

#### <a name="havok"></a>Havok

La suite modulaire d’outils et de technologies Havok aide les créateurs de jeux à atteindre de nouveaux niveaux d’interactivité et d’immersion. Havok permet de fournir des données physiques réalistes, et de réaliser des simulations interactives ainsi que des animations remarquables. La version 2015,1 et les versions ultérieures prennent officiellement en charge UWP dans Visual Studio 2015 sur x86, 64 bits et ARM.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Site web Havok</td>
        <td><a href="https://www.havok.com/">Havok</a></td>
    </tr>
    <tr>
        <td>Suite d’outils Havok</td>
        <td><a href="https://www.havok.com/products/">Vue d’ensemble des produits Havok</a></td>
    </tr>
    <tr>
        <td>Forums de support Havok</td>
        <td><a href="https://www.havok.com/">Havok</a></td>
    </tr>
</table>

#### <a name="monogame"></a>MonoGame

MonoGame est une infrastructure de développement open source inter-plateforme initialement basée sur Microsoft XNA Framework 4.0. Monogame prend actuellement en charge Windows, Windows Phone, Xbox, ainsi que Linux, Mac OS, iOS, Android et certaines autres plateformes.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>MonoGame</td>
        <td><a href="https://www.monogame.net">Site web de MonoGame</a></td>
    </tr>
    <tr>
        <td>Documentation MonoGame</td>
        <td><a href="https://www.monogame.net/documentation/">Documentation MonoGame (version la plus récente)</a></td>
    </tr>
    <tr>
        <td>Téléchargements MonoGame</td>
        <td><a href="https://www.monogame.net/downloads/">Téléchargez des versions, des builds de développement et du code source</a> sur le site web de MonoGame, ou <a href="https://www.nuget.org/profiles/MonoGame">obtenez la version la plus récente via NuGet</a>.
    </tr>
    <tr>
        <td>Exemple de jeu UWP multigame 2D</td>
        <td><a href="../get-started/get-started-tutorial-game-mg2d.md">Créer un jeu UWP dans MonoGame 2D</a></td>
    </tr>
</table>

#### <a name="cocos2d"></a>Cocos2d

Cocos2d-x est un moteur et des outils de développement de jeux Open source multiplateforme qui prend en charge la création de jeux UWP. Depuis la version 3, des fonctionnalités 3D sont également ajoutées.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Cocos2d-x</td>
        <td><a href="https://www.cocos2d-x.org/">Qu’est-ce que cocos2d-x ?</a></td>
    </tr>
    <tr>
        <td>Guide du programmeur Cocos2d-x</td>
        <td><a href="https://www.cocos2d-x.org/programmersguide/">Guide des programmeurs cocos2d-x</a></td>
    </tr>
    <tr>
        <td>Cocos2d-x sur Windows 10 (billet de blog)</td>
        <td><a href="https://blogs.windows.com/buildingapps/2015/06/15/running-cocos2d-x-on-windows-10/">Exécution de Cocos2d-x sur Windows 10</a></td>
    </tr>
    <tr>
        <td>Ajouter LiveOps à l’aide de PlayFab</td>
        <td><a href="https://api.playfab.com/docs/getting-started/cocos2d-x-getting-started-guide">Prise en main : effectuez votre premier appel d’API PlayFab à partir de votre jeu cocos2d</a></td>
    </tr>
</table>

#### <a name="unreal-engine"></a>Unreal Engine

Unreal Engine 4 est une suite complète d’outils de développement de jeux destinée à tous les types de jeu et de développement. Destiné aux jeux pour consoles et PC très exigeants, Unreal Engine est utilisé par les développeurs de jeux du monde entier.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Vue d’ensemble d’Unreal Engine</td>
        <td><a href="https://www.unrealengine.com/what-is-unreal-engine-4">Unreal Engine 4</a></td>
    </tr>
    <tr>
        <td>Ajouter LiveOps à l’aide de PlayFab-C++</td>
        <td><a href="https://api.playfab.com/docs/getting-started/unreal-cpp-getting-started">Prise en main : effectuez votre premier appel d’API PlayFab à partir de votre jeu inréel</a></td>
    </tr>
    <tr>
        <td>Ajouter LiveOps à l’aide de PlayFab-Blueprints</td>
        <td><a href="https://api.playfab.com/docs/getting-started/unreal-blueprints-getting-started">Prise en main : effectuez votre premier appel d’API PlayFab à partir de votre jeu inréel</a></td>
    </tr>
</table>

#### <a name="babylonjs"></a>BabylonJS

BabylonJS est une infrastructure JavaScript complète pour la création de jeux en 3D avec HTML5, WebGL, WebVR et l’audio Web.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>BabylonJS</td>
        <td><a href="https://www.babylonjs.com/">BabylonJS</a></td>
    </tr>
    <tr>
        <td>WebGL 3D avec HTML5 et BabylonJS (série de vidéos)</td>
        <td><a href="https://channel9.msdn.com/Series/Introduction-to-WebGL-3D-with-HTML5-and-Babylonjs/01">Découverte de 3D WebGL et BabylonJS</a></td>
    </tr>
    <tr>
        <td>Création d’un jeu WebGL multiplateforme avec BabylonJS</td>
        <td><a href="https://www.smashingmagazine.com/2016/07/babylon-js-building-sponza-a-cross-platform-webgl-game/">Utiliser BabylonJS pour développer un jeu multiplateforme</a></td>
    </tr>
</table>

### <a name="porting-your-game"></a>Portage du jeu

Si vous disposez d’un jeu, nombre de ressources et de guides disponibles vous permettent de l’importer rapidement dans la plateforme UWP. Pour vous lancer rapidement dans le portage, vous pouvez également penser à utiliser un [pont de plateforme Windows universelle (UWP)](#universal-windows-platform-bridges).

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Portage d’une application Windows 8 vers une application de plateforme Windows universelle</td>
        <td><a href="/windows/uwp/porting/w8x-to-uwp-root">Passer de Windows Runtime 8. x à UWP</a></td>
    </tr>
    <tr>
        <td>Portage d’une application Windows 8 vers une application de plateforme Windows universelle (vidéo)</td>
        <td><a href="https://channel9.msdn.com/Series/A-Developers-Guide-to-Windows-10/21">Portage d’applications Windows 8.1 vers Windows 10</a></td>
    </tr>
    <tr>
        <td>Portage d’une application iOS vers une application de plateforme Windows universelle (vidéo)</td>
        <td><a href="/windows/uwp/porting/ios-to-uwp-root">Passer d’iOS à UWP</a></td>
    </tr>
    <tr>
        <td>Portage d’une application Silverlight vers une application de plateforme Windows universelle</td>
        <td><a href="/windows/uwp/porting/wpsl-to-uwp-root">Passer de Windows Phone Silverlight à UWP</a></td>
    </tr>
    <tr>
        <td>Portage d’une application Silverlight ou XAML vers une application de plateforme Windows universelle (vidéo)</td>
        <td><a href="https://channel9.msdn.com/Events/Build/2015/3-741">Portage d’une application XAML ou Silverlight vers Windows 10</a></td>
    </tr>
    <tr>
        <td>Portage d’une application Xbox vers une application de plateforme Windows universelle</td>
        <td><a href="https://developer.xboxlive.com/platform/development/education/Documents/Porting%20from%20Xbox%20One%20to%20Windows%2010.aspx">Portage de Xbox One vers Windows 10 UWP</a></td>
    </tr>
    <tr>
        <td>Portage de DirectX 9 vers DirectX 11</td>
        <td><a href="porting-your-directx-9-game-to-windows-store.md">Porter de DirectX 9 vers la plateforme Windows universelle (UWP)</a></td>
    </tr>
    <tr>
        <td>Portage de Direct3D 11 vers Direct3D 12</td>
        <td><a href="/windows/desktop/direct3d12/porting-from-direct3d-11-to-direct3d-12">Portage de Direct3D 11 sur Direct3D 12</a></td>
    </tr>
    <tr>
        <td>Portage d’OpenGL ES vers Direct3D 11</td>
        <td><a href="port-from-opengl-es-2-0-to-directx-11-1.md">Portage d’OpenGL ES 2.0 sur Direct3D 11</a></td>
    </tr>
    <tr>
        <td>Passer d’OpenGL ES à Direct3D 11 en utilisant ANGLE</td>
        <td><a href="https://github.com/microsoft/angle/wiki">ANGLE</a></td>
    </tr>
    <tr>
        <td>Équivalents des API Windows classiques dans UWP</td>
        <td><a href="/uwp/win32-and-com/win32-and-com-for-uwp-apps">Alternatives aux API Windows dans les applications de plateforme Windows universelle (UWP)</a></td>
    </tr>
</table>

## <a name="prototype-and-design"></a>Prototype et conception

Maintenant que vous avez choisi le type de jeu à créer et les outils et la technologie graphique que vous allez utiliser pour ce faire, vous êtes prêt à passer à sa conception et à la création de son prototype. Comme le cœur de votre jeu est une application deplateforme Windows universelle, c’est par là que vous allez commencer.

### <a name="introduction-to-the-universal-windows-platform-uwp"></a>Présentation de la plateforme Windows universelle (UWP)

Windows 10 introduit la plateforme Windows universelle (UWP), qui fournit une plateforme des API courantes des appareils Windows 10. UWP évolue et développe le modèle Windows Runtime pour le perfectionner et le transformer en noyau cohérent et unifié. Les jeux qui ciblent la plateforme UWP peuvent appeler les API WinRT qui sont communes à tous les appareils. Étant donné que la plateforme UWP fournit des couches d’API garanties, vous pouvez choisir de créer un package d’application unique qui sera installé sur les appareils Windows 10. Et si vous le souhaitez, votre jeu peut toujours appeler les API (y compris des API Windows classiques de Win32 et .NET) propres aux appareils sur lesquels votre jeu s’exécute.

Les guides indiqués ci-dessous sont excellents. Ils décrivent en détail les applications de plateforme Windows universelle, et il est vivement recommandé de les lire pour mieux comprendre ce qu’est cette plateforme.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Présentation des applications de plateforme Windows universelle</td>
        <td><a href="/windows/uwp/get-started/whats-a-uwp">Qu’est-ce qu’une application de plateforme universelle Windows ?</a></td>
    </tr>
    <tr>
        <td>Vue d’ensemble de la plateforme UWP</td>
        <td><a href="/windows/uwp/get-started/universal-application-platform-guide">Guide des applications UWP</a></td>
    </tr>
</table>

### <a name="getting-started-with-uwp-development"></a>Prise en main du développement UWP

La préparation au développement d’une application Windows universelle est rapide et facile. Les guides suivants vous décrivent le processus étape par étape.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Prise en main du développement UWP</td>
        <td><a href="https://developer.microsoft.com/windows/apps/getstarted">Prise en main des applications Windows</a></td>
    </tr>
    <tr>
        <td>Préparation au développement UWP</td>
        <td><a href="/windows/uwp/get-started/get-set-up">Se préparer</a></td>
    </tr>
</table>

Si vous ne connaissez pas du tout la programmation UWP et que vous envisagez d’utiliser XAML dans votre jeu (voir [Choix de la technologie graphique et du langage de programmation](#choosing-your-graphics-technology-and-programming-language)), la série de vidéos [Développement sur Windows 10 pour les néophytes](https://channel9.msdn.com/Series/Windows-10-development-for-absolute-beginners) est idéale pour commencer.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Guide des débutants pour le développement pour Windows 10 avec le langage XAML (série de vidéos)</td>
        <td><a href="https://channel9.msdn.com/Series/Windows-10-development-for-absolute-beginners">Développement Windows 10 pour les débutants</a></td>
    </tr>
    <tr>
        <td>Annonce de la série sur Windows 10 pour néophytes utilisant XAML (billet de blog)</td>
        <td><a href="https://blogs.windows.com/buildingapps/2015/09/30/windows-10-development-for-absolute-beginners/">Développement Windows 10 pour les débutants</a></td>
    </tr>
</table>

### <a name="uwp-development-concepts"></a>Concepts de développement UWP

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Vue d’ensemble du développement d’une application de plateforme universelle Windows</td>
        <td><a href="https://developer.microsoft.com/windows/apps/develop">Développer des applications Windows</a></td>
    </tr>
    <tr>
        <td>Vue d’ensemble de la programmation réseau dans UWP</td>
        <td><a href="/windows/uwp/networking/index">Mise en réseau et services web</a></td>
    </tr>
    <tr>
        <td>Utilisation de Windows.Web.HTTP et Windows.Networking.Sockets dans les jeux</td>
        <td><a href="work-with-networking-in-your-directx-game.md">Mise en réseau pour les jeux</a></td>
    </tr>
    <tr>
        <td>Concepts de programmation asynchrone dans UWP</td>
        <td><a href="/windows/uwp/threading-async/asynchronous-programming-universal-windows-platform-apps">Programmation asynchrone</a></td>
    </tr>
</table>

### <a name="windows-desktop-apisto-uwp"></a>API de bureau Windows pour UWP

Voici quelques liens pour vous aider à migrer votre jeu de bureau Windows vers UWP.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Utiliser du code C++ existant pour le développement de jeux UWP</td>
        <td><a href="/cpp/porting/how-to-use-existing-cpp-code-in-a-universal-windows-platform-app">Comment : utiliser le code C++ existant dans une application UWP</a></td>
    </tr>
    <tr>
        <td>API Windows Runtime pour les API Win32 et COM</td>
        <td><a href="/uwp/win32-and-com/win32-and-com-for-uwp-apps">API Win32 et COM pour les applications UWP</a></td>
    </tr>
    <tr>
        <td>Fonctions CRT non prises en charge dans UWP</td>
        <td><a href="/cpp/cppcx/crt-functions-not-supported-in-universal-windows-platform-apps">Fonctions CRT non prises en charge dans les applications de la plateforme Windows universelle</a></td>
    </tr>
    <tr>
        <td>Alternatives pour les API Windows</td>
        <td><a href="/uwp/win32-and-com/alternatives-to-windows-apis-uwp">Alternatives aux API Windows dans les applications de plateforme Windows universelle (UWP)</a></td>
    </tr>
</table>

### <a name="process-lifetime-management"></a>Gestion de la durée de vie des processus

La gestion de la durée de vie des processus, ou cycle de vie des applications, décrit les différents états d’activation que peut traverser une application de plateforme Windows universelle. Votre jeu peut être activé, suspendu, rétabli ou arrêté, et il peut transiter par ces états de plusieurs manières.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Gestion des transitions du cycle de vie des applications</td>
        <td><a href="/windows/uwp/launch-resume/app-lifecycle">Cycle de vie de l’application</a></td>
    </tr>
    <tr>
        <td>Utilisation de Microsoft Visual Studio pour déclencher des transitions d’application</td>
        <td><a href="/visualstudio/debugger/how-to-trigger-suspend-resume-and-background-events-for-windows-store-apps-in-visual-studio">Comment déclencher des événements de suspension, de reprise et d’arrière-plan pour les applications UWP dans Visual Studio</a></td>
    </tr>
</table>

### <a name="designing-game-ux"></a>Conception de l’expérience utilisateur de jeux

Une conception inspirée est à la source d’un jeu réussi.

Les jeux partagent certains éléments d’interface utilisateur et des principes de conception communs avec les applications, mais ils ont souvent une apparence et un objectif de conception uniques pour leur expérience utilisateur. Les jeux rencontreront le succès si les aspects suivants sont bien pensés : quand votre jeu doit-il utiliser une expérience utilisateur testée et quand doit-il varier et innover ? La technologie de présentation que vous choisissez pour votre jeu (DirectX, XAML, HTML5 ou une combinaison de celles-ci) peut influencer les détails d’implémentation, mais les principes de conception que vous appliquez ne reposent pas sur ce choix.

Distincte de la conception de l’expérience utilisateur, la conception d’un jeu, par exemple la conception du niveau, le rythme et bien d’autres aspects sont une forme d’art en soi. Elle est de votre ressort, votre équipe et vous, et elle n’est pas traitée dans ce guide de développement.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Informations de base et recommandations sur la conception UWP</td>
        <td><a href="https://developer.microsoft.com/windows/apps/design">Conception d’applications UWP</a></td>
    </tr>
    <tr>
        <td>Conception des états de cycle de vie d’application</td>
        <td><a href="/windows/uwp/launch-resume/index">Recommandations en matière d’expérience utilisateur pour le lancement, la suspension et la reprise</a></td>
    </tr>
    <tr>
        <td>Concevoir votre application UWP pour les écrans Xbox One et Television</td>
        <td><a href="/windows/uwp/design/devices/designing-for-tv">Conception pour Xbox et TV</a></td>
    </tr>
    <tr>
        <td>Ciblage de plusieurs facteurs de forme d’appareil (vidéo)</td>
        <td><a href="https://channel9.msdn.com/Events/GDC/GDC-2015/Designing-Games-for-a-Windows-Core-World">Conception de jeux pour Windows Core</a></td>
    </tr>
</table>

#### <a name="color-guideline-and-palette"></a>Recommandations de couleur et palette

Le respect de recommandations de couleur cohérentes dans votre jeu lui apporte esthétisme, simplifie la navigation et permet d’informer le joueur sur la fonctionnalité du menu et de l’affichage à tête haute. L’application de couleurs cohérentes aux éléments du jeu comme les avertissements, dommages, XP et scores peut permettre d’obtenir une interface utilisateur plus claire et de réduire l’emploi de libellés explicites.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Guide des couleurs</td>
        <td><a href="https://assets.windowsphone.com/499cd2be-64ed-4b05-a4f5-cd0c9ad3f6a3/101_BestPractices_Color_InvariantCulture_Default.zip">Meilleures pratiques : Couleur</a></td>
    </tr>
</table>

#### <a name="typography"></a>Typographie

L’utilisation appropriée de la typographie améliore de nombreux aspects de votre jeu, notamment la disposition de l’interface utilisateur, la navigation, la lisibilité, l’ambiance, la marque et l’immersion du joueur.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Guide de la typographie</td>
        <td><a href="https://cmsresources.windowsphone.com/devcenter/common/resources/content/101_BestPractices_Typography.pdf">Meilleures pratiques : Typographie</a></td>
    </tr>
</table>

#### <a name="ui-map"></a>Carte d’interface utilisateur

Une carte d’interface utilisateur est une disposition de la navigation et des menus du jeu présentée sous la forme d’un organigramme. Le mappage d’IU aide toutes les parties prenantes impliquées à comprendre l’interface et les chemins de navigation du jeu, et peut exposer les obstacles potentiels et les fins mortes au début du cycle de développement.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Guide sur la carte d’interface utilisateur</td>
        <td><a href="https://cmsresources.windowsphone.com/devcenter/common/resources/content/101_BestPractices_UI_Map.pdf">Meilleures pratiques : Carte d’interface utilisateur</a></td>
    </tr>
</table>

### <a name="game-audio"></a>Audio du jeu

Guides et références pour l’implémentation de l’audio dans les jeux à l’aide de XAudio2, XAPO et Windows Sonic. XAudio2 est une API audio de bas niveau qui fournit un traitement de signal et une base de mixage pour le développement de moteurs audio hautes performances. L’API XAPO permet la création d’objets de traitement audio multiplateforme (XAPO) pour une utilisation dans XAudio2 à la fois sur Windows et sur Xbox. La prise en charge audio Windows Sonic vous permet d’ajouter Dolby Atmos pour Home Theater, Dolby Atmos pour casque et la prise en charge de Windows HRTF à votre application de jeu ou de diffusion multimédia en continu.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>API XAudio2</td>
        <td><a href="/windows/desktop/xaudio2/xaudio2-apis-portal">Guide de programmation et informations de référence sur les API pour XAudio2</a></td>
    </tr>
    <tr>
        <td>Créer des objets de traitement audio multiplateforme</td>
        <td><a href="/windows/desktop/xaudio2/xapo-overview">Présentation de XAPO</a></td>
    </tr>
    <tr>
        <td>Présentation des concepts audio</td>
        <td><a href="working-with-audio-in-your-directx-game.md">Audio pour les jeux</a></td>
    </tr>
    <tr>
        <td>Vue d’ensemble de Windows Sonic</td>
        <td><a href="/windows/desktop/CoreAudio/spatial-sound">Son spatial</a></td>
    </tr>
    <tr>
        <td>Exemples de son spatial Windows Sonic</td>
        <td><a href="https://github.com/Microsoft/Xbox-ATG-Samples/tree/master/UWPSamples/Audio">Exemples audio de groupe de technologies avancées Xbox</a></td>
    </tr>
    <tr>
        <td>Découvrez comment intégrer Windows Sonic à vos jeux (vidéo)</td>
        <td><a href="https://channel9.msdn.com/Events/GDC/GDC-2017/GDC2017-002">Présentation des fonctionnalités audio spatiales pour Xbox et Windows</a></td>
    </tr>
</table>

### <a name="directx-development"></a>Développement DirectX

Guides et références pour le développement de jeux DirectX.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>DirectX pour le développement UWP</td>
        <td><a href="directx-programming.md">Programmation DirectX</a></td>
    </tr>
    <tr>
        <td>Didacticiel : création d’un jeu DirectX UWP</td>
        <td><a href="tutorial--create-your-first-uwp-directx-game.md">Créez un jeu UWP simple avec DirectX</a></td>
    </tr>
    <tr>
        <td>Interaction de DirectX avec le modèle d’application UWP</td>
        <td><a href="about-the-uwp-user-interface-and-directx.md">Objet application et DirectX</a></td>
    </tr>
    <tr>
        <td>Vidéos de développement Graphics et DirectX 12 (YouTube)</td>
        <td><a href="https://www.youtube.com/channel/UCiaX2B8XiXR70jaN7NK-FpA">Microsoft DirectX 12 et Graphics Education</a></td>
    </tr>
    <tr>
        <td>Présentations et informations de référence sur DirectX</td>
        <td><a href="/windows/desktop/directx">Graphiques et jeux DirectX</a></td>
    </tr>
    <tr>
        <td>Direct3D 12 : Guide de programmation et informations de référence</td>
        <td><a href="/windows/desktop/direct3d12/direct3d-12-graphics">Graphiques Direct3D 12</a></td>
    </tr>
    <tr>
        <td>Notions fondamentales sur DirectX 12 (vidéo)</td>
        <td><a href="https://channel9.msdn.com/Events/GDC/GDC-2015/Better-Power-Better-Performance-Your-Game-on-DirectX12">Une meilleure alimentation, de meilleures performances : Votre jeu sur DirectX 12</a></td>
    </tr>
</table>

#### <a name="learning-direct3d-12"></a>Prise en main de Direct3D 12

Découvrez ce qui a changé dans Direct3D 12 et comment commencer à programmer à l’aide de Direct3D 12.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Configurer l’environnement de programmation</td>
        <td><a href="/windows/desktop/direct3d12/directx-12-programming-environment-set-up">Configuration de l’environnement de programmation Direct3D 12</a></td>
    </tr>
    <tr>
        <td>Comment créer un composant de base</td>
        <td><a href="/windows/desktop/direct3d12/creating-a-basic-direct3d-12-component">Création d’un composant Direct3D 12 de base</a></td>
    </tr>
    <tr>
        <td>Modifications apportées dans Direct3D 12</td>
        <td><a href="/windows/desktop/direct3d12/important-changes-from-directx-11-to-directx-12">Modifications importantes lors de la migration de Direct3D 11 à Direct3D 12</a></td>
    </tr>
    <tr>
        <td>Comment effectuer le portage de Direct3D 11 vers Direct3D 12</td>
        <td><a href="/windows/desktop/direct3d12/porting-from-direct3d-11-to-direct3d-12">Portage de Direct3D 11 sur Direct3D 12</a></td>
    </tr>
    <tr>
        <td>Concepts de liaison de ressource (descripteur de recouvrement, tableau de descripteur, tas de descripteur et signature racine) </td>
        <td><a href="/windows/desktop/direct3d12/resource-binding">Liaison de ressource dans Direct3D 12</a></td>
    </tr>
    <tr>
        <td>Gestion de la mémoire</td>
        <td><a href="/windows/desktop/direct3d12/memory-management">Gestion de la mémoire dans Direct3D 12</a></td>
    </tr>
</table>

#### <a name="directx-tool-kit-and-libraries"></a>Kit de ressources et bibliothèques DirectX

Le kit de ressources DirectX, la bibliothèque de traitement des textures DirectX, la bibliothèque de traitement des géométries DirectXMesh, la bibliothèque UVAtlas et la bibliothèque DirectXMath fournissent des fonctionnalités de texture, maillage, sprite etc., ainsi que des classes d’assistance pour le développement avec DirectX. Ces bibliothèques peuvent vous faire gagner du temps et de l’énergie lors du développement.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Obtenir le kit de ressources DirectX pour DirectX 11</td>
        <td><a href="https://github.com/Microsoft/DirectXTK">DirectXTK</a></td>
    </tr>
    <tr>
        <td>Obtenir le kit de ressources DirectX pour DirectX 12</td>
        <td><a href="https://github.com/Microsoft/DirectXTK12">DirectXTK 12</a></td>
    </tr>
    <tr>
        <td>Obtenir la bibliothèque de traitement des textures DirectX</td>
        <td><a href="https://github.com/Microsoft/DirectXTex">DirectXTex</a></td>
    </tr>
    <tr>
        <td>Obtenir la bibliothèque de traitement des géométries DirectXMesh</td>
        <td><a href="https://github.com/Microsoft/DirectXMesh">DirectXMesh</a></td>
    </tr>
    <tr>
        <td>Obtenir UVAtlas pour la création et la compression d’atlas de textures isochart</td>
        <td><a href="https://github.com/Microsoft/UVAtlas">UVAtlas</a></td>
    </tr>
    <tr>
        <td>Obtenir la bibliothèque DirectXMath</td>
        <td><a href="https://github.com/Microsoft/DirectXMath">DirectXMath</a></td>
    </tr>
    <tr>
        <td>Prise en charge de Direct3D 12 dans DirectXTK (billet de blog)</td>
        <td><a href="https://github.com/Microsoft/DirectXTK/issues/2">Prise en charge de DirectX 12</a></td>
    </tr>
</table>

#### <a name="directx-resources-from-partners"></a>Ressources DirectX provenant de partenaires

Voici des documentations supplémentaires sur DirectX, créées par des partenaires externes.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Nvidia: DX12 Do’s and Don’ts (billet de blog en anglais) </td>
        <td><a href="https://developer.nvidia.com/dx12-dos-and-donts-updated">DirectX 12 sur des GPU Nvidia</a></td>
    </tr>
    <tr>
        <td>Intel: Efficient rendering with DirectX 12 (en anglais)</td>
        <td><a href="https://software.intel.com/sites/default/files/managed/4a/38/Efficient-Rendering-with-DirectX-12-on-Intel-Graphics.pdf">Rendu de DirectX 12 sur graphiques Intel</a></td>
    </tr>
    <tr>
        <td>Intel: Multi adapter support in DirectX 12 (en anglais)</td>
        <td><a href="https://software.intel.com/articles/multi-adapter-support-in-directx-12">Implémentation d’une application explicite comportant plusieurs adaptateurs à l’aide de DirectX 12</a></td>
    </tr>
    <tr>
        <td>Intel: DirectX 12 tutorial (en anglais)</td>
        <td><a href="https://software.intel.com/articles/tutorial-migrating-your-apps-to-directx-12-part-1">Livre blanc collaboratif, élaboré par Intel, Suzhou Snail et Microsoft</a></td>
    </tr>
</table>

## <a name="production"></a>Production

À présent, votre studio est totalement engagé dans le cycle de production, des tâches étant distribuées à tous les membres de votre équipe. Vous peaufinez, refactorisez et étendez le prototype pour en faire un jeu complet.

### <a name="notifications-and-live-tiles"></a>Notifications et vignettes dynamiques

Une vignette est la représentation de votre jeu dans le menu Démarrer. Les vignettes et les notifications peuvent susciter l’intérêt des joueurs même s’ils n’utilisent pas votre jeu.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Développement de vignettes et de badges</td>
        <td><a href="/windows/uwp/controls-and-patterns/tiles-badges-notifications">Vignettes, badges et notifications</a></td>
    </tr>
    <tr>
        <td>Exemple illustrant les vignettes dynamiques et les notifications</td>
        <td><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Notifications">Exemples de notification</a></td>
    </tr>
    <tr>
        <td>Modèles de vignette adaptative (billet de blog)</td>
        <td><a href="/archive/blogs/tiles_and_toasts/adaptive-tile-templates-schema-and-documentation">Modèles de vignette adaptative : Schéma et documentation</a></td>
    </tr>
    <tr>
        <td>Conception de vignettes et de badges</td>
        <td><a href="/windows/uwp/controls-and-patterns/tiles-and-notifications-creating-tiles">Recommandations en matière de vignettes et de badges</a></td>
    </tr>
    <tr>
        <td>Application Windows 10 pour le développement interactif des modèles de vignette dynamique</td>
        <td><a href="https://www.microsoft.com/store/apps/9nblggh5xsl1">Notifications Visualizer</a></td>
    </tr>
    <tr>
        <td>Extension UWP Tile Generator pour Visual Studio</td>
        <td><a href="https://marketplace.visualstudio.com/items?itemName=shenchauhan.UWPTileGenerator">Outil permettant de créer toutes les vignettes requises à l’aide d’une image unique</a></td>
    </tr>
    <tr>
        <td>Extension UWP Tile Generator pour Visual Studio (billet de blog)</td>
        <td><a href="https://blogs.windows.com/buildingapps/2016/02/15/uwp-tile-generator-extension-for-visual-studio/">Conseils sur l’utilisation de l’outil UWP Tile Generator</a></td>
    </tr>
</table>

### <a name="enable-in-app-product-add-on-purchases"></a>Activer les achats du produit dans l’application (module complémentaire)

Un module complémentaire (produit dans l’application) est un élément supplémentaire que les joueurs peuvent acheter en partie. Les modules complémentaires peuvent être des niveaux de jeu, des éléments ou tout autre que vos joueurs peuvent apprécier. Utilisé correctement, les modules complémentaires peuvent fournir un chiffre d’affaires tout en améliorant l’expérience du jeu. Vous définissez et publiez les modules complémentaires de votre jeu via l’espace partenaires, et vous activez les achats dans l’application dans le code de votre jeu.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Modules complémentaires durables</td>
        <td><a href="/windows/uwp/monetize/enable-in-app-product-purchases">Activer les achats de produits in-app</a></td>
    </tr>
    <tr>
        <td>Modules complémentaires consommables</td>
        <td><a href="/windows/uwp/monetize/enable-consumable-in-app-product-purchases">Activer les achats de produits dans l’application consommables</a></td>
    </tr>
    <tr>
        <td>Détails et soumission du module complémentaire</td>
        <td><a href="/windows/uwp/publish/iap-submissions">Soumissions de modules complémentaires</a></td>
    </tr>
    <tr>
        <td>Surveiller les ventes et les données démographiques du module complémentaire pour votre jeu</td>
        <td><a href="/windows/uwp/publish/iap-acquisitions-report">Rapport sur les acquisitions des extensions</a></td>
    </tr>
</table>

### <a name="debugging-performance-optimization-and-monitoring"></a>Débogage, optimisation des performances et surveillance

Pour optimiser les performances, tirez parti du mode jeu dans Windows 10 afin de fournir à vos joueurs la meilleure expérience de jeu possible en utilisant pleinement la capacité de leur matériel actuel.

Le Kit Windows Performance Toolkit est composé d’outils d’analyse des performances qui génèrent des profils de performances détaillés des applications et des systèmes d’exploitation Windows. Il s’avère particulièrement précieux pour surveiller l’utilisation de la mémoire et améliorer les performances des jeux. Le Kit Windows Performance Toolkit est inclus dans le Kit de développement logiciel Windows 10 et dans Windows ADK. Ce kit d’outils comprend deux outils indépendants : l’enregistreur de performance Windows et Windows Performance Analyzer. ProcDump, qui fait partie de [Windows Sysinternals](/sysinternals/), est un utilitaire de ligne de commande qui surveille les pics d’utilisation de l’UC et génère des fichiers de vidage pendant les pannes de jeu.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Test de performances de votre code</td>
        <td><a href="https://azure.microsoft.com/services/devops/test-plans/">Test de charge basé sur le Cloud</a></td>
    </tr>
    <tr>
        <td>Obtient le type de console Xbox à l’aide des informations de périphérique de jeu</td>
        <td><a href="/previous-versions/windows/desktop/gamingdvcinfo/gaming-device-information-portal">Informations sur l’appareil de jeu</a></td>
    </tr>
    <tr>
        <td>Améliorez les performances en obtenant un accès exclusif ou prioritaire aux ressources matérielles à l’aide des API en mode jeu</td>
        <td><a href="/previous-versions/windows/desktop/gamemode/game-mode-portal">Mode jeu</a></td>
    </tr>
    <tr>
        <td>Obtenir le Kit Windows Performance Toolkit à partir de Windows 10 SDK</td>
        <td><a href="https://developer.microsoft.com/windows/downloads/windows-10-sdk">SDK Windows 10</a></td>
    </tr>
    <tr>
        <td>Obtenir le Kit Windows Performance Toolkit à partir de Windows ADK.</td>
        <td><a href="/windows-hardware/get-started/adk-install">Windows ADK</a></td>
    </tr>
    <tr>
        <td>Résoudre les problèmes de réactivité de l’interface utilisateur à l’aide de Windows Performance Analyzer (vidéo).</td>
        <td><a href="https://channel9.msdn.com/Shows/Defrag-Tools/Defrag-Tools-156-Critical-Path-Analysis-with-Windows-Performance-Analyzer">Analyse du chemin critique avec WPA</a></td>
    </tr>
    <tr>
        <td>Diagnostiquer l’utilisation et les fuites de mémoire à l’aide de Enregistreur de performance Windows (vidéo)</td>
        <td><a href="https://channel9.msdn.com/Shows/Defrag-Tools/Defrag-Tools-154-Memory-Footprint-and-Leaks">Encombrement et fuites de mémoire</a></td>
    </tr>
    <tr>
        <td>Obtenir ProcDump</td>
        <td><a href="/sysinternals/downloads/procdump">ProcDump</a></td>
    </tr>
    <tr>
        <td>Apprenez à utiliser ProcDump (vidéo)</td>
        <td><a href="https://channel9.msdn.com/Shows/Defrag-Tools/Defrag-Tools-131-Windows-10-SDK">Configurer ProcDump pour créer des fichiers de vidage</a></td>
    </tr>
</table>

### <a name="advanced-directx-techniques-and-concepts"></a>Techniques et concepts DirectX avancés

Certaines parties du développement DirectX peuvent être complexes et nuancées. Lorsque vous atteignez le stade de la production où vous devez examiner les détails de votre moteur DirectX ou déboguer des problèmes complexes de performance, les ressources et les informations présentées dans cette section sont susceptibles de vous aider.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>PIX sur Windows</td>
        <td><a href="https://devblogs.microsoft.com/pix/introducing-pix-on-windows-beta/">Outil de réglage et de débogage des performances pour DirectX 12 sur Windows</a></td>
    </tr>
    <tr>
        <td>Outils de débogage et de validation pour le développement D3D12 (vidéo)</td>
        <td><a href="https://channel9.msdn.com/Events/GDC/GDC-2017/GDC2017-003">Réglage et débogage des performances D3D12 avec la validation PIX et GPU</a></td>
    </tr>
    <tr>
        <td>Optimisation des graphismes et des performances (vidéo)</td>
        <td><a href="https://channel9.msdn.com/Events/GDC/GDC-2015/Advanced-DirectX12-Graphics-and-Performance">Graphismes et performances améliorés avec DirectX 12</a></td>
    </tr>
    <tr>
        <td>Débogage graphique DirectX (vidéo)</td>
        <td><a href="https://channel9.msdn.com/Events/GDC/GDC-2015/Solve-the-Tough-Graphics-Problems-with-your-Game-Using-DirectX-Tools">Résolution des problèmes graphiques épineux liés à votre jeu à l’aide des outils DirectX</a></td>
    </tr>
    <tr>
        <td>Outils Visual Studio 2015 pour le débogage de DirectX 12 (vidéo)</td>
        <td><a href="https://channel9.msdn.com/Series/ConnectOn-Demand/212">Outils DirectX pour Windows 10 dans Visual Studio 2015</a></td>
    </tr>
    <tr>
        <td>Guide de programmation pour Direct3D 12</td>
        <td><a href="/windows/desktop/direct3d12/direct3d-12-graphics">Guide de programmation Direct3D 12</a></td>
    </tr>
    <tr>
        <td>Combinaison de DirectX et XAML</td>
        <td><a href="directx-and-xaml-interop.md">Technologie interop DirectX et XAML</a></td>
    </tr>
</table>

### <a name="high-dynamic-range-hdr-content-development"></a>Développement de contenu HDR (High dynamique Range)

Créez du contenu de jeu qui utilise les fonctionnalités de couleur complète d’HDR.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Présentation des concepts HDR et de couleur (vidéo)</td>
        <td><a href="https://channel9.msdn.com/Events/Build/2017/P4061">Éclairage en haut et couleur avancée dans DirectX</a></td>
    </tr>
    <tr>
        <td>Découvrez comment afficher le contenu HDR et détecter si l’affichage actuel le prend en charge</td>
        <td><a href="https://github.com/Microsoft/DirectX-Graphics-Samples/tree/master/Samples/UWP/D3D12HDR">Exemple HDR</a></td>
    </tr>
    <tr>
        <td>Créer et configurer une couleur avancée à l’aide de DirectX</td>
        <td><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/D2DAdvancedColorImages">Exemple de rendu d’image couleur avancée Direct2D</a></td>
    </tr>
</table>

### <a name="globalization-and-localization"></a>Globalisation et localisation

Développez des jeux mondialisables pour la plate-forme Windows et découvrez les fonctionnalités internationales intégrées aux produits Microsoft les plus populaires.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Préparation de votre jeu pour le marché international</td>
        <td><a href="/windows/uwp/globalizing/globalizing-portal">Recommandations en matière de développement pour un public international</a></td>
    </tr>
    <tr>
        <td>Combler le fossé entre les langues, les cultures et la technologie</td>
        <td><a href="https://www.microsoft.com/Language/Default.aspx">Ressources en ligne pour les conventions linguistiques et la terminologie Microsoft standard</a></td>
    </tr>
</table>

## <a name="submitting-and-publishing-your-game"></a>Envoi et publication du jeu

Les informations et guides suivants contribuent à rendre le processus de soumission et de publication aussi aisé que possible.

### <a name="publishing"></a>Publication

Vous allez utiliser l' [espace partenaires](https://partner.microsoft.com/dashboard) pour publier et gérer vos packages de jeux.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Publication d’application de l’espace partenaires</td>
        <td><a href="https://developer.microsoft.com/store/publish-apps">Publier des applications Windows</a></td>
    </tr>
    <tr>
        <td>Publication avancée de l’espace partenaires (GDN)</td>
        <td><a href="https://developer.xboxlive.com/windows/documentation/Pages/home.aspx">Guide de publication avancé de l’espace partenaires</a></td>
    </tr>
    <tr>
        <td>Utiliser Azure Active Directory (AAD) pour ajouter des utilisateurs à votre compte espace partenaires</td>
        <td><a href="/windows/uwp/publish/manage-account-users">Gérer des utilisateurs de compte</a></td>
    </tr>
    <tr>
        <td>Évaluation de votre jeu (billet de blog)</td>
        <td><a href="https://blogs.windows.com/buildingapps/2016/01/06/now-available-single-age-rating-system-to-simplify-app-submissions/">Flux de travail unique pour affecter les évaluations de l’âge à l’aide du système IARC</a></td>
    </tr>
</table>

#### <a name="packaging-and-uploading"></a>Création du package et chargement

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Apprendre à utiliser l’installation en continu et les packages facultatifs (vidéo)</td>
        <td><a href="https://channel9.msdn.com/Events/Build/2017/B8093">Distribution d’applications UWP NextGen Firewall : création d’applications extensibles, en flux continu, basées sur des composants</a></td>
    </tr>
    <tr>
        <td>Diviser et regrouper le contenu pour activer l’installation en continu</td>
        <td><a href="/windows/msix/package/streaming-install">Installation de la diffusion en continu d’applications UWP</a></td>
    </tr>
    <tr>
        <td>Créer des packages facultatifs tels que le contenu du jeu DLC</td>
        <td><a href="/windows/msix/package/optional-packages">Création de packages facultatifs et d’ensembles associés</a></td>
    </tr>
    <tr>
        <td>Empaqueter votre jeu UWP</td>
        <td><a href="../packaging/index.md">Empaquetage d’applications</a></td>
    </tr>
    <tr>
        <td>Empaqueter votre jeu DirectX UWP</td>
        <td><a href="package-your-windows-store-directx-game.md">Empaqueter votre jeu DirectX UWP</a></td>
    </tr>
    <tr>
        <td>Empaquetage de votre jeu en tant que développeur tiers (billet de blog)</td>
        <td><a href="https://blogs.windows.com/buildingapps/2015/12/15/building-an-app-for-a-3rd-party-how-to-package-their-store-app/">Créer des packages téléchargeables sans accès au compte Windows Store de l’éditeur</a></td>
    </tr>
    <tr>
        <td>Création de packages d’application et d’ensembles de packages d’application à l’aide de MakeAppx</td>
        <td><a href="/windows/msix/package/create-app-package-with-makeappx-tool">Créer des packages à l’aide de l’outil de création de packages d’application MakeAppx.exe</a></td>
    </tr>
    <tr>
        <td>Signature numérique des fichiers à l’aide de SignTool</td>
        <td><a href="/windows/desktop/SecCrypto/signtool">Signer les fichiers et vérifier les signatures dans les fichiers à l’aide de SignTool</a></td>
    </tr>
    <tr>
        <td>Chargement et contrôle de version de votre jeu</td>
        <td><a href="/windows/uwp/publish/upload-app-packages">Chargement des packages d’application</a></td>
    </tr>
</table>

### <a name="policies-and-certification"></a>Stratégies et certifications

Ne laissez pas les problèmes de certification retarder la publication de votre jeu. Voici des stratégies et des problèmes courants de certification à connaître.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Contrat du développeur de l’application Microsoft Store</td>
        <td><a href="/legal/windows/agreements/app-developer-agreement">Contrat du développeur d’application</a></td>
    </tr>
    <tr>
        <td>Stratégies pour la publication d’applications dans le Microsoft Store</td>
        <td><a href="/legal/windows/agreements/store-policies">Politiques du Microsoft Store</a></td>
    </tr>
    <tr>
        <td>Comment faire pour éviter certains problèmes de certification d’application courants</td>
        <td><a href="/windows/uwp/publish/avoid-common-certification-failures">Éviter les échecs de certification courants</a></td>
    </tr>
</table>

### <a name="store-manifest-storemanifestxml"></a>Manifeste de magasin (StoreManifest.xml)

Le manifeste de magasin (StoreManifest.xml) est un fichier de configuration facultatif qui peut être inclus dans votre package d’application. Il fournit des fonctionnalités supplémentaires qui ne font pas partie du fichier AppxManifest.xml. Par exemple, vous pouvez utiliser le manifeste de magasin pour bloquer l’installation de votre jeu si un appareil cible ne possède pas le niveau de fonctionnalité DirectX minimal spécifié ou la mémoire système minimale spécifiée.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Schéma du manifeste de magasin</td>
        <td><a href="/uwp/schemas/storemanifest/storemanifestschema2015/schema-root">Schéma StoreManifest (Windows 10)</a></td>
    </tr>
</table>

## <a name="game-lifecycle-management"></a>Gestion du cycle de vie des jeux

Vous n’avez pas terminé une fois que vous avez développé et fourni votre jeu. Si vous en avez fini avec le développement de la première version, le circuit de votre jeu sur le marché commence à peine quant à lui. Vous allez surveiller son utilisation et les rapports d’erreur, répondre aux commentaires des utilisateurs, et publier des mises à jour pour votre jeu.

### <a name="partner-center-analytics-and-promotion"></a>Promotion et analytique de l’espace partenaires

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Analyse de l’espace partenaires</td>
        <td><a href="/windows/uwp/publish/analytics">Analyser le niveau de performance de l’application</a></td>
    </tr>
    <tr>
        <td>Découvrez comment vos clients sont en contact avec les fonctionnalités Xbox de votre jeu</td>
        <td><a href="../publish/xbox-analytics-report.md">Rapport d’analytique Xbox</a></td>
    </tr>
    <tr>
        <td>Réponse aux avis des clients</td>
        <td><a href="/windows/uwp/publish/respond-to-customer-reviews">Répondre aux avis des clients</a></td>
    </tr>
    <tr>
        <td>Méthodes pour promouvoir votre jeu</td>
        <td><a href="https://developer.microsoft.com/store/promote-your-apps">Promouvoir vos applications</a></td>
    </tr>
</table>

### <a name="visual-studio-application-insights"></a>Visual Studio Application Insights

Visual Studio Application Insights fournit des analyses de performance, de télémétrie et d’utilisation pour votre jeu publié. Application Insights vous permet de détecter et de résoudre les problèmes après publication de votre jeu, de surveiller et d’améliorer en continu son utilisation et de comprendre comment les joueurs ne cessent d’interagir avec votre jeu. Application Insights fonctionne par l’ajout d’un kit de développement logiciel (SDK) à votre application, qui envoie la télémétrie au [portail Azure](https://portal.azure.com/).

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Analyses de performance et d’utilisation d’application</td>
        <td><a href="/azure/azure-monitor/app/app-insights-overview">Application Insights Visual Studio</a></td>
    </tr>
    <tr>
        <td>Activer Application Insights dans les applications Windows</td>
        <td><a href="/azure/azure-monitor/overview">Application Insights pour les applications Windows Phone et les applications du Windows Store</a></td>
    </tr>
</table>

### <a name="third-party-solutions-for-analytics-and-promotion"></a>Solutions tierces pour l’analyse et la promotion

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Comprendre le comportement des lecteurs à l’aide de GameAnalytics</td>
        <td><a href="https://gameanalytics.com/">GameAnalytics</a></td>
    </tr>
    <tr>
        <td>Connectez votre jeu UWP à Google Analytics</td>
        <td><a href="https://github.com/dotnet/windows-sdk-for-google-analytics">Obtenir des SDK Windows pour Google Analytics</a></td>
    </tr>
    <tr>
        <td>En savoir plus sur l’utilisation de SDK Windows pour Google Analytics (vidéo)</td>
        <td><a href="https://channel9.msdn.com/Events/Windows/Windows-Developer-Day-Creators-Update/Getting-started-with-the-Windows-SDK-for-Google-Analytics">Prise en main de SDK Windows pour Google Analytics</a></td>
    </tr>
    <tr>
        <td>Utiliser l’application Facebook installe des publicités pour promouvoir votre jeu pour les utilisateurs de Facebook</td>
        <td><a href="https://github.com/Microsoft/winsdkfb">Obtenir des SDK Windows pour Facebook</a></td>
    </tr>
    <tr>
        <td>Découvrez comment utiliser l’application Facebook installe ADS (vidéo)</td>
        <td><a href="https://channel9.msdn.com/Events/Windows/Windows-Developer-Day-Creators-Update/Getting-started-with-Facebook-App-Install-Ads">Prise en main de SDK Windows pour Facebook</a></td>
    </tr>
    <tr>
        <td>Utilisez Vungle pour ajouter des publicités vidéo à vos jeux</td>
        <td><a href="https://publisher.vungle.com/sdk/">Obtenir SDK Windows pour Vungle</a></td>
    </tr>
</table>

### <a name="creating-and-managing-content-updates"></a>Création et gestion des mises à jour de contenu

Pour mettre à jour votre jeu publié, soumettez un nouveau package d’application avec un numéro de version supérieur. Le package est automatiquement mis à la disposition des clients en tant que mise à jour dès qu’il a passé les étapes de soumission et de certification.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Mise à jour et contrôle de version de votre jeu</td>
        <td><a href="/windows/uwp/publish/package-version-numbering">Numérotation des versions de packages</a></td>
    </tr>
    <tr>
        <td>Recommandations en matière de gestion des packages de jeu</td>
        <td><a href="/windows/uwp/publish/package-version-numbering">Aide sur la gestion des packages d’application</a></td>
    </tr>
</table>

## <a name="adding-xbox-live-to-your-game"></a>Ajout de Xbox Live à votre jeu

Xbox Live est un réseau de jeux Premier qui connecte des millions de joueurs dans le monde entier. Les développeurs ont accès à des fonctionnalités Xbox Live qui peuvent augmenter de façon naturelle le public de leur jeu, y compris la présence Xbox Live, Leaderboards, les enregistrements Cloud, les concentrateurs de jeux, les clubs, les conversations des tiers, les jeux DVR et bien plus encore.

> [!Note]
> Si vous souhaitez développer des titres Xbox Live, vous disposez de plusieurs options. Pour plus d’informations sur les différents programmes, consultez [vue d’ensemble du programme de développement](/gaming/xbox-live/developer-program-overview).

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Présentation de Xbox Live</td>
        <td><a href="/gaming/xbox-live/index.md">Guide du développeur Xbox Live</a></td>
    </tr>
    <tr>
        <td>Comprendre les fonctionnalités disponibles en fonction du programme</td>
        <td><a href="/gaming/xbox-live/developer-program-overview.md#feature-table">Vue d’ensemble du programme de développement : tableau des fonctionnalités</a></td>
    </tr>
    <tr>
        <td>Liens vers des ressources utiles pour le développement de jeux Xbox Live</td>
        <td><a href="/gaming/xbox-live/xbox-live-resources.md">Ressources Xbox Live</a></td>
    </tr>
    <tr>
        <td>Découvrez comment obtenir des informations à partir des services Xbox Live</td>
        <td><a href="/gaming/xbox-live/introduction-to-xbox-live-apis.md">Présentation des API Xbox Live</a></td>
    </tr>
</table>

### <a name="for-developers-in-the-xbox-live-creators-program"></a>Pour les développeurs dans le programme de créateurs Xbox Live

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Vue d’ensemble</td>
        <td><a href="/gaming/xbox-live/get-started-with-creators/get-started-with-xbox-live-creators.md">Prise en main du programme de créateurs Xbox Live</a></td>
    </tr>
    <tr>
        <td>Ajouter Xbox Live à votre jeu</td>
        <td><a href="/gaming/xbox-live/get-started-with-creators/creators-step-by-step-guide.md">Guide pas à pas pour intégrer le programme Xbox Live Creators</a></td>
    </tr>
    <tr>
        <td>Ajouter Xbox Live à votre jeu UWP créé à l’aide d’Unity</td>
        <td><a href="/gaming/xbox-live/get-started-with-creators/develop-creators-title-with-unity.md">Prise en main du développement d’un programme Xbox Live Creators avec le moteur de jeu Unity</a></td>
    </tr>
    <tr>
        <td>Configurer votre bac à sable (sandbox) de développement</td>
        <td><a href="/gaming/xbox-live/get-started-with-creators/xbox-live-sandboxes-creators.md">Présentation des bacs à sable Xbox Live</a></td>
    </tr>
    <tr>
        <td>Configurer des comptes de test</td>
        <td><a href="/gaming/xbox-live/get-started-with-creators/authorize-xbox-live-accounts.md">Autoriser des comptes Xbox Live dans votre environnement de test</a></td>
    </tr>
    <tr>
        <td>Exemples pour le programme Xbox Live Creators</td>
        <td><a href="https://github.com/Microsoft/xbox-live-samples/tree/master/Samples/CreatorsSDK">Exemples de code pour les développeurs de programmes pour les créateurs</a></td>
    </tr>
    <tr>
        <td>Découvrez comment intégrer des expériences de Xbox Live sur plusieurs plateformes dans les jeux UWP (vidéo)</td>
        <td><a href="https://channel9.msdn.com/Events/GDC/GDC-2017/GDC2017-005">Programme Créateurs Xbox Live</a></td>
    </tr>
</table>

### <a name="for-managed-partners-and-developers-in-the-idxbox-program"></a>Pour les partenaires et les développeurs gérés dans le ID@Xbox programme

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Vue d’ensemble</td>
        <td><a href="/gaming/xbox-live/get-started-with-partner/get-started-with-xbox-live-partner.md">Prise en main de Xbox Live en tant que partenaire géré ou développeur d’ID</a></td>
    </tr>
    <tr>
        <td>Ajouter Xbox Live à votre jeu</td>
        <td><a href="/gaming/xbox-live/get-started-with-partner/partners-step-by-step-guide.md">Guide pas à pas pour intégrer Xbox Live pour les partenaires gérés et les membres ID</a></td>
    </tr>
    <tr>
        <td>Ajouter Xbox Live à votre jeu UWP créé à l’aide d’Unity</td>
        <td><a href="/gaming/xbox-live/get-started-with-partner/partner-unity-uwp-il2cpp.md">Ajout de la prise en charge de Xbox Live à Unity pour UWP avec un backend de script IL2CPP pour l’ID et les partenaires gérés</a></td>
    </tr>
    <tr>
        <td>Configurer votre bac à sable (sandbox) de développement</td>
        <td><a href="/gaming/xbox-live/get-started-with-partner/advanced-xbox-live-sandboxes.md">Bac à sables Xbox Live avancés</a></td>
    </tr>
    <tr>
        <td>Configuration requise pour les jeux qui utilisent Xbox Live (GDN)</td>
        <td><a href="https://edadfs.partners.extranet.microsoft.com/adfs/ls/?wa=wsignin1.0&wtrealm=https%3a%2f%2fdeveloper.xboxlive.com&wctx=rm%3d0%26id%3dpassive%26ru%3d%252fen-us%252flive%252fcertification%252frequirements%252fPages%252fTCR.aspx&wct=2019-11-20T19%3a55%3a26Z">Configuration Xbox requise pour Xbox Live sur Windows 10</a></td>
    </tr>
    <tr>
        <td>Exemples</td>
        <td><a href="https://github.com/Microsoft/xbox-live-samples/tree/master/Samples/ID%40XboxSDK">Exemples de code pour ID@Xbox les développeurs</a></td>
    </tr>
    <tr>
        <td>Vue d’ensemble du développement de jeux Xbox Live (vidéo)</td>
        <td><a href="https://channel9.msdn.com/Events/GDC/GDC-2015/Developing-with-Xbox-Live-for-Windows-10">Développement avec Xbox Live pour Windows 10</a></td>
    </tr>
    <tr>
        <td>Matchmaking multiplateforme (vidéo)</td>
        <td><a href="https://channel9.msdn.com/Events/GDC/GDC-2015/Xbox-Live-Multiplayer-Introducing-services-for-cross-platform-matchmaking-and-gameplay">Xbox Live en multijoueur : Présentation des services de matchmaking et de jeu multiplateforme</a></td>
    </tr>
    <tr>
        <td>Jeu multiplateforme dans Fable Legends (vidéo)</td>
        <td><a href="https://channel9.msdn.com/Events/GDC/GDC-2015/Fable-Legends-Cross-device-Gameplay-with-Xbox-Live">Fable Legends : Jeu multiplateforme avec Xbox Live</a></td>
    </tr>
    <tr>
        <td>Xbox Live : Statistiques et succès (vidéo)</td>
        <td><a href="https://channel9.msdn.com/Events/GDC/GDC-2015/Best-Practices-for-Leveraging-Cloud-Based-User-Stats-and-Achievements-in-Xbox-Live">Meilleures pratiques pour tirer parti des statistiques et des succès des utilisateurs basés sur le cloud dans Xbox Live</a></td>
    </tr>
</table>

## <a name="additional-resources"></a>Ressources supplémentaires

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Vidéos de développement de jeux</td>
        <td><a href="/windows/uwp/gaming/game-development-videos">Vidéos de conférences majeures comme GDC et//Build</a></td>
    </tr>
    <tr>
        <td>Développement de jeux indépendants (vidéo)</td>
        <td><a href="https://channel9.msdn.com/Events/GDC/GDC-2015/New-Opportunities-for-Independent-Developers">De nouvelles opportunités pour les développeurs indépendants</a></td>
    </tr>
    <tr>
        <td>Considérations pour les appareils mobiles multicœurs (vidéo)</td>
        <td><a href="https://channel9.msdn.com/Events/GDC/GDC-2015/Sustained-gaming-performance-in-multi-core-mobile-devices">Performances de jeu soutenues sur les appareils mobiles multicœurs</a></td>
    </tr>
    <tr>
        <td>Développement de jeux de bureau Windows 10 (vidéo)</td>
        <td><a href="https://channel9.msdn.com/Events/GDC/GDC-2015/PC-Games-for-Windows-10">Jeux pour PC Windows 10</a></td>
    </tr>
</table>