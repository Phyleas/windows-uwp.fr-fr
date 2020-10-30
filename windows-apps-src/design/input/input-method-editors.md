---
description: Un éditeur de méthode d’entrée (IME) est un composant logiciel qui permet à un utilisateur d’entrer du texte dans une langue qui ne peut pas être représentée facilement sur un clavier AZERTY standard.
title: Éditeurs de méthode d’entrée (IME)
label: Input Method Editors (IME)
template: detail.hbs
keywords: IME, éditeur de méthode d’entrée, entrée, interaction
ms.date: 07/24/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 1f70f08e61df1b609a0ab505e35ef314a90a91fd
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93030082"
---
# <a name="input-method-editors-ime"></a>Éditeurs de méthode d’entrée (IME)

Un éditeur de méthode d’entrée (IME) est un composant logiciel qui permet à un utilisateur d’entrer du texte dans une langue qui ne peut pas être représentée facilement sur un clavier AZERTY standard. Cela est généralement dû au nombre de caractères dans la langue écrite de l’utilisateur, tels que les différentes langues d’Asie orientale.

Au lieu que chaque caractère apparaisse sur une seule touche du clavier, un utilisateur tape des combinaisons de touches qui sont interprétées par l’IME. L’IME génère soit le caractère qui correspond au jeu de séquences de touches, soit une liste de caractères candidats à choisir. Le caractère sélectionné est ensuite inséré dans le contrôle d’édition avec lequel l’utilisateur interagit.

> [!NOTE]
> Les éditeurs IME peuvent prendre en charge des claviers matériels et des claviers tactiles et à l’écran.

Votre application n’a pas besoin d’interagir directement avec l’IME. L’IME est intégré au système, comme le clavier tactile. Si votre application comporte une entrée de texte et que vous envisagez de prendre en charge l’entrée de texte dans les langues qui nécessitent un IME, vous devez tester l’expérience client de bout en bout pour la saisie de texte. Cela vous permet de résoudre les problèmes, tels que l’ajustement de votre interface utilisateur, de sorte qu’elle ne soit pas bloqués par le clavier tactile ou la fenêtre candidate IME.

## <a name="creating-an-ime"></a>Création d’un IME

Pour offrir une expérience d’entrée exceptionnelle pour tous les utilisateurs, Microsoft produit des éditeurs de la boîte de données qui s’accompagnent d’une variété de langues.

Outre les éditeurs de la boîte aux IME, vous pouvez créer vos propres IME personnalisés que les utilisateurs peuvent installer et utiliser comme un IME intégré.

Tous les éditeurs de logiciels IME s’exécutent dans le système Windows, qui est renforcé pour arrêter les éditeurs de logiciels malveillants et améliorer la sécurité et l’expérience utilisateur de tous les éditeurs.

Les éditeurs IME personnalisés peuvent être liés au clavier tactile par défaut et utiliser leur disposition afin que les utilisateurs finaux puissent utiliser leur IME avec le clavier tactile. Toutefois, vous ne pouvez pas fournir votre propre clavier tactile indépendant et certaines fonctions des IME intégrés pour les claviers tactiles ne sont pas disponibles pour les IME personnalisés.

## <a name="requirements-for-imes"></a>Conditions requises pour les éditeurs IME

Un IME tiers doit respecter les conditions suivantes :

- Doit être signé numériquement
- Doit être compatible avec [Text Services Framework (TSF)](/windows/win32/tsf/text-services-framework) , avec les indicateurs IME appropriés définis correctement
- Doit suivre les instructions décrites dans [Configuration requise de l’éditeur de méthode d’entrée (IME)](input-method-editor-requirements.md) et [concevoir et coder des applications Windows](../index.md)

L’exécution d’un IME tiers qui ne répond pas à ces exigences est bloquée.

> [!NOTE]
> Les éditeurs IME personnalisés hérités peuvent s’exécuter dans les applications de bureau, mais ils sont bloqués dans les applications Windows.

En outre, Windows Defender supprime les éditeurs de logiciels malveillants du système. Pour cette raison, il est important de vous familiariser avec les exigences de codage IME. Pour plus d’informations, consultez spécifications de l' [éditeur de méthode d’entrée (IME)](input-method-editor-requirements.md).

## <a name="design-guidelines-for-imes"></a>Instructions de conception pour les IME

Pour plus d’informations sur les meilleures pratiques et les instructions de conception pour les éditeurs IME, consultez la [Configuration requise pour l’éditeur de méthode d’entrée](input-method-editor-requirements.md) . En général, toutes les interfaces utilisateur IME doivent :

- Suivre les instructions d’expérience utilisateur pour les applications Windows Runtime
- Évitez les expériences modales et affichez la fenêtre IME uniquement lorsque cela est nécessaire
- inclure les icônes en noir et blanc uniquement

## <a name="related-topics"></a>Rubriques connexes

- [Spécifications de l’éditeur de méthode d’entrée (IME)](input-method-editor-requirements.md)
- [ITfFnGetPreferredTouchKeyboardLayout](/windows/win32/api/ctffunc/nn-ctffunc-itffngetpreferredtouchkeyboardlayout)
- [ITfCompartmentEventSink](/windows/win32/api/msctf/nn-msctf-itfcompartmenteventsink)
- [ITfThreadMgrEx::GetActiveFlags](/windows/win32/api/msctf/nf-msctf-itfthreadmgrex-getactiveflags)
- [ITfContextView::GetWnd](/windows/win32/api/msctf/nf-msctf-itfcontextview-getwnd)
- [TF_INPUTPROCESSORPROFILE](/windows/win32/api/msctf/ns-msctf-tf_inputprocessorprofile)
- [SendInput](/windows/win32/api/winuser/nf-winuser-sendinput)
