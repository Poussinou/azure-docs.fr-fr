---
title: Composants et versions Hadoop - Azure HDInsight | Microsoft Docs
description: "Découvrez les composants et versions Hadoop de HDInsight et les niveaux de service disponibles dans cette distribution par cloud de HortonWorks Data Platform."
keywords: "versions Hadoop,composants de l’écosystème hadoop,composants hadoop,comment vérifier la version hadoop"
services: hdinsight
editor: cgronlun
manager: asadk
author: bprakash
tags: azure-portal
documentationcenter: 
ms.assetid: 367b3f4a-f7d3-4e59-abd0-5dc59576f1ff
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/14/2017
ms.author: bprakash
ms.translationtype: Human Translation
ms.sourcegitcommit: c308183ffe6a01f4d4bf6f5817945629cbcedc92
ms.openlocfilehash: f6f50906766408226ad3ab9fdfd67b4707961b9b
ms.contentlocale: fr-fr
ms.lasthandoff: 05/17/2017

 

---
# <a name="what-are-the-hadoop-components-and-versions-available-with-hdinsight"></a>Quels sont les composants et versions Hadoop disponibles avec HDInsight ?

Découvrez les composants et versions de l’écosystème Hadoop inclus dans Azure HDInsight, ainsi que les niveaux de service Standard et Premium. Découvrez également comment vérifier les versions des composants Hadoop dans HDInsight. 

Chaque version HDInsight est une distribution par cloud d’une version de HortonWorks Data Platform (HDP).

## <a name="hadoop-components-available-with-different-hdinsight-versions"></a>Composants Hadoop disponibles avec différentes versions de HDInsight
Azure HDInsight prend en charge plusieurs versions de cluster Hadoop qui peuvent être déployées à tout moment. Le choix d'une version crée une version spécifique de la distribution de la plateforme de données Hortonworks (HDP) et un ensemble de composants qui sont contenus dans cette distribution. Notez que la version de cluster par défaut actuellement utilisée par Azure HDInsight est la version 3.5, et que, depuis le 17 février 2017, elle est basée sur HDP 2.5.

Les versions de composants associées aux versions de cluster HDInsight sont répertoriées dans le tableau suivant : 

> [!NOTE]
> La version par défaut du service peut changer sans préavis. Si vous êtes dépendant d’une version, nous vous recommandons de spécifier la version lorsque vous créez des clusters à l’aide du Kit de développement logiciel (SDK) .NET/Azure PowerShell et Azure CLI. 
>
>


