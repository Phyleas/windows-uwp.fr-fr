---
title: Porter les mémoires tampons et données de vertex
description: Lors de cette étape, vous allez définir les mémoires tampons de vertex qui contiendront vos maillages ainsi que les mémoires tampons d’index qui permettront aux nuanceurs de parcourir les vertex dans l’ordre indiqué.
ms.assetid: 9a8138a5-0797-8532-6c00-58b907197a25
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, jeux, port, mémoires tampons de vertex, données, Direct3D
ms.localizationpriority: medium
ms.openlocfilehash: 18e785fb5016e6e06225da05199df99d175afa77
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89173093"
---
# <a name="port-the-vertex-buffers-and-data"></a>Porter les mémoires tampons et données de vertex




**API importantes**

-   [**ID3DDevice::CreateBuffer**](/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createbuffer)
-   [**ID3DDeviceContext::IASetVertexBuffers**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-iasetvertexbuffers)
-   [**ID3D11DeviceContext::IASetIndexBuffer**](/windows/desktop/api/d3d10/nf-d3d10-id3d10device-iasetindexbuffer)

Lors de cette étape, vous allez définir les mémoires tampons de vertex qui contiendront vos maillages ainsi que les mémoires tampons d’index qui permettront aux nuanceurs de parcourir les vertex dans l’ordre indiqué.

Commençons par examiner le modèle codé en dur correspondant au maillage de cube de notre exemple. Dans les deux représentations, les vertex sont organisés sous forme de liste de triangles (à la différence d’une bande de triangles ou de toute autre disposition de triangles plus appropriée). Tous les vertex de ces représentations sont également associés à des valeurs d’index et de couleur. Une grande partie du code Direct3D utilisé dans cette rubrique fait référence à des variables et des objets déjà définis dans le projet Direct3D.

Voici le cube qui sera traité par OpenGL ES 2.0. Dans l’exemple d’implémentation, chaque vertex se compose de sept valeurs flottantes (trois coordonnées de position suivies de quatre valeurs de couleur RVBA).

```cpp
#define CUBE_INDICES 36
#define CUBE_VERTICES 8

GLfloat cubeVertsAndColors[] = 
{
  -0.5f, -0.5f,  0.5f, 0.0f, 0.0f, 1.0f, 1.0f,
  -0.5f, -0.5f, -0.5f, 0.0f, 0.0f, 0.0f, 1.0f,
  -0.5f,  0.5f,  0.5f, 0.0f, 1.0f, 1.0f, 1.0f,
  -0.5f,  0.5f, -0.5f, 0.0f, 1.0f, 0.0f, 1.0f,
  0.5f, -0.5f,  0.5f, 1.0f, 0.0f, 1.0f, 1.0f,
  0.5f, -0.5f, -0.5f, 1.0f, 0.0f, 0.0f, 1.0f,  
  0.5f,  0.5f,  0.5f, 1.0f, 1.0f, 1.0f, 1.0f,
  0.5f,  0.5f, -0.5f, 1.0f, 1.0f, 0.0f, 1.0f
};

GLuint cubeIndices[] = 
{
  0, 1, 2, // -x
  1, 3, 2,

  4, 6, 5, // +x
  6, 7, 5,

  0, 5, 1, // -y
  5, 6, 1,

  2, 6, 3, // +y
  6, 7, 3,

  0, 4, 2, // +z
  4, 6, 2,

  1, 7, 3, // -z
  5, 7, 1
};
```

Et voici le même cube qui sera cette fois traité par Direct3D 11.

