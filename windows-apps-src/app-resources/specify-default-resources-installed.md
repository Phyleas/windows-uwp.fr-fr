---
description: Si votre application n’a pas les ressources qui correspondent aux paramètres particuliers d’un appareil client, les ressources de l’application par défaut sont utilisées. Cette rubrique explique comment spécifier ce que sont ces ressources par défaut.
title: Préciser les ressources par défaut que votre application utilise
template: detail.hbs
ms.date: 11/14/2017
ms.topic: article
keywords: windows 10, uwp, ressources, image, MRT, qualificateur
ms.localizationpriority: medium
ms.openlocfilehash: 4db2fbce788bac38a0f3a54a108f91c8293de6ba
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93031550"
---
# <a name="specify-the-default-resources-that-your-app-uses"></a>Préciser les ressources par défaut que votre application utilise

Si votre application n’a pas les ressources qui correspondent aux paramètres particuliers d’un appareil client, les ressources de l’application par défaut sont utilisées. Cette rubrique explique comment spécifier ce que sont ces ressources par défaut.

Quand un client installe votre application à partir de la Microsoft Store, les paramètres sur l’appareil du client sont mis en correspondance avec les ressources disponibles de l’application. Cette correspondance est effectuée de sorte que seules les ressources appropriées doivent être téléchargées et installées pour cet utilisateur. Par exemple, les chaînes et les images les plus appropriées pour les préférences linguistiques de l’utilisateur, ainsi que les paramètres résolution et PPP de l’appareil, sont utilisées. Par exemple, `200` est la valeur par défaut pour `scale` , mais vous pouvez remplacer cette valeur par défaut si vous le souhaitez.

Même pour les ressources qui ne sont pas placées dans leurs propres packs de ressources (comme les images adaptées aux paramètres de contraste élevé), vous pouvez spécifier les ressources par défaut que l’application doit utiliser au moment de l’exécution si une ressource qui correspond aux paramètres de l’utilisateur est introuvable. Par exemple, `standard` est la valeur par défaut pour `contrast` , mais vous pouvez remplacer cette valeur par défaut si vous le souhaitez.

Ces valeurs par défaut sont spécifiées sous la forme de valeurs de qualificateur de ressource par défaut. Pour obtenir une explication des qualificateurs de ressources, de leur utilisation et de leur objectif, consultez [adapter vos ressources pour la langue, la mise à l’échelle, le contraste élevé et d’autres qualificateurs](tailor-resources-lang-scale-contrast.md).

Vous pouvez configurer les valeurs par défaut de l’une des deux manières suivantes : Vous pouvez ajouter un fichier de configuration à votre projet, ou vous pouvez modifier votre fichier projet directement. Utilisez l’option qui vous convient le mieux, ou celle qui convient le mieux à votre système de génération.

## <a name="option-1-use-priconfigdefaultxml-to-specify-default-qualifier-values"></a>Option 1. Utiliser priconfig.default.xml pour spécifier les valeurs de qualificateur par défaut

