---
Description: Utilisez l’encodage de caractères UTF-8 pour une compatibilité optimale entre les applications Web et d’autres plateformes basées sur * Nix (UNIX, Linux et variantes), réduisez les bogues de localisation et réduisez la charge de test.
title: Utiliser la page de codes Windows UTF-8
template: detail.hbs
ms.date: 06/12/2019
ms.topic: article
keywords: windows 10, uwp, globalisation, adaptabilité, localisation
ms.localizationpriority: medium
ms.openlocfilehash: be3aade0289911f878d960fb62bde49b8ef840a8
ms.sourcegitcommit: 3a06cf3f8bd00e5e6eac3b38ee7e3c7cf4bc5197
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/24/2019
ms.locfileid: "72888744"
---
# <a name="use-the-utf-8-code-page"></a>Utiliser la page de codes UTF-8

Utilisez l’encodage de caractères [UTF-8](http://www.utf-8.com/) pour une compatibilité optimale entre les applications Web et d’autres plateformes basées sur * Nix (UNIX, Linux et variantes), réduisez les bogues de localisation et réduisez la charge de test.

UTF-8 est la page de codes universelle pour l’internationalisation et prend en charge tous les points de code Unicode à l’aide de l’encodage de largeur variable de 1-6 octets. Il est utilisé de façon omniprésente sur le Web et est la valeur par défaut pour les plateformes * nix.

## <a name="-a-vs--w-apis"></a>API-A et-W
  
Les API Win32 prennent souvent en charge les variantes-A et-W.

-Les variantes reconnaissent la page de codes ANSI configurée sur le système et prennent en charge les `char*`, tandis que les variantes-W fonctionnent en UTF-16 et prennent en charge les `WCHAR`.

Jusqu’à récemment, Windows a mis en évidence « Unicode »-W variantes par rapport à des API. Toutefois, les versions récentes ont utilisé la page de codes ANSI et les API-A comme moyen d’introduire la prise en charge d’UTF-8 pour les applications. Si la page de codes ANSI est configurée pour UTF-8,-les API fonctionnent en UTF-8. Ce modèle présente l’avantage de prendre en charge le code existant généré à l’aide d’API-A sans modification du code.

## <a name="set-a-process-code-page-to-utf-8"></a>Définir une page de code de processus en UTF-8

À partir de Windows version 1903 (mai 2019 Update), vous pouvez utiliser la propriété ActiveCodePage dans le appxmanifest pour les applications empaquetées, ou le manifeste fusion pour les applications non empaquetées, afin de forcer un processus à utiliser UTF-8 comme page de code de processus.

Vous pouvez déclarer cette propriété et la cible/exécuter sur des versions précédentes de Windows, mais vous devez gérer la détection et la conversion de pages de codes héritées comme d’habitude. Avec une version cible minimale de Windows version 1903, la page de codes de processus sera toujours UTF-8, de sorte que la détection et la conversion des pages de codes héritées peuvent être évitées.

## <a name="examples"></a>Exemples

**Manifeste AppX pour une application empaquetée :**

```xaml
<?xml version="1.0" encoding="utf-8"?>
<Package xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10"
         ...
         xmlns:uap7="http://schemas.microsoft.com/appx/manifest/uap/windows10/7"
         xmlns:uap8="http://schemas.microsoft.com/appx/manifest/uap/windows10/8"
         ...
         IgnorableNamespaces="... uap7 uap8 ...">

  <Applications>
    <Application ...>
      <uap7:Properties>
        <uap8:ActiveCodePage>UTF-8</uap8:ActiveCodePage>
      </uap7:Properties>
    </Application>
  </Applications>
</Package>
```

**Manifeste de fusion pour une application Win32 non empaquetée :**

``` xaml
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<assembly manifestVersion="1.0" xmlns="urn:schemas-microsoft-com:asm.v1">
  <assemblyIdentity type="win32" name="..." version="6.0.0.0"/>
  <application>
    <windowsSettings>
      <activeCodePage xmlns="http://schemas.microsoft.com/SMI/2019/WindowsSettings">UTF-8</activeCodePage>
    </windowsSettings>
  </application>
</assembly>
```

> [!NOTE]
> Ajoutez un manifeste à un fichier exécutable existant à partir de la ligne de commande avec `mt.exe -manifest <MANIFEST> -outputresource:<EXE>;#1`

## <a name="code-page-conversion"></a>Conversion de page de codes

Comme Windows fonctionne en mode natif au format UTF-16 (`WCHAR`), vous devrez peut-être convertir les données UTF-8 en UTF-16 (ou vice versa) pour interagir avec les API Windows.

[MultiByteToWideChar](https://docs.microsoft.com/windows/desktop/api/stringapiset/nf-stringapiset-multibytetowidechar) et [WideCharToMultiByte](https://docs.microsoft.com/windows/desktop/api/stringapiset/nf-stringapiset-widechartomultibyte) vous permettent d’effectuer une conversion entre UTF-8 et utf-16 (`WCHAR`) (et d’autres pages de codes). Cela s’avère particulièrement utile quand une API Win32 héritée peut uniquement comprendre `WCHAR`. Ces fonctions vous permettent de convertir une entrée UTF-8 en `WCHAR` à transmettre dans une API-W, puis de convertir tous les résultats si nécessaire.
Lors de l’utilisation de ces fonctions avec `CodePage` défini sur `CP_UTF8`, utilisez `dwFlags` de `0` ou de `MB_ERR_INVALID_CHARS`, sinon une `ERROR_INVALID_FLAGS` se produit.

Remarque : `CP_ACP` équivaut à `CP_UTF8` uniquement si l’exécution sur Windows version 1903 (mise à jour 2019 mai) ou supérieure et que la propriété ActiveCodePage décrite ci-dessus est définie sur UTF-8. Dans le cas contraire, elle honore la page de codes système héritée. Nous vous recommandons d’utiliser `CP_UTF8` explicitement.

## <a name="related-topics"></a>Rubriques connexes

- [Pages de codes](https://docs.microsoft.com/windows/desktop/Intl/code-pages)
- [Identificateurs de page de codes](https://docs.microsoft.com/windows/desktop/Intl/code-page-identifiers)
