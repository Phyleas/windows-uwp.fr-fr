---
ms.assetid: 7CC11888-8DC6-4FEE-ACED-9FA476B2125E
description: Utilisez l’API Microsoft Store soumission pour créer et gérer par programmation des soumissions pour les applications qui sont inscrites auprès de votre compte espace partenaires.
title: Créer et gérer des soumissions
ms.date: 06/04/2018
ms.topic: article
keywords: API de soumission de Microsoft Store Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 38a59db4115332a374c96c8a4400dbaccff9cd82
ms.sourcegitcommit: 720413d2053c8d5c5b34d6873740be6e913a4857
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/25/2020
ms.locfileid: "88846819"
---
# <a name="create-and-manage-submissions"></a>Créer et gérer des soumissions

Utilisez l' *API de soumission Microsoft Store* pour interroger par programme et créer des soumissions pour les applications, les modules complémentaires et les vols de packages pour le compte de l’espace partenaires de votre organisation ou de votre organisation. Cette API est utile si votre compte gère beaucoup d’applications ou d’extensions et que vous voulez automatiser et optimiser le processus de soumission de ces ressources. Cette API utilise Azure Active Directory (Azure AD) pour authentifier les appels en provenance de votre application ou service.

Les étapes suivantes décrivent le processus de bout en bout de l’utilisation de l’API de soumission Microsoft Store :

