---
title: KB versus Search — which one to use?
parent: Articles
nav_order: 8
description: "Choose between lightweight knowledge organisation and semantic retrieval, or combine both for your work with Atlas."
---

# KB versus Search — which one to use?

You want to make knowledge available to yourself, your colleagues or an AI agent. Atlas offers two complementary tools: Knowledge base (KB) and Search. Both help you find information, but they approach the problem differently.

**KB helps you locate and navigate structured knowledge. Search helps you retrieve relevant information from a body of knowledge, even when you do not know where to look.**

Both tools are available to compatible agents through Atlas rather than being tied to a specific model's built-in memory. A knowledge base or Search collection can support work across different AI clients and subscriptions, subject to your access and each client's supported tools. Switching agents does not require rebuilding your knowledge collection.

## Knowledge base: structure knowledge as you work

A knowledge base is a collection of readable Markdown articles following an extended Open Knowledge Format (OKF). Articles can carry tags, dates, locations and explicit relationships. Atlas uses these to provide a file navigator, filters, timelines, graphs and other views.

A KB can live in your Atlas workspace or a Git repository. You can also explore a workspace folder as a knowledge graph without turning it into a formal KB. People and agents can read and update the underlying files directly; no embedding model or separate graph database is required.

Imagine maintaining a project's architectural decisions. An agent can locate an article by its title or tags, read it, follow links to related decisions and update it when requirements change. A historical timeline or a campaign encyclopedia works much the same way.

KB is particularly useful when:

- You are creating and revising knowledge regularly.
- You can organise the material into meaningful articles, folders and relationships.
- You or your agent can narrow down the relevant information using titles, metadata and links.
- You want knowledge that is easy to maintain, version and move.

KB is a lightweight tool for everyday knowledge work. Its practical scale is typically hundreds to thousands of articles, although the amount and organisation of the content matter more than the article count alone. Navigating an increasingly large or poorly structured collection may require more time and agent context.

## Search: retrieve information from large collections

Imagine a historical library containing 700 PDF books, or a collection of thousands of role-playing supplements. You want to know what the sources say about medieval town taxation, but do not know which books or chapters contain the answer.

Reading every likely file would be expensive and slow. Atlas Search indexes the collection in advance, so you can ask a question and retrieve relevant passages without navigating and rereading the entire library.

Search combines lexical matching, semantic similarity, metadata and graph relationships. These signals help it find and rank candidates, including material that uses different wording from your query. You can refine the results and follow source references to inspect the evidence. Its graph is primarily used to improve retrieval, rather than as an article-by-article navigation interface like KB.

This capability comes at a cost. Search needs a configured database (currently Neo4j), compatible embedding inference for semantic retrieval, and an indexing process that reads, structures and encodes your material. Changes may require refreshing the index or regenerating embeddings. Results depend on the coverage and freshness of the index.

Search is particularly useful when:

- Your collection is large, heterogeneous or contains lengthy documents.
- You do not know in advance which source holds the relevant information.
- You need to find related concepts rather than just matching keywords.
- You expect to query the same collection repeatedly, making the indexing investment worthwhile.

## Choose the right tool

| | Knowledge base | Search |
| --- | --- | --- |
| Main operation | Locate and navigate | Retrieve and rank |
| Best suited to | Structured, frequently edited knowledge | Large or difficult-to-navigate collections |
| Organisation | Articles, folders, metadata and explicit links | Indexed documents, sections, passages and associations |
| Finding information | Titles, fields, filters and relationships | Hybrid semantic retrieval |
| Updates | Edit articles and refresh lightweight field indexes | Refresh indexed content and, where needed, embeddings |
| Infrastructure | Existing Atlas workspace or Git storage | Database, inference and indexing |
| Setup and maintenance | Relatively low | Higher |

There is no fixed size at which you should switch from KB to Search. A well-organised collection of 3,000 short timeline articles may be easy to navigate, while 50 lengthy research reports may already justify semantic indexing.

The question is whether the effort of locating information repeatedly outweighs the cost of building and maintaining an index.

## Both can be used as agent memory

Agents need to retain information beyond a single conversation: decisions, observations, research findings and context for future work.

For a project with frequently updated notes and a predictable structure, a KB is often the simplest choice. An agent can maintain an article on each topic, locate it through metadata and update it as work evolves. There is little additional infrastructure or processing cost.

Search can also record knowledge directly, including findings from conversations. Its advantage becomes more apparent when accumulated memory is large and the agent needs associative retrieval: finding related observations without remembering exactly where or when they were recorded.

For rapidly changing working memory, maintaining embeddings and graph indexes may be disproportionate to the benefit. For a substantial accumulated research corpus, the same investment may be worthwhile.

Choose based on how often the memory changes, how much information it contains and how the agent needs to access it, rather than its age alone.

## Combine them: index a knowledge base

Sometimes you want both the navigability of KB and the semantic retrieval capabilities of Search.

For example, a team maintains a knowledge base of technology assessments, architecture decisions and lessons learned. People edit the articles and browse their relationships in KB. As the collection grows, they may also want to ask questions across the whole body of knowledge without knowing the relevant article titles.

Indexing the KB's files in Search provides an additional way to access that material. The articles remain the source of truth; the Search index is a retrieval representation of them. Continue editing the articles in KB and refresh the Search index as needed.

This approach retains the benefits of both tools, but also introduces the cost of indexing, keeping results current and managing access to the indexed material. Do not index a KB merely because you can. Add Search when navigating the existing knowledge is no longer sufficient for your needs.

## Getting started

Start with [Knowledge base]({{ site.baseurl }}{% link docs/apps/knowledge-base.md %}) for everyday structured knowledge work. It is intended as a baseline Atlas capability and does not require Search infrastructure.

Use [Search]({{ site.baseurl }}{% link docs/apps/search.md %}) as an optional advanced capability when you need semantic retrieval across a collection. Availability depends on your instance's configuration and your access. Ask your administrator about database connections, embedding services and indexing before getting started.

You do not need to choose one tool for all your knowledge. Keep working knowledge in a KB, index a large reference library in Search, and combine them where both navigation and retrieval provide value.