```cpp
VertexPositionColor cubeVerticesAndColors[] = 
// struct format is position, color
{
  {XMFLOAT3(-0.5f, -0.5f, -0.5f), XMFLOAT3(0.0f, 0.0f, 0.0f)},
  {XMFLOAT3(-0.5f, -0.5f,  0.5f), XMFLOAT3(0.0f, 0.0f, 1.0f)},
  {XMFLOAT3(-0.5f,  0.5f, -0.5f), XMFLOAT3(0.0f, 1.0f, 0.0f)},
  {XMFLOAT3(-0.5f,  0.5f,  0.5f), XMFLOAT3(0.0f, 1.0f, 1.0f)},
  {XMFLOAT3( 0.5f, -0.5f, -0.5f), XMFLOAT3(1.0f, 0.0f, 0.0f)},
  {XMFLOAT3( 0.5f, -0.5f,  0.5f), XMFLOAT3(1.0f, 0.0f, 1.0f)},
  {XMFLOAT3( 0.5f,  0.5f, -0.5f), XMFLOAT3(1.0f, 1.0f, 0.0f)},
  {XMFLOAT3( 0.5f,  0.5f,  0.5f), XMFLOAT3(1.0f, 1.0f, 1.0f)},
};
        
unsigned short cubeIndices[] = 
{
  0, 2, 1, // -x
  1, 2, 3,

  4, 5, 6, // +x
  5, 7, 6,

  0, 1, 5, // -y
  0, 5, 4,

  2, 6, 7, // +y
  2, 7, 3,

  0, 4, 6, // -z
  0, 6, 2,

  1, 3, 7, // +z
  1, 7, 5
};
```

Si vous analysez ce code plus en détail, vous remarquerez que le cube contenu dans le code OpenGL ES 2.0 est représenté dans un système de coordonnées orienté main droite, alors que le cube défini dans le code Direct3D est représenté dans un système de coordonnées orienté main gauche. Lorsque vous importez vos propres données de maillage, vous devez inverser les coordonnées de l’axe z de votre modèle, et modifier les index de chaque maillage de façon adéquate pour parcourir les triangles dans le nouvel ordre du système de coordonnées.

Supposons que nous venons de transposer le maillage du cube du système de coordonnées orienté main droite d’OpenGL ES 2.0 dans le système de coordonnées orienté main gauche de Direct3D. Voyons maintenant comment faire pour charger les données du cube en vue de leur traitement dans les deux modèles.

## <a name="instructions"></a>Instructions

### <a name="step-1-create-an-input-layout"></a>Étape 1 : Créer une disposition d’entrée

Dans OpenGL ES 2.0, vos données de vertex sont fournies sous forme d’attributs, qui seront ensuite transmis aux objets nuanceur pour lecture. En règle générale, vous fournissez à l’objet programme du nuanceur une chaîne contenant le nom d’attribut utilisé dans le code GLSL du nuanceur, puis vous obtenez en retour un emplacement mémoire à transmettre au nuanceur. Dans cet exemple, un objet mémoire tampon de vertex stocke une liste de structures de vertex personnalisées, qui ont été définies et mises en forme comme suit :

OpenGL ES 2.0 : configurer les attributs contenant les données de chaque vertex

``` syntax
typedef struct 
{
  GLfloat pos[3];        
  GLfloat rgba[4];
} Vertex;
```

Dans OpenGL ES 2,0, les dispositions d’entrée sont implicites. vous prenez une \_ mémoire tampon de tableau d’éléments GL à usage général \_ \_ et fournissez le Stride et le décalage de telle sorte que le nuanceur de sommets puisse interpréter les données après les avoir chargées. Avant l’étape du rendu, vous indiquez au nuanceur quels attributs il doit mapper aux différentes parties de chaque bloc de données de vertex, avec **glVertexAttribPointer**.

Dans Direct3D, vous devez fournir la disposition d’entrée qui décrit la structure des données de vertex dans la mémoire tampon de vertex au moment où vous créez cette dernière (et pas avant de dessiner la géométrie). Pour cela, utilisez une disposition d’entrée qui correspond à la disposition des données de chaque vertex en mémoire. Les informations que vous indiquez ici doivent être précises et exactes. C’est très important !

Ici, vous créez une description d’entrée sous la forme d’un tableau d' [** \_ éléments d’entrée d3d11 structures \_ \_ desc**](/windows/desktop/api/d3d11/ns-d3d11-d3d11_input_element_desc) .

