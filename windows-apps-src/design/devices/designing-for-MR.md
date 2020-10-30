---
description: Concevez votre application pour qu’elle fonctionne correctement et fonctionne bien dans la réalité mixte.
title: Conception pour la réalité mixte
ms.assetid: ''
label: Designing for Mixed Reality
template: detail.hbs
isNew: true
keywords: Réalité mixte, Hololens, réalité augmentée, point de présence, voix, contrôleur
ms.date: 02/05/2018
ms.topic: article
pm-contact: chigy
design-contact: jeffarn
dev-contact: ''
doc-status: ''
ms.localizationpriority: medium
ms.openlocfilehash: 225b91b20f35c974fca865cc4e94a96efceda84d
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93034372"
---
# <a name="designing-for-mixed-reality"></a>Conception pour la réalité mixte

Concevez votre application pour qu’elle s’affiche correctement en réalité mixte et tirez parti des nouvelles méthodes d’entrée.

## <a name="overview"></a>Vue d’ensemble

La [réalité mixte](https://developer.microsoft.com/windows/mixed-reality/mixed_reality) est le résultat de la fusion du monde physique avec l’univers numérique. Le spectre des expériences de réalité mixte comprend des appareils extrêmes tels que le HoloLens (un appareil qui combine du contenu généré par l’ordinateur avec le monde réel) et, à l’autre, une vue totalement immersive de la réalité virtuelle (telle qu’elle est affichée avec un casque Windows Mixed Reality). Pour obtenir des exemples sur la façon dont les expériences varient, consultez [types d’applications de réalité mixte](https://developer.microsoft.com/windows/mixed-reality/types_of_mixed_reality_apps) .

Presque toutes les applications UWP existantes s’exécutent dans l’environnement de réalité mixte en tant qu’applications 2D sans aucune modification, même si l’expérience de l’utilisateur peut être améliorée en suivant certaines des instructions de cette rubrique.

![Vue de la réalité mixte](images/MR-01.png)

Les casques HoloLens et Windows Mixed Reality prennent en charge les applications qui s’exécutent sur la plateforme UWP, et tous deux prennent en charge deux types d’expérience distincts. 

### <a name="2d-vs-immersive-experience"></a>Interface 2D et immersion

Une application immersive prend la totalité de l’écran visible par l’utilisateur, en la plaçant au centre d’une vue créée par l’application. Par exemple, un jeu immersif peut placer l’utilisateur sur la surface d’une planète, ou une application de guide de visite peut placer l’utilisateur dans un village d’Amérique du Sud. La création d’une application immersif requiert un graphique 3D ou une vidéo stereographic capturée. Les applications immersives sont souvent développées à l’aide d’un moteur de jeu tiers tel qu’Unity ou avec DirectX.

Si vous créez des applications immersifs, visitez le centre de [développement Windows Mixed Reality Center](https://developer.microsoft.com/mixed-reality) pour plus d’informations.

Une application 2D s’exécute en tant que fenêtre plate classique dans la vue de l’utilisateur. Sur le HoloLens, cela signifie qu’une vue a été épinglée au mur ou à un point dans l’espace dans les utilisateurs, qu’il s’agissait d’un salon ou d’un bureau réel. Dans un casque Windows Mixed Reality, l’application est épinglée à un mur dans la [réalité mixte](/windows/mixed-reality/enthusiast-guide/your-mixed-reality-home) (parfois appelée *maison* de la falaise).

![Plusieurs applications s’exécutant en réalité mixte](images/MR-multiple.png)


Ces applications 2D ne prennent pas en charge l’ensemble de la vue : elles sont placées dans celle-ci. Plusieurs applications 2D peuvent exister à la fois dans l’environnement.

Le reste de cette rubrique traite des considérations relatives à la conception de l’expérience 2D.

## <a name="launching-2d-apps"></a>Lancement d’applications 2D

![Menu Démarrer de la réalité mixte](images/MR-start-options.png)

Toutes les applications sont lancées à partir du menu Démarrer, mais il est également possible de créer un objet 3D qui agit comme un lanceur d’applications. Pour plus d’informations, consultez [cette vidéo](https://www.youtube.com/watch?v=TxIslHsEXno) .

## <a name="the-2d-app-input-overview"></a>Vue d’ensemble de l’entrée d’application 2D

Les claviers et souris sont pris en charge sur les plateformes HoloLens et de réalité mixte. Vous pouvez coupler un clavier et une souris directement avec le contrôle HoloLens sur Bluetooth. Les applications de réalité mixte prennent en charge la souris et le clavier connectés à l’ordinateur hôte. Les deux peuvent être utiles dans les situations où un niveau de contrôle plus fin est nécessaire.

D’autres méthodes d’entrée plus naturelles sont également prises en charge, et elles peuvent être particulièrement utiles lorsque l’utilisateur n’est pas assis sur un bureau avec un clavier réel à l’avant, ou lorsqu’un contrôle parfait est nécessaire.

En l’absence de matériel ou de codage supplémentaire, les applications utilisent le point de regard que votre utilisateur regarde en même temps que le pointeur de la souris lors de l’utilisation d’applications 2D. Elle est implémentée comme si le pointeur de la souris était placé sur un point de la scène virtuelle.

Dans une interaction classique, votre utilisateur regardera un contrôle dans votre application, ce qui entraînera sa mise en évidence. L’utilisateur déclenchera une action, à l’aide d’un geste (sur HoloLens) ou d’un Convert ou en donnant une commande vocale. Si l’utilisateur sélectionne un champ d’entrée de texte, le clavier logiciel s’affiche. 


![Le clavier contextuel dans la réalité mixte](images/MR-keyboard.png)

Il est important de noter que toutes ces interactions se produisent automatiquement sans codage supplémentaire de votre part, à la suite de l’exécution sur la plateforme UWP. Les entrées du casque HoloLens et de la réalité mixte s’affichent comme entrée tactile de l’application 2D. Cela signifie que de nombreuses applications UWP s’exécutent et sont utilisables en réalité mixte, par défaut. 

Cela dit, avec un travail supplémentaire, l’expérience peut être améliorée. Par exemple, [le contrôle vocal](https://developer.microsoft.com/windows/mixed-reality/voice_design) peut être particulièrement efficace. Les environnements HoloLens et de réalité mixte prennent en charge les commandes vocales pour le lancement et l’interaction avec les applications, et l’inclusion de la prise en charge vocale s’affiche comme une extension naturelle de cette approche. Pour plus d’informations sur l’ajout d’une prise en charge vocale à votre application UWP, consultez [interactions vocales]( ../input/speech-interactions.md) . 


### <a name="selecting-the-right-controller"></a>Sélection du contrôleur approprié

![Contrôleurs de mouvement de réalité mixte](images/MR-controllers.png)

Plusieurs nouvelles méthodes d’entrée ont été conçues spécialement pour une utilisation avec une réalité mixte, en particulier :

* [Mouvements manuels](https://developer.microsoft.com/windows/mixed-reality/gestures) (HoloLens uniquement, mais utilisé uniquement pour lancer des applications 2d)
* [Prise en charge du boîtier](https://developer.microsoft.com/windows/mixed-reality/hardware_accessories) de la manette (les deux environnements)
* [Appareil de clic](https://developer.microsoft.com/windows/mixed-reality/hardware_accessories) (HoloLens uniquement)
* [Contrôleurs de mouvement](/windows/mixed-reality/motion-controllers) (appareils de réalité mixte uniquement, illustrés ci-dessus).

Ces contrôleurs facilitent l’interaction avec les objets virtuels. Certaines des interactions que vous recevez gratuitement. Par exemple, le mouvement de sélection HoloLens ou le fait de cliquer sur la clé Windows ou le déclencheur du contrôleur de mouvement génère la réponse d’entrée que vous attendez, à nouveau, sans codage de votre part.

À d’autres moments, vous souhaiterez peut-être ajouter du code pour tirer parti des informations supplémentaires et des entrées mises à disposition. Par exemple, les contrôleurs de mouvement peuvent être utilisés pour manipuler des objets avec un niveau de contrôle fin, si vous écrivez du code qui prend la position et appuie sur le bouton pour prendre en compte.

> [!NOTE]
> En Résumé : le principal de guidage doit être de toujours fournir à l’utilisateur une méthode d’entrée aussi naturelle et sans friction que possible.


## <a name="2d-app-design-considerations-functionality"></a>considérations relatives à la conception d’applications 2D : fonctionnalité

Lorsque vous créez une application UWP qui sera potentiellement utilisée sur une plateforme de réalité mixte, il y a plusieurs choses à garder à l’esprit.

* La fonction glisser-déplacer peut ne pas fonctionner correctement lorsqu’elle est utilisée avec les contrôleurs de mouvement, les boîtiers ou les gestes. Si votre application dépend fortement du glisser-déplacer, vous devrez fournir une autre méthode de prise en charge de cette action, telle que la présentation d’une boîte de dialogue confirmant si les objets doivent être déplacés vers un nouvel emplacement.

* Tenez compte de l’évolution du son. Si votre application génère des effets sonores, la source du son semble être l’emplacement épinglé de votre application dans le monde virtuel. À mesure que l’utilisateur quitte l’application, son effet diminue. Pour plus d’informations, consultez [son spatial](/windows/mixed-reality/spatial-sound) .

* Examinez le champ de vue et fournissez intuitivité. Tous les appareils ne fournissent pas un champ d’affichage de grande taille en tant que moniteur d’ordinateur. Consultez le [Frame holographique](https://developer.microsoft.com/windows/mixed-reality/holographic_frame) pour obtenir des détails complets. En outre, l’utilisateur peut être éloigné d’une application en cours d’exécution. Autrement dit, l’application peut apparaître épinglée au mur à un emplacement différent dans le monde (réel ou virtuel). Il se peut que votre application doive attirer l’attention des utilisateurs ou tenir compte du fait que la vue entière n’est pas visible à tout moment. Les notifications Toast sont disponibles, mais une autre façon d’attirer l’attention de l’utilisateur peut être de générer [une alerte sonore](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/SpeechRecognitionAndSynthesis/cs/Scenario_SynthesizeText.xaml.cs) ou de son.

* Une application 2D reçoit automatiquement une [barre d’application](https://developer.microsoft.com/windows/mixed-reality/app_bar_and_bounding_box)  pour permettre à l’utilisateur de les déplacer et de les mettre à l’échelle dans l’environnement virtuel. Les vues peuvent être redimensionnées verticalement ou redimensionnées en conservant les mêmes proportions.


## <a name="2d-app-design-considerations-uiux"></a>considérations relatives à la conception d’applications 2D : UI/UX

* Les contrôles XAML qui implémentent le [système de conception Fluent](/windows/uwp/design/fluent-design-system/) , tels que le [mode navigation](../controls-and-patterns/navigationview.md), et les effets tels que l' [acrylique](../style/acrylic.md) tout fonctionnent particulièrement bien dans les applications de réalité mixte 2D.

* Testez la taille du texte et de Windows de votre application dans un appareil de réalité mixte, ou tout au moins dans le simulateur de réalité mixte. Votre application aura une taille Windows par défaut de 853x480 pixels effectifs. Utilisez des tailles de police supérieures (une taille de point d’environ 32 est recommandée) et lisez [mise à jour de votre application universelle existante pour Hololens](https://developer.microsoft.com/windows/mixed-reality/updating_your_existing_universal_app_for_hololens). La [typographie](https://developer.microsoft.com/windows/mixed-reality/typography) de l’article aborde ce sujet en détail. Quand vous travaillez dans Visual Studio, il existe un paramètre de l’éditeur de conception XAML pour une application « HoloLens 2D » 57 qui fournit une vue avec l’échelle et les dimensions correctes. 

![Le texte affiché dans les applications de réalité mixte doit être volumineux.](images/MR-text.png)

* [Votre point de regard est votre souris](https://developer.microsoft.com/windows/mixed-reality/gaze_targeting). Lorsque l’utilisateur Regarde un objet, il joue le rôle d’un événement de **pointage tactile** . par conséquent, la simple consultation d’un objet peut déclencher une fenêtre contextuelle par inadvertance ou une autre interaction indésirable. Vous devrez peut-être détecter si l’application est en cours d’exécution en réalité mixte et modifier ce comportement. Consultez **prise en charge du runtime** , ci-dessous. 

* Lorsqu’un utilisateur fait un regard sur un contrôleur de mouvement, un événement de **pointage tactile** se produit. Il s’agit d’un **PointerPoint** où **PointerType** est **Touch** , mais **IsInContact** a la **valeur false** . Quand une certaine forme de validation se produit (par exemple, un bouton de sélection est enfoncé, un appareil de l’utilisateur clique sur un déclencheur de mouvement enfoncé ou des têtes de reconnaissance vocale « Select »), une pression **tactile** se produit, avec la **valeur** de **PointerPoint** avec **IsInContact** . Pour plus d’informations sur ces événements d’entrée, consultez [interactions tactiles](../input/touch-interactions.md) .

* N’oubliez pas que le point de regard n’est pas aussi précis que le pointage de la souris. Des cibles de souris ou des boutons plus petits peuvent causer des frustrations pour vos utilisateurs, si bien que les contrôles sont redimensionnés en conséquence. S’ils sont conçus pour la saisie tactile, ils fonctionnent en réalité mixte, mais vous pouvez décider d’agrandir certains boutons lors de l’exécution. Consultez [mise à jour de votre application universelle existante pour Hololens](https://developer.microsoft.com/windows/mixed-reality/updating_your_existing_universal_app_for_hololens).

* Le HoloLens définit la couleur noire comme absence de lumière. Il n’est tout simplement pas rendu, ce qui permet au « monde réel » de s’afficher. Votre application ne doit pas utiliser de noir si cela peut entraîner une confusion. Dans un casque de réalité mixte, le noir est noir.

* HoloLens ne prend pas en charge les thèmes de couleur dans les applications et est bleu par défaut pour garantir une expérience optimale pour les utilisateurs. Pour plus d’informations sur la sélection des couleurs, consultez [cette rubrique](https://developer.microsoft.com/windows/mixed-reality/color,_light_and_materials) qui traite de l’utilisation des couleurs et des documents dans les conceptions de réalité mixte.


## <a name="other-points-to-consider"></a>Autres points à prendre en compte

* Bien que le [pont Desktop](/windows/msix/desktop/source-code-overview) puisse aider à intégrer les applications de bureau existantes (Win32) à Windows 10 et au Microsoft Store, il ne peut pas créer d’applications qui s’exécutent sur HoloLens ou dans une réalité mixte pour l’instant.




## <a name="runtime-support"></a>Prise en charge du runtime

Votre application peut déterminer si elle s’exécute sur un appareil de réalité mixte au moment de l’exécution et l’utiliser comme opportunité de redimensionner des contrôles ou d’autres manières d’optimiser l’application pour une utilisation sur un casque.

Voici un petit morceau de code qui redimensionne le texte à l’intérieur d’un contrôle TextBlock XAML uniquement si l’application est utilisée sur un appareil de réalité mixte.

```csharp

bool isViewingInMR = Windows.ApplicationModel.Preview.Holographic.HolographicApplicationPreview.IsCurrentViewPresentedOnHolographicDisplay();

            if (isViewingInMR)
            {
                // Running on headset, resize the XAML text
                textBlock.Text = "I'm running in Mixed Reality!";
                textBlock.FontSize = 32;
            }
            else
            {
                // Running on desktop
                textBlock.Text = "I'm running on the desktop.";
                textBlock.FontSize = 14;
            }

```





## <a name="related-articles"></a>Articles connexes


* [Limitations actuelles pour les applications utilisant des API à partir de l’interpréteur de commandes](https://developer.microsoft.com/windows/mixed-reality/current_limitations_for_apps_using_apis_from_the_shell)
* [Création d’applications 2D](https://developer.microsoft.com/windows/mixed-reality/building_2d_apps)
* [HoloLens : création d’applications 2D UWP pour Microsoft HoloLens](https://channel9.msdn.com/Events/Build/2016/B854)
* [XAML conditionnel](../../debug-test-perf/conditional-xaml.md)
