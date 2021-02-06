---
title: Instructions de conception Cortana-conception et développement UWP de Cortana
description: Ces instructions et recommandations décrivent comment votre application peut utiliser Cortana pour interagir avec l’utilisateur.
ms.assetid: 332ccb95-0e56-410e-ab63-cc028fce4192
label: Cortana
template: detail.hbs
ms.date: 01/27/2021
ms.topic: article
keywords: Cortana, conception
ms.localizationpriority: medium
ms.openlocfilehash: b7711f9fd653bbd635582a0b2268a5eb0ed7417b
ms.sourcegitcommit: 8fe992f3a6d8f7975af4911ad88e855bee50083e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/05/2021
ms.locfileid: "99606084"
---
# <a name="cortana-design-guidelines"></a>Recommandations relatives à la conception de Cortana

>[!WARNING]
> Cette fonctionnalité n’est plus prise en charge à partir de la mise à jour Windows 10 2020 (version 2004, nom de nom « 20H1 »).
>
> Consultez [Cortana dans Microsoft 365](/microsoft-365/admin/misc/cortana-integration) pour savoir comment Cortana transforme les expériences de productivité modernes.

Ces instructions et recommandations décrivent comment votre application peut utiliser **Cortana** de manière optimale pour interagir avec l’utilisateur, l’aider à effectuer une tâche et communiquer clairement comment elle se produit.

**Cortana** permet aux applications qui s’exécutent en arrière-plan d’inviter l’utilisateur à confirmer ou à lever l’ambiguïté, et, en retour, fournit à l’utilisateur des commentaires sur l’état de la commande vocale. Le processus est léger, rapide et ne force pas l’utilisateur à conserver l’expérience **Cortana** ou à basculer le contexte vers l’application.

Bien que l’utilisateur ait l’impression que **Cortana** contribue à rendre le processus plus clair et facile que possible, il est préférable que **Cortana** soit aussi explicite qu’il s’agit de votre application qui accomplit la tâche.

Nous utilisons une application de planification et de gestion de voyage nommée **Adventure Works** intégrée dans l’interface utilisateur de **Cortana** , illustrée ici, pour illustrer la plupart des concepts et fonctionnalités que nous abordons. Pour plus d’informations, consultez l' [exemple de commande vocale Cortana](https://go.microsoft.com/fwlink/p/?LinkID=619899).

:::image type="content" source="images/cortana/cortana-overview.png" alt-text="Capture d’écran de la zone de dessin Cortana":::

## <a name="conversational-writing"></a>Écriture conversationnel

Les interactions avec **Cortana** réussies vous obligent à suivre certains principes fondamentaux lors de la création de chaînes TTS (Text-to-Speech) et gui.

<table>
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Principe</th>
<th align="left">Exemple incorrect</th>
<th align="left">Bon exemple</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p></p>
<dl>
<dt>Possible</dt>
<dd><p>Utilisez le moins de mots possible et mettez les informations les plus importantes à l’avant.</p>
</dd>
</dl></td>
<td align="left"><p>Bien sûr, qu’est-ce que vous pouvez rechercher aujourd’hui ? Nous avons une grande collection.</p></td>
<td align="left"><p>Bien sûr, quel film recherchez-vous ?</p></td>
</tr>
<tr class="even">
<td align="left"><p></p>
<dl>
<dt>Nécessaire</dt>
<dd><p>Fournissez des informations pertinentes uniquement pour la tâche, le contenu et le contexte.</p>
</dd>
</dl></td>
<td align="left"><p>J’ai ajouté ceci à votre sélection. Vous savez simplement que votre batterie devient insuffisante.</p></td>
<td align="left"><p>J’ai ajouté ceci à votre sélection.</p></td>
</tr>
<tr class="odd">
<td align="left"><p></p>
<dl>
<dt>Effacé</dt>
<dd><p>Évitez toute ambiguïté. Utilisez le langage quotidien plutôt que le jargon technique.</p>
</dd>
</dl></td>
<td align="left"><p>Aucun résultat pour les &quot; voyages de requêtes à Las Vegas &quot; .</p></td>
<td align="left"><p>Je n’ai pas trouvé de voyage à Las Vegas.</p></td>
</tr>
<tr class="even">
<td align="left"><p></p>
<dl>
<dt>Sûr </dt>
<dd><p>Soyez le plus précis possible. Soyez transparent sur ce qui se passe en arrière-plan : si une tâche n’est pas encore terminée, ne le précisez pas. Respecter la confidentialité — ne pas lire les informations privées à haute voix.</p>
</dd>
</dl></td>
<td align="left"><p>Je n’ai pas pu trouver ce film, il ne doit pas encore avoir été relâché.</p></td>
<td align="left"><p>Je n’ai pas trouvé ce film dans notre catalogue.</p></td>
</tr>
</tbody>
</table>

