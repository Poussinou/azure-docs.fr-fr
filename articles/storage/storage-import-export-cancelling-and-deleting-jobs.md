---
title: "Annulation et suppression d’un travail d’importation Azure Import/Export | Microsoft Docs"
description: "Découvrez comment annuler et supprimer des travaux pour le service Microsoft Azure Import/Export."
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: 
ms.assetid: fd3d66f0-1dbb-4c75-9223-307d5abaeefc
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: muralikk
translationtype: Human Translation
ms.sourcegitcommit: 432752c895fca3721e78fb6eb17b5a3e5c4ca495
ms.openlocfilehash: e0a7ff391e5a03ed563912dea54c7cfe73111bcf
ms.lasthandoff: 03/30/2017


---

# <a name="canceling-and-deleting-azure-importexport-jobs"></a>Annulation et suppression de travaux du service Azure Import/Export

Vous pouvez demander qu’un travail soit annulé avant qu’il ne soit dans l’état `Packaging` en appelant l’opération [Update Job Properties](/rest/api/storageimportexport/jobs#Jobs_Update) et en définissant l’élément `CancelRequested` sur `true`. Le travail sera annulé de manière optimale. Si des données sont en cours de transfert sur les disques, cette opération peut se poursuivre même après que l’annulation ait été demandée.

 Un travail annulé passe à l’état `Completed` et sera conservé pendant 90 jours, après quoi il sera supprimé.

 Pour supprimer un travail, appelez l’opération [Delete Job](/rest/api/storageimportexport/jobs#Jobs_Delete) avant que le travail ne soit expédié (*c’est-à-dire*, pendant que le travail est dans l’état `Creating`). Vous pouvez également supprimer un travail lorsqu’il se trouve dans l’état `Completed`. Après la suppression d’un travail, ses informations et son état ne sont plus accessibles via l’API REST ou le portail Azure.

## <a name="next-steps"></a>Étapes suivantes

* [Utilisation de l’API REST du service Import/Export](storage-import-export-using-the-rest-api.md)

