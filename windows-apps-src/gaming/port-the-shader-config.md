---
title: Porter les objets nuanceur
description: Dans le cadre du portage du convertisseur simple OpenGL ES 2.0, vous devez commencer par créer les objets des nuanceurs de vertex et de fragments équivalents dans Direct3D 11, mais également vous assurer que le programme principal sera en mesure de communiquer avec ces différents objets une fois compilés.
ms.assetid: 0383b774-bc1b-910e-8eb6-cc969b3dcc08
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, jeux, port, nuanceur, Direct3D, OpenGL
ms.localizationpriority: medium
ms.openlocfilehash: a5405b49bddb31752c42ace82d89b128a71d78a2
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89171993"
---
# <a name="port-the-shader-objects"></a>Porter les objets nuanceur




**API importantes**

-   [**ID3D11Device**](/windows/desktop/api/d3d11/nn-d3d11-id3d11device)
-   [**ID3D11DeviceContext**](/windows/desktop/api/d3d11/nn-d3d11-id3d11devicecontext)

Dans le cadre du portage du convertisseur simple OpenGL ES 2.0, vous devez commencer par créer les objets des nuanceurs de vertex et de fragments équivalents dans Direct3D 11, mais également vous assurer que le programme principal sera en mesure de communiquer avec ces différents objets une fois compilés.

> **Remarque**    Avez-vous créé un nouveau projet Direct3D ? suivez les instructions fournies dans l’article [Créer un projet DirectX 11 pour la plateforme Windows universelle (UWP)](user-interface.md). Cette procédure pas à pas suppose que vous disposez des ressources DXGI et Direct3D nécessaires pour le dessin à l’écran (celles fournies dans le modèle).

 

De la même façon que dans OpenGL ES 2.0, les nuanceurs compilés dans Direct3D doivent être associés à un contexte de dessin. Toutefois, comme Direct3D n’utilise pas le concept d’objet programme de nuanceur, vous devez à la place affecter directement les nuanceurs à un objet [**ID3D11DeviceContext**](/windows/desktop/api/d3d11/nn-d3d11-id3d11devicecontext). Cette étape, qui suit le processus OpenGL ES 2.0 de création et de liaison d’objets nuanceur, vous permet d’obtenir les comportements d’API correspondants dans Direct3D.

<a name="instructions"></a>Instructions
------------

### <a name="step-1-compile-the-shaders"></a>Étape 1 : Compiler les nuanceurs

Dans cet exemple simple de code OpenGL ES 2.0, les nuanceurs sont enregistrés sous forme de fichiers texte, puis chargés en tant que données de type chaîne à compiler au moment de l’exécution.

OpenGL ES 2.0 : compiler un nuanceur

``` syntax
GLuint __cdecl CompileShader (GLenum shaderType, const char *shaderSrcStr)
// shaderType can be GL_VERTEX_SHADER or GL_FRAGMENT_SHADER. Returns 0 if compilation fails.
{
  GLuint shaderHandle;
  GLint compiledShaderHandle;
   
  // Create an empty shader object.
  shaderHandle = glCreateShader(shaderType);

  if (shaderHandle == 0)
  return 0;

  // Load the GLSL shader source as a string value. You could obtain it from
  // from reading a text file or hardcoded.
  glShaderSource(shaderHandle, 1, &shaderSrcStr, NULL);
   
  // Compile the shader.
  glCompileShader(shaderHandle);

  // Check the compile status
  glGetShaderiv(shaderHandle, GL_COMPILE_STATUS, &compiledShaderHandle);

  if (!compiledShaderHandle) // error in compilation occurred
  {
    // Handle any errors here.
              
    glDeleteShader(shaderHandle);
    return 0;
  }

  return shaderHandle;

}
```

Dans Direct3D, les nuanceurs ne sont pas compilés au moment de l’exécution : ils sont toujours compilés dans des fichiers CSO en même temps que les autres éléments du programme. Lorsque vous compilez votre application avec Microsoft Visual Studio, les fichiers HLSL sont compilés dans des fichiers CSO (.cso) que votre application doit ensuite charger. Vous devez donc veiller à bien inclure ces fichiers CSO dans le package de votre application !