Direct3D : définir une description de disposition d’entrée

``` syntax
struct VertexPositionColor
{
  DirectX::XMFLOAT3 pos;
  DirectX::XMFLOAT3 color;
};

// ...

const D3D11_INPUT_ELEMENT_DESC vertexDesc[] = 
{
  { "POSITION", 0, DXGI_FORMAT_R32G32B32_FLOAT, 0, 0,  D3D11_INPUT_PER_VERTEX_DATA, 0 },
  { "COLOR",    0, DXGI_FORMAT_R32G32B32_FLOAT, 0, 12, D3D11_INPUT_PER_VERTEX_DATA, 0 },
};

```

Cette description de disposition d’entrée définit un vertex à l’aide d’une paire de vecteurs à trois coordonnées : le premier vecteur 3D indique la position du vertex dans le système de coordonnées du modèle et le second vecteur 3D contient la valeur de couleur RVB associée au vertex. Dans cet exemple, vous utilisez trois valeurs à virgule flottante 32 bits, représentées ainsi dans le code : `XMFLOAT3(X.Xf, X.Xf, X.Xf)`. Vous devez utiliser des types de la bibliothèque [DirectXMath](/windows/desktop/dxmath/ovw-xnamath-reference)si vous gérez des données qui seront utilisées par un nuanceur car ils garantissent un packaging et un alignement corrects de ces données. (Par exemple, choisissez le type [**XMFLOAT3**](/windows/desktop/api/directxmath/ns-directxmath-xmfloat3) ou [**XMFLOAT4**](/windows/desktop/api/directxmath/ns-directxmath-xmfloat4) pour les données de vecteur et le type [**XMFLOAT4X4**](/windows/desktop/api/directxmath/ns-directxmath-xmfloat4x4) pour les matrices.)

Pour obtenir la liste de tous les types de format possibles, reportez-vous au [** \_ format dxgi**](/windows/desktop/api/dxgiformat/ne-dxgiformat-dxgi_format).

Créez l’objet disposition sur la base de la disposition d’entrée du vertex que vous venez de définir. Dans le code suivant, vous l’écrivez dans **m \_ inputLayout**, une variable de type **ComPtr** (qui pointe vers un objet de type [**ID3D11InputLayout**](/windows/desktop/api/d3d11/nn-d3d11-id3d11inputlayout)). **fileData** contient l’objet nuanceur de vertex compilé à l’étape précédente, [Porter les nuanceurs](port-the-shader-config.md).

Direct3D : créer la disposition d’entrée utilisée par la mémoire tampon de vertex

``` syntax
Microsoft::WRL::ComPtr<ID3D11InputLayout>      m_inputLayout;

// ...

m_d3dDevice->CreateInputLayout(
  vertexDesc,
  ARRAYSIZE(vertexDesc),
  fileData->Data,
  fileShaderData->Length,
  &m_inputLayout
);
```

Nous avons terminé la définition de la disposition d’entrée. À présent, créons une mémoire tampon qui utilise cette disposition et chargeons-la avec les données de maillage du cube.

### <a name="step-2-create-and-load-the-vertex-buffers"></a>Étape 2 : Créer et charger chaque mémoire tampon de vertex

Dans OpenGL ES 2.0, vous créez deux mémoires tampons : l’une pour les données de position, l’autre pour les valeurs de couleur. (Vous pourriez aussi créer une structure contenant une seule de ces mémoires tampons ou les deux.) Vous liez ces mémoires tampons, puis leur fournissez les données de position et de couleur. Plus tard, lors de l’exécution de votre fonction de rendu, vous devrez de nouveau lier les mémoires tampons et indiquer au nuanceur le format des données contenues dans chacune d’elles afin qu’il sache comment les interpréter.

OpenGL ES 2.0 : lier les mémoires tampons de vertex

