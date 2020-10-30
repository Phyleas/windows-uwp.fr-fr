---
description: Cette rubrique décrit les indexeurs spécifiques au format utilisés par l’outil MakePri.exe pour générer son index de ressources.
title: Indexeurs spécifiques au format de MakePri.exe
template: detail.hbs
ms.date: 10/18/2017
ms.topic: article
keywords: windows 10, uwp, ressources, image, MRT, qualificateur
ms.localizationpriority: medium
ms.openlocfilehash: 3794d369ae9d47cfc7aad1b24ca2768b04024581
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93031692"
---
# <a name="makepriexe-format-specific-indexers"></a>Indexeurs spécifiques au format de MakePri.exe

Cette rubrique décrit les indexeurs spécifiques au format utilisés par l’outil [MakePri.exe](compile-resources-manually-with-makepri.md) pour générer son index de ressources.

> [!NOTE]
> MakePri.exe est installé lorsque vous activez l’option **SDK Windows pour les applications gérées UWP** lors de l’installation du kit de développement logiciel (SDK) Windows. Il est installé dans le chemin d’accès `%WindowsSdkDir%bin\<WindowsTargetPlatformVersion>\x64\makepri.exe` (et dans les dossiers nommés pour les autres architectures). Par exemple : `C:\Program Files (x86)\Windows Kits\10\bin\10.0.17713.0\x64\makepri.exe`.

MakePri.exe est généralement utilisé avec les `new` `versioned` commandes, ou `resourcepack` . Consultez [MakePri.exe options de ligne de commande](makepri-exe-command-options.md). Dans ce cas, il indexe les fichiers sources pour générer un index des ressources. MakePri.exe utilise différents indexeurs individuels pour lire des conteneurs ou fichiers de ressources sources différents pour les ressources. L’indexeur de dossier le plus simple est l’indexeur de dossiers, qui indexe le contenu d’un dossier, tel que les `.jpg` `.png` images ou.

Vous identifiez les indexeurs spécifiques au format en spécifiant `<indexer-config>` des éléments dans un `<index>` élément du [ fichier de configurationMakePri.exe](makepri-exe-configuration.md). L' `type` attribut identifie l’indexeur spécifique au format utilisé.

Les conteneurs de ressources rencontrés pendant l’indexation obtiennent généralement leur contenu indexé au lieu d’être ajoutés à l’index proprement dit. Par exemple, les `.resjson` fichiers trouvés par l’indexeur de dossier peuvent être indexés par un `.resjson` indexeur, auquel cas le `.resjson` fichier lui-même n’apparaît pas dans l’index. **Notez** `<indexer-config>` qu’un élément de l’indexeur associé à ce conteneur doit être inclus dans le fichier de configuration pour que cela se produise.

En règle générale, les qualificateurs d’une entité conteneur, &mdash; tels qu’un dossier ou un `.resw` fichier &mdash; , sont appliqués à toutes les ressources qu’il contient, telles que les fichiers dans le dossier ou les chaînes dans le `.resw` fichier.

## <a name="folder"></a>Dossier

L’indexeur de dossier est identifié par un `type` attribut de Folder. Il indexe le contenu d’un dossier et détermine les qualificateurs de ressources à partir du dossier et des noms de fichiers. Son élément de configuration est conforme au schéma suivant.

```xml
<xs:schema attributeFormDefault="unqualified" elementFormDefault="qualified" xmlns:xs="http://www.w3.org/2001/XMLSchema">
    <xs:simpleType name="ExclusionTypeList">
        <xs:restriction base="xs:string">
            <xs:enumeration value="path"/>
            <xs:enumeration value="extension"/>
            <xs:enumeration value="name"/>
            <xs:enumeration value="tree"/>
        </xs:restriction>
    </xs:simpleType>
    <xs:complexType name="FolderExclusionType">
        <xs:attribute name="type" type="ExclusionTypeList" use="required"/>
        <xs:attribute name="value" type="xs:string" use="required"/>
        <xs:attribute name="doNotTraverse" type="xs:boolean" use="required"/>
        <xs:attribute name="doNotIndex" type="xs:boolean" use="required"/>
    </xs:complexType>
    <xs:simpleType name="IndexerConfigFolderType">
        <xs:restriction base="xs:string">
            <xs:pattern value="((f|F)(o|O)(l|L)(d|D)(e|E)(r|R))"/>
        </xs:restriction>
    </xs:simpleType>
    <xs:element name="indexer-config">
        <xs:complexType>
            <xs:sequence>
                <xs:element name="exclude" type="FolderExclusionType" minOccurs="0" maxOccurs="unbounded"/>
            </xs:sequence>
            <xs:attribute name="type" type="IndexerConfigFolderType" use="required"/>
            <xs:attribute name="foldernameAsQualifier" type="xs:boolean" use="required"/>
            <xs:attribute name="filenameAsQualifier" type="xs:boolean" use="required"/>
            <xs:attribute name="qualifierDelimiter" type="xs:string" use="required"/>
        </xs:complexType>
    </xs:element>
</xs:schema>
```

