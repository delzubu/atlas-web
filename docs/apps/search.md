---
title: Search
parent: Core
grand_parent: Apps
nav_order: 5
---

# Search

Use Search to find evidence in document collections, explore indexed Python code or query a Neo4j graph directly. The navigator groups datasets by database. Search ranks results using signals such as text, semantic similarity, metadata and graph relationships, depending on the dataset.

## How to use Search

1. Open Search and choose one or more indexes in the navigator. With none selected, Search queries all eligible indexes.
2. Enter a question and select **Search**. Open the result links to inspect the source material, or refine how the results are ranked.
3. Use **Statistics** to inspect index coverage and freshness. With the right access, use **Needs review** to curate indexed knowledge or **Native query** to run a read-only query on compatible databases.

Your administrator configures shared database connections. Use Knowledge base to browse and edit articles directly.

## Index documents or Python code

With Search writer access, open **Statistics**, select a writable document dataset and choose **Add documents…** to queue pasted text, an upload, a web page or a Confluence source. An authorised `search-intake` worker processes the job. Watch its status in Search.

From the command line, sign in with `search-intake login`, then run `search-intake intake --database <database> <dataset> <source>` to add and process a source. For a Code dataset, give it a local Python file or folder. Naming the database distinguishes datasets with the same name. For code, **Statistics** shows modules, classes, functions and unresolved references. [Indexing CLI guide](https://github.com/delzubu/atlas-core/blob/core-v1.2.0/apps/search/intake/AGENTS.md)

## Configure a shared database

With the Search **admin** role, select **Add a database** at the top of the navigator. Enter the Neo4j address and credentials. **Test** checks the connection and suggests what the database holds. Choose **Holds**, then give the connection an ID and display name and save it.

| Holds | Use it for |
| --- | --- |
| **Documents** | Searching indexed documents and, when writes are enabled, adding and curating material. |
| **Code** | Searching indexed Python code. Each dataset represents a codebase. |
| **Any graph** | Running native queries against an existing Neo4j graph. It starts with a **default** entry for the whole database. |

Open the database folder’s menu for **Edit database…**, **Test connection** or **All settings…**. Under **Datasets**, define how items in an **Any graph** database belong to datasets and preview the definition before saving. Those datasets are available in **Native query**. Under **Search and models**, choose an embedding model and a reranker from the options configured in Identity. Existing vectors retain the model that created them. When selected databases use different rerankers, Search ranks each database’s results with its own model and combines the rankings. [Administration settings]({{ site.baseurl }}{% link docs/apps/identity.md %}#administration-settings)

## Query your own Neo4j database

With Search writer access, open **Your databases** and choose **Connect a database…**. Enter a name, a reachable HTTPS address for the Neo4j Query API and the database credentials. Under **Holds**, choose **Documents** for an Atlas document graph or **Any graph** for another schema. **More settings** holds the database name, Cloudflare Access credentials and embedding options. Search tests the read-only connection before saving it. [Neo4j connection guide]({{ site.baseurl }}{% link docs/apps/search/connect-local-neo4j.md %})

For a database without Atlas Search datasets, select the **default** index to work with the whole graph. Open **Statistics** to inspect its labels, relationship types and indexes. In **Native query**, use **Keyword** for a full-text index, **Vector** for a vector index or **Graph** for read-only Cypher. To search a vector index with text, configure the embedding model used to build the index and an endpoint that can run it.

Each temporary connection is private to its owner for querying and expires after a day without use. Keep up to five at a time. Use **Edit** to reconnect at a new address or **Disconnect** to remove the connection immediately. Agents can manage their owner’s connections and run native queries through Search MCP v2.

## When Search earns its place

For a large collection of long documents, ask a connected agent to search a named index for a specific question, return relevant passages and original source references, and identify conflicting evidence or missing coverage. Inspect the sources before accepting a conclusion.

Search requires configured database and inference resources for semantic retrieval. It is not required for ordinary Workspace or KB editing. [KB versus Search]({{ site.baseurl }}{% link docs/articles/kb-versus-search.md %}) explains the decision; [Your data and agent access]({{ site.baseurl }}{% link docs/getting-started/data-and-access.md %}) explains operating requirements.

## Playbooks supporting work

[Query your own graph database]({{ site.baseurl }}{% link docs/playbooks/core.md %}#query-your-graph) helps an agent inspect a Neo4j schema and answer a question using evidence from the graph.

[Research a D&D topic]({{ site.baseurl }}{% link docs/playbooks/hobby.md %}#dnd-research-topic) is a workflow for gathering and recording findings from knowledge sources, including disagreements and gaps.

For the product background and the Python indexing experiment, read [Search beyond documents]({{ site.baseurl }}{% link docs/articles/search-beyond-documents.md %}).
