---
title: Azure Custom Decision Service overview | Microsoft Docs
description: This article overviews Azure Custom Decision Service, a cloud-based API for contextual decision-making that sharpens with experience.
services: cognitive-services
author: alekh
manager: slivkins
ms.service: cognitive-services
ms.topic: article
ms.date: 05/03/2017
ms.author: alekha;slivkins
ms.translationtype: Human Translation
ms.sourcegitcommit: a30a90682948b657fb31dd14101172282988cbf0
ms.openlocfilehash: c404362d98509517f41bd1cb4f01f6e13d2d19af
ms.contentlocale: fr-fr
ms.lasthandoff: 05/25/2017

---

# <a name="custom-decision-service"></a>Custom Decision Service

Azure Custom Decision Service helps you create intelligent systems with a cloud-based contextual decision-making API that sharpens with experience. Custom Decision Service harnesses the power of reinforcement learning and adapts the content in your application to maximize the overall engagement of users. The system incorporates user feedback into its decisions in real time and responds to emergent trends and breaking stories in minutes.

In a typical application, a front page links to several articles or other types of content. As the front page loads, it requests Custom Decision Service to provide a ranking of articles to include on the page. When you click an article, a second request is sent to Custom Decision Service to log the outcome of the decision.

Custom Decision Service is easy to use. The easiest integration mode requires only an RSS feed for your content and a few lines of JavaScript to be added into your application.

Custom Decision Service converts your content into features for machine learning. The system uses these features to understand your content in terms of its text, images, videos, and overall sentiment. We leverage several other [Microsoft Cognitive Services](https://www.microsoft.com/cognitive-services), such as [Entity Linking](../entitylinking/home.md), [Text Analytics](../Text-Analytics/overview.md), [Emotion](../emotion/home.md), and [Computer Vision](../computer-vision/home.md).

Some common-use cases for Custom Decision Service include:

* Personalizing articles on a news website.
* Personalizing video content on a media portal.
* Optimizing ad placements or web pages that the ad directs to.
* Ranking recommended items on a shopping website.

We are currently in *free public preview*, focused on personalizing a list of articles on a website or an app. Feature extraction works best for English language content. [Limited functionality](../Text-Analytics/overview.md) is offered for some other languages, such as Spanish, French, German, Portuguese, and Japanese. This documentation is revised as we are ready to advertise more functionality.

Custom Decision Service can be applied to applications that are not in the content personalization domain. Such applications might be a good fit for a custom preview. Contact us to learn more.

## <a name="learning-mode-pooled-or-application-specific"></a>Learning mode: pooled or application specific

Custom Decision Service can be used in two learning modes. The APIs are identical, regardless of which learning mode you use:

- Pooled learning mode uses one model for all applications and is suitable for low-traffic applications.
- Application-specific learning mode is suitable for high-traffic applications.

### <a name="pooled-learning-mode"></a>Pooled learning mode

Custom Decision Service learns from the click patterns of users in response to the content presented in your application. Learning can be slow if your application has relatively low traffic. This problem is pronounced for dynamic content, such as news. Such applications might not have enough time to learn the quality of an article and apply this learning before new content arrives.

For *low-traffic applications with dynamic content*, we recommend pooling data across multiple applications. By using the pooled data, we learn a single model for all applications that sign up for this learning mode. Then we use this model to customize their content. For example, you might promote a breaking news story if users on other websites were interested in it, even before anyone read it on your website. User privacy is respected because the raw data is never shared with any individual application. Only the decisions made by the system are shared.

### <a name="application-specific-learning-mode"></a>Application-specific learning mode

When your user-traffic volume permits, we recommend the application-specific learning mode. Then Custom Decision Service learns a model based only on how *your* users interact with *your* content. This type of model performs better than one learned in the pooled mode because other applications' users and content might be different from yours.

Custom Decision Service creates a deployment of the entire learning pipeline for your application. You can also access the collected data offline to derive further insights about user preferences.

To use this learning mode, you need to have an [Azure storage account](../../storage/storage-create-storage-account.md) where your data is logged. When you register a new application on the portal, choose **Advanced Options**. Then enter the [connection string](../../storage/storage-configure-connection-string.md) for the storage account.

## <a name="api-usage-modes"></a>API usage modes

Custom Decision Service can be applied to both webpages and apps. The APIs can be called from either a browser or an app. The API usage is similar on both modes, but some of the details are different.

## <a name="glossary-of-terms"></a>Glossary of terms

Several terms frequently occur in our documentation:

* **Action set**: The set of content items for Custom Decision Service to rank. This set can be specified as an *RSS* or *Atom* endpoint.
* **Ranking**: Each request to Custom Decision Service specifies one or more action sets. The system responds by picking all the content options from these sets and returns them in ranked order.
* **Callback function**: This function, which you specify, renders the content in your UI. The content is ordered by the rank ordering returned by Custom Decision Service.
* **Reward**: A measure of how the user responded to the rendered content. Custom Decision Service measures user response by using clicks. The clicks are reported to the system by using custom code inserted in your application.

## <a name="next-steps"></a>Next steps

* Get started with Custom Decision Service to optimize [a webpage](custom-decision-service-get-started-browser.md) or [a smartphone app](custom-decision-service-get-started-app.md).
* Consult the [API reference](custom-decision-service-api-reference.md) to learn more about the provided functionality.
