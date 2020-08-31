---
Description: Vous pouvez générer des codes promotionnels pour une application ou un module complémentaire que vous avez publié dans le Microsoft Store.
title: Générer des codes promotionnels
ms.assetid: 9B632266-64EC-4D62-A4C4-55B6643D8750
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, UWP, code de promotion, codes promotionnels, jeton, jetons
ms.localizationpriority: medium
ms.openlocfilehash: 257e04dc70bd93fc262305f1ae0e4915b75bd159
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89157973"
---
# <a name="generate-promotional-codes"></a>Générer des codes promotionnels


L' [espace partenaires](https://partner.microsoft.com/dashboard) vous permet de générer des codes promotionnels pour une application ou un module complémentaire que vous avez publié dans le Microsoft Store. Les codes promotionnels permettent d’offrir facilement à des utilisateurs influents un accès gratuit à votre application ou votre module complémentaire. Vous pouvez également utiliser des codes promotionnels dans des scénarios de service client, en offrant aux utilisateurs un accès gratuit à votre application ou votre module complémentaire, ou pour effectuer un [test bêta](beta-testing-and-targeted-distribution.md) dans Windows 10. 

Chaque code promotionnel a une URL remboursable unique correspondante sur laquelle un client peut cliquer pour échanger le code et installer votre application ou module complémentaire à partir de la Microsoft Store.  Notez que votre application doit passer la phase de publication finale du [processus de certification d’application](the-app-certification-process.md) avant que les clients puissent échanger un code promotionnel pour l’installer.

Vous pouvez générer des codes à usage unique (et en distribuer un à chaque client), ou vous pouvez choisir de générer un code qui peut être utilisé plusieurs fois par un nombre spécifié de clients.

> [!TIP]
> Vous pouvez utiliser [des notifications push ciblées](send-push-notifications-to-your-apps-customers.md) pour distribuer un code promotionnel à un segment de vos clients. Dans ce cas, veillez à utiliser un code promotionnel qui permet à plusieurs clients d’utiliser le même code.


## <a name="promotional-code-policies"></a>Stratégies de code promotionnel

Tenez compte des stratégies suivantes relatives aux codes promotionnels :

-   Vous pouvez générer des codes promotionnels pour n’importe quelle application ou tout module complémentaire (à l’exception des modules complémentaires d’abonnement) que vous avez publiés sur le Microsoft Store. Les clients peuvent échanger les codes sur n’importe quelle version de Windows prise en charge par votre application ou votre module complémentaire.
-   Pour les jeux :
    - Vous pouvez générer jusqu’à 5000 codes promotionnels par jeu.
    - Les codes promotionnels générés pour les jeux n’expirent jamais.
- Pour tous les autres types d’applications ou de modules complémentaires :
    - Au cours d’une période de six mois, vous pouvez générer jusqu’à 1600 codes promotionnels à usage unique, ou un nombre quelconque de codes d’utilisation multiples, de sorte que le total des remboursements autorisés ne dépasse pas 1600.
    - La période de 6 mois commence lorsque vous générez le premier code promotionnel est créé et dure pendant 6 mois, que vous ayez ou non défini une date d’expiration antérieure dans les codes.
    - Les codes créés pendant une période de six mois existante sont comptabilisés dans le nombre de codes générés au cours de cette période, même s’ils expirent après la fin de la période (par exemple, si vous générez un code le dernier jour de la fenêtre de six mois, il sera toujours valable pendant 6 mois à partir de sa création).
-   Vous devez respecter les exigences définies dans le [Contrat du développeur de l’application](/legal/windows/agreements/app-developer-agreement), notamment la section **3k. Codes promotionnels**.

