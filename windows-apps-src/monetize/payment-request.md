---
description: L’API de demande de paiement fournit une solution intégrée pour les applications UWP afin de contourner le processus qui consiste à demander à un utilisateur d’entrer des informations de paiement et de sélectionner des modes d’expédition.
title: Simplifier les paiements avec l’API de demande de paiement
ms.date: 09/26/2017
ms.topic: article
keywords: Windows 10, UWP, demande de paiement
ms.openlocfilehash: 3e54767496d9c8d25ce42ea389f43ca2075d128d
ms.sourcegitcommit: 26bb75084b9d2d2b4a76d4aa131066e8da716679
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/06/2020
ms.locfileid: "75685046"
---
# <a name="simplify-payments-with-the-payment-request-api"></a>Simplifier les paiements avec l’API de demande de paiement
L’API de demande de paiement pour les applications UWP est basée sur les spécifications de l' [API de demande de paiement W3C](https://w3c.github.io/browser-payment-api/). Elle vous permet de rationaliser le processus de validation dans vos applications UWP. Les utilisateurs peuvent accélérer l’extraction en utilisant les options de paiement et les adresses d’expédition déjà enregistrées avec leur compte Microsoft. Vous pouvez augmenter votre taux de conversion et réduire le risque de violations de données, car les informations de paiement sont jetons. À compter de Windows 10 Creators Update, les utilisateurs peuvent utiliser leurs options de paiement enregistrées pour payer facilement les expériences des applications UWP.

## <a name="prerequisites"></a>Prérequis
Avant de commencer à utiliser l’API de demande de paiement, vous devez effectuer quelques opérations ou vous en tenir informé.

### <a name="getting-a-merchant-id"></a>Obtention d’un ID de marchand
Dans le cadre du processus de demande de paiement, Microsoft demande des jetons de paiement en votre nom auprès de votre fournisseur de services. Avant de pouvoir commencer à utiliser l’API, nous avons besoin de votre autorisation pour demander ces jetons.  Vous devez suivre quelques étapes pour vous inscrire à un compte de vendeur et fournir l’autorisation nécessaire. Pour ce faire, accédez à [Microsoft seller Center](https://partner.microsoft.com/dashboard/registration/seller?accountprogram=uwp). Une fois que vous avez effectué cette opération, copiez l’ID de commerçant obtenu à partir de l’espace partenaires dans votre application lors de la construction de la demande de paiement. Ensuite, lorsque votre application envoie une demande de paiement, vous recevez un jeton de paiement de votre processeur dont vous aurez besoin pour soumettre votre paiement.

### <a name="geographic-restrictions-and-language-support"></a>Restrictions géographiques et prise en charge des langues
L’API de demande de paiement ne peut être utilisée que par des entreprises américaines pour traiter les transactions dans le États-Unis.

## <a name="using-the-payment-request-api-in-your-app-step-by-step"></a>Utilisation de l’API de demande de paiement dans votre application : étape par étape
Cette section montre comment utiliser l' [API de demande de paiement UWP](https://docs.microsoft.com/uwp/api/windows.applicationmodel.payments) dans votre application. Nous utilisons ici l’API dans sa forme la plus simple pour des raisons de clarté. Pour obtenir un exemple d’utilisation plus avancée de ces API, consultez l' [exemple d’application d’achat UWP sur GitHub](https://github.com/Microsoft/Windows-appsample-shopping).

### <a name="1-create-a-set-of-all-the-payment-options-that-you-accept"></a>1. Créez un ensemble de toutes les options de paiement que vous acceptez.
> [!Note]
> Remplacez le texte **commerçant-ID-from-seller-Portal** par l’ID de commerçant que vous avez reçu du Centre des vendeurs.

[!code-csharp[SnippetEnumerate](./code/PaymentsApiSample/PaymentsApiSample/MainPage.xaml.cs#SnippetEnumerate)]

### <a name="2-pull-the-payment-details-together"></a>2. collectez les détails du paiement ensemble. 

Ces détails s’affichent pour l’utilisateur dans l’application de paiement. 

[!code-csharp[SnippetDisplayItems](./code/PaymentsApiSample/PaymentsApiSample/MainPage.xaml.cs#SnippetDisplayItems)]

### <a name="3-include-the-sales-tax"></a>3. inclure les taxes de vente. 

> [!Important]
> L’API n’ajoute pas d’éléments ou calcule les taxes de vente pour vous. N’oubliez pas que les taux d’imposition varient en fonction de la juridiction. Par souci de clarté, nous utilisons un taux d’imposition hypothétique de 9,5%.

[!code-csharp[SnippetTaxes](./code/PaymentsApiSample/PaymentsApiSample/MainPage.xaml.cs#SnippetTaxes)]

### <a name="4-optional--add-discounts-or-other-modifiers-to-the-total"></a>4. (facultatif) ajoutez des remises ou d’autres modificateurs au total. 

Voici un exemple d’ajout d’une remise pour l’utilisation d’une carte de crédit contoso spécifique pour les éléments affichés. (*Contoso* est un nom fictif.)

[!code-csharp[SnippetDiscountRate](./code/PaymentsApiSample/PaymentsApiSample/MainPage.xaml.cs#SnippetDiscountRate)]

### <a name="5-assemble-all-the-payment-details"></a>5. Assembler tous les détails du paiement.

[!code-csharp[SnippetAggregate](./code/PaymentsApiSample/PaymentsApiSample/MainPage.xaml.cs#SnippetAggregate)]
[!code-csharp[SnippetPaymentOptions](./code/PaymentsApiSample/PaymentsApiSample/MainPage.xaml.cs#SnippetPaymentOptions)]

### <a name="6-submit-the-payment-request"></a>6. envoyez la demande de paiement. 

Appelez la méthode **SubmitPaymentRequestAsync** pour envoyer votre demande de paiement. L’application de paiement apparaît et affiche les options de paiement disponibles.

[!code-csharp[SnippetSubmit](./code/PaymentsApiSample/PaymentsApiSample/MainPage.xaml.cs#SnippetSubmit)]

L’utilisateur est invité à se connecter avec son compte Microsoft.

Une fois que l’utilisateur se connecte, il peut sélectionner des options de paiement et une adresse de livraison précédemment enregistrées.

![Interface utilisateur de demande de paiement](./images/33.png "Interface utilisateur de demande de paiement")

Votre application attend que l’utilisateur appuie sur **payer**, puis il termine la commande.

[!code-csharp[SnippetComplete](./code/PaymentsApiSample/PaymentsApiSample/MainPage.xaml.cs#SnippetComplete)]

Une fois le paiement terminé, l’écran **commande confirmée** est présenté à l’utilisateur.

![Commande confirmée](./images/44.png "Commande confirmée")

## <a name="see-also"></a>Articles associés
- [Documentation de référence sur Windows. ApplicationModel. Payments](https://docs.microsoft.com/uwp/api/windows.applicationmodel.payments)
- [Exemple d’application d’achat UWP sur GitHub](https://github.com/Microsoft/Windows-appsample-shopping)
- [Spécification de l’API de demande de paiement W3C](https://www.w3.org/TR/payment-request/)
- [API de demande de paiement](https://docs.microsoft.com/microsoft-edge/dev-guide/windows-integration/payment-request-api)

