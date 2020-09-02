---
ms.assetid: ''
description: Cet article explique comment utiliser la classe SoftwareBitmap avec la bibliothèque open source Vision par ordinateur Library (OpenCV).
title: Traiter les images bitmap avec OpenCV
ms.date: 03/19/2018
ms.topic: article
keywords: Windows 10, UWP, OpenCV, softwarebitmap
ms.localizationpriority: medium
ms.openlocfilehash: a917c4efc8da8fbdabbdc753aacf23724ae17055
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89363802"
---
# <a name="process-bitmaps-with-opencv"></a>Traiter les images bitmap avec OpenCV

Cet article explique comment utiliser la classe **[SoftwareBitmap](/uwp/api/Windows.Graphics.Imaging.SoftwareBitmap)** , qui est utilisée par de nombreuses API Windows Runtime différentes pour représenter des images, avec la bibliothèque open source vision par ordinateur Library (OpenCV), une bibliothèque de code natif Open source qui fournit un large éventail d’algorithmes de traitement d’image. 

Les exemples de cet article vous guident dans la création d’un code natif Windows Runtime composant qui peut être utilisé à partir d’une application UWP, y compris les applications créées à l’aide de C#. Ce composant d’assistance expose une seule méthode, **flou**, qui utilise la fonction de traitement d’image de flou de OpenCV. Le composant implémente des méthodes privées qui obtiennent un pointeur vers la mémoire tampon de données d’image sous-jacente qui peut être utilisée directement par la bibliothèque OpenCV, ce qui simplifie l’extension du composant d’assistance afin d’implémenter d’autres fonctionnalités de traitement OpenCV. 

