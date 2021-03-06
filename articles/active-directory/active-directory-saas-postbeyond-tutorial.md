---
title: "Didacticiel : intégration d’Azure Active Directory à PostBeyond | Microsoft Docs"
description: "Découvrez comment configurer l’authentification unique entre Azure Active Directory et PostBeyond."
services: active-directory
documentationcenter: 
author: jeevansd
manager: femila
editor: 
ms.assetid: 09992f08-ec50-4472-997f-ccbe719039e8
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/24/2017
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 6a635e6b767d976ad8b52df36c42ca4056c297e1
ms.openlocfilehash: 2424e3f8f43d4c5a1fe4c05913c6d80cda15a5d7
ms.lasthandoff: 03/01/2017


---
# <a name="tutorial-azure-active-directory-integration-with-postbeyond"></a>Didacticiel : Intégration d’Azure AD à PostBeyond
Dans ce didacticiel, vous allez apprendre à intégrer PostBeyond à Azure Active Directory (Azure AD).

L’intégration de PostBeyond à Azure AD vous offre les avantages suivants :

* Dans Azure AD, vous pouvez contrôler qui a accès à PostBeyond
* Vous pouvez autoriser vos utilisateurs à se connecter automatiquement à PostBeyond via l’authentification unique (SSO) avec leur compte Azure AD.
* Vous pouvez gérer vos comptes à un emplacement central : le portail Azure Classic.

