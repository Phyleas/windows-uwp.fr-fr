---
Description: Vous pouvez créer des campagnes Active Directory dans l’espace partenaires pour améliorer la promotion de votre application et augmenter la base d’utilisateurs de votre application.
title: Création d’une campagne de publicité pour votre application
ms.assetid: 10D94929-92C4-4379-AA5F-6FEF879F2463
ms.date: 02/18/2020
ms.topic: article
keywords: Windows 10, UWP, ad, campagne, promouvoir
ms.localizationpriority: medium
ms.openlocfilehash: f0294ff668ea31abb32e7d646be85a89770bfd02
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89172863"
---
# <a name="create-an-ad-campaign-for-your-app"></a>Création d’une campagne de publicité pour votre application

>[!WARNING]
> Depuis le 1er juin 2020, la plateforme de monétisation Microsoft AD pour les applications Windows UWP sera arrêtée. [En savoir plus](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/db8d44cb-1381-47f7-94d3-c6ded3fea36f/microsoft-ad-monetization-platform-shutting-down-june-1st?forum=aiamgr)

Vous pouvez créer des campagnes ad dans l' [espace partenaires](https://partner.microsoft.com/dashboard) pour améliorer la promotion de votre application et augmenter sa base d’utilisateurs. Par défaut, nous choisissons le public ciblé pour vos annonces en fonction des paramètres de votre application dans l’espace partenaires, mais vous pouvez éventuellement définir votre propre audience. Vous pouvez également utiliser un ensemble de modèles de publicité par défaut ou charger vers le serveur vos propres conceptions d’annonces. Pour plus d’informations sur les campagnes de publicité, voir [Questions courantes sur les campagnes de publicité](common-questions.md).

Vous pouvez créer des campagnes ad uniquement pour les applications qui ont passé la phase de publication finale du [processus de certification](the-app-certification-process.md)de l’application.

