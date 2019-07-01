---
title: Préparer votre application pour le changement d’ère du Japon
description: En savoir plus sur le changement d’ère du Japon de mai 2019 et comment préparer votre application.
ms.assetid: 5A945F9A-8632-4038-ADD6-C0568091EF27
ms.date: 4/26/2019
ms.topic: article
keywords: windows 10, uwp, adaptabilité, localisation, Japon, ère
ms.localizationpriority: high
ms.openlocfilehash: 54d66d0426e5f0c41d48b93ba96781786d6fab92
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66363779"
---
# <a name="prepare-your-application-for-the-japanese-era-change"></a>Préparer votre application pour le changement d’ère du Japon

> [!NOTE]
> Le nom de la nouvelle ère a été annoncé le 1 avril 2019 : Reiwa (令和). Le 25 avril, Microsoft a publié des packages pour les différents systèmes d’exploitation de Windows contenant la clé de Registre mises à jour avec le nom de la nouvelle ère. Mettre à jour votre appareil et vérifier votre Registre pour voir s’il possède la nouvelle clé et ensuite tester votre application. Vérifiez [cet article du support technique](https://support.microsoft.com/help/4469068/summary-of-new-japanese-era-updates-kb4469068) s’assurer que votre système d’exploitation doit avoir reçu la clé de Registre mises à jour.

Le calendrier japonais est divisé en ères et pour la plupart de l’ère de l’informatique moderne, nous avons été dans l’ère Heisei ; Toutefois, sur 1 mai 2019, une nouvelle ère commence. Dans la mesure où il s’agit de la première fois depuis plusieurs décennies qu’une ère change, les logiciels prenant en charge le calendrier japonais doivent être testés, pour vérifier qu’ils fonctionneront correctement lorsque la nouvelle ère commencera.

Dans les sections suivantes, vous allez découvrir ce que vous pouvez faire pour préparer et tester votre application pour la nouvelle ère à venir.

> [!NOTE]
> Nous vous recommandons d’utiliser un ordinateur de test pour ce faire, car les modifications que vous apporterez auront un impact sur le comportement de l’intégralité de la machine.

## <a name="add-a-registry-key-for-the-new-era"></a>Ajoutez une clé de Registre pour la nouvelle ère

> [!NOTE]
> Les instructions suivantes sont destinées aux appareils qui n’ont pas encore été mis à jour avec la nouvelle clé de Registre. Tout d’abord vérifier si votre appareil contient la clé de Registre et dans le cas contraire, testez en suivant les instructions suivantes.

Il est important de tester les problèmes de compatibilité avant l’ère a changé, et vous pouvez faire à présent en utilisant le nom de la nouvelle ère. Pour ce faire, ajoutez une clé de Registre pour la nouvelle ère dans l’**Éditeur du Registre** :

1. Accédez à **Computer\HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Nls\Calendars\Japanese\Eras**.
2. Sélectionnez **Édition > Nouveau > Valeur de chaîne**et donnez-lui le nom **2019 05 01**.
3. Cliquez avec le bouton droit sur la clé, puis sélectionnez **Modifier**.
4. Dans le **données de la valeur** , entrez**令和_令_Reiwa_R** (vous pouvez copier et coller à partir d’ici pour faciliter leur).

Voir [Gestion des ères pour le calendrier japonais](https://docs.microsoft.com/windows/desktop/Intl/era-handling-for-the-japanese-calendar) pour en savoir plus sur le format de ces clés de Registre.

Le nom de la nouvelle ère a été annoncé le 1 avril 2019. Le 25 avril, une mise à jour avec une nouvelle clé de Registre pour les versions Windows prises en charge qui contient le nom a été publié, ce qui vous permet de valider que votre application gère correctement. Cette mise à jour est propagée à 7 et versions antérieures de Windows 10, ainsi que Windows 8 pris en charge.

Vous pouvez supprimer votre clé de Registre d’espace réservé une fois que vous avez terminé de tester votre application. Cela permet de garantir qu’elle n’interfère pas avec la nouvelle clé de Registre qui est ajoutée lors de la mise à jour de Windows.

## <a name="change-your-devices-calendar-format"></a>Modifier le format de calendrier de votre appareil

Une fois que vous avez ajouté la clé de Registre pour la nouvelle ère, vous devez configurer votre appareil pour utiliser le calendrier japonais. Vous n’avez pas besoin de disposer d’un périphérique en japonais pour effectuer cette opération. Pour réaliser des tests approfondis, vous pouvez installer le pack de langue japonaise également, mais cela n’est pas nécessaire pour le test de base.

Pour configurer votre appareil pour utiliser le calendrier japonais :

1. Ouvrez **intl.cpl** (recherchez ce fichier dans la barre de recherche Windows).
2. Dans la liste déroulante **Format**, sélectionnez **Japonais (Japon)** .
3. Sélectionnez **Paramètres supplémentaires**.
4. Sélectionnez l’onglet **Date**.
5. Dans la liste déroulante **Type de calendrier**, sélectionnez**和暦**(*wareki*, le calendrier japonais). Il doit être la deuxième option.
6. Cliquez sur **OK**.
7. Cliquez sur **OK** dans la fenêtre **Région** .

Votre appareil doit maintenant être configuré pour utiliser le calendrier japonais, et qu’il reflète les ères présentes dans le Registre. Voici un exemple de ce que vous voyiez maintenant dans l’angle inférieur droit de l’écran :

![Date et heure au format de calendrier japonais](images/japanese-calendar-format.png)

## <a name="adjust-your-devices-clock"></a>Ajuster l’horloge de votre appareil

Pour vérifier que votre application fonctionne avec la nouvelle ère, vous devez avancer l’horloge de votre ordinateur au 1er mai 2019 ou version ultérieure. Les instructions suivantes concernent Windows 10, mais Windows 8 et 7 devraient fonctionner de la même façon :

1. Cliquez avec le bouton droit dans la zone de date et d’heure dans l’angle inférieur droit de l’écran.
2. Sélectionnez **Ajuster la date/l’heure**.
3. Dans l’application Paramètres, sous **Modifier la date et l’heure**, sélectionnez **Modifier**.
4. Modifier la date le 1er mai 2019 ou après.

> [!NOTE]
> Vous n’êtes peut-être pas en mesure de modifier la date en fonction des paramètres de l’organisation ; Si c’est le cas, contactez votre administrateur. Vous pouvez également modifier votre clé de Registre espace réservé ont une date qui est déjà passée.

## <a name="test-your-application"></a>Tester votre application

Maintenant, testez la façon dont votre application gère la nouvelle ère. Vérifiez les emplacements où s’affiche la date, comme les horodatages et les sélecteurs de date. Assurez-vous que l’ère est correcte avant le 1 mai 2019 (Heisei, 平成) et après (Reiwa, 令和).

### <a name="gannen-"></a>*Gannen* (元年)

Le format pour le calendrier japonais est généralement  **&lt;nom de l’ère&gt; &lt;année d’ère&gt;** . Par exemple, l’année 2018 correspond à **Heisei 30** (平成30年).  Toutefois, la première année d’une ère est spéciale ; au lieu d’être **&lt;Nom de l’ère&gt; 1**, elle correspond à **&lt;Nom de l’ère&gt; 元年** (*gannen*). Par conséquent, la première année de l’ère Heisei serait 平成元年 (*Heisei gannen*). Assurez-vous que votre application gère correctement la première année de la nouvelle ère et renvoie correctement 令和元年.

## <a name="related-apis"></a>API associées

Il existe plusieurs API WinRT, .NET et Win32 qui seront mises à jour pour gérer le changement d’ère, si vous les utilisez, vous ne devriez pas rencontrer de problèmes. Toutefois, même si vous vous reposez entièrement sur ces API, il est toujours judicieux de tester votre application et de vérifier que vous obtenez le comportement souhaité, surtout si vous réalisez des opérations particulières, comme analyser.

Vous pouvez suivre les mises à jour pour le système d’exploitation et les kits de développement logiciel au [pour les mises à jour peuvent 2019 Japon ère modification](https://support.microsoft.com/help/4470918/updates-for-may-2019-japan-era-change).

Les API suivantes seront impactées :

### <a name="winrt"></a>WinRT

* [Windows.Globalization Namespace](https://docs.microsoft.com/uwp/api/windows.globalization)
  * [Classe de calendrier](https://docs.microsoft.com/uwp/api/windows.globalization.calendar)
    * [AddDays, méthode](https://docs.microsoft.com/uwp/api/windows.globalization.calendar.adddays)
    * [AddEras (méthode)](https://docs.microsoft.com/uwp/api/windows.globalization.calendar.adderas)
    * [AddHours (méthode)](https://docs.microsoft.com/uwp/api/windows.globalization.calendar.addhours)
    * [AddMinutes (méthode)](https://docs.microsoft.com/uwp/api/windows.globalization.calendar.addminutes)
    * [AddMonths (méthode)](https://docs.microsoft.com/uwp/api/windows.globalization.calendar.addmonths)
    * [AddNanoseconds (méthode)](https://docs.microsoft.com/uwp/api/windows.globalization.calendar.addnanoseconds)
    * [AddPeriods, méthode](https://docs.microsoft.com/uwp/api/windows.globalization.calendar.addperiods)
    * [AddSeconds, méthode](https://docs.microsoft.com/uwp/api/windows.globalization.calendar.addseconds)
    * [AddWeeks (méthode)](https://docs.microsoft.com/uwp/api/windows.globalization.calendar.addweeks)
    * [AddYears (méthode)](https://docs.microsoft.com/uwp/api/windows.globalization.calendar.addyears)
    * [Propriété de l’ère](https://docs.microsoft.com/uwp/api/windows.globalization.calendar.era)
    * [EraAsString (méthode)](https://docs.microsoft.com/uwp/api/windows.globalization.calendar.eraasstring)
    * [Propriété de FirstYearInThisEra](https://docs.microsoft.com/uwp/api/windows.globalization.calendar.firstyearinthisera)
    * [Propriété de LastEra](https://docs.microsoft.com/uwp/api/windows.globalization.calendar.lastera)
    * [Propriété de LastYearInThisEra](https://docs.microsoft.com/uwp/api/windows.globalization.calendar.lastyearinthisera)
    * [NumberOfYearsInThisEra Property](https://docs.microsoft.com/uwp/api/windows.globalization.calendar.numberofyearsinthisera)
* [Windows.Globalization.DateTimeFormatting Namespace](https://docs.microsoft.com/uwp/api/windows.globalization.datetimeformatting)
  * [Classe de DateTimeFormatter](https://docs.microsoft.com/uwp/api/windows.globalization.datetimeformatting.datetimeformatter)
    * [Format (méthode)](https://docs.microsoft.com/uwp/api/windows.globalization.datetimeformatting.datetimeformatter.format)

### <a name="net"></a>.NET

* [Système Namespace](https://docs.microsoft.com/dotnet/api/system)
  * [Structure de date/heure](https://docs.microsoft.com/dotnet/api/system.datetime)
  * [Struct de DateTimeOffset](https://docs.microsoft.com/dotnet/api/system.datetimeoffset)
* [System.Globalization Namespace](https://docs.microsoft.com/dotnet/api/system.globalization)
  * [Classe de calendrier](https://docs.microsoft.com/dotnet/api/system.globalization.calendar)
  * [Classe DateTimeFormatInfo](https://docs.microsoft.com/dotnet/api/system.globalization.datetimeformatinfo)
  * [Classe de comme JapaneseCalendar](https://docs.microsoft.com/dotnet/api/system.globalization.japanesecalendar)
  * [Classe de JapaneseLunisolarCalendar](https://docs.microsoft.com/dotnet/api/system.globalization.japaneselunisolarcalendar)

### <a name="win32"></a>Win32

* [datetimeapi.h header](https://docs.microsoft.com/windows/desktop/api/datetimeapi/)
  * [GetDateFormatA (fonction)](https://docs.microsoft.com/windows/desktop/api/datetimeapi/nf-datetimeapi-getdateformata)
  * [GetDateFormatEx (fonction)](https://docs.microsoft.com/windows/desktop/api/datetimeapi/nf-datetimeapi-getdateformatex)
  * [GetDateFormatW (fonction)](https://docs.microsoft.com/windows/desktop/api/datetimeapi/nf-datetimeapi-getdateformatw)
* [winnls.h header](https://docs.microsoft.com/windows/desktop/api/winnls/)
  * [EnumDateFormatsA function](https://docs.microsoft.com/windows/desktop/api/winnls/nf-winnls-enumdateformatsa)
  * [EnumDateFormatsExA (fonction)](https://docs.microsoft.com/windows/desktop/api/winnls/nf-winnls-enumdateformatsexa)
  * [EnumDateFormatsExEx (fonction)](https://docs.microsoft.com/windows/desktop/api/winnls/nf-winnls-enumdateformatsexex)
  * [EnumDateFormatsExW function](https://docs.microsoft.com/windows/desktop/api/winnls/nf-winnls-enumdateformatsexw)
  * [EnumDateFormatsW (fonction)](https://docs.microsoft.com/windows/desktop/api/winnls/nf-winnls-enumdateformatsw)
  * [GetCalendarInfoA (fonction)](https://docs.microsoft.com/windows/desktop/api/winnls/nf-winnls-getcalendarinfoa)
  * [GetCalendarInfoEx (fonction)](https://docs.microsoft.com/windows/desktop/api/winnls/nf-winnls-getcalendarinfoex)
  * [GetCalendarInfoW (fonction)](https://docs.microsoft.com/windows/desktop/api/winnls/nf-winnls-getcalendarinfow)

## <a name="see-also"></a>Voir aussi

* [Ère gérant pour le calendrier japonais](https://docs.microsoft.com/windows/desktop/Intl/era-handling-for-the-japanese-calendar)
* [Y2K Moment du calendrier japonais](https://blogs.msdn.microsoft.com/shawnste/2018/04/12/the-japanese-calendars-y2k-moment/)
* [À l’aide du Registre pour tester la nouvelle ère japonaise sur Windows](https://blogs.msdn.microsoft.com/shawnste/2018/08/07/using-the-registry-to-test-the-new-japanese-era-on-windows/)
* [Vs Gannen du Ichinen](https://blogs.msdn.microsoft.com/shawnste/2018/11/12/gannen-vs-ichinen/)
* [Pour les mises à jour peuvent changer d’ère du Japon 2019](https://support.microsoft.com/help/4470918/updates-for-may-2019-japan-era-change)
* [Gestion d’une nouvelle ère dans le calendrier japonais dans .NET](https://devblogs.microsoft.com/dotnet/handling-a-new-era-in-the-japanese-calendar-in-net/)