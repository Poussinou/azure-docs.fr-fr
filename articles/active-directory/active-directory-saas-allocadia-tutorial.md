---
title: "Didacticiel : Intégration d’Azure Active Directory à Allocadia | Microsoft Docs"
description: "Découvrez comment configurer l’authentification unique entre Azure Active Directory et Allocadia."
services: active-directory
documentationcenter: 
author: jeevansd
manager: femila
editor: 
ms.assetid: c415fc55-6dc1-49f2-a8a2-2fc6e3790d65
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 3/10/2017
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 07635b0eb4650f0c30898ea1600697dacb33477c
ms.openlocfilehash: 5ec20dee8b1c173af8c28f4a919568aba5f61192
ms.lasthandoff: 03/28/2017


---
# <a name="tutorial-azure-active-directory-integration-with-allocadia"></a>Didacticiel : Intégration d’Azure Active Directory à Allocadia
Dans ce didacticiel, vous allez apprendre à intégrer Allocadia à Azure Active Directory (Azure AD).

L’intégration d’Allocadia dans Azure AD vous offre les avantages suivants :

* Dans Azure AD, vous pouvez contrôler qui a accès à Allocadia
* Vous pouvez autoriser vos utilisateurs à se connecter automatiquement à Allocadia (via l’authentification unique) avec leur compte Azure AD
* Vous pouvez gérer vos comptes à un emplacement central : le portail Azure Classic.

