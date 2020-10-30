---
description: Lorsque vous soumettez votre application, vous avez la possibilité d’utiliser la page Remarques pour la certification pour fournir des informations supplémentaires aux testeurs de certification. Ces informations peuvent aider à garantir que votre application est testée correctement.
title: Notes pour la certification
ms.assetid: 4A740A5F-F39F-4FE2-9391-EE00DB46B25A
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, UWP, notes pour les testeurs
ms.localizationpriority: medium
ms.openlocfilehash: 9d1c662de1fc30bc8cb1c92778e4e015acaaf261
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93035042"
---
# <a name="notes-for-certification"></a>Notes pour la certification


Lorsque vous soumettez votre application, vous avez la possibilité d’utiliser la pages **Remarques pour la certification** pour fournir des informations supplémentaires aux testeurs de certification. Ces informations peuvent aider à garantir que votre application est testée correctement. L’inclusion de ces notes est particulièrement importante pour les produits qui utilisent les services Xbox Live et/ou qui nécessitent une connexion à un compte. Si nous ne pouvons pas tester entièrement votre envoi, cela peut provoquer l’échec de la certification.

Prenez soin d’inclure les éléments suivants (s’ils sont applicables à votre application) :

-   **Noms d’utilisateur et mots de passe pour les comptes de test** : Si votre application exige que les utilisateurs se connectent à un service ou à un compte de réseau social, fournissez le nom d’utilisateur et le mot de passe d’un compte de test. Les testeurs de certification utiliseront ce compte lors de l’examen de votre application.

-   **Étapes d’accès aux fonctionnalités masquées ou verrouillées** : Décrivez brièvement comment les testeurs peuvent accéder aux fonctionnalités, aux modes ou au contenu qui peuvent ne pas être évidents. Les applications incomplètes risquent de ne pas obtenir de certification.

-   **Procédure de vérification de l’utilisation de l’audio en arrière-plan** : Si votre application autorise l’exécution de l’audio en arrière-plan, les testeurs peuvent avoir besoin d’instructions sur l’accès à cette fonctionnalité pour qu’elle puisse vérifier qu’elle fonctionne correctement.

-  **Différences de comportement attendues en fonction de la région ou d’autres paramètres client** : par exemple, si des clients dans des régions différentes voient un contenu différent, assurez-vous de les appeler afin que les testeurs comprennent les différences et les révisions appropriées.

-   **Informations sur les modifications apportées à une mise à jour d’application** : pour les mises à jour des applications précédemment publiées, vous pouvez faire en sorte que les testeurs sachent ce qui a changé, surtout si vos packages sont identiques et que vous apportez simplement des modifications à votre liste d’applications (par exemple, en ajoutant des captures d’écran, en modifiant la catégorie de votre application

-   **La date à laquelle vous entrez les remarques** : cette opération est particulièrement importante si vous utilisez un bac à sable (sandbox) de développement dans l’espace partenaires (par exemple, c’est le cas pour tout jeu qui s’intègre à Xbox Live), car les notes que vous entrez lors de la publication dans un bac à sable (sandbox) sont conservées lorsque vous demandez la certification. L’affichage de la date permet aux testeurs de déterminer s’il existe des problèmes temporaires qui peuvent ne plus s’appliquer.

-  **Tout ce que vous pensez que les testeurs devront comprendre sur votre envoi**

Prenez en considération les points suivants lors de la rédaction des remarques :

-   **Une personne réelle lira ces remarques.** Les testeurs apprécieront des remarques polies et claires ainsi que des instructions utiles.

-   **Soyez succinct et donnez des instructions simples.** Si vous devez vraiment expliquer un peu de détails, vous pouvez inclure l’URL d’une page contenant plus d’informations. Toutefois, gardez à l’esprit que les clients de votre application ne verront pas ces notes. Si vous pensez que vous devez fournir des instructions compliquées pour tester votre application, déterminez si votre application peut avoir besoin d’être plus simple afin que les clients (et les testeurs) sachent comment l’utiliser.

-   **Les services et les composants externes doivent être en ligne et disponibles.** Si votre application doit se connecter à un service pour fonctionner, assurez-vous que le service est en ligne et disponible. Incluez toutes les informations sur le service dont les testeurs auront besoin, par exemple les informations de connexion. Si votre application ne peut pas se connecter à un service nécessaire au cours du test, elle risque de ne pas obtenir sa certification.

 

 




