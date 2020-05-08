---
Description: Multilingual App Toolkit (MAT) 4,0 s’intègre à Microsoft Visual Studio 2019 pour fournir aux applications Windows une prise en charge de la traduction, la gestion des fichiers de traduction et des outils d’édition.
title: Utiliser la boîte à outils d’application multilingue
template: detail.hbs
ms.date: 01/23/2018
ms.topic: article
keywords: Windows 10, UWP, globalisation, adaptabilité, localisation
ms.localizationpriority: medium
ms.openlocfilehash: f11ee858be112db899e0fd25dd2fe274d5a092fd
ms.sourcegitcommit: 0dee502484df798a0595ac1fe7fb7d0f5a982821
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/08/2020
ms.locfileid: "82970964"
---
# <a name="use-the-multilingual-app-toolkit-40"></a>Utiliser le Kit de ressources Multilingual App Toolkit 4.0

Multilingual App Toolkit (MAT) 4,0 s’intègre à Microsoft Visual Studio 2019 pour fournir aux applications Windows une prise en charge de la traduction, la gestion des fichiers de traduction et des outils d’édition. Voici quelques-unes des propositions de valeur de la boîte à outils.

- Vous aide à gérer les modifications de ressources et l’état de la traduction pendant le développement.
- Fournit une interface utilisateur permettant de choisir des langues basées sur des fournisseurs de traduction configurés.
- Prend en charge le format de fichier XLIFF standard du secteur de la localisation.
- Fournit un moteur de Pseudo-langage pour aider à identifier les problèmes de traduction pendant le développement.
- Établit une connexion avec le portail linguistique Microsoft pour accéder facilement aux chaînes et à la terminologie traduites.
- Se connecte à Microsoft Translator pour des suggestions de traduction rapide.

## <a name="how-to-use-the-toolkit"></a>Comment utiliser la boîte à outils

### <a name="step-1-design-your-app-for-globalization-and-localization"></a>Étape 1. Concevoir votre application pour la globalisation et la localisation

Avant de pouvoir utiliser le fond efficacement, votre application doit être localisable. Plus précisément, votre projet doit contenir un ou plusieurs fichiers de ressources (. resw) contenant les chaînes de votre application dans la langue par défaut. Pour plus d’informations, consultez [localiser des chaînes dans votre interface utilisateur et le manifeste du package d’application](../../app-resources/localize-strings-ui-manifest.md). Une fois que vous avez terminé, le Toolkit facilite et simplifie l’ajout de langues supplémentaires.

Pour obtenir la proposition de valeur de globalisation et de localisation&mdash;, ainsi que les définitions des termes de **globalisation**, d' **adaptabilité**et de **localisation**&mdash;, consultez [globalisation et localisation](globalizing-portal.md).

Consultez également [les instructions pour la globalisation](guidelines-and-checklist-for-globalizing-your-app.md) et [rendre votre application localisable](prepare-your-app-for-localization.md).

### <a name="step-2-download-and-install-the-multilingual-app-toolkit-40"></a>Étape 2. Télécharger et installer le kit d’outils d’application multilingue 4,0

Il existe deux parties à la boîte à outils de l’application multilingue 4,0 (MAT 4,0), chacune avec son propre programme d’installation.