L' `qualifierDelimiter` attribut spécifie le caractère après lequel les qualificateurs sont spécifiés dans un nom de fichier, en ignorant l’extension. La valeur par défaut est ".".

## <a name="pri"></a>PRI

L’indexeur PRI est identifié par un `type` attribut de PRI. Il indexe le contenu d’un fichier PRI. En général, vous l’utilisez lors de l’indexation de la ressource contenue dans un autre assembly, une DLL, un kit de développement logiciel (SDK) ou une bibliothèque de classes dans le PRI de l’application.

Tous les noms de ressources, qualificateurs et valeurs contenus dans le fichier PRI sont directement gérés dans le nouveau fichier PRI. Toutefois, le mappage de ressources de niveau supérieur n’est pas conservé dans le PRI final. Les mappages de ressources sont fusionnés.

```xml
<xs:schema id="prifile" xmlns:xs="http://www.w3.org/2001/XMLSchema" elementFormDefault="qualified">
    <xs:simpleType name="IndexerConfigPriType">
        <xs:restriction base="xs:string">
            <xs:pattern value="((p|P)(r|R)(i|I))"/>
        </xs:restriction>
    </xs:simpleType>
    <xs:element name="indexer-config">
        <xs:complexType>
            <xs:attribute name="type" type="IndexerConfigPriType" use="required"/>
        </xs:complexType>
    </xs:element>
</xs:schema>
```

## <a name="priinfo"></a>PriInfo

L’indexeur PriInfo est identifié par un `type` attribut de PriInfo. Il indexe le contenu d’un fichier de vidage détaillé. Vous générez un fichier de vidage détaillé en exécutant `makepri dump` avec l' `/dt detailed` option. L’élément de configuration de l’indexeur est conforme au schéma suivant.

```xml
<xs:schema id="priinfo" xmlns:xs="http://www.w3.org/2001/XMLSchema" elementFormDefault="qualified">
  <xs:simpleType name="IndexerConfigPriInfoType">
    <xs:restriction base="xs:string">
      <xs:pattern value="((p|P)(r|R)(i|I)(i|I)(n|N)(f|F)(o|O))"/>
    </xs:restriction>
  </xs:simpleType>
  <xs:element name="indexer-config">
    <xs:complexType>
      <xs:attribute name="type" type="IndexerConfigPriInfoType" use="required"/>
      <xs:attribute name="emitStrings" type="xs:boolean" use="optional"/>
      <xs:attribute name="emitPaths" type="xs:boolean" use="optional"/>
    </xs:complexType>
  </xs:element>
</xs:schema>
```

Cet élément de configuration permet d’avoir des attributs facultatifs pour configurer le comportement de l’indexeur PriInfo. La valeur par défaut de `emitStrings` et de `emitPaths` est `true` . Si `emitStrings` la `true` valeur est, les candidats aux ressources avec l' `type` attribut défini sur « String » sont inclus dans l’index ; sinon, ils sont exclus. Si « emitPaths » est `true` alors le candidat aux ressources dont l' `type` attribut a la valeur « Path » est inclus dans l’index, sinon, ils sont exclus.

Voici un exemple de configuration qui comprend des types de ressources de type chaîne, mais ignore les types de ressources Path.

```xml
<indexer-config type="priinfo" emitStrings="true" emitPaths="false" />
```

Pour être indexé, un fichier de vidage doit se terminer par l’extension `.pri.xml` et doit être conforme au schéma suivant.

