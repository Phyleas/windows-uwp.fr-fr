---
ms.assetid: e04ebe3f-479c-4b48-99d8-3dd4bb9bfaf4
title: Provisionner le portail d’appareil avec un certificat SSL personnalisé
description: Découvrez comment provisionner le Portail d’appareil Windows avec un certificat personnalisé à utiliser dans les communications HTTPS.
ms.date: 04/19/2019
ms.topic: article
keywords: windows 10, uwp, portail d’appareil
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: ce4e45bc23f4efb636618bb4891b9d6e9d207490
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/29/2020
ms.locfileid: "63774144"
---
# <a name="provision-device-portal-with-a-custom-ssl-certificate"></a>Provisionner le portail d’appareil avec un certificat SSL personnalisé

Dans la mise à jour Windows 10 Creators Update, le Portail d’appareil Windows a ajouté la possibilité pour les administrateurs d’appareils d’installer un certificat personnalisé à utiliser dans les communications HTTPS.

Même si vous pouvez faire cela sur votre propre PC, cette fonctionnalité est principalement destinée aux entreprises qui ont déjà mis en place une infrastructure de certificats.  

Par exemple, une entreprise peut utiliser une autorité de certification (AC) pour signer des certificats pour les sites web intranet utilisant HTTPS. Cette fonctionnalité s’ajoute à cette infrastructure.

## <a name="overview"></a>Vue d’ensemble

Par défaut, le Portail d’appareil génère une autorité de certification racine auto-signée, puis l’utilise pour signer des certificats SSL pour chaque point de terminaison détecté. Cela inclut `localhost`, `127.0.0.1` et `::1` (localhost IPv6).

Le nom d’hôte de l’appareil (par exemple, `https://LivingRoomPC`) et chaque adresse IP de liaison locale attribuée à l’appareil sont également inclus (jusqu’à deux [IPv4, IPv6] par carte réseau).
Les adresses IP de liaison locale d’un appareil sont visibles dans l’outil Mise en réseau du Portail d’appareil. Elles commencent par `10.` ou `192.` pour IPv4 ou par `fe80:` pour IPv6.

Dans la configuration par défaut, un avertissement de certificat peut s’afficher dans votre navigateur à cause de l’AC racine, qui n’est pas approuvée. Plus précisément, le certificat SSL fourni par le Portail d’appareil est signé par une AC racine que le navigateur ou le PC n’approuve pas. Cela peut être résolu en créant une AC racine approuvée.

## <a name="create-a-root-ca"></a>Créer une AC racine

Cette opération ne doit être effectuée que si votre entreprise (ou domicile) n’a pas d’infrastructure de certificats configurée, et elle doit l’être qu’une seule fois. Le script PowerShell suivant crée une AC racine appelée _WdpTestCA.cer_. Si vous installez ce fichier sur les autorités de certification racine de confiance de l’ordinateur local, votre appareil approuvera les certificats SSL signés par cette AC racine. Vous pouvez (et devez) installer ce fichier .cer sur chaque PC que vous souhaitez connecter au Portail d’appareil Windows.  

```PowerShell
$CN = "PickAName"
$OutputPath = "c:\temp\"

# Create root certificate authority
$FilePath = $OutputPath + "WdpTestCA.cer"
$Subject =  "CN="+$CN
$rootCA = New-SelfSignedCertificate -certstorelocation cert:\currentuser\my -Subject $Subject -HashAlgorithm "SHA512" -KeyUsage CertSign,CRLSign
$rootCAFile = Export-Certificate -Cert $rootCA -FilePath $FilePath
```

Une fois l’AC racine créée, vous pouvez utiliser le fichier _WdpTestCA.cer_ pour signer des certificats SSL.

## <a name="create-an-ssl-certificate-with-the-root-ca"></a>Créer un certificat SSL avec l’AC racine

Les certificats SSL ont deux fonctions essentielles : sécuriser votre connexion via le chiffrement et vérifier que vous communiquez vraiment avec l’adresse affichée dans la barre du navigateur (Bing.com, 192.168.1.37, etc.) et non avec un tiers malveillant.

Le script PowerShell suivant crée un certificat SSL pour le point de terminaison `localhost`. Chaque point de terminaison détecté par le Portail d’appareil a besoin de son propre certificat. Vous pouvez remplacer l’argument `$IssuedTo` dans le script par chacun des différents points de terminaison de votre appareil : le nom d’hôte, localhost et les adresses IP.

```PowerShell
$IssuedTo = "localhost"
$Password = "PickAPassword"
$OutputPath = "c:\temp\"
$rootCA = Import-Certificate -FilePath C:\temp\WdpTestCA.cer -CertStoreLocation Cert:\CurrentUser\My\

# Create SSL cert signed by certificate authority
$IssuedToClean = $IssuedTo.Replace(":", "-").Replace(" ", "_")
$FilePath = $OutputPath + $IssuedToClean + ".pfx"
$Subject = "CN="+$IssuedTo
$cert = New-SelfSignedCertificate -certstorelocation cert:\localmachine\my -Subject $Subject -DnsName $IssuedTo -Signer $rootCA -HashAlgorithm "SHA512"
$certFile = Export-PfxCertificate -cert $cert -FilePath $FilePath -Password (ConvertTo-SecureString -String $Password -Force -AsPlainText)
```

Si vous avez plusieurs appareils, vous pouvez réutiliser les fichiers .pfx de localhost, mais vous devez toujours créer des certificats d’adresse IP et de nom d’hôte pour chaque appareil séparément.

Une fois le groupe de fichiers .pfx généré, vous devez charger ces fichiers dans le Portail d’appareil Windows.

## <a name="provision-device-portal-with-the-certifications"></a>Provisionner le Portail d’appareil avec la ou les certifications

Pour chaque fichier .pfx créé pour un appareil, vous devez exécuter la commande suivante à partir d’une invite de commandes avec élévation de privilèges.

```cmd
WebManagement.exe -SetCert <Path to .pfx file> <password for pfx>
```

Voici un exemple d’utilisation :

```cmd
WebManagement.exe -SetCert localhost.pfx PickAPassword
WebManagement.exe -SetCert --1.pfx PickAPassword
WebManagement.exe -SetCert MyLivingRoomPC.pfx PickAPassword
```

Une fois que vous avez installé les certificats, il vous suffit de redémarrer le service pour que les modifications prennent effet :

```cmd
sc stop webmanagement
sc start webmanagement
```

> [!TIP]
> Les adresses IP peuvent changer au fil du temps.
De nombreux réseaux utilisent DHCP pour distribuer les adresses IP, si bien que les appareils n’obtiennent pas toujours la même adresse IP que celles qu’ils avaient auparavant. Si vous avez créé un certificat pour une adresse IP sur un appareil et que l’adresse de cet appareil change, le Portail d’appareil Windows génère un nouveau certificat en utilisant le certificat auto-signé existant et cesse d’utiliser celui que vous avez créé. Cela fait réapparaître la page d’avertissement de certificat dans votre navigateur. Pour cette raison, nous vous recommandons de vous connecter à vos appareils via leur nom d’hôte, que vous pouvez définir sur le Portail d’appareil. Le nom d’hôte reste inchangé quelle que soit l’adresse IP.