Écrivez comment les gens parlent. N’insistez pas sur la précision grammaticale par rapport aux sons naturels. Par exemple, les raccourcis verbals respectueux de l’oreille comme « veulent » ou « vite » conviennent parfaitement à la lecture TTS.

Utilisez la première personne impliquée dans la mesure du possible et du naturel. Par exemple, « si vous recherchez votre prochain voyage Adventure Works », cela signifie qu’une personne effectue la recherche, mais n’utilise pas le mot « I » pour spécifier.

Utilisez des variantes pour rendre le son de votre application plus naturel. Fournissez différentes versions de vos chaînes TTS et GUI pour prononcer la même chose. Par exemple, « quel film souhaitez-vous voir ? » peut avoir des alternatives comme « quel film aimeriez-vous regarder ? ». Les gens n’affirment pas exactement la même façon à chaque fois. Veillez à maintenir la synchronisation de vos versions TTS et GUI.

Utilisez des expressions telles que « OK » et « tout droit » dans vos réponses judicieusement. Bien qu’elles puissent fournir des accusés de réception et un sens de la progression, elles peuvent également être répétées si elles sont utilisées trop souvent et sans variation.

> [!NOTE]
> Utilisez des expressions d’accusé de réception dans TTS uniquement. En raison de l’espace limité sur le canevas **Cortana** , ne les répétez pas dans les chaînes de l’interface utilisateur graphique correspondantes.

Utilisez des contractions dans vos réponses pour des interactions plus naturelles et un gain d’espace supplémentaire sur le canevas **Cortana** . Par exemple, « je ne parviens pas à trouver ce film » au lieu de « je n’ai pas pu trouver ce film ». Écrivez pour l’oreille, pas l’oeil.

Utilisez la langue que le système comprend. Les utilisateurs ont tendance à répéter les termes avec lesquels ils sont présentés. Identifiez ce que vous affichez.

Utilisez une variation de vos réponses en faisant pivoter ou en sélectionnant de manière aléatoire à partir d’une collection de réponses alternatives. Par exemple, « quel film souhaitez-vous voir ? » et « quel film aimeriez-vous regarder ? ». Cela rend le son de votre application plus naturel et unique.

## <a name="localization"></a>Localisation

Pour lancer une action à l’aide d’une commande vocale, votre application doit inscrire des commandes vocales dans la langue choisie par l’utilisateur sur son appareil (paramètres du système de reconnaissance vocale du système de paramètres &gt; &gt; &gt; ).

Vous devez localiser les commandes vocales auxquelles votre application répond et toutes les chaînes TTS et GUI.

Vous devez éviter de longues chaînes GUI. La zone de dessin **Cortana** fournit trois lignes pour les réponses et tronque les chaînes plus longues.

Pour plus d’informations, consultez la [section globalisation et localisation](/windows/uwp/design/globalizing/guidelines-and-checklist-for-globalizing-your-app).

## <a name="image-resources-and-scaling"></a>Ressources d’image et mise à l’échelle

Les applications de plateforme Windows universelle (UWP) peuvent sélectionner automatiquement l’image du logo d’application le plus approprié en fonction de paramètres spécifiques et de fonctionnalités de l’appareil (contraste élevé, pixels effectifs, paramètres régionaux, etc.). Il vous suffit de fournir les images et de vous assurer que vous utilisez la Convention d’affectation de noms et l’organisation de dossiers appropriées dans le projet d’application pour les différentes versions de ressource. Si vous ne fournissez pas les versions de ressources recommandées, l’accessibilité, la localisation et la qualité de l’image peuvent être affectées, en fonction des préférences, des capacités, du type d’appareil et de l’emplacement de l’utilisateur.

