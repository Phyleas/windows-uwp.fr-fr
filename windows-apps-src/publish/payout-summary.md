---
Description: La page Résumé du paiement affiche le détail des sommes rapportées par vos applications et modules complémentaires. Elle vous permet également de connaître les délais et les montants de vos paiements.
title: Résumé du paiement
ms.assetid: F0D070BE-8267-4CC9-B0D2-085EBA74AC98
ms.date: 08/02/2019
ms.topic: article
keywords: windows 10, uwp, résumé du paiement, instruction, paiements, bénéfices, revenus, paiement
ms.localizationpriority: medium
ms.openlocfilehash: aff36ace40317ff0b2ff54a8ca75381fe2c24a4e
ms.sourcegitcommit: 26bb75084b9d2d2b4a76d4aa131066e8da716679
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/06/2020
ms.locfileid: "75684990"
---
# <a name="payout-summary"></a>Résumé du paiement

La page **Récapitulatif des paiements** montre les détails de l’argent que vous avez gagné avec Microsoft. Elle vous permet également de connaître les délais et les montants de vos paiements.

Si vous vendez des produits dans Place de marché Microsoft Azure, la page **Résumé du paiement** vous présente également des informations sur les paiements qui vous ont été versés. Pour plus d’informations sur le processus de paiement d’Azure Marketplace, voir [Politiques concernant la participation à Microsoft Azure Marketplace](https://docs.microsoft.com/legal/marketplace/participation-policy) et [Microsoft Azure Marketplace Publisher Agreement (Contrat d’éditeur Microsoft Azure Marketplace)](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RE3ypvt).

> [!NOTE]
> Pour être éligible au paiement, votre produit doit atteindre le [seuil de paiement](payment-thresholds-methods-and-timeframes.md) de 50 $. Pour plus d’informations sur le seuil de paiement, consultez cette page et passez en revue le contrat de développeur d’applications.

## <a name="access-the-payout-summary-pages"></a>Accéder aux pages de résumé de paiement

Pour ouvrir l’une des pages de résumé de paiement :

1. Sélectionnez l’icône Money dans le coin supérieur droit.
2. Sélectionnez paiements, historique des transactions ou exporter des données.

## <a name="payments-page"></a>Page paiements

Les totaux de cette page représentent tous les programmes auxquels vous participez. Vous pouvez filtrer par ID participant, programme, ID paiement et type acquis. Les montants sont indiqués en dollars américains. La valeur payante est également affichée dans payer à la devise.

| Zone                   | Description                                                                                  |
|------------------------|----------------------------------------------------------------------------------------------|
| Total payé cette année   | Le total cumulé payé pour vous cette année, en dollars américains, pour tous vos programmes.       |
| Prochain paiement estimé | Seul le paiement suivant vous vient (même s’il y en a d’autres bientôt disponibles), en dollars américains. |
| Dernier paiement           | Le montant (en dollars américains), le nom du programme et le programme de votre paiement le plus récent.           |
| Paiements par source     | Montant des paiements, en dollars américains, représentés par le programme au cours des 12 derniers mois.           |
| Paiements               | Sélectionnez payant ou en attente, puis triez comme vous le souhaitez. Pour plus d’informations sur un paiement spécifique, sélectionnez affichage. Pour télécharger une copie de l’instruction de remise de paiement, sélectionnez Télécharger. Notez que les données de l’historique des transactions peuvent prendre jusqu’à 24 heures pour apparaître. vous risquez donc de ne pas voir les bénéfices associés immédiatement. |

Pour exporter les données de cette page, sélectionnez Exporter, puis suivez les instructions de la page exporter des données.

## <a name="transaction-history-page"></a>Page historique des transactions

Cette page affiche tous vos revenus individuels, y compris la date, le type et le résultat pour chacun d’entre eux. Vous pouvez sélectionner une période à afficher, et vous pouvez également filtrer par ID d’inscription, programme, ID de paiement, type en cours d’exécution, levier et état. Les données sont disponibles pour l’exercice fiscal actuel (du 1er juillet au 30 juin) et dans les deux années fiscales précédentes.

Pour obtenir plus de détails sur un gain, sélectionnez la flèche vers le bas à droite de la page. Le levier, le chiffre d’affaires et le produit s’affichent. Si, pour une raison quelconque, ces données ne sont pas disponibles, mais que vous avez besoin d’y accéder, contactez le [support technique](https://developer.microsoft.com/windows/support). Si le résultat est le résultat d’un ajustement et non d’une transaction, les champs de produit ne sont pas affichés.

Pour exporter les données de transaction sur cette page, sélectionnez Exporter, puis suivez les instructions de la page exporter des données. Les fichiers exportés à partir de la page historique des transactions affichent les données dans la devise de la transaction, les bénéfices dans la devise de la transaction et le dollar des États-Unis, et la valeur payée dans paiement à la devise

## <a name="payment-status"></a>Statut du paiement

| État de l’obtention           | Raison                                                                                                                                      | Action de partenaire requise ?                                   |
|--------------------------|---------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------|
| Non traité              | Le revenu est éligible au paiement. Il reste dans cet état pendant une période de réflexion telle que définie dans le guide du programme d’incentives. | non                                                         |
| Prochainement                 | Commande de paiement générée en attente de révisions internes avant le traitement du paiement.                                                               | non                                                         |
| Facture d’impôt en attente      | Votre facture fiscale est incomplète ou non valide.                                                                                                  | Vous devez mettre à jour votre facture fiscale avant de pouvoir payer |
| Rejeté pendant la révision   | Le paiement a été rejeté pendant la révision.                                                                                                     | Contacter le [support Microsoft](https://developer.microsoft.com/windows/support) pour plus d’informations                      |
| Failed                   | Le paiement a échoué en raison d’une erreur système Microsoft.                                                                                         | Contacter le [support Microsoft](https://developer.microsoft.com/windows/support) pour plus d’informations                      |
| En cours              | Le paiement est en cours.                                                                                                                 | non                                                         |
| Paiement incorrect        | Le remboursement est en cours.                                                                                                       | non                                                         |
| Envoyé                     | Le paiement a été envoyé à votre banque.                                                                                                     | non                                                         |
| Retraitement             | Le paiement a rencontré une erreur système Microsoft et est en cours de retraitement.                                                                  | non                                                         |
| Inversé                 | Le paiement a été inversé par votre banque et sera renvoyé dans le prochain cycle de paiement.                                                     | non                                                         |
| Facture de taxe rejetée     | Votre facture fiscale a été rejetée pendant la révision. Tous les paiements en attente seront en attente jusqu’à ce que la révision de la facture fiscale soit terminée.                 | Contacter le [support Microsoft](https://developer.microsoft.com/windows/support) pour plus d’informations                      |
| Facture fiscale en cours de révision | Votre relevé de taxes est en cours de révision. Votre paiement est lancé une fois que la facture d’impôt a été approuvée.                                   | non                                                         |
| Rejetée                 | Le paiement a été rejeté par votre banque.                                                                                                      | Pour plus d’informations, contactez votre banque.                             |

## <a name="export-data-page"></a>Page exporter des données

Suivez les instructions de cette page pour exporter les données souhaitées.

Remarques :

- La page exporter des données n’est pas actualisée automatiquement. Vous devrez peut-être actualiser la page manuellement pour afficher les données les plus récentes.
- Votre filtre peut entraîner une erreur aucune donnée n’est disponible. Cela signifie probablement que vous avez laissé la période de temps par défaut sélectionnée à trois mois, puis que vous avez sélectionné un ID de paiement d’un gain en dehors de cette période. Développez votre période, puis réessayez.

## <a name="payment-download-export"></a>Exportation du téléchargement de paiement

Cette option permet de télécharger les paiements que vous avez reçus dans votre banque pour un programme donné, la taxe associée et le montant agrégé. Ce rapport est utilisé pour de nombreux programmes de l’espace partenaires. par conséquent, certaines colonnes peuvent être inapplicables à votre rapport. Ces colonnes sont indiquées ci-dessous.

| Nom de la colonne              | Description                                                                                                                               |
|--------------------------|-----------------------------------------------------------------------------------------------------------------------------------------  |
| participantID            | Identité principale du revenu partenaire dans le cadre du programme                                                                             |
| participantIDType        | En général, ID de programme pour les programmes de motivation et ID de vendeur pour les programmes de boutique                                                                |
| participantName          | Nom du partenaire de revenu                                                                                                               |
| programName              | Nom du programme d’incentives/du Store                                                                                                              |
| tiré                   | Montant gagné dans le paiement à la devise pour ce programme/participantID                                                                       |
| earnedUSD                | Montant gagné pour le programme/ID participant, en USD                                                                                      |
| withheldTax              | Montant des taxes retenues dans le paiement à la devise pour le programme/participantID                                                               |
| salesTax                 | Montant total des taxes dans le paiement à la devise pour le programme/participantID (applicable aux programmes d’incentives uniquement)                   |
| serviceFeeTax            | Montant total de serviceFeeTax dans payer à la devise pour le programme/participantID (applicable aux programmes de stockage et à la place de marché Azure uniquement) |
| totalPayment             | Paiement total en devise locale, à l’exclusion de la taxe à retenir et des taxes de vente (le cas échéant) pour le programme/participantID   |
| currencyCode             | Paiement à code devise                                                                                                                      |
| paymentMethod            | Méthode utilisée pour payer le partenaire, par exemple, transfert bancaire électronique, note de crédit                                                             |
| paymentID                | Identificateur unique du paiement. Ce nombre est généralement visible dans votre relevé bancaire. (applicable uniquement aux paiements SAP)              |
| paymentStatus            | Statut du paiement                                                                                                                            |
| paymentStatusDescription | Description conviviale de l’état du paiement                                                                                                    |
| paymentDate              | Date à laquelle le paiement a été envoyé par Microsoft                                                                                                      |

## <a name="transaction-history-download-export"></a>Exportation du téléchargement de l’historique des transactions

Cette option fournit un téléchargement de chaque élément de ligne en cours d’obtention que vous voyez dans la page historique des transactions, le type en cours, la date, le montant de la transaction associée, le client, le produit et d’autres détails transactionnels applicables à vos programmes.

| Nom de la colonne                    | Description                                                                                                                              | Applicabilité pour les incentives/le magasin/la place de marché Azure           |
|--------------------------------|------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------|
| earningId                      | Identificateur unique de chaque revenu                                                                                                       | Toutes                                                            |
| participantId                  | Identité principale du revenu partenaire dans le cadre du programme                                                                            | Toutes                                                            |
| participantIdType              | Principalement, ID des programmes d’incentives et ID de vendeur pour les programmes du Store et la Place de marché Azure                                          | Toutes                                                            |
| participantName                | Nom du partenaire de revenu                                                                                                              | Toutes                                                            |
| partnerCountryCode             | Emplacement/pays du partenaire de revenu                                                                                                  | Toutes                                                            |
| programName                    | Nom du programme d’incentives/du Store                                                                                                             | Toutes                                                            |
| transactionId                  | Identificateur unique de la transaction                                                                                                    | Toutes                                                            |
| transactionCurrency            | Devise dans laquelle la transaction client d’origine s’est produite (il ne s’agit pas de la devise de l’emplacement partenaire)                                     | Toutes                                                            |
| transactionDate                | Date de la transaction Utile pour les programmes où de nombreuses transactions contribuent à un revenu                                           | Toutes                                                            |
| transactionExchangeRate        | Date du taux de change utilisée pour afficher la transaction correspondante en euros                                                                 | Toutes                                                            |
| transactionAmount              | Montant de la transaction dans la devise de transaction d’origine en fonction du revenu généré                                              | Toutes                                                            |
| transactionAmountUSD           | Montant de la transaction en USD                                                                                                                | Toutes                                                            |
| lever                          | Indique une règle métier pour le revenu                                                                                                  | Toutes                                                            |
| earningRate                    | Taux d’incentives appliqué au montant de la transaction pour générer un revenu                                                                      | Toutes                                                            |
| quantity                       | Varie selon le programme. Indique la quantité facturée pour les programmes transactionnels                                                            | Toutes                                                            |
| quantityType                   | Indique le type de quantité par exemple, quantité facturée, MAU                                                                                     | Toutes                                                            |
| earningType                    | Indique s’il s’agit de frais, de remise, de coop, de vente, etc.                                                                                          | Toutes                                                            |
| earningAmount                  | Montant du revenu dans la devise de la transaction d’origine                                                                                      | Toutes                                                            |
| earningAmountUSD               | Montant du revenu en USD                                                                                                                    | Toutes                                                            |
| earningDate                    | Date du revenu                                                                                                                      | Toutes                                                            |
| calculationDate                | Date à laquelle le revenu a été calculé dans le système                                                                                            | Toutes                                                            |
| earningExchangeRate            | Taux de change utilisé pour afficher le montant USD correspondant                                                                                  | Toutes                                                            |
| exchangeRateDate               | Date du taux de change utilisée pour calculer le montant USD du revenu                                                                                   | Toutes                                                            |
| paymentAmountWOTax             | Montant du revenu (hors taxe) dans la devise de destination pour les paiements « envoyés » uniquement                                                                 | Toutes                                                            |
| paymentCurrency                | Devise de destination choisie par le partenaire dans le profil de paiement. Affichée uniquement pour les paiements envoyés                                                   | Toutes                                                            |
| paymentExchangeRate            | Taux de change utilisé pour calculer paymentAmountWOTax dans la devise de paiement avec ExchangeRateDate                                            | Toutes                                                            |
| claimId                        | Identificateur unique de la revendication                                                                                                              | Incentives-certains programmes uniquement                                |
| planId                         | Identificateur unique pour le plan                                                                                                               | Incentives-certains programmes uniquement                                |
| paymentId                      | Identificateur unique du paiement. Ce nombre est généralement visible dans votre relevé bancaire                                                 | Paiements SAP uniquement                                              |
| paymentStatus                  | Statut du paiement                                                                                                                           | Toutes                                                            |
| paymentStatusDescription       | Description conviviale de l’état du paiement                                                                                                   | Toutes                                                            |
| customerId                     | Sera toujours vide                                                                                                                     | Programmes d’incentives uniquement (exception : OEM) et place de marché Azure |
| CustomerName                   | Sera toujours vide                                                                                                                     | Programmes d’incentives uniquement (exception : OEM) et place de marché Azure |
| partNumber                     | Sera toujours vide                                                                                                                     | Certains programmes d’incentives et de boutiques et la place de marché Azure        |
| productName                    | Nom de produit lié à la transaction                                                                                                       | Toutes                                                            |
| productId                      | Identificateur de produit unique                                                                                                                | Store et place de marché Azure                                    |
| parentProductId                | Identificateur unique du produit parent. Notez également que si la transaction ne présente aucun produit parent, l'ID du produit parent est l'ID du produit. | Store et place de marché Azure                                    |
| parentProductName              | Nom du produit parent. Notez également que si la transaction ne présente aucun produit parent, le nom du produit parent est identique au nom du produit.   | Store et place de marché Azure                                    |
| productType                    | Type du produit (par exemple, Application, Module complémentaire, Jeu, etc.)                                                                                        | Store et place de marché Azure                                    |
| invoiceNumber                  | Numéro de facture (applicable à EA uniquement)                                                                                                  | Incentive et place de marché Azure-certains programmes uniquement           |
| subscriptionId                 | Identificateur d’abonnement associé au client                                                                                         | Incentives-certains programmes uniquement                                 |
| subscriptionStartDate          | Date de début d’abonnement                                                                                                                  | Incentives-certains programmes uniquement                                 |
| subscriptionEndDate            | Date de fin d’abonnement                                                                                                                    | Incentives-certains programmes uniquement                                 |
| resellerId                     | Identificateur du revendeur                                                                                                                      | Incentives-certains programmes uniquement                                 |
| resellerName                   | Nom du revendeur                                                                                                                            | Incentives-certains programmes uniquement                                 |
| distributorId                  | Identificateur du serveur de distribution                                                                                                                   | Incentives-certains programmes uniquement                                 |
| distributorName                | Nom du serveur de distribution                                                                                                                         | Incentives-certains programmes uniquement                                 |
| agreementNumber                | Numéro de contrat                                                                                                                         | Incentives-certains programmes uniquement                                 |
| agreementStartDate             | Date de début de contrat                                                                                                                     | Incentives-certains programmes uniquement                                 |
| agreementEndDate               | Date de fin de contrat                                                                                                                       | Incentives-certains programmes uniquement                                 |
| charge de travail                       | Charge de travail                                                                                                                                 | Incentives-certains programmes uniquement                                 |
| transactionType                | Type de la transaction (par exemple, achat, remboursement, contrepassation, rétrofacturation, etc.)                                                               | Store et place de marché Azure                                    |
| localProviderSeller            | Fournisseur local/vendeur d’enregistrement                                                                                                          | Stockage uniquement                                                     |
| taxRemitted                    | Montant des taxes versées (taxe de vente, taxe d’utilisation ou TVA/taxe sur les biens et services).                                                                                   | Store et place de marché Azure                                    |
| taxRemitModel                  | Tiers responsable du versement des taxes (taxe de vente, taxe d’utilisation ou TVA/taxe sur les biens et services).                                                                    | Stockage uniquement                                                     |
| storeFee                       | Montant retenu par Microsoft comme frais pour rendre l’application ou le module complémentaire disponible dans le Store.                                           | Stockage uniquement                                                     |
| transactionPaymentMethod       | Instrument de paiement client utilisé pour la transaction (par exemple, carte, facturation de l’opérateur mobile, PayPal, etc.)                                | Store et place de marché Azure                                    |
| tpan                           | Indique le réseau publicitaire tiers                                                                                                     | Store-annonces uniquement                                               |
| customerCountry                | Pays du client                                                                                                                         | Store et place de marché Azure                                    |
| CustomerCity                   | Ville du client                                                                                                                            | Store et place de marché Azure                                    |
| customerState                  | État du client                                                                                                                           | Store et place de marché Azure                                    |
| customerZip                    | Code postal du client                                                                                                                 | Store et place de marché Azure                                    |
| purchaseTypeCode               | Sera toujours vide                                                                                                                     | Programme d’incentives-élément de rapport personnalisé                                        |
| purchaseOrderType              | Sera toujours vide                                                                                                                     | Programme d’incentives-élément de rapport personnalisé                                        |
| purchaseOrderCoverageStartDate | Sera toujours vide                                                                                                                     | Programme d’incentives-élément de rapport personnalisé                                        |
| purchaseOrderCoverageEndDate   | Sera toujours vide                                                                                                                     | Programme d’incentives-élément de rapport personnalisé                                        |
| programOfferingLevel           |                                                                                                                                          | Programme d’incentives-élément de rapport personnalisé                                        |
| ID de locataire                       |                                                                                                                                          | Programmes d’incentives                                             |
| externalReferenceId            | Identificateur unique du programme                                                                                                        | Programmes de paiement direct (incentive et boutique)                      |
| externalReferenceIdLabel       | Étiquette de l’identificateur unique                                                                                                                  | Programmes de paiement direct (incentive et boutique)                      |
