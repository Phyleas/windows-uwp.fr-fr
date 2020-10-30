---
description: Une application localisée est une application qui peut être localisée sur d’autres marchés, langues ou régions sans dévoiler les défauts fonctionnels de l’application. La propriété la plus essentielle d’une application localisable est que son code exécutable a été séparé proprement de ses ressources localisables.
title: Rendre votre application localisable
ms.assetid: 06E1D4BB-59EA-4D71-99AC-7CB93D2A58A7
template: detail.hbs
ms.date: 11/07/2017
ms.topic: article
keywords: Windows 10, UWP, globalisation, adaptabilité, localisation
ms.localizationpriority: medium
ms.openlocfilehash: 8dfae2f80706edcc4462960e396f228c2829600b
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93034362"
---
# <a name="make-your-app-localizable"></a>Rendre votre application localisable

Une application localisée est une application qui peut être localisée pour d’autres marchés, langues ou régions sans dévoiler les défauts fonctionnels de l’application. La propriété la plus essentielle d’une application localisable est que son code exécutable a été séparé proprement de ses ressources localisables. Vous devez donc déterminer les ressources de votre application qui doivent être localisées. Demandez-vous ce que vous devez modifier si votre application doit être localisée pour d’autres marchés.

Nous vous recommandons également de vous familiariser avec les [instructions de globalisation](guidelines-and-checklist-for-globalizing-your-app.md).

## <a name="put-your-strings-into-resources-files-resw"></a>Placer vos chaînes dans des fichiers de ressources (. resw)

Ne codez pas en dur les littéraux de chaîne dans votre code impératif, le balisage XAML ni dans votre manifeste de package d’application. Au lieu de cela, placez vos chaînes dans des fichiers de ressources (. resw) afin qu’ils puissent être adaptés à différents marchés locaux indépendamment des binaires générés de votre application. Pour plus d’informations, consultez [localiser des chaînes dans votre interface utilisateur et le manifeste du package d’application](../../app-resources/localize-strings-ui-manifest.md).

Cette rubrique montre également comment ajouter des commentaires à votre fichier de ressources par défaut (. resw). Par exemple, si vous adoptez une voix ou un ton informel, veillez à expliquer cela dans commentaires. En outre, pour réduire les dépenses, vérifiez que seules les chaînes qui doivent être traduites sont fournies aux traducteurs.

Définissez la langue par défaut de votre application de manière appropriée dans le fichier source du manifeste de votre package d’application ( `Package.appxmanifest` fichier). La langue par défaut détermine la langue utilisée lorsque les langues préférées de l’utilisateur ne correspondent à aucune des langues prises en charge par votre application. Marquez toutes vos ressources avec leur langue (y compris celles de votre langue par défaut, par exemple `\Assets\en-us\Logo.png` ) afin que le système puisse déterminer la langue dans laquelle se trouve la ressource et comment elle est utilisée dans des situations particulières.

## <a name="tailor-your-images-and-other-file-resources-for-language"></a>Adapter vos images et autres ressources de fichiers pour la langue

Idéalement, vous serez en mesure de globaliser vos images &mdash; , et de les rendre indépendantes de la culture. Pour toutes les images et autres ressources de fichiers dans lesquelles cela n’est pas possible, créez autant de variantes différentes que nécessaire et placez les qualificateurs de langue appropriés dans leurs noms de fichiers ou de dossiers. Pour plus d’informations, consultez [adapter vos ressources à la langue, à l’échelle, au contraste élevé et à d’autres qualificateurs](../../app-resources/tailor-resources-lang-scale-contrast.md).

Pour réduire les coûts de localisation, ne placez pas de texte ni de matériau culturellement sensible dans les images. Une image appropriée dans votre propre culture peut être inappropriée ou mal interprétée dans d’autres cultures. Évitez d’utiliser des images propres à la culture, telles que des boîtes aux lettres, qui ne sont pas courantes dans le monde entier. Évitez les symboles religieux, les animaux, les images propres à la politique ou au genre. L’affichage de la chair, des parties du corps ou des mouvements manuels peut également être une rubrique sensible. Si vous ne pouvez pas les éviter, vos images devront être localisées avec attention. Si vous localisez du texte vers une langue dotée d’un sens de lecture différent du vôtre, l’utilisation d’images et d’effets symétriques facilite la prise en charge de la mise en miroir.

