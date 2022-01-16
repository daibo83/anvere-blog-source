---
title: To ElasticSearch, with love
date: 2022-01-16T16:09:33.172Z
description: An appreciation letter to ElasticSearch, the inspirer of our product, Anvere.
---
I would like to dedicate my most sincerely appreciation to **ElasticSearch**, the main drive that made me started **ANVERE**.

![](/uploads/despicable-me-minions.gif)

It all began in 2016, when I was tasked with building an autocomplete API for geographical data at [Goong](https://www.goong.io/). The search engine that was employed at that time was based on SphinxSearch, took forever to return subpar results - underperforming in all aspects. This made me so frustrated that I decided to migrate to a completely new search engine instead of trying to fix it.

Enter **ElasticSearch**.

![](/uploads/es.png)

**What makes ElasticSearch appealing to us - and many more like us**

[ElasticSearch](https://www.elastic.co/) (ES) was (and still is) one of the most popular engines for information retrieval. It is built on Lucene library and enjoys wide adoption in the full-text search domain. Major factors that convinced us to employ ES include:

* Extensive documentation: Despite of what people say, I've always managed to find what I was looking for
* Well populated and helpful community: Almost 2000 opensource contributors. And most of the time, a simple Google search will surface the solution for whichever problem you might encounter
* A neat spatial index for location-based search: Perfect for our use-case, right?

 

After lots and lots of researches and lengthy discussions, my team decided to start our journey with ElasticSearch.

![](/uploads/image2.gif)

**How did it work out for us?**

A quick test, creating an index and importing a small subset of our address and POI data, showed promising results: ES automatically infers inferred the document schema, some simple queries executed perfectly, even geospatial ones. “This is gonna be a walk in the park”, so I thought. Or so I thought?

An indexing pipeline to ingest data from the PostgreSQL server that hosts our addresses and POIs data was quickly set up. With the entirety of the dataset indexed, the first difficulties arose. Just to name a few:

* Results containing less input words would often rank higher than those with more: For search input “John William Doe”, items containing only “John Doe” and “William Doe” would come out on top of “John William Doe”.
* The engine took into account neither the order nor the proximity in which the keywords were entered: A search for New York would frequently return the result “Best new restaurant in York” way before “New York City”.
* Typo-tolerant and word-prefix queries were painfully slow.

  * “Ho Chi Minh City” took 50 ms to complete.
  * “Ho Chi Mihn City” however required more than 500 ms to process.

It turned out our vanilla experience of ES leaves a lot to be desired. Fortunately, we had the ES’s excellent documentation and other community resources of other communities to rely on.

So I eagerly started my journey in exploring the vast knowledge library of ES. And, oh boy, there was a lot to learn: Tokenizers, analyzers, filters, shingles, ngrams, similarities,…

![](/uploads/image5.gif)

(My condolence if you have gone through what I had).

Over months and months of struggle, we gradually ironed out some of the kinks. Prioritizing documents with query words in close proximity was solved by shingle tokenization at index and search time, fast prefix processing was handled by using edge-ngram tokenizer etc. Using a custom plugin sped up typo correction by two orders of magnitude. We even had to implement our own scoring plugin in order to give documents with query words appearing at the start higher relevance.

Some issues, though, remained unsolved despite ES’ plethora of possible settings and huge toolset. I resorted to using other Information Retrieval and NLP algorithms. Vietnamese tokenization for instance had to be manually implemented.

It took us **12 months** to ship the Autocomplete API and **another 6 months** of post-launch improvement to attain adequate quality. We were all overjoyed when the product was approved and operated smoothly.

Nevertheless, at that point, we decided that we are done with ES. We were tired of the whole procedure, detested writing queries in ES’ proprietary query language DSL, and abhorred the work necessary to scale ES, to reduce 99th percentile response times by Java VM tuning.

![](/uploads/image6.gif)

ES was NEVER meant to be the drop-in solution for full-text search-as-you-type! It was so conceived to solve almost all problems in Information Retrieval, that in its vanilla form, it solves none of those problems. It requires a great deal of effort and willingness to climb a steep learning curve to get it to behave according to your needs. To such an extent that, ES and Solr, another Lucene-base opensource product, spawned an entire market for consulting businesses that specialize on helping companies ship and scale search experiences. Had we had the funds, we certainly would have hired a consultant.

![](/uploads/image3.gif)

**The start of something new**

By then, it had finally dawned on me that, in order to build a relevant full text search, you don’t learn ElasticSearch, or Solr for that matter. You learn Search. Most of the grasp on Search that helped us in improving our customers' searching experience was never specific to ES or Solr but attained by diving deep into the vast Information Retrieval knowledge base.

So yes, thank you very much, ElasticSearch! Without all the bumps you put me through, I would not have started Anvere, for you have introduced me to an itch that has to be scratched :)

From Anvere with love