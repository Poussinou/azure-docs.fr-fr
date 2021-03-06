---
title: "Exemple de script Azure CLI - Lier un certificat SSL personnalisé à une Function App | Microsoft Docs"
description: "Exemple de script Azure CLI - Lier un certificat SSL personnalisé à une Function App dans Azure"
services: functions
documentationcenter: 
author: ggailey777
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: eb95d350-81ea-4145-a1e2-6eea3b7469b2
ms.service: functions
ms.workload: na
ms.devlang: azurecli
ms.tgt_pltfrm: na
ms.topic: sample
ms.date: 04/10/2017
ms.author: glenga
ms.custom: mvc
ms.translationtype: Human Translation
ms.sourcegitcommit: 9568210d4df6cfcf5b89ba8154a11ad9322fa9cc
ms.openlocfilehash: 9b7f84f5ad01335a3ce72b6b2aa654968fdd9499
ms.contentlocale: fr-fr
ms.lasthandoff: 05/15/2017

---
# <a name="bind-a-custom-ssl-certificate-to-a-function-app"></a>Lier un certificat SSL personnalisé à une Function App

Cet exemple de script crée une Function App dans App Service avec ses ressources associées, puis y lie le certificat SSL d’un nom de domaine personnalisé. Pour cet exemple, vous avez besoin des éléments suivants :

* Un accès à la page de configuration DNS du Registre de votre domaine.
* Un fichier .PFX valide et son mot de passe pour le certificat SSL que vous voulez charger et lier.

Pour lier un certificat SSL, votre Function App doit être créée dans un plan App Service et non dans un plan de consommation.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

## <a name="sample-script"></a>Exemple de script

[!code-azurecli-interactive[main](../../../cli_scripts/azure-functions/configure-ssl-certificate/configure-ssl-certificate.sh?highlight=3-5 "Lier un certificat SSL personnalisé à une application web")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Explication du script

Ce script utilise les commandes suivantes. Chaque commande du tableau renvoie à une documentation spécifique.

| Commande | Remarques |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group#create) | Crée un groupe de ressources dans lequel toutes les ressources sont stockées. |
| [az appservice plan create](https://docs.microsoft.com/cli/azure/appservice/plan#create) | Crée un plan App Service requis pour lier des certificats SSL. |
| [az functionapp create]() | Crée une Function App. |
| [az appservice web config hostname add](https://docs.microsoft.com/cli/azure/appservice/web/config/hostname#add) | Mappe un domaine personnalisé à la Function App. |
| [az appservice web config ssl upload](https://docs.microsoft.com/cli/azure/appservice/web/config/ssl#upload) | Charge un certificat SSL personnalisé sur une Function App. |
| [az appservice web config ssl bind](https://docs.microsoft.com/en-us/cli/azure/appservice/web/config/ssl#bind) | Lie un certificat SSL chargé à une Function App. |

## <a name="next-steps"></a>Étapes suivantes

Pour plus d’informations sur l’interface Azure CLI, consultez la [documentation relative à l’interface Azure CLI](https://docs.microsoft.com/cli/azure/overview).

Vous trouverez des exemples supplémentaires de scripts CLI App Service dans la [documentation relative à Azure App Service]().