Évitez également l’utilisation de texte dans les images et la reconnaissance vocale dans des fichiers audio/vidéo.

## <a name="the-use-of-color-in-your-app"></a>Utilisation des couleurs dans votre application

Soyez attentif à l’utilisation de Color. L’utilisation de combinaisons de couleurs associées à des indicateurs nationaux ou à des mouvements politiques peut être problématique. Les choix de couleurs devront peut-être être revus par des experts de la culture. Il existe également des problèmes d’accessibilité liés à l’utilisation de Color. Si vous utilisez Color pour transmettre la signification, vous devez également transmettre ces mêmes informations par d’autres moyens, tels que la taille, la forme ou une étiquette.

## <a name="consider-factoring-your-strings-into-sentences"></a>Envisagez de factoriser vos chaînes en phrases

Utilisez des chaînes de taille appropriée. Les chaînes courtes sont plus faciles à traduire et permettent le recyclage des traductions (ce qui évite les dépenses, car la même chaîne n’est pas envoyée plus d’une fois au Localisateur). En outre, les chaînes extrêmement longues peuvent ne pas être prises en charge par les outils de localisation.

Toutefois, en ce qui concerne cette règle, vous risquez de réutiliser une chaîne dans différents contextes. Même les mots simples tels que &quot; activé &quot; et &quot; désactivé &quot; peuvent être traduits différemment, en fonction du contexte. En anglais, « on » et « off » peuvent être utilisés pour activer et désactiver le mode Avion, la fonctionnalité Bluetooth et des appareils. En revanche, en italien, la traduction dépend du contexte de ce qui est activé et désactivé. Vous devez créer une paire de chaînes pour chaque contexte. Vous pouvez réutiliser des chaînes si les deux contextes sont identiques. Par exemple, vous pouvez réutiliser la chaîne « Volume » pour le volume des effets sonores et le volume de la musique, car les deux font référence à l’intensité sonore. Vous ne devez pas réutiliser la même chaîne pour faire référence au volume du disque dur, car le contexte et la signification sont différents, et le mot peut être traduit différemment.

De plus, une chaîne comme « text » ou « fax » peut être utilisée en tant que verbe ou substantif en anglais, ce qui peut compliquer le processus de traduction. Créez plutôt deux chaînes distinctes pour la forme verbale et la forme nominale. Lorsque vous n’êtes pas certain que les contextes sont les mêmes, ne prenez pas de risque et utilisez deux chaînes distinctes.

En bref, factorisez vos chaînes en parties qui fonctionnent dans tous les contextes. Dans certains cas, une chaîne doit être une phrase entière.

Prenons la chaîne suivante : « le {0} n’a pas pu être synchronisé ».

Divers mots peuvent être remplacés {0} , par exemple « rendez-vous », « tâche » ou « document ». Bien que cet exemple fonctionne pour la langue anglaise, il ne fonctionne pas dans tous les cas pour la phrase correspondante dans, par exemple, l’allemand. Remarquez que dans les phrases allemandes suivantes, certains mots dans la chaîne de modèle (« Der », « Die », « Das ») doivent correspondre au mot paramétré :

| Anglais                                    | Allemand                                           |
|:------------------------------------------ |:------------------------------------------------ |
| The appointment could not be synchronized. | Der Termin konnte nicht synchronisiert werden.   |
| The task could not be synchronized.        | Die Aufgabe konnte nicht synchronisiert werden.  |
| The document could not be synchronized.    | Das Dokument konnte nicht synchronisiert werden. |

En guise d’autre exemple, considérez la phrase « me le rappeler en {0} minute (s) ». L’utilisation de « minute (s) » fonctionne pour la langue anglaise, mais d’autres langues peuvent utiliser des termes différents. Par exemple, le polonais utilise « minuta », « minuty » ou « minut » selon le contexte.

