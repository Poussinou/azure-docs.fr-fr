---
title: "Créer une application PHP sur Azure Web App | Microsoft Docs"
description: "Déployez votre premier site Hello World en PHP dans l’application web App Service en quelques minutes."
services: app-service\web
documentationcenter: 
author: syntaxc4
manager: erikre
editor: 
ms.assetid: 6feac128-c728-4491-8b79-962da9a40788
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 05/04/2017
ms.author: cfowler
ms.custom: mvc
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: 0541778e07193c4903a90ce0b91db224bdf60342
ms.contentlocale: fr-fr
ms.lasthandoff: 05/10/2017

---
# <a name="create-a-php-application-on-web-app"></a>Créer une application en PHP sur une application web

Ce didacticiel de démarrage rapide vous guide lors du développement et du déploiement d’une application en PHP dans Azure. Nous allons exécuter l’application à l’aide d’un [plan Azure App Service](https://docs.microsoft.com/azure/app-service/azure-web-sites-web-hosting-plans-in-depth-overview), puis créer et configurer une nouvelle application web à l’aide de l’interface Azure CLI. Nous allons ensuite utiliser Git pour déployer notre application en PHP sur Azure.

![hello-world-in-browser](media/app-service-web-get-started-php/hello-world-in-browser.png)

Vous pouvez suivre les étapes ci-dessous en utilisant un ordinateur Mac, Windows ou Linux. L’exécution des étapes ci-dessous ne devrait prendre que 5 minutes environ.

## <a name="prerequisites"></a>Composants requis

Avant de créer cet exemple, téléchargez et installez les composants suivants :

* [Git](https://git-scm.com/)
* [PHP](https://php.net)
* [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="download-the-sample"></a>Téléchargez l’exemple

Clonez le référentiel de l’exemple d’application Hello World sur votre ordinateur local.

```bash
git clone https://github.com/Azure-Samples/php-docs-hello-world
```

Passez au répertoire qui contient l’exemple de code.

```bash
cd php-docs-hello-world
```

## <a name="run-the-app-locally"></a>Exécutez l’application localement

Pour ce faire, ouvrez une fenêtre de terminal et utilisez la ligne de commande `php`, afin que l’exemple lance le serveur web PHP intégré.

```bash
php -S localhost:8080
```

Ouvrez un navigateur web et accédez à l’exemple.

```bash
http://localhost:8080
```

Vous pouvez voir sur la page le message **Hello World** de l’exemple d’application.

![localhost-hello-world-in-browser](media/app-service-web-get-started-php/localhost-hello-world-in-browser.png)

Dans la fenêtre de terminal, appuyez sur **Ctrl + C** pour quitter le serveur web.

## <a name="log-in-to-azure"></a>Connexion à Azure

Nous allons maintenant utiliser l’interface Azure CLI 2.0 dans une fenêtre de terminal pour créer les ressources nécessaires pour héberger notre application PHP dans Azure. Connectez-vous à votre abonnement Azure avec la commande [az login](/cli/azure/#login) et suivez les instructions à l’écran.

```azurecli
az login
```

<!-- ## Configure a Deployment User -->
[!INCLUDE [login-to-azure](../../includes/configure-deployment-user.md)]

## <a name="create-a-resource-group"></a>Créer un groupe de ressources

Créez un groupe de ressources avec la commande [az group create](/cli/azure/group#create). Un groupe de ressources Azure est un conteneur logique dans lequel les ressources Azure comme les applications web, les bases de données et les comptes de stockage sont déployées et gérées.

```azurecli
az group create --name myResourceGroup --location westeurope
```

## <a name="create-an-azure-app-service-plan"></a>Créer un plan Azure App Service

Créez un [plan App Service](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) « GRATUIT » avec la commande [az appservice plan create](/cli/azure/appservice/plan#create).

<!--
 An App Service plan represents the collection of physical resources used to ..
-->
[!INCLUDE [app-service-plan](../../includes/app-service-plan.md)]

L’exemple suivant crée un plan App Service nommé `quickStartPlan` en appliquant le niveau tarifaire **Gratuit**.

```azurecli
az appservice plan create --name quickStartPlan --resource-group myResourceGroup --sku FREE
```

Lorsque le plan App Service est créé, l’interface Azure CLI affiche des informations similaires à l’exemple suivant.

```json
{
    "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Web/serverfarms/quickStartPlan",
    "location": "West Europe",
    "sku": {
    "capacity": 1,
    "family": "S",
    "name": "S1",
    "tier": "Standard"
    },
    "status": "Ready",
    "type": "Microsoft.Web/serverfarms"
}
```

## <a name="create-a-web-app"></a>Créer une application web

Maintenant qu’un plan App Service est créé, générez une [application web](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) dans le plan App Service `quickStartPlan`. L’application web nous offre un espace d’hébergement pour déployer notre code, et fournit une URL pour nous permettre d’afficher l’application déployée. Utilisez la commande [az appservice web create](/cli/azure/appservice/web#create) pour créer l’application web.

Dans la commande ci-dessous, indiquez le nom unique de votre propre application là où se trouve `<app_name>` l’espace réservé. `<app_name>` est utilisé dans le site DNS par défaut de l’application web. Si `<app_name>` n’est pas une valeur unique, le message d’erreur « Un site Web nommé <app_name> existe déjà. » s’affiche.

<!-- removed per https://github.com/Microsoft/azure-docs-pr/issues/11878
You can later map any custom DNS entry to the web app before you expose it to your users.
-->

```azurecli
az appservice web create --name <app_name> --resource-group myResourceGroup --plan quickStartPlan
```

Une fois l’application web créée, Azure CLI affiche des informations similaires à celles de l’exemple suivant.

```json
{
    "clientAffinityEnabled": true,
    "defaultHostName": "<app_name>.azurewebsites.net",
    "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Web/sites/<app_name>",
    "isDefaultContainer": null,
    "kind": "app",
    "location": "West Europe",
    "name": "<app_name>",
    "repositorySiteName": "<app_name>",
    "reserved": true,
    "resourceGroup": "myResourceGroup",
    "serverFarmId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Web/serverfarms/quickStartPlan",
    "state": "Running",
    "type": "Microsoft.Web/sites",
}
```

Accédez au site pour voir votre nouvelle application web.

```bash
http://<app_name>.azurewebsites.net
```

![app-service-web-service-created](media/app-service-web-get-started-php/app-service-web-service-created.png)

Nous avons maintenant créé une nouvelle application web vide dans Azure.

## <a name="configure-local-git-deployment"></a>Configurer le déploiement Git local

Vous pouvez procéder au déploiement sur l’application web de plusieurs façons, notamment FTP, Git local ainsi que GitHub, Visual Studio Team Services et Bitbucket.

Utilisez la commande [az appservice web source-control config-local-git](/cli/azure/appservice/web/source-control#config-local-git) pour configurer l’accès Git local à l’application web.

```azurecli
az appservice web source-control config-local-git --name <app_name> --resource-group myResourceGroup --query url --output tsv
```

Copiez la sortie du terminal, car elle sera utilisée à l’étape suivante.

```bash
https://<username>@<app_name>.scm.azurewebsites.net:443/<app_name>.git
```

## <a name="push-to-azure-from-git"></a>Effectuer une transmission de type push vers Azure à partir de Git

Ajoutez un référentiel distant Azure dans votre référentiel Git local.

```bash
git remote add azure <paste-previous-command-output-here>
```

Effectuez une transmission de type push vers le référentiel distant Azure pour déployer votre application. Le mot de passe que vous avez fourni précédemment dans le cadre de la création de l’utilisateur du déploiement vous sera demandé. Veillez à saisir le mot de passe que vous avez indiqué dans [Configurer un utilisateur de déploiement](#configure-a-deployment-user), et non pas celui que vous utilisez pour vous connecter au portail Azure.

```bash
git push azure master
```

Au cours du déploiement, Azure App Service communiquera sa progression avec Git.

```bash
Counting objects: 2, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (2/2), done.
Writing objects: 100% (2/2), 352 bytes | 0 bytes/s, done.
Total 2 (delta 1), reused 0 (delta 0)
remote: Updating branch 'master'.
remote: Updating submodules.
remote: Preparing deployment for commit id '25f18051e9'.
remote: Generating deployment script.
remote: Running deployment command...
remote: Handling Basic Web Site deployment.
remote: Kudu sync from: '/home/site/repository' to: '/home/site/wwwroot'
remote: Copying file: '.gitignore'
remote: Copying file: 'LICENSE'
remote: Copying file: 'README.md'
remote: Copying file: 'index.php'
remote: Ignoring: .git
remote: Finished successfully.
remote: Running post deployment command(s)...
remote: Deployment successful.
To https://<app_name>.scm.azurewebsites.net/<app_name>.git
   cc39b1e..25f1805  master -> master
```

## <a name="browse-to-the-app"></a>Accéder à l’application

Accédez à l’application déployée à l’aide de votre navigateur web.

```bash
http://<app_name>.azurewebsites.net
```

Cette fois, la page qui affiche le message Hello World est en cours d’exécution à l’aide de notre code PHP, qui s’exécute en tant qu’application web Azure App Service.



## <a name="updating-and-deploying-the-code"></a>Mise à jour et déploiement du code

À l’aide d’un éditeur de texte local, ouvrez le fichier `index.php` dans l’application PHP et modifiez une petite partie du texte contenu dans la chaîne en regard de l’élément `echo` :

```php
echo "Hello Azure!";
```

Validez vos modifications dans Git, puis envoyez les modifications de code à Azure.

```bash
git commit -am "updated output"
git push azure master
```

Une fois le déploiement terminé, revenez à la fenêtre du navigateur ouverte à l’étape **Accéder à l’application**, puis cliquez sur Actualiser.

![hello-world-in-browser](media/app-service-web-get-started-php/hello-world-in-browser.png)

## <a name="manage-your-new-azure-web-app"></a>Gérer votre nouvelle application web Azure

Accédez au portail Azure pour voir l’application web que vous venez de créer.

Pour ce faire, connectez-vous au portail : [https://portal.azure.com](https://portal.azure.com).

Dans le menu de gauche, cliquez sur **App Services**, puis cliquez sur le nom de votre application web Azure.

![Navigation au sein du portail pour accéder à l’application web Azure](./media/app-service-web-get-started-php/php-docs-hello-world-app-service-list.png)

Vous accédez au _panneau_ de votre application web (une page du portail qui s’ouvre horizontalement).

Par défaut, le panneau de votre application web affiche la page **Présentation**. Cette page propose un aperçu de votre application. Ici, vous pouvez également effectuer des tâches de gestion de base (parcourir, arrêter, démarrer, redémarrer et supprimer des éléments, par exemple). Les onglets figurant sur le côté gauche du panneau affichent les différentes pages de configuration que vous pouvez ouvrir.

![Panneau App Service sur le portail Azure](media/app-service-web-get-started-php/php-docs-hello-world-app-service-detail.png)

Ces onglets affichent les nombreuses fonctionnalités exceptionnelles que vous pouvez ajouter à votre application web. La liste suivante fournit quelques exemples des possibilités :

* Mapper un nom DNS personnalisé
* Lier un certificat SSL personnalisé
* Configurer le déploiement continu
* Montée en puissance et augmentation de la taille des instances
* Ajouter une authentification utilisateur

**Félicitations !** Vous avez déployé votre première application en PHP dans App Service.

[!INCLUDE [cli-samples-clean-up](../../includes/cli-samples-clean-up.md)]

> [!div class="nextstepaction"]
> [Parcourez des exemples de scripts CLI d’applications web](app-service-cli-samples.md)


