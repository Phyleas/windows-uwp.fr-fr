---
author: mcleanbyron
ms.assetid: 48D891CD-706C-4759-AB33-B0663774A829
description: "Utilisez cette méthode dans l’API d’analyse du Windows Store pour télécharger le fichier CAB pour une erreur de pilote Windows7 ou Windows8.x. Cette méthode est uniquement destinée aux fabricants de matériel."
title: "Téléchargez le fichier CAB pour une erreur de pilote Windows 7 ou Windows8.x"
ms.author: mcleans
ms.date: 03/17/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows10, uwp, API d’analyse du Windows Store, téléchargement CAB"
ms.openlocfilehash: 622ad656a64381e80549bc286a4d30490e488631
ms.sourcegitcommit: 64cfb79fd27b09d49df99e8c9c46792c884593a7
translationtype: HT
---
# <a name="download-the-cab-file-for-a-windows-7-or-windows-8x-driver-error"></a>Téléchargez le fichier CAB pour une erreur de pilote Windows 7 ou Windows8.x

Utilisez cette méthode dans l’API d’analyse du Windows Store pour télécharger le fichier CAB associé à une erreur de pilote Windows7 ou Windows 8.x spécifique. Pour utiliser cette méthode, vous devez au préalable utiliser la méthode [obtenir des détails sur une erreur de pilote Windows7 ou Windows 8.x](get-details-for-a-windows-7-or-windows-8.x-driver-error.md) pour récupérer l’ID du fichier CAB à télécharger.

Vous pouvez obtenir d’autres informations sur les erreurs de pilote Windows7 et Windows8.x en utilisant les méthodes [Récupérer des données de rapport d'erreur pour les pilotes Windows7 et Windows8.x](get-error-reporting-data-for-windows-7-and-windows-8.x-drivers.md) et [Obtenir des détails sur une erreur de pilote Windows7 ou Windows 8.x](get-details-for-a-windows-7-or-windows-8.x-driver-error.md) dans l’API d’analyse du Windows Store.

> [!NOTE]
> Cette méthode peut uniquement être utilisée par les comptes de développeur qui participent au [programme du centre de développement matériel Windows](https://msdn.microsoft.com/windows/hardware/drivers/dashboard/get-started-with-the-hardware-dashboard).

## <a name="prerequisites"></a>Prérequis

Pour utiliser cette méthode, vous devez d’abord effectuer les opérations suivantes:

* Si ce n’est pas déjà fait, remplissez toutes les [conditions préalables](access-analytics-data-using-windows-store-services.md#prerequisites) relatives à l’API d’analyse du Windows Store.
* [Obtenez un jeton d’accès Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) à utiliser dans l’en-tête de requête de cette méthode. Après avoir obtenu un jeton d’accès, vous avez 60minutes pour l’utiliser avant expiration. Une fois le jeton arrivé à expiration, vous pouvez en obtenir un nouveau.
* Obtenez l’ID du fichier CAB à télécharger. Pour obtenir cet ID, utilisez la méthode [obtenir des détails sur une erreur de pilote Windows7 ou Windows 8.x](get-details-for-a-windows-7-or-windows-8.x-driver-error.md) pour récupérer les détails d’une erreur de pilote spécifique, en spécifiant la valeur **cabIdHash** dans le corps de la réponse.

## <a name="request"></a>Demande


### <a name="request-syntax"></a>Syntaxe de la requête

| Méthode | URI de la requête                                                          |
|--------|----------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/ihvdriver/cabdownload``` |

<span/> 

### <a name="request-header"></a>En-tête de requête

| En-tête        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | chaîne | Obligatoire. Jeton d’accès Azure AD sous la forme **Bearer** &lt;*jeton*&gt;. |

<span/> 

### <a name="request-parameters"></a>Paramètres de la requête

| Paramètre        | Type   |  Description      |  Obligatoire  |
|---------------|--------|---------------|------|
| cabIdHash | chaîne | L’ID unique du fichier CAB à télécharger. Pour obtenir cet ID, utilisez la méthode [obtenir des détails sur une erreur de pilote Windows7 ou Windows 8.x](get-details-for-a-windows-7-or-windows-8.x-driver-error.md) pour récupérer les détails d’une erreur spécifique dans votre application, en spécifiant la valeur **cabIdHash** dans le corps de la réponse. |  Oui  |

<span/>
 
### <a name="request-example"></a>Exemple de requête

L’exemple suivant montre comment télécharger un fichier CAB à l'aide de cette méthode.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/ihvdriver/cabdownload?cabIdHash=c1a51104-d682-4230-be14-7278b18e3555 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Réponse

Cette méthode renvoie un code de réponse 302 (redirection) et l'en-tête **Services de localisation** dans la réponse est attribué à l'URI de la signature d’accès partagé (SAS) du fichier CAB. L’appelant est redirigé vers cette URI pour télécharger automatiquement le fichier CAB.

## <a name="related-topics"></a>Rubriques associées

* [Récupérer des données de rapport d'erreur pour les pilotes Windows7 et Windows8.x](get-error-reporting-data-for-windows-7-and-windows-8.x-drivers.md)
* [Obtenir des informations sur une erreur de pilote Windows7 ou Windows 8.x](get-details-for-a-windows-7-or-windows-8.x-driver-error.md)