---
title: "Restaurer une base de données Azure SQL Database dans une application mutualisée | Documents Microsoft"
description: "Découvrez comment restaurer une base de données SQL à client unique après la suppression accidentelle des données"
keywords: "didacticiel sur les bases de données SQL"
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: tutorial
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 05/10/2017
ms.author: billgib;sstein
ms.translationtype: Human Translation
ms.sourcegitcommit: 95b8c100246815f72570d898b4a5555e6196a1a0
ms.openlocfilehash: 8567061a98ec5a0619a8e10cb44501dd88d8166c
ms.contentlocale: fr-fr
ms.lasthandoff: 05/18/2017


---
# <a name="restore-a-single-tenant-database"></a>Restaurer une base de données à client unique

De par sa conception, l’application SaaS Wingtip Tickets utilise un modèle de base de données par client, où chaque client possède sa propre base de données. L’un des avantages de ce modèle est la facilité avec laquelle il est possible de restaurer les données d’un client unique de manière isolée, sans affecter les autres clients.

Dans ce didacticiel, vous allez découvrir deux modèles de récupération des données :

> [!div class="checklist"]

> * Restaurer une base de données dans une base de données parallèle (côte à côte)
> * Restaurer une base de données sur place


|||
|:--|:--|
| **Restaurer le client à un point antérieur dans le temps, dans une base de données parallèle** | Ce modèle peut être utilisé par le client pour la révision, l’audit, la conformité, etc. La base de données d’origine reste inchangée et en ligne. |
| **Restaurer un client sur place** | Ce modèle est généralement utilisé pour récupérer un client à un point antérieur dans le temps, après la suppression accidentelle des données. La base de données d’origine est mise hors connexion et remplacée par la base de données restaurée. |
|||

Pour suivre ce didacticiel, vérifiez que les conditions préalables suivantes sont bien satisfaites :

* L’application WTP est déployée. Pour la déployer en moins de cinq minutes, consultez [Déployer et découvrir l’application SaaS WTP](sql-database-saas-tutorial.md).
* Azure PowerShell est installé. Pour plus d’informations, voir [Bien démarrer avec Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps).

## <a name="introduction-to-the-saas-tenant-restore-pattern"></a>Introduction au modèle SaaS de restauration d’un client

Il existe deux modèles simples pour restaurer les données d’un client individuel. Étant donné que les bases de données des clients sont isolées les unes des autres, la restauration d’un client n’a aucun impact sur les données des autres clients.

Dans le premier modèle, les données sont restaurées dans une nouvelle base de données. Le client se voit ensuite accorder l’accès à cette base de données en même temps que celui aux données de production qu’elle contient. Ce modèle permet à un administrateur client d’examiner les données restaurées et de potentiellement pouvoir les utiliser pour remplacer les valeurs de données actuelles de manière sélective. Il incombe au concepteur d’applications SaaS de déterminer le niveau de sophistication des options de récupération des données. Dans certains scénarios, il est parfois simplement demandé de pouvoir analyser les données dans l’état dans lequel elles étaient à un instant t. Si la base de données utilise la [géoréplication](sql-database-geo-replication-overview.md), nous vous recommandons de copier les données requises de la copie restaurée vers la base de données d’origine. Si vous remplacez la base de données d’origine par la base de données restaurée, vous devez reconfigurer et resynchroniser la géoréplication.

Dans le second modèle (qui suppose que le client a perdu des données ou qu’elles sont corrompues), la base de données de production du client est restaurée à un point antérieur dans le temps. Dans le modèle de restauration sur place, le client est mis hors connexion pendant un bref instant, le temps que la base de données soit restaurée et remise en ligne. La base de données d’origine est supprimée, mais elle peut toujours être restaurée si vous avez besoin de revenir à un point encore plus antérieur dans le temps. Il existe une variante de ce modèle : vous pouvez renommer la base de données au lieu de la supprimer, bien que cela n’offre aucun avantage supplémentaire en termes de sécurité des données.

## <a name="get-the-wingtip-application-scripts"></a>Obtenir les scripts d’application Wingtip