Pour en savoir plus sur l’intégration des applications SaaS avec Azure AD, consultez [Qu’est-ce que l’accès aux applications et l’authentification unique avec Azure Active Directory ?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Conditions préalables
Pour configurer l’intégration d’Azure AD avec PostBeyond, vous avez besoin des éléments suivants :

* Un abonnement Azure AD
* Un abonnement **PostBeyond** pour lequel l’authentification unique (SSO) est activée

>[!NOTE]
>Pour tester les étapes de ce didacticiel, nous déconseillons l’utilisation d’un environnement de production. 
> 

Vous devez en outre suivre les recommandations ci-dessous :

* Vous ne devez pas utiliser votre environnement de production, sauf si cela est nécessaire.
* Si vous n’avez pas d’environnement d’essai Azure AD, vous pouvez [obtenir un essai d’un mois](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Description du scénario
Dans ce didacticiel, vous testez l’authentification unique Azure AD dans un environnement de test. Le scénario décrit dans ce didacticiel se compose des deux sections principales suivantes :

1. Ajout de PostBeyond à partir de la galerie
2. Configuration et test de l’authentification unique (SSO) Azure AD

## <a name="add-postbeyond-from-the-gallery"></a>Ajouter PostBeyond à partir de la galerie
Pour configurer l’intégration de PostBeyond à Azure AD, vous devez ajouter PostBeyond, disponible dans la galerie, à votre liste d’applications SaaS gérées.

**Pour ajouter PostBeyond à partir de la galerie, procédez comme suit :**

1. Dans le volet de navigation gauche du **portail Azure Classic**, cliquez sur **Active Directory**. 
   
    ![Active Directory][1]
2. Dans la liste **Annuaire** , sélectionnez l'annuaire pour lequel vous voulez activer l'intégration d'annuaire.
3. Pour ouvrir la vue des applications, dans la vue d'annuaire, cliquez sur **Applications** dans le menu du haut.
   
    ![Applications][2]
4. Cliquez sur **Ajouter** en bas de la page.
   
    ![Applications][3]
5. Dans la boîte de dialogue **Que voulez-vous faire ?**, cliquez sur **Ajouter une application à partir de la galerie**.
   
    ![Applications][4]
6. Dans la zone de recherche, tapez **PostBeyond**.
   
    ![Création d’un utilisateur de test Azure AD](./media/active-directory-saas-postbeyond-tutorial/tutorial_postbeyond_01.png)
7. Dans le volet de résultats, sélectionnez **PostBeyond**, puis cliquez sur **Terminer** pour ajouter l’application.
   
    ![Création d’un utilisateur de test Azure AD](./media/active-directory-saas-postbeyond-tutorial/tutorial_postbeyond_02.png)

## <a name="configure-and-test-azure-ad-sso"></a>Configurer et tester l’authentification unique (SSO) Azure AD
Dans cette section, vous allez configurer et tester l’authentification unique Azure AD avec PostBeyond, avec un utilisateur de test appelé « Britta Simon ».

Pour que l’authentification unique fonctionne, Azure AD doit savoir qui est l’utilisateur PostBeyond équivalent dans Azure AD. En d’autres termes, une relation entre l’utilisateur Azure AD et l’utilisateur PostBeyond associé doit être établie.

Pour cela, affectez la valeur de **nom d’utilisateur** dans Azure AD comme valeur de **nom d’utilisateur** dans PostBeyond.

Pour configurer et tester l’authentification unique Azure AD avec PostBeyond, vous devez suivre les indications des sections suivantes :

1. **[Configuration de l’authentification unique Azure AD](#configuring-azure-ad-single-single-sign-on)** pour permettre à vos utilisateurs d’utiliser cette fonctionnalité.
2. **[Création d’un utilisateur de test Azure AD](#creating-an-azure-ad-test-user)** pour tester l’authentification unique Azure AD avec Britta Simon.
3. **[Création d’un utilisateur de test PostBeyond](#creating-a-PostBeyond-test-user)** pour avoir un équivalent de Britta Simon dans PostBeyond lié à la représentation Azure AD associée.
4. **[Affectation de l’utilisateur de test Azure AD](#assigning-the-azure-ad-test-user)** pour permettre à Britta Simon d’utiliser l’authentification unique Azure AD.
5. **[Test de l’authentification unique](#testing-single-sign-on)** pour vérifier si la configuration fonctionne.

### <a name="configure-azure-ad-sso"></a>Configurer l’authentification unique (SSO) Azure AD
L’objectif de cette section est d’activer l’authentification unique (SSO) Azure AD dans le portail Azure Classic et de la configurer dans votre application PostBeyond.

**Pour configurer l’authentification unique (SSO) Azure AD avec PostBeyond, procédez comme suit :**

1. Dans le menu situé en haut, cliquez sur **Démarrage rapide**.
   
    ![Configurer l’authentification unique][6]
2. Dans le portail Azure Classic, sur la page d’intégration d’applications **PostBeyond**, cliquez sur **Configurer l’authentification unique** pour ouvrir la boîte de dialogue **Configurer l’authentification unique**.
   
    ![Configurer l’authentification unique][7] 
3. Sur la page **Comment voulez-vous que les utilisateurs se connectent à PostBeyond**, sélectionnez **Authentification unique Azure AD**, puis cliquez sur **Suivant**.
   
    ![Configurer l’authentification unique](./media/active-directory-saas-postbeyond-tutorial/tutorial_postbeyond_06.png)
4. Sur la page **Configurer les paramètres d’application** , procédez comme suit : 
   
    ![Configurer l’authentification unique](./media/active-directory-saas-postbeyond-tutorial/tutorial_postbeyond_07.png)
 1. Dans la zone de texte URL de connexion, tapez une URL au format suivant : `https://app.postbeyond.com`. 
 2. Cliquez sur **Suivant**.

5. Dans la page **Configurer l’authentification unique sur PostBeyond**, cliquez sur **Télécharger le certificat**, puis enregistrez le fichier sur votre ordinateur. Copiez également l’URL de l’émetteur, l’URL du service d’authentification unique et l’URL du service de déconnexion unique. Vous devez partager ces informations avec le support PostBeyond pour configurer l’authentification unique.
   
    ![Configurer l’authentification unique](./media/active-directory-saas-postbeyond-tutorial/tutorial_postbeyond_08.png)
6. Pour que l’authentification unique soit configurée pour votre application, contactez l’équipe du support technique PostBeyond à l’adresse <mailto:sso@postbeyond.com>. Ils vous aideront à configurer l’authentification unique à l’aide du canal approprié et vous fourniront les éléments suivants : 
   
   * Certificat téléchargé
   * **L’URL de l’émetteur**
   * **L’URL d’authentification unique SAML**
   * l’ **URL du service d’authentification unique**
7. Dans le portail Classic, sélectionnez la confirmation de la configuration de l’authentification unique, puis cliquez sur **Suivant**.
   
    ![Authentification unique Azure AD][10]
8. Sur la page **Confirmation de l’authentification unique**, cliquez sur **Terminer**.  
   
    ![Authentification unique Azure AD][11]

### <a name="create-an-azure-ad-test-user"></a>Créer un utilisateur de test Azure AD
Dans cette section, vous allez créer un utilisateur de test appelé Britta Simon dans le portail Classic.

![Créer un utilisateur Azure AD][20]

**Pour créer un utilisateur de test dans Azure AD, procédez comme suit :**

1. Dans le volet de navigation gauche du **portail Azure Classic**, cliquez sur **Active Directory**.
   
    ![Création d’un utilisateur de test Azure AD](./media/active-directory-saas-postbeyond-tutorial/create_aaduser_09.png) 
2. Dans la liste **Annuaire** , sélectionnez l'annuaire pour lequel vous voulez activer l'intégration d'annuaire.
3. Pour afficher la liste des utilisateurs, dans le menu situé en haut, cliquez sur **Utilisateurs**.
   
    ![Création d’un utilisateur de test Azure AD](./media/active-directory-saas-postbeyond-tutorial/create_aaduser_03.png) 
4. Pour ouvrir la boîte de dialogue **Ajouter un utilisateur**, cliquez sur l’option **Ajouter un utilisateur** figurant dans la barre d’outils du bas.
   
    ![Création d’un utilisateur de test Azure AD](./media/active-directory-saas-postbeyond-tutorial/create_aaduser_04.png) 
5. Sur la page de boîte de dialogue **Dites-nous en plus sur cet utilisateur** , procédez comme suit :
   
    ![Création d’un utilisateur de test Azure AD](./media/active-directory-saas-postbeyond-tutorial/create_aaduser_05.png) 
 1. Dans Type d’utilisateur, sélectionnez Nouvel utilisateur dans votre organisation. 
 2. Dans la zone de texte **Nom d’utilisateur**, entrez **BrittaSimon**. 
 3. Cliquez sur **Suivant**.
6. Sur la page de boîte de dialogue **Profil utilisateur** , procédez comme suit :
   
   ![Création d’un utilisateur de test Azure AD](./media/active-directory-saas-postbeyond-tutorial/create_aaduser_06.png)  
 1. Dans la zone de texte **First Name**, tapez **Britta**.   
 2. Dans la zone de texte **Last Name**, tapez **Simon**. 
 3. Dans la zone de texte **Nom d’affichage**, entrez **Britta Simon**. 
 4. Dans la liste **Rôle**, sélectionnez **Utilisateur**. 
 5. Cliquez sur **Next**.
7. Sur la page de boîte de dialogue **Obtenir un mot de passe temporaire**, cliquez sur **créer**.
   
    ![Création d’un utilisateur de test Azure AD](./media/active-directory-saas-postbeyond-tutorial/create_aaduser_07.png) 
8. Sur la page de boîte de dialogue **Obtenir un mot de passe temporaire** , procédez comme suit :
   
    ![Création d’un utilisateur de test Azure AD](./media/active-directory-saas-postbeyond-tutorial/create_aaduser_08.png)  
 1. Notez la valeur du **Nouveau mot de passe**. 
 2. Cliquez sur **Terminé**.   

### <a name="create-a-postbeyond-test-user"></a>Créer un utilisateur de test PostBeyond
Dans cette section, vous allez créer un utilisateur appelé Britta Simon dans PostBeyond. Si vous ne savez pas comment ajouter Britta Simon dans PostBeyond, rapprochez-vous de l’équipe de support PostBeyond pour ajouter l’utilisateur de test et activer l’authentification unique. Contactez-la à <mailto:sso@postbeyond.com>.

### <a name="assign-the-azure-ad-test-user"></a>Affecter l’utilisateur de test Azure AD
Dans cette section, vous allez autoriser Britta Simon à utiliser l’authentification unique (SSO) Azure en lui accordant l’accès à PostBeyond.

![Affecter des utilisateurs][200] 

**Pour affecter Britta Simon à PostBeyond, procédez comme suit :**

1. Pour ouvrir l’affichage des applications dans le portail Classic, dans l’affichage de l’annuaire, cliquez sur l’option **Applications** figurant dans le menu du haut.
   
    ![Affecter des utilisateurs][201] 
2. Dans la liste des applications, sélectionnez **PostBeyond**.
   
    ![Configurer l’authentification unique](./media/active-directory-saas-postbeyond-tutorial/tutorial_postbeyond_09.png) 
3. Dans le menu situé en haut, cliquez sur **Utilisateurs**.
   
    ![Affecter des utilisateurs][203] 
4. Dans la liste Tous les utilisateurs, sélectionnez **Britta Simon**.
5. Dans la barre d’outils située en bas, cliquez sur **Attribuer**.
   
    ![Affecter des utilisateurs][205]

### <a name="test-single-sign-on"></a>Tester l’authentification unique
L’objectif de cette section est de tester la configuration de l’authentification unique (SSO) Azure AD à l’aide du volet d’accès.

Lorsque vous cliquez sur la vignette PostBeyond dans le volet d’accès, vous accédez normalement à la page de connexion à PostBeyond. Cliquez sur **Sign in with Office 365**(Se connecter avec Office 365), entrez vos informations d’identification Azure AD. Vous devriez maintenant être connecté à PostBeyond.

## <a name="additional-resources"></a>Ressources supplémentaires
* [Liste de didacticiels sur l’intégration d’applications SaaS avec Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Qu’est-ce que l’accès aux applications et l’authentification unique avec Azure Active Directory ?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-postbeyond-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-postbeyond-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-postbeyond-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-postbeyond-tutorial/tutorial_general_04.png


[5]: ./media/active-directory-saas-postbeyond-tutorial/tutorial_general_05.png
[6]: ./media/active-directory-saas-postbeyond-tutorial/tutorial_general_06.png
[7]:  ./media/active-directory-saas-postbeyond-tutorial/tutorial_general_050.png
[10]: ./media/active-directory-saas-postbeyond-tutorial/tutorial_general_060.png
[11]: ./media/active-directory-saas-postbeyond-tutorial/tutorial_general_070.png
[20]: ./media/active-directory-saas-postbeyond-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-postbeyond-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-postbeyond-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-postbeyond-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-postbeyond-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-postbeyond-tutorial/tutorial_general_205.png