Pour plus d’informations sur les ressources d’image pour les facteurs de contraste et d’échelle élevés, consultez Instructions pour les éléments multimédias en [mosaïque et en icône](/windows/uwp/app-resources/images-tailored-for-scale-theme-contrast).

Vous nommez des ressources à l’aide de qualificateurs. Les qualificateurs de ressources sont des modificateurs de dossiers et de noms de fichiers qui identifient le contexte dans lequel une version particulière d’une ressource doit être utilisée.

La Convention d’affectation de noms standard est « NomDossier/qualifierName-value \[ \_ qualifierName \] /filename.qualifierName-value \[ \_ qualifierName-value \] . ext ». Par exemple : images/logo. Scale et 100 \_contrast-white.png est simplement référencé dans le code à l’aide du dossier racine et du nom de fichier : images/logo.png. Consultez [gérer la langue et la région](/windows/uwp/design/globalizing/manage-language-and-region) et [Comment nommer des ressources à l’aide de qualificateurs](/previous-versions/windows/apps/hh965324(v=win.10)).

Nous vous recommandons de marquer la langue par défaut des fichiers de ressources de type chaîne (par exemple, « en-US \\ Resources. resw ») et le facteur d’échelle par défaut sur les images (par exemple, « logo.scale-100.png »), même si vous n’envisagez pas de fournir actuellement des ressources localisées ou à plusieurs résolutions. Toutefois, nous vous recommandons, au minimum, de fournir des ressources pour les facteurs de mise à l’échelle 100, 200 et 400.

> [!IMPORTANT]
> L’icône d’application utilisée dans la zone de titre de la zone de dessin Cortana est l’icône Square44x44Logo spécifiée dans le fichier « Package. appxmanifest ».

Vous pouvez également spécifier une icône pour chaque mosaïque de résultats pour une requête utilisateur. Les tailles d’image valides pour les icônes de résultats sont :

- 68W x 68h
- 68W x 92h
- 280W x 140h

## <a name="result-tile-templates"></a>Modèles de vignettes de résultat

Un ensemble de modèles est fourni pour les vignettes de résultats affichées sur le canevas Cortana. Utilisez ces modèles pour spécifier le titre de la vignette et si la vignette contient du texte et une image d’icône de résultat. Chaque vignette peut inclure jusqu’à trois lignes de texte et une image, selon le modèle spécifié.

Voici les modèles pris en charge (avec des exemples) :

| Nom | Exemple |
| --- | --- |
| Titre uniquement  | :::image type="content" source="images/cortana/voicecommandcontenttiletype-titleonly-small.png" alt-text="Capture d’écran de la zone de dessin Cortana montrant uniquement le titre"::: |
| Titre avec texte | :::image type="content" source="images/cortana/voicecommandcontenttiletype-titlewithtext-small.png" alt-text="Capture d’écran de la zone de dessin Cortana montrant le titre avec du texte"::: |
| Titre avec l’icône de 68x68 | aucune image |
| Titre avec l’icône et le texte 68x68 | :::image type="content" source="images/cortana/voicecommandcontenttiletype-titlewith68x68iconandtext-small.png" alt-text="Capture d’écran de la zone Cortana montrant le titre avec l’icône et le texte 68x68"::: |
| Titre avec l’icône de 68x92 | aucune image |
| Titre avec l’icône et le texte 68x92 | :::image type="content" source="images/cortana/voicecommandcontenttiletype-titlewith68x92iconandtext-small.png" alt-text="Capture d’écran de la zone Cortana montrant le titre avec l’icône et le texte 68x92"::: |
| Titre avec l’icône de 280x140 | aucune image |
| Titre avec l’icône et le texte 280x140 | :::image type="content" source="images/cortana/voicecommandcontenttiletype-titlewith280x140iconandtext-small.png" alt-text="Capture d’écran de la zone Cortana montrant le titre avec l’icône et le texte 280x140"::: |

Pour plus d’informations sur les modèles Cortana, consultez [VoiceCommandContentTileType](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandContentTileType) .