> [!NOTE]
> Vous pouvez utiliser des codes promotionnels même si votre application n’est pas disponible pour les clients (autrement dit, si vous avez sélectionné **rendre ce produit disponible mais non détectable dans le magasin** avec l’option **arrêter l’acquisition : tout client disposant d’un lien direct peut voir la liste des magasins du produit, mais il ne peut le télécharger que s’il a déjà détenu le produit ou s’il a un code promotionnel et utilise une option d’appareil Windows 10** dans la section de [découverte](choose-visibility-options.md#discoverability) de votre envoi. Avec cette option, les clients doivent se trouver sur Windows 10 (y compris la Xbox) afin d’obtenir un code promotionnel pour votre produit.


## <a name="order-promotional-codes"></a>Commander des codes promotionnels

Pour commander des codes promotionnels pour une application ou un module complémentaire :

1.  Dans le menu de navigation de gauche de l' [espace partenaires](https://partner.microsoft.com/dashboard), développez **attirer** , puis sélectionnez **codes de promotion**.

2.   Dans la page **Codes promotionnels**, cliquez sur **Commander des codes**.

3.  Dans la page **Nouvelle commande de codes promotionnels**, entrez les informations suivantes :
    -   Sélectionnez l’application ou le module complémentaire pour lequel vous voulez générer des codes. (Notez que vous ne pouvez pas générer de codes promotionnels pour les modules complémentaires d’abonnement.)
    -   Spécifiez un nom pour la commande. Ce nom permet de différencier les différentes commandes de codes lors de l’examen des données d’utilisation de votre code promotionnel.
    -   Sélectionnez le type de commande. Vous pouvez choisir de générer un ensemble de codes de promotion qui peuvent être utilisés une seule fois, ou vous pouvez choisir de générer un code de promotion qui peut être utilisé plusieurs fois.
    -   Spécifiez le nombre de codes à commander (si vous générez un jeu de codes) ou le nombre de fois où le code peut être échangé (si vous générez un code à utiliser plusieurs fois).
    -   Indiquez à quel moment les codes promotionnels doivent devenir actifs. Pour choisir une date et une heure de début précises, décochez la case **Les codes sont immédiatement actifs**. Dans le cas contraire, les codes seront immédiatement activés (même si votre produit doit avoir terminé le processus de publication pour qu’un client utilise le code).
    -   Indiquez à quel moment les codes promotionnels doivent expirer. Pour choisir une date et une heure d’expiration spécifiques antérieures à 6 mois, désactivez la case à cocher les **codes expirent après 6 mois** .

4.  Cliquez sur **Commander des codes**. Vous revenez ensuite à la page **codes promotionnels** , où vous pourrez voir votre nouvelle commande dans le tableau récapitulatif des commandes de code promotionnel pour cette application.


## <a name="download-and-distribute-promotional-codes"></a>Télécharger et distribuer des codes promotionnels

Pour télécharger une commande de code promotionnel remplie et distribuer les codes aux clients :

1.  Dans le menu de navigation de gauche de l' [espace partenaires](https://partner.microsoft.com/dashboard), développez **attirer** , puis sélectionnez **codes de promotion.**
2.  Cliquez sur le lien de **Téléchargement** pour l’ordre de code promotionnel, puis enregistrez le fichier généré sur votre ordinateur. Ce fichier contient des informations sur l’ordre des codes promotionnels au format de valeurs séparées par des tabulations (. TSV).
3.  Ouvrez le fichier. TSV dans l’éditeur de votre choix. Pour une expérience optimale, ouvrez le fichier. TSV dans une application capable d’afficher les données dans une structure tabulaire, telle que Microsoft Excel. Toutefois, vous pouvez ouvrir le fichier dans n’importe quel éditeur de texte.

    Le fichier contient les colonnes de données suivantes pour chaque code :

    -   **Nom du produit** : nom de l’application ou du module complémentaire auquel le code est associé.
    -   **Nom**de l’ordre : nom de l’ordre dans lequel ce code a été généré.
    -   **Code promotionnel** : code proprement dit. Il s’agit d’une chaîne de 5x5 caractères alphanumériques séparés par des traits d’union. Par exemple : DM3GY-M2GYM-6YMW6-4QHHT-23W2Z
    -   **URL remboursable**: URL qu’un client peut utiliser pour échanger le code et installer votre application ou module complémentaire. L’URL a le format suivant : https://go.microsoft.com/fwlink/?LinkId=532540&mstoken=&lt ;p romotional_code>
    -   **Date de début**: date à laquelle ce code est devenu actif.
    -   **Date d’expiration** : date d’expiration de ce code.
    -   **ID de code** : ID unique de ce code.
    -   **ID de commande**: ID unique de l’ordre dans lequel ce code a été généré.
    -   **Donné à**: champ vide que vous pouvez utiliser pour effectuer le suivi du client auquel vous avez donné le code.
    -   **Disponible**: nombre de fois où le code peut toujours être échangé (au moment de la génération du fichier).
    -   Valeur **échangée**: nombre de fois où le code a été échangé (au moment de la génération du fichier).

4.  Distribuez les URL remboursables à vos clients par le biais de n’importe quel format de communication (par exemple, notifications ciblées, courrier électronique, messages SMS ou cartes imprimées). Nous vous recommandons d’inclure dans votre communication les éléments suivants :
    -   Explication sur l’application ou le module complémentaire du code promotionnel, et éventuellement une description de la raison pour laquelle le client reçoit le code.
    -   URL donnant droit correspondant au code.
    -   Instructions qui guident le client à visiter l’URL remboursable, connectez-vous à l’aide de son compte Microsoft et suivez les instructions pour télécharger et installer votre application.


## <a name="code-redemption-user-experience"></a>Expérience utilisateur d’échange du code

Une fois que vous avez distribué un code promotionnel (ou son URL remboursable) à un client, vous pouvez cliquer sur l’URL pour obtenir gratuitement le produit. En cliquant sur l’URL remboursable, vous lancez une page de **votre code d’échange** authentifié à l’adresse <https://account.microsoft.com/billing/redeem> . Cette page inclut une description de l’application à laquelle l’utilisateur est sur le point d’accéder. Si le client n’est pas connecté avec son compte Microsoft, il peut être invité à le faire. Votre client peut également accéder <https://account.microsoft.com/billing/redeem> au code et le saisir directement.

> [!IMPORTANT]
> Nous vous recommandons de ne pas distribuer de codes promotionnels à vos clients tant que votre produit n’a pas terminé le processus de publication (même si vous avez sélectionné **rendre ce produit disponible mais non détectable dans le Windows Store**). Les clients verront une erreur s’ils essaient d’utiliser un code promotionnel pour un produit qui n’a pas encore été publié.

Une fois que le client a cliqué sur **rembourser**, le Microsoft Store s’ouvre sur la page vue d’ensemble de l’application (si elle se trouve sur un appareil Windows 10 ou Windows 8.1), où ils peuvent cliquer sur **installer** pour télécharger et installer l’application gratuitement. Si le client se trouve sur un ordinateur ou un périphérique sur lequel le Microsoft Store n’est pas installé, le lien lance la page Web Microsoft Store de l’application. Le code sera appliqué à la compte Microsoft du client, afin qu’il puisse télécharger gratuitement l’application sur un appareil Windows (associé au même compte Microsoft).

> [!NOTE]
> Dans certains cas, un client peut voir un bouton **acheter** au lieu de **installer**, même si l’application a été échangée avec succès par le biais du code promotionnel. Le client peut cliquer sur **acheter** pour installer l’application gratuitement.


## <a name="review-your-promotional-codes"></a>Passer en revue vos codes promotionnels

Pour consulter un résumé détaillé des commandes de code promotionnel pour vos applications et modules complémentaires, accédez à la page **codes promotionnels** (dans le menu de navigation de gauche de l’espace partenaires, développez **attirer** , puis sélectionnez **codes de promotion**). Vous pouvez consulter les détails suivants pour tous vos codes promotionnels actuels et inactifs :
-   Nom de la commande
-   Application ou module complémentaire
-   Date de début
-   Date d’expiration
-   Disponible
-   Échangés

Vous pouvez également [Télécharger](#download-and-distribute-promotional-codes) une commande à partir de cette table.

 