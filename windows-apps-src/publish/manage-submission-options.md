---
description: Gérez les options d’envoi, telles que les options de conservation de la publication, les notes pour la certification, et bien plus encore.
title: Gérer les options de soumission
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, UWP, publication en attente, date de publication, envoi d’envoi pour publication, approbation de fonctionnalité restreinte
ms.localizationpriority: medium
ms.openlocfilehash: 0044ac362cb67d356707c7eaaf12a10c5a31ed1b
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93034742"
---
# <a name="manage-submission-options"></a>Gérer les options de soumission

La page **options d’envoi** du processus d’envoi de l’application vous permet de fournir des informations supplémentaires pour nous aider à tester correctement votre produit. Il s’agit d’une étape facultative, mais elle est recommandée pour de nombreuses soumissions. Vous pouvez également définir éventuellement des options de conservation de la publication si vous souhaitez retarder le processus de publication.


## <a name="publishing-hold-options"></a>Options de conservation de la publication

Par défaut, nous publierons votre envoi dès qu’il passera la certification (ou selon les dates que vous avez spécifiées dans la section  [planification](configure-precise-release-scheduling.md) de la page **tarification et disponibilité** ). Vous pouvez éventuellement choisir de mettre en attente la publication de votre soumission jusqu’à une date donnée, ou jusqu’à ce que vous indiquez manuellement qu’elle doit être publiée. Les options de cette section sont décrites ci-dessous. 


### <a name="publish-your-submission-as-soon-as-it-passes-certification-or-per-dates-you-specify"></a>Publier votre envoi dès qu’il passe la certification (ou selon les dates que vous spécifiez)

**Publiez cette soumission dès qu’elle réussit la certification (ou que les dates que vous avez sélectionnées dans la section planification)** est la sélection par défaut et signifie que votre envoi commence le processus de publication dès qu’il réussit la certification, sauf si vous avez configuré des dates dans la section [planification](configure-precise-release-scheduling.md) de la page **tarification et disponibilité** .   

Pour la plupart des soumissions, nous vous recommandons de laisser la section **options de conservation de publication** définie sur cette option. Si vous souhaitez spécifier certaines dates pour la publication de votre envoi, utilisez l' **option publier cette soumission dès qu’elle transmet la certification (ou selon les dates que vous avez sélectionnées dans la section planification)** . Si vous laissez cette section à l’option par défaut, l’envoi n’est pas publié avant la date ou les dates que vous avez définies dans la section **planification** . Les dates que vous avez sélectionnées dans la section **planification** sont utilisées pour déterminer à quel moment votre produit devient disponible pour les clients du Store.


### <a name="publish-your-submission-manually"></a>Publier votre soumission manuellement

Si vous ne souhaitez pas encore définir une date de publication et que vous préférez que votre envoi reste non publié jusqu’à ce que vous décidiez de démarrer le processus de publication manuellement, vous pouvez choisir **ne pas publier cette soumission tant que je n’ai pas sélectionné publier maintenant** . Si vous choisissez cette option, votre soumission ne sera pas publiée tant que vous n’aurez pas indiqué qu’elle devrait l’être. Une fois que votre envoi est certifié, vous pouvez le publier en sélectionnant **publier maintenant** dans la page État de la certification, ou en sélectionnant une date spécifique de la même manière que celle décrite ci-dessous.


### <a name="start-publishing-your-submission-on-a-certain-date"></a>Commencez la publication de votre envoi à une date donnée

Choisissez **Démarrer la publication de cet envoi sur** pour vous assurer que l’envoi n’est pas publié avant une certaine date. Avec cette option, votre soumission sera publiée aussitôt que possible à la date spécifiée ou après. La date doit être postérieure de 24 heures au moins. En parallèle de la date, vous pouvez également définir l’heure à laquelle la publication de la soumission doit démarrer. 

Vous pouvez modifier cette date de publication après avoir envoyé votre produit, à condition qu’elle n’ait pas encore entré l’étape de publication. 
 
Comme indiqué précédemment, si vous souhaitez spécifier certaines dates pour la publication de votre envoi, utilisez l’option **publier cette soumission dès qu’elle transmet la certification (ou selon les dates que vous avez sélectionnées dans la section planification)** et laissez les **options de conservation des publications** définies sur la sélection par défaut. L’utilisation de l’option **Démarrer la publication de cet envoi sur** signifie que votre envoi ne démarrera pas le processus de publication jusqu’à cette date, mais les retards lors de la certification ou de la publication peuvent entraîner une date de publication réelle postérieure à la date que vous sélectionnez. 


## <a name="notes-for-certification"></a>Notes pour la certification

Lorsque vous soumettez votre application, vous avez la possibilité d’utiliser la section **Remarques pour la certification** pour fournir des informations supplémentaires aux testeurs de certification. Ces informations peuvent aider à garantir que votre application est testée correctement. 

Pour plus d’informations, consultez [Notes pour la certification](notes-for-certification.md).


## <a name="restricted-capabilities"></a>Fonctionnalités restreintes

Si nous détectons que vos packages déclarent des [fonctionnalités restreintes](../packaging/app-capability-declarations.md#restricted-capabilities), vous devez fournir des informations dans cette section afin de recevoir l’approbation. Pour chaque fonctionnalité, dites-nous pourquoi votre application doit déclarer la fonctionnalité et comment elle est utilisée. Veillez à fournir autant de détails que possible pour nous aider à comprendre pourquoi votre produit doit déclarer la fonctionnalité. 

Lors du processus de certification, nos testeurs examinent les informations que vous avez fournies afin de déterminer si votre soumission est approuvée pour utiliser la fonctionnalité. Notez que cette opération peut avoir pour effet de rallonger le processus de certification de votre soumission. Si nous approuvons votre utilisation de la fonctionnalité, votre application poursuit le processus de certification. Il n'est généralement pas nécessaire de répéter le processus d'approbation des fonctionnalités lorsque vous procédez à des mises à jour de votre application (sauf si vous déclarez des fonctionnalités supplémentaires). 

Si nous n’approuvez pas votre utilisation de la fonctionnalité, votre envoi échouera à la certification et nous fournirons des commentaires dans le rapport de certification. Vous avez ensuite la possibilité de créer un nouvel envoi et de télécharger des packages qui ne déclarent pas la fonctionnalité ou, le cas échéant, de résoudre les problèmes liés à votre utilisation de la fonctionnalité et de demander l’approbation dans une nouvelle soumission.

Notez qu’il existe des fonctionnalités restreintes qui seront très rarement approuvées. Pour plus d’informations sur chaque fonctionnalité restreinte, consultez [déclarations de fonctionnalités d’application](../packaging/app-capability-declarations.md#restricted-capabilities).

