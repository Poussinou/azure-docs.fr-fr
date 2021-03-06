---
title: "Créer votre première fonction à l’aide du Portail Azure | Microsoft Docs"
description: "Apprenez à créer votre première fonction Azure pour une exécution sans serveur à l’aide du portail Azure."
services: functions
documentationcenter: na
author: ggailey777
manager: erikre
editor: 
tags: 
ms.assetid: 96cf87b9-8db6-41a8-863a-abb828e3d06d
ms.service: functions
ms.devlang: multiple
ms.topic: hero-article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 05/10/2017
ms.author: glenga
ms.translationtype: Human Translation
ms.sourcegitcommit: 8f987d079b8658d591994ce678f4a09239270181
ms.openlocfilehash: 0c7ef7004618966ae8c2d32027c460b2b643923e
ms.contentlocale: fr-fr
ms.lasthandoff: 05/18/2017


---
# <a name="create-your-first-function-in-the-azure-portal"></a>Créer votre première fonction à l’aide du Portail Azure

Azure Functions vous permet d’exécuter votre code dans un environnement sans serveur et sans avoir à créer une machine virtuelle ou à publier une application web au préalable. Dans cette rubrique, vous allez découvrir comment utiliser Functions pour créer une fonction « hello world » dans le portail Azure.

![Créer une Function App dans le Portail Azure](./media/functions-create-first-azure-function/function-app-in-portal-editor.png)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="log-in-to-azure"></a>Connexion à Azure

Connectez-vous au [portail Azure](https://portal.azure.com/).

## <a name="create-a-function-app"></a>Créer une Function App

Vous devez disposer d’une Function App pour héberger l’exécution de vos fonctions. Une Function App vous permet de regrouper les fonctions en une unité logique pour faciliter la gestion, le déploiement et le partage des ressources. 

[!INCLUDE [Create function app Azure portal](../../includes/functions-create-function-app-portal.md)]

![Function App créée avec succès.](./media/functions-create-first-azure-function/function-app-create-success.png)

[!INCLUDE [functions-portal-favorite-function-apps](../../includes/functions-portal-favorite-function-apps.md)]

Créez ensuite une fonction dans la nouvelle Function App.

## <a name="create-function"></a>Créer une fonction déclenchée via HTTP

1. Développez votre nouvelle Function App, puis cliquez sur le bouton **+** en regard de **Fonctions**.

2.  Sur la page **Get started quickly** (Commencer rapidement), cliquez sur **WebHook + API**, choisissez le langage de votre fonction, puis cliquez sur **Créer cette fonction**. 
   
    ![Démarrage rapide de fonctions dans le portail Azure.](./media/functions-create-first-azure-function/function-app-quickstart-node-webhook.png)

Une fonction est créée dans le langage que vous avez choisi à l’aide du modèle de fonction déclenchée via HTTP. Vous pouvez exécuter la nouvelle fonction en envoyant une demande HTTP.

## <a name="test-the-function"></a>Tester la fonction

1. Dans votre nouvelle fonction, cliquez sur **</> Obtenir l’URL de fonction**, puis copiez **l’URL de fonction**. 

    ![Copier l’URL de fonction à partir du portail Azure](./media/functions-create-first-azure-function/function-app-develop-tab-testing.png)

2. Collez l’URL de la demande HTTP dans la barre d’adresses de votre navigateur. Ajoutez la chaîne de requête `&name=<yourname>` à cette URL et exécutez la demande. La capture d’écran suivante du navigateur montre la réponse renvoyée par la fonction suite à la demande GET :

    ![Réponse de la fonction dans le navigateur.](./media/functions-create-first-azure-function/function-app-browser-testing.png)

    L’URL de demande inclut une clé qui est requise, par défaut, pour accéder à votre fonction sur HTTP.   

## <a name="view-the-function-logs"></a>Afficher les journaux de fonction 

Lorsque votre fonction s’exécute, des informations de suivi sont écrites dans les journaux. Pour afficher la sortie de suivi de l’exécution précédente, revenez à votre fonction dans le portail et cliquez sur la flèche vers le haut figurant en bas de l’écran pour développer **Journaux**. 

![Affichage des journaux de fonction dans le portail Azure.](./media/functions-create-first-azure-function/function-view-logs.png)

## <a name="clean-up-resources"></a>Supprimer des ressources

[!INCLUDE [Clean up resources](../../includes/functions-quickstart-cleanup.md)]

## <a name="next-steps"></a>Étapes suivantes

Vous avez créé une Function App avec une simple fonction déclenchée via HTTP.  

[!INCLUDE [Next steps note](../../includes/functions-quickstart-next-steps.md)]

Pour plus d’informations, consultez [Liaisons HTTP et webhook Azure Functions](functions-bindings-http-webhook.md).