Les scripts et le code source de l’application Wingtip Tickets sont disponibles dans le référentiel GitHub [WingtipSaaS](https://github.com/Microsoft/WingtipSaaS). Les fichiers de script se trouvent dans le [dossier Learning Modules](https://github.com/Microsoft/WingtipSaaS/tree/master/Learning%20Modules). Téléchargez le dossier **Learning Modules** en local sur votre ordinateur, tout en conservant l’arborescence.

## <a name="simulate-a-tenant-accidentally-deleting-data"></a>Simuler la suppression accidentelle des données par le client

Pour illustrer ces scénarios de récupération, nous devons *accidentellement* supprimer certaines données dans l’une des bases de données client. Même si vous pouvez supprimer n’importe quel enregistrement, l’étape suivante dans la démonstration permet de ne pas rester bloqué suite à une violation de l’intégrité du référentiel. Elle ajoute également des données d’achat de ticket à utiliser ultérieurement dans les *didacticiels WTP Analytics*.

Exécutez le script de génération de ticket et créez des données supplémentaires. Intentionnellement, le générateur de ticket n’achète pas de ticket pour le dernier événement des clients.

1. Ouvrez ...\\Learning Modules\\Utilities\\*Demo-TicketGenerator.ps1* dans *PowerShell ISE*
1. Appuyez sur **F5** pour exécuter le script et générer les clients et les données de ventes de ticket.


### <a name="open-the-events-app-to-review-the-current-events"></a>Ouvrir l’application Events pour passer en revue les événements en cours

1. Ouvrez *Events Hub* (http://events.wtp.&lt;user&gt;.trafficmanager.net) et cliquez sur **Contoso Concert Hall** :

   ![events hub](media/sql-database-saas-tutorial-restore-single-tenant/events-hub.png)

1. Faites défiler la liste des événements, puis notez le dernier événement de la liste :

   ![dernier événement](media/sql-database-saas-tutorial-restore-single-tenant/last-event.png)


### <a name="run-the-demo-scenario-to-accidentally-delete-the-last-event"></a>Exécuter le scénario de démonstration pour supprimer accidentellement le dernier événement

1. Ouvrez ...\\Learning Modules\\Business Continuity and Disaster Recovery\\RestoreTenant\\*Demo-RestoreTenant.ps1* dans *PowerShell ISE*, puis définissez la valeur suivante :
   * **$DemoScenario** = **1** Donnez-lui la valeur **1** - Delete events with no ticket sales.
1. Appuyez sur **F5** pour exécuter le script et supprimer le dernier événement. Vous devez recevoir un message de confirmation similaire à celui ci-dessous :

   ```Console
   Deleting unsold events from Contoso Concert Hall ...
   Deleted event 'Seriously Strauss' from Contoso Concert Hall venue.
   ```

1. La page des événements de Contoso s’ouvre. Faites défiler vers le bas et vérifiez que l’événement a disparu. Si l’événement figure toujours dans la liste, actualisez la page et vérifiez qu’il a disparu.

   ![dernier événement](media/sql-database-saas-tutorial-restore-single-tenant/last-event-deleted.png)


## <a name="restore-a-tenant-database-in-parallel-with-the-production-database"></a>Restaurer une base de données client en parallèle avec la base de données de production

Cet exercice restaure la base de données Contoso Concert Hall à un point dans le temps, avant la suppression de l’événement. Après avoir supprimé l’événement dans les étapes précédentes, vous voulez le récupérer et afficher les données supprimées. Vous n’avez pas besoin de restaurer votre base de données de production avec l’enregistrement supprimé. Toutefois, pour des raisons professionnelles, vous devez récupérer l’ancienne base de données pour accéder aux anciennes données.

 Le script *Restore-TenantInParallel.ps1* crée une base de données client parallèle et une entrée de catalogue parallèle, toutes deux appelées *ContosoConcertHall\_ancien*. Ce modèle de restauration convient davantage dans le cas d’une récupération après une perte de données mineure ou dans des scénarios de récupération de conformité et d’audit. Cette approche est également recommandée lorsque vous utilisez la [géoréplication](sql-database-geo-replication-overview.md).

1. Terminez la section [simuler une suppression accidentelle de données par l’utilisateur](#simulate-a-tenant-accidentally-deleting-data).
1. Ouvrez …\\Learning Modules\\Business Continuity and Disaster Recovery\\RestoreTenant\\_Demo-RestoreTenant.ps1_ dans *PowerShell ISE*.
1. **$DemoScenario** = **2**. Donnez-lui la valeur **2** *Restore tenant in parallel*.
1. Appuyez sur **F5** pour exécuter le script.

Le script restaure la base de données client (dans une base de données parallèle) à un point dans le temps, avant la suppression de l’événement dans la section précédente. Il crée une deuxième base de données, supprime toutes les métadonnées de catalogue existantes dans cette base de données, puis ajoute la base de données au catalogue sous l’entrée *ContosoConcertHall\_old*.

Ce script de démonstration ouvre la page des événements dans votre navigateur. À noter dans l’URL : ```http://events.wtp.&lt;user&gt;.trafficmanager.net/contosoconcerthall_old``` les données sont affichées d’après la base de données restaurée et *_old* est ajouté au nom.

Faites défiler les événements répertoriés dans le navigateur pour confirmer que l’événement supprimé dans la section précédente a bien été restauré.

Notez que le fait d’exposer le client restauré en tant que client supplémentaire (avec sa propre application Events pour rechercher des tickets) n’est probablement pas ce que vous feriez pour fournir un accès client aux données restaurées. Néanmoins, cela nous permet d’illustrer facilement le modèle de restauration.

En réalité, mieux vaudrait probablement conserver cette base de données restaurée pendant une période définie uniquement. Vous pouvez supprimer l’entrée de client restauré une fois que vous avez terminé. Pour cela, appelez le script *Remove-RestoredTenant.ps1*.

1. Définissez **$DemoScenario** sur **4** pour sélectionner le scénario *remove restored tenant*.
1. **Appuyez** **sur** **F5** pour exécuter le script.
1. L’entrée *ContosoConcertHall\_old* est maintenant supprimée du catalogue. Poursuivez et fermez la page des événements pour ce client dans votre navigateur.


## <a name="restore-a-tenant-in-place-replacing-the-existing-tenant-database"></a>Restaurer un client sur place en remplaçant la base de données client existante

Cet exercice restaure le client Contoso Concert Hall à un point dans le temps, avant la suppression de l’événement. Le script *Restore-TenantInPlace* restaure la base de données client actuelle dans une nouvelle base de données qui pointe vers un point antérieur dans le temps, puis supprime la base de données d’origine. Ce modèle de restauration convient davantage dans le cas d’une récupération après une grave corruption des données, dans la mesure où le client peut faire face à une perte importante de données.

1. Ouvrez le fichier **Demo-RestoreTenant.ps1** dans PowerShell ISE.
1. Définissez **$DemoScenario** sur **5** pour sélectionner le scénario *restore tenant in place*.
1. Appuyez sur **F5** pour exécuter le script.

Le script restaure la base de données client cinq minutes avant la suppression de l’événement. Pour y parvenir, il place tout d’abord le client Contoso Concert Hall hors connexion pour qu’aucune nouvelle mise à jour ne soit apportée aux données. Ensuite, une base de données parallèle est créée laquelle bénéficie d’une restauration à partir du point de restauration. Elle est horodatée dans son nom pour garantir que le nom de la base de données n’entre pas en conflit avec le nom de base de données client existante. Ensuite, l’ancienne base de données client est supprimée, et la base de données restaurée est renommée d’après le nom de la base de données d’origine. Enfin, la connexion de Contoso Concert Hall est rétablie pour permettre l’accès de l’application à la base de données restaurée.

Vous avez correctement restauré la base de données à un point dans le temps, avant la suppression de l’événement. La page Events s’ouvre. Confirmez alors que le dernier événement a été restauré.


## <a name="next-steps"></a>Étapes suivantes

Dans ce didacticiel, vous avez appris à :

> [!div class="checklist"]

> * Restaurer une base de données dans une base de données parallèle (côte à côte)
> * Restaurer une base de données sur place

[Gérer le schéma de base de données client](sql-database-saas-tutorial-schema-management.md)

## <a name="additional-resources"></a>Ressources supplémentaires

* [Autres didacticiels reposant sur le déploiement initial d’applications Wingtip Tickets Platform (WTP)](sql-database-wtp-overview.md#sql-database-wtp-saas-tutorials)
* [Vue d’ensemble de la continuité de l’activité avec la base de données Azure SQL](sql-database-business-continuity.md)
* [En savoir plus sur les sauvegardes SQL Database](sql-database-automated-backups.md)
