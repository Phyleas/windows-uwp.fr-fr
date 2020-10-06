---
Description: Les rapports de paiement affichent des informations sur l’argent que vous avez obtenu avec vos applications et modules complémentaires. Elle vous permet également de savoir quand vous recevrez des paiements et le montant que vous allez percevoir.
title: Rapports de revenu
ms.assetid: F0D070BE-8267-4CC9-B0D2-085EBA74AC98
ms.date: 08/02/2019
ms.topic: article
keywords: Windows 10, UWP, Résumé du paiement, relevé, paiements, bénéfices, versements, paiement, frais
ms.localizationpriority: medium
ms.openlocfilehash: 7eab86cc1856f5ad206aa8bbceb2f2e04f5410d2
ms.sourcegitcommit: a30808f38583f7c88fb5f54cd7b7e0b604db9ba6
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/06/2020
ms.locfileid: "91763106"
---
# <a name="payout-reports"></a>Rapports de revenu

Le **Résumé du paiement** vous donne des détails sur l’argent que vous avez obtenu auprès de Microsoft. Elle vous permet également de savoir quand vous recevrez des paiements et le montant que vous allez percevoir.

Si vous vendez des produits dans la place de marché Azure, vous verrez également des informations sur les versements réussis dans le **Résumé du paiement**. Pour plus d’informations sur le processus de paiement d’Azure Marketplace, voir [Politiques concernant la participation à Microsoft Azure Marketplace](/legal/marketplace/participation-policy) et [Microsoft Azure Marketplace Publisher Agreement (Contrat d’éditeur Microsoft Azure Marketplace)](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RE3ypvt).

> [!NOTE]
> Pour être éligible au paiement, votre produit doit atteindre le [seuil de paiement](payment-thresholds-methods-and-timeframes.md) de 50 $. Pour plus d’informations sur le seuil de paiement, consultez cette page et examinez le contrat du développeur de l’application.

