---
title: "Démarrage rapide du cluster Kubernetes dans Azure | Microsoft Docs"
description: "Déploiement et prise en main d’un cluster Kubernetes dans Azure Container Service"
services: container-service
documentationcenter: 
author: anhowe
manager: timlt
editor: 
tags: acs, azure-container-service, kubernetes
keywords: 
ms.assetid: 8da267e8-2aeb-4c24-9a7a-65bdca3a82d6
ms.service: container-service
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/08/2017
ms.author: anhowe
ms.custom: H1Hack27Feb2017
ms.translationtype: Human Translation
ms.sourcegitcommit: a643f139be40b9b11f865d528622bafbe7dec939
ms.openlocfilehash: 0604a85192ed632b621113b98cc44172c584ea01
ms.contentlocale: fr-fr
ms.lasthandoff: 05/31/2017

---

# <a name="get-started-with-a-kubernetes-cluster-in-container-service"></a>Prise en main d’un cluster Kubernetes dans Container Service


Cette procédure pas à pas vous montre comment utiliser les commandes Azure CLI 2.0 pour créer un cluster Kubernetes dans Azure Container Service. Vous pouvez ensuite utiliser l’outil de ligne de commande `kubectl` pour commencer à travailler avec des conteneurs dans le cluster.

L’image suivante représente l’architecture d’un cluster Azure Container Service comportant un maître Linux et deux agents Linux. Le nœud maître sert l’API REST de Kubernetes. Les nœuds d’agent sont rassemblés dans un groupe à haute disponibilité Azure et exécutent vos conteneurs. Toutes les machines virtuelles figurent dans le même réseau privé virtuel et sont entièrement accessibles les unes par rapport aux autres.

![Image du cluster Kubernetes sur Azure](media/container-service-kubernetes-walkthrough/kubernetes.png)

