---
ms.assetid: E322DFFE-8EEC-499D-87BC-EDA5CFC27551
description: Chaque transaction Microsoft Store qui produit un achat de produit réussi peut éventuellement retourner un accusé de réception de transaction.
title: Utiliser des reçus pour vérifier les achats de produits
ms.date: 04/16/2018
ms.topic: article
keywords: Windows 10, UWP, achats dans l’application, IAPs, reçus, Windows. ApplicationModel. Store
ms.localizationpriority: medium
ms.openlocfilehash: ba87de0755469f373f9000f3d96d3021c9197985
ms.sourcegitcommit: 28bd367ab8acc64d4b6f3f73adca12100cbd359f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/24/2020
ms.locfileid: "82148885"
---
# <a name="use-receipts-to-verify-product-purchases"></a>Utiliser des reçus pour vérifier les achats de produits

Chaque transaction Microsoft Store qui produit un achat de produit réussi peut éventuellement retourner un accusé de réception de transaction. Ce reçu fournit des informations sur le produit et le coût monétaire pour le client.

Le fait d’avoir accès à ces informations prend en charge des scénarios dans lesquels votre application doit vérifier qu’un utilisateur a acheté votre application ou a effectué des achats (également appelés produits dans l’application ou IAP) à partir de la Microsoft Store. Par exemple, imaginez un jeu qui propose du contenu téléchargé. Si l’utilisateur qui a acheté le contenu du jeu veut jouer à ce jeu sur un autre appareil, vous devez vérifier qu’il a bien acheté le contenu. Voici comment faire.

