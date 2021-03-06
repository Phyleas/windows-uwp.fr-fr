---
description: Un contrôle de progression offre un retour à l’utilisateur lorsqu’une longue opération est en cours.
title: Recommandations en matière de contrôles de progression
ms.assetid: FD53B716-C43D-408D-8B07-522BC1F3DF9D
label: Progress controls
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp
pm-contact: kisai
design-contact: jeffarn
dev-contact: mitra
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 24ba67dfa51c039055cc5bc4cb31d4aff4de6765
ms.sourcegitcommit: b99fe39126fbb457c3690312641f57d22ba7c8b6
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/04/2020
ms.locfileid: "96603884"
---
# <a name="progress-controls"></a>Contrôles de progression

Un contrôle de progression offre un retour à l’utilisateur lorsqu’une longue opération est en cours. Cela peut signifier que l’utilisateur ne peut pas interagir avec l’application lorsque l’indicateur de progression est visible et peut également indiquer le temps d’attente en fonction de l’indicateur utilisé.

**Obtenir la bibliothèque d’interface utilisateur Windows**

:::row:::
   :::column:::
      ![Logo WinUI](images/winui-logo-64x64.png)
   :::column-end:::
   :::column span="3":::
      Le contrôle **ProgressBar** est inclus dans la bibliothèque d’interface utilisateur Windows, package NuGet qui contient de nouveaux contrôles et fonctionnalités d’interface utilisateur destinés aux applications Windows. Pour plus d’informations, notamment des instructions d’installation, consultez la [bibliothèque d’interface utilisateur Windows](/uwp/toolkits/winui/).
   :::column-end:::
   :::column:::

   :::column-end:::
:::row-end:::

> **API de la bibliothèque d’interface utilisateur Windows :** [classe ProgressBar](/uwp/api/Microsoft.UI.Xaml.Controls.ProgressBar), [propriété IsIndeterminate](/uwp/api/Microsoft.ui.xaml.controls.progressbar.isindeterminate), [classe ProgressRing](/uwp/api/Microsoft.UI.Xaml.Controls.ProgressRing), [propriété IsActive](/uwp/api/Microsoft.ui.xaml.controls.progressring.isactive)
>
> **API de plateforme :** [classe ProgressBar](/uwp/api/Windows.UI.Xaml.Controls.ProgressBar), [propriété IsIndeterminate](/uwp/api/windows.ui.xaml.controls.progressbar.isindeterminate), [classe ProgressRing](/uwp/api/Windows.UI.Xaml.Controls.ProgressRing), [propriété IsActive](/uwp/api/windows.ui.xaml.controls.progressring.isactive)

> [!NOTE]
> Il existe deux versions des contrôles ProgressBar et ProgressRing : l’une dans la plateforme représentée par l’espace de noms Windows.UI.Xaml et l’autre dans la bibliothèque d’interface utilisateur Windows, l’espace de noms Microsoft.UI.Xaml. Bien que les API pour ProgressRing et ProgressBar soient identiques, les apparences du contrôle diffèrent entre les deux versions. Ce document montre des images de la version la plus récente de la bibliothèque d’interface utilisateur Windows.
Tout au long de ce document, nous allons utiliser l’alias **muxc** en XAML pour représenter les API de la bibliothèque d’interface utilisateur Windows que nous avons incluses dans notre projet. Nous l’avons ajouté à notre élément [Page](/uwp/api/windows.ui.xaml.controls.page) :

```xaml
xmlns:muxc="using:Microsoft.UI.Xaml.Controls"
```

Dans le code-behind, nous allons également utiliser l’alias **muxc** en C# pour représenter les API de la bibliothèque d’interface utilisateur Windows que nous avons incluses dans notre projet. Nous avons ajouté cette instruction **using** en haut du fichier :

```csharp
using muxc = Microsoft.UI.Xaml.Controls;
```

```vb
Imports muxc = Microsoft.UI.Xaml.Controls
```

## <a name="types-of-progress"></a>Types de progression

Deux contrôles permettent d’indiquer à l’utilisateur qu’une opération est en cours d’exécution : ProgressBar ou ProgressRing. ProgressBar et ProgressRing ont tous deux des états qui communiquent si l’utilisateur peut interagir avec l’application, ou non. 

-   L’état *déterminé* de ProgressBar et de ProgressRing indique le pourcentage d’achèvement d’une tâche. Ce contrôle doit être utilisé au cours d’une opération dont la durée est connue, mais dont la progression ne doit pas bloquer l’interaction de l’utilisateur avec l’application.
-   L’état *indéterminé* de ProgressBar indique qu’une opération est en cours, qu’elle ne bloque pas l’interaction de l’utilisateur avec l’application et que son heure de fin est inconnue.
-   L’état *indéterminé* de ProgressRing indique qu’une opération est en cours, qu’elle bloque l’interaction de l’utilisateur avec l’application et que son heure de fin est inconnue.


