---
title: Demander une clé d’authentification de cartes
description: Votre application Windows universelle doit être authentifiée pour pouvoir utiliser le MapControl et les services cartographiques dans l’espace de noms Windows.Services.Maps.
ms.assetid: 13B400D7-E13F-4F07-ACC3-9C34087F0F73
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, clé d’authentification cartographique, contrôle de carte
ms.localizationpriority: medium
ms.openlocfilehash: 195c643fe10daf45ae709962a3b675162874c77b
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89171833"
---
# <a name="request-a-maps-authentication-key"></a>Demander une clé d’authentification de cartes

> [!WARNING]
> Les services Online Maps ne sont peut-être pas disponibles dans les versions antérieures de Windows 10. Dans les versions suivantes, collection MapControl peut ne plus afficher les mappages et les API dans l’espace de noms Windows. services. Maps ne peuvent pas retourner de résultats :
> - Windows 10, version 1607 et versions antérieures : les services cartographiques ne seront pas disponibles dans le monde entier à partir du 2020 octobre
> - Windows 10, version 1703 et versions antérieures : les services cartographiques ne sont pas disponibles sur [certains appareils vendus en Chine](/windows-hardware/customize/desktop/unattend/microsoft-windows-mapcontrol-desktop-chinavariantwin10)

Votre [application Windows universelle](../get-started/universal-application-platform-guide.md) doit être authentifiée avant de pouvoir utiliser les services [**collection MapControl**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl) et map dans l’espace de noms [**Windows. services. Maps**](/uwp/api/Windows.Services.Maps) . Pour authentifier votre application, vous devez spécifier une clé d’authentification de cartes. Cette rubrique décrit comment demander une clé d’authentification de cartes au [Centre de développement Bing Cartes](https://www.bingmapsportal.com/) et comment l’ajouter à votre application.

**Conseil** Pour en savoir plus sur l’utilisation de cartes dans votre application, téléchargez l’exemple suivant à partir du [référentiel Windows-universal-samples](https://github.com/Microsoft/Windows-universal-samples) sur GitHub :

-   [Exemple de carte pour UWP (plateforme Windows universelle)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MapControl)

## <a name="get-a-key"></a>Obtenir une clé


Le [Centre de développement Bing Cartes](https://www.bingmapsportal.com/) permet de créer et de gérer des clés d’authentification de cartes pour vos applications universelles Windows.

Pour créer une clé

1.  Dans votre navigateur, accédez au centre de développement Bing Maps ( [https://www.bingmapsportal.com](https://www.bingmapsportal.com/) ).

2.  Si vous êtes invité à vous connecter, entrez votre compte Microsoft, puis cliquez sur **Se connecter**.

3.  Choisissez le compte à associer à votre compte Bing Cartes. Si vous voulez utiliser votre compte Microsoft, cliquez sur **Oui**. Sinon, cliquez sur **Se connecter avec un autre compte**.

4.  Si vous n’avez pas encore de compte Bing Cartes, créez-en un. Renseignez les champs suivants : **Nom du compte**, **Nom du contact**, **Nom de la société**, **Adresse e-mail** et **Numéro de téléphone**. Après avoir accepté les conditions d’utilisation, cliquez sur **Créer**.

5.  Dans le menu **mon compte** , cliquez sur **mes clés**.

6.  Si vous avez déjà créé une clé, cliquez sur le lien pour créer une nouvelle clé. Sinon, passez au formulaire créer une clé.

7.  Complétez le formulaire **Créer une clé**, puis cliquez sur **Créer**.

    -   **Nom de l’application :** le nom de votre application.
    -   **URL de l’application (facultatif) :** l’URL de votre application.
    -   **Type de clé :** sélectionnez **De base** ou **Entreprise**.
    -   **Type d’application :** Sélectionnez **application Windows** pour une utilisation dans votre application Windows universelle.

    Voici un exemple de formulaire :

    ![Exemple du formulaire Créer une clé.](images/createkeydialog.png)

8.  Une fois que vous avez cliqué sur **Créer**, la nouvelle clé s’affiche en dessous du formulaire **Créer une clé**. Copiez-la dans un emplacement sûr ou ajoutez-la immédiatement à votre application, comme indiqué à l’étape suivante.

## <a name="add-the-key-to-your-app"></a>Ajouter la clé à votre application


La clé d’authentification de mappage est nécessaire pour utiliser les [**collection MapControl**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl) et les services de mappage ([**Windows. services. Maps**](/uwp/api/Windows.Services.Maps)) dans votre application Windows universelle. Le cas échéant, ajoutez-la aux objets contrôle de carte et services de carte.

### <a name="to-add-the-key-to-a-map-control"></a>Pour ajouter la clé à un contrôle de carte

Pour authentifier le [**collection MapControl**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl), définissez la propriété [**MapServiceToken**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.mapservicetoken) sur la valeur de la clé d’authentification. Vous pouvez définir cette propriété dans le code ou dans le balisage XAML, en fonction de vos préférences. Pour plus d’informations sur l’utilisation du **MapControl**, voir [Afficher des cartes avec des vues 2D, 3D et Streetside](display-maps.md).

-   Cet exemple définit le **MapServiceToken** sur la valeur de la clé d’authentification dans le code.

    ```cs
    MapControl1.MapServiceToken = "abcdef-abcdefghijklmno";
    ```

-   Cet exemple définit le **MapServiceToken** sur la valeur de la clé d’authentification dans le balisage XAML.

    ```xml
    <Maps:MapControl x:Name="MapControl1" MapServiceToken="abcdef-abcdefghijklmno"/>
    ```

### <a name="to-add-the-key-to-map-services"></a>Pour ajouter la clé aux services cartographiques

Pour utiliser les services dans l’espace de noms [**Windows.Services.Maps**](/uwp/api/Windows.Services.Maps), définissez la propriété [**ServiceToken**](/uwp/api/windows.services.maps.mapservice.servicetoken) sur la valeur de clé d’authentification. Pour plus d’informations sur l’utilisation des services cartographiques, voir [Afficher des itinéraires et indications](routes-and-directions.md) et [Effectuer un géocodage et un géocodage inverse](geocoding.md).

-   Cet exemple définit le **ServiceToken** sur la valeur de la clé d’authentification dans le code.

    ```cs
    MapService.ServiceToken = "abcdef-abcdefghijklmno";
    ```

## <a name="related-topics"></a>Rubriques connexes

* [Espace partenaires Bing Cartes](https://www.bingmapsportal.com/)
* [Exemple de carte UWP](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MapControl)
* [Recommandations de conception pour les cartes](./display-maps.md)
* [Vidéos de la build 2015 : utilisation de cartes et de localisation sur un téléphone, une tablette et un PC dans vos applications Windows](https://channel9.msdn.com/Events/Build/2015/2-757)
* [Exemple d’application de trafic UWP](https://github.com/Microsoft/Windows-appsample-trafficapp)