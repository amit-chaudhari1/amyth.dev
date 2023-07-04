---
title: "Factorio & the basics of Concurrency: Bottlenecks"
date: "2022-09-17T22:05:05+05:30"
# weight: 1
# aliases: ["/first"]
tags: ["draft"]
author: "Amit Chaudhari"
categories: ["Concurrency"]
series: ["Factorio & the basics of Concurrency"]
# author: ["Me", "You"] # multiple authors
showToc: true
TocOpen: false
draft: false
hidemeta: false
comments: false
# description: "Desc Text."
canonicalURL: "https://canonical.url/to/page"
disableHLJS: true # to disable highlightjs
disableShare: false
disableHLJS: false
hideSummary: false
searchHidden: false
ShowReadingTime: true
ShowBreadCrumbs: true
ShowPostNavLinks: true
ShowWordCount: true
ShowRssButtonInSectionTermList: true
UseHugoToc: true
cover:
    image: "<image path/url>" # image path/url
    alt: "<alt text>" # alt text
    caption: "<text>" # display caption under cover
    relative: false # when using page bundles set this to true
    hidden: true # only hide on current single page
editPost:
    URL: "https://github.com/amit-chaudhari1/amyth.dev/labels/"
    Text: "Suggest Changes" # edit text
    appendFilePath: true # to append file path to Edit link
---



> In many ways. In Factorio, your factory can have concurrency, parallelism, race conditions, bottlenecks, deadlocks, resource starvation, etc, etc.
> Looking for those types of problems and solving them is a very fun feedback loop.
> You can spend a few hours just walking around a large factory, making small incremental improvements as you go, and your entire factory will be visibly better off.
> The only thing preventing you from doing this to your application is the lack of visibility into the problems.
> There's a bottleneck in your code right now, but can you find it? They're almost never detectable by simple inspection of the source code, so it takes specialized tools.
> I would say then that building a large factory is like an optimal form of programming where nothing is opaque or hidden from you.

\-  [https://news.ycombinator.com/item?id=24157150](https://news.ycombinator.com/item?id=24157150)
 
