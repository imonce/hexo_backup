---
title: '[Reading Notes] UniCrawl: A Practical Geographically Distributed Web Crawler'
date: 2016-12-11 00:13:19
tags: [Web Crawler, Unicrawl]
comments: true
---

## Abstract

Cause the wealth of information available on the web keeps growing, we want to use web crawler to get them. But the traditional method has a fatal limit of its large infrastructure cost. To reduce it, we developed this method, unicrawl, which can show a performance improvement of 93.6% in terms of network bandwidth consumption, and a speedup factor of 1.75.

## I. introduction

Nowadays, it's common that to use parallel process on a large number of machines to achieve a reasonable collection time. While this method requires large computing infrastructures. Like Google and Bing, who rely on big data centers. 
<!-- more-->
As for the public crawl repositories, they require externalizing computation and data hosting to a commercial cloud provider. Which may pose the problem of data availability in the mid-long term. And cause there are large amounts of data unnecessary, postprocessing is needed.

A solution to those problems is to distribute the crawling effort over several geographically distributed locations. For instance, by allowing several small companies to mutualize their crawling infrastructures. In addition, such an approach leverages data locality as sites can crawl web servers that are geographically nearby. But in this way, the synchronization between the crawler at the different sites is a new problem. Our goal is to reduce such communication costs.

UniCrawl is an efficient geo-distributed crawler that aims at minimizing inter-site communication costs. Our design is both practical and scalable. We assess this claim with a detailed evaluation of UniCrawl in a controlled environment using the ClueWeb12 dataset, as well as in geo-distributed setting using 3 distinct sites located in Germany.

### outline:
Section II is related work. Section III introduces the crawler architecture, refining it from existing well-founded central disigns. Section IV is the details about the internal implementaion. Section V presents the experimental results, both in-vitro, and in-vivo over multiple geographical locations in Germany. We discuss out results and future work in Section V. We conclude the paper inSection VII.

## II. Related work

There are several problems for every crawler to solve:

1. since the amount of information to parse is huge, a crawler must scale
2. a crawler should select which information to download first, and which information to refresh over time
3. a crawler should not be a burden for the web sites that host the content
4. adversaries, e.g., spider traps, need to be avoided with care

Mercator/Polybot/IBM WebFountain/Ubicrawl and etc..

## III. Distributed crawler architecture

### A. Single site Design

1. Map-reduce:
 - spill
 - shuffle
 - reduce
2. site storage
 - In UniCrawl, the crawl database of a site is implemented as a single distributed map structure. This map contains for each page its URL, content, and outlinks.
 - INFINISPAN, a distributed key-value store stat supports the following features: 
     - Routing: Notes are organized in a ring
     - Elasticity
     - Storage
     - Reliability
     - Interface
     - Consistency
     - Querying
3. Detail of Phases
 - Generate: The goal of the generate phase is to select a set of pages to process during the round.
 - Fetch: During the fetch phase, the map step first groups by host the pages that were generated in the previous phase.
 - Parse: Once the pages are fetched, they are analyzed during the parse phase.
 - Update: The goal of the update phase is to refresh the scores of pages that belong to the frontier in order to prioritize them.

### B. Multi-site Operations

Several key ideas allow UniCrawl to be practical in this setting:
1. Each site is independent and crawls the web autonomously
2. We unite all the site data stores
3. Sites exchanges dynamically the URLs they discover over the course of the crawl

1. Federating the storage: One of the key design concerns of UniCrawl is to bring small monifications to the site code base in order be usable over multiple geographical locations.
2. collaboration between sites: Following the approach advocated by Cho and Garcia-Molina. UniCrawl exchanges newly discovered URLs over time. This exchange occurs at the end of the update phase.
 - We implement the crawl database as a distributed ensemble map that span all the sites. This map operates in frontier mode with a replication factor of one.
3. Crawl quality and cost: The quality of the crawling operation is not only measured by means of pure web-graph exploration but also by the rounds it takes to discover the most interesting pages.

## IV. Implementation

We implemented UniCrawl inJava, starting from the code base of Nutch version 2.5.3.

Nutch makes use of Apache Gora, an open-source framework that provides an in-memory and persistent data model for big data.

Intotal, our contribution accounts for about a dozen thousands lines of code (LOC) split as follows: 9.4 kLOC for Ensemble, 1.1 kLOC for Gora and 2.3 kLOC patch for Nutch

### A. Merging phases

Cause each new map-reduce job creation is expensive as it requires to start a dedicated Java virtual machine, and deploy the appropriate jars. To lower this cost, we merge the fetch and parse phases in out UniCrawl implementation. This means that whenever a reducer fetches a new page, it parses its content and extract the out-links. These links are then directly inserted in the crawl database together with the fetched page.

### B. Caching

To avoid sending out an URL multiple times across sites, we use a distributed solution. In more details, this cache is a bounded ENSENMBLE map C local to each site and replicated at all nodes in a site. During the update phase, when a reducer selects a URL in the frontier that is associated to a remote site, it first check locally with C is this URL woa previously sent. If this is the case , the reducer simply skips the call to putIfAbsent. Since C is replicated at all nodes, every map-reduce node is co-located with an INFINISPAN node, and C is in memory, this inclusion test costs less than a millisecond.

## V. Evaluation

Evaluate UniCrawl through several key metrics such as the page processing rate, the memory usage and the network traffic across sites.

Two parts:
1. Evaluate our approach in-vitro, by running UniCrawl against the ClueWeb12 benchmark in an emulated multi-site architecture and crawling from a local repository.
2. Report several experimental results where we deploy UniCrawl at multiple localtions in Germany and access actual web sites.

### A. In-vitro validation

1. Single site performance
2. Emulating multiple sites

### B. UniCrawl in the wild

1. URL Exchange
2. Comparison with Nutch
3. Scalability