```xml
<xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema" elementFormDefault="qualified" >
  <xs:simpleType name="candidateType">
    <xs:restriction base="xs:string">
      <xs:pattern value="Path|String"/>
    </xs:restriction>
  </xs:simpleType>
  <xs:complexType name="scopeType">
    <xs:sequence>
      <xs:element name="ResourceMapSubtree" type="scopeType" minOccurs="0" maxOccurs="unbounded"/>
      <xs:element name="NamedResource" minOccurs="0" maxOccurs="unbounded">
        <xs:complexType>
          <xs:sequence>
            <xs:element name="Decision" minOccurs="0" maxOccurs="unbounded">
              <xs:complexType>
                <xs:sequence>
                  <xs:any processContents="skip" minOccurs="0" maxOccurs="unbounded"/>
                </xs:sequence>
                <xs:anyAttribute processContents="skip" />
              </xs:complexType>
            </xs:element>
            <xs:element name="Candidate" minOccurs="0" maxOccurs="unbounded">
              <xs:complexType>
                <xs:sequence>
                  <xs:element name="QualifierSet" minOccurs="0" maxOccurs="unbounded">
                    <xs:complexType>
                      <xs:sequence>
                        <xs:element name="Qualifier" minOccurs="0" maxOccurs="unbounded">
                          <xs:complexType>
                            <xs:attribute name="name" type="xs:string" use="required" />
                            <xs:attribute name="value" type="xs:string" use="required" />
                            <xs:attribute name="priority" type="xs:integer" use="required" />
                            <xs:attribute name="scoreAsDefault" type="xs:decimal" use="required" />
                            <xs:attribute name="index" type="xs:integer" use="required" />
                          </xs:complexType>
                        </xs:element>
                      </xs:sequence>
                      <xs:anyAttribute processContents="skip" />
                    </xs:complexType>
                  </xs:element>
                  <xs:element name="Value" type="xs:string"  minOccurs="0" maxOccurs="unbounded"/>
                </xs:sequence>
                <xs:attribute name="type" type="candidateType" use="required" />
              </xs:complexType>
            </xs:element>
          </xs:sequence>
          <xs:attribute name="name" use="required" type="xs:string" />
          <xs:anyAttribute processContents="skip" />
        </xs:complexType>
      </xs:element>
    </xs:sequence>
    <xs:attribute name="name" use="required" type="xs:string" />
    <xs:anyAttribute processContents="skip" />
  </xs:complexType>
  <xs:element name="PriInfo">
    <xs:complexType>
      <xs:sequence>
        <xs:element name="PriHeader" >
          <xs:complexType>
            <xs:sequence>
              <xs:any minOccurs ="0" maxOccurs="unbounded" processContents="skip" />
            </xs:sequence>
            <xs:anyAttribute processContents="skip" />
          </xs:complexType>
        </xs:element>
        <xs:element name="QualifierInfo">
          <xs:complexType>
            <xs:sequence>
              <xs:any minOccurs="0" maxOccurs="unbounded" processContents="skip" />
            </xs:sequence>
          </xs:complexType>
        </xs:element>
        <xs:element name="ResourceMap">
          <xs:complexType>
            <xs:sequence>
              <xs:element name="VersionInfo">
                <xs:complexType>
                  <xs:anyAttribute processContents="skip" />
                </xs:complexType>
              </xs:element>
              <xs:element minOccurs="0" maxOccurs="unbounded" name="ResourceMapSubtree" type="scopeType" />
            </xs:sequence>
            <xs:attribute name="name" type="xs:string" use="required" />
            <xs:anyAttribute processContents="skip" />
          </xs:complexType>
        </xs:element>
      </xs:sequence>
    </xs:complexType>
  </xs:element>
</xs:schema>
```

MakePri.exe prend en charge les types de vidage « Basic », « detailed », « Schema » et « Summary ». Pour configurer MakePri.exe pour émettre le type de vidage que l’indexeur PriInfo peut lire, incluez « /DumpType detailed » lors de l’utilisation de la `dump` commande.

Plusieurs éléments du `.pri.xml` fichier sont ignorés par MakePri.exe. Ces éléments sont calculés pendant l’indexation ou spécifiés dans le fichier de configuration MakePri.exe. Les noms de ressources, les qualificateurs et les valeurs qui sont contenus dans le fichier dump sont directement gérés dans le nouveau fichier PRI. Toutefois, le mappage de ressources de niveau supérieur n’est pas conservé dans le PRI final. Les mappages de ressources sont fusionnés dans le cadre de l’indexation.

Il s’agit d’un exemple de ressource de type chaîne valide à partir d’un fichier dump.