> [!NOTE]
> Cette section de la documentation explique comment créer une campagne publicitaire dans l’espace partenaires. D’autres options de campagne pour créer et gérer des campagnes de publicité par programmation incluent [Vungle](https://vungle.com/) et l' [API Microsoft Store promotions](../monetize/run-ad-campaigns-using-windows-store-services.md).

## <a name="instructions"></a>Instructions

Voici comment créer une campagne publicitaire pour promouvoir une application.

1.  Dans le menu de navigation de gauche de l' [espace partenaires](https://partner.microsoft.com/dashboard), développez **attirer** , puis sélectionnez **campagnes Active Directory**.
2.  Sélectionnez **créer une campagne** (ou, si vous avez déjà créé des campagnes, sélectionnez **nouvelle campagne**).
3.  Sur la page suivante, dans la section **type d’objectif** , choisissez l’une des options suivantes :
    * **Augmenter les installations de votre application**. Sélectionnez cette option si votre campagne de publicité est conçue pour convaincre les clients d’installer votre application.
    * **Augmenter l’intérêt pour votre application**. Sélectionnez cette option si votre campagne publicitaire est destinée à faire en sorte que vos clients augmentent l’utilisation de votre application. Lorsque vous sélectionnez cette option, vous pouvez cibler votre campagne de publicité sur les [segments de clientèle](create-customer-segments.md) spécifiques que vous définissez.

4.  Sélectionnez l’application que vous souhaitez promouvoir avec cette campagne. Notez que l’application doit déjà être disponible dans le magasin.
5.  Vérifiez le nom fourni pour votre campagne dans le champ nom de la **campagne** et apportez des modifications, si vous le souhaitez.
6.  Sous **Type de campagne**, choisissez l’une des options suivantes :
    * **Publicité payante**: ces publicités s’exécutent dans n’importe quelle application qui correspond à l’appareil et à la catégorie de votre application. Pour les nouvelles campagnes créées après le 9 janvier 2017, ces publicités s’affichent également dans MSN.com, Outlook.com, Skype et d’autres propriétés Microsoft Premium. Les campagnes de promotion d’applications qui ciblent les applications et les propriétés Microsoft Premium sont appelées campagnes *universelles* .
    * **Communauté ad (gratuit)**: ces publicités s’exécuteront dans des applications publiées par d’autres développeurs qui créent également des campagnes AD de la communauté. Avant de pouvoir sélectionner cette option, vous devez avoir choisi d’illustrer les publicités de la Communauté dans la page **de la**  ->  **publicité dans les annonces dans l’application** . Pour en savoir plus, voir [À propos des annonces de la communauté](about-community-ads.md).
    * **Maison publicitaire (gratuit)**: ces publicités s’exécutent uniquement dans vos applications qui correspondent au type d’appareil de l’application publiée. Les publicités maison sont gratuites. Pour plus d’informations, consultez [À propos des publicités maison](about-house-ads.md).

7.  Pour les campagnes ad payantes, confirmez la durée de la **campagne** (la période pendant laquelle votre budget de campagne sera dépensé). L’option par défaut est **mensuelle**, ce qui signifie que le budget de votre campagne sera dépensé chaque mois sur une base récurrente, jusqu’à ce que vous arrêtiez la campagne. Si vous avez un compte Premium, vous pouvez éventuellement choisir **personnalisé** pour spécifier une plage de dates et d’heures personnalisée pendant laquelle le budget de votre campagne sera dépensé. Pour plus d’informations sur les comptes premium, consultez [Questions courantes sur les campagnes de publicité](common-questions.md#how-can-i-increase-the-maximum-monthly-budget-amount-allowed-for-my-ad-campaign).

8.  Confirmez votre budget et vos informations de paiement. (Si vous créez une campagne de maison ou de communauté, ces options ne s’affichent pas, car ces campagnes sont gratuites.)
    * Sous **budget**, utilisez le curseur pour définir la somme d’argent que vous souhaitez dépenser chaque mois pour exécuter la publicité (ou le budget total, si vous avez sélectionné une durée de campagne personnalisée).

        Le budget mensuel est calculé au prorata du mois au cours duquel vous avez créé la campagne de publicité. En d’autres termes, si vous créez une campagne au milieu du mois, vous êtes facturé la moitié du budget mensuel fixé pour le mois concerné.

    * Spécifiez un mode de paiement pour votre campagne publicitaire en cliquant sur **Ajouter un nouveau mode de paiement** et en complétant les informations de votre compte. Si vous avez déjà fourni un instrument de paiement, vous pouvez sélectionner **choisir un autre mode de paiement** si vous devez le mettre à jour. Le pays/la région de l’adresse de facturation de votre moyen de paiement doit correspondre au pays ou à la région associés à votre compte de développeur.

    * Si vous avez reçu un coupon d’un représentant Microsoft pour payer une campagne publicitaire, cliquez sur **utiliser un coupon**, entrez le code du coupon, puis cliquez sur **appliquer** pour appliquer le coupon à la campagne.

    Lorsque vous avez terminé, cliquez sur **enregistrer et sur suivant** pour passer à l’étape de l' **audience** . Cette étape n’est pas disponible pour les campagnes ad maison, car elles s’exécutent uniquement dans vos propres applications.

9.  Dans la page **audience** , nous vous présenterons les paramètres d’audience que nous recommandons pour votre campagne. Vous pouvez éventuellement ajuster ces informations :
    * **Pays/régions**: sélectionnez jusqu’à 5 pays ou régions dans lesquels vous souhaitez que votre publicité apparaisse. Pour obtenir la liste des pays ou régions pris en charge, consultez [questions courantes sur les campagnes publicitaires](common-questions.md#where-will-my-ad-appear).

    * **Appareils**: choisissez les types d’appareils sur lesquels vous souhaitez que ces publicités s’affichent. Seuls les types d’appareils pris en charge par votre application sont affichés.

    * **Surface**: choisissez **universel** pour autoriser votre publicité à apparaître dans les applications, ainsi que MSN.com, Outlook.com, Skype et d’autres propriétés Microsoft Premium. Choisissez **application** si vous souhaitez que votre publicité s’affiche uniquement dans les applications.

    * **Système d’exploitation**: choisissez le ou les systèmes d’exploitation sur lesquels votre publicité doit s’afficher. Seuls les systèmes d’exploitation pris en charge par votre application sont affichés.

    * **Sexe**: choisissez s’il faut limiter l’audience de votre publicité par sexe.

    * **Plage d’âge**: sélectionnez la ou les tranches d’âge pour le public souhaité.

    Cette section affiche également un graphique **Portée estimée**. Ce graphique montre le public que vous pouvez vous attendre à atteindre avec vos sélections de ciblage actuelles sous forme de pourcentage de tous les utilisateurs d’applications compatibles avec Windows Active Directory dans les marchés sélectionnés.

10.  Si vous avez choisi **augmenter l’engagement dans votre application** comme objectif de campagne, vous pouvez sélectionner l’un des segments de votre client à cibler. Les publicités créées à l’aide de cette campagne s’affichent uniquement pour les clients inclus dans le segment. Vous ne pouvez sélectionner qu’un segment par campagne de publicité. Pour plus d’informations sur les segments, consultez [Créer des segments de clients](create-customer-segments.md). Lorsque vous avez terminé, cliquez sur **enregistrer, puis sur suivant** pour passer à l’étape de **conception ad** . Cette étape n’est pas disponible pour les campagnes ad maison, car elles s’exécutent uniquement dans vos propres applications.

11.  Dans la page de **conception ad** , choisissez l’une des options suivantes :
    * **Généré automatiquement**. Il s’agit de l’option par défaut, qui vous permet de créer une publicité à partir de nos modèles par défaut. Vous pouvez effectuer des sélections pour personnaliser le contenu de votre publicité. nous afficherons un aperçu de ce à quoi ressemblera votre annonce en fonction de vos choix (mise à jour automatiquement à mesure que vous effectuez vos sélections).
        * Dans la liste déroulante **langue** , sélectionnez la langue de votre publicité. Le texte du badge de Microsoft Store s’affiche dans la langue que vous sélectionnez.
        * Pour ajouter une ligne de texte supplémentaire à votre publicité, entrez du texte dans le champ **slogan personnalisé** .
            > [!NOTE]
            > Le texte que vous entrez ici doit être localisé dans la langue sélectionnée. Le slogan personnalisé sera rejeté si le texte n’est pas conforme aux [politiques Bing Ads](https://advertise.bingads.microsoft.com/bing-ads-policies). Lisez cette page pour obtenir des conseils sur le style et les contenus non autorisés.
        * Pour personnaliser l’annonce davantage, développez **Personnaliser la conception d’annonce/Voir toutes les tailles d’annonces** et choisissez l’une des options suivantes :
            * **Couleur d’arrière-plan**. Choisissez parmi les options disponibles.
            * **Images**. Choisissez l’une des images disponibles (extraites de la liste des boutiques de votre application).
            * **Afficher la notation de mon application**. Activez cette case à cocher si vous souhaitez afficher la notation de l’application.
            * **Indiquer que mon application est gratuite**. Si votre application est disponible sur tous les marchés sélectionnés, vous avez la possibilité d’activer cette case à cocher.
            * **Appel à action**. Si vous avez choisi l’option **augmenter l’engagement de votre application** comme objectif de campagne, vous pouvez définir le bouton d’action appeler votre annonce sur **ouvrir**, **lire**, **lire**, **écouter**ou **acheter**.  

    * **Personnalisez**. Choisissez cette option pour utiliser vos propres conceptions d’annonces. Notez que si vous avez précédemment sélectionné un segment de client, vous devez utiliser des éléments créatifs personnalisés. Vous pouvez charger différents fichiers pour chacune des tailles d’annonce disponibles. Les fichiers doivent répondre aux exigences et directives suivantes :
        * Chaque fichier doit être un .png ou .jpg inférieur ou égal à 40 Ko.
        * Vos conceptions d’annonces doivent respecter les critères spécifiés dans la [Microsoft Creative Acceptance Policy](https://about.ads.microsoft.com/solutions/ad-products/display-advertising/creative-acceptance-policies).
        * Le contenu de vos conceptions d’annonces doit être approprié à l’application dont vous faites la promotion. Les conceptions d’annonces qui ne sont pas liées à l’application ne seront pas distribuées aux publicités au sein des autres applications.
        * Tout le contenu de vos conceptions d’annonces doit être clairement lisible. Par exemple, le contenu ne doit pas être flou, pixelisé ou déformé.

12.  Si vous avez un [compte premium](common-questions.md#how-can-i-increase-the-maximum-monthly-budget-amount-allowed-for-my-ad-campaign), vous pouvez utiliser la zone **URL de destination** pour contrôler ce qui se passe quand un client clique sur votre annonce.
    * Si vous laissez la zone vide, quand un client clique sur votre annonce, la liste de vos applications dans le Windows Store s’affiche.
    * Si vous utilisez ajuster, Kochava, régler ou Vungle pour mesurer l’analyse d’installation pour votre application, entrez votre URL de suivi des installations. Lorsque vous enregistrez la campagne, l’URL de suivi est validée pour s’assurer qu’elle correspond à la page de liste de votre application dans la Microsoft Store. Pour plus d’informations sur le suivi des installations avec ces services, consultez la documentation sur l' [ajustement](https://docs.adjust.com/en/), la [Kochava](https://support.kochava.com/), le [réglage](https://help.tune.com/hasoffers/)et la [Vungle](https://support.vungle.com/hc/en-us) .
    * Si vous avez choisi **augmenter l’engagement dans votre application** comme objectif de campagne, vous pouvez spécifier un [URI de lien profond](../launch-resume/handle-uri-activation.md) pour rediriger les clients du segment sélectionné vers une page spécifique de votre application.
    * Si vous spécifiez une destination qui n’est pas la page de description de votre application ni une page interne à votre application, votre campagne est automatiquement suspendue.

13.  Enfin, cliquez sur **Révision** pour vérifier les paramètres de votre campagne de publicité et pour vérifier son budget et les informations de paiement s’il s’agit d’une campagne de publicité payante. Cliquez sur **Confirmer**. Dans les heures qui suivent, vos annonces s’afficheront progressivement sur les appareils.

## <a name="review-ad-campaign-performance"></a>Vérifier les performances des campagnes Active Directory

Pour voir comment vos campagnes s’exécutent, revenez à la page **campagnes publicitaires** . Sélectionnez **Filtres de section** pour limiter les éléments inclus dans le rapport par **Date**, **Objectif de campagne**, **Nom d’application**, **Type de campagne** ou **État**. Outre l’accès à des informations sur les **impressions**, les **clics**, les **conversions** et les **dépenses** de votre campagne, vous pouvez utiliser le rapport pour **interrompre** ou **reprendre** une campagne. Pour plus d’informations, consultez [rapport des campagnes Active Directory](/windows/uwp/publish/ad-campaign-report).

Pour modifier une campagne, sélectionnez son nom dans la liste.