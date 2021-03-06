---
title: Exemple de script Azure CLI - Supprimer un Cache Redis Azure | Microsoft Docs
description: Exemple de script Azure CLI - Supprimer un Cache Redis Azure
services: redis-cache
documentationcenter: 
author: steved0x
manager: douge
editor: 
tags: azure-service-management
ms.assetid: 7beded7a-d2c9-43a6-b3b4-b8079c11de4a
ms.service: cache-redis
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 04/14/2017
ms.author: sdanie
translationtype: Human Translation
ms.sourcegitcommit: e851a3e1b0598345dc8bfdd4341eb1dfb9f6fb5d
ms.openlocfilehash: f959823b3a7c5b0262f693ecad1e6efc4eec4f35
ms.lasthandoff: 04/15/2017

---

# <a name="delete-an-azure-redis-cache"></a>Supprimer un Cache Redis Azure

Dans ce scénario, vous apprenez comment supprimer un Cache Redis Azure.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

## <a name="sample-script"></a>Exemple de script

[!code-azurecli[main](../../../cli_scripts/redis-cache/delete-cache/delete-cache.sh "Cache Redis Azure")]

[!INCLUDE [cli-script-clean-up](../../../includes/redis-cli-script-clean-up.md)]

## <a name="script-explanation"></a>Explication du script

Ce script utilise les commandes suivantes pour supprimer une instance du Cache Redis Azure. Chaque commande du tableau renvoie à une documentation spécifique.

| Commande | Remarques |
|---|---|
| [az redis delete](https://docs.microsoft.com/cli/azure/redis#delete) | Supprimez une instance de Cache Redis. |


## <a name="next-steps"></a>Étapes suivantes

Pour plus d’informations sur l’interface Azure CLI, consultez la [documentation relative à l’interface Azure CLI](https://docs.microsoft.com/cli/azure/overview).

Pour des exemples supplémentaires de scripts CLI de Cache Redis Azure, voir la [documentation du Cache Redis Azure](../cli-samples.md).
