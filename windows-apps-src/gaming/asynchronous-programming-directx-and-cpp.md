---
title: Programmation asynchrone (DirectX et C++)
description: Cette rubrique traite des divers points à prendre en considération lorsque vous utilisez la programmation asynchrone et les threads avec DirectX.
ms.assetid: 17613cd3-1d9d-8d2f-1b8d-9f8d31faaa6b
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, jeux, programmation asynchrone, DirectX
ms.localizationpriority: medium
ms.openlocfilehash: e5a8eb1ee9cd6a7b5a00eaf13bf04d8956df42c5
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89172063"
---
# <a name="asynchronous-programming-directx-and-c"></a>Programmation asynchrone (DirectX et C++)



Cette rubrique traite des divers points à prendre en considération lorsque vous utilisez la programmation asynchrone et les threads avec DirectX.

## <a name="async-programming-and-directx"></a>Programmation asynchrone et DirectX


Que vous soyez débutant ou expert dans l’utilisation de DirectX, mettez l’ensemble de votre pipeline de traitement graphique dans un seul thread. Dans n’importe quelle scène donnée d’un jeu, il existe des ressources usuelles telles que les bitmaps, les nuanceurs et d’autres éléments qui nécessitent un accès exclusif. Vous devez synchroniser l’accès à ces ressources sur les threads parallèles. Le rendu est un processus difficile à paralléliser sur plusieurs threads.

Toutefois, si votre jeu est suffisamment complexe ou si vous cherchez à obtenir de meilleures performances, vous pouvez utiliser la programmation asynchrone pour paralléliser certains composants non spécifiques à votre pipeline de rendu. Le matériel moderne comporte des UC basées sur le multicœur ou l’hyperthreading. Par conséquent, votre application doit pouvoir tirer parti de ces fonctionnalités ! Pour ce faire, utilisez la programmation asynchrone pour certains des composants de votre jeu qui n’ont pas besoin d’accès direct au contexte de périphérique Direct3D, notamment :

-   E/S de fichiers
-   physique
-   Intelligence artificielle
-   networking
-   audio
-   controls
-   Composants d’interface utilisateur en XAML

Votre application peut gérer ces composants sur plusieurs threads simultanés. Les E/S de fichiers, en particulier le chargement des ressources, bénéficient grandement du chargement asynchrone, car votre jeu ou application peut être dans un état interactif pendant que plusieurs (voire plusieurs centaines) de mégaoctets de ressources sont en cours de chargement ou de transmission en continu. La façon la plus simple de créer et gérer ces threads est d’utiliser la [Bibliothèque de modèles parallèles](/cpp/parallel/concrt/parallel-patterns-library-ppl) et le modèle **task**, que vous trouverez dans l’espace de noms **concurrency** défini dans PPLTasks.h. L’utilisation de la [Bibliothèque de modèles parallèles](/cpp/parallel/concrt/parallel-patterns-library-ppl) permet de tirer parti des UC basées sur le multicœur et l’hyperthreading. En outre, elle permet d’améliorer de nombreux aspects, qu’il s’agisse des temps de chargement perçus, des défaillances ou des retards liés aux calculs intensifs de l’UC ou au réseau.

> **Remarque**    Dans une application plateforme Windows universelle (UWP), l’interface utilisateur s’exécute entièrement dans un thread cloisonné (STA). Si vous créez une interface utilisateur pour votre jeu DirectX à l’aide de l’[interopérabilité XAML](directx-and-xaml-interop.md), vous ne pouvez accéder aux contrôles qu’à l’aide du thread unique cloisonné (STA).

 

## <a name="multithreading-with-direct3d-devices"></a>Multithreading avec des périphériques Direct3D