## <a name="example"></a>Exemple

Cet exemple illustre un workflow de bout en bout pour une application en arrière-plan dans **Cortana**. Nous utilisons l’application **Adventure Works** pour annuler un voyage à Las Vegas. Cet exemple utilise le modèle « titre avec l’icône et le texte 68x68 ».

:::image type="content" source="images/cortana/e2e-canceltrip.png" alt-text="Capture d’écran de la zone de dessin Cortana pour le workflow de l’application en arrière-plan Cortana de bout en bout":::

Voici les étapes décrites dans cette image :

1. L’utilisateur appuie sur le microphone pour initier **Cortana**.
2. L’utilisateur dit « annuler mon voyage Adventure Works à Vegas » pour lancer l’application **Adventure Works** en arrière-plan. L’application utilise la voix et la zone de travail **Cortana** pour interagir avec l’utilisateur.
3. **Cortana** passe à un écran de remise qui donne des commentaires sur l’accusé de réception de l’utilisateur (« je vais faire fonctionner Adventure Works sur cela. »), une barre d’État et un bouton Annuler.
4. Dans ce cas, l’utilisateur a plusieurs voyages qui correspondent à la requête. par conséquent, l’application fournit un écran de désambiguïsation qui répertorie tous les résultats de correspondance et demande « que voulez-vous annuler ? ».
5. L’utilisateur spécifie l’élément « Conférence technique Vegas ».
6. Étant donné que l’annulation ne peut pas être annulée, l’application fournit un écran de confirmation qui demande à l’utilisateur de confirmer son intention.
7. L’utilisateur indique « Oui ».
8. L’application fournit ensuite un écran de saisie semi-automatique qui indique le résultat de l’opération.

Nous explorons ces étapes plus en détail ici.

### <a name="handoff"></a>Handoff

:::image type="content" source="images/cortana/cortana-backgroundapp-result.png" alt-text="Capture d’écran de la zone de travail Cortana pour le workflow de l’application en arrière-plan Cortana de bout en bout à l’aide d’AdventureWorks voyage à venir sans aucun transfert":::*AdventureWorks « prochain voyage » sans écran de remise*

:::image type="content" source="images/cortana/cortana-backgroundapp-progress-result.png" alt-text="Capture d’écran de la zone de travail Cortana pour le workflow de l’application en arrière-plan Cortana de bout en bout à l’aide d’AdventureWorks voyage à venir avec le transfert":::*AdventureWorks « prochain voyage » avec écran de remise*

Les tâches qui prennent moins de 500 ms pour répondre à votre application et qui ne nécessitent aucune information supplémentaire de la part de l’utilisateur peuvent être effectuées sans aucune participation supplémentaire de **Cortana**, à l’exception de l’écran de saisie semi-automatique.

Si votre application nécessite plus de 500 ms pour répondre, **Cortana** fournit un écran de remise. L’icône et le nom de l’application s’affichent, et vous devez fournir des chaînes de transfert de l’interface graphique et du TTS pour indiquer que la commande vocale a été correctement comprise. L’écran de remise s’affichera jusqu’à 5 secondes. Si votre application ne répond pas dans ce délai, **Cortana** présente un écran d’erreur générique.

### <a name="gui-and-tts-guidelines-for-handoff-screens"></a>Instructions relatives à l’interface graphique utilisateur et TTS pour les écrans de transfert

Indiquez clairement que la tâche est en cours.

Utilisez l’intensité actuelle.

Utilisez un verbe d’action qui confirme le lancement de la tâche et la référence à l’entité spécifique.

Utilisez un verbe générique qui n’est pas validé sur l’action incomplète demandée. Par exemple, « à la recherche de votre voyage » au lieu de « annuler votre voyage ». Dans ce cas, si aucun résultat n’est retourné, l’utilisateur n’entend pas «annuler votre voyage à Las Vegas... Je n’ai pas trouvé de voyage à Las Vegas».

Il est clair que la tâche n’a pas déjà été effectuée si l’application doit toujours résoudre l’entité demandée. Par exemple, remarquez que nous disons « à la recherche de votre voyage » au lieu de « annuler votre voyage », car zéro, un ou plusieurs voyages peuvent être mis en correspondance, et nous ne connaissons pas encore le résultat.

