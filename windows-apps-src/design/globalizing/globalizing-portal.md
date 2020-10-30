---
description: Découvrez les avantages de la globalisation et de la localisation de votre application, ainsi que la signification exacte de ces termes.
Search.SourceType: Video
title: Globalisation et localisation
ms.assetid: c0791eec-5bb8-4a13-8977-61d7d98e35ce
label: Intro
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: Windows 10, UWP, globalisation, adaptabilité, localisation
ms.localizationpriority: medium
ms.openlocfilehash: 3ed8c015947b073fce258cd46ff63a8c005d1916
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93033742"
---
# <a name="globalization-and-localization"></a>Globalisation et localisation

Windows est utilisé dans le monde entier par des personnes de diverses cultures, régions et langues. Vos utilisateurs parlent différentes langues dans divers pays et régions. Certains utilisateurs parlent plusieurs langues. Par conséquent, votre application s’exécute sur des configurations qui impliquent de nombreuses permutations de paramètres système pour la langue, la région et la culture. Vous pouvez augmenter le marché potentiel de votre application en la concevant pour qu’elle soit facilement adaptable, à l’aide de la *globalisation* et de la *localisation* .

Cette vidéo fournit une brève introduction à la préparation de votre application pour le monde entier : [Présentation de la globalisation et](https://channel9.msdn.com/Blogs/One-Dev-Minute/Introduction-to-globalization-and-localization)de la localisation.

La **globalisation** est le processus de conception et de développement de votre application de telle sorte qu’elle fonctionne de manière appropriée sur différents marchés globaux (sur des systèmes avec des configurations de langue et de culture différentes) sans nécessiter de modifications ou de personnalisations spécifiques à la culture.

- Prenez en compte la culture lors de la manipulation de chaînes, par exemple, ne modifiez pas la casse des chaînes avant de les comparer.
- Utilisez les calendriers appropriés pour la culture actuelle.
- Utilisez les API de globalisation pour afficher les données mises en forme de manière appropriée pour le pays ou la région, par exemple les nombres, les dates, les heures et les devises.
- Prenez en compte la différence entre les différentes cultures et les règles de classement (tri) du texte et d’autres données.

Votre code doit fonctionner tout aussi bien dans les cultures que vous avez décidé que votre application prendra en charge. Dans l’idéal, votre code fonctionnera tout aussi bien dans le contexte d’un langage, d’une région ou d' *une* culture. Le moyen le plus efficace de globaliser les fonctions de votre application consiste à utiliser le concept de cultures/paramètres régionaux. Une culture/des paramètres régionaux est un ensemble de règles et un ensemble de données spécifiques à une langue et à une zone géographique données. Ces règles et données incluent des informations sur les éléments suivants.

- Classification des caractères
- Système d’écriture
- Mise en forme de la date et de l’heure
- Conventions numériques, de devise, de poids et de mesure
- Tri des règles

>[!NOTE]
> Pour obtenir la liste des noms de paramètres régionaux pris en charge par la version du système d’exploitation Windows, consultez la colonne balise de langue du tableau de l' [annexe a : comportement du produit](/openspecs/windows_protocols/ms-lcid/a9eac961-e77d-41a6-90a5-ce1a8b0cdb9c) dans la [référence LCID (Language code identifier) de Windows](/openspecs/windows_protocols/ms-lcid/70feba9f-294e-491e-b6eb-56532684c37f).

L' **adaptabilité** est le processus qui consiste à préparer une application globalisée pour la localisation et/ou à vérifier que l’application est prête pour la localisation. Le fait de rendre une application localisable de manière appropriée signifie que le processus de localisation ultérieur ne dévoilera pas les défauts fonctionnels de l’application. La propriété la plus essentielle d’une application localisable est que son code exécutable a été séparé proprement des ressources localisables de l’application.

- Les chaînes traduites dans des langages différents peuvent varier considérablement en longueur. Par conséquent, concevez votre interface utilisateur pour prendre en charge différentes longueurs de texte et tailles de police pour les étiquettes et les contrôles de saisie de texte.
- Essayez d’éviter le texte et/ou les éléments sensibles à la culture dans les images.
- Ne codez pas en dur les chaînes et les images dépendantes de la culture dans le code et le balisage de votre application. Au lieu de cela, stockez-les en tant que ressources de type chaîne et image afin qu’elles puissent être adaptées à différents marchés locaux indépendamment des binaires générés de votre application.
- Pseudo-Localisez votre application pour divulguer les problèmes d’adaptabilité.

La **localisation** est le processus consistant à adapter ou traduire les ressources localisables de votre application pour répondre aux exigences linguistiques, culturelles et politiques des marchés locaux spécifiques que l’application est destinée à prendre en charge. Quand vous atteignez l’étape de localisation de votre application, si votre application est localisable, vous n’aurez pas à modifier la logique au cours de ce processus.

- Traduisez les ressources de type chaîne et d’autres ressources de l’application pour le nouveau marché.
- Modifiez les images dépendantes de la culture si nécessaire.
- Les fichiers peuvent également varier en fonction de la région d’un utilisateur, indépendamment de sa langue. Par exemple, une carte peut avoir des bordures différentes en fonction de l’emplacement de l’utilisateur, mais les étiquettes doivent respecter la langue préférée de l’utilisateur.

La plupart des équipes de localisation utilisent des outils spéciaux pour faciliter le processus. Par exemple, en recyclant des traductions de texte périodique.

| Article | Description |
|---------|-------------|
| [Directives relatives à la globalisation](guidelines-and-checklist-for-globalizing-your-app.md) | Concevez et développez votre application de façon à ce qu’elle fonctionne correctement sur les systèmes avec des configurations de langue et de culture différentes. |
| [Comprendre les langues de profil utilisateur et les langues du manifeste de l’application](manage-language-and-region.md) | Cette rubrique définit les termes « liste des langues du profil utilisateur », « liste des langues du manifeste de l’application » et « liste des langues du runtime de l’application ». Nous utiliserons ces termes dans cette rubrique et d’autres sujets dans ce domaine de fonctionnalités. il est donc important de savoir ce qu’ils signifient. |
| [Globaliser vos formats de date/heure/chiffres](use-global-ready-formats.md) | Concevez votre application pour qu’elle soit prête à l’international en mettant en forme de manière appropriée les dates, les heures, les nombres, les numéros de téléphone et les devises. Vous pourrez ensuite adapter votre application à des cultures, régions et langues supplémentaires sur le marché mondial. |
| [Utiliser des modèles de format des dates et heures](use-patterns-to-format-dates-and-times.md) | Utilisez les classes de l’espace de noms [**Windows. Globalization. DateTimeFormatting**](/uwp/api/windows.globalization.datetimeformatting?branch=live) avec des modèles et modèles personnalisés pour afficher les dates et les heures exactement au format de votre choix. |
| [Ajuster la disposition et les polices, et prendre en charge le sens du flux DàG](adjust-layout-and-fonts--and-support-rtl.md) | Concevez votre application pour prendre en charge les dispositions et les polices de plusieurs langues, y compris le sens du déroulement de droite à gauche. |
| [Valeurs de NumeralSystem](glob-numeralsystem-values.md) | Cette rubrique répertorie les valeurs disponibles pour la propriété **NumeralSystem** de différentes classes dans l’espace de noms [**Windows. Globalization**](/uwp/api/windows.globalization?branch=live) . |
| [Rendre votre application localisable](prepare-your-app-for-localization.md) | Une application localisée est une application qui peut être localisée sur d’autres marchés, langues ou régions sans dévoiler les défauts fonctionnels de l’application. La propriété la plus essentielle d’une application localisable est que son code exécutable a été séparé proprement de ses ressources localisables. |
| [Polices internationales](loc-international-fonts.md) | Cette rubrique répertorie les polices disponibles pour les applications Windows localisées dans des langues autres que l’anglais (États-Unis). |
| [Concevoir votre application pour le texte bidirectionnel](design-for-bidi-text.md) | Concevez votre application pour fournir une prise en charge bidirectionnelle du texte (bidirectionnel) afin de pouvoir combiner le script de systèmes d’écriture de gauche à droite et de droite à gauche. |
| [Utiliser le Kit de ressources Multilingual App Toolkit 4.0](use-mat.md) | Multilingual App Toolkit (MAT) 4,0 s’intègre à Microsoft Visual Studio 2017 et versions ultérieures pour fournir des applications Windows avec prise en charge de la traduction, la gestion des fichiers de traduction et les outils de l’éditeur. |
| [Forum aux questions (FAQ) sur l’application multilingue 4,0 & résolution des problèmes](mat-faq-troubleshooting.md) | Cette rubrique fournit des réponses aux questions fréquemment posées et aux problèmes liés à la boîte à outils d’application multilingue (MAT) 4,0. |
| [Utiliser la page de codes UTF-8](use-utf8-code-page.md) | UTF-8 est la page de codes universelle pour l’internationalisation. |
| [Préparer votre application pour la modification de l’ère japonaise](japanese-era-change.md) | Découvrez le changement d’ère du Japon de mai 2019 et comment préparer votre application. |
