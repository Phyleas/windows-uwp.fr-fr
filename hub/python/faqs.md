---
title: Questions fréquemment posées sur l’utilisation de Python sur Windows
description: Questions fréquemment posées sur l’utilisation de Python sur Windows
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: python, windows 10, microsoft, pip, py.exe, chemins d’accès aux fichiers, PYTHONPATH, déploiement python, création de package python
ms.localizationpriority: medium
ms.date: 07/19/2019
ms.openlocfilehash: 6dbf86e0f9435e44140159ebb2bcbc3d67928999
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/29/2020
ms.locfileid: "74663556"
---
# <a name="frequently-asked-questions-about-using-python-on-windows"></a>Questions fréquemment posées sur l’utilisation de Python sur Windows

## <a name="why-cant-i-pip-install-a-certain-package"></a>Pourquoi ne puis-je pas « installer un certain package à l’aide de pip ?

Une installation peut échouer pour plusieurs raisons. Dans la plupart des cas, la solution appropriée consiste à contacter le développeur du package.

La cause la plus courante de problèmes est la tentative d’installation à un emplacement que vous n’avez pas l’autorisation de modifier. Par exemple, l’emplacement d’installation par défaut peut nécessiter des privilèges d’administrateur, que Python n’aura pas par défaut. La meilleure solution consiste à créer un environnement virtuel pour y effectuer l’installation.

