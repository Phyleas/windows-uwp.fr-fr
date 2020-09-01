---
Description: Certains types d’applications (dictionnaires multilingues, outils de traduction, etc.) doivent remplacer le comportement par défaut d’un ensemble d’applications et créer des ressources dans le package d’application plutôt que dans des packages de ressources distincts. Cette rubrique explique la procédure à suivre.
title: Créer des ressources dans votre package d’application
template: detail.hbs
ms.date: 11/14/2017
ms.topic: article
keywords: windows 10, uwp, ressources, image, MRT, qualificateur
ms.localizationpriority: medium
ms.openlocfilehash: b975dcf88ecd26dc5a24d602c117b779fa2aada6
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89174523"
---
# <a name="build-resources-into-your-app-package-instead-of-into-a-resource-pack"></a>Générer des ressources dans votre package d’application, plutôt que dans un pack de ressources

Certains types d’applications (dictionnaires multilingues, outils de traduction, etc.) doivent remplacer le comportement par défaut d’un bundle d’applications et créer des ressources dans le package d’application au lieu de les faire dans des packages de ressources (ou des packs de ressources) distincts. Cette rubrique explique la procédure à suivre.

Par défaut, lorsque vous générez un [bundle d’applications (. appxbundle)](/windows/msix/package/packaging-uwp-apps), seules vos ressources par défaut pour la langue, l’échelle et le niveau de fonctionnalité DirectX sont intégrées dans le package d’application. Vos ressources traduites &mdash; et vos ressources adaptées à des niveaux de fonctionnalités de mise à l’échelle non définie par défaut et/ou DirectX &mdash; sont intégrées aux packages de ressources, et elles sont uniquement téléchargées sur les appareils qui en ont besoin. Si un client achète votre application à partir de l’Microsoft Store à l’aide d’un appareil dont la langue est définie sur espagnol, seule votre application et le package de ressources espagnol sont téléchargés et installés. Si le même utilisateur modifie par la suite ses préférences de langue en français dans **paramètres**, le package de ressources français de votre application est téléchargé et installé. Des choses similaires se produisent avec vos ressources qualifiées pour le niveau de fonctionnalité de mise à l’échelle et de DirectX. Pour la majorité des applications, ce comportement constitue une efficacité importante et c’est exactement ce que vous et le client *souhaitez* faire.

Toutefois, si votre application permet à l’utilisateur de changer la langue à la volée à partir de l’application (au lieu des **paramètres**via), le comportement par défaut n’est pas approprié. Vous souhaitez en fait que toutes les ressources de votre langue soient téléchargées et installées de manière non conditionnelle avec l’application une fois, puis restent sur l’appareil. Vous souhaitez créer toutes ces ressources dans votre package d’application plutôt que dans des packages de ressources distincts.

**Remarque** L’inclusion de ressources dans un package d’application augmente essentiellement la taille de l’application. C’est la raison pour laquelle elle ne s’avère utile que si la nature de l’application en a besoin. Si ce n’est pas le cas, vous n’avez rien à faire sauf créer un bundle d’applications standard comme d’habitude.

Vous pouvez configurer Visual Studio pour créer des ressources dans votre package d’application de deux manières. Vous pouvez ajouter un fichier de configuration à votre projet, ou vous pouvez modifier votre fichier projet directement. Utilisez l’option qui vous convient le mieux, ou celle qui convient le mieux à votre système de génération.

## <a name="option-1-use-priconfigpackagingxml-to-build-resources-into-your-app-package"></a>Option 1. Utiliser priconfig.packaging.xml pour créer des ressources dans votre package d’application

1. Dans Visual Studio, ajoutez un nouvel élément à votre projet. Choisissez fichier XML, puis nommez le fichier `priconfig.packaging.xml` .
2. Dans Explorateur de solutions, sélectionnez `priconfig.packaging.xml` et vérifiez le fenêtre Propriétés. L’action de génération du fichier doit avoir la valeur aucun et la valeur copier dans le répertoire de sortie doit être définie sur ne pas copier.
3. Remplacez le contenu du fichier par ce code XML.
   ```xml
   <packaging>
      <autoResourcePackage qualifier="Language" />
      <autoResourcePackage qualifier="Scale" />
      <autoResourcePackage qualifier="DXFeatureLevel" />
   </packaging>
   ```
4. Chaque `<autoResourcePackage>` élément indique à Visual Studio de fractionner automatiquement les ressources pour le nom de qualificateur donné en packages de ressources distincts. C’est ce que l’on appelle le *fractionnement automatique*. Avec le contenu du fichier que vous avez jusqu’à présent, vous n’avez pas réellement modifié le comportement de Visual Studio. En d’autres termes, Visual Studio se *comporte déjà comme si* ce fichier était présent avec ces contenus, car il s’agit des valeurs par défaut. Si vous ne souhaitez pas que Visual Studio se fractionne automatiquement sur un nom de qualificateur, supprimez cet `<autoResourcePackage>` élément du fichier. Voici à quoi ressemblerait le fichier si vous souhaitiez que toutes vos ressources linguistiques soient intégrées dans le package d’application au lieu d’être répartie automatiquement dans des packages de ressources distincts.
   ```xml
   <packaging>
      <autoResourcePackage qualifier="Scale" />
      <autoResourcePackage qualifier="DXFeatureLevel" />
   </packaging>
   ```