Les chaînes GUI et TTS peuvent être identiques, mais elles ne doivent pas être les mêmes. Essayez de conserver la chaîne GUI Short pour éviter la troncation et la duplication d’autres ressources visuelles.

| TTS | Interface graphique utilisateur |
| --- | --- |
| À la recherche de votre prochain voyage Adventure Works. | À la recherche de votre prochain voyage... |
| Recherche de votre voyage Adventure Works pour aller à la ville. | Recherche du trajet vers la ville... |

### <a name="progress"></a>Avancement

:::image type="content" source="images/cortana/e2e-canceltrip-progress.png" alt-text="Capture d’écran de la zone de travail Cortana pour le workflow de l’application en arrière-plan Cortana de bout en bout avec AdventureWorks annulation de la progression":::*AdventureWorks « annuler le trajet »*

Quand une tâche prend un certain temps, votre application doit effectuer un pas à pas détaillé et mettre à jour l’utilisateur sur ce qui se passe sur un écran de progression. L’icône d’application s’affiche, et vous devez fournir les chaînes de progression de l’interface graphique utilisateur et TTS pour indiquer que la tâche est en cours d’exécution.

Vous devez fournir un lien vers votre application avec des paramètres de lancement pour démarrer l’application dans l’état approprié. Cela permet à l’utilisateur d’afficher ou de terminer la tâche proprement dit. **Cortana** fournit le texte du lien (par exemple, « accéder à Adventure Works »).

Les écrans de progression s’affichent pendant 5 secondes chacun, après quoi ils doivent être suivis par un autre écran, sinon la tâche expire.

Ces écrans peuvent suivre un écran de progression :

- Avancement
- Confirmation (explicite, décrite plus loin)
- Lever les ambiguïtés
- Completion

### <a name="gui-and-tts-guidelines-for-progress-screens"></a>Instructions relatives à l’interface graphique utilisateur et TTS pour les écrans de progression

Utilisez l’intensité actuelle.

Utilisez un verbe d’action qui confirme que la tâche est en cours d’exécution.

**GUI**: si l’entité est affichée, utilisez une référence à celle-ci (« annulation de ce voyage... »); Si aucune entité n’est affichée, appelez explicitement l’entité (« Canceling » Vegas Technical Conference «»).

**TTS**: vous ne devez inclure qu’une chaîne TTS sur le premier écran de progression. Si des écrans de progression supplémentaires sont requis, envoyez une chaîne vide, {} , comme chaîne TTS, et fournissez uniquement une chaîne gui.

| Conditions  | TTS | Interface graphique utilisateur |
| --- | --- | --- |
| LECTURE D’ENTITÉ À L’ACTIVATION/L’ENTITÉ PRÉCÉDENTE AFFICHÉE À L’AFFICHAGE     | Annulation de ce voyage...          | Annulation de ce voyage...          |
| ENTITÉ NON LUE SUR LE TOUR/L’ENTITÉ PRÉCÉDENTE AFFICHÉE À L’AFFICHAGE | Annulation de votre voyage à Vegas... | Annulation de ce voyage...          |
| L’ENTITÉ N’EST PAS LUE SUR LE TOUR/L’ENTITÉ PRÉCÉDENT NON AFFICHÉ        | Annulation de votre voyage à Vegas... | Annulation de votre voyage à Vegas... |

### <a name="confirmation"></a>Confirmation

:::image type="content" source="images/cortana/e2e-canceltrip-confirmation.png" alt-text="Capture d’écran de la zone de travail Cortana pour le workflow de l’application en arrière-plan Cortana de bout en bout à l’aide de la confirmation d’annulation de trajet":::*AdventureWorks « annuler le trajet »*

Certaines tâches peuvent être confirmées implicitement par la nature de la commande de l’utilisateur. d’autres sont potentiellement plus sensibles et nécessitent une confirmation explicite. Voici quelques recommandations relatives à l’utilisation de la confirmation explicite ou implicite.

Les chaînes de l’interface graphique utilisateur et du TTS sur l’écran de confirmation sont spécifiées par votre application, et l’icône de l’application, si elle est fournie, est affichée à la place de l’avatar **Cortana** .