Certains packages comportent du code natif qui nécessite l’utilisation d’un compilateur C ou C++ pour l’installation. En général, les développeurs de packages doivent publier des versions précompilées, mais ce n’est pas souvent le cas. Certains de ces packages peuvent fonctionner si vous [installez Build Tools pour Visual Studio](https://visualstudio.microsoft.com/downloads/#build-tools-for-visual-studio-2019) et sélectionnez l’option C++. Dans la plupart des cas, vous devrez contacter le développeur du package.

[Suivez la discussion sur Stack Overflow](https://stackoverflow.com/questions/4750806/how-do-i-install-pip-on-windows/12476379).

## <a name="what-is-pyexe"></a>Qu’est-ce que py.exe ?

Il se peut que plusieurs versions de Python soient installées sur votre ordinateur, dans la mesure où vous travaillez sur différents types de projets Python. Étant donné que tous utilisent la commande `python`, il peut être difficile de déterminer la version de Python vous utilisez. En guise de norme, il est recommandé d’utiliser la commande `python3` (ou `python3.7` pour sélectionner une version spécifique).

Le [service de lancement py.exe](https://docs.python.org/3/using/windows.html#launcher) sélectionne automatiquement la version installée de Python la plus récente. Vous pouvez également utiliser des commandes comme `py -3.7` pour sélectionner une version particulière, ou `py --list` pour voir quelles versions peuvent être utilisées. **TOUTEFOIS**, le service de lancement py.exe ne fonctionne que si vous utilisez une version de Python installée à partir de [python.org](https://www.python.org/downloads/windows/). Lorsque vous installez Python à partir de Microsoft Store, la commande `py`**n’est pas incluse**. Pour Linux, macOS, WSL et la version Microsoft Store de Python, vous devez utiliser la commande `python3` (ou `python3.7`).

## <a name="why-does-running-pythonexe-open-the-microsoft-store"></a>Pourquoi l’exécution du fichier python.exe ouvre-t-elle le Microsoft Store ?

Pour aider les nouveaux utilisateurs à trouver la version installée adéquate de Python, nous avons ajouté un raccourci à Windows qui permet d’accéder directement à la dernière version du package de la communauté publiée dans le Microsoft Store. Ce package peut être installé facilement, sans autorisations d’administrateur, et remplace les commandes `python` et `python3` par les commandes réelles.

L’exécution du fichier exécutable du raccourci avec n’importe quel argument de ligne de commande renverra un code d’erreur pour indiquer que Python n’a pas été installé. Cela permet d’éviter que des fichiers et scripts batch ouvrent l’application du Windows Store sans raison.

Si vous installez Python à l’aide des programmes d’installation depuis [python.org](https://www.python.org/downloads/windows/) et sélectionnez l’option « Ajouter au chemin », la nouvelle commande `python` prend la priorité sur le raccourci. Notez que les autres programmes d’installation peuvent ajouter `python` selon un niveau de priorité _inférieur_ à celui du raccourci intégré.

Pour désactiver les raccourcis sans installer Python, ouvrez « Gérer les alias d’exécution de l’application » depuis le menu Démarrer, recherchez les entrées Python « Programme d’installation d’application » et désactivez-les.

## <a name="why-dont-file-paths-work-in-python-when-i-copy-paste-them"></a>Pourquoi les chemins d’accès aux fichiers ne fonctionnent-ils pas dans Python lorsque je les copie-colle ?

Les chaînes python utilisent des « échappements » pour les caractères spéciaux. Par exemple, pour insérer un caractère de nouvelle ligne dans une chaîne, vous devez taper `\n`. Comme les chemins d’accès aux fichiers sur Windows utilisent des barres obliques inverses, certains éléments peuvent être convertis en caractères spéciaux.

Pour coller un chemin d’accès en tant que chaîne dans Python, ajoutez le préfixe `r`. Cela indique qu’il s’agit d’une chaîne `raw`. Ainsi, aucun caractère d’échappement n’est utilisé à l’exception de \” (vous devrez peut-être supprimer la dernière barre oblique inverse de votre chemin d’accès). Votre chemin peut donc ressembler à ceci : `r"C:\Users\MyName\Documents\Document.txt"`

Lorsque vous utilisez des chemins d’accès dans Python, nous vous recommandons d’utiliser le module pathlib standard. Celui-ci permet de convertir la chaîne en un objet de chemin d’accès enrichi pouvant effectuer les manipulations de chemin d’accès de manière cohérente, qu’il utilise des barres obliques ou des barres obliques inverses, afin que votre code fonctionne mieux sur différents systèmes d’exploitation.

## <a name="what-is-pythonpath"></a>Qu’est-ce que PYTHONPATH ?

La variable d’environnement PYTHONPATH est utilisée par Python pour spécifier une liste de répertoires à partir desquels les modules peuvent être importés. Lors de l’exécution, vous pouvez inspecter la variable `sys.path` pour connaître les répertoires dans lesquels effectuer la recherche lorsque vous importez un événement.

Pour définir cette variable à partir de l’invite de commandes, utilisez : `set PYTHONPATH=list;of;paths`.

Pour définir cette variable à partir de PowerShell, utilisez : `$env:PYTHONPATH=’list;of;paths’` juste avant de lancer Python.

La définition globale de cette variable via les paramètres de **variables d’environnement** n’est **pas** recommandée, car elle peut être utilisée par n’importe quelle version de Python au lieu de celle que vous prévoyez d’utiliser.

## <a name="where-can-i-find-help-with-packaging-and-deployment"></a>Où puis-je trouver de l’aide sur la création de packages et le déploiement ?

[Docker](https://code.visualstudio.com/docs/azure/docker) : [VSCode extension](https://code.visualstudio.com/docs/azure/docker) vous aide à créer rapidement des pacakges et à les déployer avec des modèles fichier Dockerfile et docker-compose.yml (générez les fichiers Docker appropriés pour votre projet).

[Azure Kubernetes Service (AKS)](https://docs.microsoft.com/azure/aks/) permet de déployer et de gérer des applications conteneurisées tout en effectuant la montée en charge des ressources à la demande.

## <a name="what-if-i-need-to-work-across-different-machines"></a>Que se passe-t-il si je dois travailler sur différents ordinateurs ?

[Settings Sync](https://marketplace.visualstudio.com/items?itemName=Shan.code-settings-sync) permet de synchroniser vos paramètres VS Code sur différentes installations à l’aide de GitHub. Si vous travaillez sur plusieurs ordinateurs, cela permet de garantir la cohérence de votre environnement.

## <a name="what-if-im-used-to-using-pycharm-atom-sublime-text-emacs-or-vim"></a>Que se passe-t-il si j’utilise PyCharm, Atom, Sublime Text, Emacs, ou Vim ?

L’extension VSCode [Keymaps](https://marketplace.visualstudio.com/search?target=VSCode&category=Keymaps&sortBy=Downloads) peut permettre de personnaliser votre environnement.

## <a name="how-do-mac-shortcut-keys-map-to-windows-shortcut-keys"></a>Comment les touches de raccourci Mac sont-elles mappées aux touches de raccourci de Windows?

Certains boutons du clavier et raccourcis système sont légèrement différents entre un ordinateur Windows et un Macintosh. Ce [guide de transition Mac vers Windows](../dev-environment/mac-to-windows.md) aborde les notions de base.
