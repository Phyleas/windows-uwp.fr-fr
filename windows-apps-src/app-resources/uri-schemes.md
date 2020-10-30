---
description: Vous pouvez utiliser plusieurs schémas d’URI (Uniform Resource Identifier) pour faire référence à des fichiers qui proviennent de votre package d’application, des dossiers de données de votre application ou du cloud. Vous pouvez également utiliser un schéma d’URI pour faire référence à des chaînes chargées à partir des fichiers de ressources (.resw) de votre application.
title: Schémas d’URI
template: detail.hbs
ms.date: 10/16/2017
ms.topic: article
keywords: windows 10, uwp, ressources, image, MRT, qualificateur
ms.localizationpriority: medium
ms.openlocfilehash: 8806992ebb7f4335ca0a748c1b2bce4a6de39fae
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93031543"
---
# <a name="uri-schemes"></a>Schémas d’URI

Vous pouvez utiliser plusieurs schémas d’URI (Uniform Resource Identifier) pour faire référence à des fichiers qui proviennent de votre package d’application, des dossiers de données de votre application ou du cloud. Vous pouvez également utiliser un schéma d’URI pour faire référence à des chaînes chargées à partir des fichiers de ressources (.resw) de votre application. Vous pouvez utiliser ces schémas d’URI dans votre code, dans votre balisage XAML, dans votre manifeste de package d’application, ou dans vos modèles de notification de vignette et de Toast.

## <a name="common-features-of-the-uri-schemes"></a>Fonctionnalités courantes des schémas d’URI