* Pour une introduction à l’utilisation de **SoftwareBitmap**, consultez [créer, modifier et enregistrer des images bitmap](imaging.md). 
* Pour savoir comment utiliser la bibliothèque OpenCV, accédez à [https://opencv.org](https://opencv.org) .
* Pour savoir comment utiliser le composant d’assistance OpenCV présenté dans cet article avec **[MediaFrameReader](/uwp/api/windows.media.capture.frames.mediaframereader)** pour implémenter le traitement d’image en temps réel des frames à partir d’un appareil photo, consultez [utilisation de OpenCV avec MediaFrameReader](use-opencv-with-mediaframereader.md).
* Pour obtenir un exemple de code complet qui implémente des effets différents, consultez l' [exemple frames de caméra + OpenCV](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraOpenCV) dans le référentiel GitHub des exemples universels Windows.

> [!NOTE] 
> La technique utilisée par le composant OpenCVHelper, décrite en détail dans cet article, exige que les données de l’image soient traitées dans la mémoire de l’UC, et non dans la mémoire du GPU. Ainsi, pour les API qui vous permettent de demander l’emplacement de mémoire des images, telles que la classe **[MediaCapture](/uwp/api/windows.media.capture.mediacapture)** , vous devez spécifier la mémoire de l’UC.

## <a name="create-a-helper-windows-runtime-component-for-opencv-interop"></a>Créer un composant d’assistance Windows Runtime pour l’interopérabilité OpenCV

### <a name="1-add-a-new-native-code-windows-runtime-component-project-to-your-solution"></a>1. Ajouter un nouveau code natif Windows Runtime projet de composant à votre solution

1. Ajoutez un nouveau projet à votre solution dans Visual Studio en cliquant avec le bouton droit sur votre solution dans Explorateur de solutions et en sélectionnant **Ajouter->nouveau projet**. 
2. Sous la catégorie **Visual C++** , sélectionnez **Windows Runtime composant (Windows universel)**. Pour cet exemple, nommez le projet « OpenCVBridge » et cliquez sur **OK**. 
3. Dans la boîte de dialogue **nouveau projet Windows universel** , sélectionnez la cible et la version minimale du système d’exploitation de votre application, puis cliquez sur **OK**.
4. Cliquez avec le bouton droit sur le fichier autogerenated Class1. cpp dans Explorateur de solutions, puis sélectionnez **supprimer**, lorsque la boîte de dialogue de confirmation s’affiche, choisissez **supprimer**. Ensuite, supprimez le fichier d’en-tête Class1. h.
5. Cliquez avec le bouton droit sur l’icône du projet OpenCVBridge et sélectionnez **Ajouter->la classe...**. Dans la boîte de dialogue **Ajouter une classe** , entrez « OpenCVHelper » dans le champ nom de la **classe** , puis cliquez sur **OK**. Du code sera ajouté aux fichiers de classe créés dans une étape ultérieure.

### <a name="2-add-the-opencv-nuget-packages-to-your-component-project"></a>2. ajouter les packages NuGet OpenCV à votre projet de composant

1. Cliquez avec le bouton droit sur l’icône du projet OpenCVBridge dans Explorateur de solutions, puis sélectionnez **gérer les packages NuGet...**
2. Quand la boîte de dialogue Gestionnaire de package NuGet s’ouvre, sélectionnez l’onglet **Parcourir** et tapez « OpenCV. Win » dans la zone de recherche.
3. Sélectionnez « OpenCV. win. Core », puis cliquez sur **installer**. Dans la boîte de dialogue **Aperçu** , cliquez sur **OK**.
4. Utilisez la même procédure pour installer le package « OpenCV. win. ImgProc ».

>[!NOTE]
>OpenCV. win. Core et OpenCV. win. ImgProc ne sont pas régulièrement mis à jour et ne réussissent pas les vérifications de conformité de la Banque. par conséquent, ces packages sont destinés uniquement à l’expérimentation.

### <a name="3-implement-the-opencvhelper-class"></a>3. implémentez la classe OpenCVHelper

Collez le code suivant dans le fichier d’en-tête OpenCVHelper. h. Ce code comprend des fichiers d’en-tête OpenCV pour les packages *Core* et *imgproc* que nous avons installés et déclare trois méthodes qui seront affichées dans les étapes suivantes.

:::code language="cpp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ImagingWin10/cs/OpenCVBridge/OpenCVHelper.h" id="SnippetOpenCVHelperHeader":::

Supprimez le contenu existant du fichier OpenCVHelper. cpp, puis ajoutez les directives include suivantes. 

:::code language="cpp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ImagingWin10/cs/OpenCVBridge/OpenCVHelper.cpp" id="SnippetOpenCVHelperInclude":::

Après les directives include, ajoutez les directives **using** suivantes. 

:::code language="cpp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ImagingWin10/cs/OpenCVBridge/OpenCVHelper.cpp" id="SnippetOpenCVHelperUsing":::

Ensuite, ajoutez la méthode **GetPointerToPixelData** à OpenCVHelper. cpp. Cette méthode prend un **[SoftwareBitmap](/uwp/api/Windows.Graphics.Imaging.SoftwareBitmap)** et, via une série de conversions, obtient une représentation d’interface com des données de pixels via laquelle nous pouvons obtenir un pointeur vers la mémoire tampon de données sous-jacente sous la forme d’un tableau de **caractères** . 

Tout d’abord, un **[BitmapBuffer](/uwp/api/windows.graphics.imaging.bitmapbuffer)** contenant les données de pixels est obtenu en appelant **[LockBuffer](/uwp/api/windows.graphics.imaging.softwarebitmap.lockbuffer)**, en demandant un tampon de lecture/écriture afin que la bibliothèque OpenCV puisse modifier ces données de pixels.  **[CreateReference](/uwp/api/windows.graphics.imaging.bitmapbuffer.CreateReference)** est appelé pour recevoir un objet **[IMemoryBufferReference](/uwp/api/windows.foundation.imemorybufferreference)** . Ensuite, l’interface **IMemoryBufferByteAccess** est castée en **IInspectable**, l’interface de base de toutes les classes Windows Runtime, et **[QueryInterface](/windows/desktop/api/unknwn/nf-unknwn-iunknown-queryinterface(q_))** est appelé pour obtenir une interface com **[IMemoryBufferByteAccess](/previous-versions/mt297505(v=vs.85))** qui nous permettra d’obtenir la mémoire tampon de données en pixels sous la forme d’un tableau de **caractères** . Enfin, remplissez le tableau de **caractères** en appelant **[IMemoryBufferByteAccess :: GetBuffer](/windows/desktop/WinRT/imemorybufferbyteaccess-getbuffer)**. Si l’une des étapes de conversion de cette méthode échoue, la méthode retourne la **valeur false**, ce qui indique que le traitement supplémentaire ne peut pas continuer.

:::code language="cpp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ImagingWin10/cs/OpenCVBridge/OpenCVHelper.cpp" id="SnippetOpenCVHelperGetPointerToPixelData":::

Ajoutez ensuite la méthode **TryConvert** illustrée ci-dessous. Cette méthode prend un **SoftwareBitmap** et tente de la convertir en un objet **mat** , qui est l’objet Matrix que OpenCV utilise pour représenter des mémoires tampons de données image. Cette méthode appelle la méthode **GetPointerToPixelData** définie ci-dessus pour obtenir une représentation sous forme de tableau de **caractères** de la mémoire tampon de données de pixels. En cas de réussite, le constructeur de la classe **mat** est appelé, passant la largeur et la hauteur en pixels obtenues à partir de l’objet **SoftwareBitmap** source. 

> [!NOTE] 
> Cet exemple spécifie la constante CV_8UC4 comme format de pixel pour l’objet **mat** créé. Cela signifie que les **SoftwareBitmap** passés dans cette méthode doivent avoir une valeur de propriété **[BitmapPixelFormat](/uwp/api/windows.graphics.imaging.softwarebitmap.BitmapPixelFormat)** de  **[BGRA8](/uwp/api/Windows.Graphics.Imaging.BitmapPixelFormat)** avec une valeur alpha prémultipliée, l’équivalent de CV_8UC4, pour fonctionner avec cet exemple.

Une copie superficielle de l’objet **mat** créé est retournée à partir de la méthode afin que le traitement ultérieur fonctionne sur le même tampon de données de pixels de données référencé par **SoftwareBitmap** et non une copie de cette mémoire tampon.

:::code language="cpp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ImagingWin10/cs/OpenCVBridge/OpenCVHelper.cpp" id="SnippetOpenCVHelperTryConvert":::

Enfin, cet exemple de classe d’assistance implémente une méthode de traitement d’image unique, **Blur**, qui utilise simplement la méthode **TryConvert** définie ci-dessus pour récupérer un objet de **tapis** représentant la bitmap source et la bitmap cible pour l’opération de flou, puis appelle la méthode **Blur** à partir de la bibliothèque OpenCV imgproc. L’autre paramètre à **blurr** spécifie la taille de l’effet de flou dans les directions X et Y.

:::code language="cpp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ImagingWin10/cs/OpenCVBridge/OpenCVHelper.cpp" id="SnippetOpenCVHelperBlur":::


## <a name="a-simple-softwarebitmap-opencv-example-using-the-helper-component"></a>Exemple simple de OpenCV SoftwareBitmap à l’aide du composant d’assistance
Maintenant que le composant OpenCVBridge a été créé, nous pouvons créer une application C# simple qui utilise la méthode OpenCV **Blur** pour modifier un **SoftwareBitmap**. Pour accéder au composant Windows Runtime à partir de votre application UWP, vous devez d’abord ajouter une référence au composant. Dans Explorateur de solutions, cliquez avec le bouton droit sur le nœud **références** dans votre projet d’application UWP, puis sélectionnez **Ajouter une référence...**. Dans la boîte de dialogue Gestionnaire de références, sélectionnez **projets->solution**. Cochez la case en regard du projet OpenCVBridge et cliquez sur **OK**.

L’exemple de code ci-dessous permet à l’utilisateur de sélectionner un fichier image, puis utilise **[BitmapDecoder](/uwp/api/windows.graphics.imaging.bitmapencoder)** pour créer une représentation **SoftwareBitmap** de l’image. Pour plus d’informations sur l’utilisation de **SoftwareBitmap**, consultez [créer, modifier et enregistrer des images bitmap](./imaging.md).

Comme décrit précédemment dans cet article, la classe **OpenCVHelper** exige que toutes les images **SoftwareBitmap** fournies soient encodées à l’aide du format de pixel BGRA8 avec des valeurs alpha prémultipliées. par conséquent, si l’image n’est pas déjà dans ce format, l’exemple de code appelle **[Convert](/uwp/api/windows.graphics.imaging.softwarebitmap.BitmapAlphaMode)** pour convertir l’image au format attendu.

Ensuite, un **SoftwareBitmap** est créé pour être utilisé comme cible de l’opération de flou. Les propriétés de l’image d’entrée sont utilisées comme arguments du constructeur pour créer une image bitmap avec le format correspondant.

Une nouvelle instance de **OpenCVHelper** est créée, et la méthode **Blur** est appelée, passant les bitmaps source et cible. Enfin, un **SoftwareBitmapSource** est créé pour assigner l’image de sortie à un contrôle d' **image** XAML.

Cet exemple de code utilise des API provenant des espaces de noms suivants, en plus des espaces de noms inclus dans le modèle de projet par défaut.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ImagingWin10/cs/MainPage.OpenCV.xaml.cs" id="SnippetOpenCVMainPageUsing":::

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ImagingWin10/cs/MainPage.OpenCV.xaml.cs" id="SnippetOpenCVBlur":::

## <a name="related-topics"></a>Rubriques connexes

* [Référence des options BitmapEncoder](bitmapencoder-options-reference.md)
* [Métadonnées de l’image](image-metadata.md)
 

 