5. Enregistrez et fermez le fichier, puis régénérez votre projet.

Pour confirmer que vos choix de fractionnement automatique sont pris en compte, recherchez le fichier `<ProjectFolder>\obj\<ReleaseConfiguration folder>\split.priconfig.xml` et vérifiez que son contenu correspond à vos choix. Si c’est le cas, vous avez correctement configuré Visual Studio pour créer les ressources de votre choix dans le package d’application.

Vous devez effectuer une dernière étape. **Mais uniquement si vous avez supprimé le `Language` nom du qualificateur**. Vous devez spécifier l’Union de l’ensemble de la langue prise en charge par votre application comme langue par défaut de votre application. Pour plus d’informations, consultez [spécifier les ressources par défaut que votre application utilise](specify-default-resources-installed.md). C’est ce que `priconfig.default.xml` vous contiendra si vous incluiez des ressources pour l’anglais, l’espagnol et le français dans votre package d’application.

```xml
   <default>
      <qualifier name="Language" value="en;es;fr" />
      ...
   </default>
```

### <a name="how-does-this-work"></a>Comment cela fonctionne ?

En arrière-plan, Visual Studio lance un outil nommé `MakePri.exe` pour générer un fichier connu sous le nom d’index de ressources de package, qui décrit toutes les ressources de votre application, notamment l’indication des noms de qualificateurs de ressources à fractionner automatiquement. Pour plus d’informations sur cet outil, consultez [compiler des ressources manuellement avec MakePri.exe](compile-resources-manually-with-makepri.md). Visual Studio transmet un fichier de configuration à `MakePri.exe` . Le contenu de votre `priconfig.packaging.xml` fichier est utilisé en tant qu' `<packaging>` élément de ce fichier de configuration, qui est la partie qui détermine le fractionnement automatique. Ainsi, l’ajout et `priconfig.packaging.xml` la modification influencent finalement le contenu du fichier d’index de ressources de package généré par Visual Studio pour votre application, ainsi que le contenu des packages dans votre offre groupée d’applications.

### <a name="using-a-different-file-name-than-priconfigpackagingxml"></a>Utilisation d’un nom de fichier différent de celui de `priconfig.packaging.xml`

Si vous nommez votre fichier `priconfig.packaging.xml` , Visual Studio le reconnaît et l’utilise automatiquement. Si vous lui donnez un autre nom, vous devez laisser Visual Studio le savoir. Dans votre fichier projet, entre les balises d’ouverture et de fermeture du premier `<PropertyGroup>` élément, ajoutez ce code XML.

```xml
<AppxPriConfigXmlPackagingSnippetPath>FILE-PATH-AND-NAME</AppxPriConfigXmlPackagingSnippetPath>
```

Remplacez `FILE-PATH-AND-NAME` par le chemin d’accès et le nom de votre fichier.

## <a name="option-2-use-your-project-file-to-build-resources-into-your-app-package"></a>Option 2. Utiliser votre fichier projet pour créer des ressources dans votre package d’application

Il s’agit d’une alternative à l’option 1. Une fois que vous avez compris le fonctionnement de l’option 1, vous pouvez opter pour l’option 2, si cela convient mieux à votre flux de travail de développement et/ou de génération.

Dans votre fichier projet, entre les balises d’ouverture et de fermeture du premier `<PropertyGroup>` élément, ajoutez ce code XML.

```xml
<AppxBundleAutoResourcePackageQualifiers>Language|Scale|DXFeatureLevel</AppxBundleAutoResourcePackageQualifiers>
```

Voici à quoi cela ressemble une fois que vous avez supprimé le premier nom de qualificateur.

```xml
<AppxBundleAutoResourcePackageQualifiers>Scale|DXFeatureLevel</AppxBundleAutoResourcePackageQualifiers>
```

Enregistrez et fermez, puis régénérez votre projet.

Vous devez effectuer une dernière étape. **Mais uniquement si vous avez supprimé le `Language` nom du qualificateur**. Vous devez spécifier l’Union de l’ensemble de la langue prise en charge par votre application comme langue par défaut de votre application. Pour plus d’informations, consultez [spécifier les ressources par défaut que votre application utilise](specify-default-resources-installed.md). C’est ce que votre fichier projet doit contenir si vous avez inclus des ressources pour l’anglais, l’espagnol et le français dans votre package d’application.

```xml
<AppxDefaultResourceQualifiers>Language=en;es;fr</AppxDefaultResourceQualifiers>
```

## <a name="related-topics"></a>Rubriques connexes

* [Créer un package d’application UWP avec Visual Studio](/windows/msix/package/packaging-uwp-apps)
* [Compiler des ressources manuellement avec MakePri.exe](compile-resources-manually-with-makepri.md)
* [Préciser les ressources par défaut que votre application utilise](specify-default-resources-installed.md)