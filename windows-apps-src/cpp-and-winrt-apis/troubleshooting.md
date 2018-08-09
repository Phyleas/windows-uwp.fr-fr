---
author: stevewhims
description: Le tableau de résolution des problèmes et des solutions de cette rubrique peut vous être utile, que vous coupiez un nouveau code ou portiez une application existante.
title: Résolution des problèmes C++/WinRT
ms.author: stwhi
ms.date: 04/10/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp, standard, c++, cpp, winrt, projection, résolution des problèmes, HRESULT, erreur
ms.localizationpriority: medium
ms.openlocfilehash: 21f5fc4773979b2d7940b85871264e27d56d29c4
ms.sourcegitcommit: ab92c3e0dd294a36e7f65cf82522ec621699db87
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
ms.locfileid: "1832263"
---
# <a name="troubleshooting-cwinrtwindowsuwpcpp-and-winrt-apisintro-to-using-cpp-with-winrt-issues"></a>Résolution des problèmes [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)
> [!NOTE]
> **Certaines informations concernent la version préliminaire de produits susceptibles d’être considérablement modifiés d’ici leur commercialisation. Microsoft ne donne aucune garantie, expresse ou implicite, concernant les informations fournies ici.**

> [!NOTE]
> Pour plus d’informations sur la disponibilité actuelle de l'extension Visual Studio (VSIX) C++/WinRT (qui fournit la prise en charge des modèles de projet, ainsi que les propriétés et cibles MSBuild C++/WinRT), voir [Prise en charge de Visual Studio pour C++/WinRT et VSIX](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-and-the-vsix).

Cette rubrique est fournie en amont afin que vous en ayez connaissance immédiatement, même si vous n’en avez pas encore utilité. Le tableau de résolution des problèmes et des solutions ci-après peut vous être utile, que vous coupiez un nouveau code ou portiez une application existante. Si vous effectuez un portage et que vous êtes impatient d’avancer et de passer à l’étape de développement et d’exécution de votre projet, vous pouvez avancer provisoirement en commentant ou en remplaçant du code non essentiel posant problème, pour revenir ensuite combler cette lacune ultérieurement.

## <a name="tracking-down-xaml-issues"></a>Suivi des problèmes XAML
Les exceptions d’analyse XAML peuvent être difficiles à diagnostiquer&mdash;en particulier si l’exception ne présente aucun message d’erreur explicite. Assurez-vous que le débogueur est configuré pour intercepter les exceptions de première chance (pour essayer d’intercepter l’exception d’analyse le plus tôt possible). Vous pourrez peut-être inspecter la variable d’exception dans le débogueur pour déterminer si la valeur HRESULT ou le message comportent des informations utiles. Vérifiez également la fenêtre de sortie de Visual Studio pour voir si elle contient des messages d’erreur de l’analyseur XAML.

Si votre application s’arrête et que tout ce que vous savez c’est qu’une exception non gérée a été levée pendant l’analyse de balisage XAML, cela peut être le résultat d’une référence (par clé) à une ressource manquante. Il peut également s’agir d’une exception levée à l’intérieur d’une classe UserControl, d’un contrôle personnalisé ou d’un panneau de disposition personnalisé. En dernier recours, vous pouvez effectuer un fractionnement binaire. Supprimez environ la moitié du balisage d’une page XAML et réexécutez l’application. Vous saurez alors si l’erreur se situe quelque part dans la moitié que vous avez supprimée (que vous devez restaurer maintenant dans tous les cas) ou dans la partie que vous n’avez pas supprimée. Répétez ce processus en fractionnant la moitié qui contient l’erreur et ainsi de suite jusqu’à ce que vous ayez ciblé le problème.