Pour résoudre ce problème, localisez la phrase entière, plutôt qu’un mot individuel. Cela peut paraître comme une charge de travail supplémentaire et une solution dépourvue d’élégance, mais il s’agit de la solution optimale pour les raisons suivantes :

- Un message correct grammaticalement s’affiche pour tous les langages.
- Votre traducteur n’aura pas besoin de vous demander ce à quoi les chaînes seront remplacées.
- Vous n’aurez pas besoin d’implémenter un correctif de code onéreux lorsqu’un problème de ce type se révélera une fois votre application terminée.

## <a name="other-considerations-for-strings"></a>Autres considérations relatives aux chaînes

Évitez les colloquialisms et les métaphores dans les chaînes que vous créez dans votre langue par défaut. La langue propre à un groupe démographique, telle que la culture et l’âge, peut être difficile à comprendre ou à traduire, car seules les personnes de ce groupe démographique utilisent cette langue. De la même façon, les métaphores peuvent avoir un sens pour une personne, mais ne rien évoquer pour une autre. Par exemple, &quot;vendanger&quot; a un sens précis dans le jargon du football, qui échappe aux non-initiés.

N’utilisez pas de jargon, d’abréviations ni d’acronymes. Un langage technique aura moins de chances d’être compris d’un public non initié ou de personnes issues d’autres cultures ou régions et il sera difficile à traduire. Ce vocabulaire n’est pas utilisé dans les conversations de tous les jours. La langue technique apparaît souvent dans les messages d’erreur pour identifier les problèmes matériels et logiciels, mais vous ne devez utiliser des chaînes *qui ne sont techniques que si l’utilisateur a besoin de ce niveau d’informations et peut l’agir d’une action ou d’une personne qui peut y accéder* .

L’utilisation d’une voix ou d’un ton informel dans vos chaînes est un choix valide. Vous pouvez utiliser des commentaires dans votre fichier de ressources par défaut (. resw) pour indiquer cette intention.

## <a name="pseudo-localization"></a>Pseudo-localisation

Pseudo-Localisez votre application pour dévoiler les problèmes d’adaptabilité. La pseudo-localisation est un type de test d’exécution à sec ou de divulgation. Vous obtenez un ensemble de ressources qui ne sont pas vraiment traduites ; ils n’ont pas le même aspect. Vos chaînes sont environ 40% plus longues que dans la langue par défaut, par exemple, et elles comportent des délimiteurs afin que vous puissiez voir d’un coup d’œil si elles ont été tronquées dans l’interface utilisateur.

## <a name="deployment-considerations"></a>Points à prendre en considération pour le déploiement

Lorsque vous installez une application qui contient des données linguistiques localisées, vous pouvez constater que seule la langue par défaut est disponible pour l’application, même si vous avez initialement inclus des ressources pour plusieurs langues. Cela est dû au fait que le processus d’installation est optimisé pour installer uniquement les ressources de langue qui correspondent à la langue et à la culture actuelles de l’appareil. Par conséquent, si votre appareil est configuré pour en-US, seules les ressources de langue en-US sont installées avec votre application.

> [!NOTE]
> Il n’est pas possible d’installer une prise en charge linguistique supplémentaire pour votre application après l’installation initiale. Si vous modifiez la langue par défaut après l’installation d’une application, l’application continue à utiliser uniquement les ressources de langue d’origine.

Si vous souhaitez vous assurer que toutes les ressources de langue sont disponibles après l’installation, créez un fichier de configuration pour le package d’application qui spécifie que certaines ressources sont requises lors de l’installation (y compris les ressources de langue). Cette fonctionnalité d’installation optimisée est automatiquement activée lorsque le. appxbundle de votre application est généré pendant l’empaquetage. Pour plus d’informations, consultez [vérifier que les ressources sont installées sur un appareil, qu’un appareil en ait besoin ou non](/previous-versions/dn482043(v=vs.140)).

