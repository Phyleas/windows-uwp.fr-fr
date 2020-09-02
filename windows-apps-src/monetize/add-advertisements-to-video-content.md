---
ms.assetid: cc24ba75-a185-4488-b70c-fd4078bc4206
description: Découvrez comment utiliser la classe AdScheduler pour afficher des publicités dans du contenu vidéo dans une application plateforme Windows universelle (UWP) qui a été écrite à l’aide de JavaScript avec HTML.
title: Afficher des publicités dans du contenu vidéo
ms.date: 02/18/2020
ms.topic: article
keywords: Windows 10, UWP, ADS, publicité, vidéo, planificateur, JavaScript
ms.localizationpriority: medium
ms.openlocfilehash: 9d1a5c08d9965422d6fcd543ee38d3e628be8432
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89364062"
---
# <a name="show-ads-in-video-content"></a>Afficher des publicités dans du contenu vidéo

>[!WARNING]
> Depuis le 1er juin 2020, la plateforme de monétisation Microsoft AD pour les applications Windows UWP sera arrêtée. [En savoir plus](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/db8d44cb-1381-47f7-94d3-c6ded3fea36f/microsoft-ad-monetization-platform-shutting-down-june-1st?forum=aiamgr)

Cette procédure pas à pas montre comment utiliser la classe **AdScheduler** pour afficher les publicités du contenu vidéo dans une application plateforme Windows universelle (UWP) qui a été écrite à l’aide de JavaScript avec html.

> [!NOTE]
> Cette fonctionnalité n’est actuellement prise en charge que pour les applications UWP écrites à l’aide de JavaScript avec HTML.

**AdScheduler** fonctionne avec les médias à diffusion progressive ou continue et utilise les formats de charge utile standard de l’IAB : VAST (Video Ad Serving Template) 2.0/3.0 et VMAP. L’utilisation des normes rend **AdScheduler** indépendant du service de publicité avec lequel il interagit.

La publicité pour contenu vidéo varie selon que le programme dure moins de dix minutes (format court) ou plus de dix minutes (format long). Bien que ce dernier soit plus difficile à mettre en place au niveau du service, la façon d’écrire le code côté client ne présente en fait aucune différence. Si **AdScheduler** reçoit une charge utile VAST avec une seule publicité au lieu d’un manifeste, elle est traitée comme si le manifeste appelait une publicité précédant la vidéo (s’arrêtant à 00:00).

## <a name="prerequisites"></a>Prérequis

* Installez le [Kit de développement logiciel (SDK) Microsoft Advertising](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDK) avec Visual Studio 2015 ou une version ultérieure.