Une fois que le client répond à la confirmation, votre application doit fournir l’écran suivant dans 500 ms pour éviter d’accéder à un écran de progression.

Utiliser Explicit quand...

- Le contenu quitte l’utilisateur (par exemple, un message texte, un message électronique ou une publication sociale)
- Une action ne peut pas être annulée (par exemple, en effectuant un achat ou en supprimant un événement)
- Le résultat peut être gênant (par exemple, en appelant la mauvaise personne)
- Une reconnaissance plus complexe est nécessaire (par exemple, transcription ouverte)

Utiliser implicite quand...

- Le contenu est enregistré uniquement pour l’utilisateur (par exemple, une note à proprement dit)
- Il existe un moyen simple d’annuler (par exemple, activer ou désactiver une alarme)
- La tâche doit être rapide (par exemple, en capturant rapidement une idée avant de l’oublier)
- La précision est élevée (par exemple, un menu simple)

### <a name="gui-and-tts-guidelines-for-confirmation-screens"></a>Instructions relatives à l’interface graphique utilisateur et TTS pour les écrans de confirmation

Utilisez l’intensité actuelle.

Demandez à l’utilisateur une question non ambiguë à laquelle il est possible de répondre avec « oui » ou « non ». La question doit confirmer explicitement ce que l’utilisateur tente de faire et il ne doit y avoir aucune autre option évidente.

Fournissez une variante de la question pour une nouvelle invite, si la commande vocale n’est pas comprise la première fois.

**GUI**: si l’entité est affichée, utilisez une référence à celle-ci. Si aucune entité n’est affichée, appelez explicitement l’entité.

**TTS**: pour plus de clarté, référencez toujours l’élément ou l’entité spécifique, sauf s’il a été lu par le système au tour du précédent.

| Conditions | TTS | Interface graphique utilisateur |
| --- | --- | --- |
| ENTITÉ NON LUE SUR LE TOUR/L’ENTITÉ PRÉCÉDENTE AFFICHÉE À L’AFFICHAGE | Voulez-vous annuler la Conférence technique de Vegas ? | Annuler ce voyage ?                             |
| L’ENTITÉ N’EST PAS LUE SUR LE TOUR/L’ENTITÉ PRÉCÉDENT NON AFFICHÉ        | Voulez-vous annuler la Conférence technique de Vegas ? | Annuler la Conférence technique de Vegas ?                 |
| LECTURE D’ENTITÉ SUR LE TOUR/L’ENTITÉ PRÉCÉDENTE NON AFFICHÉE            | Voulez-vous annuler ce voyage ?             | Annuler ce voyage ?                             |
| REDEMANDE AVEC L’ENTITÉ AFFICHÉE                              | Voulez-vous annuler ce voyage ?            | Voulez-vous annuler ce voyage ?             |
| REDEMANDE D’UNE ENTITÉ NON AFFICHÉE                          | Voulez-vous annuler ce voyage ?            | Voulez-vous annuler la Conférence technique de Vegas ? |

### <a name="disambiguation"></a>Lever les ambiguïtés

:::image type="content" source="images/cortana/cortana-disambiguation-screen.png" alt-text="Capture d’écran de la zone de travail Cortana pour le workflow d’application en arrière-plan Cortana de bout en bout à l’aide de la désambiguïsation d’annulation de parcours":::AdventureWorks *« Annuler le trajet »*

Certaines tâches peuvent nécessiter que l’utilisateur sélectionne dans une liste d’entités pour terminer la tâche.

Les chaînes GUI et TTS de l’écran de désambiguïsation sont spécifiées par votre application, et l’icône de l’application, si elle est fournie, s’affiche à la place de l’avatar **Cortana** .

Une fois que le client répond à la question de la désambiguïsation, votre application doit fournir l’écran suivant dans 500 ms pour éviter d’accéder à un écran de progression.

### <a name="gui-and-tts-guidelines-for-disambiguation-screens"></a>Instructions relatives à l’interface graphique utilisateur et TTS pour les écrans de désambiguïsation

Utilisez l’intensité actuelle.

