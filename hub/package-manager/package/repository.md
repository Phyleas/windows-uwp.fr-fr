---
title: Envoyer votre manifeste au dépôt
description: Une fois que vous avez créé un manifeste de package qui décrit votre application, vous pouvez le soumettre au dépôt du Gestionnaire de package Windows.
ms.date: 04/29/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: fef6cc96604639f84ee2c81e2de4fb28442e3f8d
ms.sourcegitcommit: 40b890c7b862f333879887cc22faff560c49eae6
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/16/2020
ms.locfileid: "97598800"
---
# <a name="submit-your-manifest-to-the-repository"></a>Envoyer votre manifeste au dépôt

[!INCLUDE [preview-note](../../includes/package-manager-preview.md)]

Une fois que vous avez créé un [manifeste de package](manifest.md) qui décrit votre application, vous pouvez l’envoyer au dépôt du Gestionnaire de package Windows. Il s’agit d’un dépôt public qui contient une collection de manifestes accessibles par l’outil **winget**. Pour envoyer votre manifeste, vous devez le charger sur le dépôt open source [https://github.com/microsoft/winget-pkgs](https://github.com/microsoft/winget-pkgs) sur GitHub.

Une fois que vous avez envoyé une demande de tirage (pull request) pour ajouter un nouveau manifeste au dépôt GitHub, un processus automatisé valide votre fichier manifeste et vérifie que le package n’est pas considéré comme malveillant. Si la validation réussit, votre package est ajouté au dépôt du Gestionnaire de package Windows public afin de pouvoir être découvert par l’outil client **winget**. Notez la distinction entre les manifestes du dépôt GitHub open source et le dépôt du Gestionnaire de package Windows public.

> [!IMPORTANT]
> Microsoft se réserve le droit de refuser tout envoi qu’elle qu’en soit la raison.

## <a name="third-party-repositories"></a>Dépôts tiers

Il n’existe actuellement aucun dépôt tiers connu. Microsoft collabore avec plusieurs partenaires afin de développer des protocoles ou une API pour activer des dépôts tiers.

## <a name="manifest-validation"></a>Validation du manifeste

Quand vous envoyez un manifeste au dépôt [https://github.com/microsoft/winget-pkgs](https://github.com/microsoft/winget-pkgs) sur GitHub, votre manifeste est validé et évalué automatiquement pour la sécurité de l’écosystème Windows. Les manifestes peuvent également être examinés manuellement.

## <a name="how-to-submit-your-manifest"></a>Comment envoyer votre manifeste ?

Pour envoyer un manifeste au dépôt, effectuez les étapes suivantes.

### <a name="step-1-validate-your-manifest"></a>Étape 1 : Valider votre manifeste

L’outil **winget** fournit la commande [validate](..\winget\validate.md) pour confirmer que vous avez créé votre manifeste correctement. Pour valider votre manifeste, utilisez cette commande.

```CMD
winget validate \<manifest-file>
```

Si la validation échoue, utilisez les erreurs pour rechercher le numéro de ligne et apporter une correction. Une fois votre manifeste validé, vous pouvez l’envoyer au dépôt.

### <a name="step-2-clone-the-repository"></a>Étape 2 : Cloner le dépôt

Ensuite, créez une duplication (fork) du dépôt et clonez-la.

1. Accédez à [https://github.com/microsoft/winget-pkgs](https://github.com/microsoft/winget-pkgs) dans votre navigateur, puis cliquez sur **Fork**.
    ![image de duplication](images\fork.png)

2. À partir d’un environnement de ligne de commande tel que l’invite de commandes Windows ou PowerShell, utilisez la commande suivante pour cloner votre duplication.
    ```CMD
    git clone \<your-fork-name>
    ```

 3. Si vous effectuez plusieurs envois, créez une branche plutôt qu’une duplication. Nous n’autorisons actuellement qu’un seul fichier manifeste par envoi.
    ```CMD
    git checkout -b \<branch-name>
    ```

### <a name="step-3-add-your-manifest-to-the-local-repository"></a>Étape 3 : Ajouter votre manifeste au dépôt local

Vous devez ajouter votre fichier manifeste au dépôt dans la structure de dossiers suivante :

**manifests** / **publisher** / **application** / **version.yaml**

* Le dossier **manifests** est le dossier racine pour tous les manifestes du dépôt.
* Le dossier **publisher** est le nom de la société qui publie le logiciel. Par exemple, **Microsoft**.
* Le dossier **application** est le nom de l’application ou de l’outil. Par exemple, **VSCode**.
* **version.yaml** est le nom de fichier du manifeste. Le nom de fichier doit être défini sur la version actuelle de l’application. Par exemple, **1.0.0.yaml**.

>[!IMPORTANT]
> La valeur `Id` dans le manifeste doit correspondre aux noms de l’éditeur et de l’application indiqués dans le chemin du dossier du manifeste, et la valeur `version` dans le manifeste doit correspondre à la version indiquée dans le nom de fichier. Pour plus d’informations, consultez [Créer votre manifeste de package](manifest.md#tips-and-best-practices).

### <a name="step-4-submit-your-manifest-to-the-remote-repository"></a>Étape 4 : Envoyer votre manifeste vers le dépôt distant

Vous êtes maintenant prêt à envoyer votre nouveau manifeste vers le dépôt distant.

1. Utilisez la commande `add` pour préparer l’envoi.
    ```CMD
    git add manifests\Contoso\ContosoApp\1.0.0.yaml
    ```

2. Utilisez la commande `commit` pour valider la modification et fournir des informations sur l’envoi.
    ```CMD
    git commit -m "Submitting  ContosoApp version 1.0.0.yaml"
    ```

3. Utilisez la commande `push` pour envoyer (push) les modifications au dépôt distant.
    ```CMD
    git push
    ```

### <a name="step-5-create-a-pull-request"></a>Étape 5 : Créer une demande de tirage

Une fois que vous avez envoyé vos modifications, revenez à [https://github.com/microsoft/winget-pkgs](https://github.com/microsoft/winget-pkgs) et créez une demande de tirage pour fusionner votre duplication ou branche avec la branche principale.

![image de l’onglet de demande de tirage](images\pull-request.png)

## <a name="validation-process"></a>Processus de validation

Quand vous créez une demande de tirage, cette opération démarre un processus d’automatisation qui valide le manifeste et traite votre demande de tirage. Nous ajoutons des étiquettes à votre demande de tirage afin que vous puissiez suivre la progression.

### <a name="submission-expectations"></a>Attentes concernant les envois

Tous les envois d’application au dépôt du Gestionnaire de package Windows doivent avoir un comportement correct. Voici quelques-unes des attentes concernant les envois :

* Le manifeste est conforme aux [spécifications du schéma](manifest.md#manifest-contents).
* Toutes les URL du manifeste mènent à des sites web sécurisés.
* Le programme d’installation et l’application sont sans virus. Le package peut être identifié à tort comme étant un programme malveillant. Si vous pensez qu’il s’agit d’un faux positif, vous pouvez faire analyser le programme d’installation par l’équipe Defender à partir de [cette page](https://www.microsoft.com/wdsi/filesubmission).
* L’application s’installe et se désinstalle correctement pour les administrateurs et les non-administrateurs.
* Le programme d’installation prend en charge les modes non interactifs.
* Toutes les entrées du manifeste sont justes et sans équivoque.
* Le programme d’installation provient directement du site web de l’éditeur.

### <a name="pull-request-labels"></a>Étiquettes de demande de tirage

Pendant la validation, nous appliquons une série d’étiquettes à notre demande de tirage pour indiquer la progression.

* **Needs: author feedback** : une erreur s’est produite lors de l’envoi. Nous allons vous réassigner la demande de tirage. Si vous ne résolvez pas le problème dans les 10 jours, nous fermerons la demande de tirage.
* **Manifest-Validation-Error** : le manifeste envoyé contient une erreur de syntaxe.
* **URL-Validation-Error** : une ou plusieurs URL dans l’envoi ont échoué lors de la validation [SmartScreen](/windows/security/threat-protection/microsoft-defender-smartscreen/microsoft-defender-smartscreen-overview).
* **Binary-Validation-Error** : le programme d’installation d’application envoyé a échoué au test d’analyse antivirus, ou il y a une incompatibilité de hachage.
* **Pull-Request-Error** : il y a un problème avec la demande de tirage. Par exemple, la structure de dossiers n’a pas le [format requis](#step-3-add-your-manifest-to-the-local-repository).
* **Validation-Error** : l’application envoyée a échoué à un test de validation générale.
* **Validation-Installation-Error** : l’application envoyée a échoué aux tests d’installation.
* **Validation-Uninstall-Error** : l’application envoyée a échoué aux tests de désinstallation.
* **Validation-Virus-Scan-Error** : l’application envoyée a échoué aux tests d’analyse antivirus.
* **Azure-Pipeline-Passed** : le manifeste a terminé la première partie de la validation. Après cette étape, votre demande de tirage est assignée à notre équipe de test en vue de la validation finale.
* **Validation-Completed** : la validation est terminée et votre demande de tirage va être fusionnée.