> **Remarque**    L’exemple suivant effectue le chargement et la compilation du nuanceur de manière asynchrone à l’aide du mot clé **auto** et de la syntaxe lambda. La méthode ReadDataAsync(), implémentée pour le modèle, lit les données d’un fichier CSO sous forme de tableau d’octets (fileData).

 

Direct3D 11 : compiler un nuanceur

``` syntax
auto loadVSTask = DX::ReadDataAsync(m_projectDir + "SimpleVertexShader.cso");
auto loadPSTask = DX::ReadDataAsync(m_projectDir + "SimplePixelShader.cso");

auto createVSTask = loadVSTask.then([this](Platform::Array<byte>^ fileData) {

m_d3dDevice->CreateVertexShader(
  fileData->Data,
  fileData->Length,
  nullptr,
  &m_vertexShader);

auto createPSTask = loadPSTask.then([this](Platform::Array<byte>^ fileData) {
  m_d3dDevice->CreatePixelShader(
    fileData->Data,
    fileData->Length,
    nullptr,
    &m_pixelShader;
};
```

### <a name="step-2-create-and-load-the-vertex-and-fragment-pixel-shaders"></a>Étape 2 : Créer et charger les nuanceurs de vertex et de fragments (pixels)

OpenGL ES 2.0 renferme la notion de « programme » de nuanceur. Ce programme joue le rôle d’interface de communication entre le programme principal exécuté par le processeur UC et les différents nuanceurs qui sont exécutés par le processeur GPU. Les nuanceurs sont compilés (ou chargés à partir des ressources compilées), puis ils sont associés à un programme exécutable par le processeur GPU.

OpenGL ES 2.0 : charger les nuanceurs de vertex et de fragments dans un programme d’ombrage

``` syntax
GLuint __cdecl LoadShaderProgram (const char *vertShaderSrcStr, const char *fragShaderSrcStr)
{
  GLuint programObject, vertexShaderHandle, fragmentShaderHandle;
  GLint linkStatusCode;

  // Load the vertex shader and compile it to an internal executable format.
  vertexShaderHandle = CompileShader(GL_VERTEX_SHADER, vertShaderSrcStr);
  if (vertexShaderHandle == 0)
  {
    glDeleteShader(vertexShaderHandle);
    return 0;
  }

   // Load the fragment/pixel shader and compile it to an internal executable format.
  fragmentShaderHandle = CompileShader(GL_FRAGMENT_SHADER, fragShaderSrcStr);
  if (fragmentShaderHandle == 0)
  {
    glDeleteShader(fragmentShaderHandle);
    return 0;
  }

  // Create the program object proper.
  programObject = glCreateProgram();
   
  if (programObject == 0)    return 0;

  // Attach the compiled shaders
  glAttachShader(programObject, vertexShaderHandle);
  glAttachShader(programObject, fragmentShaderHandle);

  // Compile the shaders into binary executables in memory and link them to the program object..
  glLinkProgram(programObject);

  // Check the project object link status and determine if the program is available.
  glGetProgramiv(programObject, GL_LINK_STATUS, &linkStatusCode);

  if (!linkStatusCode) // if link status <> 0
  {
    // Linking failed; delete the program object and return a failure code (0).

    glDeleteProgram (programObject);
    return 0;
  }

  // Deallocate the unused shader resources. The actual executables are part of the program object.
  glDeleteShader(vertexShaderHandle);
  glDeleteShader(fragmentShaderHandle);

  return programObject;
}

// ...

glUseProgram(renderer->programObject);
```

Direct3D n’utilise pas le concept d’objet programme de nuanceur. Au lieu de cela, les nuanceurs sont créés lorsque l’une des méthodes de création de nuanceur sur l’interface [**ID3D11Device**](/windows/desktop/api/d3d11/nn-d3d11-id3d11device) (comme [**ID3D11Device :: CreateVertexShader**](/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createvertexshader) ou [**ID3D11Device :: CreatePixelShader**](/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createpixelshader)) est appelée. Pour définir les nuanceurs du contexte de dessin actuel, nous les fournissons aux [**ID3D11DeviceContext**](/windows/desktop/api/d3d11/nn-d3d11-id3d11devicecontext) correspondants à l’aide d’une méthode de nuanceur définie, telle que [**ID3D11DeviceContext :: VSSetShader**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-vssetshader) pour le nuanceur de sommets ou [**ID3D11DeviceContext ::P ssetshader**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-pssetshader) pour le nuanceur de fragments.