```xml
<NamedResource name="SampleString " index="96" uri="ms-resource://SampleApp/resources/SampleString ">
  <Decision index="2">
    <QualifierSet index="1">
      <Qualifier name="Language" value="EN-US" priority="900" scoreAsDefault="1.0" index="1"/>
    </QualifierSet>
  </Decision>
  <Candidate type="String">
    <QualifierSet index="1">
      <Qualifier name="Language" value="EN-US" priority="900" scoreAsDefault="1.0" index="1"/>
    </QualifierSet>
    <Value>A Sample String Value</Value>
  </Candidate>
</NamedResource>
```

Il s’agit d’un exemple de ressource de type chemin d’accès valide avec deux candidats à partir d’un fichier dump.

```xml
<NamedResource name="Sample.png" index="77" uri="ms-resource://SampleApp/Files/Images/Sample.png">
  <Decision index="2">
    <QualifierSet index="1">
      <Qualifier name="Scale" value="180" priority="500" scoreAsDefault="1.0" index="1"/>
    </QualifierSet>
    <QualifierSet index="2">
      <Qualifier name="Scale" value="140" priority="500" scoreAsDefault="0.7" index="2"/>
    </QualifierSet>
  </Decision>
  <Candidate type="Path">
    <QualifierSet index="1">
      <Qualifier name="Scale" value="180" priority="500" scoreAsDefault="1.0" index="1"/>
    </QualifierSet>
    <Value>Images\Sample.scale-180.png</Value>
  </Candidate>
  <Candidate type="Path">
    <QualifierSet index="2">
      <Qualifier name="Scale" value="140" priority="500" scoreAsDefault="1.0" index="1"/>
    </QualifierSet>
    <Value>Images\Sample.scale-140.png</Value>
  </Candidate>
</NamedResource>
```

## <a name="resfiles"></a>ResFiles

L’indexeur ResFiles est identifié par un `type` attribut de ResFiles. Il indexe le contenu d’un `.resfiles` fichier. Son élément de configuration est conforme au schéma suivant.

```xml
<xs:schema id="resx" xmlns:xs="http://www.w3.org/2001/XMLSchema" elementFormDefault="qualified">
    <xs:simpleType name="IndexerConfigResFilesType">
        <xs:restriction base="xs:string">
            <xs:pattern value="((r|R)(e|E)(s|S)(f|F)(i|I)(l|L)(e|E)(s|S))"/>
        </xs:restriction>
    </xs:simpleType>
    <xs:element name="indexer-config">
        <xs:complexType>
            <xs:attribute name="type" type="IndexerConfigResFilesType" use="required"/>
            <xs:attribute name="qualifierDelimiter" type="xs:string" use="required"/>
        </xs:complexType>
    </xs:element>
</xs:schema>
```

Un `.resfiles` fichier est un fichier texte contenant une liste plate de chemins d’accès aux fichiers. Un `.resfiles` fichier peut contenir des commentaires « // ». Voici un exemple.

<blockquote>
<pre>
Strings\component1\fr\elements.resjson
Images\logo.scale-100.png
Images\logo.scale-140.png
Images\logo.scale-180.png
</pre>
</blockquote>

## <a name="resjson"></a>ResJSON

L’indexeur ResJSON est identifié par un `type` attribut de ResJSON. Il indexe le contenu d’un `.resjson` fichier, qui est un fichier de ressources de type chaîne. Son élément de configuration est conforme au schéma suivant.

```xml
<xs:schema id="resjson" xmlns:xs="http://www.w3.org/2001/XMLSchema" elementFormDefault="qualified">
    <xs:simpleType name="IndexerConfigResJsonType">
        <xs:restriction base="xs:string">
            <xs:pattern value="((r|R)(e|E)(s|S)(j|J)(s|S)(o|O)(n|N))"/>
        </xs:restriction>
    </xs:simpleType>
    <xs:element name="indexer-config">
        <xs:complexType>
            <xs:attribute name="type" type="IndexerConfigResJsonType" use="required"/>
            <xs:attribute name="initialPath" type="xs:string" use="optional"/>
        </xs:complexType>
    </xs:element>
</xs:schema>
```