* Votre projet doit utiliser le contrôle [MediaPlayer](https://github.com/Microsoft/TVHelpers/wiki/MediaPlayer-Overview) pour afficher le contenu vidéo dans lequel les publicités doivent apparaître. Ce contrôle est disponible dans la collection [TVHelpers](https://github.com/Microsoft/TVHelpers) des bibliothèques mises à la disposition sur GitHub par Microsoft.

  L’exemple suivant montre comment déclarer un [MediaPlayer](https://github.com/Microsoft/TVHelpers/wiki/MediaPlayer-Overview) dans le balisage HTML. En règle générale, ce code appartient à la section `<body>` du fichier index.html (ou un autre fichier html, selon votre projet).

  ``` html
  <div id="MediaPlayerDiv" data-win-control="TVJS.MediaPlayer">
    <video src="URL to your content">
    </video>
  </div>
  ```

  L’exemple suivant montre comment établir un [MediaPlayer](https://github.com/Microsoft/TVHelpers/wiki/MediaPlayer-Overview) dans du code JavaScript.

  :::code language="javascript" source="~/../snippets-windows/windows-uwp/monetize/AdvertisingSamples/AdSchedulerSamples/js/js/main.js" id="Snippet1":::

## <a name="how-to-use-the-adscheduler-class-in-your-code"></a>Comment utiliser la classe AdScheduler dans votre code

1. Dans Visual Studio, ouvrez votre projet ou créez-en un.

2. Si votre projet cible **Toute UC**, mettez-le à jour pour utiliser une sortie de génération propre à l’architecture (par exemple, **x86**). Si votre projet cible **Toute UC**, vous ne pourrez pas ajouter une référence à la bibliothèque de publicités Microsoft dans les étapes suivantes. Pour plus d’informations, consultez [Erreurs de référence provoquées par le ciblage de Toute UC dans votre projet](known-issues-for-the-advertising-libraries.md#reference_errors).

3. Ajoutez une référence à la bibliothèque du **Kit de développement logiciel (SDK) Microsoft Advertising pour JavaScript** à votre projet.

    1. Dans la fenêtre **Explorateur de solutions** , cliquez avec le bouton droit sur **références**, puis sélectionnez **Ajouter une référence...**
    2. Dans **Gestionnaire de références**, développez **Windows universel**, cliquez sur **Extensions**, puis cochez la case en regard de **Kit de développement logiciel (SDK) Microsoft Advertising pour JavaScript** (version 10.0).
    3. Dans **Gestionnaire de références**, cliquez sur OK.

4.  Ajoutez le fichier AdScheduler.js à votre projet :

    1. Dans Visual Studio, cliquez sur **Projet** et sur **Gérer les packages NuGet**.
    2. Dans la zone de recherche, tapez **Microsoft.StoreServices.VideoAdScheduler**, puis installez le package Microsoft.StoreServices.VideoAdScheduler. Le fichier AdScheduler.js est ajouté au sous-répertoire .. /js de votre projet.

5.  Ouvrez le fichier index.html (ou un autre fichier html en fonction de votre projet). Dans la section `<head>`, après les références JavaScript des fichiers default.css et main.js du projet, ajoutez la référence à ad.js et adscheduler.js.

    ``` html
    <script src="//Microsoft.Advertising.JavaScript/ad.js"></script>
    <script src="/js/adscheduler.js"></script>
    ```

    > [!NOTE]
    > Cette ligne doit être placée dans la `<head>` section après l’inclusion de main.js ; dans le cas contraire, vous rencontrerez une erreur lors de la génération de votre projet.

6.  Dans le fichier main.js de votre projet, ajoutez le code qui crée un objet **AdScheduler**. Transmettez le **MediaPlayer** qui héberge votre contenu vidéo. Le code doit être placé de telle sorte qu’il s’exécute après [WinJS.UI.processAll](/previous-versions/windows/apps/hh440975).

    :::code language="javascript" source="~/../snippets-windows/windows-uwp/monetize/AdvertisingSamples/AdSchedulerSamples/js/js/main.js" id="Snippet2":::

7.  Utilisez les méthodes **requestSchedule** ou **requestScheduleByUrl** de l’objet **AdScheduler** pour demander une planification ad à partir du serveur, puis insérez-la dans la chronologie de **MediaPlayer** , puis lisez le support vidéo.

    * Si vous êtes partenaire Microsoft et que vous avez reçu l’autorisation de demander une planification publicitaire auprès du serveur d’annonces Microsoft, utilisez **requestSchedule** et spécifiez l’ID d’application et l’ID d’unité publicitaire qui vous ont été fournis par votre représentant Microsoft.

        Cette méthode prend la forme d’une [promesse](../threading-async/asynchronous-programming-universal-windows-platform-apps.md#asynchronous-patterns-in-uwp-using-javascript), qui est une construction asynchrone où deux pointeurs de fonction sont passés : un pointeur pour la fonction **OnComplete** à appeler lorsque la promesse se termine avec succès et un pointeur pour la fonction **OnError** à appeler si une erreur est rencontrée. Dans la fonction **OnComplete** , démarrez la lecture de votre contenu vidéo. La publicité commence à l’heure planifiée. Dans votre fonction **OnError** , gérez l’erreur, puis démarrez la lecture de votre vidéo. Votre contenu vidéo s’exécutera sans publicité. L’argument de la fonction **OnError** est un objet qui contient les membres suivants.

        :::code language="javascript" source="~/../snippets-windows/windows-uwp/monetize/AdvertisingSamples/AdSchedulerSamples/js/js/main.js" id="Snippet3":::

    * Pour demander une planification ad à partir d’un serveur AD non-Microsoft, utilisez **requestScheduleByUrl**et transmettez l’URI du serveur. Cette méthode prend également la forme d’une **promesse** qui accepte des pointeurs pour les fonctions **OnComplete** et **OnError** . La charge utile Active Directory qui est retournée par le serveur doit être conforme au format de charge utile de la vidéo Active Directory (vaste) ou de la sélection de vidéo (VMAP).

        :::code language="javascript" source="~/../snippets-windows/windows-uwp/monetize/AdvertisingSamples/AdSchedulerSamples/js/js/main.js" id="Snippet4":::

    > [!NOTE]
    > Vous devez attendre que **requestSchedule** ou **requestScheduleByUrl** retourne avant de commencer à lire le contenu vidéo principal dans le **MediaPlayer**. À partir du début de la lecture du média avant que **requestSchedule** ne retourne (dans le cas d’une publication préliminaire), le pré-roll interrompt le contenu vidéo principal. Vous devez appeler **Play** même si la fonction échoue, car le **AdScheduler** indique au **MediaPlayer** d’ignorer la ou les annonces et de se déplacer directement vers le contenu. Vous pouvez avoir une exigence différente, telle que l’insertion d’une publicité intégrée dans le cas où une annonce ne peut pas être récupérée à distance.

8.  Pendant la lecture, vous pouvez gérer d’autres événements qui permettent à votre application de suivre la progression et/ou les erreurs susceptibles de se produire après le processus de sélection d’annonce initial. Le code suivant contient certains de ces événements, dont **onPodStart**, **onPodEnd**, **onPodCountdown**, **onAdProgress**, **onAllComplete** et **onErrorOccurred**.

    :::code language="javascript" source="~/../snippets-windows/windows-uwp/monetize/AdvertisingSamples/AdSchedulerSamples/js/js/main.js" id="Snippet5":::

## <a name="adscheduler-members"></a>Membres AdScheduler

Cette section fournit des détails sur les membres de l’objet **AdScheduler** . Pour plus d’informations sur ces membres, consultez les commentaires et les définitions dans le fichier AdScheduler.js de votre projet.

### <a name="requestschedule"></a>requestSchedule

Cette méthode demande une planification ad à partir du serveur Microsoft ad et l’insère dans la chronologie du **MediaPlayer** qui a été passé au constructeur **AdScheduler** .

Le troisième paramètre facultatif (*adTags*) est une collection JSON de paires nom/valeur qui peut être utilisée pour les applications qui ont un ciblage avancé. Par exemple, une application qui joue un grand nombre de vidéos liées automatiquement peut compléter l’ID d’unité ad avec la marque et le modèle de la voiture affichée. Ce paramètre est destiné à être utilisé uniquement par les partenaires qui reçoivent l’approbation de Microsoft pour utiliser des balises ad.

Les éléments suivants doivent être notés quand vous faites référence à *adTags*:

* Ce paramètre est une option très rarement utilisée. Le serveur de publication doit travailler en étroite collaboration avec Microsoft avant d’utiliser adTags.
* Les noms et les valeurs doivent être prédéterminés sur le service Active Directory. Les balises ad ne sont pas ouvertes termes de recherche ou Mots clés.
* Le nombre maximal de balises prises en charge est de 10.
* Les noms de balises sont limités à 16 caractères.
* Les valeurs des balises ne doivent pas dépasser 128 caractères.

### <a name="requestschedulebyuri"></a>requestScheduleByUri

Cette méthode demande une planification ad à partir du serveur AD non-Microsoft spécifié dans l’URI et l’insère dans la chronologie du **MediaPlayer** qui a été passé au constructeur **AdScheduler** . La charge utile Active Directory qui est retournée par le serveur Active Directory doit être conforme au format de charge utile de la vidéo Active Directory (vaste) ou de la sélection de vidéo (VMAP).

### <a name="mediatimeout"></a>mediaTimeout

Cette propriété obtient ou définit le nombre de millisecondes pendant lesquelles le média doit être lisible. La valeur 0 indique au système de ne jamais avoir expiré. La valeur par défaut est 30000 ms (30 secondes).

### <a name="playskippedmedia"></a>playSkippedMedia

Cette propriété obtient ou définit une valeur **booléenne** qui indique si le média planifié est lu si l’utilisateur avance jusqu’à un point situé après une heure de début planifiée.

Le client Active Directory et le lecteur multimédia appliquent les règles en termes de publication pendant le transfert rapide et le rembobinage du contenu vidéo principal. Dans la plupart des cas, les développeurs d’applications n’autorisent pas l’ignorance complète des publications, mais elles souhaitent fournir une expérience raisonnable à l’utilisateur. Les deux options suivantes répondent aux besoins de la plupart des développeurs :

1. Autorisez les utilisateurs finaux à ignorer les modules de publication à l’adresse.
2. Autorisez les utilisateurs à ignorer les gousses de publication, mais à lire le bloc le plus récent lors de la reprise de la lecture.

La propriété **playSkippedMedia** a les conditions suivantes :

* Les publications ne peuvent pas être ignorées ou transmises rapidement après le début de la publication.
* Toutes les publications dans un pod de publication sont lues une fois que le POD a démarré.
* Une fois jouée, une publication n’est pas lue pendant le contenu principal (film, épisode, etc.); les marqueurs de publication sont marqués comme lus ou supprimés.
* Les publications de pré-déploiement ne peuvent pas être ignorées.

Quand vous reprenez du contenu contenant des publicités, définissez **playSkippedMedia** sur **false** pour ignorer les présélections et empêcher la dernière exécution de la dernière. Ensuite, une fois le contenu démarré, affectez à **playSkippedMedia** la **valeur true** pour vous assurer que les utilisateurs ne peuvent pas avancer rapidement par le biais de publicités ultérieures.

> [!NOTE]
> Un Pod est un groupe de publicités qui s’exécutent dans une séquence, par exemple un groupe de publicités qui s’exécutent pendant une pause commerciale. Pour plus d’informations, consultez la spécification du modèle de service d’annonces numériques de l’IAB.

### <a name="requesttimeout"></a>requestTimeout

Cette propriété obtient ou définit le nombre de millisecondes d’attente d’une réponse de demande Active Directory avant le dépassement du délai d’attente. La valeur 0 indique au système de ne jamais avoir expiré. La valeur par défaut est 30000 ms (30 secondes).

### <a name="schedule"></a>schedule

Cette propriété obtient les données de planification qui ont été récupérées à partir du serveur AD. Cet objet comprend la hiérarchie complète des données qui correspond à la structure du modèle de service vidéo (vaste) ou à la charge utile de la playlist de vidéo multiple ad (VMAP).

### <a name="onadprogress"></a>onAdProgress  

Cet événement est déclenché lorsque la lecture Active Directory atteint les points de contrôle du quartile. Le deuxième paramètre du gestionnaire d’événements (*EventInfo*) est un objet JSON avec les membres suivants :

* **Progress**: état de la lecture ad (l’une des valeurs d’énumération **MediaProgress** définie dans AdScheduler.js).
* **clip**: le clip vidéo en cours de lecture. Cet objet n’est pas destiné à être utilisé dans votre code.
* **adPackage**: objet qui représente la partie de la charge active ad qui correspond à la publicité en cours de diffusion. Cet objet n’est pas destiné à être utilisé dans votre code.

### <a name="onallcomplete"></a>onAllComplete  

Cet événement est déclenché lorsque le contenu principal atteint la fin et que toute publicité postérieure à la planification est également terminée.

### <a name="onerroroccurred"></a>onErrorOccurred  

Cet événement est déclenché lorsque le **AdScheduler** rencontre une erreur. Pour plus d’informations sur les valeurs des codes d’erreur, consultez [ErrorCode](/uwp/api/microsoft.advertising.errorcode).

### <a name="onpodcountdown"></a>onPodCountdown

Cet événement est déclenché lorsqu’une publicité est lue et indique le temps restant dans le pod actuel. Le deuxième paramètre du gestionnaire d’événements (*EventData*) est un objet JSON avec les membres suivants :

* **remainingAdTime**: nombre de secondes restantes pour la publicité active.
* **remainingPodTime**: nombre de secondes restantes pour le pod actuel.

> [!NOTE]
> Un Pod est un groupe de publicités qui s’exécutent dans une séquence, par exemple un groupe de publicités qui s’exécutent pendant une pause commerciale. Pour plus d’informations, consultez la spécification du modèle de service d’annonces numériques de l’IAB.

### <a name="onpodend"></a>onPodEnd  

Cet événement est déclenché lorsqu’un bloc publicitaire se termine. Le deuxième paramètre du gestionnaire d’événements (*EventData*) est un objet JSON avec les membres suivants :

* **StartTime**: l’heure de début du Pod, en secondes.
* **Pod**: objet qui représente le POD. Cet objet n’est pas destiné à être utilisé dans votre code.

### <a name="onpodstart"></a>onPodStart

Cet événement est déclenché lors du démarrage d’un pod. Le deuxième paramètre du gestionnaire d’événements (*EventData*) est un objet JSON avec les membres suivants :

* **StartTime**: l’heure de début du Pod, en secondes.
* **Pod**: objet qui représente le POD. Cet objet n’est pas destiné à être utilisé dans votre code.
