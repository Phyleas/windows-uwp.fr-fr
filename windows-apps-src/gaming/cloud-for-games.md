---
title: Utilisation des services de cloud computing pour les jeux UWP
description: Apprenez-en davantage sur l’implémentation de services de cloud computing en tant que serveur principal pour vos jeux UWP.
ms.assetid: 1a7088e0-0d7b-11e6-8e05-0002a5d5c51b
ms.date: 03/27/2018
ms.topic: article
keywords: Windows 10, UWP, jeux, cloud services
ms.localizationpriority: medium
ms.openlocfilehash: 3d959c490474a6280d878be9679abafc31b565d8
ms.sourcegitcommit: 720413d2053c8d5c5b34d6873740be6e913a4857
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/25/2020
ms.locfileid: "88846859"
---
#  <a name="using-cloud-services-for-uwp-games"></a>Utilisation des services de cloud computing pour les jeux UWP

La plateforme Windows universelle (UWP) dans Windows 10 propose un ensemble d’API qui peuvent être utilisées sur les appareils Microsoft pour le développement de jeux. Lorsque vous développez des jeux sur différentes plateformes et différents appareils, vous pouvez utiliser un serveur principal dans le cloud pour accroître leur portée en fonction de la demande.

Si vous recherchez une solution de backend Cloud complète pour votre jeu, consultez [logiciel en tant que service pour le serveur principal du jeu](#software-as-a-service-for-game-backend).

##  <a name="what-is-cloud-computing"></a>Qu’est-ce que le cloud computing ?

Le cloud computing utilise des ressources informatiques et des applications à la demande sur Internet pour stocker et traiter les données de vos appareils. Le terme _cloud_ est une métaphore faisant référence à la disponibilité des vastes ressources qu’il contient (ressources non locales). Vous accédez à celles-ci à partir d’emplacements non spécifiques.
Le principe de cloud computing offre une nouvelle façon de consommer les ressources et les logiciels. Les utilisateurs n’ont plus besoin de payer à l’avance pour l’intégralité du produit ou des ressources. Au lieu de cela, ils peuvent consommer la plateforme, les logiciels et les ressources en tant que service. Les fournisseurs de services de cloud computing facturent souvent leurs clients d’après des plans d’utilisation ou de services.

##  <a name="why-use-cloud-services"></a>Pourquoi utiliser des services de cloud computing ?

L’un des avantages à utiliser des services de cloud computing pour les jeux réside dans le fait que vous n’avez pas besoin d’investir dans des serveurs physiques ni de les payer au comptant. Vous avez uniquement besoin de payer a posteriori en fonction du plan d’utilisation ou de services mis en place. C’est un moyen de gérer les risques induits par le développement d’un nouveau titre de jeu. 

Un autre avantage réside dans le fait que votre jeu peut s’appuyer sur un large éventail de ressources de cloud computing pour gagner en évolutivité (gestion efficace des hausses soudaines du nombre de joueurs simultanés, des calculs de jeu en temps réel ou des exigences liées aux données). Cela préserve la stabilité des performances de votre jeu, à tout moment. En outre, les ressources de cloud computing sont accessibles depuis tout type d’appareil s’exécutant sur tout type de plateforme, n’importe où dans le monde. Cela signifie que vous êtes en mesure de proposer votre jeu aux utilisateurs du monde entier.

Il est important de proposer aux joueurs une expérience de jeu incroyable. Étant donné que les serveurs de jeu qui s’exécutent dans le cloud ne dépendent pas de mises à jour côté client, ils offrent à votre jeu un environnement mieux contrôlé et plus sécurisé dans l’ensemble.   Vous pouvez également parvenir à une cohérence de jeu dans le cloud en ne faisant jamais confiance au client et en adoptant une logique de jeu côté serveur. Rien ne vous empêche non plus de configurer les connexions d’un service à l’autre pour parvenir à une expérience de jeu mieux intégrée. Par exemple, vous pouvez lier les achats dans le jeu à différents modes de paiement, créer une passerelle entre différents réseaux de jeux et partager les mises à jour dans le jeu sur les portails de réseaux sociaux les plus courants comme Facebook et Twitter. 

Vous pouvez également utiliser des serveurs de cloud computing dédiés pour créer un vaste univers de jeu permanent, construire une communauté de joueurs, collecter et analyser les données des joueurs dans le temps afin d’améliorer l’expérience de jeu et optimiser le modèle de conception de monétisation de votre jeu.

Par ailleurs, les jeux qui nécessitent des fonctionnalités de gestion des données de jeu et beaucoup de ressources (par exemple, les jeux sociaux dotés de mécanismes multijoueurs asynchrones) peuvent être implémentés à l’aide de services de cloud computing.

##  <a name="how-game-companies-use-the-cloud-technology"></a>Comment les entreprises spécialisées dans le développement de jeux vidéo utilisent la technologie de cloud computing

Découvrez comment des développeurs ont implémenté des solutions de cloud computing dans leurs jeux.

<table>
    <colgroup>
    <col width="10%" />
    <col width="30%" />
    <col width="30%" />
    <col width="30%" />
    </colgroup>
    <tr class="header" align="left">
        <th>Développeur</th>
        <th>Description</th>
        <th>Principaux scénarios de jeu</th>
        <th>En savoir plus</th>
    </tr>
    <tr>
        <td><a href="https://www.tencent.com">Jeux Tencent</a></td>
        <td><b>Tencent Games</b> a développé une solution innovante qui utilise Azure Service Fabric permettant aux jeux de PC traditionnels d’être fournis en tant que service. Leur solution cloud Game utilise un modèle « léger client + riche Cloud » exécutant des charges de travail en tant que microservices dans le serveur principal.</td>
        <td>
            <ul>
                <li>Les jeux de PC traditionnels sont fournis sous forme de jeux Cloud aux utilisateurs du monde entier <li>Processus de distribution de jeu optimisé <li>Fonctionnalités de jeu isolées en tant que microservices pour réduire la complexité, réduire la répétition des charges de travail en raison de dépendances et possibilité de mettre à niveau les nouvelles fonctionnalités indépendamment <li>Téléchargement du petit package d’installation pour lire le contenu du jeu le plus récent (réduction de la taille des packages de Go à Mo) <li>Réduction des coûts de maintenance </ul>
        </td>
        <td>
            <ul>
                <li><a href="https://customers.microsoft.com/story/tencent-telecommunications-azure-service-fabric-windows-server-en">Les jeux Tencent et Microsoft ont créé la solution cloud Game</a>
                <li><a href="https://channel9.msdn.com/Shows/Cloud+Cover/Episode-228-Building-Games-with-Service-Fabric#time=38m33s">Création de jeux avec Service Fabric : détails sur l’implémentation de tencent (vidéo)</a>
            </ul>
        </td>
    </tr>
    <tr>
        <td><a href="https://www.halowaypoint.com/">343 Industries</a></td>
        <td><b>Halo 5 : les gardiens</b> ont implémenté <a href="https://www.halowaypoint.com/spartan-companies">Halo : Spartan sociétés</a> comme plateforme de jeu sociale à l’aide d’Azure Cosmos dB (via l’API DocumentDB), qui a été sélectionnée pour sa vitesse et sa flexibilité en raison de ses capacités d’indexation automatique.</td>
        <td>
            <ul>
                <li>Couche Données évolutive pour gérer la création de groupes et l’expérience de jeu en mode multijoueur <li>Intégration des jeux et des réseaux sociaux <li>Demandes de données en temps réel par le biais de plusieurs attributs <li>Synchronisation des scores et statistiques de jeu </ul>
        </td>
        <td>
            <ul>
                <li><a href="https://azure.microsoft.com/blog/how-halo-5-guardians-implemented-social-gameplay-using-azure-documentdb/">Jeu social implémenté à l’aide de Azure Cosmos DB (via l’API DocumentDB)</a>
            </ul>
        </td>
    </tr>
    <tr>
        <td><a href="https://web.ageofascent.com/">Illyriad Games</a></td>
        <td>Illyriad Games a créé <b>Age of Ascent</b>, un jeu 3D épique dans l’espace de type MMO (en ligne massivement multijoueur) auquel il est possible de jouer sur les appareils équipés de navigateurs modernes. Ce jeu peut donc être lu sur des PC, des ordinateurs portables, des téléphones mobiles et d’autres appareils mobiles sans plug-ins. Le jeu utilise ASP.NET Core, HTML5, WebGL et Azure.</td>
        <td>
            <ul>
                <li>Jeu multiplateforme sur navigateur <li>Grand monde ouvert, persistant et unique <li>Gestion d’importants calculs de jeu en temps réel <li>Évolutivité en fonction du nombre de joueurs </ul>
        </td>
        <td>
            <ul>
                <li><a href="https://channel9.msdn.com/Shows/Cloud+Cover/Episode-228-Building-Games-with-Service-Fabric#time=06m52s">Création de jeux avec Service Fabric : Age of Ascent MMO) Game (vidéo)</a>
                <li><a href="https://channel9.msdn.com/Events/Build/2016/KEY02#time=57m20s">Gérer les composants de jeu en tant que microservices à l’aide d’Azure Service Fabric (vidéo)</a> 
                <li><a href="/Blogs/Azure/Age-of-Ascent-from-Illyriad-Powered-by-Azure-Service-Fabric-and-ASPNET">Entretien avec Age of Ascent Developers (vidéo)</a>
            </ul>
        </td>
    </tr>
    <tr>
        <td><a href="https://www.nextgames.com/">Prochains Jeux</a></td>
        <td>Next Games est le créateur du jeu vidéo <b>The Walking Dead: No Man’s Land</b> d’après une série originale d’AMC. Le jeu The Walking Dead utilisait Azure comme serveur principal. Le week-end de son ouverture, 1 000 000 de téléchargements ont eu lieu. Dès la première semaine, il était numéro 1 des applications gratuites de l’App Store sur iPhone et iPad aux États-Unis, numéro 1 des applications gratuites dans 12 pays et numéro 1 des jeux gratuits dans 13 pays.
        </td>
        <td>
            <ul>
                <li>Système multiplateforme <li>Mode multijoueur <li>Performances élastiques à grande échelle <li>Protection contre les fraudes au joueur <li>Distribution de contenu dynamique </ul>
        </td>
        <td>
            <ul>
                <li><a href="https://azure.microsoft.com/blog/how-we-built-it-next-games-global-online-gaming-platform-on-azure/">Comment nous l’avons créé : nouvelle plateforme de jeux en ligne globale sur Azure (blog avec vidéo)</a>
                <li><a href="https://azure.microsoft.com/blog/the-walking-dead-no-mans-land-game-soars-to-1-with-azure-documentdb/">Parcours mort utilise Azure Cosmos DB (via l’API DocumentDB) pour accélérer le cycle de développement et un jeu plus attrayant</a>
            </ul>
        </td>
    </tr>
    <tr>
        <td><a href="http://www.crimecoast.com/">Pixels Squad</a></td>
        <td>Pixels Squad a développé <b>Crime Coast</b> à l’aide du moteur de jeu Unity et d’Azure. <b>Crime Coast</b> est un jeu social de stratégie disponible sur les plateformes Android, iOS et Windows. Ont été utilisés dans ce jeu le stockage d’objets Blob Azure, un cache Azure Redis géré, un ensemble d’ordinateurs virtuels IIS à charge équilibrée et un hub de notification Microsoft. Découvrez comment cette société a géré la mise à l’échelle et l’augmentation du nombre de joueurs avec 5 000 joueurs simultanés.
        </td>
        <td>
            <ul>
                <li>Système multiplateforme <li>Jeu en ligne en mode multijoueur <li>Système évolutif en fonction du nombre de joueurs </ul>
        </td>
        <td>
            <ul>
                <li><a href="https://channel9.msdn.com/Blogs/The-Game-Blog/BizSpark-Interview-with-Pixel-Squad-How-the-used-Azure-Cloud-Services-to-make-an-MMO-with-a-3-man-te">Comment le jeu MMO) de la côte crime a utilisé Azure cloud services</a>
            </ul>
        </td>
    </tr> 
