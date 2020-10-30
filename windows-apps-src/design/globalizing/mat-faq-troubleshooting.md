---
description: Cette rubrique fournit des réponses aux questions fréquemment posées et aux problèmes liés à la boîte à outils d’application multilingue (MAT) 4,0.
title: Forum aux questions sur le & de la résolution des problèmes d’application multilingue
template: detail.hbs
ms.date: 11/13/2017
ms.topic: article
keywords: Windows 10, UWP, globalisation, adaptabilité, localisation
ms.localizationpriority: medium
ms.openlocfilehash: 86c7805f92adf3551729783e2359c85103a0c13e
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93030752"
---
# <a name="multilingual-app-toolkit-40-faq--troubleshooting"></a>FAQ et résolution des problèmes liés au kit de ressources Multilingual App Toolkit 4.0

Cette rubrique fournit des réponses aux questions fréquemment posées et aux problèmes liés à la boîte à outils d’application multilingue (MAT) 4,0.

Consultez également [utiliser le kit de l’application multilingue 4,0](use-mat.md).

**Remarque** La boîte à outils prend en charge les fichiers. resw (XAML) et. resjson (JavaScript). Toutefois, dans cette rubrique, nous allons nous référer uniquement aux fichiers. resw. Un fichier. resw est connu sous le nom de fichier de ressources. Elle contient des chaînes, soit dans la langue par défaut, soit traduites dans une autre langue. Le dossier qui contient un fichier. resw est généralement nommé pour la valeur d’une balise de langue.

## <a name="do-i-need-resw-files-in-multiple-languages"></a>Ai-je besoin de fichiers. resw dans plusieurs langues ?

Non. L’un des principaux avantages de la boîte à outils est que les fichiers. resw dans plusieurs langues ne sont pas requis. Le kit de ressources gère et synchronise les ressources de votre application à l’aide des fichiers. XLF. Cela supprime les défis associés au maintien de la synchronisation du contenu sur plusieurs fichiers. resw.

Les projets qui contiennent des fichiers. resw et. XLF correspondants provoquent l’ignorance des traductions du fichier. XLF. Dans ce cas, un avertissement s’affiche pendant la génération pour vous informer que les traductions. XLF ne sont pas incluses dans l’application finale. Un fichier. resw et un fichier. XLF correspondent lorsqu’ils ont un langage cible avec le même code de langue. Un exemple d’une paire correspondante serait `Strings\de-DE\Resources.resw` et un `<project-name>.de-DE.xlf` fichier (contenant `target-language="de-DE"` ).

## <a name="can-i-have-resw-files-in-multiple-languages"></a>Puis-je avoir des fichiers. resw dans plusieurs langues ?

Vous pouvez, mais nous ne le recommandons pas. Si vous souhaitez inclure des fichiers. resw dans plusieurs langues dans votre projet et utiliser la boîte à outils, assurez-vous que vous n’avez pas de fichiers. resw et. XLF correspondants.

## <a name="i-dont-see-an-option-in-the-tools-menu-to-enable-the-multilingual-app-toolkit"></a>Je ne vois pas d’option dans le menu outils pour activer le kit d’outils d’application multilingue

Essayez ces étapes.

- Veillez à sélectionner le nœud du projet, et non le nœud de la solution, avant d’ouvrir le menu **Outils** .
- Vérifiez que l’extension du Toolkit est installée à l’aide du gestionnaire d’extensions Visual Studio.
- Confirmez que votre projet est un projet UWP.

## <a name="when-i-build-my-project-i-dont-see-a-message-saying-that-a-multilingual-app-toolkit-build-has-started"></a>Lorsque je crée mon projet, je ne vois pas un message indiquant qu’une génération d’application Toolkit multilingue a démarré

Vérifiez que vous avez activé le tapis pour votre projet. Dans le menu **Outils** , sélectionnez **Pack d’outils d’application multilingue**  >  **activer la sélection** . Si votre projet a été activé avec une version précédente, désactivez et réactivez le fond à l’aide du menu **Outils** . Cela met à jour le projet pour fonctionner avec la nouvelle version de la boîte à outils.

