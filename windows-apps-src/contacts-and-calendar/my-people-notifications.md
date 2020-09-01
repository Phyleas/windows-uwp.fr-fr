---
title: Notifications de Mes Contacts
description: Explique comment créer et utiliser des notifications My People, un nouveau type de Toast.
ms.date: 10/25/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 3e00e3de9445a8b7c63ebaead70173c29b637b54
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89166323"
---
# <a name="my-people-notifications"></a>Notifications de Mes Contacts

Les notifications de mes personnes offrent aux utilisateurs une nouvelle façon de se connecter aux personnes qui les intéressent, par le biais de gestes Express rapides. Cet article explique comment concevoir et implémenter des notifications My People dans votre application. Pour obtenir des implémentations complètes, consultez l' [exemple mes notifications de personnes.](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/MyPeopleNotifications)

![notification d’Emoji cardiaque](images/heart-emoji-notification-small.gif)

## <a name="requirements"></a>Spécifications

+ Windows 10 et Microsoft Visual Studio 2019. Pour plus d’informations sur l’installation, consultez la page [obtenir une configuration avec Visual Studio](../get-started/get-set-up.md).
+ Connaissances de base de C# ou d’un langage de programmation orienté objet similaire. Pour commencer à utiliser C#, consultez [créer une application « Hello, World »](../get-started/create-a-hello-world-app-xaml-universal.md).

## <a name="how-it-works"></a>Fonctionnement

Comme alternative aux notifications Toast génériques, vous pouvez désormais envoyer des notifications via la fonctionnalité mes personnes pour offrir une expérience plus personnelle aux utilisateurs. Il s’agit d’un nouveau type de Toast, envoyé à partir d’un contact épinglé dans la barre des tâches de l’utilisateur avec la fonctionnalité mes contacts. Lorsque la notification est reçue, l’image de contact de l’expéditeur s’anime dans la barre des tâches et un son est lu, signalant que la notification démarre. L’animation ou l’image spécifiée dans la charge utile est affichée pendant 5 secondes (ou, si la charge utile est une animation inférieure à 5 secondes, elle est lue jusqu’à ce que 5 secondes se soient écoulées).

## <a name="supported-image-types"></a>Types d’images pris en charge

+ GIF
+ Image statique (JPEG, PNG)
+ SpriteSheet (vertical uniquement)

> [!NOTE]
> Un SpriteSheet est une animation dérivée d’une image statique (JPEG ou PNG). Les images individuelles sont organisées verticalement, de telle sorte que la première image se trouve au premier plan (vous pouvez cependant spécifier une autre image de départ dans la charge utile Toast). Chaque frame doit avoir la même hauteur, que le programme parcourt pour créer une séquence animée (comme un livret animé dont les pages sont présentées verticalement). Vous trouverez ci-dessous un exemple de SpriteSheet.

![arc-en-SpriteSheet](images/shoulder-tap-rainbow-spritesheet.png)

## <a name="notification-parameters"></a>Paramètres de notification
Les notifications mes personnes utilisent l’infrastructure de [notification Toast](../design/shell/tiles-and-notifications/adaptive-interactive-toasts.md) , mais nécessitent un nœud de liaison supplémentaire dans la charge utile Toast. Cette deuxième liaison doit inclure le paramètre suivant :

```xml
experienceType="shoulderTap"
```

Cela indique que le Toast doit être traité comme une notification mes contacts.

Le nœud d’image à l’intérieur de la liaison doit inclure les paramètres suivants :

+ **src**
    + URI de l’élément multimédia. Il peut s’agir de l’URI Web HTTP/HTTPs, d’un URI msappx ou d’un chemin d’accès à un fichier local.
+ **SpriteSheet-SRC**
    + URI de l’élément multimédia. Il peut s’agir de l’URI Web HTTP/HTTPs, d’un URI msappx ou d’un chemin d’accès à un fichier local. Obligatoire uniquement pour les animations SpriteSheet.
+ **SpriteSheet-hauteur**
    + Hauteur du frame (en pixels). Obligatoire uniquement pour les animations SpriteSheet.
+ **SpriteSheet-fps**
    + Images par seconde (FPS). Obligatoire uniquement pour les animations SpriteSheet. Seules les valeurs 1-120 sont prises en charge.
+ **spritesheet-startingFrame**
    + Numéro de frame à partir duquel commencer l’animation. Utilisé uniquement pour les animations SpriteSheet et par défaut 0 si non fourni.
+ **alt**
    + Chaîne de texte utilisée pour la narration du lecteur d’écran.

> [!NOTE]
> Quand vous créez une notification animée, vous devez toujours spécifier une image statique dans le paramètre « SRC ». Il sera utilisé comme un basculement si l’affichage de l’animation échoue.

En outre, le nœud Toast de niveau supérieur doit inclure le paramètre **Hint-People** pour spécifier le contact d’envoi. Ce paramètre peut prendre les valeurs suivantes :

+ **Adresse e-mail** 
    + Par exemple, ` mailto:johndoe@mydomain.com `
+ **Numéro de téléphone** 
    + Par exemple, Tél. : 888-888-8888
+ **Identifiant distant** 
    + Par exemple, remoteid : 1234

