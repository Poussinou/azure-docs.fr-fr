---
title: "Vue d’ensemble d’Azure Cloud Shell (version préliminaire) | Microsoft Docs"
description: "Vue d’ensemble d’Azure Cloud Shell."
services: 
documentationcenter: 
author: jluk
manager: timlt
tags: azure-resource-manager
ms.assetid: 
ms.service: 
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.author: juluk
ms.translationtype: Human Translation
ms.sourcegitcommit: e7da3c6d4cfad588e8cc6850143112989ff3e481
ms.openlocfilehash: 63f1c468b5f8f4b0bb298cb67adea8c01b065427
ms.contentlocale: fr-fr
ms.lasthandoff: 05/16/2017

---
# <a name="overview-of-azure-cloud-shell-preview"></a>Vue d’ensemble d’Azure Cloud Shell (version préliminaire)
Azure Cloud Shell est un shell interactif, accessible par navigateur pour la gestion des ressources Azure.

![](media/startup.gif)

## <a name="features"></a>Caractéristiques
### <a name="browser-based-shell-experience"></a>Expérience shell basée sur navigateur
Cloud Shell permet d’accéder à une expérience de ligne de commande basée sur navigateur avec les tâches de gestion Azure à l’esprit. Exploitez Cloud Shell pour travailler librement à partir d’une machine locale d’une façon que seul le cloud peut fournir.

### <a name="pre-configured-azure-workstation"></a>Station de travail Azure préconfigurée
Cloud Shell est préinstallé avec des outils de ligne de commande et la prise en charge de langages populaires afin de pouvoir travailler plus rapidement.

[Consultez la liste complète des outils pour Azure Cloud Shell ici.](features.md#tools)

### <a name="automatic-authentication"></a>Authentification automatique
Cloud Shell s’authentifie automatiquement de façon sécurisée sur chaque session pour accéder immédiatement à vos ressources via Azure CLI 2.0.

### <a name="connect-your-azure-file-storage"></a>Connexion à votre stockage de fichiers Azure
Les machines Cloud Shell sont temporaires et nécessitent ainsi qu’un partage de fichiers Azure soit monté en tant que `clouddrive` pour conserver votre répertoire $Home.
Lors du premier lancement, Cloud Shell vous invite à créer un groupe de ressources, un compte de stockage et un partage de fichiers en votre nom. Il s’agit d’une étape unique, et ces ressources sont automatiquement jointes pour toutes les sessions. 

![](media/storage-prompt.png)

Un compte de stockage LRS est créé en votre nom avec un partage de fichiers Azure contenant une image de disque de 5 Go par défaut. Le partage de fichiers se monte en tant que `clouddrive` pour l’interaction du partage de fichiers avec l’image de disque utilisée pour synchroniser et conserver votre répertoire $Home. Les coûts de stockage standard s’appliquent.

Trois ressources sont créées en votre nom :
1. Groupe de ressources nommé : `cloud-shell-storage-<region>`
2. Compte de stockage nommé : `cs-uniqueGuid`
3. Partage de fichiers nommé : `cs-<user>-<domain>-com-uniqueGuid`

> [!Note]
> Tous les fichiers figurant dans votre répertoire $Home, tels que les clés SSH, sont conservés dans l’image de disque utilisateur stockée dans votre partage de fichiers monté. Appliquez les meilleures pratiques lors de l’enregistrement des fichiers dans votre répertoire $Home et le partage de fichiers monté.

[Découvrez le stockage Cloud Shell, la mise à jour des partages de fichiers et le chargement/téléchargement de fichiers.] (persistance-shell-storage.md).

## <a name="concepts"></a>Concepts
* Cloud Shell s’exécute sur une machine temporaire fournie par session et par utilisateur
* Cloud Shell expire après 10 minutes sans activité interactive
* Cloud Shell est accessible uniquement avec un partage de fichiers attaché
* Cloud Shell est affecté à une machine par compte d’utilisateur
* Les autorisations sont définies en tant qu’utilisateur Linux standard

[Découvrez-en davantage sur toutes les fonctionnalités de Cloud Shell.](features.md)

## <a name="examples"></a>Exemples
* Créer ou modifier des scripts pour gérer des ressources Azure à partir de n’importe quel navigateur
* Gérer simultanément des ressources via le portail Azure et Azure CLI 2.0
* Essayer Azure CLI 2.0

[Essayez tous ces exemples sur le démarrage rapide de Cloud Shell.](quickstart.md)

## <a name="pricing"></a>Tarification
La machine qui héberge Cloud Shell est gratuite, avec comme condition préalable le montage d’un partage de fichiers Azure pour conserver votre répertoire $Home. Les coûts de stockage standard s’appliquent.

## <a name="supported-browsers"></a>Navigateurs pris en charge
Cloud Shell est recommandé pour Chrome, Safari et Edge. Si Cloud Shell est pris en charge par Chrome, Firefox, Safari, IE et Edge, il est soumis aux paramètres propres au navigateur.
Pour connaître les limitations spécifiques connues, consultez les [limitations de Cloud Shell](limitations.md).