Assurez-vous que le composant « tâche de build pour toutes les éditions Visual Studio » a été installé. Ce composant de build est installé avec l’extension, mais il peut être désélectionné manuellement au cours de l’installation. Ce composant est nécessaire pour mettre à jour les fichiers. XLF et ajouter la traduction dans le fichier PRI. Lorsque ce composant est installé et fonctionne correctement, vous verrez ces messages de génération.

```dosbatch
1> Multilingual App Toolkit build started.
1> Multilingual App Toolkit build completed successfully.
```

## <a name="the-toolkit-is-reporting-that-it-didnt-locate-any-xliff-language-files-during-the-build"></a>La boîte à outils signale qu’elle n’a pas trouvé de fichiers de langage XLIFF pendant la génération

```dosbatch
No XLIFF language files were found. The app will not contain any localized resources.
```

Ce message s’affiche lorsque la boîte à outils ne trouve pas de fichiers dans le projet avec l’extension. XLF. La boîte à outils génère et conserve ces fichiers dans le `MultilingualResources` dossier par défaut. Elles peuvent être déplacées. mais il est préférable de les conserver dans ce dossier, car cela permet à l’éditeur multilingue de localiser les fichiers de métadonnées associés.

## <a name="my-xlf-file-is-not-included-in-the-list-of-files-processed-by-the-toolkit-during-build"></a>Mon fichier. XLF n’est pas inclus dans la liste des fichiers traités par la boîte à outils lors de la génération

Si vous excluez manuellement un fichier. XLF du projet et que vous l’incluez à nouveau, l’élément de type de fichier risque de ne pas être défini correctement. Dans Visual Studio, sélectionnez le fichier et vérifiez le Fenêtre Propriétés. L’action de génération du fichier doit être définie sur XliffResource et le copier dans le répertoire de sortie doit être défini sur ne pas copier. Voici comment la référence doit s’afficher dans votre fichier projet.

```xml
<XliffResource Include="MultilingualResources\<project-name>.fr-FR.xlf" />
```

## <a name="ive-added-xlf-based-languages-where-are-my-strings"></a>J’ai ajouté des langages basés sur XLF. Où sont mes chaînes ?

Votre fichier de ressources de langue par défaut (. resw) est votre « schéma » canonique des chaînes que votre application utilise. Les fichiers de ressources traduites peuvent contenir la totalité ou un sous-ensemble de ces chaînes.

Lorsque vous générez votre projet, vos fichiers de ressources et vos fichiers. XLF sont synchronisés.

- Les fichiers. XLF sont mis à jour pour refléter toute chaîne ajoutée ou supprimée, ou des fichiers de ressources ajoutés ou supprimés.
- Les fichiers de ressources sont mis à jour pour refléter toutes les chaînes traduites dans les fichiers. XLF.

Cela est expliqué en détail dans [la rubrique utilisation du kit de l’application multilingue 4,0](use-mat.md).

## <a name="when-i-build-my-project-the-xlf-files-remain-empty"></a>Lorsque je crée mon projet, les fichiers. XLF restent vides

Avant de pouvoir utiliser le fond efficacement, votre application doit être localisable. Cela est expliqué en détail dans [la rubrique utilisation du kit de l’application multilingue 4,0](use-mat.md).

## <a name="what-is-microsoft-translator"></a>Qu’est-ce que Microsoft Translator ?