Tous les schémas décrits dans cette rubrique suivent les règles de modèle d’URI typiques pour la normalisation et la récupération des ressources. Consultez la [RFC 3986](https://www.ietf.org/rfc/rfc3986.txt) pour connaître la syntaxe générique d’un URI.

Tous les schémas d’URI définissent la partie hiérarchique par [RFC 3986](https://www.ietf.org/rfc/rfc3986.txt) en tant que composants d’autorité et de chemin d’accès de l’URI.

```syntax
URI         = scheme ":" hier-part [ "?" query ] [ "#" fragment ]
hier-part   = "//" authority path-abempty
            / path-absolute
            / path-rootless
            / path-empty
```

Cela signifie qu’il existe essentiellement trois composants à un URI. Le fait de suivre immédiatement les deux barres obliques du *schéma* d’URI est un composant (qui peut être vide) appelé *autorité* . Et immédiatement après cela est le *chemin d’accès* . En utilisant l’URI `http://www.contoso.com/welcome.png` comme exemple, le schéma est « `http://` », l’autorité est « `www.contoso.com` » et le chemin d’accès est « `/welcome.png` ». Un autre exemple est l’URI `ms-appx:///logo.png` , où les composants d’autorité sont vides et prennent une valeur par défaut.

Le composant fragment est ignoré par le traitement propre au schéma des URI mentionnés dans cette rubrique. Pendant la récupération et la comparaison des ressources, le composant de fragment n’a pas de palier. Toutefois, les couches au-dessus de l’implémentation spécifique peuvent interpréter le fragment pour récupérer une ressource secondaire.

La comparaison se produit octet pour Byte après la normalisation de tous les composants IRI.

## <a name="case-insensitivity-and-normalization"></a>Non-respect de la casse et normalisation

Tous les schémas d’URI décrits dans cette rubrique suivent les règles URI typiques (RFC 3986) pour la normalisation et la récupération des ressources pour les schémas. La forme normalisée de ces URI gère la casse et le pourcentage de décodage des caractères non réservés RFC 3986.

Pour tous les schémas d’URI décrits dans cette rubrique, le *schéma* , l' *autorité* et le *chemin d’accès* ne sont pas sensibles à la casse par standard, ou sont traités par le système sans respect de la casse. **Remarque** La seule exception à cette règle est l' *autorité* de qui respecte la `ms-resource` casse.

## <a name="ms-appx-and-ms-appx-web"></a>MS-AppX et MS-AppX-Web

Utilisez le `ms-appx` `ms-appx-web` modèle URI ou pour faire référence à un fichier qui provient du package de votre application (consultez [empaquetage d’applications](../packaging/index.md)). Les fichiers de votre package d’application sont généralement des images statiques, des données, du code et des fichiers de disposition. Le `ms-appx-web` schéma accède aux mêmes fichiers que `ms-appx` , mais dans le compartiment Web. Pour obtenir des exemples et des informations supplémentaires, consultez [référencer une image ou une autre ressource à partir du balisage et du code XAML](images-tailored-for-scale-theme-contrast.md#reference-an-image-or-other-asset-from-xaml-markup-and-code).

### <a name="scheme-name-ms-appx-and-ms-appx-web"></a>Nom du schéma (MS-AppX et MS-AppX-Web)

Le nom du modèle d’URI est la chaîne « MS-AppX » ou « MS-AppX-Web ».

```xml
ms-appx://
```

```xml
ms-appx-web://
```

### <a name="authority-ms-appx-and-ms-appx-web"></a>Autorité (MS-AppX et MS-AppX-Web)

L’autorité est le nom de l’identité du package qui est défini dans le manifeste du package. Elle est donc limitée à la fois dans l’URI et dans le formulaire IRI (International Resource Identifier) à l’ensemble de caractères autorisé dans un nom d’identité de package. Le nom du package doit être le nom de l’un des packages dans le graphique des dépendances du package de l’application en cours d’exécution.

```xml
ms-appx://Contoso.MyApp/
ms-appx-web://Contoso.MyApp/
```

Si un autre caractère apparaît dans l’autorité, la récupération et la comparaison échouent. La valeur par défaut de l’autorité est le package de l’application en cours d’exécution.

```xml
ms-appx:///
ms-appx-web:///
```

### <a name="user-info-and-port-ms-appx-and-ms-appx-web"></a>Informations utilisateur et port (MS-AppX et MS-AppX-Web)

`ms-appx`Contrairement à d’autres schémas populaires, le schéma ne définit pas un composant d’informations utilisateur ou de port. Comme « @" and " : » ne sont pas autorisés en tant que valeurs d’autorité valides, la recherche échoue si elles sont incluses. Chacun des éléments suivants échoue.

```xml
ms-appx://john@contoso.myapp/default.html
ms-appx://john:password@contoso.myapp/default.html
ms-appx://contoso.myapp:8080/default.html
ms-appx://john:password@contoso.myapp:8080/default.html
```

### <a name="path-ms-appx-and-ms-appx-web"></a>Chemin d’accès (MS-AppX et MS-AppX-Web)

Le composant Path correspond à la syntaxe RFC 3986 générique et prend en charge les caractères non-ASCII dans IRIs. Le composant Path définit le chemin logique ou physique d’un fichier. Ce fichier se trouve dans un dossier associé à l’emplacement d’installation du package d’application, pour l’application spécifiée par l’autorité.

Si le chemin d’accès fait référence à un chemin d’accès physique et à un nom de fichier, cette ressource de fichier physique est récupérée. Toutefois, si aucun fichier physique de ce type n’est trouvé, la ressource réelle retournée pendant la récupération est déterminée à l’aide de la négociation de contenu au moment de l’exécution. Cette détermination est basée sur les paramètres de l’application, du système d’exploitation et de l’utilisateur, tels que la langue, l’affichage du facteur d’échelle, du thème, du contraste élevé et d’autres contextes d’exécution. Par exemple, une combinaison des langues de l’application, des paramètres d’affichage du système et des paramètres de contraste élevé de l’utilisateur peut être prise en compte lors de la détermination de la valeur réelle de la ressource à récupérer.

```xml
ms-appx:///images/logo.png
```

L’URI ci-dessus peut en fait récupérer un fichier dans le package de l’application actuelle avec le nom de fichier physique suivant.

<blockquote>
<pre>
\Images\fr-FR\logo.scale-100_contrast-white.png
</blockquote>
</pre>

Vous pouvez également récupérer ce même fichier physique en y faisant référence directement par son nom complet.

```xaml
<Image Source="ms-appx:///images/fr-FR/logo.scale-100_contrast-white.png"/>
```

Le composant de chemin d’accès de `ms-appx(-web)` est, comme les URI génériques, sensible à la casse. Toutefois, lorsque le système de fichiers sous-jacent sur lequel la ressource est accédée ne respecte pas la casse, par exemple pour NTFS, la récupération de la ressource est effectuée sans respect de la casse.

La forme normalisée de l’URI conserve la casse, et le pourcentage de décodage (un symbole « % » suivi de la représentation hexadécimale à deux chiffres) des caractères non réservés RFC 3986. Les caractères «  ? », « # », « / », « * » et «» (guillemet double) doivent être encodés en pourcentage dans un chemin d’accès pour représenter des données telles que des noms de fichiers ou de dossiers. Tous les caractères encodés en pourcentage sont décodés avant la récupération. Par conséquent, pour récupérer un fichier nommé hello # World.html, utilisez cet URI.

```xml
ms-appx:///Hello%23World.html
```

### <a name="query-ms-appx-and-ms-appx-web"></a>Requête (MS-AppX et MS-AppX-Web)

Les paramètres de requête sont ignorés pendant la récupération des ressources. La forme normalisée des paramètres de requête conserve la casse. Les paramètres de requête ne sont pas ignorés pendant la comparaison.

## <a name="ms-appdata"></a>ms-appdata

Utilisez le `ms-appdata` schéma d’URI pour faire référence aux fichiers provenant des dossiers de données locaux, itinérants et temporaires de l’application. Pour plus d’informations sur ces dossiers de données d’application, consultez [stocker et récupérer des paramètres et d’autres données d’application](../design/app-settings/store-and-retrieve-app-data.md).

Le `ms-appdata` schéma d’URI n’effectue pas la négociation de contenu au moment de l’exécution que [MS-AppX et MS-AppX-Web](#ms-appx-and-ms-appx-web) font. Toutefois, vous pouvez répondre au contenu de [ResourceContext. QualifierValues](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.QualifierValues) et charger les ressources appropriées à partir des données d’application à l’aide de leur nom de fichier physique complet dans l’URI.

### <a name="scheme-name-ms-appdata"></a>Nom du schéma (MS-AppData)

Le nom du schéma d’URI est la chaîne « MS-AppData ».

```xml
ms-appdata://
```

### <a name="authority-ms-appdata"></a>Autorité (MS-AppData)

L’autorité est le nom de l’identité du package qui est défini dans le manifeste du package. Elle est donc limitée à la fois dans l’URI et dans le formulaire IRI (International Resource Identifier) à l’ensemble de caractères autorisé dans un nom d’identité de package. Le nom du package doit être le nom du package de l’application en cours d’exécution.

```xml
ms-appdata://Contoso.MyApp/
```

Si un autre caractère apparaît dans l’autorité, la récupération et la comparaison échouent. La valeur par défaut de l’autorité est le package de l’application en cours d’exécution.

```xml
ms-appdata:///
```

### <a name="user-info-and-port-ms-appdata"></a>Informations utilisateur et port (MS-AppData)

`ms-appdata`Contrairement à d’autres schémas populaires, le schéma ne définit pas un composant d’informations utilisateur ou de port. Comme « @" and " : » ne sont pas autorisés en tant que valeurs d’autorité valides, la recherche échoue si elles sont incluses. Chacun des éléments suivants échoue.

```xml
ms-appdata://john@contoso.myapp/local/data.xml
ms-appdata://john:password@contoso.myapp/local/data.xml
ms-appdata://contoso.myapp:8080/local/data.xml
ms-appdata://john:password@contoso.myapp:8080/local/data.xml
```

### <a name="path-ms-appdata"></a>Chemin d’accès (MS-AppData)

Le composant Path correspond à la syntaxe RFC 3986 générique et prend en charge les caractères non-ASCII dans IRIs. Dans l’emplacement [Windows. Storage. ApplicationData](/uwp/api/Windows.Storage.ApplicationData?branch=live) se trouvent trois dossiers réservés pour le stockage d’état local, itinérant et temporaire. Le `ms-appdata` schéma autorise l’accès aux fichiers et aux dossiers dans ces emplacements. Le premier segment du composant de chemin d’accès doit spécifier le dossier particulier de la manière suivante. Par conséquent, le format « chemin d’accès vide » de « hier-part » n’est pas légal.

Dossier local.

```xml
ms-appdata:///local/
```

Dossier temporaire.

```xml
ms-appdata:///temp/
```

Dossier d’itinérance.

```xml
ms-appdata:///roaming/
```

Le composant de chemin d’accès de `ms-appdata` est, comme les URI génériques, sensible à la casse. Toutefois, lorsque le système de fichiers sous-jacent sur lequel la ressource est accédée ne respecte pas la casse, par exemple pour NTFS, la récupération de la ressource est effectuée sans respect de la casse.

La forme normalisée de l’URI conserve la casse, et le pourcentage de décodage (un symbole « % » suivi de la représentation hexadécimale à deux chiffres) des caractères non réservés RFC 3986. Les caractères «  ? », « # », « / », « * » et «» (guillemet double) doivent être encodés en pourcentage dans un chemin d’accès pour représenter des données telles que des noms de fichiers ou de dossiers. Tous les caractères encodés en pourcentage sont décodés avant la récupération. Par conséquent, pour récupérer un fichier local nommé hello # World.html, utilisez cet URI.

```xml
ms-appdata://local/Hello%23World.html
```

La récupération de la ressource, ainsi que l’identification du segment de chemin d’accès de niveau supérieur, sont gérées après la normalisation des points («.. /./b/c"). Par conséquent, les URI ne peuvent pas se déconnecter de l’un des dossiers réservés. Ainsi, l’URI suivant n’est pas autorisé.

```xml
ms-appdata:///local/../hello/logo.png
```

Mais cet URI est autorisé (quoique redondant).

```xml
ms-appdata:///local/../roaming/logo.png
```

### <a name="query-ms-appdata"></a>Requête (MS-AppData)

Les paramètres de requête sont ignorés pendant la récupération des ressources. La forme normalisée des paramètres de requête conserve la casse. Les paramètres de requête ne sont pas ignorés pendant la comparaison.

## <a name="ms-resource"></a>MS-Resource

Utilisez le `ms-resource` schéma d’URI pour faire référence aux chaînes chargées à partir des fichiers de ressources de votre application (. resw). Pour obtenir des exemples et des informations supplémentaires sur les fichiers de ressources, consultez [localiser des chaînes dans votre interface utilisateur et le manifeste du package d’application](localize-strings-ui-manifest.md).

### <a name="scheme-name-ms-resource"></a>Nom du schéma (MS-Resource)

Le nom du schéma d’URI est la chaîne « MS-Resource ».

```xml
ms-resource://
```

### <a name="authority-ms-resource"></a>Autorité (MS-Resource)

L’autorité est la carte de ressources de niveau supérieur définie dans l’index de ressource de package (PRI), qui correspond généralement au nom d’identité du package défini dans le manifeste du package. Consultez [empaquetage d’applications](../packaging/index.md)). Elle est donc limitée à la fois dans l’URI et dans le formulaire IRI (International Resource Identifier) à l’ensemble de caractères autorisé dans un nom d’identité de package. Le nom du package doit être le nom de l’un des packages dans le graphique des dépendances du package de l’application en cours d’exécution.

```xml
ms-resource://Contoso.MyApp/
ms-resource://Microsoft.WinJS.1.0/
```

Si un autre caractère apparaît dans l’autorité, la récupération et la comparaison échouent. La valeur par défaut de l’autorité est le nom du package qui respecte la casse de l’application en cours d’exécution.

```xml
ms-resource:///
```

L’autorité respecte la casse et la forme normalisée conserve son cas. Toutefois, la recherche d’une ressource ne respecte pas la casse.

### <a name="user-info-and-port-ms-resource"></a>Informations utilisateur et port (MS-Resource)

`ms-resource`Contrairement à d’autres schémas populaires, le schéma ne définit pas un composant d’informations utilisateur ou de port. Comme « @" and " : » ne sont pas autorisés en tant que valeurs d’autorité valides, la recherche échoue si elles sont incluses. Chacun des éléments suivants échoue.

```xml
ms-resource://john@contoso.myapp/Resources/String1
ms-resource://john:password@contoso.myapp/Resources/String1
ms-resource://contoso.myapp:8080/Resources/String1
ms-resource://john:password@contoso.myapp:8080/Resources/String1
```

### <a name="path-ms-resource"></a>Chemin d’accès (MS-Resource)

Le chemin d’accès identifie l’emplacement hiérarchique de la sous-arborescence [ResourceMap](/uwp/api/Windows.ApplicationModel.Resources.Core.ResourceMap?branch=live) (consultez le [système de gestion des ressources](/previous-versions/windows/apps/jj552947(v=win.10))) et le [NamedResource](/uwp/api/Windows.ApplicationModel.Resources.Core.NamedResource?branch=live) qu’il contient. En général, il correspond au nom de fichier (extension) des fichiers de ressources (. resw) et à l’identificateur d’une ressource de chaîne qu’il contient.

Pour obtenir des exemples et plus d’informations, consultez [localiser des chaînes dans votre interface utilisateur et le manifeste du package d’application](localize-strings-ui-manifest.md) , ainsi que [prise en charge des notifications par vignette et toast pour le langage,](../design/shell/tiles-and-notifications/tile-toast-language-scale-contrast.md)la mise à l’échelle et le contraste élevé.

Le composant de chemin d’accès de `ms-resource` est, comme les URI génériques, sensible à la casse. Toutefois, la récupération sous-jacente fait un [comparestringordinal,](/windows/desktop/api/winstring/nf-winstring-windowscomparestringordinal) avec *ignoreCase* défini sur `true` .

La forme normalisée de l’URI conserve la casse, et le pourcentage de décodage (un symbole « % » suivi de la représentation hexadécimale à deux chiffres) des caractères non réservés RFC 3986. Les caractères «  ? », « # », « / », « * » et «» (guillemet double) doivent être encodés en pourcentage dans un chemin d’accès pour représenter des données telles que des noms de fichiers ou de dossiers. Tous les caractères encodés en pourcentage sont décodés avant la récupération. Par conséquent, pour récupérer une ressource de type chaîne à partir d’un fichier de ressources nommé `Hello#World.resw` , utilisez cet URI.

```xml
ms-resource:///Hello%23World/String1
```

### <a name="query-ms-resource"></a>Requête (MS-Resource)

Les paramètres de requête sont ignorés pendant la récupération des ressources. La forme normalisée des paramètres de requête conserve la casse. Les paramètres de requête ne sont pas ignorés pendant la comparaison. Les paramètres de requête sont comparés en respectant la casse.

Les développeurs de composants particuliers superposés au-dessus de cette analyse URI peuvent choisir d’utiliser les paramètres de requête tels qu’ils s’affichent.

## <a name="related-topics"></a>Rubriques connexes

* [Uniform Resource Identifier (URI) : syntaxe générique](https://www.ietf.org/rfc/rfc3986.txt)
* [Empaquetage d’applications](../packaging/index.md)
* [Référencer une image ou une autre ressource à partir du balisage et du code XAML](images-tailored-for-scale-theme-contrast.md#reference-an-image-or-other-asset-from-xaml-markup-and-code)
* [Stocker et récupérer des paramètres et autres données d’application](../design/app-settings/store-and-retrieve-app-data.md)
* [Localiser les chaînes dans l’interface utilisateur et le manifeste du package d’application](localize-strings-ui-manifest.md)
* [Système de gestion des ressources](/previous-versions/windows/apps/jj552947(v=win.10))
* [Prise en charge des notifications par vignette et toast pour la langue, la mise à l’échelle et le contraste élevé](../design/shell/tiles-and-notifications/tile-toast-language-scale-contrast.md)