Si vous le souhaitez, pour vous assurer que toutes les ressources sont installées (pas seulement un sous-ensemble), vous pouvez désactiver la génération. appxbundle quand vous empaquetez votre application. Toutefois, cela n’est pas recommandé, car il peut augmenter le temps d’installation de votre application.

Désactivez la génération automatique du package. appxbundle en affectant à l’attribut « générer un bundle d’applications » la valeur « jamais » :

1. Dans Visual Studio, cliquez avec le bouton droit sur le nom du projet
2. Sélectionnez **stocker**  ->  **créer des packages d’application...**
3. Dans la boîte de dialogue **créer vos packages** , sélectionnez **je souhaite créer des packages à charger sur le Microsoft Store à l’aide d’un nouveau nom d’application** , puis cliquez sur **suivant** .
4. Dans la boîte de dialogue **Sélectionner un nom d’application** , sélectionnez/Créez un nom d’application pour votre package.
5. Dans la boîte de dialogue **Sélectionner et configurer des packages** , affectez à **générer un bundle d’applications** la valeur **jamais** .

## <a name="geopolitical-awareness"></a>Sensibilisation géopolitique

Évitez toute infraction politique dans les cartes ou en faisant référence à des régions. Les cartes peuvent inclure des frontières régionales ou nationales controversées, et elles sont une source fréquente d’infractions politiques. Veillez à ce que toute interface utilisateur utilisée pour la sélection d’une nation lui fasse référence en tant que &quot;pays/région&quot;. L’affichage d’un secteur en litige dans une liste de &quot; pays &quot; &mdash; , tels que dans un formulaire d’adresse, peut ingérer &mdash; certains utilisateurs.

## <a name="language--and-region-changed-events"></a>Événements de modification de langue et de région