- [Extension multilingue d’application Toolkit 4,0 pour Visual Studio 2017 et versions ultérieures](https://marketplace.visualstudio.com/items?itemName=MultilingualAppToolkit.MultilingualAppToolkit-18308). Contient l’extension MAT 4,0 pour Visual Studio 2019, sous la forme d’un programme d’installation. vsix.
- [Éditeur Multilingual App Toolkit 4,0](https://developer.microsoft.com/windows/develop/multilingual-app-toolkit). Il contient l’outil d’édition multilingue autonome MAT 4,0, sous la forme d’un programme d’installation. msi. Il comprend également l’extension MAT 4,0 pour Visual Studio 2015 et pour Visual Studio 2013.

Si vous utilisez Visual Studio 2017 ou Visual Studio 2019, téléchargez et exécutez les deux programmes d’installation, l’un après l’autre. Si vous utilisez Visual Studio 2015 ou Visual Studio 2013, téléchargez et exécutez le programme d’installation. msi.

### <a name="step-3-enable-the-multilingual-app-toolkit-for-your-project"></a>Étape 3. Activer la boîte à outils d’application multilingue pour votre projet

Pour que vous puissiez commencer à localiser l’application, vous devez activer la MAT pour votre projet. Voici comment activer la boîte à outils.

- Ouvrez la solution de projet dans Visual Studio.
- Sélectionnez le projet souhaité dans Explorateur de solutions.
- Dans le menu **Outils** , sélectionnez **Pack d’outils** > d’application multilingue**activer la sélection**. 

Dans la fenêtre sortie (affichage de la sortie de la boîte à outils d’application multilingue `Project '<project-name>' was enabled. The project's source culture is '<language-tag>' <language-name>`), observez le message. Si ce message s’affiche, le tapis est prêt à être utilisé.

### <a name="step-4-add-languages-to-your-project"></a>Étape 4. Ajouter des langues à votre projet

Procédez comme suit pour ajouter des langues à votre projet.

1. Cliquez avec le bouton droit sur le nœud du projet dans l'Explorateur de solutions.
2. Cliquez sur boîte à >  **Outils d’application multilingue****Ajouter des langues de traduction...**.
3. Dans la boîte de dialogue langues de traduction, sélectionnez la ou les langues que vous souhaitez prendre en charge, puis cliquez sur OK.

Le Toolkit effectue ces actions en réponse.

- Pour chaque langue que vous avez ajoutée, un nouveau dossier est créé nommé pour la [balise de langue BCP-47](https://tools.ietf.org/html/bcp47) de la langue. À l’intérieur de ce dossier, de nouveaux fichiers de ressources (. resw) sont créés pour correspondre à un ou plusieurs fichiers contenant les chaînes de langue par défaut.
- S’il s’agit de la première fois que vous ajoutez une langue, un nouveau `MultilingualResources` dossier nommé est ajouté au projet. À l’intérieur de ce dossier, un fichier. XLF est ajouté pour chaque langue. Les fichiers. XLF contiennent une unité de traduction pour chaque chaîne dans chaque fichier de ressources (. resw) de votre projet.
- La fenêtre Sortie confirme l’ajout de la ou des langues que vous avez ajoutées.

Chaque fois que vous ajoutez/supprimez un fichier de ressources de langue par défaut (. resw), ou que vous ajoutez/supprimez une chaîne dans un fichier de ressources de langue par défaut (. resw), régénérez le projet pour resynchroniser les fichiers. XLF. Cela permet de s’assurer que les fichiers. XLF contiennent l’Union des chaînes dans la langue par défaut.

Des fournisseurs&mdash;de traduction installés tels que le [portail linguistique Microsoft](https://www.microsoft.com/Language/) et [Microsoft Translator](https://www.microsofttranslator.com/)&mdash;peuvent être utilisés pour traduire les ressources de votre application. Lorsqu’un fournisseur prend en charge une langue spécifique, l’icône du fournisseur s’affiche à côté du nom de la langue dans la boîte de dialogue langues de traduction.

Dans la boîte de dialogue langues de traduction, la zone de sélection de tous les langages. XLF existants détectés par la boîte à outils est précochée pour indiquer que la langue est déjà incluse dans le projet.

Une fois qu’une langue est ajoutée au projet, elle ne peut pas être supprimée en désactivant la case à cocher dans la boîte de dialogue langues de traduction. Pour supprimer une langue, cliquez avec le bouton droit sur le fichier. XLF spécifique à une langue, puis sélectionnez **supprimer**. Si vous confirmez, cette opération supprimera également le fichier de ressources correspondant (. resw).

### <a name="step-5-test-your-app-using-pseudo-language"></a>Étape 5. Tester votre application à l’aide de Pseudo-langage

Le Pseudo-langage est une modification artificielle du produit logiciel destiné à simuler la localisation du langage réel, mais qui reste lisible aux orateurs natifs. La Pseudo-traduction remplace les caractères et développe la longueur de la chaîne de ressource pour détecter les problèmes potentiels d’adaptabilité ou les bogues au début du cycle du projet et avant le début de la localisation dans commençaient.

Procédez comme suit pour Pseudo-localiser et tester votre projet.

1. Utilisez la boîte de dialogue langues de traduction pour ajouter le Pseudo-langage (Pseudo) [RPS-Ploc] à votre projet.
2. Dans Explorateur de solutions, cliquez `<project-name>.qps-ploc.xlf` avec le bouton droit sur le fichier, puis cliquez sur boîte à outils > d' **application multilingue****générer des traductions de machine**.
3. Dans **paramètres** > de l'**heure & langue** > **&** > **langues**, cliquez sur **Ajouter une langue**.
5. Dans la zone de recherche, tapez `qps-ploc`.
6. Cliquez `English (qps-ploc)` pour l’ajouter.
7. Dans la liste langue, sélectionnez `English (qps-ploc)` , puis cliquez sur **définir par défaut**.
8. Testez votre application Pseudo-localisée. Par exemple, recherchez les problèmes de disposition de l’interface utilisateur où la totalité d’une chaîne n’est pas affichée (la chaîne est tronquée) ou les chaînes qui ne sont pas traduites (mais à code dur).

Outre le remplacement et l’expansion de caractères, le Pseudo-moteur fournit un identificateur de suivi unique pour chaque ressource. Ce dispositif de suivi est ajouté au début de chaque chaîne et placé entre `[xxxxx]`crochets. Vous pouvez utiliser ces suivis pendant le test d’inspection de l’interface utilisateur de Visual. Ils peuvent faciliter le suivi des ressources spécifiques dans le produit, en particulier si plusieurs ressources ont un texte similaire ou dupliqué.

Dans ce « Hello, World ! » exemple de texte, la Pseudo traduction se développe pour prendre environ 30 pour cent d’espace d’écran, puis applique le dispositif de suivi des ressources.

`"Hello World" -> "Ĥèĺļõ Ŵòŗłđ" -> "[!!_Ĥèĺļõ Ŵòŗłđ_!!]" -> "[hJ8s1][!!_Ĥèĺļõ Ŵòŗłđ_!!]"`

### <a name="step-6-translate-your-app-into-selected-languages"></a>Étape 6. Traduire votre application dans les langues sélectionnées

Le kit d’outils d’application multilingue est intégré au processus de génération. Pendant une génération, les chaînes mises à jour sont automatiquement ajoutées à chaque fichier Language. XLF.
Une fois que vous avez testé votre application à l’aide de Pseudo-langage, vous avez le choix entre trois options pour traduire votre application dans d’autres langues pour la version finale.

#### <a name="option-1-translate-the-strings-yourself"></a>Option 1. Traduisez les chaînes vous-même

Vous pouvez utiliser l’éditeur multilingue pour traduire des chaînes individuellement. Comme mentionné précédemment, cela est inclus dans [le programme d’installation. msi](https://developer.microsoft.com/windows/develop/multilingual-app-toolkit).

- Cliquez avec le bouton droit sur le fichier. XLF que vous souhaitez traduire.
- Cliquez sur **Ouvrir avec...** , puis sélectionnez Éditeur multilingue. Vous pouvez éventuellement cliquer sur **définir comme valeur par défaut**.
- Pour chaque chaîne, **source** affiche la chaîne d’origine dans la langue par défaut. Dans **traduction**, tapez la chaîne traduite dans la langue appropriée pour le fichier. XLF que vous modifiez.
- Lorsque vous avez terminé, enregistrez et fermez le fichier.

Régénérez votre projet pour que les chaînes traduites soient copiées dans le fichier de ressources (. resw) qui correspond au fichier. XLF que vous venez de modifier.

Vous pouvez également lancer l’éditeur multilingue comme celui-ci. Accédez à démarrer, afficher toutes les applications, ouvrez le dossier Application Pack multilingue, puis cliquez sur éditeur multilingue pour le lancer.

#### <a name="option-2-send-the-xlf-files-to-a-third-party-for-translation"></a>Option 2. Envoyer les fichiers. XLF à un tiers pour la traduction

Pour répartir le travail de traduction et de modification sur les localisateurs, sélectionnez les fichiers. XLF souhaités dans Explorateur de solutions, cliquez dessus avec le bouton droit, puis cliquez sur la boîte à >  **Outils d’application multilingue**exporter les**traductions...**.

Sélectionnez **sortie : destinataire du courrier** dans la boîte de dialogue Exporter les ressources de chaîne, puis cliquez sur OK. vos fichiers sont compressés et joints à un nouvel e-mail. Sélectionnez **sortie : emplacement du dossier de fichiers**, navigateur pour un dossier, cliquez sur OK, choisissez éventuellement pour les fichiers à compresser, cliquez à nouveau sur OK. vos fichiers seront (zippés et) enregistrés à l’emplacement que vous avez choisi, à l’intérieur d’un nouveau dossier nommé pour votre projet.

Une fois que vos localisateurs ont terminé le travail de traduction et vous a envoyé les fichiers. XLF traduits, vous pouvez les importer dans votre projet. Sélectionnez les fichiers. XLF souhaités dans Explorateur de solutions, cliquez dessus avec le bouton droit, puis cliquez sur **multilingue application Toolkit** > **Importer/recycler les traductions...**. Cliquez sur **Ajouter**, accédez aux fichiers. XLF ou. zip, puis cliquez sur **Importer**.

**Remarque** Le processus d’importation effectue une validation de base avant l’importation. Cela permet de s’assurer que les informations de culture cible dans les fichiers importés correspondent à celles des fichiers. XLF existants.

Régénérez votre projet pour que les chaînes traduites soient copiées dans le ou les fichiers de ressources (. resw) qui correspondent au (x) fichier (s). XLF que vous venez d’importer.

Ces fournisseurs tiers offrent des services de localisation et peuvent être en mesure de vous aider.

- [Elanex](https://www.strakertranslations.com/)
- [Keywords Studios](https://www.keywordsstudios.com/)
- [Lionbridge](https://www.lionbridge.com)
- [Moravia](https://www.rws.com/what-we-do/rws-moravia/)
- [MAINTAIN](https://www.sdl.com/translate/get-started/instant-quote.html)
- [Welocalize](https://www.welocalize.com/)

> [!NOTE]
> La liste ci-dessus est fournie à titre d’information uniquement et n’est pas une approbation. Microsoft n’émet aucune déclaration ni aucune garantie à l’égard de ces fournisseurs ou de leurs services et ne pourra en aucun cas être tenu responsable en cas de recours à ces derniers. Toute question, réclamation ou réclamation concernant de tels fournisseurs ou leurs services doit être dirigée vers le fournisseur approprié.

#### <a name="option-3-use-the-integrated-translation-services"></a>Option 3. Utiliser les services de traduction intégrés

Les services de traduction sont intégrés à l’IDE de Visual Studio, ainsi qu’à l’éditeur multilingue. Cela permet d’accéder facilement aux services de traduction tout en développant votre produit et en localisé vos ressources. Pour ce service, vous avez besoin d’un abonnement à un compte Azure, comme décrit dans [Microsoft Translator pour passer au portail Azure](https://multilingualapptoolkit.uservoice.com/knowledgebase/articles/1167898-microsoft-translator-moves-to-the-azure-portal).

Pour accéder aux services de traduction à l’intérieur de Visual Studio, sélectionnez et cliquez avec le bouton droit sur un ou plusieurs fichiers. XLF dans Explorateur de solutions, puis cliquez sur **générer des traductions d’ordinateurs**.

L’éditeur multilingue fournit la même prise en charge de traduction, ainsi que l’ajout de suggestions de traduction interactives, qui vous permettent de sélectionner la traduction qui correspond le mieux à vos chaînes de ressources. Une fois la suggestion de traduction fournie, vous pouvez ajuster la chaîne pour votre style de traduction.

Deux fournisseurs sont fournis avec le kit d’outils d’application multilingue.

- Le fournisseur du [portail de langage Microsoft](https://www.microsoft.com/Language/) permet la prise en charge de la traduction et du recyclage en fonction des traductions du texte de l’interface utilisateur pour les produits et services Microsoft.
- Le fournisseur [Microsoft Translator](https://www.microsofttranslator.com/) active les services de traduction automatique à la demande.

Vous et vos traducteurs pouvez gérer l’état des traductions dans l’éditeur multilingue pour revoir ultérieurement les traductions incertaines. Vous pouvez définir l’état de chaque chaîne sous l’onglet **Propriétés** . les valeurs d’État sont : **nouveau**, **révision**, **traduit**, **final**et **déconnecté**. L’indicateur à gauche de la ligne indique l’État. Lorsque toutes les lignes affichent le vert dans l’éditeur multilingue, votre travail de traduction est terminé.

Régénérez votre projet pour que les chaînes traduites soient copiées dans le ou les fichiers de ressources (. resw) qui correspondent au (x) fichier (s). XLF que vous venez de modifier.

### <a name="step-7-upload-your-app-to-the-microsoft-store"></a>Étape 7. Téléchargez votre application sur le Microsoft Store

Avant de commencer le processus de certification Microsoft Store, vous devez exclure `<project-name>.qps-ploc.xlf` le fichier de votre projet. Le Pseudo-langage est utilisé pour détecter les problèmes potentiels d’adaptabilité ou les bogues, mais il ne s’agit pas d’un langage de Microsoft Store valide. S’il n’est pas supprimé, votre application échouera au cours du processus de certification Microsoft Store.

## <a name="related-topics"></a>Rubriques connexes

* [Localiser les chaînes dans l’interface utilisateur et le manifeste du package d’application](../../app-resources/localize-strings-ui-manifest.md)
* [Globalisation et localisation](globalizing-portal.md)
* [Directives relatives à la globalisation](guidelines-and-checklist-for-globalizing-your-app.md)
* [Rendre votre application localisable](prepare-your-app-for-localization.md)
* [Balise de langue BCP-47](https://tools.ietf.org/html/bcp47)

## <a name="downloads"></a>Téléchargements

* [Programme d’installation de l’application multilingue 4,0. vsix](https://marketplace.visualstudio.com/items?itemName=MultilingualAppToolkit.MultilingualAppToolkit-18308)
* [Programme d’installation du kit d’outils d’application multilingue 4,0. msi](https://developer.microsoft.com/windows/develop/multilingual-app-toolkit)

## <a name="translation-services"></a>Services de traduction

* [Portail des langues Microsoft](https://www.microsoft.com/Language/)
* [Microsoft Translator](https://www.microsofttranslator.com/)
