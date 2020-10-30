---
description: Cette rubrique décrit le schéma du fichier de configuration XML MakePri.exe.
title: Fichier de configuration de MakePri.exe
template: detail.hbs
ms.date: 10/18/2017
ms.topic: article
keywords: windows 10, uwp, ressources, image, MRT, qualificateur
ms.localizationpriority: medium
ms.openlocfilehash: 5427927322f61f44cf3a8b53112f5f7811520290
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93031722"
---
# <a name="makepriexe-configuration-file"></a>Fichier de configuration de MakePri.exe

Cette rubrique décrit le schéma du fichier de configuration XML [MakePri.exe](compile-resources-manually-with-makepri.md) ; également connu sous le nom de fichier de configuration PRI. L’outil MakePri.exe possède une [commande createconfig](makepri-exe-command-options.md#createconfig-command) que vous pouvez utiliser pour créer un fichier de configuration PRI initialisé.

> [!NOTE]
> MakePri.exe est installé lorsque vous activez l’option **SDK Windows pour les applications gérées UWP** lors de l’installation du kit de développement logiciel (SDK) Windows. Il est installé dans le chemin d’accès `%WindowsSdkDir%bin\<WindowsTargetPlatformVersion>\x64\makepri.exe` (et dans les dossiers nommés pour les autres architectures). Par exemple : `C:\Program Files (x86)\Windows Kits\10\bin\10.0.17713.0\x64\makepri.exe`.

Le fichier de configuration PRI contrôle les ressources qui sont indexées et comment. Le fichier XML de configuration doit se conformer au schéma suivant.

```xml
<?xml version="1.0" encoding="utf-8"?>
<xs:schema attributeFormDefault="unqualified" elementFormDefault="qualified" xmlns:xs="http://www.w3.org/2001/XMLSchema">
  <xs:element name="resources">
    <xs:complexType>
      <xs:sequence>
        <xs:element name="packaging" maxOccurs="1" minOccurs="0">
          <xs:complexType>
            <xs:sequence>
              <xs:element name="autoResourcePackage" maxOccurs="unbounded" minOccurs="0">
                <xs:complexType>
                  <xs:attribute name="qualifier" type="xs:string" use="required" />
                </xs:complexType>
              </xs:element>
              <xs:element name="resourcePackage" maxOccurs="unbounded" minOccurs="0">
                <xs:complexType>
                  <xs:sequence>
                    <xs:element name="qualifierSet" maxOccurs="unbounded" minOccurs="0">
                      <xs:complexType>
                        <xs:attribute name="definition" type="xs:string" use="required" />
                      </xs:complexType>
                    </xs:element>
                  </xs:sequence>
                  <xs:attribute name="name" type="xs:string" use="required" />
                </xs:complexType>
              </xs:element>
            </xs:sequence>
          </xs:complexType>
        </xs:element>
        <xs:element maxOccurs="unbounded" name="index">
          <xs:complexType>
            <xs:sequence>
              <xs:element name="qualifiers" minOccurs="0" maxOccurs="unbounded">
                <xs:complexType>
                  <xs:sequence>
                    <xs:element minOccurs="1" maxOccurs="unbounded" name="qualifier">
                      <xs:complexType>
                        <xs:attribute name="name" type="xs:string" use="required" />
                        <xs:attribute name="value" type="xs:string" use="required" />
                      </xs:complexType>
                    </xs:element>
                  </xs:sequence>
                </xs:complexType>
              </xs:element>
              <xs:element name="default" minOccurs="0" maxOccurs="unbounded">
                <xs:complexType>
                  <xs:sequence>
                    <xs:element minOccurs="1" maxOccurs="unbounded" name="qualifier">
                      <xs:complexType>
                        <xs:attribute name="name" type="xs:string" use="required" />
                        <xs:attribute name="value" type="xs:string" use="required" />
                      </xs:complexType>
                    </xs:element>
                  </xs:sequence>
                </xs:complexType>
              </xs:element>
              <xs:element name="indexer-config" minOccurs="0" maxOccurs="unbounded">
                <xs:complexType>
                  <xs:sequence>
                    <xs:any minOccurs="0" maxOccurs="unbounded" processContents="skip"/>
                  </xs:sequence>
                  <xs:attribute name="type" type="xs:string" use="required" />
                  <xs:anyAttribute processContents="skip"/>
                </xs:complexType>
              </xs:element>
            </xs:sequence>
            <xs:attribute name="root" type="xs:string" use="required" />
            <xs:attribute name="startIndexAt" type="xs:string" use="required" />
          </xs:complexType>
        </xs:element>
      </xs:sequence>
      <xs:attribute name="isDeploymentMergeable" type="xs:boolean" use="optional" />
      <xs:attribute name="majorVersion" type="xs:positiveInteger" use="optional" />
      <xs:attribute name="targetOsVersion" type="xs:string" use="optional" />
    </xs:complexType>
  </xs:element>
</xs:schema>
```

- L' `default` élément spécifie le contexte (langage, échelle, contraste, etc.) qui doit être utilisé pour résoudre les ressources lorsque le contexte d’exécution ne correspond à aucun candidat aux ressources. Étant donné que ce contexte est spécifié au moment de la génération et qu’il ne change pas, les ressources sont résolues dans ce contexte lors de la création de qualificateurs. Le score de correspondance est stocké au moment de la génération. Chaque qualificateur doit avoir une valeur spécifiée. Pour plus d’informations sur la façon dont les ressources sont choisies, consultez [ResourceContext](resource-management-system.md#resourcecontext) .
- L' `index` élément définit les passes d’indexation discrète effectuées sur les éléments multimédias. Chaque passe d’indexation détermine les [indexeurs spécifiques au format](makepri-exe-format-specific-indexers.md) à utiliser et les ressources à indexer.
- L' `qualifiers` élément définit les qualificateurs initiaux pour le premier fichier ou dossier hérité par d’autres ressources. Chaque élément qualificateur doit avoir un nom et une valeur valides (consultez [adapter vos ressources pour connaître la langue, l’échelle, le contraste élevé et d’autres qualificateurs](tailor-resources-lang-scale-contrast.md)).
- L' `root` attribut est la racine du chemin d’accès du fichier physique pour la passe d’index. Il peut être relatif ou absolu. S’il est relatif, il est ajouté à la racine du projet que vous fournissez dans la ligne de commande. S’il est absolu, il est utilisé directement comme racine de passe d’index. Les barres obliques arrière ou avant sont acceptables. Les barres obliques de fin sont tronquées. La racine de la passe d’index détermine le dossier dans lequel toutes les ressources sont considérées comme relatives.
- L' `startIndexAt` attribut est le fichier ou dossier initial de départ utilisé dans l’indexation. Il est relatif à la racine de passe d’index. Une valeur vide est supposée être le dossier racine de réussite d’index.

## <a name="default-pri-config-file"></a>Fichier de configuration PRI par défaut

MakePri.exe génère ce nouveau fichier de configuration PRI initialisé lors de l’émission de la [commande createconfig](makepri-exe-command-options.md#createconfig-command) .

```xml
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<resources targetOsVersion="10.0.0" majorVersion="1">
  <packaging>
    <autoResourcePackage qualifier="Language"/>
    <autoResourcePackage qualifier="Scale"/>
    <autoResourcePackage qualifier="DXFeatureLevel"/>
  </packaging>
  <index root="\" startIndexAt="\">
    <default>
      <qualifier name="Language" value="en-US"/>
      <qualifier name="Contrast" value="standard"/>
      <qualifier name="Scale" value="100"/>
      <qualifier name="HomeRegion" value="001"/>
      <qualifier name="TargetSize" value="256"/>
      <qualifier name="LayoutDirection" value="LTR"/>
      <qualifier name="Theme" value="dark"/>
      <qualifier name="AlternateForm" value=""/>
      <qualifier name="DXFeatureLevel" value="DX9"/>
      <qualifier name="Configuration" value=""/>
      <qualifier name="DeviceFamily" value="Universal"/>
      <qualifier name="Custom" value=""/>
    </default>
    <indexer-config type="folder" foldernameAsQualifier="true" filenameAsQualifier="true" qualifierDelimiter="."/>
    <indexer-config type="resw" convertDotsToSlashes="true" initialPath=""/>
    <indexer-config type="resjson" initialPath=""/>
    <indexer-config type="PRI"/>
  </index>
  <!--<index startIndexAt="Start Index Here" root="Root Here">-->
  <!--        <indexer-config type="resfiles" qualifierDelimiter="."/>-->
  <!--        <indexer-config type="priinfo" emitStrings="true" emitPaths="true" emitEmbeddedData="true"/>-->
  <!--</index>-->
</resources>
```

## <a name="packaging-element"></a>Élément Packaging

L' `packaging` élément définit les informations de fractionnement PRI. Le schéma de l' `packaging` élément est défini pour la configuration automatique (prise en charge d' `autoResourcePackage` une dimension spécifique) et la configuration manuelle.

Cet exemple montre comment utiliser `autoResourcePackage` avec une dimension spécifique.

```xml
    <packaging>
        <autoResourcePackage qualifier="Language"/>
        <autoResourcePackage qualifier="Scale"/>
        <autoResourcePackage qualifier="DXFeatureLevel"/>
    </packaging>
```

Cet exemple montre comment utiliser Manual `resourcePackage` .

```xml
  <packaging>
    <resourcePackage name="Germany">
      <qualifierSet definition="lang-de-de"/>
      <qualifierSet definition="lang-es-es"/>
    </resourcePackage>  
    <resourcePackage name="France">
      <qualifierSet definition="lang-fr-fr"/>
    </resourcePackage>  
    <resourcePackage name="HighRes1">
      <qualifierSet definition="scale-200"/>
    </resourcePackage>
    <resourcePackage name="HighRes2">
      <qualifierSet definition="scale-400"/>
    </resourcePackage>
  </packaging>
```

MakePri.exe ne bloque pas explicitement la génération de fichiers PRI de ressource sur une dimension spécifique. Les restrictions le long d’un certain ensemble de dimensions sont définies et implémentées en externe par MakeAppx.exe ou d’autres outils dans le pipeline.

MakePri.exe analyse l' `packaging` élément après tous les `index` nœuds pour remplir tous les qualificateurs par défaut. MakePri.exe collecte des informations analysées dans ces structures de données.

```csharp
enum ResourcePackageMode
{
    None,
    AutoPackQualifier,
    ManualPack
}

ResourcePackageMode eResourcePackageMode;
list<string> RPQualifierList; // To store AutoResourcePackage Qualifiers
map<string, list<string>> RPNameToQSIMap; // To store ResourcePackage name to QualifierSet list mapping.
```

## <a name="resourcesisdeploymentmergeable-attribute"></a>Attribut resources@isDeploymentMergeable

Cet attribut définit un indicateur dans le fichier PRI qui provoque

- Fusion du déploiement pour identifier que ce fichier PRI peut être fusionné.
- GetFullyQualifiedReference pour retourner une erreur au cas où cet indicateur est défini et que Resource Manager a été initialisé avec un fichier.

La valeur par défaut de cet attribut est `true`. MakePri.exe définit uniquement l’indicateur dans PRI si vous ciblez Windows 10.

Nous vous recommandons d’omettre `isDeploymentMergeable` (ou de lui affecter explicitement la valeur `true` ) pour la création du Pack de ressources si vous ciblez Windows 10.

MakePri.exe ajoute la valeur de `isDeploymentMergeable` au fichier dump si `makepri dump` est exécuté avec l' `/dt detailed` option.

```xml
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<PriInfo>
    <PriHeader>
        ...
        <IsDeploymentMergeable>true</IsDeploymentMergeable>
        ...
    </PriHeader>
  ...
</PriInfo>
```

## <a name="resourcesmajorversion-attribute"></a>Attribut resources@majorVersion

La valeur par défaut de cet attribut est 1. Si vous fournissez une valeur explicite et si vous utilisez également l' `/VersionMajor(vma)` option de ligne de commande déconseillée pour l’outil MakePri.exe, la valeur dans le fichier de configuration est prioritaire.

Voici un exemple.

```xml
<resources majorVersion="2">
  <packaging ... />
  <index root="\" startIndexAt="\">
    ...
  </index>
</resources>
```

## <a name="resourcestargetosversion-attribute"></a>Attribut resources@targetOsVersion

Indique la version du système d’exploitation cible. Le tableau ci-dessous montre les valeurs prises en charge. la valeur par défaut est 6.3.0.

| Valeur | Signification |
| ----- | ------- |
| 10.0.0 | Windows 10 |
| 6.3.0 (par défaut) | Windows 8.1 |
| 6.2.1 | Windows 8 |

Voici un exemple.

```xml
<resources targetOsVersion="10.0.0">
  <packaging ... />
  <index root="\" startIndexAt="\">
    ...
  </index>
</resources>
```

**Remarque** Windows est à compatibilité descendante en ce qui concerne les fichiers PRI ; mais pas toujours une compatibilité ascendante.

MakePri.exe ajoute la valeur de `targetOsVersion` au fichier dump si `makepri dump` est exécuté avec l' `/dt detailed` option.

```xml
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<PriInfo>
    <PriHeader>
        ...
        <TargetOS version="10.0.0"/>
        ...
    </PriHeader>
  ...
</PriInfo>
```

## <a name="validation-error-messages"></a>Messages d’erreur de validation

Voici quelques exemples de conditions d’erreur et le message d’erreur correspondant.

| Condition | severity | Message |
| --------- | -------- | ------- |
| Un targetOsVersion autre que l’une des valeurs prises en charge est spécifié. | Erreur | Configuration non valide : targetOsVersion spécifié non valide. |
| Un targetOsVersion de « 6.2.1 » est spécifié et un `packaging` élément est présent. | Erreur | Configuration non valide : le nœud’Packaging’n’est pas pris en charge avec ce targetOsVersion. |
| Plus d’un mode trouvé dans la configuration. Par exemple, Manual et AutoResourcePackage sont spécifiés. | Erreur | Configuration non valide : le nœud’Packaging’ne peut pas avoir plus d’un mode de fonctionnement. |
| Un qualificateur par défaut est listé sous package de ressources. | Erreur | Configuration non valide : <Qualifiername> = <QualifierValue> est un qualificateur par défaut et ses candidats ne peuvent pas être ajoutés à un package de ressources. |
| Le qualificateur AutoResourcePackage contient plusieurs qualificateurs. Par exemple, language_scale. | Erreur | Configuration non valide : AutoResourcePackage avec plusieurs qualificateurs n’est pas pris en charge. |
| ResourcePackage QualifierSet contient plusieurs qualificateurs. Par exemple, language-en-us_scale-100 | Erreur | Configuration non valide : QualifierSet avec plusieurs qualificateurs n’est pas pris en charge. |
| Nom de ResourcePack en double trouvé. | Erreur | Configuration non valide : nom du Pack de ressources en double <rpname> . |
| Même qualificateur défini dans deux packages de ressources. | Erreur | Configuration non valide : plusieurs instances de QualifierSet " <qualifier tags> " ont été trouvées. |
| Aucun candidat n’a été trouvé pour les QualifierSet répertoriés pour le nœud « ResourcePackage ». | Avertissement | Configuration non valide : aucun candidat trouvé pour <Resource Package Name> . |
| Aucun candidat trouvé pour le qualificateur listé sous le nœud « AutoResourcePackage ». | Avertissement | Configuration non valide : aucun candidat trouvé pour le qualificateur <qualifier name> . Package de ressources non généré. |
| Aucun des modes n’a été trouvé. Autrement dit, un nœud « Packaging » vide a été trouvé. | Avertissement | Configuration non valide : aucun mode de Packaging n’est spécifié. |

## <a name="related-topics"></a>Rubriques connexes

* [Compiler des ressources manuellement avec MakePri.exe](compile-resources-manually-with-makepri.md)
* [ Options de ligne de commandeMakePri.exe &mdash; createconfig, commande](makepri-exe-command-options.md#createconfig-command)
* [Personnaliser vos ressources pour la langue, l’échelle, le contraste élevé et d’autres qualificateurs](tailor-resources-lang-scale-contrast.md)
* [ResourceContext système de gestion des ressources &mdash;](resource-management-system.md#resourcecontext)