``` syntax
// upload the data for the vertex position buffer
glGenBuffers(1, &renderer->vertexBuffer);    
glBindBuffer(GL_ARRAY_BUFFER, renderer->vertexBuffer);
glBufferData(GL_ARRAY_BUFFER, sizeof(VERTEX) * CUBE_VERTICES, renderer->vertices, GL_STATIC_DRAW);   
```

Dans Direct3D, les mémoires tampons accessibles par le Shader sont représentées en tant que structures de [** \_ \_ données**](/windows/desktop/api/d3d11/ns-d3d11-d3d11_subresource_data) de sous-ressources d3d11. Pour lier l’emplacement de ce tampon à l’objet Shader, vous devez créer une \_ structure DESC de tampon CD3D11 \_ pour chaque mémoire tampon avec [**ID3DDevice :: CreateBuffer**](/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createbuffer), puis définir la mémoire tampon du contexte de périphérique Direct3D en appelant une méthode Set spécifique au type de mémoire tampon, par exemple [**ID3DDeviceContext :: IASetVertexBuffers**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-iasetvertexbuffers).

Lorsque vous définissez la mémoire tampon, vous devez configurer le stride (la taille de l’élément de données pour un vertex) ainsi que le décalage (là où commence le tableau des données de vertex) à partir du début de la mémoire.

Notez que nous attribuons le pointeur au tableau **vertexIndices** dans le champ **pSysMem** de la structure de données de sous- [** \_ ressource \_ d3d11**](/windows/desktop/api/d3d11/ns-d3d11-d3d11_subresource_data) . Si cela n’est pas défini correctement, votre maillage sera endommagé ou vide.

Direct3D : créer et définir la mémoire tampon de vertex

``` syntax
D3D11_SUBRESOURCE_DATA vertexBufferData = {0};
vertexBufferData.pSysMem = cubeVertices;
vertexBufferData.SysMemPitch = 0;
vertexBufferData.SysMemSlicePitch = 0;
CD3D11_BUFFER_DESC vertexBufferDesc(sizeof(cubeVertices), D3D11_BIND_VERTEX_BUFFER);
        
m_d3dDevice->CreateBuffer(
  &vertexBufferDesc,
  &vertexBufferData,
  &m_vertexBuffer);
        
// ...

UINT stride = sizeof(VertexPositionColor);
UINT offset = 0;
m_d3dContext->IASetVertexBuffers(
  0,
  1,
  m_vertexBuffer.GetAddressOf(),
  &stride,
  &offset);
```

### <a name="step-3-create-and-load-the-index-buffer"></a>Étape 3 : Créer et charger la mémoire tampon d’index

Les mémoires tampons d’index sont très utiles, car elles permettent au nuanceur de vertex de rechercher des vertex spécifiques. Nous avons intégré ces mémoires dans notre exemple de convertisseur, mais ce n’est pas une obligation. À l’instar des mémoires tampons de vertex dans OpenGL ES 2.0, vous créez et liez une mémoire tampon d’index en tant que mémoire tampon générale, puis vous y copiez les index de vertex que vous aviez précédemment créés.

Lorsque vous êtes prêt à dessiner, vous liez de nouveau les deux mémoires tampons (index et vertex) et vous appelez **glDrawElements**.

OpenGL ES 2.0 : envoyer l’ordre des index à l’appel de dessin

``` syntax
GLuint indexBuffer;

// ...

glGenBuffers(1, &renderer->indexBuffer);    
glBindBuffer(GL_ELEMENT_ARRAY_BUFFER, renderer->indexBuffer);   
glBufferData(GL_ELEMENT_ARRAY_BUFFER, 
  sizeof(GLuint) * CUBE_INDICES, 
  renderer->vertexIndices, 
  GL_STATIC_DRAW);

// ...
// Drawing function

// Bind the index buffer
glBindBuffer(GL_ELEMENT_ARRAY_BUFFER, renderer->indexBuffer);
glDrawElements (GL_TRIANGLES, renderer->numIndices, GL_UNSIGNED_INT, 0);
```