> [!NOTE]
> Si vous avez besoin d’aide concernant les paiements, notamment sur la configuration des comptes de paiement, les paiements manquants, la mise en attente des paiements ou d’autres sujets, contactez le support [ici](https://developer.microsoft.com/windows/support).

## <a name="access-the-payout-summary-pages"></a>Accéder aux pages de synthèse des revenus

Pour ouvrir l’une des pages de synthèse des revenus :

1. Sélectionnez l’icône Revenu dans le coin supérieur droit.
2. Sélectionnez Historique des transactions, Paiements ou Exporter les données.

## <a name="transaction-history-page"></a>Page Historique des transactions

Cette page affiche tous vos bénéfices individuels avec pour chacun la date, le type et le montant. Vous pouvez afficher ces informations pour une période déterminée, mais aussi filtrer par ID d’inscription, Programme, ID de paiement, Type de bénéfice, Levier et Statut. Les données sont disponibles pour l’exercice actuel (du 1er juillet au 30 juin) et les deux exercices précédents.

Pour afficher plus de détails sur un bénéfice, sélectionnez la flèche vers le bas située dans la partie droite de la page. Vous obtenez alors le levier, le montant de chiffre d’affaires et le produit. Si, pour une raison quelconque, ces données ne sont pas disponibles, mais que vous avez besoin d’y accéder, contactez le [support technique](https://developer.microsoft.com/windows/support). Si le résultat est le résultat d’un ajustement et non d’une transaction, les champs de produit ne sont pas affichés.

Pour exporter les données de transaction sur cette page, sélectionnez Exporter, puis suivez les instructions de la page exporter des données. Les fichiers exportés à partir de la page historique des transactions affichent les données dans la devise de la transaction, les bénéfices dans la devise de la transaction et le dollar des États-Unis, et la valeur payée dans paiement à la devise

## <a name="payments-page"></a>Page Paiements

Les totaux de cette page représentent tous les programmes auxquels vous participez. Vous pouvez filtrer par ID de participant, Programme, ID de paiement et Type de bénéfice. Les montants sont indiqués en dollars américains. La valeur payée est également affichée dans la devise d’arrivée.

| Domaine                   | Description                                                                                  |
|------------------------|----------------------------------------------------------------------------------------------|
| Total payé cette année   | Le total cumulé payé pour vous cette année, en dollars américains, pour tous vos programmes.       |
| Prochain paiement estimé | Seul le paiement suivant vous vient (même s’il y en a d’autres bientôt disponibles), en dollars américains. |
| Dernier paiement           | Le montant (en dollars américains), le nom du programme et le programme de votre paiement le plus récent.           |
| Paiements par source     | Montant des paiements, en dollars américains, représentés par le programme au cours des 12 derniers mois.           |
| Paiements               | Sélectionnez payant ou en attente, puis triez comme vous le souhaitez. Pour plus d’informations sur un paiement spécifique, sélectionnez Afficher. Pour télécharger une copie du relevé des envois de paiements, sélectionnez Télécharger. Notez que les données de l’historique des transactions peuvent prendre jusqu’à 24 heures pour apparaître. vous risquez donc de ne pas voir les bénéfices associés immédiatement. |

Pour exporter les données de cette page, sélectionnez Exporter, puis suivez les instructions de la page exporter des données.

## <a name="payment-status"></a>État du paiement

| État du bénéfice           | Motif                                                                                                                                      | Intervention nécessaire du partenaire ?                                   |
|--------------------------|---------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------|
| Non traité              | Le revenu est éligible au paiement. Il reste dans cet état pendant une période de réflexion telle que définie dans le guide du programme d’incentives. | Non                                                         |
| À venir                 | Commande de paiement générée en attente de révisions internes avant le traitement du paiement.                                                               | Non                                                         |
| Facture fiscale en attente      | Votre facture fiscale est incomplète ou non valide.                                                                                                  | Vous devez mettre à jour votre facture fiscale avant de pouvoir être payé |
| Rejeté pendant la vérification   | Le paiement a été rejeté pendant la révision.                                                                                                     | Contactez le [Support Microsoft](https://developer.microsoft.com/windows/support) pour obtenir des détails                      |
| Échec                   | Le paiement a échoué en raison d’une erreur système Microsoft.                                                                                         | Contactez le [Support Microsoft](https://developer.microsoft.com/windows/support) pour obtenir des détails                      |
| En cours              | Le paiement est en cours.                                                                                                                 | Non                                                         |
| Paiement incorrect        | Le remboursement est en cours.                                                                                                       | Non                                                         |
| Envoyé                     | Le paiement a été envoyé à votre banque.                                                                                                     | Non                                                         |
| Retraitement             | Le paiement a rencontré une erreur système Microsoft et est en cours de retraitement.                                                                  | Non                                                         |
| Inversé                 | Le paiement a été inversé par votre banque et sera renvoyé dans le prochain cycle de paiement.                                                     | Non                                                         |
| Facture fiscale rejetée     | Votre facture fiscale a été rejetée pendant la vérification. Tous les paiements en attente seront mis en attente tant que l’examen de la facture fiscale n’aura pas été effectué.                 | Contactez le [Support Microsoft](https://developer.microsoft.com/windows/support) pour obtenir des détails                      |
| Facture fiscale en cours de vérification | Votre facture fiscale est en cours de vérification. Votre paiement sera débloqué une fois que la facture fiscale aura été approuvée.                                   | Non                                                         |
| Rejeté                 | Le paiement a été rejeté par votre banque.                                                                                                      | Contactez votre banque pour obtenir des détails.                             |

## <a name="export-data-page"></a>Page Exporter les données

Suivez les instructions de cette page pour exporter les données souhaitées.

Remarques :

- La page Exporter les données ne s’actualise pas automatiquement. Vous devrez peut-être l’actualiser manuellement pour afficher les données les plus récentes.
- Votre filtre peut déclencher l’erreur Pas de données disponibles. Cela signifie probablement que vous avez laissé la période de temps par défaut sélectionnée à trois mois, puis que vous avez sélectionné un ID de paiement d’un gain en dehors de cette période. Élargissez la période et réessayez.

## <a name="payments"></a>Paiements

![Exporter les paiements](images/pc-export-payments.png)

Cette option vous permet de télécharger les paiements que vous avez reçus à votre banque pour un programme donné, les taxes associées et le montant cumulé des bénéfices. Ce rapport étant utilisé pour divers programmes de l’Espace partenaires, certaines colonnes peuvent ne pas s’appliquer à votre rapport. Ces colonnes sont signalées ci-dessous.

| Nom de la colonne              | Description                                                                                                                               |
|--------------------------|-----------------------------------------------------------------------------------------------------------------------------------------  |
| participantID            | Identité principale du revenu partenaire dans le cadre du programme                                                                             |
| participantIDType        | En général, ID de programme pour les programmes de motivation et ID de vendeur pour les programmes de boutique                                                                |
| participantName          | Nom du partenaire de revenu                                                                                                               |
| programName              | Nom du programme d’incentives/du Store                                                                                                              |
| earned                   | Montant du chiffre d’affaires dans la devise d’arrivée pour le programme/ID de participant en question                                                                       |
| earnedUSD                | Montant de chiffres d’affaires enregistré pour le programme/ID de participant, en USD                                                                                      |
| withheldTax              | Montant des taxes retenues dans la devise d’arrivée pour le programme/ID de participant                                                               |
| salesTax                 | Montant total des taxes sur les ventes dans la devise d’arrivée pour le programme/ID de participant (s’applique uniquement aux programmes d’incentives)                   |
| serviceFeeTax            | Montant total de serviceFeeTax dans la devise d’arrivée pour le programme/ID de participant (s’applique uniquement aux programmes du Store et à la Place de marché Azure) |
| totalPayment             | Paiement total dans la devise locale à l’exclusion de la retenue à la source et en incluant les taxes sur les ventes (le cas échéant) pour le programme/ID de participant   |
| currencyCode             | Code de la devise d’arrivée                                                                                                                      |
| paymentMethod            | Méthode utilisée pour payer le partenaire, par exemple, transfert bancaire électronique, note de crédit                                                     |
| paymentId                | Identificateur unique du paiement. Ce nombre est généralement visible dans votre relevé bancaire. (applicable uniquement aux paiements SAP)              |
| paymentStatus            | État du paiement                                                                                                                            |
| paymentStatusDescription | Description conviviale de l’état du paiement                                                                                                    |
| paymentDate              | Date à laquelle le paiement a été envoyé par Microsoft                                                                                                      |

## <a name="transaction-history"></a>Historique des transactions

![Exporter l’historique des transactions](images/pc-export-transaction.png)

Cette option permet de télécharger chaque ligne de bénéfice que vous voyez dans la page Historique des transactions, le type de bénéfice, la date, le montant de transaction associé, le client, le produit et d’autres détails transactionnels applicables à vos programmes.

| Nom de la colonne                    | Description                                                                                                                              | Applicabilité pour les incentives/le Store/la Place de marché Azure           |
|--------------------------------|------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------|
| earningId                      | Identificateur unique de chaque revenu                                                                                                       | Tous                                                            |
| participantId                  | Identité principale du revenu partenaire dans le cadre du programme                                                                            | Tous                                                            |
| participantIdType              | Principalement, ID des programmes d’incentives et ID de vendeur pour les programmes du Store et la Place de marché Azure                                          | Tous                                                            |
| participantName                | Nom du partenaire de revenu                                                                                                              | Tous                                                            |
| partnerCountryCode             | Emplacement/pays du partenaire de revenu                                                                                                  | Tous                                                            |
| programName                    | Nom du programme d’incentives/du Store                                                                                                             | Tous                                                            |
| transactionId                  | Identificateur unique de la transaction                                                                                                    | Tous                                                            |
| transactionCurrency            | Devise dans laquelle la transaction du client d’origine a été effectuée (il ne s’agit pas de la devise locale du partenaire)                                     | Tous                                                            |
| transactionDate                | Date de la transaction Utile pour les programmes où de nombreuses transactions contribuent à un revenu                                           | Tous                                                            |
| transactionExchangeRate        | Date du taux de change utilisé pour afficher le montant en USD correspondant de la transaction                                                                 | Tous                                                            |
| transactionAmount              | Montant de la transaction dans la devise de transaction d’origine en fonction du revenu généré                                              | Tous                                                            |
| transactionAmountUSD           | Montant de la transaction en USD                                                                                                                | Tous                                                            |
| lever                          | Indique une règle métier pour le revenu                                                                                                  | Tous                                                            |
| earningRate                    | Taux d’incentives appliqué au montant de la transaction pour générer un revenu                                                                      | Tous                                                            |
| quantité                       | Varie selon le programme. Indique la quantité facturée pour les programmes transactionnels                                                            | Tous                                                            |
| quantityType                   | Indique le type de quantité par exemple, quantité facturée, MAU                                                                             | Tous                                                            |
| earningType                    | Indique s’il s’agit de frais, de remise, de coop, de vente, etc.                                                                                          | Tous                                                            |
| earningAmount                  | Montant du revenu dans la devise de la transaction d’origine                                                                                      | Tous                                                            |
| earningAmountUSD               | Montant du revenu en USD                                                                                                                    | Tous                                                            |
| earningDate                    | Date du revenu                                                                                                                      | Tous                                                            |
| calculationDate                | Date à laquelle le revenu a été calculé dans le système                                                                                            | Tous                                                            |
| earningExchangeRate            | Taux de change utilisé pour afficher le montant USD correspondant                                                                                  | Tous                                                            |
| exchangeRateDate               | Date du taux de change utilisée pour calculer le montant USD du revenu                                                                                   | Tous                                                            |
| paymentAmountWOTax             | Montant du revenu (hors taxe) dans la devise de destination pour les paiements « envoyés » uniquement                                                                 | Tous                                                            |
| paymentCurrency                | Devise de destination choisie par le partenaire dans le profil de paiement. Affichée uniquement pour les paiements envoyés                                                   | Tous                                                            |
| paymentExchangeRate            | Taux de change utilisé pour calculer paymentAmountWOTax dans la devise de paiement avec ExchangeRateDate                                            | Tous                                                            |
| claimId                        | Identificateur unique de la demande                                                                                                              | Incentives – certains programmes uniquement                                |
| planId                         | Identificateur unique du plan                                                                                                               | Incentives – certains programmes uniquement                                |
| paymentId                      | Identificateur unique du paiement. Ce numéro figure généralement sur votre relevé bancaire                                                 | Paiements SAP uniquement                                              |
| paymentStatus                  | État du paiement                                                                                                                           | Tous                                                            |
| paymentStatusDescription       | Description conviviale de l’état du paiement                                                                                                   | Tous                                                            |
| customerId                     | Sera toujours vide                                                                                                                     | Programmes d’incentives uniquement (exception : OEM) et la Place de marché Azure |
| customerName                   | Sera toujours vide                                                                                                                     | Programmes d’incentives uniquement (exception : OEM) et la Place de marché Azure |
| partNumber                     | Sera toujours vide                                                                                                                     | Certains programmes d’incentives et du Store et la Place de marché Azure        |
| ProductName                    | Nom de produit lié à la transaction                                                                                                       | Tous                                                            |
| productId                      | Identificateur de produit unique                                                                                                                | Store et Place de marché Azure                                    |
| parentProductId                | Identificateur de produit parent unique. Notez également que si la transaction ne présente aucun produit parent, l'ID du produit parent est l'ID du produit. | Store et Place de marché Azure                                    |
| parentProductName              | Nom du produit parent. Notez également que si la transaction ne présente aucun produit parent, le nom du produit parent est identique au nom du produit.   | Store et Place de marché Azure                                    |
| productType                    | Type de produit (par exemple application, module complémentaire, jeu, etc.)                                                                                        | Store et Place de marché Azure                                    |
| invoiceNumber                  | Numéro de facture (applicable à EA uniquement)                                                                                                  | Incentive et Place de marché Azure – certains programmes uniquement           |
| subscriptionId                 | Identificateur d’abonnement associé au client                                                                                         | Incentive – certains programmes uniquement                                 |
| subscriptionStartDate          | Date de début de l’abonnement                                                                                                                  | Incentive – certains programmes uniquement                                 |
| subscriptionEndDate            | Date de fin de l’abonnement                                                                                                                    | Incentive – certains programmes uniquement                                 |
| resellerId                     | Identificateur du revendeur                                                                                                                      | Incentive – certains programmes uniquement                                 |
| resellerName                   | Nom du revendeur                                                                                                                            | Incentive – certains programmes uniquement                                 |
| distributorId                  | Identificateur du serveur de distribution                                                                                                                   | Incentive – certains programmes uniquement                                 |
| distributorName                | Nom du serveur de distribution                                                                                                                         | Incentive – certains programmes uniquement                                 |
| agreementNumber                | Numéro de l’accord                                                                                                                         | Incentive – certains programmes uniquement                                 |
| agreementStartDate             | Date de début de l’accord                                                                                                                     | Incentive – certains programmes uniquement                                 |
| agreementEndDate               | Date de fin de l’accord                                                                                                                       | Incentive – certains programmes uniquement                                 |
| charge de travail                       | Charge de travail                                                                                                                                 | Incentive – certains programmes uniquement                                 |
| transactionType                | Type de transaction (achat, remboursement, contrepassation, rétrofacturation, etc.)                                                               | Store et Place de marché Azure                                    |
| localProviderSeller            | Fournisseur local/vendeur d’enregistrement                                                                                                          | Store uniquement                                                     |
| taxRemitted                    | Montant des taxes versées (ventes, consommation, ou TVA/GST).                                                                                   | Store et Place de marché Azure                                    |
| taxRemitModel                  | Tiers responsable du versement des taxes (ventes, consommation, ou TVA/GST).                                                                    | Store uniquement                                                     |
| storeFee                       | Montant retenu par Microsoft comme frais pour rendre l’application ou le module complémentaire disponible dans le Store.                                           | Store uniquement                                                     |
| transactionPaymentMethod       | Moyen de paiement client utilisé pour la transaction (par exemple carte, facturation d’opérateur de téléphonie mobile, PayPal, etc.)                                | Store et Place de marché Azure                                    |
| tpan                           | Indique le réseau publicitaire tiers                                                                                                     | Store – Annonces uniquement                                               |
| customerCountry                | Pays du client                                                                                                                         | Store et Place de marché Azure                                    |
| CustomerCity                   | Ville du client                                                                                                                            | Store et Place de marché Azure                                    |
| customerState                  | État du client                                                                                                                           | Store et Place de marché Azure                                    |
| customerZip                    | Code postal du client                                                                                                                 | Store et Place de marché Azure                                    |
| purchaseTypeCode               | Sera toujours vide                                                                                                                     | Programme d’incentives – AGI                                        |
| purchaseOrderType              | Sera toujours vide                                                                                                                     | Programme d’incentives – AGI                                        |
| purchaseOrderCoverageStartDate | Sera toujours vide                                                                                                                     | Programme d’incentives – AGI                                        |
| purchaseOrderCoverageEndDate   | Sera toujours vide                                                                                                                     | Programme d’incentives – AGI                                        |
| programOfferingLevel           |                                                                                                                                          | Programme d’incentives – AGI                                        |
| ID de locataire                       |                                                                                                                                          | Programmes d’incentives                                             |
| externalReferenceId            | Identificateur unique du programme                                                                                                        | Programmes de paiement direct (Incentive et Store)                      |
| externalReferenceIdLabel       | Étiquette de l’identificateur unique                                                                                                                  | Programmes de paiement direct (Incentive et Store)                      |

## <a name="historical-statements"></a>Instructions historiques

![Exporter les instructions historiques](images/pc-export-statements.png)

L’historique des transactions antérieures au 1er juillet 2019 est traité séparément. Les instructions utilisent les champs suivants à la place des champs actuels.

> [!NOTE]
> L’historique des transactions existant comporte une colonne appelée « Réservé » qui correspond à la colonne « Bénéfices » dans l’historique moderne, sauf qu’elle exclut tous les bénéfices dont le statut est « Paiement envoyé ».

> [!NOTE]
> Les filtres (3M, 6 M, 12M, etc.) ne s’appliquent pas à la section des **instructions historiques** .

| Nom du champ              | Description                                                                                                                                                             |
|-------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Source du chiffre d’affaires          | La source de vos revenus, en fonction de l’endroit où la transaction s’est produite (par exemple Microsoft Store, le magasin de Windows Phone, le Windows Store 8, la publicité, etc.)                  |
| ID de commande                | Identificateur de commande unique. Cet ID permet d’identifier les transactions d’achat ainsi que les opérations sans achat (par exemple : remboursements, rétrofacturations, etc.). Les deux ont le même ID de commande. En outre, en cas de paiement fractionné, où plusieurs modes de paiement sont utilisés pour un achat unique, l’ID de commande vous permettra de lier les transactions d’achat. |
| ID de transaction          | Identificateur de transaction unique.                                                                                                                                          |
| Date et heure de la transaction   | Date et heure auxquelles la transaction a eu lieu (UTC).                                                                                                                       |
| ID du produit parent       | Identificateur de produit parent unique. Notez également que si la transaction ne présente aucun produit parent, l'ID du produit parent est l'ID du produit.                                |
| Product ID              | Identificateur de produit unique.                                                                                                                                              |
| Nom du produit parent     | Nom du produit parent. Notez également que si la transaction ne présente aucun produit parent, le nom du produit parent est identique au nom du produit.                                  |
| Nom du produit            | Nom du produit.                                                                                                                                                    |
| Type de produit            | Type de produit (par exemple application, module complémentaire, jeu, etc.)                                                                                                                       |
| Quantité                | Quand la source du chiffre d’affaires est le Microsoft Store pour Entreprises, la Quantité représente le nombre de licences achetées. Pour toutes les autres sources de chiffre d’affaires, la Quantité est toujours égale à 1. Remarque : même si une transaction unique est scindée en deux articles en raison du recours à deux méthodes de paiement différentes, chaque article affiche une Quantité égale à 1. |
| Type de transaction        | Type de transaction (achat, remboursement, contrepassation, rétrofacturation, etc.)                                                                                              |
| Mode de paiement          | Moyen de paiement client utilisé pour la transaction (par exemple carte, facturation d’opérateur de téléphonie mobile, PayPal, etc.)                                                               |
| Pays/région        | Pays/région d’exécution de la transaction.                                                                                                                          |
| Fournisseur local/vendeur | Fournisseur/vendeur local de l’enregistrement.                                                                                                                                        |
| Devise de la transaction    | Devise utilisée pour la transaction.                                                                                                                                            |
| Montant de la transaction      | Montant de la transaction.                                                                                                                                              |
| Taxes versées            | Montant des taxes versées (ventes, consommation, ou TVA/GST).                                                                                                                  |
| Encaissements nets            | Montant de la transaction moins les taxes versées.                                                                                                                                   |
| Frais du Store               | Pourcentage des recettes nettes retenues par Microsoft à titre de frais de mise à disposition de l’application ou du module complémentaire dans le Windows Store.                                                      |
| Revenus des applications            | Recettes nettes moins les frais du Windows Store.                                                                                                                                       |
| Taxes retenues          | Montant de l’impôt sur le revenu retenu. (Non inclus dans le fichier. csv **réservé** .)                                                                                                |
| Paiement                 | Revenus des applications moins la retenue à la source éventuellement applicable (montant affiché dans Devise de la transaction). (Non inclus dans le fichier. csv **réservé** .)                               |
| Taux de change                 | Taux de change utilisé pour convertir la devise de la transaction en devise du paiement.                                                                                         |
| Devise de paiement        | Devise dans laquelle votre paiement a été effectué.                                                                                                                                       |
| Paiement converti       | Montant du paiement converti en devise du paiement à l’aide du taux de change.                                                                                                         |
| Modèle de paiement des taxes         | Tiers responsable du versement des taxes (ventes, consommation, ou TVA/GST).                                                                                                   |
| Date et heure d’éligibilité   | Date et heure auxquelles le produit de la transaction devient éligible au paiement (UTC). Lorsqu’un paiement est créé, il inclut les revenus de transactions dont la date et l’heure d’admissibilité sont antérieures à la date de création du paiement. (Inclus uniquement dans le fichier .csv **Réservé**) |
| Charges                 | Ventilation détaillée de tous les frais agrégés dans la colonne Montant de transaction. (Uniquement pour Microsoft Azure Marketplace ; non inclus dans le fichier .csv **Réservé**) |