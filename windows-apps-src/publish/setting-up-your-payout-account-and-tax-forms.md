---
Description: Pour recevoir de l’argent des ventes de l’application dans le Microsoft Store, vous devez configurer votre compte de paiement et remplir les formulaires fiscaux nécessaires.
title: Configurer votre compte de paiement et vos déclarations de taxe
ms.assetid: 690A2EBC-11B1-4547-B422-54F15A6C26A7
ms.date: 1/17/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: b7f7209d77e05453126f885e37a251e8b6511e95
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89172873"
---
# <a name="set-up-your-payout-account-and-tax-forms"></a>Configurer votre compte de paiement et vos déclarations de taxe

> [!NOTE]
> Si vous avez besoin d’aide concernant les paiements, notamment sur la configuration des comptes de paiement, les paiements manquants, la mise en attente des paiements ou d’autres sujets, contactez le support [ici](https://partner.microsoft.com/support).

Pour recevoir de l’argent des ventes de l’application dans le Microsoft Store, vous devez configurer votre compte de paiement et remplir les formulaires fiscaux nécessaires dans l' [espace partenaires](https://partner.microsoft.com/dashboard).

Si vous envisagez de référencer uniquement des applications gratuites (et que vous ne voulez pas proposer d’achats in-app ou utiliser Microsoft Advertising), vous n’avez pas besoin de configurer de compte de revenu ni de remplir de déclaration fiscale. Si vous changez d’avis par la suite et que vous décidez de vendre des applications (ou des modules complémentaires), vous pouvez configurer votre compte de paiement et remplir les formulaires fiscaux à ce moment-là. Vous ne serez pas en mesure de soumettre des applications ou des modules complémentaires payants tant que votre compte de paiement et votre profil fiscal n’ont pas été remplis.

> [!NOTE]
> Sur [certains marchés](account-types-locations-and-fees.md#developer-account-and-app-submission-markets), les développeurs peuvent uniquement envoyer des applications gratuites. Si votre compte est inscrit sur l’un de ces marchés, vous n’aurez pas la possibilité de configurer un compte de paiement.

Une fois que vous avez [configuré votre compte de développeur](opening-a-developer-account.md), vous devez effectuer deux opérations pour pouvoir vendre des applications (ou des modules complémentaires) dans le Microsoft Store :

- [Remplissez votre déclaration de taxe](#tax-forms)
- [Configurez votre compte de paiement](#payout-account)

> [!NOTE]
> Pour plus d’informations sur le mode de paiement et le moment où vous serez payé pour l’argent de vos applications, consultez la page de [paiement](getting-paid-apps.md).

## <a name="tax-forms"></a>Déclaration de taxe

### <a name="filling-out-your-tax-forms"></a>Remplissez votre déclaration de taxe

Tout d’abord, vous devez créer un profil fiscal et l’attribuer aux programmes auxquels vous participez. Vous pouvez créer votre *Profil fiscal* pour le Microsoft Store en procédant comme suit :

- Indiquez votre pays de résidence et votre nationalité.
- Remplissez les déclarations de taxe appropriées.

Vous pouvez compléter et soumettre vos formulaires fiscaux par voie électronique dans l’Espace partenaires. En général, vous n’avez pas besoin d’imprimer ou d’envoyer de formulaires par courrier.

> [!IMPORTANT]
> Les différents pays et régions ont des exigences fiscales différentes. Le montant exact des taxes dont vous devez vous affranchir varie selon les pays et les régions où vous vendez vos applications. Pour savoir dans quels pays Microsoft vous dispense des taxes d’utilisation et sur les ventes, voir le [Contrat du développeur de l’application](/legal/windows/agreements/app-developer-agreement) Dans d’autres pays, selon l’endroit où vous êtes inscrit, vous devrez peut-être verser directement les taxes d’utilisation et les taxes sur les ventes de vos applications à l’administration fiscale locale. En outre, les recettes de la vente d’application que vous recevez peuvent être considérées comme du revenu imposable. Nous vous encourageons vivement à contacter l’autorité appropriée pour votre pays ou région qui peut vous aider à déterminer les bonnes informations fiscales pour vos activités de développeur Microsoft Store.

1. Dans l' [espace partenaires](https://partner.microsoft.com/dashboard), sélectionnez l’icône **paramètres de compte** dans le coin supérieur droit, puis sélectionnez Paramètres du **développeur**.
2. Dans le menu de navigation situé à gauche, sélectionnez **Paiement et taxes**, puis **Attributions de paiement et de taxes**.

    ![Attribution du profil de paiement et de taxe](images/payout-tax-profile-assignment.png)

3. Sélectionnez la combinaison programme et nom du vendeur pour laquelle vous souhaitez configurer des informations fiscales.

    ![Paiement-sélectionner un ID de vendeur](images/payout-select-seller-id.png)

4. Si vous souhaitez utiliser un profil fiscal existant, sélectionnez-le dans la liste déroulante. Dans le cas contraire, sélectionnez **Créer un nouveau profil** puis appuyez sur **Envoyer**. Vous serez redirigé vers la page des profils fiscaux.
5. Cliquez sur le bouton **Modifier** pour modifier vos informations fiscales.
6. Sélectionnez la case d’option appropriée, puis indiquez votre pays si vous y êtes invité. Cette étape détermine l’entité métier Microsoft qui sera utilisée pour effectuer des versements sur votre compte.

    ![Paiement-sélectionner le pays fiscal](images/payout-select-tax-country.png)

7. En fonction de vos sélections à l’étape 6, vous serez invité à fournir les informations fiscales requises pour votre pays.

> [!NOTE]
> Quel que soit votre pays de résidence ou de citoyenneté, vous devez remplir États-Unis formulaires fiscaux pour vendre des applications ou des modules complémentaires par le biais du Microsoft Store. Les développeurs répondant à certains critères de résidence aux États-Unis doivent remplir le formulaire W-9 du fisc américain (IRS). Les autres développeurs résidant en dehors des États-Unis doivent compléter le formulaire W-8 de l'IRS. Vous pouvez remplir ces formulaires en ligne lorsque vous complétez votre profil fiscal.

### <a name="withholding-rates"></a>Taux de retenue d’impôt

Les informations que vous envoyez dans vos formulaires fiscaux déterminent le taux de retenue d’impôt approprié. Le taux de retenue d’impôt s’applique uniquement aux ventes que vous réalisez aux États-Unis. Les ventes réalisées dans des régions en dehors des États-Unis ne sont pas soumises à la retenue d’impôt. Le taux de retenue d’impôt peut varier, mais pour la plupart des développeurs inscrits en dehors des États-Unis, le taux par défaut est de 30 %. Vous pouvez réduire ce taux si votre pays a signé une convention fiscale avec les États-Unis.

### <a name="tax-treaty-benefits"></a>Avantages issus des conventions fiscales

Si vous êtes situé en dehors des États-Unis, vous pourrez peut-être tirer parti des avantages liés aux conventions fiscales. Ces avantages varient d’un pays à l’autres, et peuvent vous permettre de réduire le montant des taxes que le Microsoft Store retenue. Vous pouvez demander des avantages issus des conventions fiscales en complétant la partie II du formulaire W-8BEN. Nous vous conseillons de communiquer avec les ressources appropriées dans votre pays ou région pour déterminer si ces avantages peuvent s’appliquer à vous.

> [!NOTE]
> Aucun numéro individuel de contribuable (ITIN) n’est nécessaire pour recevoir des paiements de la part de Microsoft ou pour bénéficier des avantages découlant des conventions fiscales.

## <a name="payout-account"></a>Compte de paiement

Un compte de paiement est le compte bancaire sur lequel nous envoyons les recettes de vos ventes. Vous pouvez afficher tous les comptes de paiement que vous avez entrés dans la page de profil.

> [!NOTE]
> Sur certains marchés, PayPal peut être utilisé pour votre compte de paiement. Consultez les [seuils de paiement, les méthodes et les délais](payment-thresholds-methods-and-timeframes.md) pour savoir si PayPal est pris en charge pour un marché spécifique et lisez les informations relatives à [PayPal](#paypal-info) ci-dessous pour plus de détails.

### <a name="create-a-payment-profile"></a>Créer un profil de paiement

1. Dans l' [espace partenaires](https://partner.microsoft.com/dashboard), sélectionnez l’icône d’engrenage **paramètres** dans le coin supérieur droit, puis sélectionnez **paramètres du développeur**.
2. Sous le titre *Paiement et taxe*, sélectionnez **Attribution de profil de paiement et de taxe**.

    > [!NOTE]
    > Puisqu’il s’agit d’informations sensibles, vous pouvez être invité à vous reconnecter.

3. Sélectionnez le mode de paiement que vous souhaitez configurer.

    ![Sélection du type de compte de paiement](images/payout-account-type-selection.png)

4. Sélectionnez un profil de paiement existant ou cliquez sur **Créer un nouveau profil de paiement** pour créer un nouveau profil pour le mode de paiement choisi.

> [!NOTE]
> Si, pour une raison quelconque, votre compte n’est pas prêt à recevoir des fonds de Microsoft, vous pouvez activer la case à cocher **conserver mon paiement** . Vous continuerez à gagner vos ventes, mais les paiements ne seront pas distribués tant que vous n’aurez pas désactivé **conserver mon paiement.**

### <a name="create-a-bank-based-payment-profile"></a>Créer un profil de paiement bancaire

Si vous avez choisi d’utiliser un compte bancaire pour recevoir des paiements, vous devez effectuer la procédure suivante pour le configurer.

1. Sur la page *profil bancaire*, fournissez les informations requises sur votre banque.
2. Fournissez les détails de votre compte bancaire.

    > [!NOTE]
    > Les champs que vous utilisez pour fournir les informations de votre compte acceptent uniquement des caractères alphanumériques.

    ![Informations bancaires sur le paiement](images/payout-bank-info.png)

3. Fournissez les détails du bénéficiaire.
4. De retour sur la page*Attribution de profil*, sélectionnez la devise que vous souhaitez que nous utilisions pour émettre vos paiements.

    > [!WARNING]
    > Assurez-vous que votre banque accepte la devise de paiement que vous sélectionnez.

5. Vous devrez sélectionner un profil de paiement pour chaque programme auquel vous participez, même si vous pouvez utiliser le même profil pour plusieurs programmes.

    ![Profil bancaire d’utilisation de paiement](images/payout-use-bank-profile.png)

6. Cliquez sur Envoyer pour enregistrer vos changements.

> [!NOTE]
> Microsoft peut prendre jusqu’à 48 heures pour valider les informations de votre profil. Une fois ce processus terminé, *l’état de vérification* indique **Terminé**

Pour garantir le succès du paiement, notez les points suivants :

- Le **nom du titulaire du compte** entré pour votre compte de paiement dans l’espace partenaires doit correspondre exactement au nom associé à votre compte bancaire. Par exemple, si le nom de votre compte bancaire contient un deuxième prénom, ajoutez un deuxième prénom au **nom du titulaire du compte**.
- Les paiements sont transférés directement de Microsoft vers votre compte bancaire en devise USD.
- Les informations bancaires saisies dans l’espace partenaires en caractères latins sont traduites en caractères cyrilliques.

### <a name="editing-existing-payment-profiles"></a>Modification des profils de paiement existants

Vous pouvez modifier les profils de paiement existants si vous devez apporter des modifications ou rectifier des informations erronées.

1. Dans l' [espace partenaires](https://partner.microsoft.com/dashboard), sélectionnez l’icône d’engrenage **paramètres** dans le coin supérieur droit, puis sélectionnez **paramètres du développeur**.
2. Sous le titre *Paiement et taxe*, sélectionnez **Profils de paiement et de taxe**.
3. Vos profils de paiement sont listés avec leur état. Recherchez le profil que vous souhaitez modifier, puis cliquez sur **Modifier** à droite de l’écran

> [!IMPORTANT]
> La modification de votre compte de paiement peut retarder vos paiements jusqu’à un cycle de paiement. Ce retard se produit parce que nous devons vérifier le changement de compte, comme nous l’avons fait lorsque vous avez configuré le compte de paiement pour la première fois. Vous serez payé pour le montant total une fois que votre compte aura été vérifié. Tout paiement dû dans le cycle de paiement actuel sera ajouté au cycle suivant Pour plus d’informations, consultez la section[Processus de paiement](getting-paid-apps.md).

### <a name="paypal-info"></a>Informations PayPal

Dans Sélectionner les pays et les régions, vous pouvez créer un compte de paiement en entrant vos informations PayPal. Toutefois, avant de choisir PayPal comme option de compte de paiement :

- Vérifiez les [seuils, les méthodes et les délais de paiement](payment-thresholds-methods-and-timeframes.md) pour vérifier si PayPal est un mode de paiement pris en charge dans votre pays ou région.
- Consultez les FAQ suivantes. Selon votre situation, PayPal peut ne pas être la meilleure option de compte de paiement pour vous et vous devrez peut-être opter pour un compte bancaire.

Questions fréquemment posées sur l’utilisation de PayPal comme mode de paiement :

- **De quels paramètres PayPal ai-je besoin pour recevoir des paiements ?** Vous devez vous assurer que votre compte PayPal ne bloque pas les paiements par eCheck. Ce paramètre est géré dans la page Préférences de réception de paiement de PayPal. Pour plus d’informations, consultez la [page de configuration de compte PayPal](https://developer.paypal.com/webapps/developer/docs/classic/admin/setup-account/).
- **Est-ce que mon pays/ma région est pris en charge ?** Consultez les [seuils, les méthodes et les délais de paiement](payment-thresholds-methods-and-timeframes.md) pour savoir quels pays/quelles régions prennent en charge PayPal comme mode de paiement.
- **Mon compte PayPal doit-il être inscrit dans le même pays ou la même région que mon compte espace partenaires ?** Non. Quand vous configurez un compte PayPal, vous pouvez accepter la configuration par défaut. Vous ne devriez pas rencontrer de problèmes avec d’autres pays/régions et devises, sauf si vous avez bloqué le paiement dans certaines devises. Ce paramètre est géré dans la page Préférences de réception de paiement de PayPal.
- **Dois-je accepter les paiements PayPal manuellement ?** Non. Par défaut, les comptes PayPal sont configurés de manière à contraindre les utilisateurs à accepter les paiements manuellement. Par conséquent, le paiement sera renvoyé si vous ne l’acceptez pas dans un délai de 30 jours. Vous pouvez modifier ce paramètre en désactivant l’option « Me demander » dans la page Paramètres supplémentaires de PayPal.
- **Quelles sont les devises prises en charge par PayPal ?** Veuillez consulter la [page de support de PayPal](https://developer.paypal.com/docs/classic/api/currency-codes/#paypal) pour obtenir la liste actuelle

### <a name="specific-requirements-for-certain-countriesregions"></a>Exigences particulières pour certains pays/certaines régions

Dans certains pays et certaines régions, il convient de respecter des exigences supplémentaires pour les comptes de paiement. Si vous résidez au Pakistan, en Russie ou en Ukraine, notez les obligations suivantes.

#### <a name="pakistan"></a>Pakistan

Le formulaire est une exigence de réglementation bancaire pakistanaise. Il est utilisé pour indiquer le but et le motif de la réception de fonds étranger. Par conséquent, chaque fois que vous êtes éligible à un paiement mensuel de Microsoft, vous devez envoyer le formulaire R à votre banque pour recevoir le paiement. Pour savoir comment obtenir une copie du formulaire R, contactez votre branche locale.

Vous devrez soumettre un formulaire R à votre banque chaque mois où vous êtes admissible à un paiement. Par exemple, si vous pensez recevoir un paiement tous les mois, vous devrez envoyer le formulaire R 12 fois (une fois par mois).

Une fois que le paiement a été envoyé à votre banque, vous disposez de 30 jours pour envoyer le formulaire R. Passé ce délai, les fonds sont renvoyés à Microsoft.

#### <a name="russia"></a>Russie

Si vous êtes un développeur vivant en Russie, vous pouvez avoir besoin de fournir des documents à votre banque pour que celle-ci puisse déposer des fonds sur votre compte. Lorsque vous serez admissible au paiement, nous vous fournirons les documents suivants par e-mail :

1. Avis d’attestation (AC) : indique le montant du paiement devant être transféré sur votre compte.
2. App Developer Agreement (ADA) - copie signée de l’accord de développeur qui doit être contresignée.

Pour garantir le succès du paiement, notez les points suivants :

- Le **nom du titulaire du compte** entré pour votre compte de paiement dans l’espace partenaires doit correspondre exactement au nom associé à votre compte bancaire. Par exemple, si le nom de votre compte bancaire contient un deuxième prénom, ajoutez un deuxième prénom au **nom du titulaire du compte**.
- Les paiements sont transférés directement de Microsoft vers votre compte bancaire en rouble (RUB) russe.
- Les informations bancaires saisies dans l’espace partenaires en caractères latins sont traduites en caractères cyrilliques.
- Le paiement doit être effectué sur un compte bancaire et non sur une carte bancaire.

#### <a name="ukraine"></a>Ukraine

Si vous êtes un développeur vivant en Ukraine, vous pouvez avoir besoin de fournir des documents à votre banque pour que celle-ci puisse déposer des fonds sur votre compte. Lorsque vous serez admissible au paiement, nous vous fournirons les documents suivants par e-mail :

1. Avis d’attestation (AC) : indique le montant du paiement devant être transféré sur votre compte.
2. App Developer Agreement (ADA) - copie signée de l’accord de développeur qui doit être contresignée.
3. Contrat de modification (AA) : votre banque peut utiliser ce document pour vous aider à identifier vos fonds de paiement.

Microsoft fournit ces trois documents lors de votre première tentative de paiement. Pour les paiements suivants, vous recevrez uniquement l’avis d’attestation (document AC). Conservez les documents ADA et AA au cas où vous en auriez besoin pour recevoir de futurs paiements de votre banque.

### <a name="create-a-paypal-payment-profile"></a>Créer un profil de paiement PayPal

Si vous avez choisi d’utiliser un compte bancaire pour recevoir des paiements, vous devez effectuer la procédure suivante pour le configurer.

1. Sur la page *PayPal*, fournissez les informations requises sur votre compte PayPal.
2. Fournissez les détails de votre compte PayPal.

    > [!NOTE]
    > Les champs que vous utilisez pour fournir les informations de votre compte acceptent uniquement des caractères alphanumériques.

    ![Paiement des infos PayPal](images/payout-paypal-info.png)

3. Fournissez les détails du bénéficiaire.
4. De retour sur la page*Attribution de profil*, sélectionnez la devise que vous souhaitez que nous utilisions pour émettre vos paiements.
5. Vous devrez sélectionner un profil de paiement pour chaque programme auquel vous participez, même si vous pouvez utiliser le même profil pour plusieurs programmes.
6. Cliquez sur Envoyer pour enregistrer vos changements.