| Composant | HDInsight version 3.6 | HDInsight Version 3.5 (par défaut) | HDInsight version 3.4 | HDInsight version 3.3 | HDInsight Version 3.2 | HDInsight version 3.1 | HDInsight Version 3,0 |
| --- | --- | --- | --- | --- | --- | --- |--- |
| Hortonworks Data Platform |2.6 |2.5 |2.4 |2.3 |2.2 |2.1.7 |2.0 |
| Apache Hadoop et YARN |2.7.3 |2.7.3 |2.7.1 |2.7.1 |2.6.0 |2.4.0 |2.2.0 |
| Apache Tez |0.7.0 |0.7.0 |0.7.0 |0.7.0 |0.5.2 |0.4.0 |-|
| Apache Pig |0.16.0 |0.16.0 |0.15.0 |0.15.0 |0.14.0 |0.12.1 |0.12.0 |
| Apache Hive et HCatalog |1.2.1 |1.2.1 |1.2.1 |1.2.1 |0.14.0 |0.13.1 |0.12.0 |
| Apache Hive2 | 2.1.0 |-|-|-|-|-|-|
| Apache Tez-Hive2 | 0.8.4 |-|-|-|-|-|-|
| Apache Ranger | 0.7.0 |0.6.0 |-|-|-|-|-|
| Apache HBase |1.1.2 |1.1.2 |1.1.2 |1.1.1 |0.98.4 |0.98.0 |-|
| Apache Sqoop |1.4.6 |1.4.6 |1.4.6 |1.4.6 |1.4.5 |1.4.4 |1.4.4 |
| Apache Oozie |4.2.0 |4.2.0 |4.2.0 |4.2.0 |4.1.0 |4.0.0 |4.0.0 |
| Apache Zookeeper |3.4.6 |3.4.6 |3.4.6 |3.4.6 |3.4.6 |3.4.5 |3.4.5 |
| Apache Storm |1.1.0 |1.0.1 |0.10.0 |0.10.0 |0.9.3 |0.9.1 |-|
| Apache Mahout |0.9.0+ |0.9.0+ |0.9.0+ |0.9.0+ |0.9.0 |0.9.0 |-|
| Apache Phoenix |4.7.0 |4.7.0 |4.4.0 |4.4.0 |4.2.0 |4.0.0.2.1.7.0-2162 |-|
| Apache Spark |2.1.0 (Linux uniquement) |1.6.2 + 2.0 (Linux uniquement) |1.6.0 (Linux uniquement) |1.5.2 (Linux uniquement/version expérimentale) |1.3.1 (Windows uniquement) |-|-|
| Apache Kafka | 0.10.0 | 0.10.0 | 0.9.0 |-|-|-|-|
| Apache Ambari | 2.5.0 | 2.4.0 | 2.2.1 | 2.1.0 |-|-|-|
| Apache Zeppelin | 0.7.0 |-|-|-|-|-|-|
| Mono |4.2.1 |4.2.1 |3.2.8 |-|-|-|


## <a name="how-to-check-current-hadoop-component-version-information"></a>Comment vérifier les informations de version du composant Hadoop actuel

Les versions de composant d’écosystème Hadoop associées aux versions de cluster HDInsight peuvent changer avec les mises à jour de HDInsight. Pour vérifier les composants Hadoop ainsi que les versions utilisées pour un cluster, utilisez l’API REST Ambari. La commande **GetComponentInformation** extrait des informations sur les composants du service. Pour plus d’informations, consultez la page [Documentation Ambari][ambari-docs].

Pour les clusters basés sur Windows uniquement : une autre manière d’obtenir les versions des composants consiste à se connecter à un cluster à l’aide du Bureau à distance, puis à examiner directement le contenu du répertoire « C:\apps\dist\".

