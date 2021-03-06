---
title: "Gérer le schéma Azure SQL Database dans une application mutualisée | Microsoft Docs"
description: "Gérer un schéma pour plusieurs locataires dans une application mutualisée qui utilise Azure SQL Database"
keywords: "didacticiel sur les bases de données SQL"
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: scale out apps
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.author: billgib; sstein
ms.translationtype: Human Translation
ms.sourcegitcommit: a30a90682948b657fb31dd14101172282988cbf0
ms.openlocfilehash: cbe2b6bbc8e193bdbbf08572a8488239c633548d
ms.contentlocale: fr-fr
ms.lasthandoff: 05/25/2017


---
# <a name="manage-schema-for-multiple-tenants-in-the-wingtip-saas-application"></a>Gérer un schéma pour plusieurs clients dans l’application SaaS Wingtip

Le [premier didacticiel SaaS Wingtip](sql-database-saas-tutorial.md) montre comment l’application peut approvisionner une base de données client et l’enregistrer dans le catalogue. Comme n’importe quelle application, l’application SaaS Wingtip évolue au fil du temps et nécessite parfois l’apport de modifications à la base de données. Ces modifications peuvent inclure un schéma nouveau ou modifié, des données de référence nouvelles ou modifiées et des tâches de maintenance de routine de la base de données pour garantir des performances optimales de l’application. Avec une application SaaS, ces modifications doivent être déployées de façon coordonnée dans un parc potentiellement immense de bases de données de locataire. Les modifications doivent également être incorporées dans le processus d’approvisionnement pour les bases de données de locataire ultérieures.

Ce didacticiel explore deux scénarios : le déploiement de mises à jour des données de référence pour tous les locataires et la reconstruction d’un index sur la table contenant les données de référence. La fonctionnalité de [travaux élastiques](sql-database-elastic-jobs-overview.md) est utilisée pour exécuter ces opérations sur l’ensemble des clients et sur la base de données de client *principale* qui sert de modèle pour les nouvelles bases de données.

Ce didacticiel vous montre comment effectuer les opérations suivantes :

> [!div class="checklist"]

> * Créer un compte de travail élastique pour interroger plusieurs locataires
> * Mettre à jour les données dans toutes les bases de données de locataire
> * Créer un index sur une table dans toutes les bases de données de locataire


Pour suivre ce didacticiel, vérifiez que les conditions préalables ci-dessous sont bien satisfaites :

* L’application SaaS Wingtip est déployée. Pour procéder à un déploiement en moins de cinq minutes, consultez la page [Déployer et explorer une application SaaS Wingtip](sql-database-saas-tutorial.md)
* Azure PowerShell est installé. Pour plus d’informations, consultez [Bien démarrer avec Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps).
* La dernière version de SQL Server Management Studio (SSMS) est installée. [Télécharger et installer SSMS](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms)

