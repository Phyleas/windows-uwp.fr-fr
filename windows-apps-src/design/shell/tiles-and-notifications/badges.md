---
description: Découvrez comment utiliser les vignettes, badges, toasts et notifications pour fournir des points d’entrée dans votre application et maintenir les utilisateurs informés.
title: Notifications de badge pour les applications Windows
ms.assetid: 48ee4328-7999-40c2-9354-7ea7d488c538
label: Tiles, badges, and notifications
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: a49d771b7efdbb7e787db0cbadea45c255a1120e
ms.sourcegitcommit: 140bbbab0f863a7a1febee85f736b0412bff1ae7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/13/2020
ms.locfileid: "91984595"
---
# <a name="badge-notifications-for-windows-apps"></a>Notifications de badge pour les applications Windows

 

<div style="float:left; font-size:80%; text-align:left; margin: 0px 15px 15px 0px;">
<img src="images/badge-example.png" alt="A tile with a numeric badge displaying the number 63 to indicate 63 unread mails." style="padding-bottom:0.0em; margin-bottom: 2px" /><br/>Une vignette avec un badge numérique affichant<br/> le nombre 63 pour indiquer 63 e-mails non lus.</div>

Un badge de notification transmet des informations récapitulatives ou d’état propres à votre application. Ces informations peuvent être numériques (1 à 99) ou correspondre à l’un des ensembles de glyphes fournis par le système. Voici quelques exemples des informations les mieux transmises par l’intermédiaire d’un badge : état de la connexion réseau dans un jeu en ligne, statut de l’utilisateur dans une application de messagerie, nombre de messages non lus dans une application de courrier électronique et nombre de nouveaux billets dans une application de médias sociaux. 

Les badges de notification apparaissent sur l’icône de la barre des tâches de votre application et dans le coin inférieur droit de sa vignette d’accueil, que l’application soit en cours d’exécution ou non. Il est possible d’afficher les badges sur des vignettes de toute taille.  

> [!NOTE]
> Il n’est pas possible de fournir votre propre image de badge. Seules les images de badge fournies par le système sont utilisables.


## <a name="numeric-badges"></a>Badges numériques

Value | Badge | XML
--|--|--
Nombre compris entre 1 et 99 Une valeur de 0 est équivalente à la valeur de glyphe « aucuneˆ» et efface le badge. | <img src="images/badges/badge-numeric.png" alt="A numeric badge less than 100." /> | `<badge value="1"/>`
N’importe quel nombre supérieur à 99 | <img src="images/badges/badge-numeric-greater.png" alt="A numeric badge greater than 99." /></td> | `<badge value="100"/>`

## <a name="glyph-badges"></a>Badges de glyphe
Au lieu d’un nombre, un badge peut afficher l’un des ensembles de glyphes d’état non extensibles. 

Statut | Glyphe | XML
--|--|--
Aucun | (Aucun badge affiché) | `<badge value="none"/>`
activity | <img src="images/badges/badge-activity.png" alt="Glyph" /> | `<badge value="activity"/>`
alarme | <img src="images/badges/badge-alarm.png" alt="Glyph" /> | `<badge value="alarm"/>`
alerte | <img src="images/badges/badge-alert.png" alt="Glyph" /> | `<badge value="alert"/>`
attention | <img src="images/badges/badge-attention.png" alt="Glyph" /> | `<badge value="attention"/>`
disponible | <img src="images/badges/badge-available.png" alt="Glyph" /> | `<badge value="available"/>`
absent | <img src="images/badges/badge-away.png" alt="Glyph" /> | `<badge value="away"/>`
occupé | <img src="images/badges/badge-busy.png" alt="Glyph" /> | `<badge value="busy"/>`
error | <img src="images/badges/badge-error.png" alt="Glyph" /> | `<badge value="error"/>`
nouveau message | <img src="images/badges/badge-newMessage.png" alt="Glyph" /> | `<badge value="newMessage"/>`
en pause | <img src="images/badges/badge-paused.png" alt="Glyph" /> | `<badge value="paused"/>`
lecture en cours | <img src="images/badges/badge-playing.png" alt="Glyph" /> | `<badge value="playing"/>`
non disponible | <img src="images/badges/badge-unavailable.png" alt="Glyph" /> | `<badge value="unavailable"/>`</td>

## <a name="create-a-badge"></a>Créer un badge

Ces exemples vous montrent comment créer une mise à jour de badge.

### <a name="create-a-numeric-badge"></a>Créer un badge numérique

````csharp
private void setBadgeNumber(int num)
{

    // Get the blank badge XML payload for a badge number
    XmlDocument badgeXml = 
        BadgeUpdateManager.GetTemplateContent(BadgeTemplateType.BadgeNumber);

    // Set the value of the badge in the XML to our number
    XmlElement badgeElement = badgeXml.SelectSingleNode("/badge") as XmlElement;
    badgeElement.SetAttribute("value", num.ToString());

    // Create the badge notification
    BadgeNotification badge = new BadgeNotification(badgeXml);

    // Create the badge updater for the application
    BadgeUpdater badgeUpdater = 
        BadgeUpdateManager.CreateBadgeUpdaterForApplication();

    // And update the badge
    badgeUpdater.Update(badge);

}
````

### <a name="create-a-glyph-badge"></a>Créer un badge de glyphe
````csharp
private void updateBadgeGlyph()
{
    string badgeGlyphValue = "alert";

    // Get the blank badge XML payload for a badge glyph
    XmlDocument badgeXml = 
        BadgeUpdateManager.GetTemplateContent(BadgeTemplateType.BadgeGlyph);

    // Set the value of the badge in the XML to our glyph value
    Windows.Data.Xml.Dom.XmlElement badgeElement = 
        badgeXml.SelectSingleNode("/badge") as Windows.Data.Xml.Dom.XmlElement;
    badgeElement.SetAttribute("value", badgeGlyphValue);

    // Create the badge notification
    BadgeNotification badge = new BadgeNotification(badgeXml);

    // Create the badge updater for the application
    BadgeUpdater badgeUpdater = 
        BadgeUpdateManager.CreateBadgeUpdaterForApplication();

    // And update the badge
    badgeUpdater.Update(badge);

}
````

### <a name="clear-a-badge"></a>Effacer un badge

````csharp
private void clearBadge()
{
    BadgeUpdateManager.CreateBadgeUpdaterForApplication().Clear();
}
````

## <a name="get-the-sample-code"></a>Obtenir l’exemple de code

* [Exemples de notification](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Notifications)<br/> Montre comment créer des vignettes dynamiques, envoyer des mises à jour de badge et afficher des notifications toast. 

## <a name="related-articles"></a>Articles connexes

* [Notifications toast adaptatives et interactives](adaptive-interactive-toasts.md)
* [Créer des vignettes](creating-tiles.md)
* [Créer des vignettes adaptatives](create-adaptive-tiles.md)