Direct3D 11 : définir les nuanceurs pour le contexte de dessin du périphérique graphique

``` syntax
m_d3dContext->VSSetShader(
  m_vertexShader.Get(),
  nullptr,
  0);

m_d3dContext->PSSetShader(
  m_pixelShader.Get(),
  nullptr,
  0);
```

### <a name="step-3-define-the-data-to-supply-to-the-shaders"></a>Étape 3 : Indiquer les données à transmettre aux nuanceurs

Dans notre exemple de code OpenGL ES 2.0, nous devons déclarer la variable **uniform** pour le pipeline des nuanceurs :

-   **u \_ mvpMatrix**: tableau 4x4 de valeurs float qui représente la matrice de transformation modèle-vue-projection finale qui prend les coordonnées du modèle pour le cube et les transforme en coordonnées de projection 2D pour la conversion de l’analyse.

Nous devons également déclarer deux valeurs **attribute** pour les données de vertex :

-   ** \_ position**: vecteur de 4-float pour les coordonnées de modèle d’un vertex.
-   ** \_ couleur**: vecteur à quatre nombres pour la valeur de couleur RVBA associée au vertex.

Open GL ES 2.0 : définitions GLSL pour les variables uniformes et les attributs

``` syntax
uniform mat4 u_mvpMatrix;
attribute vec4 a_position;
attribute vec4 a_color;
```

Ici, les variables du programme principal correspondantes sont définies sous forme de champs de l’objet convertisseur. (Voir l’en-tête dans [Procédure : portage d’un convertisseur simple OpenGL ES 2.0 sur Direct3D 11](port-a-simple-opengl-es-2-0-renderer-to-directx-11-1.md).) Nous devons ensuite indiquer à quels emplacements mémoire le programme principal doit fournir ces valeurs destinées au pipeline de nuanceurs. Cela se passe généralement juste avant un appel de dessin :

OpenGL ES 2.0 : marquage de l’emplacement des variables uniformes et des attributs

``` syntax

// Inform the shader of the attribute locations
loc = glGetAttribLocation(renderer->programObject, "a_position");
glVertexAttribPointer(loc, 3, GL_FLOAT, GL_FALSE, 
    sizeof(Vertex), 0);
glEnableVertexAttribArray(loc);

loc = glGetAttribLocation(renderer->programObject, "a_color");
glVertexAttribPointer(loc, 4, GL_FLOAT, GL_FALSE, 
    sizeof(Vertex), (GLvoid*) (sizeof(float) * 3));
glEnableVertexAttribArray(loc);


// Inform the shader program of the uniform location
renderer->mvpLoc = glGetUniformLocation(renderer->programObject, "u_mvpMatrix");
```

Direct3D n’utilise pas le concept de variables uniformes (« uniform ») et d’attributs (« attribute ») sous le même angle (ou pas avec la même syntaxe). À la place, il se sert de mémoires tampons constantes, représentant des sous-ressources Direct3D qui sont partagées entre le programme principal et les programmes de nuanceur. Certaines de ces sous-ressources, telles que les positions et couleurs de vertex, sont décrites sous forme de sémantiques HLSL. Pour plus d’informations sur les mémoires tampons constantes et les sémantiques HLSL sous-jacentes aux concepts OpenGL ES 2.0, voir [Porter des objets tampon de trame, des variables uniformes et des attributs](porting-uniforms-and-attributes.md).

Pour adapter ce processus à Direct3D, nous convertissons la variable uniforme en mémoire tampon constante Direct3D (cbuffer) et nous lui affectons un registre pour la recherche, avec la sémantique HLSL **register**. Les deux attributs de vertex sont gérés en tant qu’éléments d’entrée lors des étapes du pipeline de nuanceurs et sont également associés à des [sémantiques HLSL](/windows/desktop/direct3dhlsl/dcl-usage---ps) (POSITION et COLOR0) qui renseignent les nuanceurs sur la position et la couleur du vertex. Le nuanceur de pixels prend une \_ position de SV, avec le \_ préfixe VP indiquant qu’il s’agit d’une valeur système générée par le GPU. (Dans notre exemple, la valeur correspond à la position d’un pixel générée lors de la conversion de numérisation.) VertexShaderInput et PixelShaderInput ne sont pas déclarés en tant que mémoires tampons constantes, car VertexShaderInput est utilisé pour définir la mémoire tampon de vertex (voir [Porter les mémoires tampons et données de vertex](port-the-vertex-buffers-and-data-config.md)), et les données de PixelShaderInput sont générées lors d’une étape précédente dans le pipeline, à savoir ici le nuanceur de vertex.