Le multithreading pour les contextes de périphérique est disponible uniquement sur les périphériques graphiques qui prennent en charge un niveau de fonctionnalité Direct3D de 11 \_ 0 ou plus. Toutefois, vous pouvez être amené à maximiser l’utilisation de la puissance de l’unité de traitement graphique (GPU) sur de nombreuses plateformes, par exemple les plateformes de jeux dédiées. Dans le cas de figure le plus simple, vous pouvez séparer le rendu de la superposition d’un affichage à tête haute de la projection et du rendu de la scène 3D. Par ailleurs, ces deux composants utilisent des pipelines parallèles distincts. Les deux threads doivent utiliser le même [**ID3D11DeviceContext**](/windows/desktop/api/d3d11/nn-d3d11-id3d11devicecontext) pour créer et gérer les objets de ressource (textures, maillages, nuanceurs et autres éléments multimédias), mais qui sont à thread unique et qui requiert l’implémentation d’une sorte de mécanisme de synchronisation (comme les sections critiques) pour y accéder en toute sécurité. Bien que vous puissiez créer des listes de commandes distinctes pour le contexte de périphériques sur différents threads (à des fins de rendu différé), vous ne pouvez pas lire ces listes de commandes simultanément sur la même instance de **ID3D11DeviceContext**.

À présent, votre application peut également utiliser [**ID3D11Device**](/windows/desktop/api/d3d11/nn-d3d11-id3d11device), qui est sécurisé pour le multithreading, pour créer des objets de ressource. Alors, pourquoi ne pas toujours utiliser **ID3D11Device** à la place de [**ID3D11DeviceContext**](/windows/desktop/api/d3d11/nn-d3d11-id3d11devicecontext) ? Actuellement, la prise en charge des pilotes pour le multithreading n’est pas toujours disponible pour certaines interfaces graphiques. Vous pouvez interroger l’appareil et déterminer s’il prend en charge le multithreading. Toutefois, si vous cherchez à atteindre le public le plus large possible, utilisez simplement le **ID3D11DeviceContext** à thread unique pour la gestion des objets de ressources. Néanmoins, lorsque le pilote de périphérique graphique ne prend pas en charge le multithreading ou les listes de commandes, Direct3D 11 tente de gérer l’accès synchronisé au contexte de périphérique de manière interne. Si les listes de commandes ne sont pas prises en charge, il fournit une implémentation logicielle. Par conséquent, vous pouvez écrire du code multithread qui s’exécute sur les plateformes dont les interfaces graphiques n’ont pas de pilotes prenant en charge l’accès au contexte de périphérique à l’aide du multithreading.

Si votre application prend en charge les threads distincts pour le traitement des listes de commandes et l’affichage des trames, vous pouvez maintenir l’unité de traitement graphique (GPU) active et permettre le traitement des listes de commandes tout en affichant les trames en temps voulu, sans saccade ou décalage perceptible. Dans ce cas, vous pouvez utiliser un [**ID3D11DeviceContext**](/windows/desktop/api/d3d11/nn-d3d11-id3d11devicecontext) distinct pour chaque thread et partager des ressources (comme les textures) en les créant avec l' \_ \_ indicateur partagé misc de la ressource d3d11 \_ . Dans ce scénario, [**ID3D11DeviceContext :: Flush**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-flush) doit être appelé sur le thread de traitement pour terminer l’exécution de la liste de commandes avant d’afficher les résultats du traitement de l’objet de ressource dans le thread d’affichage.

## <a name="deferred-rendering"></a>Rendu différé


Le rendu différé enregistre les commandes graphiques dans une liste de commandes afin qu’elles puissent être lues à un autre moment. Cette fonctionnalité est conçue pour la prise en charge du rendu d’un thread unique pendant l’enregistrement des commandes de rendu des threads supplémentaires. Une fois ces commandes terminées, elles peuvent être exécutées sur le thread qui génère l’objet d’affichage final (tampon de trame, texture ou toute autre sortie graphique).

Créez un contexte différé à l’aide de [**ID3D11Device :: CreateDeferredContext**](/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createdeferredcontext) (au lieu de [**D3D11CreateDevice**](/windows/desktop/api/d3d11/nf-d3d11-d3d11createdevice) ou [**D3D11CreateDeviceAndSwapChain**](/windows/desktop/api/d3d11/nf-d3d11-d3d11createdeviceandswapchain), qui crée un contexte immédiat). Pour plus d’informations, voir [Rendu immédiat et différé](/windows/desktop/direct3d11/overviews-direct3d-11-render-multi-thread-render).

## <a name="related-topics"></a>Rubriques connexes


* [Introduction au multithreading dans Direct3D 11](/windows/desktop/direct3d11/overviews-direct3d-11-render-multi-thread-intro)

 

 