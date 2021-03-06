---
title: Approvisionnement automatique des utilisateurs pour les applications SaaS dans Azure AD | Microsoft Docs
description: "Cette introduction explique comment utiliser Azure AD pour approvisionner, annuler l’approvisionnement et mettre à jour de façon continue des comptes d’utilisateurs dans diverses applications SaaS tierces."
services: active-directory
documentationcenter: 
author: asmalser-msft
manager: femila
editor: 
ms.assetid: 58c5fa2d-bb33-4fba-8742-4441adf2cb62
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/09/2016
ms.author: asmalser
translationtype: Human Translation
ms.sourcegitcommit: f41fbee742daf2107b57caa528e53537018c88c6
ms.openlocfilehash: 39714145f84f5f9229dbf4bbe136103d4d102bed
ms.lasthandoff: 03/31/2017


---
# <a name="automate-user-provisioning-and-deprovisioning-to-saas-applications-with-azure-active-directory"></a>Automatisation de l’approvisionnement et de la l’annulation de l’approvisionnement des utilisateurs pour les applications SaaS avec Azure Active Directory
## <a name="what-is-automated-user-provisioning-for-saas-apps"></a>Présentation de l’approvisionnement automatique des utilisateurs pour les applications SaaS
Azure Active Directory (Azure AD) vous permet d’automatiser la création, la maintenance et la suppression d’identités utilisateur dans les applications cloud ([SaaS](https://azure.microsoft.com/overview/what-is-saas/)) comme Dropbox, Salesforce, ServiceNow et bien plus encore.

**Voici quelques exemples d’utilisation de cette fonctionnalité :**

* Créez automatiquement des comptes dans les applications SaaS adéquates pour les nouvelles personnes rejoignant votre équipe.
* Désactivez automatiquement les comptes d’applications SaaS des personnes quittant votre équipe.
* Vérifiez que les identités de vos applications SaaS sont tenues à jour selon les modifications apportées à l’annuaire.
* Procédez à l’approvisionnement d’objets non-utilisateur tels que des groupes, pour que des applications SaaS les prennent en charge.

**L’approvisionnement automatique d’utilisateurs inclut également les fonctionnalités suivantes :**

* Possibilité de faire correspondre des identités existantes entre Azure AD et des applications SaaS
* Options de personnalisation pour permettre à Azure AD de s’ajuster aux configurations des applications SaaS actuellement utilisées par votre organisation
* Alertes par courrier électronique pour les erreurs d’approvisionnement
* Création de rapports et journaux d’activité pour faciliter la surveillance et la résolution des problèmes

## <a name="why-use-automated-provisioning"></a>Pourquoi utiliser l’approvisionnement automatique ?
Voici les principales raisons pour utiliser cette fonctionnalité :

* Réduction des coûts, des inefficacités et des erreurs humaines associées au processus d’approvisionnement manuel
* Sécurisation de votre organisation en supprimant instantanément les identités des utilisateurs des applications SaaS lorsqu’ils quittent l’organisation
* Importation facile de nombreux utilisateurs en bloc dans une application SaaS précise
* Exécution pratique de votre solution d’approvisionnement à partir des mêmes stratégies d’accès aux applications que celles définies pour l’authentification unique Azure AD

## <a name="frequently-asked-questions"></a>Forum Aux Questions (FAQ)
**À quelle fréquence Azure AD écrit-il les modifications d’annuaire dans l’application SaaS ?**

Azure AD vérifie les modifications toutes les cinq à dix minutes. Si l’application SaaS renvoie plusieurs erreurs (par exemple, dans le cas d’informations d’identification d’administration incorrectes), Azure AD ralentit progressivement sa fréquence de vérification jusqu’à une fois par jour, jusqu'à ce que les erreurs soient résolues.

**Combien de temps faut-il pour approvisionner mes utilisateurs ?**

Les modifications incrémentielles se produisent presque instantanément, mais si vous essayez d’approvisionner la plupart de votre annuaire, la durée dépend du nombre d’utilisateurs et de groupes que vous avez. L’opération ne prend que cinq à dix minutes pour les petits annuaires, une trentaine de minutes pour les annuaires de taille moyenne et plusieurs heures pour les annuaires volumineux.

**Comment puis-je suivre la progression de la tâche d’approvisionnement actuelle ?**

Vous pouvez consulter le rapport d’approvisionnement du compte sous la section Rapports de votre annuaire. Une autre option consiste à consulter l’onglet Tableau de bord de l’application SaaS que vous approvisionnez, puis à consulter le contenu affiché sous la section « Statut d’intégration » en bas de la page.

**Comment savoir si les utilisateurs ne sont pas approvisionnés correctement ?**

À la fin de l’Assistant Configuration de l’approvisionnement, vous pouvez vous inscrire à l’envoi de notifications par courrier électronique en cas d’erreur d’approvisionnement. Vous pouvez également consulter le rapport d’erreurs d’approvisionnement pour savoir quels utilisateurs n’ont pas été approvisionnés correctement et pourquoi.

**Azure AD peut-il écrire en différé les modifications de l’application SaaS vers l’annuaire ?**

Pour la plupart des applications SaaS, l’approvisionnement s’effectue uniquement via une connexion sortante, ce qui signifie que les utilisateurs sont écrits à partir de l’annuaire vers l’application, et que les modifications de l’application ne peuvent pas être écrites en différé dans l’annuaire. Cependant, pour [Workday](https://msdn.microsoft.com/library/azure/dn762434.aspx), l’approvisionnement s’effectue uniquement via une connexion entrante, ce qui signifie que les utilisateurs sont importés dans l’annuaire à partir de Workday et que les modifications apportées à l’annuaire ne sont pas écrites en différé dans Workday.

**Comment puis-je envoyer des commentaires à l’équipe d’ingénierie ?**

Veuillez nous contacter via le [forum des commentaires sur Azure Active Directory](https://feedback.azure.com/forums/169401-azure-active-directory/).

## <a name="how-does-automated-provisioning-work"></a>Comment fonctionne l’approvisionnement automatique ?
Azure AD approvisionne des utilisateurs pour les applications SaaS en se connectant aux points de terminaison d’approvisionnement fournis par chaque fournisseur de l’application. Ces points de terminaison permettent à Azure AD de créer, de mettre à jour et de supprimer des utilisateurs par programmation. Voici une brève présentation des différentes étapes que suit Azure AD pour automatiser l’approvisionnement.

1. Lorsque vous activez l’approvisionnement d’une application pour la première fois, les actions suivantes sont effectuées :
   * Azure AD tente de faire correspondre tous les utilisateurs existants dans l’application SaaS à leurs identités dans l’annuaire. Lorsqu'un utilisateur est mis en correspondance, il n’est *pas* automatiquement activé pour l’authentification unique. Pour qu’un utilisateur puisse accéder à l’application, il doit être affecté de manière explicite à l’application dans Azure AD, soit directement, soit via une appartenance au groupe.
   * Si vous avez déjà spécifié quels utilisateurs doivent être affectés à l’application et si Azure AD ne parvient pas à trouver des comptes existants pour ces utilisateurs, Azure AD configure de nouveaux comptes pour eux dans l’application.
2. Une fois la synchronisation initiale terminée comme décrit ci-dessus, Azure AD vérifie les modifications suivantes toutes les 10 minutes :
   * Si de nouveaux utilisateurs sont affectés à l’application (directement ou via l’appartenance à un groupe), ils sont approvisionnés par un nouveau compte dans l’application SaaS.
   * Si l’accès d’un utilisateur a été supprimé, son compte dans l’application SaaS est marqué comme désactivé (les utilisateurs ne sont jamais complètement supprimés, ce qui empêche toute perte de données en cas d’erreur de configuration).
   * Si un utilisateur a été récemment affecté à une application SaaS dans laquelle il a déjà un compte, ce compte est marqué comme activé et certaines propriétés de l’utilisateur sont mises à jour si elles sont obsolètes par rapport à l’annuaire.
   * Si les informations d’un utilisateur (comme son numéro de téléphone, le lieu où se trouve son bureau, etc.) ont été modifiées dans l’annuaire, elles sont également mises à jour dans l’application SaaS.

Pour plus d’informations sur la façon dont les attributs sont mappés entre Azure AD et votre application SaaS, consultez l’article [Personnalisation des mappages d’attributs](active-directory-saas-customizing-attribute-mappings.md).

## <a name="list-of-apps-that-support-automated-user-provisioning"></a>Liste des applications prenant en charge l’approvisionnement automatique des utilisateurs
Toutes les applications « à la une » de la galerie d’applications Azure AD prennent en charge l’approvisionnement automatique des utilisateurs. [La liste des applications à la une peut être vue ici.](https://azuremarketplace.microsoft.com/marketplace/apps/category/azure-active-directory-apps?page=1&subcategories=featured)

Pour qu’une application puisse prendre en charge l’approvisionnement automatique des utilisateurs, elle doit d’abord fournir les points de terminaison requis pour permettre aux programmes externes d’automatiser la création, la maintenance et la suppression des utilisateurs. Par conséquent, toutes les applications SaaS ne sont pas compatibles avec cette fonctionnalité. L’équipe d’ingénierie d’Azure AD peut développer un connecteur d’approvisionnement pour les applications qui le sont. Ce travail s’effectue en fonction des besoins des clients actuels et à venir.

Pour contacter l’équipe d’ingénierie d’Azure AD afin de demander une prise en charge de l’approvisionnement pour des applications supplémentaires, veuillez écrire un message sur le [forum des commentaires sur Azure Active Directory](https://feedback.azure.com/forums/374982-azure-active-directory-application-requests/category/172035-user-provisioning).

## <a name="related-articles"></a>Articles connexes
* [Index d’articles pour la gestion des applications dans Azure Active Directory](active-directory-apps-index.md)
* [Personnalisation des mappages d’attributs pour l’approvisionnement des utilisateurs](active-directory-saas-customizing-attribute-mappings.md)
* [Écriture d’expressions pour les mappages d’attributs](active-directory-saas-writing-expressions-for-attribute-mappings.md)
* [Filtres d’étendue pour l’approvisionnement des utilisateurs](active-directory-saas-scoping-filters.md)
* [Utilisation de SCIM pour activer la configuration automatique des utilisateurs et des groupes d’Azure Active Directory sur des applications](active-directory-scim-provisioning.md)
* [Notifications d’approvisionnement de comptes](active-directory-saas-account-provisioning-notifications.md)
* [Liste des didacticiels sur l’intégration des applications SaaS](active-directory-saas-tutorial-list.md)


