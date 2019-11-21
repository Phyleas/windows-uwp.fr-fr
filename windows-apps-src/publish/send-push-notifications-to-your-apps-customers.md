---
Description: Découvrez comment envoyer des notifications de l’espace partenaires à votre application pour inciter des groupes de clients à prendre une mesure, comme évaluer une application ou acheter un module complémentaire.
title: Envoyer des notifications push ciblées aux clients de votre application
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp, notifications ciblées, notifications push, toast, vignette
ms.assetid: 16386c81-702d-47cd-9f91-67659f5dca73
ms.localizationpriority: medium
ms.openlocfilehash: 8ac9d464588cada11cd9d41cf0eb7858f593a0f2
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74259937"
---
# <a name="send-notifications-to-your-apps-customers"></a>Envoyer des notifications aux clients de votre application

Impliquer vos clients au bon moment et en utilisant le message approprié est la clé de votre réussite en tant que développeur d’applications. Les notifications permettent d’inciter vos clients à exécuter une action, par exemple évaluer une application, acheter une extension, essayer une nouvelle fonctionnalité ou télécharger une autre application (éventuellement gratuitement avec un [code promotionnel](generate-promotional-codes.md) que vous fournissez).

L' [espace partenaires](https://partner.microsoft.com/dashboard) fournit une plateforme d’engagement client pilotée par les données que vous pouvez utiliser pour envoyer des notifications à tous les clients de votre application, ou uniquement ciblée vers un sous-ensemble des clients Windows 10 de votre application qui répondent aux critères que vous avez définis dans un [segment de clientèle](create-customer-segments.md). Vous pouvez également créer une notification pour qu’elle soit envoyée aux clients de plusieurs de vos applications.

> [!IMPORTANT]
> Ces notifications sont uniquement utilisables avec les applications UWP.