> [!IMPORTANT]
> Linux est le seul système d’exploitation utilisé sur HDInsight version 3.4 ou supérieure. Pour plus d’informations, voir [Obsolescence de HDInsight 3.3](#hdi-version-33-nearing-deprecation-date). 

### <a name="release-notes"></a>Notes de publication

Pour d’autres notes de publication sur les dernières versions de HDInsight, consultez la page [Notes de publication de HDInsight](hdinsight-release-notes.md) .

## <a name="supported-hdinsight-versions"></a>Versions de HDInsight prises en charge
Le tableau suivant répertorie les versions de HDInsight actuellement disponibles, les versions correspondantes de la plateforme de données Hortonworks utilisées par ces dernières, ainsi que leurs dates de lancement. La date d’expiration de la prise en charge et la date d’obsolescence sont également indiquées. Notez les informations suivantes concernant les versions :

* Seuls les clusters à haute disponibilité avec deux nœuds principaux sont déployés par défaut pour les clusters HDInsight 2.1 et versions ultérieures. Ils ne sont pas disponibles pour les clusters HDInsight 1.6.
* Une fois que la prise en charge a expiré pour une version particulière, elle n’est plus disponible par l’intermédiaire du portail Azure. Le tableau suivant indique quelles versions sont disponibles sur le portail Azure Classic. Les versions des clusters restent disponibles avec le paramètre `Version` de la commande Windows PowerShell [New-AzureRmHDInsightCluster](https://msdn.microsoft.com/library/mt619331.aspx) et le Kit de développement logiciel (SDK) .NET jusqu’à leur date d’obsolescence.

| Version de HDInsight | Version de HDP | SYSTÈME D’EXPLOITATION DE LA MACHINE VIRTUELLE | Haute disponibilité | Date de lancement | Disponible sur le portail Azure | Date d'expiration du support | Date d'obsolescence |
| --- | --- | --- | --- | --- | --- | --- | --- |
| HDI 3.6 |HDP 2.6 |Ubuntu 16 |Oui |04/06/2017 |Oui | | |
| HDI 3.5 |HDP 2.5 |Ubuntu 16 |Oui |9/30/2016 |Oui |07/05/2017 |05/31/2018 |
| HDI 3.4 |HDP 2.4 |Ubuntu 14.0.4 LTS |Oui |29/03/2016 |Oui |12/29/2016 |1/9/2018 |
| HDI 3.3 |HDP 2.3 |Ubuntu 14.0.4 LTS ou Windows Server 2012R2 |Oui |02/12/2015 |Oui |27/06/2016 |31/07/2017 |
| HDI 3.2 |HDP 2.2 |Ubuntu 12.04 LTS ou Windows Server 2012R2 |Oui |2/18/2015 |Non |03/01/2016 |04/01/2017 |
| HDI 3,1 |HDP 2,1 |Windows Server 2012 R2 |Oui |6/24/2014 |Non |18/05/2015 |30/06/2016 |
| HDI 3,0 |HDP 2,0 |Windows Server 2012 R2 |Oui |02/11/2014 |Non |09/17/2014 |06/30/2015 |
| HDI 2,1 |HDP 1,3 |Windows Server 2012 R2 |Oui |10/28/2013 |Non |05/12/2014 |05/31/2015 |
| HDI 1.6 |HDP 1.1 | |Non |10/28/2013 |Non |04/26/2014 |05/31/2015 |

## <a name="hdi-version-33-nearing-deprecation-date"></a>HDI 3.3 approche de sa date d’obsolescence
La prise en charge du cluster HDI 3.3 a expiré le 27/06/2016 et il sera déconseillé le 31/07/2017. Si vous avez un cluster HDI 3.3, mettez-le rapidement à niveau en passant à HDI 3.5 ou 3.6. Le calendrier d’obsolescence pour HDI 3.3 Windows peut varier d’une région à une autre. Si la date d’obsolescence planifiée de votre région est différente, vous êtes averti séparément.

### <a name="the-service-level-agreement-for-hdinsight-cluster-versions"></a>Contrat de niveau de service pour les versions de cluster HDInsight
Le contrat de niveau de service (SLA) est défini en termes de **fenêtre de support**. Une fenêtre de support désigne la durée pendant laquelle une version de cluster HDInsight est prise en charge par le support technique et le service clientèle Microsoft. Un cluster HDInsight est en dehors de la fenêtre de support si la **Date d'expiration du support** correspondant à la version de ce cluster est postérieure à la date actuelle. Le tableau ci-dessus contient une liste des versions de cluster HDInsight prises en charge. La date d'expiration du support pour une version donnée de HDInsight (lorsqu'une version X+1 plus récente est disponible) est calculée comme suit, la date la plus tardive prévalant :  

* Formule 1 : ajoutez 180 jours à la date de lancement du cluster HDInsight version X.
* Formule 2 : ajoutez 90 jours à la date à laquelle le cluster HDInsight version X+1 (la version qui suit la version X) est mis à disposition dans le portail.

La **Date d’obsolescence** est la date après laquelle la version du cluster ne peut plus être créée sur HDInsight. À partir du 31 juillet 2017, il ne sera plus possible de redimensionner un cluster après sa date d’obsolescence. 

> [!NOTE]
> Les clusters HDInsight sous Windows (y compris les versions 2.1, 3.0, 3.1, 3.2 et 3.3) s’exécutent sur la 4e famille de systèmes d’exploitation invités d’Azure, qui utilise la version 64 bits de Windows Server 2012 R2 et prend en charge .NET Framework 4.0, 4.5, 4.5.1 et 4.5.2.
>
>

## <a name="hortonworks-release-notes-associated-with-hdinsight-versions"></a>Notes de publication de Hortonworks associées aux versions de HDInsight
* Le cluster HDInsight version 3.6 utilise une distribution Hadoop basée sur [Hortonworks Data Platform 2.6](http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.6.0/bk_release-notes/content/ch_relnotes.html). 
* Le cluster HDInsight version 3.5 utilise une distribution Hadoop basée sur [Hortonworks Data Platform 2.5](http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.5.0/bk_release-notes/content/ch_relnotes_v250.html). Il s’agit du cluster Hadoop **par défaut** créé lors de l’utilisation du portail.
* Le cluster HDInsight version 3.4 utilise une distribution Hadoop basée sur [Hortonworks Data Platform 2.4](http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.4.0/bk_HDP_RelNotes/content/ch_relnotes_v240.html). 
* Le cluster HDInsight version 3.3 utilise une distribution Hadoop basée sur [Hortonworks Data Platform 2.3](http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.3.0/bk_HDP_RelNotes/content/ch_relnotes_v230.html).

  * Les notes de publication d’Apache Storm sont disponibles [ici](https://storm.apache.org/2015/11/05/storm0100-released.html).
  * Les notes de publication d’Apache Hive sont disponibles [ici](https://issues.apache.org/jira/secure/ReleaseNote.jspa?version=12332384&styleName=Text&projectId=12310843).
* Le cluster HDInsight version 3.2 utilise une distribution Hadoop basée sur [Hortonworks Data Platform 2.2][hdp-2-2].  

  * Notes de publication pour les composants spécifiques d’Apache - [Hive 0.14](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310843&version=12326450), [Pig 0.14](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310730&version=12326954), [HBase 0.98.4](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310753&version=12326810), [Phoenix 4.2.0](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12315120&version=12327581), [M/R 2.6](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310941&version=12327180), [HDFS 2.6](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310942&version=12327181), [YARN 2.6](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12313722&version=12327197), [Common](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310240&version=12327179), [Tez 0.5.2](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12314426&version=12328742), [Ambari 2.0](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12312020&version=12327486), [Storm 0.9.3](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12314820&version=12327112), [Oozie 4.1.0](https://issues.apache.org/jira/secure/ReleaseNote.jspa?version=12324960&projectId=12311620).
* Le cluster HDInsight version 3.1 utilise une distribution Hadoop basée sur [Hortonworks Data Platform 2.1.7][hdp-2-1-7]. Les clusters HDInsight 3.1 créés avant le 07/11/2014 étaient basés sur [Hortonworks Data Platform 2.1.1][hdp-2-1-1].
* Le cluster HDInsight version 3.0 utilise une distribution Hadoop basée sur [Hortonworks Data Platform 2.0][hdp-2-0-8].
* Le cluster HDInsight version 2.1 utilise une distribution Hadoop basée sur [Hortonworks Data Platform 1.3][hdp-1-3-0].
* Le cluster HDInsight version 1.6 utilise une distribution Hadoop basée sur [Hortonworks Data Platform 1.1][hdp-1-1-0].

## <a name="hdinsight-standard-and-hdinsight-premium"></a>HDInsight Standard et HDInsight Premium

Azure HDInsight propose deux catégories d’offres de cloud Big Data : **Standard** et **Premium**. Le tableau sous la section répertorie les fonctionnalités disponibles **uniquement dans le cadre de l’édition Premium**. Les fonctionnalités qui ne sont pas explicitement indiquées dans ce tableau sont disponibles dans le cadre de l’édition Standard.

> [!NOTE]
> L’offre HDInsight Premium est actuellement en version préliminaire, disponible uniquement pour les clients Linux.
>
>

| Fonctionnalité HDInsight Premium | Description |
| --- | --- |
| Clusters HDInsight joints à un domaine |Joignez des clusters HDInsight à des domaines Azure Active Directory (AAD) pour la sécurité de niveau entreprise. Vous pouvez maintenant configurer une liste d’employés de votre entreprise qui peuvent s’authentifier via Azure Active Directory pour se connecter à un cluster HDInsight. L’administrateur d’entreprise peut également configurer le contrôle d’accès en fonction du rôle pour la sécurité Hive à l’aide [d’Apache Ranger](http://hortonworks.com/apache/ranger/), limitant ainsi l’accès aux données de manière appropriée. Enfin, l’administrateur peut auditer les données auxquelles les employés ont accès et les modifications apportées aux stratégies de contrôle d’accès, d’où un degré élevé de gouvernance des ressources de l’entreprise. Pour plus d’informations, consultez la section [Configurer des clusters HDInsight joints à un domaine](hdinsight-domain-joined-configure.md). |

### <a name="cluster-types-supported-for-hdinsight-premium"></a>Types de clusters pris en charge pour HDInsight Premium
Le tableau suivant répertorie le type de cluster HDInsight et la matrice de support Premium.

| Type de cluster | Standard | Premium (version préliminaire) |
| --- | --- | --- |
| Hadoop |Oui |Oui (HDInsight 3.5 et 3.6 uniquement) |
| Spark |Oui |Non |
| HBase |OUI |Non |
| Storm |OUI |Non |
| R Server  |Oui |Non |
| Hive interactif (version préliminaire) |Oui |Non |
| Kafka (préversion)|Oui|Non| 

Ce tableau est mis à jour au fur et à mesure que des types de clusters supplémentaires sont inclus dans HDInsight Premium.

### <a name="features-not-supported-for-hdinsight-premium"></a>Fonctionnalités non prises en charge pour HDInsight Premium

Les fonctionnalités suivantes ne sont actuellement pas prises en charge pour les clusters HDInsight Premium.

* **Pas de prise en charge pour Azure Data Lake Store comme stockage principal**. Vous pouvez néanmoins utiliser Azure Data Lake Store comme stockage complémentaire avec des clusters HDInsight Premium.

### <a name="pricing-and-sla"></a>Tarifs et contrat SLA
Pour plus d’informations sur la tarification et le contrat SLA pour l’édition Premium de HDInsight, consultez [Tarification HDInsight](https://azure.microsoft.com/pricing/details/hdinsight/).


[image-hdi-versioning-versionscreen]: ./media/hdinsight-component-versioning/hdi-versioning-version-screen.png

[wa-forums]: http://azure.microsoft.com/support/forums/

[connect-excel-with-hive-ODBC]: hdinsight-connect-excel-hive-ODBC-driver.md

[hdp-2-2]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.2.0/HDP_2.2.0_Release_Notes_20141202_version/index.html

[hdp-2-1-7]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.7-Win/bk_releasenotes_HDP-Win/content/ch_relnotes-HDP-2.1.7.html

[hdp-2-1-1]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.1/bk_releasenotes_hdp_2.1/content/ch_relnotes-hdp-2.1.1.html

[hdp-2-0-8]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.0.8.0/bk_releasenotes_hdp_2.0/content/ch_relnotes-hdp2.0.8.0.html

[hdp-1-3-0]: http://docs.hortonworks.com/HDPDocuments/HDP1/HDP-1.3.0/bk_releasenotes_hdp_1.x/content/ch_relnotes-hdp1.3.0_1.html

[hdp-1-1-0]: http://docs.hortonworks.com/HDPDocuments/HDP1/HDP-Win-1.1/bk_releasenotes_HDP-Win/content/ch_relnotes-hdp-win-1.1.0_1.html

[ambari-docs]: https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md

[zookeeper]: http://zookeeper.apache.org/