1. Dans Visual Studio, ajoutez un nouvel élément à votre projet. Choisissez fichier XML, puis nommez le fichier `priconfig.default.xml` .
2. Dans Explorateur de solutions, sélectionnez `priconfig.default.xml` et vérifiez le fenêtre Propriétés. L’action de génération du fichier doit avoir la valeur aucun et la valeur copier dans le répertoire de sortie doit être définie sur ne pas copier.
3. Remplacez le contenu du fichier par ce code XML.
   ```xml
   <default>
      <qualifier name="Language" value="LANGUAGE-TAG(S)" />
      <qualifier name="Contrast" value="standard" />
      <qualifier name="Scale" value="200" />
      <qualifier name="HomeRegion" value="001" />
      <qualifier name="TargetSize" value="256" />
      <qualifier name="LayoutDirection" value="LTR" />
      <qualifier name="DXFeatureLevel" value="DX9" />
      <qualifier name="Configuration" value="" />
      <qualifier name="AlternateForm" value="" />
   </default>
   ```
   
   **Remarque** La valeur `LANGUAGE-TAG(S)` doit être synchronisée avec la langue par défaut de votre application. S’il s’agit d’une [balise de langue BCP-47](https://tools.ietf.org/html/bcp47)unique, la langue par défaut de votre application doit être la même balise. S’il s’agit d’une liste de balises de langue séparées par des virgules, la langue par défaut de votre application doit être la première balise de la liste. Vous définissez la langue par défaut de votre application dans le champ **langue par défaut** de l’onglet **application** dans le fichier source du manifeste de package d’application ( `Package.appxmanifest` ).

4. Chaque `<qualifier>` élément indique à Visual Studio la valeur à utiliser par défaut pour chaque nom de qualificateur. Avec le contenu du fichier que vous avez jusqu’à présent, vous n’avez pas réellement modifié le comportement de Visual Studio. En d’autres termes, Visual Studio *se comporte déjà comme si* ce fichier était présent avec ces contenus, car il s’agit des valeurs par défaut. Par conséquent, pour remplacer une valeur par défaut par votre propre valeur par défaut, vous devez modifier une valeur dans le fichier. Voici un exemple de la façon dont le fichier peut se présenter si vous avez modifié les trois premières valeurs.
   ```xml
   <default>
      <qualifier name="Language" value="de-DE" />
      <qualifier name="Contrast" value="black" />
      <qualifier name="Scale" value="400" />
      <qualifier name="HomeRegion" value="001" />
      <qualifier name="TargetSize" value="256" />
      <qualifier name="LayoutDirection" value="LTR" />
      <qualifier name="DXFeatureLevel" value="DX9" />
      <qualifier name="Configuration" value="" />
      <qualifier name="AlternateForm" value="" />
   </default>
   ```
5. Enregistrez et fermez le fichier, puis régénérez votre projet.

Pour confirmer que les valeurs par défaut remplacées sont prises en compte, recherchez le fichier `<ProjectFolder>\obj\<ReleaseConfiguration folder>\priconfig.xml` et vérifiez que son contenu correspond à vos remplacements. Si c’est le cas, vous avez correctement configuré les valeurs de qualificateur des ressources que votre application utilisera par défaut. Si aucune correspondance n’est trouvée pour les paramètres de l’utilisateur, les ressources sont utilisées dont les noms de dossier ou de fichier contiennent les valeurs de qualificateur par défaut que vous avez définies ici.

### <a name="how-does-this-work"></a>Comment cela fonctionne ?

En arrière-plan, Visual Studio lance un outil nommé `MakePri.exe` pour générer un fichier connu sous le nom d’index de ressources de package (PRI), qui décrit toutes les ressources de votre application, notamment l’indication des ressources par défaut. Pour plus d’informations sur cet outil, consultez [compiler des ressources manuellement avec MakePri.exe](compile-resources-manually-with-makepri.md). Visual Studio transmet un fichier de configuration à `MakePri.exe` . Le contenu de votre `priconfig.default.xml` fichier est utilisé en tant qu' `<default>` élément de ce fichier de configuration, qui est la partie qui spécifie le jeu de valeurs de qualificateur considéré comme étant default. Ainsi, l’ajout et `priconfig.default.xml` la modification influencent en fin de compte le contenu du fichier d’index de ressources de package que Visual Studio génère pour votre application et comprend dans son package d’application.

**Remarque** Chaque fois que vous modifiez la valeur de l' `<qualifier name="Language" ... />` élément, vous devez synchroniser cette modification avec la langue par défaut de votre application. Cela signifie que les ressources de langue indexées dans le fichier PRI de votre application correspondent à la langue par défaut du manifeste de votre application. La valeur de l' `<qualifier name="Language" ... />` élément substitue la valeur dans le manifeste par rapport au contenu de `<ProjectFolder>\obj\<ReleaseConfiguration folder>\priconfig.xml` , mais ce fichier et le manifeste de votre application doivent correspondre.

### <a name="using-a-different-file-name-than-priconfigdefaultxml"></a>Utilisation d’un nom de fichier différent de celui de `priconfig.default.xml`

Si vous nommez votre fichier `priconfig.default.xml` , Visual Studio le reconnaît et l’utilise automatiquement. Si vous lui donnez un autre nom, vous devez laisser Visual Studio le savoir. Dans votre fichier projet, entre les balises d’ouverture et de fermeture du premier `<PropertyGroup>` élément, ajoutez ce code XML.

```xml
<AppxPriConfigXmlDefaultSnippetPath>FILE-PATH-AND-NAME</AppxPriConfigXmlDefaultSnippetPath>
```

Remplacez `FILE-PATH-AND-NAME` par le chemin d’accès et le nom de votre fichier.

## <a name="option-2-use-your-project-file-to-specify-default-qualifier-values"></a>Option 2. Utiliser votre fichier projet pour spécifier les valeurs de qualificateur par défaut

Il s’agit d’une alternative à l’option 1. Une fois que vous avez compris le fonctionnement de l’option 1, vous pouvez opter pour l’option 2, si cela convient mieux à votre flux de travail de développement et/ou de génération.

Dans votre fichier projet, entre les balises d’ouverture et de fermeture du premier `<PropertyGroup>` élément, ajoutez ce code XML.

```xml
<AppxDefaultResourceQualifiers>Language=LANGUAGE-TAG(S)|Contrast=standard|Scale=200|HomeRegion=001|TargetSize=256|LayoutDirection=LTR|DXFeatureLevel=DX9|Configuration=|AlternateForm=</AppxDefaultResourceQualifiers>
```

Voici un exemple de la façon dont cela peut se présenter après avoir modifié les trois premières valeurs.

```xml
<AppxDefaultResourceQualifiers>Language=de-DE|Contrast=black|Scale=400|HomeRegion=001|TargetSize=256|LayoutDirection=LTR|DXFeatureLevel=DX9|Configuration=|AlternateForm=</AppxDefaultResourceQualifiers>
```

Enregistrez et fermez, puis régénérez votre projet.

**Remarque** Chaque fois que vous modifiez la `Language=` valeur, vous devez synchroniser cette modification avec la langue par défaut de votre application dans le concepteur de manifeste (en ouvrant `Package.appxmanifest` ).

## <a name="related-topics"></a>Rubriques connexes

* [Personnaliser vos ressources pour la langue, l’échelle, le contraste élevé et d’autres qualificateurs](tailor-resources-lang-scale-contrast.md)
* [Balise de langue BCP-47](https://tools.ietf.org/html/bcp47)
* [Compiler des ressources manuellement avec MakePri.exe](compile-resources-manually-with-makepri.md)
