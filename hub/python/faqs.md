---
title: Forum aux questions sur l’utilisation de Python sur Windows
description: Forum aux questions sur l’utilisation de Python sur Windows
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: Python, Windows 10, Microsoft, PIP, py. exe, chemins de fichier, PYTHONPATH, déploiement Python, empaquetage python
ms.localizationpriority: medium
ms.date: 07/19/2019
ms.openlocfilehash: 4132ef0089ee707367666b4d6340333e538b1130
ms.sourcegitcommit: 13faf9dab9946295986f8edd79b5fae0db4ed0f6
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/15/2019
ms.locfileid: "72313375"
---
# <a name="frequently-asked-questions-about-using-python-on-windows"></a>Forum aux questions sur l’utilisation de Python sur Windows

## <a name="why-cant-i-pip-install-a-certain-package"></a>Pourquoi ne puis-je pas « installer PIP » d’un certain package ?

Il existe plusieurs raisons pour lesquelles une installation échoue. dans la plupart des cas, la solution appropriée consiste à contacter le développeur du package.

La cause la plus courante de problèmes est la tentative d’installation dans un emplacement que vous n’avez pas l’autorisation de modifier. Par exemple, l’emplacement d’installation par défaut peut nécessiter des privilèges d’administrateur, mais par défaut, Python ne les aura pas. La meilleure solution consiste à créer un environnement virtuel et à l’installer.