*Ce didacticiel utilise des fonctionnalités du service SQL Database en préversion limitée (travaux de base de données élastiques). Si vous souhaitez réaliser ce didacticiel, envoyez votre ID d’abonnement à SaaSFeedback@microsoft.com avec l’objet Préversion des travaux élastiques. Une fois que vous avez reçu la confirmation que votre abonnement a été activé, [téléchargez et installez les dernières applets de commande pour les travaux en version préliminaire](https://github.com/jaredmoo/azure-powershell/releases). Comme il s’agit d’une préversion limitée, vous devez contacter SaaSFeedback@microsoft.com pour toute question ou demande de support associée.*


## <a name="introduction-to-saas-schema-management-patterns"></a>Présentation des modèles de gestion de schéma SaaS

Le modèle SaaS de base de données à un seul locataire bénéficie à de nombreux égards de l’isolation des données qui en résulte, mais il s’accompagne également de la complexité associée à la maintenance et à la gestion d’un grand nombre de bases de données. Les [travaux élastiques](sql-database-elastic-jobs-overview.md) facilitent l’administration et la gestion de la couche de données SQL. Les travaux vous permettent d’exécuter de manière sécurisée et fiable des tâches (scripts T-SQL) indépendantes des interactions ou des entrées des utilisateurs sur un groupe de bases de données. Cette méthode peut être utilisée pour déployer les modifications de schéma et de données de référence commune sur tous les locataires d’une application. Les travaux élastiques permettent également de maintenir à jour une copie *principale* de la base de données utilisée pour créer de nouveaux locataires afin de s’assurer qu’elle contient en permanence le schéma et les données de référence les plus récents.

![Écran](media/sql-database-saas-tutorial-schema-management/schema-management.png)


## <a name="elastic-jobs-limited-preview"></a>Préversion limitée des travaux élastiques

Il existe une nouvelle version des travaux élastiques qui est désormais une fonctionnalité intégrée d’Azure SQL Database (qui ne nécessite pas de services ou de composants supplémentaires). Cette nouvelle version des travaux élastiques est pour le moment en préversion limitée. Cette préversion limitée prend actuellement en charge PowerShell pour créer des comptes de travail et T-SQL pour créer et gérer des travaux.

> [!NOTE]
> *Ce didacticiel utilise des fonctionnalités du service SQL Database en préversion limitée (travaux de base de données élastiques). Si vous souhaitez réaliser ce didacticiel, envoyez votre ID d’abonnement à SaaSFeedback@microsoft.com avec l’objet Préversion des travaux élastiques. Une fois que vous avez reçu la confirmation que votre abonnement a été activé, [téléchargez et installez les dernières applets de commande pour les travaux en version préliminaire](https://github.com/jaredmoo/azure-powershell/releases). Comme il s’agit d’une préversion limitée, vous devez contacter SaaSFeedback@microsoft.com pour toute question ou demande de support associée.*

## <a name="get-the-wingtip-application-scripts"></a>Obtenir les scripts d’application Wingtip

Les scripts et le code source de l’application SaaS Wingtip sont disponibles dans le référentiel GitHub [WingtipSaaS](https://github.com/Microsoft/WingtipSaaS). [Procédure de téléchargement des scripts SaaS Wingtip](sql-database-wtp-overview.md#download-the-wingtip-saas-scripts).

## <a name="create-a-job-account-database-and-new-job-account"></a>Créer une base de données de compte de travail et un compte de travail

Ce didacticiel nécessite l’utilisation de PowerShell pour créer la base de données de compte de travail et le compte de travail. Comme MSDB et SQL Agent, les travaux élastiques s’appuient sur une base de données SQL Azure pour stocker les définitions, l’état et l’historique des travaux. Une fois le compte de travail créé, vous pouvez immédiatement créer et surveiller des travaux.

1. Ouvrez …\\Learning Modules\\Schema Management\\*Demo-SchemaManagement.ps1* dans **PowerShell ISE**.
1. Appuyez sur **F5** pour exécuter le script.

Le script *Demo-SchemaManagement.ps1* appelle le script *Deploy-SchemaManagement.ps1* pour créer une base de données *S2* nommée **jobaccount** sur le serveur de catalogue. Il crée ensuite le compte de travail en transmettant la base de données jobaccount en tant que paramètre à l’appel de création du compte de travail.

## <a name="create-a-job-to-deploy-new-reference-data-to-all-tenants"></a>Créer un travail pour déployer les nouvelles données de référence sur tous les locataires

Chaque base de données de locataire inclut un ensemble de types de lieux qui définissent le type d’événements qui se déroulent dans chacun de ces lieux. Dans cet exercice, vous allez déployer une mise à jour dans toutes les bases de données de locataire afin d’ajouter deux types de lieux supplémentaires : *Motorcycle Racing* (Courses de moto) *Swimming Club* (Club de natation). Ces types de lieux correspondent à l’image d’arrière-plan visible dans l’application que s’affiche dans l’application Events.

Cliquez sur le menu déroulant Venue Type (Type de lieu) et vérifiez que 10 options seulement sont disponibles, et en particulier que la liste ne contient pas les options « Motorcycle Racing » et « Swimming Club ».

Maintenant, nous allons créer un travail pour mettre à jour la table *VenueTypes* dans toutes les bases de données de locataire et ajouter les nouveaux types de lieux.

Pour créer un travail, nous utilisons un ensemble de procédures stockées système dédiées aux travaux créées dans la base de données jobaccount lors de la création du compte de travail.

1. Ouvrez SSMS et connectez-vous au serveur de catalogue catalog-\<user\>.database.windows.net.
1. Connectez-vous également au serveur de locataire tenants1-\<user\>.database.windows.net.
1. Accédez à la base de données *contosoconcerthall* sur le serveur *tenants1* et interrogez la table *VenueTypes* pour vérifier que *Motorcycle Racing* et *Swimming Club* **ne figurent pas** dans la liste des résultats.
1. Ouvrez le fichier ...\\Learning Modules\\Schema Management\\DeployReferenceData.sql.
1. Remplacez les trois occurrences de \<user\> dans le script par le nom d’utilisateur utilisé lors du déploiement de l’application Wingtip.
1. Assurez-vous que vous êtes connecté à la base de données jobaccount, puis appuyez sur **F5** pour exécuter le script.

* **sp\_add\_target\_group** crée le nom de groupe cible DemoServerGroup. Nous devons à présent ajouter des membres cibles.
* **sp\_add\_target\_group\_member** ajoute un type de membre cible *server*, qui estime que toutes les bases de données de ce serveur (notez qu’il s’agit du serveur customer1-&lt;User&gt; contenant les bases de données de locataire) au moment de l’exécution du travail doivent être incluses dans le travail, un type de membre *database*, à savoir la base de données principale baseTenantDB, hébergée sur le serveur catalog-&lt;User&gt;, et enfin un autre type de membre de groupe cible *database* afin d’inclure la base de données adhocanalytics utilisée dans un prochain didacticiel.
* **sp\_add\_job** crée une tâche appelée « Reference Data Deployment » (Déploiement des données de référence).
* **sp\_add\_jobstep** crée l’étape du travail contenant le texte de la commande T-SQL pour mettre à jour la table de référence, VenueTypes.
* Les autres vues du script indiquent l’existence des objets et contrôlent l’exécution du travail. Examinez la valeur de l’état dans la colonne **lifecycle**. Le travail a été accompli avec succès sur toutes les bases de données de locataire et les deux autres bases de données contenant la table de référence.

1. Dans SSMS, accédez à la base de données *contosoconcerthall* sur le serveur *tenants1* et interrogez la table *VenueTypes* pour vérifier que *Motorcycle Racing* et *Swimming Club* **figurent** à présent dans la liste des résultats.


## <a name="create-a-job-to-manage-the-reference-table-index"></a>Créer une tâche pour gérer l’index de la table de référence

De façon similaire à l’exercice précédent, cet exercice crée un travail pour reconstruire l’index sur la clé primaire de la table de référence, une opération de gestion de base de données classique qu’un administrateur peut être amené à exécuter après le chargement d’un grand volume de données dans une table.

Créez un travail en utilisant les mêmes procédures stockées « système » dédiées aux travaux.

1. Ouvrez SSMS et connectez-vous au serveur catalog-&lt;User&gt;.database.windows.net.
1. Ouvrez le fichier ...\\Learning Modules\\Schema Management\\OnlineReindex.sql.
1. Si vous n’êtes pas déjà connecté, cliquez avec le bouton droit, sélectionnez Connexion et connectez-vous au serveur de catalogue &lt;User&gt;.database.windows.net
1. Assurez-vous que vous êtes connecté à la base de données jobaccount, puis appuyez sur F5 pour exécuter le script.

* sp\_add\_job crée un travail appelé « Online Reindex PK\_\_VenueTyp\_\_265E44FD7FD4C885 ».
* sp\_add\_jobstep crée l’étape du travail contenant le texte de la commande T-SQL pour mettre à jour l’index.




## <a name="next-steps"></a>Étapes suivantes

Dans ce didacticiel, vous avez appris à effectuer les opérations suivantes :

> [!div class="checklist"]

> * Créer un compte de travail élastique pour interroger plusieurs locataires
> * Mettre à jour les données dans toutes les bases de données de locataire
> * Créer un index sur une table dans toutes les bases de données de locataire

[Didacticiel sur les analyses ad hoc](sql-database-saas-tutorial-adhoc-analytics.md)


## <a name="additional-resources"></a>Ressources supplémentaires

* [Autres didacticiels reposant sur le déploiement de l’application SaaS Wingtip](sql-database-wtp-overview.md#sql-database-wingtip-saas-tutorials)
* [Gestion des bases de données cloud avec montée en charge](sql-database-elastic-jobs-overview.md)
* [Créer et gérer des bases de données cloud avec montée en charge](sql-database-elastic-jobs-create-and-manage.md)
