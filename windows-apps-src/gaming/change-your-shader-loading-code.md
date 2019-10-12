---
title: Comparer le pipeline nuanceur d’OpenGL ES 2.0 à celui de Direct3D
description: D’un point de vue conceptuel, le pipeline nuanceur de Direct3D 11 est très similaire à celui d’OpenGL ES 2.0.
ms.assetid: 3678a264-e3f9-72d2-be91-f79cd6f7c4ca
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, uwp, jeux, opengl, direct3d, pipeline nuanceur
ms.localizationpriority: medium
ms.openlocfilehash: 7a35102fed9993ca37afa1d1f47850427235ed49
ms.sourcegitcommit: cbd900f350569a3901086a44b2d5007bb6fb7bed
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/11/2019
ms.locfileid: "72276294"
---
# <a name="compare-the-opengl-es-20-shader-pipeline-to-direct3d"></a>Comparer le pipeline nuanceur d’OpenGL ES 2.0 à celui de Direct3D




**API importantes**

-   [Étape assembleur d’entrée](https://docs.microsoft.com/windows/desktop/direct3d11/d3d10-graphics-programming-guide-input-assembler-stage)
-   [Étape vertex-shader](https://docs.microsoft.com/previous-versions/bb205146(v=vs.85))
-   [Étape nuanceur de pixels](https://docs.microsoft.com/previous-versions/bb205146(v=vs.85))

D’un point de vue conceptuel, le pipeline nuanceur de Direct3D 11 est très similaire à celui d’OpenGL ES 2.0. En termes de conception d’API, les composants majeurs de création et de gestion des étapes de nuanceur appartiennent aux deux interfaces principales, [**ID3D11Device1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11device1) et [**ID3D11DeviceContext1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1). Cette rubrique tente d’établir une correspondance dans ces interfaces entre les modèles courants d’API du pipeline nuanceur d’OpenGL ES 2.0 et ceux de Direct3D 11.

## <a name="reviewing-the-direct3d-11-shader-pipeline"></a>Exploration du pipeline nuanceur de Direct3D 11


Les objets nuanceurs sont créés par des méthodes dans l’interface [**ID3D11Device1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11device1), telles que [**ID3D11Device1::CreateVertexShader**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createvertexshader) et [**ID3D11Device1::CreatePixelShader**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createpixelshader).

Le pipeline graphique de Direct3D 11 est géré par des instances de l’interface [**ID3D11DeviceContext1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1) et comprend les étapes suivantes :

-   [Étape de l’assembleur d’entrée](https://docs.microsoft.com/windows/desktop/direct3d11/d3d10-graphics-programming-guide-input-assembler-stage). Cette étape fournit des données (triangles, lignes et points) au pipeline. Les méthodes [**ID3D11DeviceContext1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1) qui prennent en charge cette étape portent le préfixe « IA ».
-   [Étape du nuanceur de vertex](https://docs.microsoft.com/previous-versions/bb205146(v=vs.85)). Cette étape traite les vertex, généralement par le biais d’opérations de type transformation, application d’apparence et éclairage. Un nuanceur de vertex prend toujours un seul vertex d’entrée et produit un seul vertex de sortie. Les méthodes [**ID3D11DeviceContext1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1) qui prennent en charge cette étape sont précédées de « vs ».
-   [Étape de sortie de flux](https://docs.microsoft.com/windows/desktop/direct3d11/d3d10-graphics-programming-guide-output-stream-stage). Cette étape diffuse des données primitives du pipeline dans la mémoire vers le rastériseur. Les données peuvent être diffusées vers la sortie et/ou dans le rastériseur. Les données diffusées dans la mémoire peuvent être renvoyées dans le pipeline en tant que données d’entrée ou lues par le processeur. Les méthodes [**ID3D11DeviceContext1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1) qui prennent en charge cette étape portent le préfixe « so ».
-   [Étape du rastériseur](https://docs.microsoft.com/windows/desktop/direct3d11/d3d10-graphics-programming-guide-rasterizer-stage). Le rastériseur extrait les primitives, les prépare pour le nuanceur de pixels et détermine le mode d’invocation des nuanceurs de pixels. Vous pouvez désactiver la pixellisation en indiquant au pipeline qu’il n’existe aucun nuanceur de pixels (définissez l’étape nuanceur de pixels sur NULL avec [**ID3D11DeviceContext ::P ssetshader**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-pssetshader)) et désactivez les tests de profondeur et de stencil (définissez DepthEnable et STENCILENABLE sur false dans [ **D3D11 @ no__t-4DEPTH @ no__t-5STENCIL @ no__t-6DESC**](https://docs.microsoft.com/windows/desktop/api/d3d11/ns-d3d11-d3d11_depth_stencil_desc)). Quand la rastérisation est désactivée, les compteurs du pipeline relatif à la rastérisation ne sont pas mis à jour.
-   [Étape du nuanceur de pixels](https://docs.microsoft.com/previous-versions/bb205146(v=vs.85)). Cette étape reçoit les données interpolées pour une primitive et génère des données par pixel telles que la couleur. Les méthodes [**ID3D11DeviceContext1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1) qui prennent en charge cette étape portent le préfixe « PS ».
-   [Étape de fusion de sortie](https://docs.microsoft.com/windows/desktop/direct3d11/d3d10-graphics-programming-guide-output-merger-stage). Cette étape combine plusieurs types de données de sortie (valeurs du nuanceur de pixels, informations de profondeur et gabarit) avec le contenu de la cible de rendu et des tampons de profondeur/gabarit pour générer le résultat final du pipeline. Les méthodes [**ID3D11DeviceContext1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1) qui prennent en charge cette étape portent le préfixe « OM ».

(Il existe également des étapes pour les nuanceurs de géométrie, les nuanceurs de coque, les tesselators et les nuanceurs de domaine, mais étant donné qu’ils n’ont pas de analogues dans OpenGL ES 2,0, nous ne les aborderons pas ici.) Pour obtenir la liste complète des méthodes pour ces étapes, reportez-vous aux pages de référence [**ID3D11DeviceContext**](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11devicecontext) et [**ID3D11DeviceContext1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1) . **ID3D11DeviceContext1** développe **ID3D11DeviceContext** pour Direct3D 11.

## <a name="creating-a-shader"></a>Création d’un nuanceur


Dans Direct3D, les ressources de nuanceur ne sont pas créées avant leur compilation et leur chargement. En réalité, elles sont créées lors du chargement du HLSL. Par conséquent, il n’existe pas de fonction directement analogue à glCreateShader, qui crée une ressource de nuanceur initialisée d’un type spécifique (par exemple, GL @ no__t-0VERTEX @ no__t-1SHADER ou GL @ no__t-2FRAGMENT @ no__t-3SHADER). Les nuanceurs sont créés après le chargement du HLSL avec des fonctions spécifiques telles que [**ID3D11Device1::CreateVertexShader**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createvertexshader) et [**ID3D11Device1::CreatePixelShader**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createpixelshader), et adoptent le fichier HLSL compilé pour définir leurs paramètres.

| OpenGL ES 2.0  | Direct3D 11                                                                                                                                                                                                                                                             |
|----------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| glCreateShader | Appelez [**ID3D11Device1::CreateVertexShader**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createvertexshader) et [**ID3D11Device1::CreatePixelShader**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createpixelshader) après avoir chargé l’objet nuanceur compilé dans un tampon, et transmettez-leur le tampon. |

 

## <a name="compiling-a-shader"></a>Compilation d’un nuanceur


Les nuanceurs Direct3D doivent être précompilés en tant que fichiers d’objets de nuanceur compilés (. CSO) dans les applications plateforme Windows universelle (UWP) et chargés à l’aide de l’une des API de fichier Windows Runtime. (Les applications de bureau peuvent compiler les nuanceurs à partir de fichiers texte ou de chaînes au moment de l’exécution.) Les fichiers CSO sont créés à partir de tous les fichiers. HLSL qui font partie de votre projet Microsoft Visual Studio et conservent les mêmes noms, uniquement avec l’extension de fichier. CSO. Assurez-vous qu’ils sont inclus dans votre package au moment de la publication.

| OpenGL ES 2.0                          | Direct3D 11                                                                                                                                                                   |
|----------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| glCompileShader                        | Non applicable Compilez les nuanceurs en fichiers .cso dans Visual Studio et incluez-les dans votre package.                                                                                     |
| Utilisation de glGetShaderiv pour vérifier le statut de la compilation | Non applicable Affichez le fichier de compilation dans le compilateur FX (FXC) de Visual Studio pour vérifier les éventuelles erreurs de compilation. Si la compilation est réussie, un fichier CSO est créé. |

 

## <a name="loading-a-shader"></a>Chargement d’un nuanceur


Comme indiqué dans la section Création d’un nuanceur, Direct3D 11 crée le nuanceur quand le fichier CSO correspondant est chargé dans un tampon et transmis à l’une des méthodes du tableau suivant.

| OpenGL ES 2.0 | Direct3D 11                                                                                                                                                                                                                           |
|---------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ShaderSource  | Appelez [**ID3D11Device1::CreateVertexShader**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createvertexshader) et [**ID3D11Device1::CreatePixelShader**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createpixelshader) une fois que l’objet nuanceur compilé est correctement chargé. |

 

## <a name="setting-up-the-pipeline"></a>Configuration du pipeline


OpenGL ES 2.0 inclut l’objet « programme de nuanceur », qui contient plusieurs nuanceurs pour l’exécution. Les nuanceurs individuels sont rattachés à l’objet programme de nuanceur. Toutefois, dans Direct3D 11, vous travaillez directement avec le contexte de rendu ([**ID3D11DeviceContext1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1)) sur lequel vous créez des nuanceurs.

| OpenGL ES 2.0   | Direct3D 11                                                                                   |
|-----------------|-----------------------------------------------------------------------------------------------|
| glCreateProgram | Non applicable Direct3D 11 n’utilise pas l’abstraction de l’objet programme de nuanceur.                          |
| glLinkProgram   | Non applicable Direct3D 11 n’utilise pas l’abstraction de l’objet programme de nuanceur.                          |
| glUseProgram    | Non applicable Direct3D 11 n’utilise pas l’abstraction de l’objet programme de nuanceur.                          |
| glGetProgramiv  | Utilisez la référence à [**ID3D11DeviceContext1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1) que vous avez créée. |

 

Créez une instance de [**ID3D11DeviceContext1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1) et [**ID3D11Device1**](https://docs.microsoft.com/windows/desktop/api/d3d11_2/nn-d3d11_2-id3d11device2) avec la méthode [**D3D11CreateDevice**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-d3d11createdevice) statique.

``` syntax
Microsoft::WRL::ComPtr<ID3D11Device1>          m_d3dDevice;
Microsoft::WRL::ComPtr<ID3D11DeviceContext1>  m_d3dContext;

// ...

D3D11CreateDevice(
  nullptr, // Specify nullptr to use the default adapter.
  D3D_DRIVER_TYPE_HARDWARE,
  nullptr,
  creationFlags, // Set set debug and Direct2D compatibility flags.
  featureLevels, // List of feature levels this app can support.
  ARRAYSIZE(featureLevels),
  D3D11_SDK_VERSION, // Always set this to D3D11_SDK_VERSION for UWP apps.
  &device, // Returns the Direct3D device created.
  &m_featureLevel, // Returns feature level of device created.
  &m_d3dContext // Returns the device's immediate context.
);
```

## <a name="setting-the-viewports"></a>Définition d’une ou plusieurs fenêtres d’affichage


La définition d’une fenêtre d’affichage dans Direct3D 11 est très similaire à la méthode utilisée dans OpenGL ES 2.0. Dans Direct3D 11, appelez [**ID3D11DeviceContext :: RSSetViewports**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-rssetviewports) avec un [**CD3D11 @ no__t-4VIEWPORT**](https://docs.microsoft.com/previous-versions/windows/desktop/legacy/jj151722(v=vs.85))configuré.

Direct3D 11 : Définition d’une fenêtre d’affichage.

``` syntax
CD3D11_VIEWPORT viewport(
        0.0f,
        0.0f,
        m_d3dRenderTargetSize.Width,
        m_d3dRenderTargetSize.Height
        );
m_d3dContext->RSSetViewports(1, &viewport);
```

| OpenGL ES 2.0 | Direct3D 11                                                                                                                                  |
|---------------|----------------------------------------------------------------------------------------------------------------------------------------------|
| glViewport    | [**CD3D11 @ no__t-2VIEWPORT**](https://docs.microsoft.com/previous-versions/windows/desktop/legacy/jj151722(v=vs.85)), [ **ID3D11DeviceContext :: RSSetViewports**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-rssetviewports) |

 

## <a name="configuring-the-vertex-shaders"></a>Configuration des nuanceurs de vertex


La configuration d’un nuanceur de vertex dans Direct3D 11 est effectuée après le chargement du nuanceur. Les uniformes sont transmis sous forme de mémoires tampons constantes à l’aide de [**ID3D11DeviceContext1::VSSetConstantBuffers1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nf-d3d11_1-id3d11devicecontext1-vssetconstantbuffers1).

| OpenGL ES 2.0                    | Direct3D 11                                                                                               |
|----------------------------------|-----------------------------------------------------------------------------------------------------------|
| glAttachShader                   | [**ID3D11Device1::CreateVertexShader**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createvertexshader)                       |
| glGetShaderiv, glGetShaderSource | [**ID3D11DeviceContext1::VSGetShader**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-vsgetshader)                       |
| glGetUniformfv, glGetUniformiv   | [**ID3D11DeviceContext1 :: VSGetConstantBuffers1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nf-d3d11_1-id3d11devicecontext1-vsgetconstantbuffers1). |

 

## <a name="configuring-the-pixel-shaders"></a>Configuration des nuanceurs de pixels


La configuration d’un nuanceur de pixels dans Direct3D 11 est effectuée après le chargement du nuanceur. Les uniformes sont transmis sous forme de mémoires tampons constantes à l’aide de [**ID3D11DeviceContext1::PSSetConstantBuffers1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nf-d3d11_1-id3d11devicecontext1-pssetconstantbuffers1).

| OpenGL ES 2.0                    | Direct3D 11                                                                                               |
|----------------------------------|-----------------------------------------------------------------------------------------------------------|
| glAttachShader                   | [**ID3D11Device1::CreatePixelShader**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createpixelshader)                         |
| glGetShaderiv, glGetShaderSource | [**ID3D11DeviceContext1 ::P SGetShader**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-psgetshader)                       |
| glGetUniformfv, glGetUniformiv   | [**ID3D11DeviceContext1 ::P sgetconstantbuffers1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nf-d3d11_1-id3d11devicecontext1-psgetconstantbuffers1). |

 

## <a name="generating-the-final-results"></a>Génération des résultats finaux


Une fois le pipeline terminé, vous dessinez les résultats des étapes du nuanceur dans le tampon d’arrière-plan. Dans Direct3D 11, tout comme dans Open GL ES 2.0, cela implique d’appeler une commande de dessin pour représenter les résultats sous forme de table des couleurs dans le tampon d’arrière-plan, puis d’envoyer ce tampon d’arrière-plan à l’affichage.

| OpenGL ES 2.0  | Direct3D 11                                                                                                                                                                                                                                         |
|----------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| glDrawElements | [**ID3D11DeviceContext1 ::D RAW**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-draw), [**ID3D11DeviceContext1 ::D rawindexed**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-drawindexed) (ou d’autres méthodes Draw @ No__t-4 sur [**ID3D11DeviceContext1**](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11devicecontext)). |
| eglSwapBuffers | [**IDXGISwapChain1 ::P resent1**](https://docs.microsoft.com/windows/desktop/api/dxgi1_2/nf-dxgi1_2-idxgiswapchain1-present1)                                                                                                                                                                              |

 

## <a name="porting-glsl-to-hlsl"></a>Portage GLSL vers HLSL


GLSL et HLSL ne sont pas très différents, au-delà de la prise en charge des types complexes et d’une partie de la syntaxe générale. Bon nombre de développeurs trouvent qu’il est plus facile d’effectuer le portage en créant des alias des instructions et des définitions courantes d’OpenGL ES 2.0 pour leur équivalent HLSL. Notez que Direct3D utilise la version Shader Model pour exprimer l’ensemble des fonctionnalités du HLSL pris en charge par une interface graphique. OpenGL présente des spécifications de version différentes pour HLSL. Le tableau suivant tente de vous donner une idée approximative des équivalences entre les ensembles de fonctionnalités de langage du nuanceur définis pour Direct3D 11 et OpenGL ES 2.0.

| Langage du nuanceur           | version de la fonctionnalité GLSL                                                                                                                                                                                                      | Shader Model Direct3D |
|---------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------|
| HLSL Direct3D 11          | ~4.30.                                                                                                                                                                                                                    | SM 5.0                |
| GLSL ES pour OpenGL ES 2.0 | 1.40. Les anciennes implémentations de GLSL ES pour OpenGL ES 2.0 peuvent utiliser les versions 1.10 à 1.30. Vérifiez votre code d’origine avec glGetString (GL @ no__t-0SHADING @ no__t-1LANGUAGE @ no__t-2VERSION) ou glGetString (Shading @ no__t-3LANGUAGE @ no__t-4VERSION) pour le déterminer. | ~SM 2.0               |

 

Pour plus d’informations sur les différences de langages entre les deux nuanceurs, de même que sur les mappages de la syntaxe courante, voir [Informations de référence sur le passage de GLSL à HLSL](glsl-to-hlsl-reference.md).

## <a name="porting-the-opengl-intrinsics-to-hlsl-semantics"></a>Portage des intrinsèques OpenGL vers les sémantiques HLSL


Les sémantiques HLSL de Direct3D 11 sont des chaînes qui, à l’instar d’un uniforme ou d’un nom d’attribut, sont utilisées pour identifier une valeur transmise entre l’application et un programme de nuanceur. Bien qu’il existe un grand nombre de chaînes possibles, on utilise en général une chaîne de type POSITION ou COLOR qui indique l’utilisation. Vous devez assigner ces sémantiques quand vous créez un tampon constant ou un schéma d’entrée de tampon. Vous pouvez aussi ajouter un nombre compris entre 0 et 7 à la sémantique afin d’utiliser des registres séparés pour des valeurs similaires. Exemple : COLOR0, COLOR1, COLOR2...

La sémantique précédée de « SV @ no__t-0 » est une sémantique de valeur système écrite par votre programme de nuanceur ; votre application elle-même (exécutée sur l’UC) ne peut pas les modifier. Généralement, elles contiennent des valeurs qui sont des entrées ou des sorties d’une autre étape de nuanceur dans le pipeline graphique, ou qui sont générées entièrement par le processeur graphique.

En outre, la sémantique SV @ no__t-0 a des comportements différents lorsqu’ils sont utilisés pour spécifier une entrée ou une sortie à partir d’une étape de nuanceur. Par exemple, SV @ no__t-0POSITION (output) contient les données de vertex transformées au cours de l’étape du nuanceur de sommets, tandis que SV @ no__t-1POSITION (Input) contient les valeurs de position de pixel interpolées pendant la pixellisation.

Voici quelques mappages d’intrinsèques de nuanceur courants d’OpenGL ES 2.0 :

| Valeur système OpenGL | Utilisez cette sémantique HLSL                                                                                                                                                   |
|---------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| GL @ no__t-0Position        | POSITION(n) pour les données de tampon de vertex. SV @ no__t-0POSITION fournit une position de pixel au nuanceur de pixels et ne peut pas être écrite par votre application.                                        |
| GL @ no__t-0Normal          | NORMAL(n) pour des données normales fournies par le tampon de vertex.                                                                                                                 |
| GL @ no__t-0TexCoord @ no__t-1N @ no__t-2   | TEXCOORD(n) pour des données de coordonnées de texture UV (ST dans certaines documentations OpenGL) fournies à un nuanceur.                                                                       |
| GL @ no__t-0FragColor       | COLOR(n) pour des données de couleur RVBA fournies à un nuanceur. Notez qu’elles sont traitées de la même façon que les données de coordonnées. La sémantique vous aide simplement à identifier qu’il s’agit de données de couleur. |
| GL @ no__t-0FragData @ no__t-1N @ no__t-2   | SV @ no__t-0Target @ no__t-1N @ no__t-2 pour l’écriture à partir d’un nuanceur de pixels vers une texture cible ou une autre mémoire tampon de pixels.                                                                               |

 

La méthode de codage des sémantiques n’est pas la même que celle des intrinsèques dans OpenGL ES 2.0. Dans OpenGL, vous pouvez accéder directement à la plupart des intrinsèques sans configuration, ni déclaration. Dans Direct3D, vous devez déclarer un champ dans une mémoire tampon constante spécifique pour utiliser une sémantique en particulier, ou la déclarer en tant que valeur de retour d’une méthode **main()** de nuanceur.

Voici un exemple de sémantique utilisée dans une définition de tampon constant :

```cpp
struct VertexShaderInput
{
  float3 pos : POSITION;
  float3 color : COLOR0;
};

// The position is interpolated to the pixel value by the system. The per-vertex color data is also interpolated and passed through the pixel shader. 
struct PixelShaderInput
{
  float4 pos : SV_POSITION;
  float3 color : COLOR0;
};
```

Ce code définit une paire de tampons constants simples

Et voici un exemple de sémantique utilisée pour définir la valeur renvoyée par un nuanceur de fragments :

```cpp
// A pass-through for the (interpolated) color data.
float4 main(PixelShaderInput input) : SV_TARGET
{
  return float4(input.color,1.0f);
}
```

Dans ce cas, SV @ no__t-0TARGET est l’emplacement de la cible de rendu dans lequel la couleur de pixel (définie en tant que vecteur avec quatre valeurs float) est écrite dans lorsque l’exécution du nuanceur se termine.

Pour plus d’informations sur l’utilisation de la sémantique avec Direct3D, voir [Sémantique HLSL](https://docs.microsoft.com/windows/desktop/direct3dhlsl/dx-graphics-hlsl-semantics).

 

 