</table>

    
### <a name="other-links"></a>Autres liens

* [Hitman et Azure : créer des fonctionnalités de jeu comme des cibles insaisissables qui ne sont possibles qu’avec le Cloud](https://channel9.msdn.com/Series/Hitman)
* [Azure, l’arme secrète de Hitcents, Game Troopers et InnoSpark](https://news.microsoft.com/features/game-developers-use-microsoft-azure-as-secret-sauce-for-scale-and-growth-2/)
* [Les start-up du jeu du programme Bizspark utilisent Azure](https://blogs.technet.microsoft.com/bizspark_featured_startups/2015/09/25/azure-open-for-gaming-startups/)


## <a name="how-to-design-your-cloud-backend"></a>Comment concevoir votre serveur principal dans le cloud

Alors que les fabricants et les concepteurs de jeu discutent des fonctions et fonctionnalités nécessaires, nous vous recommandons de vous pencher sur la façon dont vous voulez concevoir votre infrastructure de jeu. À partir du moment où vous voulez développer des jeux pour différents appareils et sur différentes plateformes parmi les principales, pensez à Azure comme serveur principal pour votre jeu.

### <a name="understanding-iaas-paas-or-saas"></a>Comprendre IaaS, PaaS ou SaaS

Tout d’abord, vous devez prendre en considération le niveau de service le mieux adapté à votre jeu. Une bonne connaissance des trois services suivants et de leurs différences peut vous aider à déterminer l’approche à adopter pour créer votre serveur principal.

* [Infrastructure en tant que service (IaaS)](https://azure.microsoft.com/overview/what-is-iaas/)

    L’infrastructure en tant que service (IaaS) est une infrastructure informatique instantanée, mise en service et gérée via Internet. Imaginez pouvoir utiliser de nombreuses machines immédiatement disponibles pour rapidement augmenter ou réduire votre offre, en fonction de la demande. L’infrastructure en tant que service vous permet d’éviter d’acheter vos propres serveurs physiques et toute autre infrastructure de centre de données, ainsi qu’elle vous libère de la complexité de tels systèmes.

* [Plateforme en tant que service (PaaS)](https://azure.microsoft.com/overview/what-is-paas/)

    La plateforme en tant que service (PaaS) est comme l’infrastructure en tant que service, à ceci près qu’elle intègre également une gestion de l’infrastructure comme les serveurs, le stockage et la mise en réseau. Aussi, non seulement vous n’êtes pas obligé d’acheter des serveurs physiques et une infrastructure de base de données, mais vous n’êtes pas obligé non plus d’acheter ni de gérer des licences logicielles, une infrastructure applicative sous-jacente, un middleware, des outils de développement ou autres ressources.

* [Logiciel en tant que service (SaaS)](https://azure.microsoft.com/overview/what-is-saas/)

    Le modèle SaaS permet aux utilisateurs de se connecter via Internet à des applications basées sur le cloud et de les utiliser. Il fournit une solution logicielle complète que vous achetez sur la base d’un paiement à l’exécution d’un fournisseur de services Cloud.  Les exemples les plus courants sont les outils de messagerie, de calendrier et Office (tels que les applications Microsoft 365 Office). Vous louez l’utilisation d’une application pour votre organisation et vos utilisateurs s’y connectent via Internet, généralement avec un navigateur Web. Toute l’infrastructure sous-jacente, incluant l’intergiciel (middleware), les logiciels applicatifs et les données d’application, est située dans le centre de données du fournisseur de services. Le fournisseur de services gère le matériel et les logiciels, et avec le contrat de service approprié, garantira également la disponibilité et la sécurité du jeu et de vos données. SaaS permet à votre organisation de disposer rapidement d’une application opérationnelle, moyennant un investissement initial minimal.


### <a name="design-your-game-infrastructure-using-azure"></a>Conception de votre infrastructure de jeu avec Microsoft Azure

Voici quelques-unes des façons dont vous pouvez utiliser les offres de services de cloud computing Azure pour un jeu. Azure fonctionne avec Windows, Linux et les technologies Open Source que vous connaissez comme Ruby, Python, Java et PHP. Pour plus d’informations, voir [Azure pour les jeux](https://azure.microsoft.com/solutions/gaming/).

| Configuration requise                 | Scénarios d’activité                            | Offre de produit                      | Fonctionnalités de produit                                    |
|-----------------------------------|-----------------------------------------------|---------------------------------------|----------------------------------------------------|
| Hébergement de votre domaine dans le cloud     | Répondre efficacement aux requêtes DNS            | [DNS Azure](https://azure.microsoft.com/services/dns/) | Hébergement de votre domaine avec des performances et une disponibilité élevées  |
| Connexion et vérification d’identité      | Le joueur se connecte et son identité est authentifiée  | [Azure Active Directory](https://azure.microsoft.com/services/active-directory/) | Authentification unique à toute application web sur site et dans le cloud avec une authentification multifacteur            | 
| Jeu utilisant le modèle Infrastructure en tant que service (IaaS)      | Le jeu est hébergé sur des ordinateurs virtuels dans le cloud       | [Machines virtuelles Azure](https://azure.microsoft.com/services/virtual-machines/) | Mise à l’échelle de 1 à plusieurs milliers d’instances d’ordinateur virtuel en tant que serveurs de jeu avec mise en réseau virtuelle intégrée et équilibrage de la charge ; cohérence hybride avec les systèmes sur site           |
| Jeux web ou sur appareil mobile utilisant le modèle Plateforme en tant que service (PaaS)            | Le jeu est hébergé sur une plateforme gérée                | [Azure App Service](https://azure.microsoft.com/services/app-service/) | Plateforme en tant que service pour les jeux sur site web ou sur appareil mobile (soit des ordinateurs virtuels Azure avec middleware/outils de développement/aide à la décision/gestion de base de données)   |
| Jeu Cloud multiniveau haute disponibilité et évolutif avec davantage de contrôle du système d’exploitation (PaaS)        | Le jeu est hébergé sur une plateforme gérée                | [Service cloud Azure](https://azure.microsoft.com/services/app-service/) | PaaS conçu pour prendre en charge des applications évolutives, fiables et peu onéreuses pour fonctionner   |
| Équilibrage de charge entre les régions pour améliorer les performances et la disponibilité | Achemine les demandes entrantes de jeux. Peut agir comme premier niveau d’équilibrage de charge.       | [Azure Traffic Manager](https://azure.microsoft.com/services/traffic-manager/) | Offre plusieurs options de basculement automatique et permet de distribuer votre trafic de manière égale ou avec des valeurs pondérées. Peut combiner en toute transparence des systèmes locaux et Cloud. |
| Stockage dans le cloud des données de jeu       | Les dernières données de jeu sont stockées dans le cloud et envoyées aux appareils des clients | [Stockage Blob Azure](https://azure.microsoft.com/services/storage/blobs/)| Aucune restriction sur les types de fichiers qui peuvent être stockés ; stockage d’objets pour de grandes quantités de données non structurées comme les images, les fichiers audio, vidéo et plus encore  |
| Tables de stockage de données temporaires| Les transactions de jeu (changements dans les états de jeu) sont stockées temporairement dans les tables | [Stockage de tables Azure](https://azure.microsoft.com/services/storage/tables/)| Les données de jeu peuvent être stockées dans un schéma flexible en fonction des besoins du jeu |
| Mise en file d’attente des transactions/requêtes de jeu| Les transactions de jeu sont traitées sous la forme d’une file d’attente | [Stockage File d’attente Azure](https://azure.microsoft.com/services/storage/queues/)| Les files d’attente absorbent les pics de trafic inattendus et peuvent empêcher les serveurs d’être submergés par un flux soudain de demandes en cours de jeu   |
| Base de données de jeu relationnelle évolutive| Stockage structuré de données relationnelles de type transactions dans le jeu vers base de données | [Azure SQL Database](https://azure.microsoft.com/services/sql-database/)| Base de données SQL en tant que service ([comparaison avec SQL sur un ordinateur virtuel](https://azure.microsoft.com/documentation/articles/data-management-azure-sql-database-and-sql-server-iaas/))  |
| Base de données de jeu évolutive et distribuée à faible latence| Rapidité des lectures, écritures et requêtes de jeu et des données du joueur avec flexibilité du schéma | [Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/)| Base de données de documents NoSQL à faible latence, gérée en tant que service   |
| Utilisation de votre propre centre de données avec les services Azure | Le jeu est récupéré depuis votre propre centre de données et envoyé sur les appareils du client | [Azure Stack](https://azure.microsoft.com/overview/azure-stack/) | Permet à votre organisation de transmettre des services Windows Azure depuis votre propre centre de données pour plus d’efficacité  |
| Transfert de blocs de données volumineux| Les fichiers volumineux comme les images, les fichiers audio et les vidéos du jeu peuvent être envoyés aux utilisateurs à partir de l’emplacement POP CDN (réseau de diffusion de contenu) le plus proche avec Azure CDN    | [Azure Content Delivery Network](https://azure.microsoft.com/services/cdn/) | Basé sur une topologie réseau moderne avec nœuds centralisés de grande taille, Azure CDN gère les pics soudains de trafic et les charges élevées pour augmenter considérablement la vitesse et la disponibilité, ce qui conduit à une amélioration significative de l’expérience utilisateur  |
| Latence faible               | Effectuer une mise en cache pour créer des jeux rapides et évolutifs, avec plus de contrôle et de garantie d’isolation des données ; peut également être utilisé pour améliorer la fonctionnalité de matchmaking | [Cache Redis Azure](https://azure.microsoft.com/services/cache/) | Débit élevé et accès aux données cohérent et à faible latence pour alimenter rapidement les applications Azure évolutives  |
| Évolutivité élevée, faible latence | Gère les fluctuations du nombre d’utilisateurs du jeu avec des lectures et des écritures à faible latence | [Azure Service Fabric](https://azure.microsoft.com/services/service-fabric/) | Possibilité d’alimenter les scénarios les plus complexes à faible latence avec volumes importants de données et d’assurer une mise à l’échelle fiable pour gérer un plus grand nombre d’utilisateurs à la fois Service Fabric vous permet de développer des jeux sans avoir à créer un magasin ou un cache séparé, tel que cela est demandé pour les applications sans état |
| Capacité à collecter des millions d’événements par seconde depuis les appareils                         | Consigner des millions d’événements par seconde depuis les appareils | [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/) | Ingestion de télémétrie à l’échelle du cloud depuis les jeux, les sites web, les applications et les appareils  |
| Traitement des données de jeu en temps réel  | Effectuer une analyse en temps réel des données des joueurs pour améliorer l’expérience de jeu| [Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/) | Traitement du flux en temps réel dans le cloud  |
| Développement d’une expérience de jeu prédictive         | Créer une expérience de jeu dynamique et personnalisée en fonction des données du joueur  | [Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) | Service cloud entièrement géré qui vous permet de facilement développer, déployer et partager des solutions d’analyse prédictive  |
| Collecte et analyse des données de jeu| Traitement parallèle massif des données à partir de bases de données relationnelles et non relationnelles | [Azure Data Warehouse](https://azure.microsoft.com/services/sql-data-warehouse/)| Entrepôt de données élastique en tant que service doté de fonctionnalités adaptées aux besoins des entreprises   |
| Impliquer les utilisateurs pour augmenter l’utilisation et la rétention| Envoyer des notifications push ciblées à n’importe quelle plateforme à partir de n’importe quel back end pour générer un intérêt et encourager des actions de jeu spécifiques | [Azure Notification Hubs](https://azure.microsoft.com/services/notification-hubs/)| Diffusion Push rapide pour atteindre des millions d’appareils mobiles sur toutes les principales plateformes &mdash; iOS, Android, Windows, Kindle, Baidu. Votre jeu peut être hébergé sur n’importe quel back end &mdash; Cloud ou en local.|
| Diffuser du contenu multimédia vers votre public local et dans le monde entier tout en protégeant votre contenu| Les remorques et les clips cinématiques de la qualité de la diffusion peuvent être surveillés sur tous les appareils| [Azure Media Services](https://azure.microsoft.com/services/media-services/)| Streaming de vidéo en direct et à la demande avec des fonctionnalités de réseau de distribution de contenu intégrées. Utilisez un joueur pour tous vos besoins de lecture, y compris la protection et le chiffrement du contenu.| 
| Développement, distribution et test bêta de vos applications mobiles | Testez et distribuez votre application mobile. Performances des applications et gestion de l’expérience utilisateur. | [HockeyApp](https://azure.microsoft.com/services/hockeyapp/)| Intègre la génération de rapports d’incidents et les métriques des utilisateurs avec une plateforme de distribution d’applications et de commentaires utilisateur. Prend en charge les applications Android, Cordova, iOS, OS X, Unity, Windows et Xamarin. Envisagez également le contrôle mission de [Visual Studio Mobile Center](https://visualstudio.microsoft.com/app-center/) &mdash; pour les applications qui combinent des analyses riches, des rapports d’incidents, des notifications push, la distribution d’applications et bien plus encore. |
| Création de campagnes marketing pour accroître l’utilisation et la fidélisation  | Envoyer des notifications push à certains joueurs pour susciter l’intérêt et encourager des actions de jeu spécifiques en fonction de l’analyse des données | [Mobile engagement](https://azure.microsoft.com/services/mobile-engagement/) : sera mis hors service le 2018 mars et est actuellement disponible uniquement pour les clients existants |  Augmenter le temps de jeu et la fidélisation des utilisateurs sur toutes les principales plateformes (iOS, Android, Windows, Windows Phone) |


##  <a name="startup-and-developer-resources"></a>Ressources pour start-up et développeurs

* [Microsoft for Startups](https://startups.microsoft.com)

    Microsoft for Startups fournit des avantages produits, techniques et Go-to-Market pour accélérer la croissance des démarrages. L’un des avantages consiste à obtenir un compte gratuit Azure. Vous avez $200 de crédit pour explorer les services pendant 30 jours, 12 mois de services gratuits populaires et toujours gratuitement 25 services +. Pour plus d’informations, consultez [la section donner vie à vos idées de démarrage avec un compte gratuit Azure](https://azure.microsoft.com/free/startups/).
    
* [Programmes pour développeurs](e2e.md#developer-programs)

    Microsoft propose plusieurs programmes de développement tels que [ID@Xbox](https://www.xbox.com/Developers/id) et le programme de création de [Xbox Live](https://developer.microsoft.com/games/xbox/xboxlive/creator) pour vous aider à développer et à publier des jeux.

## <a name="learning-resources"></a>Ressources d’apprentissage

* Build 2016 : [CodeLabs &mdash; utilisez Microsoft Azure App service et Microsoft SQL Azure backend pour enregistrer le score de jeu dans Unity](https://github.com/Microsoft-Build-2016/CodeLabs-GameDev-6-Azure)
* Build 2017 : [diffusion d’expériences de jeux de classe mondiale à l’aide de Microsoft Azure : leçons tirées de titres tels que Halo, Hitman et de parcours mort (vidéo)](https://channel9.msdn.com/Events/Build/2017/P4062)
* Ensemble réutilisable de blocs de construction, projets, services et meilleures pratiques conçus pour prendre en charge les charges de travail de jeu courantes à l’aide d’Azure sur GitHub : [des blocs de construction pour les jeux sur Azure](https://github.com/MicrosoftDX/nether)
* [Services de jeux sur Azure (vidéos)](https://channel9.msdn.com/Series/Gaming-Services-on-Azure)

## <a name="tools-and-other-useful-links"></a>Outils et autres liens utiles

* [Forums MSDN &mdash; plateforme Azure](https://social.msdn.microsoft.com/Forums/azure/home?category=windowsazureplatform)
* [Outil de test de charge basé sur le Cloud](https://visualstudio.microsoft.com/team-services/cloud-load-testing/)
* [Kits de développement logiciel (SDK) et outils en ligne de commande](https://azure.microsoft.com/downloads/)
    
## <a name="software-as-a-service-for-game-backend"></a>Serveur principal de jeu et logiciels en tant que service

[PlayFab](https://playfab.com/) gère actuellement plus de 1 200 jeux en direct avec 80 millions lecteurs actifs mensuels. Il s’agit d’une plateforme principale complète qui comprend un LiveOps de pile complet avec contrôle en temps réel. 

Vous pouvez intégrer cette solution dans vos jeux mobiles, PC ou console à l’aide de kits de développement logiciel (SDK). Des kits de développement logiciel (SDK) sont disponibles pour tous les moteurs de jeux et plateformes populaires, y compris Android, iOS, inréel, Unity et Windows. Pour commencer, consultez [la documentation](https://api.playfab.com/).

Il offre des services de jeu tels que l’authentification, la gestion des données du lecteur, le multijoueur et l’analyse en temps réel pour aider votre jeu à augmenter sa base d’utilisateurs. Tirez parti de la puissance du pipeline de données en temps réel et de LiveOps pour impliquer vos utilisateurs avec des éléments, des événements et des promotions personnalisés. Vous avez également la possibilité d’effectuer des tests A/B, de générer des rapports, d’envoyer des notifications push et bien plus encore. 

Nous innovons en permanence et ajoutons de nouvelles fonctionnalités. Pour plus d’informations, consultez [fonctionnalités](https://playfab.com/features/) et pour la tarification, consultez [tarification simple qui s’adapte à vous](https://playfab.com/pricing/).

## <a name="related-links"></a>Liens connexes

* [Guide de développement de jeux Windows 10](https://docs.microsoft.com/windows/uwp/gaming/e2e)
* [Azure pour les jeux](https://azure.microsoft.com/solutions/gaming/)
* [Playfab](https://playfab.com/)
* [Microsoft for Startups](https://startups.microsoft.com)
* [ID@Xbox](https://www.xbox.com/Developers/id)


 

 