> [!IMPORTANT]
> Cet article explique comment utiliser les membres de l’espace de noms [Windows. ApplicationModel. Store](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store) pour obtenir et valider un accusé de réception pour un achat dans une application. Si vous utilisez l’espace de noms [Windows. services. Store](https://docs.microsoft.com/uwp/api/Windows.Services.Store) pour les achats dans l’application (introduits dans Windows 10, version 1607 et disponibles pour les projets qui ciblent l' **édition anniversaire windows 10 (10,0 ; Build 14393)** ou une version ultérieure dans Visual Studio), cet espace de noms ne fournit pas d’API pour obtenir des reçus d’achat pour les achats dans l’application. Toutefois, vous pouvez utiliser une méthode REST dans l’API de collection Microsoft Store pour obtenir des données pour une transaction d’achat. Pour plus d’informations, consultez [Reçus d’achats in-app](in-app-purchases-and-trials.md#receipts).

## <a name="requesting-a-receipt"></a>Demande d’un reçu


L’espace de noms **Windows.ApplicationModel.Store** prend en charge plusieurs modes pour obtenir un reçu :

* Lorsque vous effectuez un achat à l’aide de [CurrentApp.RequestAppPurchaseAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentapp.requestapppurchaseasync) ou [CurrentApp.RequestProductPurchaseAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentapp.requestproductpurchaseasync) (ou l’une des autres surcharges de cette méthode), la valeur de retour contient le reçu.
* Vous pouvez appeler la méthode [CurrentApp.GetAppReceiptAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentapp.getappreceiptasync) pour récupérer les informations du reçu de votre application et des modules complémentaires de votre application.

Un reçu d’application ressemble à ceci.

> [!NOTE]
> Cet exemple est mis en forme pour faciliter la lecture du XML. Les véritables accusés de réception d’application n’incluent pas d’espace entre les éléments.

> [!div class="tabbedCodeSnippets"]
```xml
<Receipt Version="1.0" ReceiptDate="2012-08-30T23:10:05Z" CertificateId="b809e47cd0110a4db043b3f73e83acd917fe1336" ReceiptDeviceId="4e362949-acc3-fe3a-e71b-89893eb4f528">
    <AppReceipt Id="8ffa256d-eca8-712a-7cf8-cbf5522df24b" AppId="55428GreenlakeApps.CurrentAppSimulatorEventTest_z7q3q7z11crfr" PurchaseDate="2012-06-04T23:07:24Z" LicenseType="Full" />
    <ProductReceipt Id="6bbf4366-6fb2-8be8-7947-92fd5f683530" ProductId="Product1" PurchaseDate="2012-08-30T23:08:52Z" ExpirationDate="2012-09-02T23:08:49Z" ProductType="Durable" AppId="55428GreenlakeApps.CurrentAppSimulatorEventTest_z7q3q7z11crfr" />
    <Signature xmlns="http://www.w3.org/2000/09/xmldsig#">
        <SignedInfo>
            <CanonicalizationMethod Algorithm="http://www.w3.org/2001/10/xml-exc-c14n#" />
            <SignatureMethod Algorithm="http://www.w3.org/2001/04/xmldsig-more#rsa-sha256" />
            <Reference URI="">
                <Transforms>
                    <Transform Algorithm="http://www.w3.org/2000/09/xmldsig#enveloped-signature" />
                </Transforms>
                <DigestMethod Algorithm="http://www.w3.org/2001/04/xmlenc#sha256" />
                <DigestValue>cdiU06eD8X/w1aGCHeaGCG9w/kWZ8I099rw4mmPpvdU=</DigestValue>
            </Reference>
        </SignedInfo>
        <SignatureValue>SjRIxS/2r2P6ZdgaR9bwUSa6ZItYYFpKLJZrnAa3zkMylbiWjh9oZGGng2p6/gtBHC2dSTZlLbqnysJjl7mQp/A3wKaIkzjyRXv3kxoVaSV0pkqiPt04cIfFTP0JZkE5QD/vYxiWjeyGp1dThEM2RV811sRWvmEs/hHhVxb32e8xCLtpALYx3a9lW51zRJJN0eNdPAvNoiCJlnogAoTToUQLHs72I1dECnSbeNPXiG7klpy5boKKMCZfnVXXkneWvVFtAA1h2sB7ll40LEHO4oYN6VzD+uKd76QOgGmsu9iGVyRvvmMtahvtL1/pxoxsTRedhKq6zrzCfT8qfh3C1w==</SignatureValue>
    </Signature>
</Receipt>
```

Un reçu de produit ressemble à ceci.

> [!NOTE]
> Cet exemple est mis en forme pour faciliter la lecture du XML. Les accusés de réception de produits réels n’incluent pas d’espace entre les éléments.

> [!div class="tabbedCodeSnippets"]
```xml
<Receipt Version="1.0" ReceiptDate="2012-08-30T23:08:52Z" CertificateId="b809e47cd0110a4db043b3f73e83acd917fe1336" ReceiptDeviceId="4e362949-acc3-fe3a-e71b-89893eb4f528">
    <ProductReceipt Id="6bbf4366-6fb2-8be8-7947-92fd5f683530" ProductId="Product1" PurchaseDate="2012-08-30T23:08:52Z" ExpirationDate="2012-09-02T23:08:49Z" ProductType="Durable" AppId="55428GreenlakeApps.CurrentAppSimulatorEventTest_z7q3q7z11crfr" />
    <Signature xmlns="http://www.w3.org/2000/09/xmldsig#">
        <SignedInfo>
            <CanonicalizationMethod Algorithm="http://www.w3.org/2001/10/xml-exc-c14n#" />
            <SignatureMethod Algorithm="http://www.w3.org/2001/04/xmldsig-more#rsa-sha256" />
            <Reference URI="">
                <Transforms>
                    <Transform Algorithm="http://www.w3.org/2000/09/xmldsig#enveloped-signature" />
                </Transforms>
                <DigestMethod Algorithm="http://www.w3.org/2001/04/xmlenc#sha256" />
                <DigestValue>Uvi8jkTYd3HtpMmAMpOm94fLeqmcQ2KCrV1XmSuY1xI=</DigestValue>
            </Reference>
        </SignedInfo>
        <SignatureValue>TT5fDET1X9nBk9/yKEJAjVASKjall3gw8u9N5Uizx4/Le9RtJtv+E9XSMjrOXK/TDicidIPLBjTbcZylYZdGPkMvAIc3/1mdLMZYJc+EXG9IsE9L74LmJ0OqGH5WjGK/UexAXxVBWDtBbDI2JLOaBevYsyy+4hLOcTXDSUA4tXwPa2Bi+BRoUTdYE2mFW7ytOJNEs3jTiHrCK6JRvTyU9lGkNDMNx9loIr+mRks+BSf70KxPtE9XCpCvXyWa/Q1JaIyZI7llCH45Dn4SKFn6L/JBw8G8xSTrZ3sBYBKOnUDbSCfc8ucQX97EyivSPURvTyImmjpsXDm2LBaEgAMADg==</SignatureValue>
    </Signature>
</Receipt>
```

Vous pouvez utiliser ces exemples de reçu pour tester votre code de validation. Pour plus d’informations sur le contenu du reçu, consultez la [description des éléments et des attributs](#receipt-descriptions).

## <a name="validating-a-receipt"></a>Validation d’un reçu

Pour valider l’authenticité d’un reçu, vous avez besoin de votre système dorsal (service web ou autre) afin d’en vérifier la signature à l’aide du certificat public. Pour accéder à ce certificat, utilisez l' ```https://lic.apps.microsoft.com/licensing/certificateserver/?cid=CertificateId%60%60%60, where ```URL CertificateId «» est la valeur **CertificateId** dans la réception.

Voici un exemple de ce processus de validation. Ce code s’exécute dans une application de console .NET Framework, qui inclut une référence à l’assemblage **System.Security**.

> [!div class="tabbedCodeSnippets"]
[!code-csharp[ReceiptVerificationSample](./code/ReceiptVerificationSample/cs/Program.cs#ReceiptVerificationSample)]

<span id="receipt-descriptions" />

## <a name="element-and-attribute-descriptions-for-a-receipt"></a>Description des éléments et des attributs d’un reçu

Cette section décrit les éléments et attributs d’un reçu.

### <a name="receipt-element"></a>Élément d’un reçu

L’élément racine de ce fichier est l’élément **Receipt**, qui contient des informations sur l’application et les achats in-app. Cet élément contient les éléments enfants suivants :

|  Élément  |  Obligatoire  |  Quantité  |  Description   |
|-------------|------------|--------|--------|
|  [AppReceipt](#appreceipt)  |    Non         |  0 ou 1  |  Contient des informations sur l’achat pour l’application actuelle.            |
|  [ProductReceipt](#productreceipt)  |     Non        |  0 ou plus    |   Contient des informations sur un achat in-app pour l’application actuelle.     |
|  Signature  |      Oui      |  1   |   Cet élément est une construction [XML-DSIG](https://www.w3.org/TR/xmldsig-core/) standard. Il contient un élément **SignatureValue** qui contient la signature que vous pouvez utiliser pour valider le reçu, un élément **SignedInfo**.      |

L’élément **Receipt** a les attributs suivants :

|  Attribut  |  Description   |
|-------------|-------------------|
|  **Version**  |    Numéro de version du reçu.            |
|  **CertificateId**  |     Empreinte de certificat utilisée pour signer le reçu.          |
|  **ReceiptDate**  |    Date de signature et de téléchargement du reçu.           |  
|  **ReceiptDeviceId**  |   Identifie l’appareil utilisé pour demander ce reçu.         |  |

<span id="appreceipt" />

### <a name="appreceipt-element"></a>Élément AppReceipt

Cet élément contient des informations sur l’achat pour l’application actuelle.

L’élément **AppReceipt** a les attributs suivants :

|  Attribut  |  Description   |
|-------------|-------------------|
|  **Id**  |    Identifie l’achat.           |
|  **AppId**  |     Nom de la famille de packages, utilisé par le système d’exploitation pour l’application.           |
|  **LicenseType**  |    **Full**, si l’utilisateur a acheté la version complète de l’application. **Trial**, si l’utilisateur a téléchargé une version d’évaluation de l’application.           |  
|  **PurchaseDate**  |    Date d’acquisition de l’application.          |  |

<span id="productreceipt" />

### <a name="productreceipt-element"></a>Élément ProductReceipt

Cet élément contient des informations sur un achat in-app pour l’application actuelle.

L’élément **ProductReceipt** a les attributs suivants :

|  Attribut  |  Description   |
|-------------|-------------------|
|  **Id**  |    Identifie l’achat.           |
|  **AppId**  |     Identifie l’application avec laquelle l’utilisateur a effectué l’achat.           |
|  **Réf**  |     Identifie le produit acheté.           |
|  **ProductType**  |    Détermine le type de produit. Actuellement, ne prend en charge que la valeur **Durable**.          |  
|  **PurchaseDate**  |    Date à laquelle l’achat a eu lieu.          |  |

 

 