Pour plus d’informations, consultez la [présentation d’Azure Container Service](container-service-intro.md) et la [documentation de Kubernetes](https://kubernetes.io/docs/home/).

## <a name="prerequisites"></a>Composants requis
Pour créer un cluster Azure Container Service à l’aide d’Azure CLI 2.0, vous devez :
* disposer d’un compte Azure ([obtenir une version d’évaluation gratuite](https://azure.microsoft.com/pricing/free-trial/))
* avoir installé et configuré [Azure CLI 2.0](/cli/azure/install-az-cli2)

En outre, il vous faut les éléments suivants (ou vous pouvez utiliser l’interface de ligne de commande Azure pour les générer automatiquement pendant le déploiement de cluster) :

* **Clé publique RSA SSH** : pour créer à l’avance des clés RSA SSH (Secure Shell), consultez les conseils sur [macOS et Linux](../virtual-machines/linux/mac-create-ssh-keys.md) ou [Windows](../virtual-machines/linux/ssh-from-windows.md). 

* **Secret et ID client du principal de service** : pour plus d’informations sur la création d’un principal de service Azure Active Directory, consultez [À propos du principal de service d’un cluster Kubernetes](container-service-kubernetes-service-principal.md).

 Dans cet article, l’exemple de commande génère automatiquement les clés SSH et un principal de service.

## <a name="create-your-kubernetes-cluster"></a>Créer votre cluster Kubernetes

Voici quelques brèves commandes de l’interpréteur de commandes Bash utilisant Azure CLI 2.0 pour créer votre cluster. 

### <a name="create-a-resource-group"></a>Créer un groupe de ressources
Pour créer votre cluster, créez d’abord un groupe de ressources dans un emplacement où Azure Container Service est [disponible](https://azure.microsoft.com/regions/services/). Exécutez des commandes similaires à la suivante :

```azurecli
RESOURCE_GROUP=my-resource-group
LOCATION=westus
az group create --name=$RESOURCE_GROUP --location=$LOCATION
```

### <a name="create-a-cluster"></a>Créer un cluster
Créez un cluster Kubernetes dans votre groupe de ressources à l’aide de la commande `az acs create` avec `--orchestrator-type=kubernetes`. Pour la syntaxe de commande, consultez l’[aide](/cli/azure/acs#create) de `az acs create`.

Cette version de la commande génère automatiquement les clés RSA SSH et le principal de service du cluster Kubernetes.



```azurecli
DNS_PREFIX=some-unique-value
CLUSTER_NAME=any-acs-cluster-name
az acs create --orchestrator-type=kubernetes --resource-group $RESOURCE_GROUP --name=$CLUSTER_NAME --dns-prefix=$DNS_PREFIX --generate-ssh-keys
```

Après quelques minutes, la commande termine son exécution. Vous devez alors disposer d’un cluster Kubernetes opérationnel.

> [!IMPORTANT]
> Si votre compte ne dispose pas des autorisations pour créer un principal de service Azure AD, la commande génère une erreur semblable à **Privilèges insuffisants pour effectuer l’opération**. Pour en savoir plus, consultez [À propos du principal du service pour un cluster Kubernetes](container-service-kubernetes-service-principal.md) (en anglais).
> 



### <a name="connect-to-the-cluster"></a>Connexion au cluster

Pour vous connecter au cluster Kubernetes depuis l’ordinateur client, vous utilisez l’outil [`kubectl`](https://kubernetes.io/docs/user-guide/kubectl/), le client de ligne de commande Kubernetes. 

Si vous n’avez pas encore installé `kubectl`, vous pouvez le faire à l’aide de `az acs kubernetes install-cli`. (Vous pouvez également le télécharger à partir du [site Kubernetes](https://kubernetes.io/docs/tasks/kubectl/install/).)

```azurecli
sudo az acs kubernetes install-cli
```

> [!TIP]
> Par défaut, cette commande installe le binaire de l’outil `kubectl` dans le répertoire `/usr/local/bin/kubectl` sur un système Linux ou macOS, ou dans `C:\Program Files (x86)\kubectl.exe` sous Windows. Pour spécifier un autre chemin d’installation, utilisez le paramètre `--install-location`.
>
> Une fois `kubectl` installé, vérifiez que le répertoire correspondant figure dans le chemin d’accès système. Le cas échéant, ajoutez-le. 
>


Ensuite, exécutez la commande suivante pour télécharger la configuration principale du cluster Kubernetes dans le fichier `~/.kube/config` :

```azurecli
az acs kubernetes get-credentials --resource-group=$RESOURCE_GROUP --name=$CLUSTER_NAME
```

Pour accéder à d’autres options d’installation et de configuration de `kubectl`, voir [Connexion à un cluster Azure Container Service](container-service-connect.md).

À ce stade, vous devriez pouvoir accéder à votre cluster à partir de votre machine. Essayez d’exécuter :

```bash
kubectl get nodes
```

Puis vérifiez que vous pouvez voir les machines dans votre cluster.

## <a name="create-your-first-kubernetes-service"></a>Création de votre premier service Kubernetes

Une fois cette procédure terminée, vous saurez comment :
* déployer une application Docker et l’exposer au public ;
* utiliser `kubectl exec` pour exécuter des commandes dans un conteneur ; 
* accéder au tableau de bord Kubernetes.

### <a name="start-a-container"></a>Démarrer un conteneur
Vous pouvez exécuter un conteneur (dans le cas présent, le serveur web Nginx) en exécutant :

```bash
kubectl run nginx --image nginx
```

Cette commande démarre le conteneur Docker Nginx dans un bloc sur l’un des nœuds.

Pour voir le conteneur en cours d’exécution, exécutez :

```bash
kubectl get pods
```

### <a name="expose-the-service-to-the-world"></a>Rendre le service accessible au public
Pour exposer le service au public, créez un Kubernetes `Service` de type `LoadBalancer` :

```bash
kubectl expose deployments nginx --port=80 --type=LoadBalancer
```

Suite à cette commande, Kubernetes crée une règle d’équilibreur de charge Azure avec une adresse IP publique. La propagation de cette modification de l’équilibreur de charge dure quelques minutes. Pour en savoir plus, consultez la rubrique [Équilibrer la charge des conteneurs dans un cluster Kubernetes dans Azure Container Service](container-service-kubernetes-load-balancing.md).

Exécutez la commande suivante et vous verrez le statut du service passer à `pending` pour afficher une adresse IP externe :

```bash
watch 'kubectl get svc'
```

  ![Image de la transition du statut En attente vers Adresse IP externe](media/container-service-kubernetes-walkthrough/kubernetes-nginx3.png)

Une fois que vous voyez l’adresse IP externe, vous pouvez y accéder dans votre navigateur :

  ![Image de la navigation vers Nginx](media/container-service-kubernetes-walkthrough/kubernetes-nginx4.png)  


### <a name="browse-the-kubernetes-ui"></a>Parcourir l’interface utilisateur Kubernetes
Pour afficher l’interface web de Kubernetes, vous pouvez utiliser :

```bash
kubectl proxy
```
Cette commande exécute un proxy authentifié sur localhost, ce qui vous permet d’afficher l’interface utilisateur web Kubernetes s’exécutant sur [http://localhost:8001/ui](http://localhost:8001/ui). Pour en savoir plus, consultez la rubrique [Utilisation de l’interface utilisateur Web Kubernetes avec Azure Container Service](container-service-kubernetes-ui.md).

![Image du tableau de bord Kubernetes](media/container-service-kubernetes-walkthrough/kubernetes-dashboard.png)

### <a name="remote-sessions-inside-your-containers"></a>Sessions à distance à l’intérieur de vos conteneurs
Kubernetes vous permet d’exécuter des commandes dans un conteneur Docker à distance qui est en cours d’exécution dans votre cluster.

```bash
# Get the name of your nginx pods
kubectl get pods
```

À l’aide de votre nom de bloc, vous pouvez exécuter une commande à distance sur votre bloc. Par exemple :

```bash
kubectl exec <pod name> date
```

Vous pouvez également obtenir une session entièrement interactive à l’aide des indicateurs `-it` :

```bash
kubectl exec <pod name> -it bash
```

![Session à distance à l’intérieur d’un conteneur](media/container-service-kubernetes-walkthrough/kubernetes-remote.png)



## <a name="next-steps"></a>Étapes suivantes

Pour en faire plus avec votre cluster Kubernetes, consultez les ressources suivantes :

* [Camp d’entraînement Kubernetes](https://katacoda.com/embed/kubernetes-bootcamp/1/) : montre comment déployer, mettre à l’échelle, mettre à jour et déboguer des applications.
* [Guide d’utilisateur Kubernetes](http://kubernetes.io/docs/user-guide/) : fournit des informations sur les programmes en cours d’exécution dans un cluster Kubernetes existant.
* [Exemples de Kubernetes](https://github.com/kubernetes/kubernetes/tree/master/examples) : fournit des exemples d’exécution d’applications réelles avec Kubernetes.

