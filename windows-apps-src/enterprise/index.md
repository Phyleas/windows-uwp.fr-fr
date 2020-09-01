---
ms.assetid: 4b0c86d3-f05b-450b-bf9c-6ab4d3f07d31
description: Cette feuille de route propose une vue d’ensemble des principales fonctionnalités d’entreprise pour les applications UWP (plateforme Windows universelle) et Windows 10.
title: Entreprise
ms.date: 01/16/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: ff96cd25c32418f518f6b1807e08eb72c3e14c6a
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89168433"
---
# <a name="enterprise"></a>Entreprise

Cet article propose une vue d’ensemble des principales fonctionnalités d’entreprise fournies par les applications UWP (plateforme Windows universelle) pour Windows 10. Pour regarder une vidéo présentant en détail certaines de ces fonctionnalités, consultez [Rapidly Construct LOB Applications with UWP and Visual Studio](https://channel9.msdn.com/Events/Build/2018/BRK3502).

## <a name="feature-highlights"></a>Présentation des fonctionnalités

<a id="template-studio" />

### <a name="windows-template-studio"></a>Windows Template Studio

Windows Template Studio est une extension Visual Studio 2019 qui accélère la création d’applications UWP (plateforme Windows universelle) à l’aide d’un Assistant. Le projet UWP résultant est constitué de code bien mis en forme et lisible, qui intègre les dernières fonctionnalités de Windows 10, tout en implémentant des modèles éprouvés et des bonnes pratiques.

![Windows Template Studio](images/windows-template-studio.png)

Consultez [Windows Template Studio](https://marketplace.visualstudio.com/items?itemName=WASTeamAccount.WindowsTemplateStudio)

<a id="desktop-style-UI" />

### <a name="controls-to-create-desktop-style-uis"></a>Contrôles permettant de créer des IU d’applications de bureau

Nous avons publié de nouveaux contrôles UWP XAML qui comblent le fossé entre une IU d’Application de bureau classique et une IU UWP.

Par exemple, les nouveaux contrôles [MenuBar](../design/controls-and-patterns/menus.md), [DropDownButton](../design/controls-and-patterns/buttons.md#create-a-drop-down-button), [SplitButton](../design/controls-and-patterns/buttons.md#create-a-split-button) et [CommandBarFlyout](../design/controls-and-patterns/command-bar-flyout.md) vous offrent des moyens plus souples d’exposer les commandes. De plus, [EditableComboBox](../design/controls-and-patterns/combo-box.md#make-a-combo-box-editable) permet à l’utilisateur d’entrer des valeurs qui ne figurent pas dans une liste prédéfinie d’options.

![MenuBar](images/menu-bar.png)

<a id="enterprise" />

### <a name="controls-to-support-enterprise-scenarios"></a>Contrôles permettant de prendre en charge des scénarios d’entreprise

[DataGridView](/windows/communitytoolkit/controls/datagrid) offre un moyen flexible d’afficher une collection de données en lignes et en colonnes.

[TreeView](../design/controls-and-patterns/tree-view.md) active une liste hiérarchique comportant des nœuds que vous pouvez développer et réduire, et qui contiennent des éléments imbriqués. Vous pouvez l’utiliser pour illustrer une structure de dossiers ou des relations imbriquées dans votre IU.

![Contrôle DataGrid](images/DataGrid.gif)


### <a name="windows-ui-library"></a>Bibliothèque d’IU Windows

La bibliothèque d’IU Windows est un ensemble de packages NuGet qui fournissent des contrôles et autres éléments d’interface utilisateur pour les applications UWP. Elle permet également une compatibilité de bas niveau avec les versions antérieures de Windows 10, pour que votre application fonctionne même si les utilisateurs n’ont pas le dernier système d’exploitation.

![Bibliothèque d’IU Windows](images/win-ui.png)

Consultez [Bibliothèque d’IU Windows (préversion)](/uwp/toolkits/winui/).

<a id="xaml-islands" />

### <a name="uwp-controls-in-desktop-applications-xaml-islands"></a>Contrôles UWP dans les applications de bureau (XAML Islands)

Windows 10 vous permet maintenant d’utiliser les contrôles UWP dans les applications de bureau WPF, Windows Forms et C++ Win32 avec une fonctionnalité appelée *XAML Islands*. Cela signifie que vous pouvez améliorer l’apparence et les fonctionnalités de vos Applications de bureau existantes à l’aide des dernières fonctionnalités de l’IU Windows 10. Celles-ci sont disponibles uniquement via les contrôles UWP, tels que Windows Ink, et les contrôles prenant en charge Fluent Design System. Cette fonctionnalité est désignée sous le nom d’îles XAML.

Consultez [Contrôles UWP dans les applications de bureau](/windows/apps/desktop/modernize/xaml-islands).

<a id="standard" />

### <a name="net-standard-20"></a>.NET Standard 2.0

.NET Standard comprend plus de 20 000 API supplémentaires par rapport à .NET Standard 1.x. Cela rend tellement plus facile la migration des bibliothèques .NET Framework existantes, puis leur utilisation dans différentes applications .NET, notamment votre application UWP.

![net-standard](images/dot-net-standard-project-template.png)

Consultez [Partager du code entre une application de bureau et une application UWP](../porting/desktop-to-uwp-migrate.md).

<a id="sql-server" />

### <a name="sql-server-connectivity"></a>Connectivité SQL Server

Votre application peut se connecter directement à une base de données SQL Server, puis stocker et récupérer des données à l’aide de classes de l’espace de noms [System.Data.SqlClient](/dotnet/api/system.data.sqlclient).

Consultez [Utiliser une base de données SQL Server dans une application UWP](../data-access/sql-server-databases.md).

<a id="MSIX" />

### <a name="msix-deployment"></a>Déploiement de MSIX

MSIX est un format de package d’application Windows qui regroupe les meilleures fonctionnalités de MSI, .appx, App-V et ClickOnce pour proposer une expérience d’empaquetage moderne et fiable dans toutes les applications Windows. Le format de package MSIX conserve les packages d’applications et les fichiers d’installation existants tout en proposant des fonctionnalités d’empaquetage et de déploiement modernes pour les applications Win32, WPF et Windows Forms. 

![Icône MSIX](images/MSIX-App-Package.ico)

Consultez la [documentation de MSIX](/windows/msix/).

<a id="distribution" />

## <a name="security"></a>Sécurité

Windows 10 fournit une suite de fonctionnalités de sécurité qui permet aux développeurs d’applications de protéger l’identité de leurs utilisateurs, la sécurité des réseaux d’entreprise ainsi que toutes les données professionnelles stockées sur des appareils. Nouveauté de Windows 10, Microsoft Passport constitue une alternative à l’utilisation de mots de passe. Il s’agit d’une solution d’authentification à deux facteurs facile à déployer et accessible à l’aide d’un code PIN ou de Windows Hello, qui fournit une sécurité de qualité professionnelle et prend en charge la reconnaissance des empreintes digitales et de l’iris ainsi que la reconnaissance faciale.

| Rubrique | Description |
|-------|-------------|
| [Présentation du développement d’applications Windows sécurisées](../security/intro-to-secure-windows-app-development.md) | Cet article introductif décrit les différentes fonctionnalités de sécurité Windows lors des phases d’authentification, de données en transit et de données au repos. Il explique aussi comment intégrer ces phases dans vos applications. Il couvre une large gamme de rubriques et vise essentiellement à aider les architectes d’application à mieux comprendre les fonctionnalités Windows qui facilitent et accélèrent la création d’applications de plateforme Windows universelle. |
| [Authentification et identité des utilisateurs](../security/authentication-and-user-identity.md) | Les applications UWP disposent de plusieurs options pour l’authentification des utilisateurs qui sont décrites dans cet article. Pour l’entreprise, l’utilisation de la nouvelle fonctionnalité Microsoft Passport est fortement recommandée. Microsoft Passport remplace les mots de passe par la méthode d’authentification à 2 facteurs (2FA) forte en vérifiant les informations d’identification existantes et en créant des informations d’identification spécifiques à l’appareil, protégées par un mouvement de l’utilisateur reposant sur l’entrée de son code PIN ou par la biométrie. |
| [Cryptographie](../security/cryptography.md) | La section sur le chiffrement fournit une vue d’ensemble des fonctionnalités de chiffrement disponibles pour les applications UWP. Les articles comprennent des procédures pas à pas introductives pour chiffrer facilement les données professionnelles sensibles et couvrent également des sujets plus spécifiques comme la manipulation des clés de chiffrement et l’utilisation des codes d’authentification de message (MAC), codes de hachage et signatures. |
| [Protection des informations Windows (WIP)](wip-hub.md) | Il s’agit d’une rubrique centrale destinée aux développeurs qui décrit de manière exhaustive la relation de la Protection des informations Windows avec les fichiers, les mémoires tampon, le Presse-papiers, les réseaux, les tâches en arrière-plan et la protection des données verrouillées. |

## <a name="data-binding-and-databases"></a>Liaison de données et bases de données

La liaison de données est un moyen dont dispose l’interface utilisateur de votre application pour afficher des données provenant d’une source externe (par exemple, une base de données) et éventuellement rester synchronisée avec ces données. La liaison de données vous permet de séparer les problématiques liées aux données de celles liées à l’interface utilisateur, ce qui se traduit par un modèle conceptuel plus simple et l’amélioration de la lisibilité, de la testabilité et de la gestion de la maintenance de votre application.

| Rubrique | Description |
|-------|-------------|
| [Vue d’ensemble de la liaison de données](../data-binding/data-binding-quickstart.md) | Cette rubrique vous montre comment lier un contrôle (ou tout autre élément d’IU) à un seul élément, ou comment lier un contrôle d’éléments à une collection d’éléments dans une application UWP (plateforme Windows universelle). Elle explique également comment contrôler le rendu des éléments, implémenter une vue des détails en fonction d’une sélection et convertir des données pour l’affichage. |
| [Entity Framework 7 pour UWP](/ef/core/get-started/) | Entity Framework 7, qui prend en charge UWP, vous permet d’exécuter facilement des requêtes complexes dans de grands ensembles de données. Dans cette procédure pas à pas, vous allez générer une application UWP qui propose un accès de base aux données d’une base de données SQLite locale à l’aide d’Entity Framework. |
| [Base de données SQLite locale](https://channel9.msdn.com/Series/A-Developers-Guide-to-Windows-10/10) | Cette vidéo est un guide du développeur complet sur l’utilisation de SQLite, la solution recommandée pour les bases de données d’applications locales. Visitez [SQLite](https://www.sqlite.org/download.html) pour télécharger la dernière version pour UWP, ou utilisez la version fournie avec le kit SDK Windows 10. |

## <a name="networking-and-data-serialization"></a>Réseau et sérialisation des données

Les applications métier ont souvent besoin de communiquer avec des données ou de les stocker dans d’autres systèmes divers. Cette opération est généralement effectuée en vous connectant à un service réseau (à l’aide de protocoles tels que REST ou SOAP), puis en sérialisant ou désérialisant les données dans un format commun. L’utilisation des réseaux et de la sérialisation des données dans les applications UWP est similaire à celle dans les applications WPF, WinForms et ASP.NET. Consultez les articles suivants pour plus d’informations.

| Rubrique | Description |
|-------|-------------|
| [Notions de base en matière de réseau](../networking/networking-basics.md) | Cette procédure pas à pas explique les concepts réseau de base qui s’appliquent à toutes les applications UWP, quels que soient les protocoles de communication utilisés.  |
| [Quelle technologie de réseau ?](../networking/which-networking-technology.md) | Vue d’ensemble des technologies réseau disponibles pour les applications UWP, avec des conseils qui vous aideront à choisir les technologies appropriées pour votre application. |
| [Sérialisation XML et SOAP](/dotnet/framework/serialization/xml-and-soap-serialization) | La sérialisation XML convertit les objets en un flux XML conforme à un langage XSD (définition de schéma XML) spécifique. Pour effectuer une conversion entre XML et une classe fortement typée, vous pouvez utiliser la classe [XDocument](/dotnet/api/system.xml.linq.xdocument) native ou une bibliothèque externe. |
| [Sérialisation JSON](/uwp/api/Windows.Data.Json) | La sérialisation JSON (JavaScript Object Notation) est un format populaire utilisé pour la communication avec les API REST. [Newtonsoft Json.NET](https://www.newtonsoft.com/json), qui est entièrement pris en charge pour les applications UWP. |

## <a name="devices"></a>Appareils

Afin d’interagir avec des outils métier comme des imprimantes, des scanneurs de codes-barres ou des lecteurs de cartes à puce, vous jugerez peut-être nécessaire d’intégrer des appareils ou des capteurs externes à votre application. Voici quelques exemples des fonctionnalités que vous pouvez ajouter à votre application à l’aide de la technologie décrite dans cette section.

| Rubrique  | Description |
|--------|-------------|
| [Énumérer les appareils](../devices-sensors/enumerate-devices.md) | Cet article décrit comment utiliser l’espace de noms [Windows.Devices.Enumeration](/uwp/api/Windows.Devices.Enumeration) pour rechercher des appareils connectés au système, en interne, en externe ou détectables sur les protocoles sans fil ou réseau. Commencez ici si vous créez une application qui fonctionne avec des appareils. |
| [Impression et numérisation](../devices-sensors/printing-and-scanning.md) | Explique comment imprimer et numériser à partir de votre application, et notamment comment se connecter aux appareils métier (par exemple, les systèmes de point de vente (PDV), les imprimantes de reçus ainsi que les scanneurs à chargeur à grande capacité) et comment les utiliser. |
| [Bluetooth](../devices-sensors/bluetooth.md) | En plus de l’utilisation des connexions Bluetooth traditionnelles pour envoyer et recevoir des données ou contrôler les appareils, Windows 10 permet d’utiliser la technologie Bluetooth Low Energy (BTLE) pour envoyer ou recevoir des balises en arrière-plan. Utilisez-la pour afficher des notifications ou activer des fonctionnalités quand un utilisateur s’approche d’un endroit particulier ou le quitte. |
| [Stockage partagé d’entreprise](enterprise-shared-storage.md) | Dans les scénarios où l’appareil est verrouillé, découvrez comment partager les données au sein de la même application, entre les instances d’une application ou entre les applications. |

## <a name="device-targeting"></a>Ciblage des appareils

Aujourd’hui, de nombreux utilisateurs travaillent avec leur propre téléphone ou tablette, des appareils qui présentent des facteurs de forme et des tailles d’écran différents. Avec la plateforme Windows universelle (UWP), vous pouvez écrire une application métier unique qui s’exécute indifféremment sur tous les types d’appareils, notamment les ordinateurs de bureau et les écrans de résolution PPP diverse, ce qui vous permet d’optimiser la portée de votre application et l’efficacité de votre code.

| Rubrique | Description |
|-------|-------------|
| [Guide des applications UWP](../get-started/universal-application-platform-guide.md) | Dans ce guide introductif, vous allez découvrir la plateforme UWP Windows 10, notamment : ce qu’est une famille d’appareils et comment déterminer celle à cibler, quels sont les nouveaux volets et contrôles d’interface utilisateur permettant d’adapter votre interface utilisateur à différents facteurs de forme d’appareil, et comment utiliser et contrôler la surface d’API disponible dans votre application. |
| [Exemple de code d’IU XAML adaptative](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics) | Cet exemple de code montre toutes les options de disposition et tous les contrôles possibles pour votre application, quel que soit le type d’appareil. Il vous permet d’interagir avec les panneaux pour découvrir comment obtenir les dispositions que vous recherchez. En plus de vous présenter la façon dont chaque contrôle répond à différents facteurs de forme, l’application réagit et indique les différentes méthodes permettant d’obtenir une interface utilisateur adaptative. |
| [Rubrique Xamarin](/xamarin/) | Xamarin pour le ciblage du téléphone |

## <a name="deployment"></a>Déploiement

Vous disposez d’options pour distribuer les applications aux utilisateurs de votre organisation en utilisant les packages MSIX. Vous pouvez configurer un déploiement basé sur le Programme d’installation d’application, utiliser des outils de gestion d’appareils comme Microsoft Endpoint Configuration Manager et Microsoft Intune, publier sur le Microsoft Store pour Entreprises ou effectuer un sideloading d’applications sur les appareils. Vous pouvez également mettre vos applications à la disposition du grand public en les publiant sur le Microsoft Store.

| Rubrique | Description |
|-------|-------------|
| [Documentation MSIX](/windows/msix/) | MSIX est un format de package d’application Windows qui regroupe les meilleures fonctionnalités de MSI, .appx, App-V et ClickOnce pour proposer une expérience d’empaquetage moderne et fiable. |
| [Distribuer des applications métier aux entreprises](../publish/distribute-lob-apps-to-enterprises.md) | Découvrez les options pour distribuer des applications métier sans mettre les applications à la disposition du public, notamment le déploiement basé sur le Programme d’installation d’application, Microsoft Endpoint Configuration Manager et Microsoft Intune, et la publication sur le Microsoft Store pour Entreprises. |
| [Charger la version test des applications](/windows/deploy/sideload-apps-in-windows-10) | Quand vous chargez une application de façon indépendante (sideloading), vous déployez un package d’application signé sur un appareil. Vous conservez la signature, l’hébergement et le déploiement de ces applications. Le processus de sideloading d’applications est simplifié pour Windows 10.             |
| [Publier des applications sur le Microsoft Store](https://developer.microsoft.com/store/publish-apps) | Le Microsoft Store unifié vous permet de publier et de gérer toutes vos applications pour l’ensemble des appareils Windows. Personnalisez la disponibilité de votre application avec un tarif par marché, des contrôles de distribution et de visibilité et d’autres options. |

## <a name="enterprise-uwp-samples"></a>Exemples UWP pour entreprises

| Rubrique |  Description |
|------ |--------------|
| [Exemple de l’inventaire VanArsdel](https://github.com/Microsoft/InventorySample) | Exemple d’application UWP qui présente des scénarios métier. L’exemple est basé sur la création et la gestion de clients, de commandes et de produits pour la société fictive VanArsdel. |
| [Exemple de base de données de commandes de clients](https://github.com/Microsoft/Windows-appsample-customers-orders-database) | Exemple d’application UWP qui présente des fonctionnalités utiles aux développeurs d’entreprise, par exemple l’authentification AAD (Azure Active Directory), les contrôles d’IU (notamment une grille de données), l’intégration de bases de données SQL Azure et Sqlite, Entity Framework ainsi que les services d’API cloud. L’exemple est basé sur la création et la gestion de comptes clients, de commandes et de produits pour la société fictive Contoso. |

## <a name="patterns-and-practices"></a>Modèles et pratiques

Les bases de code pour les applications d’entreprise à grande échelle peuvent être difficiles à gérer. Prism est un framework permettant de générer des applications XAML pouvant être testées, faciles à gérer et faiblement couplées dans WPF, Windows 10 UWP et Xamarin Forms. Prism fournit une implémentation d’une collection de modèles de conception utiles pour écrire des applications XAML bien structurées et faciles à gérer, notamment des modèles MVVM, d’injection de dépendance, de commandes, EventAggregator, etc.

Pour plus d’informations sur Prism, consultez le [dépôt GitHub](https://github.com/PrismLibrary/Prism).

 

 