Dans Direct3D, le processus est quasiment similaire, bien qu’un peu plus technique. Fournissez le tampon d’index en tant que sous-ressource Direct3D au [**ID3D11DeviceContext**](/windows/desktop/api/d3d11/nn-d3d11-id3d11devicecontext) que vous avez créé lors de la configuration de Direct3D. Pour ce faire, appelez [**ID3D11DeviceContext :: IASetIndexBuffer**](/windows/desktop/api/d3d10/nf-d3d10-id3d10device-iasetindexbuffer) avec la sous-ressource configurée pour le tableau d’index, comme suit. (Là encore, remarquez que vous attribuez le pointeur au tableau **cubeIndices** dans le champ **pSysMem** de la structure de données de sous- [** \_ \_ ressource d3d11**](/windows/desktop/api/d3d11/ns-d3d11-d3d11_subresource_data) .)

Direct3D : créer la mémoire tampon d’index

``` syntax
m_indexCount = ARRAYSIZE(cubeIndices);

D3D11_SUBRESOURCE_DATA indexBufferData = {0};
indexBufferData.pSysMem = cubeIndices;
indexBufferData.SysMemPitch = 0;
indexBufferData.SysMemSlicePitch = 0;
CD3D11_BUFFER_DESC indexBufferDesc(sizeof(cubeIndices), D3D11_BIND_INDEX_BUFFER);

m_d3dDevice->CreateBuffer(
  &indexBufferDesc,
  &indexBufferData,
  &m_indexBuffer);

// ...

m_d3dContext->IASetIndexBuffer(
  m_indexBuffer.Get(),
  DXGI_FORMAT_R16_UINT,
  0);
```

Vous allez ensuite dessiner les triangles en appelant [**ID3D11DeviceContext::DrawIndexed**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-drawindexed) (ou [**ID3D11DeviceContext::Draw**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-draw) pour les vertex non indexés), comme expliqué ci-après. (Pour plus d’informations, voir l’étape suivante [Dessiner à l’écran](draw-to-the-screen.md).)

Direct3D : dessiner les vertex indexés

``` syntax
m_d3dContext->IASetPrimitiveTopology(D3D11_PRIMITIVE_TOPOLOGY_TRIANGLELIST);
m_d3dContext->IASetInputLayout(m_inputLayout.Get());

// ...

m_d3dContext->DrawIndexed(
  m_indexCount,
  0,
  0);
```

## <a name="previous-step"></a>Étape précédente


[Porter les objets nuanceur](port-the-shader-config.md)

## <a name="next-step"></a>Étape suivante

[Porter le langage GLSL](port-the-glsl.md)

## <a name="remarks"></a>Remarques

Lorsque vous structurez votre Direct3D, séparez le code qui appelle des méthodes sur [**ID3D11Device**](/windows/desktop/api/d3d11/nn-d3d11-id3d11device) dans une méthode appelée chaque fois que les ressources de l’appareil doivent être recréées. Dans le modèle de projet Direct3D, ce code se trouve dans les méthodes **CreateDeviceResource** de l’objet convertisseur. Le code employé pour la mise à jour du contexte de périphérique ([**ID3D11DeviceContext**](/windows/desktop/api/d3d11/nn-d3d11-id3d11devicecontext)) est placé dans la méthode **Render**, car c’est l’endroit où vous élaborez les étapes du nuanceur et liez les données requises.

## <a name="related-topics"></a>Rubriques connexes


* [Comment : porter un convertisseur OpenGL ES 2,0 simple vers Direct3D 11](port-a-simple-opengl-es-2-0-renderer-to-directx-11-1.md)
* [Porter les objets nuanceur](port-the-shader-config.md)
* [Porter les mémoires tampons et données de vertex](port-the-vertex-buffers-and-data-config.md)
* [Porter le langage GLSL](port-the-glsl.md)

 

 