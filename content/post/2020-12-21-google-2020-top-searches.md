---
title: "Top Google Searches in 2020"
date: 2020-12-21T16:07:11+11:00
slug: "google-2020-top-searches"
description: ""
draft: false
tags: ["R", "Visualization"]
math: false
showToc: true
---

<img class="special-img-class" style="width:100%" src="/image/top-searches-2020.png" />

<!-- ![2020-ridgeplot](/image/top-searches-2020.png) -->

So the motive was to recreate a ridgeplot summarizing some keywords of year 2020. One of my long time subscribed RSS feed [rud.is](https://rud.is/b/2019/12/27/short-attention-span-theatre-reproducing-axios-1-big-thing-google-trends-2019-news-in-review-with-ggplot2/) happened to have one of such tutorial post back from last year.

In that post, [hrbrmstr](https://rud.is/b/author/hrbrmstr/) was in fact, recreating [another post](https://www.axios.com/insane-news-cycle-attention-2019-google-interest-f89e7bc8-399d-4681-b479-00e8b6a4ccbc.html) by Axios.

Can't wait to do it myself with fresh new data in 2020. Well, it's not the end of year 2020 technically, but that will do as well.

Everything was fine until the last step, where a plot should be rendered. But instead, I had the infamous "polygon edge not found" error which somehow is related with disabled Arial font:

```
Error in grid.Call(C_textBounds, as.graphicsAnnot(x$label), x$x, x$y,  :
polygon edge not found
In addition: Warning message:
In grid.Call(C_textBounds, as.graphicsAnnot(x$label), x$x, x$y,  :
no font could be found for family "EconSansCndReg"
```

A quick fix for this is to use default theme in `{hrbrthemes}` package, replace `theme_ipsum_es()` with `theme_ipsum()`.

Additionally, the original code:

```r
theme(strip.text.y = element_text(angle = 180, hjust = 1, vjust = 0))
```

does not effectively rotate the facet group name here. Here's what I do:

```r
theme(strip.text.y.left = element_text(angle = 0, hjust = 1, vjust = 0, size=rel(0.618)))
```

***

> Interest over time: Numbers represent search interest relative to the highest point on the chart for the given region and time. A value of 100 is the peak popularity for the term. A value of 50 means that the term is half as popular. A score of 0 means that there was not enough data for this term.