> [!NOTE]
> Si votre application utilise les [API ContactStore](/uwp/api/windows.applicationmodel.contacts.contactstore) et utilise la propriété [StoredContact. RemoteId](/uwp/api/Windows.Phone.PersonalInformation.StoredContact.RemoteId) pour lier des contacts stockés sur le PC à des contacts stockés à distance, il est essentiel que la valeur de la propriété RemoteId soit stable et unique. Cela signifie que l’ID distant doit identifier de manière cohérente un compte d’utilisateur unique et doit contenir une balise unique pour garantir qu’il n’est pas en conflit avec les ID distants d’autres contacts sur le PC, y compris les contacts appartenant à d’autres applications.
> Si les ID distants utilisés par votre application ne sont pas forcément stables et uniques, vous pouvez utiliser la [classe RemoteIdHelper](/previous-versions/windows/apps/jj207024(v=vs.105)#BKMK_UsingtheRemoteIdHelperclass) pour ajouter une balise unique à tous vos ID distants avant de les ajouter au système. Vous pouvez également choisir de ne pas utiliser la propriété RemoteId et créer à la place une propriété étendue personnalisée dans laquelle stocker les ID distants de vos contacts.

En plus de la deuxième liaison et de la charge utile, vous devez inclure une autre charge utile dans la première liaison pour le toast de secours. La notification l’utilise si elle est forcée à revenir à un toast normal (expliqué plus loin à la [fin de cet article](#falling-back-to-toast)).

## <a name="creating-the-notification"></a>Création de la notification
Vous pouvez créer un modèle de notification mes personnes comme vous le feriez pour une [notification de Toast](../design/shell/tiles-and-notifications/adaptive-interactive-toasts.md).

Voici un exemple de création d’une notification mes personnes avec une charge utile d’image statique :

```xml
<toast hint-people="mailto:johndoe@mydomain.com">
    <visual lang="en-US">
        <binding template="ToastGeneric">
            <text hint-style="body">Toast fallback</text>
            <text>Add your fallback toast content here</text>
        </binding>
        <binding template="ToastGeneric" experienceType="shoulderTap">
            <image src="https://docs.microsoft.com/windows/uwp/contacts-and-calendar/images/shoulder-tap-static-payload.png"/>
        </binding>
    </visual>
</toast>
```

Quand vous démarrez la notification, elle doit ressembler à ceci :

![notification d’image statique](images/static-image-notification-small.gif)

Voici un exemple de création d’une notification avec une charge utile SpriteSheet animée. Ce SpriteSheet a une hauteur de trame de 80 pixels, que nous allons animer à 25 images par seconde. Nous avons défini le frame de départ sur 15 et nous l’avons fourni avec une image de secours statique dans le paramètre « SRC ». L’image de secours est utilisée si l’animation SpriteSheet ne s’affiche pas.

```xml
<toast hint-people="mailto:johndoe@mydomain.com">
    <visual lang="en-US">
        <binding template="ToastGeneric">
            <text hint-style="body">Toast fallback</text>
            <text>Add your fallback toast content here</text>
        </binding>
        <binding template="ToastGeneric" experienceType="shoulderTap">
            <image src="https://docs.microsoft.com/windows/uwp/contacts-and-calendar/images/shoulder-tap-pizza-static.png"
                spritesheet-src="https://docs.microsoft.com/windows/uwp/contacts-and-calendar/images/shoulder-tap-pizza-spritesheet.png"
                spritesheet-height='80' spritesheet-fps='25' spritesheet-startingFrame='15'/>
        </binding>
    </visual>
</toast>
```

Quand vous démarrez la notification, elle doit ressembler à ceci :

![notification SpriteSheet](images/pizza-notification-small.gif)

## <a name="starting-the-notification"></a>Démarrage de la notification
Pour démarrer une notification mes personnes, nous devons convertir le modèle de Toast en un objet [XmlDocument](/uwp/api/windows.data.xml.dom.xmldocument) . Une fois que vous avez défini le Toast dans un fichier XML (nommé « content.xml »), vous pouvez utiliser ce code pour le démarrer :

```CSharp
string xmlText = File.ReadAllText("content.xml");
XmlDocument xmlContent = new XmlDocument();
xmlContent.LoadXml(xmlText);
```

Vous pouvez ensuite utiliser ce code pour créer et envoyer la notification toast :

```CSharp
ToastNotification notification = new ToastNotification(xmlContent);
ToastNotificationManager.CreateToastNotifier().Show(notification);
```

## <a name="falling-back-to-toast"></a>Revenir à Toast
Dans certains cas, une notification mes personnes s’affiche à la place en tant que notification Toast normale. Une notification mes personnes revient à Toast dans les conditions suivantes :

+ L’affichage de la notification échoue
+ Les notifications mes personnes ne sont pas activées par le destinataire
+ Le contact de l’expéditeur n’est pas épinglé à la barre des tâches du récepteur

Si une notification mes personnes revient à Toast, la deuxième liaison propre à mes personnes est ignorée et seule la première est utilisée pour afficher le Toast. C’est pourquoi il est essentiel de fournir une charge utile de secours dans la première liaison Toast.

## <a name="see-also"></a>Voir aussi
+ [Exemple de notifications My People](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/MyPeopleNotifications)
+ [Ajout de la prise en charge de mes contacts](my-people-support.md)
+ [Notifications de Toast adaptatif](../design/shell/tiles-and-notifications/adaptive-interactive-toasts.md)
+ [ToastNotification, classe](/uwp/api/windows.ui.notifications.toastnotification)