Un `.resjson` fichier contient du texte JSON (consultez [le type de média application/json pour JavaScript Object Notation (JSON)](https://www.ietf.org/rfc/rfc4627.txt)). Le fichier doit contenir un seul objet JSON avec des propriétés hiérarchiques. Chaque propriété doit être un autre objet JSON ou une valeur de chaîne.

Les propriétés JSON dont les noms commencent par un trait de soulignement (« _ ») ne sont pas compilées dans le fichier PRI final, mais sont conservées dans le fichier journal.

Le fichier peut également contenir des commentaires « // » qui sont ignorés pendant l’analyse.

L' `initialPath` attribut place toutes les ressources sous ce chemin d’accès initial en l’ajoutant au nom de la ressource. En général, vous pouvez l’utiliser lors de l’indexation des ressources de bibliothèque de classes. La valeur par défaut est vide.

## <a name="resw"></a>ResW

L’indexeur ResW est identifié par un `type` attribut de ResW. Il indexe le contenu d’un `.resw` fichier, qui est un fichier de ressources de type chaîne. Son élément de configuration est conforme au schéma suivant.

```xml
<xs:schema id="resw" xmlns:xs="http://www.w3.org/2001/XMLSchema" elementFormDefault="qualified">
    <xs:simpleType name="IndexerConfigResxType">
        <xs:restriction base="xs:string">
            <xs:pattern value="((r|R)(e|E)(s|S)(w|W))"/>
        </xs:restriction>
    </xs:simpleType>
    <xs:element name="indexer-config">
        <xs:complexType>
            <xs:attribute name="type" type="IndexerConfigResxType" use="required"/>
            <xs:attribute name="convertDotsToSlashes" type="xs:boolean" use="required"/>
            <xs:attribute name="initialPath" type="xs:string" use="optional"/>
        </xs:complexType>
    </xs:element>
</xs:schema>
```

Un `.resw` fichier est un fichier XML conforme au schéma suivant.

```xml
  <xsd:schema id="root" xmlns="" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:msdata="urn:schemas-microsoft-com:xml-msdata">
    <xsd:import namespace="http://www.w3.org/XML/1998/namespace" />
    <xsd:element name="root" msdata:IsDataSet="true">
      <xsd:complexType>
        <xsd:choice maxOccurs="unbounded">
          <xsd:element name="metadata">
            <xsd:complexType>
              <xsd:sequence>
                <xsd:element name="value" type="xsd:string" minOccurs="0" />
              </xsd:sequence>
              <xsd:attribute name="name" use="required" type="xsd:string" />
              <xsd:attribute name="type" type="xsd:string" />
              <xsd:attribute name="mimetype" type="xsd:string" />
              <xsd:attribute ref="xml:space" />
            </xsd:complexType>
          </xsd:element>
          <xsd:element name="assembly">
            <xsd:complexType>
              <xsd:attribute name="alias" type="xsd:string" />
              <xsd:attribute name="name" type="xsd:string" />
            </xsd:complexType>
          </xsd:element>
          <xsd:element name="data">
            <xsd:complexType>
              <xsd:sequence>
                <xsd:element name="value" type="xsd:string" minOccurs="0" msdata:Ordinal="1" />
                <xsd:element name="comment" type="xsd:string" minOccurs="0" msdata:Ordinal="2" />
              </xsd:sequence>
              <xsd:attribute name="name" type="xsd:string" use="required" msdata:Ordinal="1" />
              <xsd:attribute name="type" type="xsd:string" msdata:Ordinal="3" />
              <xsd:attribute name="mimetype" type="xsd:string" msdata:Ordinal="4" />
              <xsd:attribute ref="xml:space" />
            </xsd:complexType>
          </xsd:element>
          <xsd:element name="resheader">
            <xsd:complexType>
              <xsd:sequence>
                <xsd:element name="value" type="xsd:string" minOccurs="0" msdata:Ordinal="1" />
              </xsd:sequence>
              <xsd:attribute name="name" type="xsd:string" use="required" />
            </xsd:complexType>
          </xsd:element>
        </xsd:choice>
      </xsd:complexType>
    </xsd:element>
  </xsd:schema>
```

L' `convertDotsToSlashes` attribut convertit tous les caractères de point (".") trouvés dans les noms de ressources (attributs de nom d’élément de données) en barre oblique « / », sauf si les caractères de point sont compris entre « [ » et « ] ».

L' `initialPath` attribut place toutes les ressources sous ce chemin d’accès initial en l’ajoutant au nom de la ressource. En général, vous l’utilisez lors de l’indexation des ressources de bibliothèque de classes. La valeur par défaut est vide.

## <a name="related-topics"></a>Rubriques connexes

* [Compiler des ressources manuellement avec MakePri.exe](compile-resources-manually-with-makepri.md)
* [Options de ligne de commande de MakePri.exe](makepri-exe-command-options.md)
* [Fichier de configuration de MakePri.exe](makepri-exe-configuration.md)
* [Type de média application/JSON pour JavaScript Object Notation (JSON)](https://www.ietf.org/rfc/rfc4627.txt)