## <a name="symptoms-and-remedies"></a>Symptômes et solutions
| Symptôme | Solution |
|---------|--------|
| Une exception est levée lors de l’exécution avec une valeur HRESULT égale à REGDB_E_CLASSNOTREGISTERED. | L’une des causes de cette erreur est que votre composant Windows Runtime ne peut pas être chargé. Assurez-vous que le fichier de métadonnées Windows Runtime du composant (`.winmd`) porte le même nom que le fichier binaire du composant (`.dll`), qui est également le nom du projet et le nom de l’espace de noms racine. Assurez-vous également que les métadonnées Windows Runtime et le fichier binaire ont été correctement copiés par le processus de génération dans le dossier `Appx` de l’application d’utilisation. Et vérifiez que le fichier `AppxManifest.xml` de l’application d’utilisation (également dans le dossier `Appx`) contient un élément **&lt;InProcessServer&gt;** qui déclare correctement la classe activable et le nom du fichier binaire. Cette erreur peut également se produire si vous commettez l’erreur d’instancier une classe runtime implémentée localement via le constructeur par défaut du type projeté. Voir [Contrôles XAML; liaison à une propriété C++/WinRT](binding-property.md) pour plus d’informations sur la façon d’utiliser correctement le type projeté dans ce cas. |
| Le compilateur C++ génère l’erreur «*'implements_type': is not a member of any direct or indirect base class of '&lt;projected type&gt;'*» ('type_implémentation': n’est pas un membre d’une classe de base directe ou indirecte du '<type projeté>'). | Cela peut se produire lorsque vous appelez **make** avec le nom complet de l’espace de noms de votre type d’implémentation (**MyRuntimeClass**, par exemple), et que vous n’avez pas encore inclus l’en-tête de ce type. Le compilateur interprète **MyRuntimeClass** comme le type projeté. La solution consiste à inclure l’en-tête pour votre type d’implémentation (`MyRuntimeClass.h`, par exemple). |
| Le compilateur C++ génère l’erreur «*attempting to reference a deleted function*» (tentative de référence d’une fonction supprimée). | Cela peut se produire lorsque vous appelez **make** et que le type d’implémentation que vous transmettez en tant que paramètre de modèle a un constructeur par défaut `= delete`. Modifiez le fichier d’en-tête du type d’implémentation et remplacez `= delete` par `= default`. Vous pouvez également ajouter un constructeur dans le fichier IDL pour la classe runtime. |
| Vous avez implémenté [**INotifyPropertyChanged**](/uwp/api/windows.ui.xaml.data.inotifypropertychanged), mais vos liaisons XAML ne sont pas mises à jour (et l’interface utilisateur ne s’inscrit pas à [**PropertyChanged**](/uwp/api/windows.ui.xaml.data.inotifypropertychanged.PropertyChanged)). | N’oubliez pas de définir `Mode=OneWay` (ou TwoWay) sur votre expression de liaison dans le balisage XAML. Voir [Contrôles XAML; liaison à une propriété C++/WinRT](binding-property.md). |
| Vous liez un contrôle d’éléments XAML à une collection observable, et une exception est levée lors de l’exécution avec le message «The parameter is incorrect» (le paramètre est incorrect). | Dans votre fichier IDL et votre implémentation, déclarez n’importe quelle collection observable comme le type **Windows.Foundation.Collections.IVector<IInspectable>**. Mais retournez un objet qui implémente **Windows.Foundation.Collections.IObservableVector<T>**, où T est votre type d’élément. Voir [Contrôles d’éléments XAML; liaison à une collection C++/WinRT](binding-collection.md).  |
| Le compilateur C++ génère une erreur de type: «*'MyImplementationType_base&lt;MyImplementationType&gt;': no appropriate default constructor available*» ('MyImplementationType_base<MyImplementationType>': aucun constructeur par défaut disponible).|Cela peut se produire lorsque vous avez dérivé à partir d’un type qui a un constructeur non trivial. Le constructeur de votre type dérivé doit transmettre les paramètres dont a besoin le constructeur du type de base. Pour un exemple élaboré, voir [Dérivation à partir d’un type qui possède un constructeur non trivial](author-apis.md#deriving-from-a-type-that-has-a-non-trivial-constructor).|
| Le compilateur C++ génère l’erreur «*cannot convert from 'const std::vector&lt;std::wstring,std::allocator&lt;_Ty&gt;&gt;' to 'const winrt::param::async_iterable&lt;winrt::hstring&gt; &'*» (impossible de convertir de 'const std::vector<std::wstring,std::allocator<_Ty>>' vers 'const winrt::param::async_iterable<winrt::hstring> &').|Cela peut se produire lorsque vous transmettez un std::vector de std::wstring vers une API Windows Runtime qui attend une collection. Pour plus d’informations, voir [Types de données C++ standard et C++/WinRT](std-cpp-data-types.md).|
| Le compilateur C++ génère l’erreur «*cannot convert from 'const std::vector&lt;winrt::hstring,std::allocator&lt;_Ty&gt;&gt;' to 'const winrt::param::async_iterable&lt;winrt::hstring&gt; &'*» (impossible de convertir de 'const std::vector<winrt::hstring,std::allocator<_Ty>>' vers 'const winrt::param::async_iterable<winrt::hstring> &').|Cela peut se produire lorsque vous transmettez un std::vector de winrt::hstring à une API Windows Runtime asynchrone qui attend une collection, et que vous n’avez ni copié, ni déplacé le vecteur vers l’appelé asynchrone. Pour plus d’informations, voir [Types de données C++ standard et C++/WinRT](std-cpp-data-types.md).|
| Lorsque vous ouvrez un projet, Visual Studio génère l’erreur: «*The application for the project is not installed*» (l’application pour le projet n’est pas installée).|Si vous ne l’avez pas déjà fait, vous devez installer les **outils Windows universels pour le développement C++** à partir de la boîte de dialogue **Nouveau projet** de Visual Studio. Si cela ne résout pas le problème, le projet peut dépendre de l’extension Visual Studio (VSIX) C++/WinRT (voir [Prise en charge de Visual Studio pour C++/WinRT et VSIX](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-and-the-vsix)).|
| Les tests du Kit de certification des applications Windows génèrent une erreur stipulant que l’une de vos classes d’exécution «*ne dérive pas d’une classe de base Windows. Toutes les classes composables doivent au final dériver d’un type dans l’espace de noms Windows*».|La classe de base ultime de chaque classe runtime *déclarée dans l’application* doit être un type provenant d’un espace de noms Windows.*. Vous pouvez dériver un modèle d’affichage de [**Windows.UI.Xaml.DependencyObject**](/uwp/api/windows.ui.xaml.dependencyobject). Vous pouvez également déclarer une classe de base pouvant être liée dérivée de **DependencyObject**, et en dériver vos modèles d’affichage.|
| Le compilateur C++ génère une erreur «*must be WinRT type*» (doit être de type WinRT) pour une spécialisation de délégué EventHandler ou TypedEventHandler.|Envisagez d’utiliser **winrt::delegate&lt;...T&gt;** à la place. Voir [Créer des événements en C++/WinRT](author-events.md).|
| Le compilateur C++ génère une erreur «*must be WinRT type*» (doit être de type WinRT) pour une spécialisation d’opération asynchrone Windows Runtime.|Envisagez de retourner une bibliothèque de modèles parallèles (PPL) [**task**](https://msdn.microsoft.com/library/hh750113) à la place. Voir [Opérations concurrentes et asynchrones](concurrency.md).|
| Le compilateur C++ génère l’erreur suivante: «*error C2220: warning treated as error - no 'object' file generated*» (erreur C2220: avertissement traité comme erreur - aucun fichier 'objet' généré).|Corrigez l’avertissement, ou définissez **C/C++** > **General** > **Treat Warning As Error** sur **No (/WX-)**.|
| Votre application se bloque, car un gestionnaire d’événements dans votre objet C++/WinRT est appelé une fois que l’objet a été supprimé.|Voir [Utiliser l’objet *this* dans un gestionnaire d’événements](handle-events.md#using-the-this-object-in-an-event-handler).|
| Le compilateur C++ génère l'erreur suivante: «*error C2338: This is only for weak ref support*» (erreur C2338: réservé à la prise en charge des références faibles).|Vous demandez une référence faible pour un type qui a transmis la structure de marqueur **winrt::no_weak_ref** comme argument de modèle à sa classe de base. Voir [Refus de la prise en charge des références faibles](weak-references.md#opting-out-of-weak-reference-support)|
| L’éditeur de liens C++ génère l'erreur suivante: «*error LNK2019: Unresolved external symbol*» (erreur LNK2019: symbole externe non résolu) pour une API à partir des en-têtes d’espaces de noms Windows pour la projection C++/ WinRT (dans l’espace de noms winrt).|L’API est déclarée en avance dans un en-tête que vous avez inclus, mais sa définition se trouve dans un en-tête que vous n’avez pas encore inclus. Incluez l’en-tête nommé pour l’espace de noms de l’API et régénérez.|