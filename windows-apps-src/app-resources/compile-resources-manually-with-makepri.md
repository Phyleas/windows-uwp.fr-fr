---
description: MakePri.exe est un outil en ligne de commande que vous pouvez utiliser pour créer et vider des fichiers IRP. Il est intégré en tant qu’élément de MSBuild dans Microsoft Visual Studio, mais il peut aussi être utilisé pour la création de packages manuellement ou avec un système de génération personnalisé.
title: Compiler des ressources manuellement avec MakePri.exe
template: detail.hbs
ms.date: 10/23/2017
ms.topic: article
keywords: windows 10, uwp, ressources, image, MRT, qualificateur
ms.localizationpriority: medium
ms.openlocfilehash: 579aaf5f833f2d3ddb8e4d8c080a6f94ac21f40f
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93031862"
---
# <a name="compile-resources-manually-with-makepriexe"></a>Compiler des ressources manuellement avec MakePri.exe

MakePri.exe est un outil en ligne de commande que vous pouvez utiliser pour créer et vider des fichiers IRP. Il est intégré en tant qu’élément de MSBuild dans Microsoft Visual Studio, mais il peut aussi être utilisé pour la création de packages manuellement ou avec un système de génération personnalisé.

> [!NOTE]
> MakePri.exe est installé lorsque vous activez l’option **SDK Windows pour les applications gérées UWP** lors de l’installation du kit de développement logiciel (SDK) Windows. Il est installé dans le chemin d’accès `%WindowsSdkDir%bin\<WindowsTargetPlatformVersion>\x64\makepri.exe` (et dans les dossiers nommés pour les autres architectures). Par exemple : `C:\Program Files (x86)\Windows Kits\10\bin\10.0.17713.0\x64\makepri.exe`.

## <a name="in-this-section"></a>Contenu de cette section
|Rubrique|Description|
|-|-|
| [Options de ligne de commande de MakePri.exe](makepri-exe-command-options.md) | MakePri.exe a le jeu de commandes `createconfig` ,,, `dump` `new` `resourcepack` et `versioned` . Cette rubrique décrit en détail les options de ligne de commande pour leur utilisation. |
| [Fichier de configuration de MakePri.exe](makepri-exe-configuration.md) | Cette rubrique décrit le schéma du fichier de configuration XML MakePri.exe. |
| [Indexeurs spécifiques au format de MakePri.exe](makepri-exe-format-specific-indexers.md) | Cette rubrique décrit les indexeurs spécifiques au format utilisés par l’outil MakePri.exe pour générer son index de ressources. |

## <a name="makepriexe-command-line-options"></a>Options de ligne de commande de MakePri.exe

MakePri.exe a le jeu de commandes `createconfig` ,,, `dump` `new` `resourcepack` et `versioned` . Pour plus d’informations sur leur utilisation, consultez [MakePri.exe options de ligne de commande](makepri-exe-command-options.md).

## <a name="makepriexe-configuration"></a>Configuration de MakePri.exe

Le fichier de configuration XML PRI détermine comment et quelles ressources sont indexées. Le schéma du fichier XML de configuration est décrit dans [MakePri.exe configuration](makepri-exe-configuration.md).

## <a name="format-specific-indexers"></a>Indexeurs spécifiques au format

MakePri.exe est généralement utilisé avec les `new` `versioned` options, et `resourcepack` . Dans ce cas, il indexe les fichiers sources pour générer un index des ressources. MakePri.exe utilise différents indexeurs individuels pour lire des conteneurs ou fichiers de ressources sources différents pour les ressources. L’indexeur de dossier le plus simple est l’indexeur de dossiers, qui indexe le contenu d’un dossier pour les ressources telles que les `.jpg` `.png` images ou. Pour plus d’informations, consultez [MakePri.exe indexeurs spécifiques au format](makepri-exe-format-specific-indexers.md).

## <a name="makepriexe-warnings-and-error-messages"></a>MakePri.exe les avertissements et les messages d’erreur

### <a name="resources-found-for-languages-languages-but-no-resources-found-for-default-languages-languages-change-the-default-language-or-qualify-resources-with-the-default-language"></a>Ressources trouvées pour la ou les langues « <langue (s) > », mais aucune ressource n’a été trouvée pour la ou les langues par défaut : « <Language (s) > ». Modifiez la langue par défaut ou qualifiez les ressources avec la langue par défaut.

