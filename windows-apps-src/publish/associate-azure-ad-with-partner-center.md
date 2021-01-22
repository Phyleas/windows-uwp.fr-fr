---
description: Pour pouvoir ajouter et gérer des utilisateurs de comptes, vous devez d’abord associer votre compte espace partenaires aux Azure Active Directory de votre organisation.
title: Associer Azure Active Directory à votre compte espace partenaires
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, UWP, Azure ad, client Azure, locataire AAD, client Azure ad, gestion des locataires, locataires
ms.localizationpriority: medium
ms.openlocfilehash: 84cdaac2b98e166640f96df46dd9785d1c50fbf8
ms.sourcegitcommit: 069f5ab4be85a7d638fc2a426afaed824e5dfeae
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/21/2021
ms.locfileid: "98668720"
---
# <a name="associate-azure-active-directory-with-your-partner-center-account"></a>Associer Azure Active Directory à votre compte espace partenaires

Pour pouvoir [Ajouter et gérer des utilisateurs de comptes](add-users-groups-and-azure-ad-applications.md), vous devez d’abord associer votre compte espace partenaires aux Azure Active Directory de votre organisation. 

L' [espace partenaires](https://partner.microsoft.com/dashboard) utilise Azure AD pour l’accès et la gestion des comptes multi-utilisateurs. Si votre organisation utilise déjà Microsoft 365 ou d’autres services professionnels de Microsoft, vous avez déjà Azure AD. Dans le cas contraire, vous pouvez créer un locataire Azure AD à partir de l’espace partenaires sans frais supplémentaires.

> [!TIP]
> Cette rubrique est spécifique au programme de développement d’applications Windows dans l' [espace partenaires](https://partner.microsoft.com/dashboard), mais l’Association d’un locataire et la gestion des utilisateurs fonctionnent de la même façon pour les comptes du programme d’application de bureau Windows (voir [programme d’application de bureau Windows](/windows/desktop/appxpkg/windows-desktop-application-program#add-and-manage-account-users) [pour plus](/windows-hardware/drivers/dashboard/dashboard-administration) d’informations) et dans le programme Windows Hardware Developer (où les références au rôle **Gestionnaire** s’appliquent également aux comptes matériels **avec le rôle**

Un locataire Azure AD unique peut être associé à plusieurs comptes de l’espace partenaires. Vous n’avez besoin que d’un seul Azure AD client associé à votre compte espace partenaires pour ajouter plusieurs utilisateurs de compte, mais vous avez également la possibilité d’ajouter plusieurs locataires Azure AD à un seul compte de l’espace partenaires. Les utilisateurs disposant du rôle **Manager** dans le compte de l’Espace partenaires peuvent ajouter et supprimer des locataires Azure AD du compte.

> [!IMPORTANT]
> Une fois que vous avez associé votre compte espace partenaires à votre locataire Azure AD, pour ajouter et gérer des utilisateurs de comptes dans ce locataire, vous devez vous connecter à l’espace partenaires en tant qu’utilisateur du même locataire qui possède le rôle **Gestionnaire** .


## <a name="associate-your-partner-center-account-with-your-organizations-existing-azure-ad-tenant"></a>Associer votre compte espace partenaires au client Azure AD existant de votre organisation

Si votre organisation utilise déjà Azure AD, procédez comme suit pour lier votre compte espace partenaires.

1.  Dans l' [espace partenaires](https://partner.microsoft.com/dashboard), sélectionnez l’icône d’engrenage (près de l’angle supérieur droit du tableau de bord), puis sélectionnez **paramètres du compte**. Dans le menu **paramètres** , sélectionnez **locataires**.
2.  Sélectionnez **associer Azure ad à votre compte espace partenaires**.
3.  Saisissez les informations d’identification Azure AD du locataire que vous souhaitez associer.
4.  Passez en revue l’organisation et le nom de domaine du locataire Azure AD. Pour compléter l’association, sélectionnez **Confirmer**.
5.  Si l’association est réussie, vous pourrez ajouter et gérer les utilisateurs de compte dans la section **Utilisateurs** dans l’Espace partenaires.