Certains packages incluent du code natif qui nécessite l’installation C++ d’un compilateur C ou. En général, les développeurs de packages doivent publier des versions précompilées, mais ce n’est pas souvent le cas. Certains de ces packages peuvent fonctionner si vous [Installez les outils de génération pour Visual Studio](https://visualstudio.microsoft.com/downloads/#build-tools-for-visual-studio-2019) et C++ sélectionnez l’option, mais dans la plupart des cas, vous devrez contacter le développeur du package.

[Suivez la discussion sur StackOverflow](https://stackoverflow.com/questions/4750806/how-do-i-install-pip-on-windows/12476379).

## <a name="what-is-pyexe"></a>Qu’est-ce que py. exe ?

Vous pouvez vous retrouver avec plusieurs versions de Python installées sur votre ordinateur, car vous travaillez sur différents types de projets Python. Étant donné que tous utilisent la commande `python`, il peut être difficile de déterminer quelle version de Python vous utilisez. En guise de norme, il est recommandé d’utiliser la commande `python3` (ou `python3.7` pour sélectionner une version spécifique).

Le [lanceur py. exe](https://docs.python.org/3/using/windows.html#launcher) sélectionne automatiquement la version la plus récente de Python que vous avez installée. Vous pouvez également utiliser des commandes comme `py -3.7` pour sélectionner une version particulière, ou `py --list` pour voir quelles versions peuvent être utilisées. **Toutefois**, le lanceur py. exe ne fonctionne que si vous utilisez une version de Python installée à partir de [Python.org](https://www.python.org/downloads/windows/). Lorsque vous installez Python à partir de la Microsoft Store, la commande `py` n’est **pas incluse**. Pour Linux, macOS, WSL et la version Microsoft Store de Python, vous devez utiliser la commande `python3` (ou `python3.7`).

## <a name="why-does-running-pythonexe-open-the-microsoft-store"></a>Pourquoi l’exécution de Python. exe ouvre-t-elle le Microsoft Store ?

Pour aider les nouveaux utilisateurs à trouver une bonne installation de Python, nous avons ajouté un raccourci vers Windows qui vous permet d’accéder directement à la dernière version du package de la communauté publiée dans le Microsoft Store. Ce package peut être installé facilement, sans autorisations d’administrateur, et remplace les commandes `python` et `python3` par défaut par les commandes réelles.

L’exécution de l’exécutable de raccourci avec les arguments de ligne de commande renverra un code d’erreur pour indiquer que Python n’a pas été installé. Cela permet d’éviter que des fichiers et scripts batch ouvrent l’application du Windows Store alors qu’elle n’était probablement pas prévue.

Si vous installez Python à l’aide des programmes d’installation de [Python.org](https://www.python.org/downloads/windows/) et que vous sélectionnez l’option « Ajouter au chemin d’accès », la nouvelle commande `python` prend la priorité sur le raccourci. Notez que les autres programmes d’installation peuvent ajouter `python` à une priorité _inférieure_ à celle du raccourci intégré.

Vous pouvez désactiver les raccourcis sans installer Python en ouvrant « gérer les alias d’exécution de l’application » dans Démarrer, en recherchant les entrées python « App installer » et en les basculant sur « désactivé ».

## <a name="why-dont-file-paths-work-in-python-when-i-copy-paste-them"></a>Pourquoi les chemins d’accès aux fichiers ne fonctionnent-ils pas dans python lorsque je les copie-colle ?

Les chaînes python utilisent des « échappements » pour les caractères spéciaux. Par exemple, pour insérer un caractère de nouvelle ligne dans une chaîne, tapez `\n`. Comme les chemins d’accès aux fichiers sur Windows utilisent des barres obliques inverses, certaines parties peuvent être converties en caractères spéciaux.

Pour coller un chemin d’accès en tant que chaîne dans Python, ajoutez le préfixe `r`. Cela indique qu’il s’agit d’une chaîne `raw`, et qu’aucun caractère d’échappement n’est utilisé à l’exception de \ "(vous devrez peut-être supprimer la dernière barre oblique inverse dans votre chemin d’accès). Votre chemin peut donc ressembler à ceci : r « C:\Users\MyName\Documents\Document.txt »

Lorsque vous utilisez des chemins d’accès dans Python, nous vous recommandons d’utiliser le module pathlib standard. Cela vous permet de convertir la chaîne en un objet de chemin d’accès riche qui peut effectuer les manipulations de chemin d’accès de manière cohérente, qu’elle utilise des barres obliques ou des barres obliques inverses, afin que votre code fonctionne mieux sur différents systèmes d’exploitation.

## <a name="what-is-pythonpath"></a>Qu’est-ce que PYTHONPATH ?

La variable d’environnement PYTHONPATH est utilisée par python pour spécifier une liste de répertoires à partir desquels les modules peuvent être importés. Lorsque vous exécutez, vous pouvez inspecter la variable `sys.path` pour connaître les répertoires dans lesquels effectuer la recherche lorsque vous importez un événement.

Pour définir cette variable à partir de l’invite de commandes, utilisez : `set PYTHONPATH=list;of;paths`.

Pour définir cette variable à partir de PowerShell, utilisez : `$env:PYTHONPATH=’list;of;paths’` juste avant de lancer Python.

Il n’est **pas** recommandé de définir cette variable globalement à l’aide des paramètres de **variables d’environnement** , car elle peut être utilisée par n’importe quelle version de Python au lieu de celle que vous prévoyez d’utiliser.

## <a name="where-can-i-find-help-with-packaging-and-deployment"></a>Où puis-je trouver de l’aide sur l’empaquetage et le déploiement ?

[Ancrage](https://code.visualstudio.com/docs/azure/docker): L' [extension VSCode](https://code.visualstudio.com/docs/azure/docker) vous aide à empaqueter et à déployer rapidement avec des modèles fichier dockerfile et docker-compose. yml (générer les fichiers d’ancrage appropriés pour votre projet).

[Azure Kubernetes service (AKS)](https://docs.microsoft.com/azure/aks/) vous permet de déployer et de gérer des applications en conteneur tout en mettant à l’échelle les ressources à la demande.

## <a name="what-if-i-need-to-work-across-different-machines"></a>Que se passe-t-il si je dois travailler sur différents ordinateurs ?

La [synchronisation des paramètres](https://marketplace.visualstudio.com/items?itemName=Shan.code-settings-sync) vous permet de synchroniser vos paramètres de vs code sur différentes installations à l’aide de github. Si vous travaillez sur des ordinateurs différents, cela permet de garantir la cohérence de votre environnement entre eux.

## <a name="what-if-im-used-to-using-pycharm-atom-sublime-text-emacs-or-vim"></a>Que se passe-t-il si j’utilise PyCharm, Atom, sublime Text, Emacs ou vim ?

Les [keymaps](https://marketplace.visualstudio.com/search?target=VSCode&category=Keymaps&sortBy=Downloads) de l’extension VSCode peuvent vous aider dans votre environnement chez vous.

## <a name="how-do-mac-shortcut-keys-map-to-windows-shortcut-keys"></a>Comment les touches de raccourci Mac sont-elles mappées aux touches de raccourci de Windows ?

Certains boutons du clavier et raccourcis système sont légèrement différents entre un ordinateur Windows et un Macintosh. Ce [Guide de transition Mac vers Windows](../dev-environment/mac-to-windows.md) couvre les principes de base.