Lorsque vous examinez le contenu de vos notifications, tenez compte des points suivants :
- Le contenu de vos notifications doit être conforme avec les [Politiques relatives au contenu](https://docs.microsoft.com/legal/windows/agreements/store-policies#content_policies) du Store.
- Vos notifications ne doivent aucunement comporter d’informations confidentielles ou potentiellement sensibles.
- Si nous faisons de notre mieux pour transmettre les notifications dans les délais prévus, des problèmes de latence peuvent parfois affecter ces remises.
- Veillez à ne pas envoyer trop souvent de notifications. Un intervalle entre les remises inférieur à 30 minutes peut sembler intrusif (dans de nombreux scénarios, il est préférable d’observer une fréquence moindre).
- N’oubliez pas que si un client qui utilise votre application (en étant connecté avec son compte Microsoft au moment de la détermination de l’appartenance du segment) prête ultérieurement son appareil à un autre utilisateur, ce dernier a accès à la notification transmise au client d’origine. Pour plus d’informations, voir [Configurer votre application pour recevoir des notifications Push du Centre de développement](../monetize/configure-your-app-to-receive-dev-center-notifications.md#notification-customers).
- Si vous envoyez la même notification aux clients de plusieurs applications, vous ne pouvez pas cibler de segment particulier ; la notification est envoyée à tous les clients de l'application sélectionnée.


## <a name="getting-started-with-notifications"></a>Prise en main des notifications

De façon générale, vous devez effectuer trois opérations afin d’utiliser des notifications pour impliquer vos clients.

1. **Inscrivez votre application pour recevoir des notifications push.** Pour ce faire, ajoutez une référence au kit de développement logiciel (SDK) Microsoft Store services dans votre application, puis ajoutez quelques lignes de code qui inscrivent un canal de notification entre l’espace partenaires et votre application. Nous utiliserons ce canal pour transmettre vos notifications à vos clients. Pour plus de détails, consultez l’article [Configurer votre application pour les notifications Push ciblées](../monetize/configure-your-app-to-receive-dev-center-notifications.md).
2. **Déterminez les clients à cibler.** Vous pouvez envoyer votre notification à l’ensemble des clients de votre application, ou (pour les notifications créées pour une application unique) à un groupe de clients appelé *segment*, que vous pouvez définir en fonction de critères démographiques ou de revenus. Pour plus d’informations, voir [Créer des segments de clients](create-customer-segments.md).
3. **Créez le contenu de votre notification et envoyez-la.** Par exemple, vous pouvez créer une notification qui encourage les nouveaux clients à évaluer votre application, ou envoyer une notification pour promouvoir une offre spéciale d’achat d’une extension.


## <a name="to-create-and-send-a-notification"></a>Pour créer et envoyer une notification

Suivez ces étapes pour créer une notification dans l’espace partenaires et l’envoyer à un segment de client particulier.

> [!NOTE]
> Pour qu’une application puisse recevoir des notifications de l’espace partenaires, vous devez d’abord appeler la méthode [RegisterNotificationChannelAsync](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager.registernotificationchannelasync) dans votre application pour inscrire votre application afin de recevoir des notifications. Cette méthode est disponible dans le [Microsoft Store Services SDK](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftStoreServicesSDK). Pour plus d’informations sur l’appel de cette méthode, notamment pour consulter un exemple de code, consultez l’article [Configurer votre application pour les notifications Push ciblées](../monetize/configure-your-app-to-receive-dev-center-notifications.md).

1. Dans l' [espace partenaires](https://partner.microsoft.com/dashboard), développez la section **engagement** , puis sélectionnez **notifications**.
2. Sur la page **Notifications**, sélectionnez **Nouvelle notification**.
3. Dans la section **Sélectionner un modèle** , choisissez le [type de notification](#notification-template-types) que vous souhaitez envoyer, puis cliquez sur **OK**.
4. À la page suivante, utilisez le menu déroulant pour choisir une **Application unique** ou **Plusieurs applications** pour lesquels vous souhaitez générer une notification. Vous ne pouvez sélectionner que les applications qui ont été [configurées pour recevoir des notifications à l’aide du kit de développement logiciel (SDK) Microsoft Store services](../monetize/configure-your-app-to-receive-dev-center-notifications.md).
5. Dans la section **Paramètres de notification**, choisissez un **nom** pour votre notification et, si applicable, choisissez le **groupe de clients** à qui vous souhaitez envoyer la notification. (Les notifications envoyées à plusieurs applications peuvent uniquement être envoyées à tous les clients de ces applications.) Si vous souhaitez utiliser un segment que vous n'avez pas encore créé, sélectionnez **Créer un nouveau groupe client**. Remarque : Il faut 24 heures avant qu’un nouveau segment puisse être utilisé pour les notifications. Pour plus d’informations, voir [Créer des segments de clients](create-customer-segments.md).
6. Si vous voulez indiquer à quel moment envoyer la notification, décochez la case **Notification immédiate** et choisissez une date et une heure spécifiques (au format UTC pour tous les clients, sauf si vous indiquez d’utiliser le fuseau horaire local de chaque client).
7. Si vous voulez que la notification expire à un moment donné, décochez la case **La notification n’expire jamais** et choisissez une date et une heure d’expiration spécifiques (au format UTC).
8. **Pour les notifications d'une application unique :** Si vous souhaitez filtrer les destinataires afin que votre notification soit envoyée uniquement aux personnes qui utilisent certaines langues ou qui se trouvent dans des fuseaux horaires spécifiques, cochez la case **Utiliser des filtres**. Vous pouvez ensuite spécifier les options de langues et/ou de fuseaux horaires que vous souhaitez utiliser.
8. **Pour les notifications de plusieurs applications :** Indiquez si vous souhaitez envoyer la notification uniquement à la dernière application active sur chaque appareil (par client) ou à toutes les applications sur chaque appareil.
10. Dans la section **Contenu de la notification**, dans le menu **Langue**, choisissez les langues dans lesquelles vous souhaitez que votre notification s’affiche. Pour plus d’informations, voir [Traduire vos notifications](#translate-your-notifications).
11. Dans la section **Options**, entrez du texte et configurez toutes les autres options que vous souhaitez. Si vous avez commencé avec un modèle, certaines de ces options sont fournies par défaut, mais vous pouvez apporter des modifications si vous le souhaitez.

    Les options disponibles varient selon le type de notification que vous utilisez. Voici certaines des options :

    * **Type d’activation** (notification de type toast interactif). Vous pouvez choisir **Premier plan**, **Arrière-plan**, ou **Protocole**.
    * **Lancer** (notification de type toast interactif). Vous pouvez décider que la notification ouvre une application ou un site web.
    * **Suivre la fréquence de lancement d’application** (notification de type toast interactif). Si vous souhaitez mesurer l’implication de vos clients par le biais de chaque notification, activez cette case à cocher. Pour plus d’informations, voir [Mesurer les performances des notifications](#measure-notification-performance).
    * **Durée** (notification de type toast interactif). Vous pouvez choisir **Court** ou **Long**.
    * **Scénario** (notification de type toast interactif). Vous pouvez choisir **Par défaut**, **Alarme**, **Rappel** ou **Appel entrant**.
    * **URI de base** (notification de type toast interactif). Pour plus d’informations, voir [BaseUri](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.baseuri#Windows_UI_Xaml_FrameworkElement_BaseUri).
    * **Ajouter une demande d’image** (notification de type toast interactif). Pour plus d’informations, voir [addImageQuery](https://docs.microsoft.com/uwp/schemas/tiles/toastschema/element-visual#attributes-and-elements).
    * **Éléments visuels**. Une image, une vidéo ou un son. Pour plus d’informations, voir [visual](https://docs.microsoft.com/uwp/schemas/tiles/toastschema/element-visual).
    * **Entrée**/**Action**/**Sélection** (notification de type toast interactif). Vous permet de laisser les utilisateurs interagir avec la notification. Pour plus d’informations, voir [Notifications toast adaptatives et interactives](../design/shell/tiles-and-notifications/adaptive-interactive-toasts.md).
    * **Liaison** (notification de type vignette interactive). Le modèle de toast. Pour plus d’informations, voir [binding](https://docs.microsoft.com/uwp/schemas/tiles/toastschema/element-binding).

    > [!TIP]
    > Essayez d’utiliser l’application [Notifications Visualizer](https://www.microsoft.com/store/apps/9nblggh5xsl1) pour concevoir et tester vos notifications de type vignettes adaptatives et toasts interactifs.

12. Sélectionnez **Enregistrer comme brouillon** pour continuer à travailler sur la notification plus tard, ou sélectionnez **Envoyer** si vous avez terminé.


## <a name="notification-template-types"></a>Types de modèles de notification

Vous pouvez choisir parmi différents modèles de notification.

-   **Vide (Toast).** Commencez avec une notification toast vide que vous pouvez personnaliser. Une notification toast est un élément d’interface utilisateur contextuel qui apparaît sur votre écran pour permettre à l’application de communiquer avec le client quand il se trouve dans une autre application, sur l’écran d’accueil ou sur le Bureau.
-   **Vide (vignette).** Commencez avec une notification vignette vide que vous pouvez personnaliser. Les vignettes correspondent à la représentation d’une application sur l’écran d’accueil. Les vignettes peuvent être « dynamiques », ce qui signifie que le contenu qu’elles affichent peut changer suite à des notifications.
-   **Demander des évaluations (Toast).** Une notification toast qui demande à vos clients d’évaluer votre application. Lorsque le client sélectionne la notification, la page d’évaluation de votre application dans la boutique s’affiche.
-   **Demander des commentaires (Toast).** Une notification toast qui demande à vos clients de donner leur avis sur votre application. Lorsque le client sélectionne la notification, la page Hub de commentaires de votre application s’affiche.
    > [!NOTE]
    > Si vous choisissez ce type de modèle, dans la zone **Lancer**, n’oubliez pas de remplacer l’espace réservé {PACKAGE_FAMILY_NAME} par le nom de la famille de packages (PFN) réel de votre application. Vous trouverez le PFN de votre application sur la page [Identité des applications](view-app-identity-details.md) (**Gestion des applications** > **Identité des applications**).

    ![Zone de lancement de la notification toast de commentaire](images/push-notifications-feedback-toast-launch-box.png)

-   **Promouvoir croisée (Toast).** Une notification toast visant à promouvoir une autre application de votre choix. Lorsque le client sélectionne la notification, la description de l’application dans la boutique s’affiche.
    > [!NOTE]
    > Si vous choisissez ce type de modèle, dans la zone **Lancer**, n’oubliez pas de remplacer l’espace réservé **{ID du produit à promouvoir ici}** par l’ID Store réel de l’élément que vous voulez promouvoir. Vous trouverez l’ID sur la page [Identité des applications](view-app-identity-details.md) (**Gestion des applications** > **Identité des applications**).

    ![Zone de lancement de la notification toast de promotion](images/push-notifications-promote-toast-launch-box.png)

-   **Promouvoir une vente (Toast).** Une notification toast que vous pouvez utiliser pour présenter une offre concernant votre application. Lorsque le client sélectionne la notification, la description de votre application dans la boutique s’affiche.
-   **Demander la mise à jour (Toast).** Une notification toast qui encourage les clients utilisant une ancienne version de votre application à installer la dernière version. Lorsque le client sélectionne la notification, l’application Store est lancée et affiche la liste **Téléchargements et mises à jour**. Notez que ce modèle peut uniquement être utilisé avec une seule application et que vous ne pouvez pas cibler un segment de clients particulier ni définir une heure d’envoi. Nous planifierons toujours l’envoi de cette notification dans les 24 heures et mettrons tout en œuvre pour cibler tous les utilisateurs qui n’utilisent pas encore la dernière version de votre application.


## <a name="measure-notification-performance"></a>Mesurer les performances des notifications

Vous pouvez mesurer l’implication de vos clients par le biais de chaque notification.


### <a name="to-measure-notification-performance"></a>Pour mesurer les performances des notifications

1.  Lorsque vous créez une notification, dans la section **Contenu de la notification**, cochez la case **Suivre la fréquence de lancement d’application**.
2.  Dans votre application, appelez la méthode [ParseArgumentsAndTrackAppLaunch](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager.parseargumentsandtrackapplaunch) pour notifier le Centre des partenaires que votre application a été lancée en réponse à une notification ciblée. Cette méthode est fournie par le Microsoft Store Services SDK. Pour plus d’informations sur l’appel de cette méthode, consultez [configurer votre application pour recevoir des notifications de l’espace partenaires](../monetize/configure-your-app-to-receive-dev-center-notifications.md).


### <a name="to-view-notification-performance"></a>Pour afficher les performances des notifications

Une fois que vous avez configuré la notification et votre application pour mesurer les performances de notification comme décrit ci-dessus, vous pouvez voir l’efficacité de vos notifications.

Pour examiner les données détaillées pour chaque notification :

1.  Dans l’espace partenaires, développez la section **engagement** et sélectionnez **notifications**.
2.  Dans le tableau des notifications existantes, sélectionnez **en cours** ou **terminé**, puis examinez les colonnes **taux de remise** et taux de lancement de l' **application** pour afficher les performances de haut niveau de chaque notification.
3.  Pour afficher des informations plus granulaires sur les performances, sélectionnez le nom d’une notification. Dans la section **Statistiques de livraison**, vous pouvez visualiser le **nombre** et le **pourcentage** pour les types d’**états** de notification suivants :
    * **Échec** : la notification n’a pas été transmise pour une raison quelconque. Cela peut se produire, par exemple, si un problème survient dans le service de notification Windows.
    * **Échec**de l’expiration du canal : la notification n’a pas pu être remise car le canal entre l’application et l’espace partenaires a expiré. Cela peut se produire, par exemple, si le client n’a pas ouvert votre application depuis longtemps.
    * **Envoi** : la notification est dans la file d’attente pour être envoyée.
    * **Envoyée** : la notification a été envoyée.
    * **Lancée** : la notification a été envoyée, le client a cliqué dessus et votre application s’est ouverte. Notez que cette option suit uniquement les lancements de l’application. Les notifications qui invitent le client à effectuer d’autres actions (ouvrir la boutique pour laisser une évaluation, par exemple) ne sont pas comptabilisées dans cet état.
    * **Inconnu** : nous n’avons pas pu déterminer l’état de cette notification.

Pour analyser les données d’activité des utilisateurs pour toutes vos notifications :

1.  Dans l’espace partenaires, développez la section **engagement** et sélectionnez **notifications**.
2.  Sur la page **notifications** , cliquez sur l’onglet **analyser** . Cet onglet affiche les données suivantes :
    * Affichage graphique des différents États d’action utilisateur pour vos toasts et notifications du centre de notifications.
    * Vues cartographiques du monde des taux de clic pour vos toasts et notifications du centre de maintenance.
3. Dans la zone supérieure de la page, vous pouvez sélectionner la période sur laquelle portent les données qui vous intéressent. La valeur par défaut est de 30D (30 jours), mais vous pouvez choisir d’afficher les données portant sur des périodes de 3, 6 ou 12 mois, ou sur une plage de dates personnalisée que vous spécifiez. Vous pouvez également développer des **filtres** pour filtrer toutes les données par application et marché.

## <a name="translate-your-notifications"></a>Traduire vos notifications

Afin de maximiser l’impact de vos notifications, pensez à les traduire dans les langues préférées de vos clients. L’espace partenaires vous facilite la traduction automatique de vos notifications en tirant parti de la puissance du service [Microsoft Translator](https://www.microsoft.com/translator/home.aspx) .

1.  Une fois votre notification rédigée dans votre langue par défaut, sélectionnez **Ajouter des langues** (sous le menu **Langues** dans la section **Contenu de la notification**).
2.  Dans la fenêtre **Ajouter des langues**, sélectionnez les langues supplémentaires dans lesquelles vous voulez que vos notifications apparaissent, puis sélectionnez **Mettre à jour**.
Votre notification sera automatiquement traduite dans les langues que vous avez choisies dans la fenêtre **Ajouter des langues** et ces langues seront ajoutées au menu **Langue**.
3.  Pour afficher la traduction de votre notification, dans le menu **Langue**, sélectionnez la langue que vous venez d’ajouter.

Éléments à garder à l’esprit concernant la traduction :
 - Vous pouvez remplacer la traduction automatique en saisissant un autre texte dans la zone **Contenu** pour cette langue.
 - Si vous ajoutez une autre zone de texte à la version anglaise de la notification après avoir remplacé une traduction automatique, la nouvelle zone de texte ne sera pas ajoutée à la notification traduite. Dans ce cas, vous devez ajouter manuellement la nouvelle zone de texte à chacune des notifications traduites.
 - Si vous modifiez le texte anglais une fois que la notification a été traduite, nous mettrons automatiquement à jour les notifications traduites pour prendre en compte la modification. Cependant, cette action ne s’effectuera pas si vous avez précédemment remplacé la traduction initiale.

## <a name="related-topics"></a>Rubriques connexes
- [Vignettes pour les applications UWP](../design/shell/tiles-and-notifications/creating-tiles.md)
- [Vue d’ensemble des services de notifications Push Windows (WNS)](../design/shell/tiles-and-notifications/windows-push-notification-services--wns--overview.md)
- [Application du visualiseur de notifications](https://www.microsoft.com/store/apps/9nblggh5xsl1)
- [StoreServicesEngagementManager. RegisterNotificationChannelAsync () | registerNotificationChannelAsync (), méthode](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager.registernotificationchannelasync)