Cet avertissement s’affiche quand MakePri.exe ou MSBuild Découvre des fichiers ou des ressources de type chaîne pour une ressource nommée donnée qui semblent être marqués avec des qualificateurs de langue, mais qu’aucun candidat n’est trouvé pour une langue par défaut. Le processus d’utilisation de qualificateurs dans les noms de fichiers et de dossiers est décrit dans [adapter vos ressources à la langue, à l’échelle et à d’autres qualificateurs](tailor-resources-lang-scale-contrast.md). Un fichier ou un dossier peut contenir un nom de langue, mais aucune ressource n’est détectée et qualifiée pour la langue par défaut exacte. Par exemple, si un projet utilise « en-US » comme langue par défaut et possède un fichier nommé « de/logo.png », mais qu’il n’a pas de fichiers marqués avec la langue par défaut « en-US », cet avertissement s’affiche. Pour supprimer cet avertissement, le ou les deux fichiers ou ressources de chaîne doivent être qualifiés avec la langue par défaut, ou la langue par défaut doit être modifiée. Pour modifier la langue par défaut, avec votre solution ouverte dans Visual Studio, ouvrez `Package.appxmanifest` . Sous l’onglet application, vérifiez que la langue par défaut est définie correctement (par exemple, « fr » ou « en-US »).

### <a name="no-default-or-neutral-resource-given-for-resource-identifier-the-application-may-throw-an-exception-for-certain-user-configurations-when-retrieving-the-resources"></a>Aucune ressource neutre ou par défaut donnée pour « <resource identifier> ». L’application peut lever une exception pour certaines configurations d’utilisateur lors de la récupération des ressources.

Cet avertissement s’affiche lorsque MakePri.exe ou MSBuild Découvre des fichiers ou des ressources qui semblent être marqués avec des qualificateurs de langue pour lesquels les ressources ne sont pas claires. Il existe des qualificateurs, mais il n’existe aucune garantie qu’un candidat à une ressource spécifique peut être retourné pour cet identificateur de ressource au moment de l’exécution. Si aucun candidat aux ressources pour une langue particulière, homeregion ou un autre qualificateur ne peut être trouvé qui est une valeur par défaut ou correspondra toujours au contexte d’un utilisateur, cet avertissement s’affiche. Au moment de l’exécution, pour des configurations utilisateur particulières, telles que les préférences linguistiques d’un utilisateur ou l’emplacement d’hébergement ( **paramètres**  >  **& région de langue**  >  **& langue** ), les API utilisées pour récupérer la ressource peuvent lever une exception inattendue. Pour supprimer cet avertissement, des ressources par défaut doivent être fournies, telles qu’une ressource dans la langue par défaut ou la région d’hébergement globale du projet (homeregion-001).

## <a name="using-makepriexe-in-a-build-system"></a>Utilisation de MakePri.exe dans un système de génération

Les systèmes de génération doivent utiliser la commande MakePri.exe, `new` `versioned` ou, `resourcepack` selon le type de projet en cours de génération. Les systèmes de génération qui créent un fichier PRI actualisé doivent utiliser la `new` commande. Les systèmes de génération qui doivent garantir la compatibilité des décalages internes via les itérations peuvent utiliser la `versioned` commande. Créez des systèmes qui doivent créer un fichier PRI contenant des variantes de ressources supplémentaires, avec la validation pour s’assurer qu’aucune nouvelle ressource n’est ajoutée pour cette variante, utilisez la `resourcepack` commande.

Les systèmes de génération qui requièrent un contrôle explicite sur les fichiers sources qui sont indexés peuvent utiliser l’indexeur ResFiles au lieu d’indexer un dossier. Les systèmes de génération peuvent également utiliser plusieurs passes d’index avec différents [indexeurs spécifiques au format](makepri-exe-format-specific-indexers.md) pour générer un fichier PRI unique.

Les systèmes de génération peuvent également utiliser l’indexeur de format PRI spécifique pour ajouter des fichiers PRI prédéfinis dans le PRI pour le package à partir d’autres composants, tels que des bibliothèques de classes, des assemblys, des kits de développement logiciel (SDK) et des dll.

Lorsque des fichiers PRI sont générés pour d’autres composants, bibliothèques de classes, assemblys, dll et kits de développement logiciel (SDK), la configuration **initialPath** doit être utilisée pour s’assurer que les ressources de composants ont leurs propres mappages de sous-ressources qui ne sont pas en conflit avec l’application dans laquelle ils sont inclus.

## <a name="related-topics"></a>Rubriques connexes
* [Options de ligne de commande de MakePri.exe](makepri-exe-command-options.md)
* [ Configuration deMakePri.exe](makepri-exe-configuration.md)
* [Indexeurs spécifiques au format de MakePri.exe](makepri-exe-format-specific-indexers.md)
* [Adaptez vos ressources à la langue, à la mise à l’échelle et à d’autres qualificateurs](tailor-resources-lang-scale-contrast.md)
