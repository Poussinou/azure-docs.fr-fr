---
title: How to page through the available videos | Microsoft Docs
description: Shows how to page through all of the videos that Bing can return.
services: cognitive-services
author: swhite-msft
manager: ehansen
ms.assetid: 910A485F-BCF3-42B9-958D-DD48BDEDA965
ms.service: cognitive-services
ms.technology: bing-video-search
ms.topic: article
ms.date: 04/15/2017
ms.author: scottwhi
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: 903adb8b4d276a853a0f0197a4a7c2f1353d3521
ms.contentlocale: fr-fr
ms.lasthandoff: 05/10/2017

---

# <a name="paging-videos"></a>Paging videos

When you call the Video Search API, Bing returns a list of results. The list is a subset of the total number of results that are relevant to the query. To get the estimated total number of available results, access the answer object's [totalEstimatedMatches](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v5-reference#videos-totalestimatedmatches) field.  
  
The following example shows the `totalEstimatedMatches` field that a Video answer includes.  
  
```  
{
    "_type" : "Videos",
    "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=81EF7545D56...",
    "totalEstimatedMatches" : 1000,
    "value" : [...]
}  
```  
  
To page through the available videos, use the [count](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v5-reference#count) and [offset](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v5-reference#offset) query parameters.  
  
The `count` parameter specifies the number of results to return in the response. The maximum number of results that you may request in the response is 105. The default is 35. The actual number delivered may be less than requested.

The `offset` parameter specifies the number of results to skip. The `offset` is zero-based and should be less than (`totalEstimatedMatches` - `count`).  
  
If you want to display 20 videos per page, you would set `count` to 20 and `offset` to 0 to get the first page of results. For each subsequent page, you would increment `offset` by 20 (for example, 20, 40).  

The following shows an example that requests 20 videos beginning at offset 40.  
  
```  
GET https://api.cognitive.microsoft.com/bing/v5.0/videos/search?q=sailing+dinghies&count=20&offset=40&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
Host: api.cognitive.microsoft.com  
```  

> [!NOTE]
> V7 Preview request:

> ```  
> GET https://api.cognitive.microsoft.com/bing/v7.0/videos/search?q=sailing+dinghies&count=20&offset=40&mkt=en-us HTTP/1.1  
> Ocp-Apim-Subscription-Key: 123456789ABCDE  
> Host: api.cognitive.microsoft.com  
> ```  

If the default `count` value works for your implementation, you only need to specify the `offset` query parameter.  
  
```  
GET https://api.cognitive.microsoft.com/bing/v5.0/videos/search?q=sailing+dinghies&offset=40&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
Host: api.cognitive.microsoft.com  
```  

> [!NOTE]
> V7 Preview request:

> ```  
> GET https://api.cognitive.microsoft.com/bing/v7.0/videos/search?q=sailing+dinghies&offset=40&mkt=en-us HTTP/1.1  
> Ocp-Apim-Subscription-Key: 123456789ABCDE  
> Host: api.cognitive.microsoft.com  
> ```  

Typically, if you page 35 videos at a time, you would set the `offset` query parameter to 0 on your first request, and then increment `offset` by 35 on each subsequent request. However, some of the results in the subsequent response may be duplicates of the previous response. For example, the first two videos in the response may be the same as the last two videos from the previous response.

To eliminate duplicate results, use the [nextOffsetAddCount](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v5-reference#videos-nextoffset) field of the `Videos` object. The `nextOffsetAddCount` field tells you the number to add to your next `offset`.

For example, if you want to page 30 videos at a time, you'd set `count` to 30 and `offset` to 0 in your first request. In your next request, you'd set 'count' to 30 and `offset` to 30 plus the value of `nextOffsetAddCount`. The value of `nextOffsetAddCount` is zero (0) if there are no duplicates or it may be 2 if there are two duplicates.

> [!NOTE]
> V7 Preview changes to paging:
>
> Renamed the `nextOffsetAddCount` field of [Videos](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#videos) to `nextOffset`. In v7, you set the `offset` query parameter to the `nextOffset` value.


> [!NOTE]
> Paging applies only to videos search (/videos/search), and not to video insights (/videos/details) or trending videos (/videos/trending).