De plus, un contrôle de progression est en lecture seule et n’est pas interactif. Ce qui signifie que l’utilisateur ne peut pas appeler ou utiliser ces contrôles directement.

|Control|Afficher|
|---|---|
| ProgressBar indéterminé | ![ProgressBar - Indéterminé](images/progressbar-indeterminate.gif) |
| ProgressBar déterminé | ![ProgressBar - Déterminé](images/progressbar-determinate.png)|
| ProgressRing indéterminé | ![État de ProgressRing - Indéterminé](images/progressring-indeterminate.gif)|
| ProgressRing - Déterminé | ![État de ProgressRing - Déterminé](images/progress_ring.jpg)|


## <a name="examples"></a>Exemples

<table>
<th align="left">Galerie de contrôles XAML<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Si vous disposez de l’application <strong style="font-weight: semi-bold">Galerie de contrôles XAML</strong>, cliquez ici pour ouvrir l’application et voir l’objet <a href="xamlcontrolsgallery:/item/ProgressBar">ProgressBar</a> ou <a href="xamlcontrolsgallery:/item/ProgressRing">ProgressRing</a> en action.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenir l’application Galerie de contrôles XAML (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtenir le code source (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="when-to-use-each-control"></a>Quand utiliser chaque contrôle

Il n’est pas toujours évident de savoir quel contrôle ou quel état (déterminé ou indéterminé) utiliser lorsque vous essayez d’indiquer que quelque chose se produit. Parfois, une tâche est suffisamment évidente pour ne pas nécessiter de contrôle de progression, et parfois, même si un contrôle de progression est utilisé, une ligne de texte reste nécessaire pour indiquer à l’utilisateur le type d’opération en cours.

### <a name="progressbar"></a>ProgressBar
-   **Le contrôle a-t-il une durée définie ou une fin prévisible ?**

    Utilisez un contrôle ProgressBar déterminé et mettez à jour le pourcentage ou la valeur en conséquence.

-   **L’utilisateur peut-il poursuivre sans avoir à surveiller la progression de l’opération ?**

    Quand un contrôle ProgressBar est utilisé, l’interaction est non modale, ce qui signifie généralement que l’utilisateur n’est pas bloqué tant que cette opération n’est pas terminée et peut continuer à utiliser les autres fonctionnalités de l’application dans l’intervalle.

-   **Mots clés**

    Si ces mots clés s’appliquent à votre opération ou si vous affichez du texte pendant sa progression qui correspond à ces mots clés, envisagez d’utiliser un contrôle ProgressBar :

    - *Chargement...*
    - *Récupération*
    - *En cours...*

### <a name="progressring"></a>ProgressRing

-   **L’opération impose-t-elle à l’utilisateur d’attendre avant de continuer ?**

    Si une opération nécessite une interaction complète (ou très importante) avec l’application qui implique d’attendre son achèvement, le ProgressRing avec l’état Indéterminé est le meilleur choix.

    -   **Le contrôle a-t-il une durée définie ou une fin prévisible ?**

    Utilisez un ProgressRing avec l’état Déterminé si vous voulez que le visuel soit un anneau plutôt qu’une barre, et mettez à jour le pourcentage ou la valeur en conséquence. 

-   **L’application attend-elle que l’utilisateur termine une tâche ?**

    Si tel est le cas, utilisez un ProgressRing avec l’état Indéterminé, car il sert à indiquer à l’utilisateur un temps d’attente inconnu.

-   **Mots clés**

    Si ces mots clés s’appliquent à votre opération ou si vous affichez du texte pendant sa progression qui correspond à ces mots clés, envisagez d’utiliser un contrôle ProgressRing :

    - *Actualisation*
    - *Connexion en cours...*
    - *Connexion...*

### <a name="no-progress-indication-necessary"></a>Aucune indication de progression n’est nécessaire
-   **L’utilisateur a-t-il besoin de savoir que quelque chose se produit ?**

    Par exemple, si l’application exécute un téléchargement en arrière-plan et que l’utilisateur n’en est pas l’initiateur, il n’est pas nécessaire de l’en informer.

-   **L’opération est-elle une activité en arrière-plan qui ne bloque pas celle de l’utilisateur et dont l’intérêt est minime (mais tout de même existant) pour l’utilisateur ?**

    Utilisez du texte lorsque votre application effectue des tâches qui n’ont pas à être visibles tout le temps, mais dont vous souhaitez quand même indiquer le statut.

-   **L’utilisateur se soucie-t-il uniquement de l’achèvement de l’opération ?**

    Il est parfois préférable d’afficher un avis seulement une fois que l’opération a abouti ou d’indiquer par un élément visuel que l’opération a été exécutée immédiatement pour apporter les touches finales en arrière-plan.

## <a name="progress-controls-best-practices"></a>Meilleures pratiques en termes de contrôles de progression

Il est parfois préférable de voir des représentations visuelles indiquant quand et comment utiliser ces différents contrôles de progression :

**ProgressBar - Déterminé**

![Exemple de contrôle ProgressBar déterminé.](images/progress-bar-determinate-example.png)

Le premier exemple est le contrôle ProgressBar déterminé. Un contrôle ProgressBar est préférable lorsque la durée de l’opération est connue, lors de l’installation, lors du téléchargement, lors de la configuration, etc.

**ProgressBar - Indéterminé**

![Exemple de contrôle ProgressBar indéterminé.](images/progress-bar-indeterminate-example.png)

Utilisez un contrôle ProgressBar indéterminé lorsque la durée de l’opération est inconnue. Les contrôles ProgressBar indéterminés s’appliquent également en cas de remplissage d’une liste virtualisée et la création d’une transition visuelle fluide d’un contrôle ProgressBar indéterminé vers un contrôle ProgressBar déterminé.

-   **L’opération se trouve-t-elle dans une collection virtualisée ?**

    Si tel est le cas, ne placez pas d’indicateur de progression dès l’affichage de chaque élément de liste. Au lieu de cela, utilisez un contrôle ProgressBar et placez-le en haut de la collection d’éléments en cours de chargement, afin d’indiquer que les éléments sont en cours de recherche.

**ProgressRing - Indéterminé**

![Exemple de contrôle ProgressRing indéterminé](images/PR_IndeterminateExample.png)

Le contrôle ProgressRing indéterminé est utilisé quand toute autre interaction de l’utilisateur avec l’application est bloquée ou que l’application attend l’entrée de l’utilisateur pour continuer. L’exemple « Connexion en cours... » ci-dessus est un scénario idéal pour le contrôle ProgressRing. L’utilisateur ne peut pas continuer à utiliser l’application tant que la connexion n’a pas été établie.

**ProgressRing - Déterminé**

![Exemple de ProgressRing avec l’état Déterminé](images/progress_ring_determinate_example.png)

Quand la durée de l’opération est connue et que le visuel Anneau est souhaité, un ProgressRing avec l’état Déterminé est préférable lors de l’installation, du téléchargement, de la configuration, etc.

## <a name="customizing-a-progress-control"></a>Personnalisation d’un contrôle de progression

Les deux contrôles de progression sont assez simples ; toutefois, certaines fonctionnalités visuelles des contrôles ne sont pas faciles à personnaliser.

**Dimensionnement du contrôle ProgressRing**

Le contrôle ProgressRing peut être agrandi à la taille que vous souhaitez, mais ne peut pas être réduit à moins de 20x20epx. Pour redimensionner un contrôle ProgressRing, vous devez définir sa hauteur et sa largeur. Si seule la hauteur ou largeur est définie, le contrôle suppose un dimensionnement minimal (20x20epx). À l’inverse, si la hauteur et la largeur sont définies sur deux tailles différentes, la plus d’entre elles sera prise en considération.
Pour garantir que le contrôle ProgressRing est approprié pour vos besoins, définissez la hauteur et la largeur sur la même valeur :

```XAML
<ProgressRing Height="100" Width="100"/>
```

Pour rendre votre contrôle ProgressRing visible et l’animer, définissez la propriété IsActive sur True :

```XAML
<ProgressRing IsActive="True" Height="100" Width="100"/>
```

```C#
progressRing.IsActive = true;
```

**Couleur des contrôles de progression**

Par défaut, la couleur principale des contrôles de progression est définie sur la couleur d’accentuation du système. Pour modifier cette couleur,, il suffit de modifier la propriété de premier plan sur chaque contrôle.

```XAML
<ProgressRing IsActive="True" Height="100" Width="100" Foreground="Blue"/>
<muxc:ProgressBar Width="100" Foreground="Green"/>
```

La modification de la couleur de premier plan du contrôle de progression a pour effet de changer la couleur de remplissage de l’anneau. La propriété de premier plan du contrôle ProgressBar change la couleur de remplissage de la barre. Pour modifier la partie vide de la barre, il suffit de remplacer la propriété d’arrière-plan.

**Affichage d’un curseur d’attente**

Il est parfois préférable d’afficher brièvement un curseur d’attente quand l’application ou l’opération a besoin d’un temps de réflexion et que vous devez indiquer à l’utilisateur que l’application ou la zone où le curseur d’attente est visible ne doit faire l’objet d’aucune interaction tant que ce dernier est affiché.

```C#
Window.Current.CoreWindow.PointerCursor = new Windows.UI.Core.CoreCursor(Windows.UI.Core.CoreCursorType.Wait, 10);
```

## <a name="get-the-sample-code"></a>Obtenir l’exemple de code

- [Exemple de Galerie de contrôles XAML](https://github.com/Microsoft/Xaml-Controls-Gallery) : tous les contrôles XAML dans un format interactif.

## <a name="related-articles"></a>Articles connexes

- [Classe ProgressBar](/uwp/api/Windows.UI.Xaml.Controls.ProgressBar)
- [Classe ProgressRing](/uwp/api/Windows.UI.Xaml.Controls.ProgressRing)