S’abonner aux événements déclenchés lorsque les paramètres de langue et de région du système changent. Procédez ainsi pour pouvoir recharger les ressources, le cas échéant. Pour plus d’informations, consultez [mise à jour de chaînes en réponse aux événements de modification de valeur de qualificateur](../../app-resources/localize-strings-ui-manifest.md#updating-strings-in-response-to-qualifier-value-change-events) et [mise à jour d’images en réponse aux événements de modification de valeur de qualificateur](../../app-resources/images-tailored-for-scale-theme-contrast.md#updating-images-in-response-to-qualifier-value-change-events).

## <a name="ensure-the-correct-parameter-order-when-formatting-strings"></a>Garantir l’ordre correct des paramètres lors de la mise en forme des chaînes

Ne partez pas du principe que toutes les langues expriment les paramètres dans le même ordre. Par exemple, considérez ce format.

```csharp
    string.Format("Every {0} {1}", monthName, dayNumber); // For example, "Every April 1".
```

La chaîne de format dans cet exemple fonctionne pour l’anglais (États-Unis). Toutefois, il n’est pas approprié pour l’allemand (Allemagne), par exemple, où le jour et le mois s’affichent dans l’ordre inverse. Assurez-vous que le traducteur connaît l’intention de chacun des paramètres afin qu’ils puissent inverser l’ordre des éléments de mise en forme dans la chaîne de format (par exemple, « {1} {0} »), en fonction de la langue cible.

## <a name="dont-over-localize"></a>Ne pas dépasser la localité

Envoyer uniquement le langage naturel aux traducteurs ; langage de programmation et balisage non. Une `<link>` balise n’est pas en langage naturel. Prenons l’exemple ci-dessous.

| Ne pas localiser ce                   | Localiser ce |
|:--------------------------------------- |:-------------------------- |
| &lt;lier les &gt; conditions d’utilisation &lt; /Link&gt;   | conditions d’utilisation               |
| &lt;lier la &gt; politique de confidentialité &lt; /Link&gt; | politique de confidentialité             |

L’inclusion `<link>` de la balise dans votre fichier de ressources (. resw) signifie qu’il est probable qu’elle sera traduite. Cela rendrait la balise non valide. Si vous avez des chaînes longues qui doivent inclure le balisage afin de conserver le contexte et de garantir le classement, faites-le clairement dans les commentaires ce qui ne doit pas être traduit.

## <a name="choose-an-appropriate-translation-approach"></a>Choisir une approche de traduction appropriée

Une fois que les chaînes ont été séparées en fichiers de ressources, elles peuvent être traduites. Le moment idéal pour traduire des chaînes est après la finalisation de ces chaînes dans le projet, laquelle survient habituellement vers la fin d’un projet. Vous pouvez adopter des approches différentes du processus de traduction. Cela peut dépendre du volume des chaînes à traduire, le nombre de langues cibles et la manière dont la traduction sera réalisée (par exemple, en interne ou en faisant appel à un prestataire externe).

Envisagez ces options.

- **Les fichiers de ressources peuvent être traduits en étant ouverts directement dans le projet.** Cette approche fonctionne bien pour un projet qui comporte un petit volume de chaînes qui doivent être traduites en deux ou trois langues. Elle est adaptée pour un scénario dans lequel un développeur parlerait plusieurs langues et accepterait de traiter le processus de traduction. Cette approche tire parti de la rapidité, ne nécessite pas d’outils et minimise le risque de problèmes de traduction. Mais il n’est pas évolutif. En particulier, les ressources dans les différentes langues peuvent aisément se désynchroniser, ce qui peut entraîner de mauvaises expériences utilisateur et des difficultés de maintenance.
- **Les fichiers de ressources de type chaîne sont au format de texte XML ou ResJSON. ils peuvent donc être rendus pour la traduction à l’aide de n’importe quel éditeur de texte. Les fichiers traduits sont ensuite recopiés dans le projet.** Cette approche présente un risque, car les traducteurs pourraient modifier accidentellement les balises XML. Cependant, elle permet le déroulement de la traduction en dehors du projet Microsoft Visual Studio. Cette approche pourrait bien fonctionner pour les projets devant être traduits vers un nombre réduit de langues. Le format XLIFF est un format XML spécifiquement conçu pour être utilisé en localisation et devrait être bien pris en charge par les prestataires de localisation et les outils de localisation. Vous pouvez utiliser le [Kit de ressources pour application multilingue](/previous-versions/windows/apps/jj572370(v=win.10)) pour générer des fichiers XLIFF à partir d’autres fichiers de ressources, tels que les fichiers .resw ou .resjson.

> [!NOTE]
> La localisation peut également être nécessaire pour d’autres ressources, y compris des images et des fichiers audio.

Vous devez également prendre en compte les éléments suivants :

- **Outils de localisation** Un certain nombre d’outils de localisation sont disponibles pour analyser les fichiers de ressources et autoriser uniquement la modification des chaînes convertibles par les traducteurs. Cette approche réduit le risque qu’un traducteur modifie accidentellement les balises XML. Elle présente cependant l’inconvénient d’introduire un nouvel outil et un nouveau processus dans le processus de localisation. Un outil de localisation est parfait pour les projets avec un grand nombre de chaînes, mais un petit nombre de langages. Pour en savoir plus, voir [Comment utiliser le Kit de ressources pour application multilingue](/previous-versions/windows/apps/jj572370(v=win.10)).
- **Fournisseurs de localisation** Envisagez d’utiliser un fournisseur de localisation si votre application contient des chaînes étendues qui doivent être traduites dans un grand nombre de langues. Un prestataire de localisation peut conseiller des outils et des processus, ainsi que traduire vos fichiers de ressources. Il s’agit d’une solution idéale, mais c’est aussi l’option la plus coûteuse et elle peut augmenter le délai de traitement de votre contenu traduit.

## <a name="keep-access-keys-and-labels-consistent"></a>Maintenir la cohérence des clés d’accès et des étiquettes

La « synchronisation » des touches d’accès rapide utilisées dans les options d’accessibilité avec l’affichage des touches d’accès rapide localisées relève du défi, car les deux ressources de type chaîne sont classées dans deux sections distinctes. Veillez à fournir des commentaires pour la chaîne d’étiquette, par exemple : `Make sure that the emphasized shortcut key  is synchronized with the access key.`

## <a name="support-furigana-for-japanese-strings-that-can-be-sorted"></a>Prendre en charge Furigana pour les chaînes japonaises qui peuvent être triées

Les caractères Kanji Japonais ont la propriété de plusieurs lectures (prononciation) selon le mot dans lequel elles sont utilisées. Cela génère des problèmes lorsque vous tentez de trier des objets nommés en japonais, tels que des noms d’applications, de fichiers, de chansons, etc. Dans le passé, le Kanji japonais a généralement été trié dans une commande compréhensible par machine appelée XJIS. Malheureusement, comme cet ordre de tri n’est pas phonétique, il n’est pas très utile pour les hommes.

*Furigana* contourne ce problème en permettant à l’utilisateur ou au créateur de spécifier les phonétiques pour les caractères qu’ils utilisent. Si vous utilisez la procédure suivante pour ajouter Furigana au nom de votre application, vous pouvez vous assurer qu’il est trié à l’emplacement approprié dans la liste des applications. Si le nom de votre application contient des caractères Kanji et que Furigana n’est pas fourni lorsque la langue de l’interface utilisateur de l’utilisateur ou l’ordre de tri est défini sur japonais, Windows fait le meilleur effort pour générer la prononciation appropriée. Toutefois, il est possible de classer les noms d’applications contenant des prononciations rares ou uniques sous une prononciation plus courante. Par conséquent, la meilleure pratique pour les applications japonaises (en particulier celles contenant des caractères Kanji dans leurs noms) consiste à fournir une version Furigana de leur nom d’application dans le cadre du processus de localisation japonais.

1. Ajoutez « ms-resource:Appname » comme nom complet du package et nom complet de l’application.
2. Créez un dossier ja-JP sous le dossier « strings » et ajoutez deux fichiers de ressources comme suit :

    ``` syntax
    strings\
        en-us\
        ja-jp\
            Resources.altform-msft-phonetic.resw
            Resources.resw
    ```

3. Dans Resources.resw pour le dossier général ja-JP : ajoutez une ressource de type chaîne pour le nom d’application « 希蒼 ».
4. Dans Resources. altform-msft-Phonetic. resw pour les ressources japonaises Furigana : ajouter la valeur Furigana pour AppName « のあ »

L’utilisateur peut rechercher le nom de l’application « 希蒼 » à l’aide de la valeur Furigana « のあ » (NOA) et de la valeur phonétique (à l’aide de la fonction **GetPhonetic** de l’éditeur de méthode d’entrée (IME)) « まれあお » (Mare-AO).

Le classement suit le format de l’application **Région** du Panneau de configuration :

- Sous les paramètres régionaux d’un utilisateur japonais,
  - Si Furigana est activé, « 希蒼 » est trié sous « の ».
  - Si Furigana est manquant, « 希蒼 » est trié sous « ま ».
- Sous des paramètres régionaux utilisateur non japonais,
  - Si Furigana est activé, « 希蒼 » est trié sous « の ».
  - Si Furigana est manquant, « 希蒼 » est trié sous « 漢字 ».

## <a name="related-topics"></a>Rubriques connexes

- [Directives relatives à la globalisation](guidelines-and-checklist-for-globalizing-your-app.md)
- [Localiser les chaînes dans l’interface utilisateur et le manifeste du package d’application](../../app-resources/localize-strings-ui-manifest.md)
- [Personnaliser vos ressources pour la langue, l’échelle, le contraste élevé et d’autres qualificateurs](../../app-resources/tailor-resources-lang-scale-contrast.md)
- [Ajuster la disposition et les polices, et prendre en charge le sens du flux DàG](adjust-layout-and-fonts--and-support-rtl.md)
- [Mise à jour des images en réponse aux événements de modification de valeur de qualificateur](../../app-resources/images-tailored-for-scale-theme-contrast.md#updating-images-in-response-to-qualifier-value-change-events)

## <a name="samples"></a>Exemples

- [Exemple de ressources d’application et de localisation](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%23%5D-Windows%208%20app%20samples/C%23/Windows%208%20app%20samples/Application%20resources%20and%20localization%20sample%20(Windows%208))