Pour en savoir plus sur l’intégration des applications SaaS avec Azure AD, consultez [Qu’est-ce que l’accès aux applications et l’authentification unique avec Azure Active Directory ?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Composants requis
Pour configurer l'intégration d'Azure AD avec Allocadia, vous avez besoin des éléments suivants :

* Un abonnement Azure AD
* Un abonnement Allocadia pour lequel l’authentification unique est activée

>[!NOTE]
>Pour tester les étapes de ce didacticiel, nous déconseillons l’utilisation d’un environnement de production. 
> 

Vous devez en outre suivre les recommandations ci-dessous :

* Vous ne devez pas utiliser votre environnement de production, sauf si cela est nécessaire.
* Si vous n’avez pas d’environnement d’essai Azure AD, vous pouvez [obtenir un essai d’un mois](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Description du scénario
Dans ce didacticiel, vous testez l’authentification unique Azure AD dans un environnement de test. 

Le scénario décrit dans ce didacticiel se compose des deux sections principales suivantes :

1. Ajout d’Allocadia à partir de la galerie
2. Configuration et test de l’authentification unique Azure AD

## <a name="add-allocadia-from-the-gallery"></a>Ajouter Allocadia à partir de la galerie
Pour configurer l’intégration d’Allocadia à Azure AD, vous devez ajouter Allocadia à partir de la galerie à votre liste d’applications SaaS gérées.

**Pour ajouter Allocadia à partir de la galerie, procédez comme suit :**

1. Dans le volet de navigation gauche du **portail Azure Classic**, cliquez sur **Active Directory**. 
   
    ![Active Directory][1]

2. Dans la liste **Annuaire** , sélectionnez l'annuaire pour lequel vous voulez activer l'intégration d'annuaire.

3. Pour ouvrir la vue des applications, dans la vue d'annuaire, cliquez sur **Applications** dans le menu du haut.
   
    ![Applications][2]

4. Cliquez sur **Ajouter** en bas de la page.
   
    ![Applications][3]

5. Dans la boîte de dialogue **Que voulez-vous faire ?**, cliquez sur **Ajouter une application à partir de la galerie**.
   
    ![Applications][4]

6. Dans la zone de recherche, tapez **Allocadia**.
   
    ![Création d’un utilisateur de test Azure AD](./media/active-directory-saas-allocadia-tutorial/tutorial_allocadia_01.png)

7. Dans le volet de résultats, sélectionnez **Allocadia**, puis cliquez sur **Terminer** pour ajouter l’application.
   
    ![Création d’un utilisateur de test Azure AD](./media/active-directory-saas-allocadia-tutorial/tutorial_allocadia_06.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurer et tester l’authentification unique Azure AD
Dans cette section, vous allez configurer et tester l’authentification unique (SSO) Azure AD avec Allocadia pour un utilisateur de test nommé « Britta Simon ».

Pour que l’authentification unique (SSO) fonctionne, Azure AD doit savoir qui est l’utilisateur Allocadia équivalent dans Azure AD. En d’autres termes, une relation entre un utilisateur Azure AD et un utilisateur Allocadia associé doit être établie.

Pour cela, affectez la valeur du **nom d’utilisateur** dans Azure AD comme valeur du **nom d’utilisateur** dans Allocadia.

Pour configurer et tester l’authentification unique Azure AD avec Allocadia, vous devez suivre les indications des sections suivantes :

1. **[Configuration de l’authentification unique Azure AD](#configuring-azure-ad-single-single-sign-on)** pour permettre à vos utilisateurs d’utiliser cette fonctionnalité.
2. **[Création d’un utilisateur de test Azure AD](#creating-an-azure-ad-test-user)** pour tester l’authentification unique Azure AD avec Britta Simon.
3. **[Création d’un utilisateur test Allocadia](#creating-an-allocadia-test-user)** pour avoir un équivalent de Britta Simon dans Allocadia lié à la représentation Azure AD associée.
4. **[Affectation de l’utilisateur de test Azure AD](#assigning-the-azure-ad-test-user)** pour permettre à Britta Simon d'utiliser l'authentification unique Azure AD.
5. **[Test de l’authentification unique](#testing-single-sign-on)** pour vérifier si la configuration fonctionne.

### <a name="configure-azure-ad-single-sign-on"></a>Configurer l’authentification unique Azure AD
Dans cette section, vous allez activer l’authentification unique Azure AD dans le portail Azure Classic et configurer l’authentification unique dans votre application Allocadia.

L’application Allocadia attend les assertions SAML dans un format spécifique. Configurez les revendications suivantes pour cette application. Vous pouvez gérer les valeurs de ces attributs à partir de l’onglet **Attribut** de l’application. La capture d’écran suivante montre un exemple : 

![Configurer l’authentification unique](./media/active-directory-saas-allocadia-tutorial/tutorial_allocadia_07.png) 

**Pour configurer l’authentification unique Azure AD avec Hightail, procédez comme suit :**

1. Dans le portail Azure Classic, dans la page d’intégration d’application **Allocadia**, dans le menu situé en haut, cliquez sur **Attributs**.
   
    ![Configurer l’authentification unique](./media/active-directory-saas-allocadia-tutorial/tutorial_general_80.png) 

2. Dans la boîte de dialogue **Attributs du jeton SAML** , pour chaque ligne indiquée dans le tableau ci-dessous, procédez comme suit :
   
    | Nom de l'attribut | Valeur de l’attribut |
    | --- | --- |
    | firstname |user.givenname |
    | lastname |user.surname |
    | email |user.mail |

  1. Cliquez sur **ajouter un attribut utilisateur** pour ouvrir la boîte de dialogue **Ajouter un attribut utilisateur**.

    ![Configurer l’authentification unique](./media/active-directory-saas-allocadia-tutorial/tutorial_general_81.png) 
  2. Dans la zone de texte **Nom d’attribut** , tapez le nom d’attribut indiqué pour cette ligne.
  3. Dans la liste **Valeur d’attribut** , sélectionnez la valeur d’attribut indiquée pour cette ligne.
  4. Cliquez sur **Terminé**.    
 
3. Dans le menu situé en haut, cliquez sur **Démarrage rapide**.
   
    ![Configurer l’authentification unique](./media/active-directory-saas-allocadia-tutorial/tutorial_general_83.png)  

4. Dans la page **Comment voulez-vous que les utilisateurs se connectent à Allocadia ?**, sélectionnez **Authentification unique Azure AD**, puis cliquez sur **Suivant**.
   
    ![Configurer l’authentification unique](./media/active-directory-saas-allocadia-tutorial/tutorial_allocadia_03.png) 

5. Sur la page de boîte de dialogue **Configurer les paramètres de l’application** , procédez comme suit :
   
    ![Configurer l’authentification unique](./media/active-directory-saas-allocadia-tutorial/tutorial_allocadia_04.png)  
  1. Dans la zone IDENTIFER, tapez l’URL au format suivant : pour l’environnement de test, utilisez l’URL **« https://na2standby.allocadia.com »** et, pour l'environnement de production, **« https://na2.allocadia.com »**.
  2. Dans la zone URL de réponse, tapez l’URL au format suivant : pour l’environnement de test, utilisez l’URL **« https://na2standby.allocadia.com/allocadia/saml/SSO »** et, pour l'environnement de production, **« https://na2.allocadia.com/allocadia/saml/SSO »**

6. Dans la page **Configurer l’authentification unique sur Allocadia** , procédez comme suit :
   
    ![Configurer l’authentification unique](./media/active-directory-saas-allocadia-tutorial/tutorial_allocadia_05.png) 
  1. Cliquez sur **Télécharger les métadonnées**, puis enregistrez le fichier sur votre ordinateur.
  2. Cliquez sur **Suivant**.

7. Afin de configurer l’authentification unique pour votre application, contactez l'équipe de [support Allocadia](mailTo:support@allocadia.com) , qui vous aidera à configurer l’authentification unique. Notez que vous devez envoyer un courrier électronique et joindre le fichier de métadonnées téléchargé pour configurer l’authentification unique côté Allocadia.
   
   >[!NOTE]
   >Assurez-vous que l'équipe Allocadia a défini la valeur Identifier dans l’environnement de test sur **« https://na2standby.allocadia.com »** et, pour l'environnement de production, sur **« https://na2.allocadia.com »**
   >  

8. Dans le portail Classic, sélectionnez la confirmation de la configuration de l’authentification unique, puis cliquez sur **Suivant**.
   
    ![Authentification unique Azure AD][10]

9. Sur la page **Confirmation de l’authentification unique**, cliquez sur **Terminer**.  
   
    ![Authentification unique Azure AD][11]

### <a name="create-an-azure-ad-test-user"></a>Créer un utilisateur de test Azure AD
Dans cette section, vous allez créer un utilisateur de test appelé Britta Simon dans le portail Classic.

* Dans la liste Utilisateurs, sélectionnez **Britta Simon**.

![Créer un utilisateur Azure AD][20]

**Pour créer un utilisateur de test dans Azure AD, procédez comme suit :**

1. Dans le volet de navigation gauche du **portail Azure Classic**, cliquez sur **Active Directory**.
   
    ![Création d’un utilisateur de test Azure AD](./media/active-directory-saas-allocadia-tutorial/create_aaduser_09.png) 

2. Dans la liste **Annuaire** , sélectionnez l'annuaire pour lequel vous voulez activer l'intégration d'annuaire.

3. Pour afficher la liste des utilisateurs, dans le menu situé en haut, cliquez sur **Utilisateurs**.
   
    ![Création d’un utilisateur de test Azure AD](./media/active-directory-saas-allocadia-tutorial/create_aaduser_03.png) 

4. Pour ouvrir la boîte de dialogue **Ajouter un utilisateur**, cliquez sur l’option **Ajouter un utilisateur** figurant dans la barre d’outils du bas.
   
    ![Création d’un utilisateur de test Azure AD](./media/active-directory-saas-allocadia-tutorial/create_aaduser_04.png) 

5. Sur la page de boîte de dialogue **Dites-nous en plus sur cet utilisateur** , procédez comme suit :
   
    ![Création d’un utilisateur de test Azure AD](./media/active-directory-saas-allocadia-tutorial/create_aaduser_05.png) 
 1. Dans Type d’utilisateur, sélectionnez Nouvel utilisateur dans votre organisation.   
 2. Dans la zone de texte **Nom d’utilisateur**, entrez **BrittaSimon**. 
 3. Cliquez sur **Suivant**.

6. Sur la page de boîte de dialogue **Profil utilisateur** , procédez comme suit :
   
    ![Création d’un utilisateur de test Azure AD](./media/active-directory-saas-allocadia-tutorial/create_aaduser_06.png)  
 1. Dans la zone de texte **First Name**, tapez **Britta**.    
 2. Dans la zone de texte **Last Name**, tapez **Simon**. 
 3. Dans la zone de texte **Nom d’affichage**, entrez **Britta Simon**. 
 4. Dans la liste **Rôle**, sélectionnez **Utilisateur**. 
 5. Cliquez sur **Next**.

7. Sur la page de boîte de dialogue **Obtenir un mot de passe temporaire**, cliquez sur **créer**.
   
    ![Création d’un utilisateur de test Azure AD](./media/active-directory-saas-allocadia-tutorial/create_aaduser_07.png) 

8. Sur la page de boîte de dialogue **Obtenir un mot de passe temporaire** , procédez comme suit :
   
    ![Création d’un utilisateur de test Azure AD](./media/active-directory-saas-allocadia-tutorial/create_aaduser_08.png)  
 1. Notez la valeur du **Nouveau mot de passe**.  
 2. Cliquez sur **Terminé**.   

### <a name="create-an-allocadia-test-user"></a>Créer un utilisateur de test Allocadia
Dans cette section, vous allez créer un utilisateur appelé Britta Simon dans Allocadia. L'application Allocadia prend en charge la configuration de l'utilisateur au moment opportun. Si vous avez configuré les revendications comme indiqué plus haut dans la section **[Configuration de l’authentification unique Azure AD](#configuring-azure-ad-single-single-sign-on)** , les utilisateurs de l’application vont être configurés. 

>[!NOTE]
>Si vous avez besoin de créer un utilisateur manuellement ou un groupe d’utilisateurs, vous devez contacter l’équipe de support Allocadia. 
> 

### <a name="assign-the-azure-ad-test-user"></a>Affecter l’utilisateur de test Azure AD
Dans cette section, vous allez autoriser Britta Simon à utiliser l’authentification unique (SSO) Azure en lui accordant l’accès à Allocadia.

![Affecter des utilisateurs][200] 

**Pour affecter Britta Simon à Allocadia, procédez comme suit :**

1. Pour ouvrir l’affichage des applications dans le portail classique, dans l’affichage du répertoire, cliquez sur l’option **Applications** figurant dans le menu supérieur.
   
    ![Affecter des utilisateurs][201] 

2. Dans la liste des applications, sélectionnez **Allocadia**.
   
    ![Configurer l’authentification unique](./media/active-directory-saas-allocadia-tutorial/tutorial_allocadia_50.png) 

3. Dans le menu situé en haut, cliquez sur **Utilisateurs**.
   
    ![Affecter des utilisateurs][203] 

4. Dans la liste Utilisateurs, sélectionnez **Britta Simon**.

5. Dans la barre d’outils située en bas, cliquez sur **Attribuer**.
   
    ![Affecter des utilisateurs][205]

### <a name="test-single-sign-on"></a>Tester l’authentification unique
Dans cette section, vous allez tester la configuration SSO Azure AD à l’aide du volet d’accès.

Lorsque vous cliquez sur la mosaïque Allocadia dans le volet d’accès, vous devez être connecté automatiquement à votre application Allocadia.

## <a name="additional-resources"></a>Ressources supplémentaires
* [Liste de didacticiels sur l’intégration d’applications SaaS avec Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Qu’est-ce que l’accès aux applications et l’authentification unique avec Azure Active Directory ?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-allocadia-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-allocadia-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-allocadia-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-allocadia-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-allocadia-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-allocadia-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-allocadia-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-allocadia-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-allocadia-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-allocadia-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-allocadia-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-allocadia-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-allocadia-tutorial/tutorial_general_205.png