Direct3D : définitions HLSL pour les mémoires tampons constantes et les données de vertex

``` syntax
cbuffer ModelViewProjectionConstantBuffer : register(b0)
{
  matrix mvp;
};

// Per-vertex data used as input to the vertex shader.
struct VertexShaderInput
{
  float4 pos : POSITION;
  float4 color : COLOR0;
};

// Per-vertex color data passed through the pixel shader.
struct PixelShaderInput
{
  float4 pos : SV_POSITION;
  float3 color : COLOR0;
};
```

Pour plus d’informations sur le portage des mémoires tampons constantes et l’application de sémantiques HLSL, voir [Porter des objets tampon de trame, des variables uniformes et des attributs](porting-uniforms-and-attributes.md).

Voici les structures de disposition des données qui sont transmises au pipeline de nuanceurs avec une mémoire tampon constante ou de vertex.

Direct3D 11 : déclaration de la disposition d’une mémoire tampon constante ou de vertex

``` syntax
// Constant buffer used to send MVP matrices to the vertex shader.
struct ModelViewProjectionConstantBuffer
{
  DirectX::XMFLOAT4X4 modelViewProjection;
};

// Used to send per-vertex data to the vertex shader.
struct VertexPositionColor
{
  DirectX::XMFLOAT4 pos;
  DirectX::XMFLOAT4 color;
};
```

Utilisez les types XM DirectXMath \* pour vos éléments de mémoire tampon constantes, car ils fournissent une compression et un alignement appropriés pour le contenu lorsqu’ils sont envoyés au pipeline du nuanceur. Si vous utilisez les types et tableaux de valeurs flottantes standard de la plateforme Windows, vous devez effectuer le packaging et l’alignement manuellement.

Pour lier une mémoire tampon constante, créez une description de disposition en tant que structure [** \_ \_ desc de tampon CD3D11**](/windows/desktop/api/d3d11/ns-d3d11-cd3d11_buffer_desc) et transmettez-la à [**ID3DDevice :: CreateBuffer**](/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createbuffer). Ensuite, dans votre méthode de rendu, transmettez la mémoire tampon constante à [**ID3D11DeviceContext::UpdateSubresource**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-updatesubresource) avant de procéder au dessin.

Direct3D 11 : lier la mémoire tampon constante

``` syntax
CD3D11_BUFFER_DESC constantBufferDesc(sizeof(ModelViewProjectionConstantBuffer), D3D11_BIND_CONSTANT_BUFFER);

m_d3dDevice->CreateBuffer(
  &constantBufferDesc,
  nullptr,
  &m_constantBuffer);

// ...

// Only update shader resources that have changed since the last frame.
m_d3dContext->UpdateSubresource(
  m_constantBuffer.Get(),
  0,
  NULL,
  &m_constantBufferData,
  0,
  0);
```

La mémoire tampon de vertex est créée et mise à jour de manière similaire. Nous verrons cela dans le cadre de la prochaine étape, [Porter les mémoires tampons et données de vertex](port-the-vertex-buffers-and-data-config.md).

<a name="next-step"></a>Étape suivante
---------

[Porter les mémoires tampons et données de vertex](port-the-vertex-buffers-and-data-config.md)
## <a name="related-topics"></a>Rubriques connexes


[Comment : porter un convertisseur OpenGL ES 2,0 simple vers Direct3D 11](port-a-simple-opengl-es-2-0-renderer-to-directx-11-1.md)

[Porter les mémoires tampons et données de vertex](port-the-vertex-buffers-and-data-config.md)

[Porter le langage GLSL](port-the-glsl.md)

[Dessiner à l’écran](draw-to-the-screen.md)

 

 