---
title: Bien démarrer avec l’utilisation de Docker pour un développement à distance à l’aide de conteneurs
description: Guide pour bien démarrer avec Docker Desktop sur Windows ou WSL.
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
ms.technology: windows-nodejs
keywords: Microsoft, Windows, Docker, WSL, développement à distance, Containers, Docker Desktop, comparaison entre Windows et WSL
ms.date: 09/24/2020
ms.openlocfilehash: b3fc2509aa6a623bebd9f4566f3b8e75301251db
ms.sourcegitcommit: c65f62bda57563f6196691e7b9c25cbf5a8b16e5
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/07/2020
ms.locfileid: "91780584"
---
# <a name="overview-of-docker-remote-development-on-windows"></a>Vue d’ensemble du développement à distance Docker sur Windows

L’utilisation de conteneurs pour le développement à distance et le déploiement d’applications avec la plateforme Docker est une solution très populaire qui présente de nombreux avantages. Découvrez la variété des prises en charge proposées par les outils et services Microsoft, notamment le sous-système Windows pour Linux (WSL), Visual Studio, Visual Studio Code, .NET, ainsi qu’un large éventail de services Azure.

## <a name="docker-on-windows-10"></a>Docker sur Windows 10

:::row:::
    :::column:::
       [![Icône de la documentation Docker](../../images/docker-docs-icon.png)](https://docs.docker.com/docker-for-windows/install/)<br>
        **[Installer Docker Desktop pour Windows](https://docs.docker.com/docker-for-windows/install/)**<br>
        Découvrez les étapes d’installation, la configuration requise, ce qui est inclus dans le programme d’installation, la procédure de désinstallation, les différences entre la version stable et la version Edge, et comment passer d’un conteneur Windows à un conteneur Linux.
    :::column-end:::
    :::column:::
       [![Capture d’écran de Docker en cours d’exécution](../../images/docker-running-screenshot.png)](https://docs.docker.com/get-started/)<br>
        **[Prise en main de Docker](https://docs.docker.com/get-started/)**<br>
        Documentation sur l’orientation et la configuration de Docker, avec des instructions pas à pas pour bien démarrer, y compris une vidéo d’explication étape par étape.
    :::column-end:::
    :::column:::
       [![Capture d’écran des cours Microsoft Learn sur Docker](../../images/docker-learn-course.png)](/learn/modules/intro-to-docker-containers/)<br>
        **[Cours MS Learn : Présentation des conteneurs Docker](/learn/modules/intro-to-docker-containers/)**<br>
        Microsoft Learn offre un cours d’introduction gratuit sur les conteneurs Docker, en plus d’un [large éventail de cours](/learn/browse/?terms=docker) permettant de bien démarrer avec Docker et la connexion aux services Azure.
    :::column-end:::
    :::column:::
       [![Capture d’écran du menu WSL2 de Docker Desktop](../../images/docker-wsl2.png)](/windows/wsl/tutorials/wsl-containers)<br>
        **[Bien démarrer avec les conteneurs distants Docker sur WSL2](/windows/wsl/tutorials/wsl-containers)**<br>
        Découvrez comment configurer Docker Desktop pour Windows à l’aide de WSL2 (le sous-système Windows pour Linux version 2) en vue d’une utilisation avec une ligne de commande Linux (Ubuntu, Debian, SUSE, etc.).
    :::column-end:::
:::row-end:::

## <a name="vs-code-and-docker"></a>VS Code et Docker

:::row:::
    :::column:::
       [![Image d’un conteneur distant VS Code](../../images/vscode-remote-containers.png)](https://code.visualstudio.com/docs/remote/create-dev-container)<br>
        **[Créer un conteneur Docker avec VS Code](https://code.visualstudio.com/docs/remote/containers-tutorial)**<br>
        Configurez un environnement de développement complet à l’intérieur d’un conteneur avec l’[extension Remote - Containers](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers), puis recherchez des tutoriels afin de configurer un [conteneur NodeJS](https://code.visualstudio.com/docs/containers/quickstart-node), un [conteneur Python](https://code.visualstudio.com/docs/containers/quickstart-python) ou un [conteneur ASP.NET Core](https://code.visualstudio.com/docs/containers/quickstart-aspnet-core).
    :::column-end:::
    :::column:::
       [![Capture d’écran de l’option Attacher Visual Studio Code de Docker](../../images/vscode-attach-docker.png)](https://code.visualstudio.com/docs/remote/attach-container)<br>
        **[Attacher VS Code à un conteneur Docker](https://code.visualstudio.com/docs/remote/attach-container)**<br>
        Découvrez comment attacher Visual Studio Code à un conteneur Docker qui est déjà en cours d’exécution, ou à un [conteneur situé dans un cluster Kubernetes](https://code.visualstudio.com/docs/remote/attach-container#_attach-to-a-container-in-a-kubernetes-cluster).
    :::column-end:::
    :::column:::
       [![Capture d’écran du menu Conteneur VSCode](../../images/vscode-advanced-docker.png)](https://code.visualstudio.com/docs/remote/containers-advanced)<br>
        **[Configuration avancée du conteneur](https://code.visualstudio.com/docs/remote/containers-advanced)**<br>
        Découvrez les scénarios de configuration avancée pour l’utilisation des conteneurs Docker avec Visual Studio Code, ou lisez cet article pour [inspecter les conteneurs](https://code.visualstudio.com/blogs/2019/10/31/inspecting-containers) en vue d’un débogage avec VS Code.
    :::column-end:::
    :::column:::
       [![Capture d’écran de VSCode Docker Desktop avec WSL](../../images/vscode-docker-wsl.png)](https://code.visualstudio.com/blogs/2020/07/01/containers-wsl)<br>
        **[Utilisation des conteneurs distants dans WSL 2](https://code.visualstudio.com/blogs/2020/07/01/containers-wsl)**<br>
        Découvrez l’utilisation des conteneurs Docker avec WSL 2 (sous-système Windows pour Linux version 2) et découvrez comment tout configurer avec VS Code. Vous pouvez également lire un article sur [son fonctionnement](https://code.visualstudio.com/blogs/2020/03/02/docker-in-wsl2#_how-it-works).
    :::column-end:::
:::row-end:::

## <a name="visual-studio-and-docker"></a>Visual Studio et Docker

:::row:::
    :::column:::
       [![Icône Visual Studio](../../images/visualstudio.png)](/visualstudio/containers/overview#docker-support-in-visual-studio-1)<br>
        **[Prise en charge de Docker dans Visual Studio](/visualstudio/containers/overview#docker-support-in-visual-studio-1)**<br>
        Découvrez la prise en charge Docker qui est disponible pour les projets ASP.NET, ASP.NET Core et .NET Core, ainsi que pour les projets de console .NET Framework dans Visual Studio, en plus de la prise en charge de l’orchestration des conteneurs.
    :::column-end:::
    :::column:::
       [![Menu Docker dans Visual Studio](../../images/visualstudio-docker-menu.png)](/visualstudio/containers/container-tools)<br>
        **[Démarrage rapide : Docker dans Visual Studio](/visualstudio/containers/container-tools)**<br>
        Découvrez comment générer, déboguer et exécuter des applications .NET, ASP.NET ou ASP.NET Core conteneurisées, et comment les publier sur Azure Container Registry (ACR), Docker Hub, Azure App Service, ou sur votre propre registre de conteneurs avec Visual Studio.
    :::column-end:::
    :::column:::
       [![Capture d’écran du tutoriel VS](../../images/visualstudio-tutorial.png)](/visualstudio/containers/tutorial-multicontainer)<br>
        **[Didacticiel : Créer une application multiconteneur avec Docker Compose](/visualstudio/containers/tutorial-multicontainer)**<br>
        Découvrez comment gérer plusieurs conteneurs et établir une communication entre eux lorsque vous utilisez les outils de conteneur dans Visual Studio. Vous pouvez également trouver des liens vers des tutoriels, par exemple [Utiliser Docker avec une application monopage React](/visualstudio/containers/container-tools-react).
    :::column-end:::
    :::column:::
       [![Liens vers les conteneurs VS](../../images/visualstudio-container-links.png)](/visualstudio/containers)<br>
        **[Outils de conteneur dans Visual Studio](/visualstudio/containers)**<br>
        Découvrez les rubriques qui expliquent comment exécuter les outils de génération dans un conteneur, [déboguer des applications Docker](/visualstudio/containers/edit-and-refresh), résoudre les problèmes liés aux outils de développement, déployer des conteneurs Docker et relier Kubernetes à Visual Studio.
    :::column-end:::
:::row-end:::

![Schéma de la taxonomie de base Docker concernant les conteneurs, les images et les registres](../../images/taxonomy-of-docker-terms-and-concepts.png)

## <a name="net-core-and-docker"></a>.NET Core et Docker

:::row:::
    :::column:::
       [![Couverture du guide sur les microservices .NET](../../images/dotnet-microservice-guide.png)](/dotnet/architecture/microservices/)<br>
        **[Guide .NET : Applications et conteneurs de microservice](/dotnet/architecture/microservices/)**<br>
        Guide de présentation des applications basées sur les microservices et gérées à l’aide de conteneurs.
    :::column-end:::
    :::column:::
       [![Schéma sur Docker](../../images/dotnet-docker-infographic.png)](/dotnet/architecture/microservices/container-docker-introduction/docker-defined)<br>
        **[Qu’est-ce que Docker ?](/dotnet/architecture/microservices/container-docker-introduction/docker-defined)**<br>
        Explication de base des conteneurs Docker, y compris la [comparaison entre les conteneurs Docker et les machines virtuelles](/dotnet/architecture/microservices/container-docker-introduction/docker-defined#comparing-docker-containers-with-virtual-machines), ainsi qu’une [taxonomie de base avec les termes et les concepts Docker](/dotnet/architecture/microservices/container-docker-introduction/docker-containers-images-registries) expliquant la différence entre les conteneurs, les images et les registres.
    :::column-end:::
    :::column:::
       [![Schéma de la taxonomie Docker](../../images/taxonomy-of-docker-terms-and-concepts.png)](/dotnet/core/docker/build-container?tabs=windows)<br>
        **[Didacticiel : Conteneuriser une application .NET Core](/dotnet/core/docker/build-container?tabs=windows)**<br>
        Découvrez comment créer un conteneur pour une application .NET Core avec Docker, y compris comment créer un fichier Dockerfile, utiliser les commandes essentielles et nettoyer les ressources.
    :::column-end:::
    :::column:::
       [![Schéma d’un workflow de développement de boucles internes avec Docker](../../images/dotnet-docker-workflow.png)](/dotnet/architecture/microservices/docker-application-development-process/docker-app-development-workflow)<br>
        **[Workflow de développement des applications Docker](/dotnet/architecture/microservices/docker-application-development-process/docker-app-development-workflow)**<br>
        Décrit le workflow de développement de boucles internes pour les applications basées sur un conteneur Docker.
    :::column-end:::
:::row-end:::

## <a name="azure-container-services"></a>Azure Container Services

:::row:::
    :::column:::
       [![Capture d’écran d’Azure Container Instances](../../images/azure-container-instances.png)](/azure/container-instances/)<br>
        **[Azure Container Instances](/azure/container-instances/)**<br>
        Découvrez comment exécuter des conteneurs Docker à la demande dans un environnement Azure managé et Serverless, comment effectuer un déploiement avec l’interface CLI Docker, ARM ou le portail Azure, comment créer des groupes multiconteneurs, comment partager des données entre les conteneurs, comment se connecter à un réseau virtuel, et bien plus encore.
    :::column-end:::
    :::column:::
       [![Capture d’écran d’Azure Container Registry](../../images/azure-container-registry-icon.png)](/azure/container-registry)<br>
        **[Azure Container Registry](/azure/container-registry)**<br>
        Découvrez comment créer, stocker et gérer des images conteneur et des artefacts dans un registre privé pour tous les types de déploiement de conteneur. Créez des registres de conteneurs Azure pour vos pipelines de développement et de déploiement de conteneurs existants, configurez des tâches d’automatisation et découvrez comment gérer vos registres à l’aide de la géoréplication et des bonnes pratiques.
    :::column-end:::
    :::column:::
       [![Capture d’écran d’Azure Service Fabric](../../images/azure-service-fabric.png)](/azure/service-fabric)<br>
        **[Azure Service Fabric](/azure/service-fabric)**<br>
        Découvrez Azure Service Fabric, qui est une plateforme de systèmes distribués facilitant la création de packages, le déploiement et la gestion de microservices et de conteneurs fiables et scalables.
    :::column-end:::
    :::column:::
       [![Capture d’écran d’Azure App Service](../../images/azure-app-service.png)](/azure/app-service)<br>
        **[Azure App Service](/azure/app-service)**<br>
        Découvrez comment créer et héberger des applications web, des back-ends mobiles et des API RESTful dans le langage de programmation de votre choix sans avoir à gérer l’infrastructure. Suivez le [cours MS Learn sur Azure App Service](/learn/modules/deploy-run-container-app-service) pour déployer une application web basée sur une image Docker et configurer un déploiement continu.
    :::column-end:::
:::row-end:::

Découvrez les [services Azure qui prennent en charge les conteneurs](https://azure.microsoft.com/overview/containers/).

## <a name="docker-containers-explainer-video"></a>Vidéo d’explication sur les conteneurs Docker

> [!VIDEO https://www.youtube.com/embed/0oEsMwSxBsk]

## <a name="kubernetes-and-container-orchestration-explainer-video"></a>Vidéo d’explication sur l’orchestration de Kubernetes et des conteneurs

> [!VIDEO https://www.youtube.com/embed/3RTvoI-A7UQ]

## <a name="containers-on-windows"></a>Conteneurs sur Windows

:::row:::
    :::column:::
       [![Icône des conteneurs Windows Server](../../images/windows-server-containers.png)](/virtualization/windowscontainers)<br>
        **[Documentation concernant les conteneurs sur Windows](/virtualization/windowscontainers)**<br>
        Empaquetez des applications avec leurs dépendances et tirez parti de la virtualisation au niveau du système d’exploitation pour fournir des environnements rapides et entièrement isolés sur un même système. Découvrez les [conteneurs Windows](/virtualization/windowscontainers/about) par le biais de guides de démarrage rapide, de guides de déploiement et d’exemples.
    :::column-end:::
    :::column:::
       [![Icône de la FAQ](../../images/faq.png)](/virtualization/windowscontainers/about/faq)<br>
        **[Questions fréquentes concernant les conteneurs Windows](/virtualization/windowscontainers/about/faq)**<br>
        Consultez les questions fréquentes sur les conteneurs. Lisez également l’explication suivante dans StackOverflow : « [What’s the difference between Docker for Windows and Docker on Windows?](https://stackoverflow.com/questions/38464724/whats-the-difference-between-docker-for-windows-and-docker-on-windows/40320748) ».
    :::column-end:::
    :::column:::
       [![Icône de conteneur Windows](../../images/windows-container.png)](/virtualization/windowscontainers/quick-start/set-up-environment?tabs=Windows-10-Client)<br>
        **[Configurer votre environnement](/virtualization/windowscontainers/quick-start/set-up-environment?tabs=Windows-10-Client)**<br>
        Découvrez comment configurer Windows 10 ou Windows Server pour créer, exécuter et déployer des conteneurs, notamment les prérequis, l’installation de Docker et l’utilisation des [images de base de conteneur Windows](/virtualization/windowscontainers/manage-containers/container-base-images).
    :::column-end:::
    :::column:::
       [![Icône AKS](../../images/kubernettes.png)](/azure/aks/windows-container-cli)<br>
        **[Créer un conteneur Windows Server sur Azure Kubernetes Service (AKS)](/azure/aks/windows-container-cli)**<br>
        Découvrez comment déployer un exemple d’application ASP.NET dans un conteneur Windows Server situé sur un cluster AKS à l’aide d’Azure CLI.
    :::column-end:::
:::row-end:::