> [!IMPORTANT]
> Afin de créer des utilisateurs ou d’apporter d’autres modifications à votre Azure AD, vous devez vous connecter à ce Azure AD locataire à l’aide d’un compte disposant d’une [autorisation d’administrateur général](/azure/active-directory/users-groups-roles/directory-assign-admin-roles) pour ce locataire. Toutefois, vous n’avez pas besoin d’une autorisation d’administrateur général pour associer le locataire, ou pour ajouter des utilisateurs qui existent déjà dans ce locataire à votre compte espace partenaires.


## <a name="create-a-brand-new-azure-ad-to-associate-with-your-partner-center-account"></a>Créer un tout nouveau Azure AD à associer à votre compte espace partenaires

Si vous devez configurer un nouveau Azure AD à lier à votre compte espace partenaires, procédez comme suit.

1.  Dans l' [espace partenaires](https://partner.microsoft.com/dashboard), sélectionnez l’icône d’engrenage (près de l’angle supérieur droit du tableau de bord), puis sélectionnez **paramètres du compte**. Dans le menu **paramètres** , sélectionnez **locataires**.
2.  Sélectionnez **créer un Azure ad**.
3.  Entrez les informations de répertoire de votre nouveau locataire Azure AD :
    - **Nom de domaine**: nom unique que nous allons utiliser pour votre domaine Azure ad, ainsi que « . onmicrosoft.com ». Par exemple, si vous entrez « example », votre domaine Azure AD sera « example.onmicrosoft.com ».
    - **E-mail du contact** : Une adresse e-mail à laquelle nous pouvons vous contacter sur votre compte si nécessaire.
    - **Informations du compte utilisateur de l’administrateur général** : Le prénom, nom de famille, nom d’utilisateur et mot de passe que vous souhaitez utiliser pour le nouveau compte d’administrateur général.
4.  Cliquez sur **Créer** pour confirmer les nouvelles informations de domaine et de compte.
5.  Connectez-vous avec votre Azure AD nouveau nom d’utilisateur et mot de passe d’administrateur général pour commencer à [Ajouter et à gérer des utilisateurs de comptes supplémentaires](add-users-groups-and-azure-ad-applications.md).


## <a name="manage-azure-ad-tenant-associations"></a>Gérer les associations de locataires Azure AD

Une fois que vous avez associé un client Azure AD à votre compte espace partenaires, vous pouvez ajouter de nouveaux locataires ou supprimer des locataires existants de la page **locataires** .


### <a name="add-multiple-azure-ad-tenants-to-your-partner-center-account"></a>Ajouter plusieurs locataires Azure AD à votre compte espace partenaires

Tout utilisateur disposant du rôle **Gestionnaire** pour un compte espace partenaires peut associer Azure ad locataires au compte.

Pour associer un nouveau locataire, sélectionnez **associer un autre locataire Azure ad**, puis suivez les étapes indiquées ci-dessus. Notez que vous serez invité à entrer vos informations d’identification dans le locataire Azure AD que vous souhaitez associer.


### <a name="remove-an-azure-ad-tenant-from-your-partner-center-account"></a>Supprimer un locataire Azure AD de votre compte espace partenaires

Tout utilisateur disposant du rôle **Gestionnaire** pour un compte espace partenaires peut supprimer Azure ad locataires du compte.

> [!IMPORTANT]
> Lorsque vous supprimez un locataire, tous les utilisateurs qui ont été ajoutés au compte Espace partenaires depuis ce locataire ne pourront plus se connecter au compte. 

Pour supprimer un locataire, recherchez son nom dans la page **locataires** (dans **paramètres du compte**), puis sélectionnez **supprimer**. Un message va s’afficher pour vous demander de confirmer la suppression du locataire. Une fois cette opération effectuée, les utilisateurs de ce locataire ne pourront plus se connecter au compte de l’Espace partenaires et les autorisations que vous avez configurées pour ces utilisateurs seront supprimées.

> [!TIP]
> Vous ne pouvez pas supprimer un locataire si vous êtes actuellement connecté à l’Espace partenaires à l’aide d’un compte de ce locataire. Pour supprimer un locataire, vous devez vous connecter à l’Espace partenaires en tant que **Manager** pour un autre locataire associé au compte. S’il n’existe qu’un seul locataire associé au compte, ce locataire peut être supprimé uniquement après vous être connecté avec le compte Microsoft qui a ouvert le compte.