Demandez à l’utilisateur une question non ambiguë à laquelle il est possible de répondre avec le titre ou la ligne de texte d’une entité affichée.

Jusqu’à 10 entités peuvent être affichées.

Chaque entité doit avoir un titre unique.

Fournissez une variante de la question pour une nouvelle invite, si la commande vocale n’est pas comprise la première fois.

**TTS**: pour plus de clarté, référencez toujours l’élément ou l’entité spécifique, à moins qu’il ait été parlé au tour précédent.

**TTS**: ne lisez pas la liste d’entités, à moins qu’il y en ait au moins trois et qu’ils soient courts.

| Conditions                 | TTS                                                                            | Interface graphique utilisateur                              |
|----------------------------|--------------------------------------------------------------------------------|----------------------------------|
| INVITE-3 ÉLÉMENTS OU MOINS  | Quel voyage Vegas voulez-vous annuler ? Un tiers ou une conférence technique Vegas à Vegas ? | Lequel voulez-vous annuler ? |
| INVITE-PLUS DE 3 ÉLÉMENTS | Quel voyage Vegas voulez-vous annuler ?                                          | Lequel voulez-vous annuler ? |
| RÉINVITER                   | Quel voyage Vegas voulez-vous annuler ?                                         | Lequel voulez-vous annuler ? |

### <a name="completion"></a>Completion

:::image type="content" source="images/cortana/e2e-canceltrip-completion.png" alt-text="Capture d’écran de la zone de travail Cortana pour le workflow de l’application en arrière-plan Cortana de bout en bout à l’aide d’AdventureWorks annuler la fin du parcours":::*AdventureWorks « annuler le trajet »*

Une fois l’exécution de la tâche terminée, votre application doit informer l’utilisateur que la tâche demandée a été effectuée avec succès.

Les chaînes GUI et TTS sur l’écran de saisie semi-automatique sont spécifiées par votre application, et l’icône de l’application, si elle est fournie, s’affiche à la place de l’avatar **Cortana** .

Vous devez fournir un lien vers votre application avec des paramètres de lancement pour démarrer l’application dans l’état approprié. Cela permet à l’utilisateur d’afficher ou de terminer la tâche proprement dit. **Cortana** fournit le texte du lien (par exemple, « accéder à Adventure Works »).

### <a name="gui-and-tts-guidelines-for-completion-screens"></a>Instructions relatives à l’interface graphique et TTS pour les écrans de saisie semi-automatique

Utilisez des dizaines de temps.

Utilisez un verbe d’action pour indiquer explicitement que la tâche est terminée.

Si l’entité est affichée, ou si elle a été référencée avant l’activation précédente, référencez-la uniquement.

| Conditions                                       | TTS                                             | Interface graphique utilisateur                                |
|--------------------------------------------------|-------------------------------------------------|------------------------------------|
| ENTITÉ AFFICHÉE/ENTITÉ LUE AU PRÉALABLE         | J’ai annulé ce voyage.                       | A annulé ce voyage.               |
| ENTITÉ NON AFFICHÉE/ENTITÉ NON LUE SUR LA FORME PRÉCÉDENTE | J’ai annulé votre voyage à Vegas Technical Conference. | « Conférence technique Vegas » annulée. |

### <a name="error"></a>Error

:::image type="content" source="images/cortana/e2e-canceltrip-error.png" alt-text="Capture d’écran de la zone de travail Cortana pour le workflow de l’application en arrière-plan Cortana de bout en bout avec l’erreur d’annulation de parcours":::AdventureWorks *« Annuler le trajet »*

Lorsque l’une des erreurs suivantes se produit, **Cortana** affiche le même message d’erreur générique.

- L’app service se termine de manière inattendue.
- **Cortana** ne parvient pas à communiquer avec l’app service.
- L’application ne parvient pas à fournir un écran lorsque **Cortana** affiche un écran de remise ou un écran de progression pendant 5 secondes.

## <a name="related-articles"></a>Articles connexes

- [Interactions Cortana dans les applications Windows](cortana-interactions.md)
- [Éléments et attributs d’un fichier VCD v1.2](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2)
- [Exemple de commande vocale Cortana](https://go.microsoft.com/fwlink/p/?LinkID=619899)