1.  Vérifiez que vous avez rempli toutes les [conditions préalables](#prerequisites).
3.  Avant d’appeler une méthode dans l’API de soumission Microsoft Store, [obtenez un jeton d’accès Azure ad](#obtain-an-azure-ad-access-token). Une fois que vous avez obtenu un jeton, vous avez 60 minutes pour utiliser ce jeton dans les appels à l’API de soumission Microsoft Store avant l’expiration du jeton. Une fois le jeton arrivé à expiration, vous pouvez en générer un nouveau.
4.  [Appelez l’API de soumission Microsoft Store](#call-the-windows-store-submission-api).

<span id="not_supported" />

> [!IMPORTANT]
> Si vous utilisez cette API pour créer une soumission pour une application, un vol de package ou un module complémentaire, veillez à apporter d’autres modifications à l’envoi uniquement à l’aide de l’API, plutôt que dans l’espace partenaires. Si vous utilisez l’espace partenaires pour modifier une soumission créée à l’origine à l’aide de l’API, vous ne pourrez plus modifier ou valider cette soumission à l’aide de l’API. Dans certains cas, la soumission peut être laissée dans un état d’erreur où elle ne peut pas continuer dans le processus de soumission. Si cela se produit, vous devez supprimer l’envoi et créer une nouvelle soumission.

> [!IMPORTANT]
> Vous ne pouvez pas utiliser cette API pour publier des soumissions pour des [achats en volume via le Microsoft Store pour l’entreprise et Microsoft Store pour l’éducation](../publish/organizational-licensing.md) ou pour publier des soumissions pour les [applications métier](../publish/distribute-lob-apps-to-enterprises.md) directement aux entreprises. Pour ces deux scénarios, vous devez utiliser publier l’envoi dans l’espace partenaires.

> [!NOTE]
> Cette API ne peut pas être utilisée avec des applications ou des modules complémentaires qui utilisent des mises à jour d’application obligatoires et des modules complémentaires consommables gérés par la Banque. Si vous utilisez l’API de soumission Microsoft Store avec une application ou un module complémentaire qui utilise l’une de ces fonctionnalités, l’API renverra un code d’erreur 409. Dans ce cas, vous devez utiliser l’espace partenaires pour gérer les soumissions de l’application ou du module complémentaire.

<span id="prerequisites" />

## <a name="step-1-complete-prerequisites-for-using-the-microsoft-store-submission-api"></a>Étape 1 : remplir les conditions préalables à l’utilisation de l’API de soumission Microsoft Store

Avant de commencer à écrire du code pour appeler l’API de soumission Microsoft Store, assurez-vous que vous avez rempli les conditions préalables suivantes.

* Vous (ou votre organisation) devez disposer d’un annuaire Azure AD et de l’autorisation [Administrateur général](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles) sur l’annuaire. Si vous utilisez déjà Microsoft 365 ou d’autres services professionnels de Microsoft, vous disposez déjà d’un annuaire Azure AD. Dans le cas contraire, vous pouvez [créer un nouveau Azure ad dans l’espace partenaires](../publish/associate-azure-ad-with-partner-center.md#create-a-brand-new-azure-ad-to-associate-with-your-partner-center-account) sans frais supplémentaires.

* Vous devez [associer une application Azure AD à votre compte Espace partenaires](#associate-an-azure-ad-application-with-your-windows-partner-center-account) et récupérer votre ID tenant, votre ID client et votre clé. Ces valeurs sont nécessaires pour obtenir un jeton d’accès Azure AD, que vous utiliserez dans les appels à l’API de soumission au Microsoft Store.

* Préparez votre application pour une utilisation avec l’API de soumission Microsoft Store :

  * Si votre application n’existe pas encore dans l’espace partenaires, vous devez la [créer en réservant son nom dans l’espace partenaires](https://docs.microsoft.com/windows/uwp/publish/create-your-app-by-reserving-a-name). Vous ne pouvez pas utiliser l’API de soumission Microsoft Store pour créer une application dans l’espace partenaires. vous devez travailler dans l’espace partenaires pour le créer, puis, après cela, vous pouvez utiliser l’API pour accéder à l’application et créer par programme des soumissions pour celle-ci. En revanche, vous pouvez vous servir de l’API pour créer des extensions et des versions d’évaluation de package par programmation avant de créer des soumissions pour ces ressources.

  * Avant de pouvoir créer une soumission pour une application donnée à l’aide de cette API, vous devez d’abord [créer une soumission pour l’application dans l’espace partenaires](https://docs.microsoft.com/windows/uwp/publish/app-submissions), y compris pour répondre au questionnaire sur les évaluations de l' [âge](https://docs.microsoft.com/windows/uwp/publish/age-ratings) . Après quoi, vous pourrez créer des soumissions par programmation pour cette application à l’aide de l’API. Vous n’avez pas besoin de créer une soumission d’extension ou de version d’évaluation de package avant d’utiliser l’API pour ces types de soumission.

  * Si vous créez ou mettez à jour une soumission d’application et que vous devez inclure un package d’application, [préparez le package d’application](https://docs.microsoft.com/windows/uwp/publish/app-package-requirements).

  * Si vous créez ou mettez à jour une soumission d’application et que vous devez inclure des captures d’écran ou des images à des fins de présentation sur le Store, [préparez les captures d’écran et les images de l’application](https://docs.microsoft.com/windows/uwp/publish/app-screenshots-and-images).

  * Si vous créez ou mettez à jour une soumission d’extension et que vous devez inclure une icône, [préparez le package l’icône](https://docs.microsoft.com/windows/uwp/publish/create-iap-descriptions).

<span id="associate-an-azure-ad-application-with-your-windows-partner-center-account" />

### <a name="how-to-associate-an-azure-ad-application-with-your-partner-center-account"></a>Associer une application Azure AD à un compte Espace partenaires

Avant de pouvoir utiliser l’API de soumission Microsoft Store, vous devez associer une application Azure AD à votre compte espace partenaires, récupérer l’ID de locataire et l’ID client pour l’application et générer une clé. L’application Azure AD représente l’application ou le service à partir duquel vous souhaitez appeler l’API de soumission Microsoft Store. Il vous faut l’ID tenant, l’ID client et la clé pour obtenir un jeton d’accès Azure AD à transmettre à l’API.

> [!NOTE]
> Vous ne devez effectuer cette tâche qu’une seule fois. Une fois que vous les avez, vous pouvez réutiliser l’ID tenant, l’ID client et la clé chaque fois que vous devez créer un jeton d’accès Azure AD.

1.  Dans l’Espace partenaires, [associez le compte Espace partenaires de votre organisation à l’annuaire Azure AD de votre organisation](../publish/associate-azure-ad-with-partner-center.md).

2.  Ensuite, sur la page **Utilisateurs** de la section **Paramètres de compte** de l’Espace partenaires, [ajoutez l’application Azure AD](../publish/add-users-groups-and-azure-ad-applications.md#add-azure-ad-applications-to-your-partner-center-account) représentant l’application ou le service que vous utiliserez pour accéder aux soumissions de votre compte Espace partenaires. Veillez à attribuer à cette application le rôle **Gestionnaire**. Si l’application n’existe pas encore dans votre annuaire Azure AD, vous pouvez [créer une application Azure AD dans l’Espace partenaires](../publish/add-users-groups-and-azure-ad-applications.md#create-a-new-azure-ad-application-account-in-your-organizations-directory-and-add-it-to-your-partner-center-account).  

3.  Revenez à la page **Utilisateurs**, cliquez sur le nom de votre application Azure AD pour accéder à ses paramètres, puis copiez les valeurs **ID tenant** et **ID client**.

4. Cliquez sur **Ajouter une nouvelle clé**. Sur l’écran suivant, copiez la valeur **Clé**. Vous ne pourrez plus accéder à cette information une fois que vous aurez quitté cette page. Pour plus d’informations, voir [Gérer les clés pour une application Azure AD](../publish/add-users-groups-and-azure-ad-applications.md#manage-keys).

<span id="obtain-an-azure-ad-access-token" />

## <a name="step-2-obtain-an-azure-ad-access-token"></a>Étape 2 : Obtenir un jeton d’accès Azure AD

Avant d’appeler l’une des méthodes de l’API de soumission Microsoft Store, vous devez d’abord obtenir un jeton d’accès Azure AD que vous transmettez à l’en-tête **authorization** de chaque méthode de l’API. Une fois que vous avez récupéré le jeton d’accès, vous avez 60 minutes pour l’utiliser avant qu’il n’expire. Une fois le jeton arrivé à expiration, vous pouvez l’actualiser pour pouvoir continuer à l’utiliser dans d’autres appels à l’API.

Pour obtenir le jeton d’accès, suivez les instructions présentées dans l’article [Appels de service à service à l’aide des informations d’identification du client](https://azure.microsoft.com/documentation/articles/active-directory-protocols-oauth-service-to-service/) pour envoyer une requête HTTP POST au point de terminaison ```https://login.microsoftonline.com/<tenant_id>/oauth2/token```. Voici un exemple de requête.

```json
POST https://login.microsoftonline.com/<tenant_id>/oauth2/token HTTP/1.1
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded; charset=utf-8

grant_type=client_credentials
&client_id=<your_client_id>
&client_secret=<your_client_secret>
&resource=https://manage.devcenter.microsoft.com
```

Pour la *valeur \_ ID de locataire* dans l’URI de publication et les paramètres * \_ ID client* et clé * \_ secrète client* , spécifiez l’ID de locataire, l’ID client et la clé de votre application que vous avez récupéré dans l’espace partenaires dans la section précédente. Pour le paramètre *resource*, vous devez spécifier ```https://manage.devcenter.microsoft.com```.

Une fois votre jeton d’accès arrivé à expiration, vous pouvez l’actualiser en suivant les instructions fournies [ici](https://azure.microsoft.com/documentation/articles/active-directory-protocols-oauth-code/#refreshing-the-access-tokens).

Pour obtenir des exemples qui montrent comment obtenir un jeton d’accès à l’aide de code C#, Java ou python, consultez [exemples de code](#code-examples)de l’API de soumission Microsoft Store.

<span id="call-the-windows-store-submission-api">

## <a name="step-3-use-the-microsoft-store-submission-api"></a>Étape 3 : Utiliser l’API de soumission au Microsoft Store

Une fois que vous disposez d’un jeton d’accès Azure AD, vous pouvez appeler des méthodes dans l’API de soumission Microsoft Store. L’API propose diverses méthodes qui sont regroupées dans des scénarios pour applications, extensions et versions d’essai de package. Pour créer ou mettre à jour des envois, vous appelez généralement plusieurs méthodes dans l’API Microsoft Store soumission dans un ordre spécifique. Pour plus d’informations sur chaque scénario et sur la syntaxe de chacune de ces méthodes, voir les articles indiqués dans le tableau suivant.

> [!NOTE]
> Une fois que vous avez obtenu un jeton d’accès, vous avez 60 minutes pour appeler les méthodes de l’API d’envoi Microsoft Store avant l’expiration du jeton.

| Scénario       | Description                                                                 |
|---------------|----------------------------------------------------------------------|
| Apps |  Récupérez les données de toutes les applications qui sont inscrites auprès de votre compte espace partenaires et créez des soumissions pour les applications. Pour plus d’informations sur ces méthodes, voir les articles suivants : <ul><li>[Obtenir des données d’application](get-app-data.md)</li><li>[Gérer les soumissions d’applications](manage-app-submissions.md)</li></ul> |
| Modules complémentaires | Obtient, crée ou supprime des extensions pour vos applications, puis obtient, crée ou supprime des soumissions pour les extensions. Pour plus d’informations sur ces méthodes, voir les articles suivants : <ul><li>[Gérer les extensions](manage-add-ons.md)</li><li>[Gérer les soumissions d’extensions](manage-add-on-submissions.md)</li></ul> |
| Versions d’évaluation de package | Obtient, crée ou supprime des versions d’évaluation de package pour vos applications, puis obtient, crée ou supprime des soumissions pour les versions d’évaluation de package. Pour plus d’informations sur ces méthodes, voir les articles suivants : <ul><li>[Gérer les versions d’évaluation de package](manage-flights.md)</li><li>[Gérer les soumissions de versions d’évaluation de package](manage-flight-submissions.md)</li></ul> |

<span id="code-samples"/>

## <a name="code-examples"></a>Exemples de code

Les articles suivants fournissent des exemples de code détaillés qui montrent comment utiliser l’API de soumission Microsoft Store dans différents langages de programmation :

* [Exemple de code C# : soumissions d’applications, d’extensions et de versions d’évaluation](csharp-code-examples-for-the-windows-store-submission-api.md)
* [Exemple de code C# : soumission d’applications avec options de jeu et bandes-annonces](csharp-code-examples-for-submissions-game-options-and-trailers.md)
* [Exemple de code Java : soumissions d’applications, d’extensions et de versions d’évaluation](java-code-examples-for-the-windows-store-submission-api.md)
* [Exemple de code Java : soumission d’applications avec options de jeu et bandes-annonces](java-code-examples-for-submissions-game-options-and-trailers.md)
* [Exemple de code Python : soumissions d’applications, d’extensions et de versions d’évaluation](python-code-examples-for-the-windows-store-submission-api.md)
* [Exemple de code Python : soumission d’applications avec options de jeu et bandes-annonces](python-code-examples-for-submissions-game-options-and-trailers.md)

## <a name="storebroker-powershell-module"></a>Module PowerShell StoreBroker

Au lieu d’appeler directement l’API de soumission Microsoft Store, nous fournissons également un module PowerShell Open source qui implémente une interface de ligne de commande en plus de l’API. Ce module est appelé [StoreBroker](https://github.com/Microsoft/StoreBroker). Vous pouvez utiliser ce module pour gérer vos envois d’application, de vol et de module complémentaire à partir de la ligne de commande au lieu d’appeler directement l’API de soumission Microsoft Store, ou vous pouvez simplement parcourir la source pour voir d’autres exemples sur l’appel de cette API. Le module StoreBroker est utilisé activement au sein de Microsoft comme méthode principale que de nombreuses applications de première partie sont soumises au magasin.

Pour plus d’informations, consultez notre [page StoreBroker sur GitHub](https://github.com/Microsoft/StoreBroker).

## <a name="troubleshooting"></a>Résolution des problèmes

| Problème      | Résolution                                          |
|---------------|---------------------------------------------|
| Après avoir appelé l’API de soumission Microsoft Store à partir de PowerShell, les données de réponse pour l’API sont endommagées si vous la convertissez au format JSON en un objet PowerShell à l’aide de l’applet de commande [ConvertFrom-JSON](https://docs.microsoft.com/powershell/module/5.1/microsoft.powershell.utility/ConvertFrom-Json) , puis revenez au format JSON à l’aide de l’applet [de commande ConvertTo-JSON](https://docs.microsoft.com/powershell/module/5.1/microsoft.powershell.utility/ConvertTo-Json) . |  Par défaut, le paramètre *-Depth* de l’applet de commande [ConvertTo-JSON](https://docs.microsoft.com/powershell/module/5.1/microsoft.powershell.utility/ConvertTo-Json) est défini sur 2 niveaux d’objets, ce qui est trop superficiel pour la plupart des objets JSON retournés par l’API de soumission Microsoft Store. Quand vous appelez l’applet de commande [ConvertTo Json](https://docs.microsoft.com/powershell/module/5.1/microsoft.powershell.utility/ConvertTo-Json), attribuez au paramètre *-Depth* une valeur supérieure, par exemple 20. |

## <a name="additional-help"></a>Aide supplémentaire

Si vous avez des questions sur l’API de soumission Microsoft Store ou si vous avez besoin d’aide pour gérer vos envois avec cette API, utilisez les ressources suivantes :

* Posez vos questions sur nos [forums](https://social.msdn.microsoft.com/Forums/windowsapps/home?forum=wpsubmit).
* Visitez notre [page de support](https://developer.microsoft.com/windows/support) et demandez l’une des options de support assisté pour l’espace partenaires. Si vous êtes invité à choisir un type de problème et une catégorie, choisissez respectivement **Soumission d’application et certification** et **Soumission d’une application**.  

## <a name="related-topics"></a>Rubriques connexes

* [Obtenir des données d’application](get-app-data.md)
* [Gérer les soumissions d’applications](manage-app-submissions.md)
* [Gérer les extensions](manage-add-ons.md)
* [Gérer les soumissions d’extensions](manage-add-on-submissions.md)
* [Gérer les versions d’évaluation de package](manage-flights.md)
* [Gérer les soumissions de versions d’évaluation de package](manage-flight-submissions.md)