Microsoft Translator est un service basé sur le Cloud qui fournit une traduction basée sur l’ordinateur. La traduction automatique est idéale pour accéder à la traduction lorsque la traduction humaine n’est pas raisonnable à obtenir. Pour plus d’informations, consultez [Microsoft Translator](https://www.microsofttranslator.com/).

La boîte à outils utilise le service Microsoft Translator pour vous fournir des suggestions de traduction. Vous pouvez voir les langues prises en charge par Microsoft Translator lorsque l’icône Microsoft Translator est présente dans la boîte de dialogue langues de traduction.

Vous pouvez traduire rapidement votre application à l’aide de Microsoft Translator à partir de l’éditeur multilingue en sélectionnant une chaîne et en cliquant sur **traduire** .

## <a name="what-is-pseudo-language-and-what-are-pseudo-resource-trackers"></a>Qu’est-ce que le Pseudo-langage et quels sont les « suivis de ressources Pseudo-ressources » ?

Le Pseudo-langage est une modification artificielle du produit logiciel destiné à simuler la localisation des langues réelles. Vous trouverez plus d’informations sur le Pseudo-langage et les outils de suivi des Pseudo-ressources dans [utilisation du kit de ressources d’application multilingue 4,0](use-mat.md).

## <a name="how-do-i-set-my-language-preference-to-pseudo-language-so-that-i-can-test-my-pseudo-locd-strings"></a>Comment faire définir ma préférence de langue en Pseudo-langage, afin de pouvoir tester mes chaînes Pseudo-Loc ?

Cela est expliqué dans [la rubrique utilisation de l’application mui 4,0](use-mat.md).

## <a name="what-kind-of-localizability-issues-can-i-find-using-pseudo-language"></a>Quels types de problèmes d’adaptabilité puis-je trouver en utilisant le langage Pseudo-langage ?

Cela est expliqué dans [la rubrique utilisation de l’application mui 4,0](use-mat.md).

## <a name="im-not-seeing-any-translations-when-i-launch-my-app-or-my-app-is-only-partially-translated"></a>Je ne vois pas de traductions lorsque je lance mon application, ou mon application n’est traduite que partiellement

Ouvrez le fichier. XLF dans l’éditeur multilingue pour voir si les traductions sont présentes. Lorsque les chaînes du fichier Language. resw par défaut sont modifiées explicitement, les traductions correspondantes sont supprimées des fichiers. XLF. Cela permet de s’assurer qu’une traduction correspond à sa chaîne source. Traduisez la ou les chaînes dans le ou les fichiers. XLF, puis régénérez pour mettre à jour le ou les fichiers. resw de langue non définie par défaut.

Si vos chaînes sont converties dans le ou les fichiers. XLF, mais qu’elles n’apparaissent pas dans votre application, régénérez votre projet pour mettre à jour le ou les fichiers. resw de langue non définie par défaut. Visual Studio optimise la commande de génération pour générer uniquement les fichiers qui ont été modifiés depuis la dernière génération.

Vérifiez l’ordre des préférences linguistiques. Assurez-vous que la langue que vous souhaitez tester est répertoriée en haut de votre liste de préférences de langue dans **paramètres** .

## <a name="the-toolkit-is-reporting-error--0x80004004-in-the-build-output"></a>La boîte à outils signale une erreur 0x80004004 dans la sortie de la génération

```dosbatch
Merge of Loc PRI file failed calling makepri.exe: "0x80004004"
```

Ce message peut s’afficher lorsque le format de la région est en conflit avec l’opération de génération de la boîte à outils. La solution de contournement consiste à remplacer la langue dans les **paramètres** par en-US lors de la génération.


## <a name="the-toolkit-is-reporting-error--0x80004005-in-the-build-output"></a>La boîte à outils signale l’erreur 0x80004005 dans la sortie de la génération

```dosbatch
Merge of Loc PRI file failed calling makepri.exe: "0x80004005"
```

Ce message peut s’afficher lorsque le fichier. XLF contient un langage cible non pris en charge. Par exemple, « zh-CHT » est incorrect (remplacez-le par « zh-Hant ») et « zh-CHS » est incorrect (remplacez-le par « zh-Hans »).

## <a name="is-there-a-way-to-find-out-more-information-about-the-errors-im-seeing"></a>Existe-t-il un moyen de trouver plus d’informations sur les erreurs que je vois ?

Oui, vous pouvez activer la journalisation détaillée dans Visual Studio. Cliquez sur **Outils**  >  **options**  >  **projets et solutions**  >  **générer et exécuter** . Modifiez le niveau de détail de la **sortie de génération du projet MSBuild** de minimal à normal ou plus.

L’exécution de MSBuild à partir de la ligne de commande peut également générer des messages supplémentaires.

```dosbatch
msbuild /t:rebuild <project-name>
```

## <a name="import-translation-failed"></a>Échec de l’importation de la traduction

Le processus d’importation effectue une validation de base avant l’importation. Cela permet de s’assurer que les informations de culture cible dans les fichiers importés correspondent à celles des fichiers. XLF existants. Ouvrez les fichiers. XLF dans l’éditeur multilingue et assurez-vous que les informations de culture correspondent.

## <a name="what-if-my-translator-doesnt-have-windows-10-andor-visual-studio-andor-the-multilingual-app-toolkit-installed"></a>Que se passe-t-il si mon traducteur n’a pas installé Windows 10 et/ou Visual Studio et/ou le kit d’outils d’application multilingue ?

Lorsque vous sélectionnez **sortie : destinataire du courrier** dans la boîte de dialogue Exporter des ressources de type chaîne, le message contient un lien permettant de télécharger et d’installer Multilingual App Toolkit (MAT) 4,0. Votre traducteur peut toujours installer l’outil d’édition multilingue autonome MAT 4,0 même sans Windows 10 ni Visual Studio.

Pour plus d’informations, consultez [use the Multilingual App Toolkit 4,0](use-mat.md).

## <a name="what-happened-to-the-markuprulesxml-and-resourceslocksxml-files"></a>Qu’est-il arrivé aux `MarkupRules.xml` `ResourcesLocks.xml` fichiers et ?

Le kit d’outils d’application multilingue 4,0 n’utilise pas de fichiers de verrouillage des ressources propriétaires. Au lieu de cela, la balise XLIFF 1,2 `<mrk>` est ajoutée directement au fichier. XLF pour identifier les chaînes qui ne sont pas modifiées lors de la traduction de l’ordinateur. Cela permet au fichier XLIFF d’être autonome et autorise le verrouillage des ressources par fichier.

Ces fichiers de support supplémentaires ne sont plus nécessaires et vous pouvez les supprimer en toute sécurité si vous en avez.

## <a name="what-happened-to-the-tpx-file"></a>Qu’est-il arrivé au fichier. TPX ?

Le fichier. TPX a fourni un moyen simple d’inclure `MarkupRules.xml` les `ResourcesLocks.xml` fichiers et lorsque le fichier. XLF a été envoyé pour la traduction. Cette fonctionnalité n’est plus nécessaire.

Si vous avez des traductions dans un fichier. TPX que vous devez récupérer, renommez simplement l’extension de fichier. TPX en. zip. Cela vous permet d’ouvrir et d’extraire le contenu avec l’Explorateur de fichiers ou n’importe quel outil compatible. zip.

## <a name="i-think-ive-done-everything-right-but-it-still-isnt-working"></a>Je pense que j’ai fait tout ce qui est correct, mais cela ne fonctionne toujours pas

Essayez ces étapes.

1. Ajoutez des traductions à l’aide de l’une des méthodes déjà décrites.
2. Videz le fichier. pri (consultez [MakePri.exe options de ligne de commande](../../app-resources/makepri-exe-command-options.md)) pour voir si vos traductions se trouvent dans le fichier. pri. Les traductions s’affichent avec le code de langue et la valeur traduite comme suit.
   ```xml
   <Candidate qualifiers="Language-QPS-PLOC" type="String">
       <Value>[!!_Ŝéãřćĥ_!!]</Value>
   </Candidate>
   ```
3. Créez à partir d’une invite de commandes ; l’erreur résultante peut avoir plus de détails que ce qui est signalé dans la sortie de la génération.

## <a name="my-app-failed-certification-to-the-microsoft-store"></a>Mon application n’a pas de certification pour le Microsoft Store

Avant de commencer le processus de certification Microsoft Store, vous devez exclure le `<project-name>.qps-ploc.xlf` fichier de votre projet. Le Pseudo-langage est utilisé pour détecter les problèmes potentiels d’adaptabilité ou les bogues, mais il ne s’agit pas d’un langage de Microsoft Store valide. S’il n’est pas supprimé, votre application échouera au cours du processus de certification Microsoft Store.

## <a name="related-topics"></a>Rubriques connexes

* [Utiliser le Kit de ressources Multilingual App Toolkit 4.0](use-mat.md)
* [Microsoft Translator](https://www.microsofttranslator.com/)
* [Options de ligne de commande de MakePri.exe](../../app-resources/makepri-exe-command-options.md)
