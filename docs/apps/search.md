---
title: Search
parent: Core
grand_parent: Apps
nav_order: 5
---

# Search

Use Search to query the indexes available on your Atlas instance. Refine a search and inspect the articles returned by the index. Atlas combines signals such as text, semantic similarity, metadata, and graph relationships to rank results.

## How to use Search

1. Open Search and choose one or more knowledge indexes in the navigator. With none selected, Search queries all eligible knowledge indexes.
2. Enter a question and select **Search**. Open the result links to inspect the source material, or refine how the results are ranked.
3. Use **Statistics** to inspect index coverage and freshness. With the right access, use **Needs review** to curate indexed knowledge or **Native query** to run a read-only query on compatible databases.

Your administrator configures shared database connections. Use Knowledge base to browse and edit articles directly.

## Query your own Neo4j database

With Search writer access, open **Your databases** and choose **Connect a database…**. Supply a name, a reachable HTTPS address for the Neo4j Query API and the database credentials. Search tests the connection before saving it. For setup on your own computer, see the [Neo4j connection guide](https://github.com/delzubu/atlas-core/blob/core-v1.1.2/apps/search/web/help/connect-local-neo4j.md).

For a database without Atlas Search datasets, select the **default** index to work with the whole graph. Open **Statistics** to inspect its labels, relationship types and indexes. In **Native query**, use **Keyword** for a full-text index, **Vector** for a vector index or **Graph** for read-only Cypher. To search a vector index with text, configure the embedding model used to build the index and an endpoint that can run it.

Each temporary connection is private to its owner for querying and expires after a day without use. Keep up to five at a time. Use **Edit** to reconnect at a new address or **Disconnect** to remove the connection immediately. Agents can manage their owner’s connections and run native queries through Search MCP v2.

## Playbooks supporting work

[Query your own graph database]({{ site.baseurl }}{% link docs/playbooks/core.md %}#query-your-graph) helps an agent inspect a Neo4j schema and answer a question using evidence from the graph.

[Research a D&D topic]({{ site.baseurl }}{% link docs/playbooks/hobby.md %}#dnd-research-topic) is a workflow for gathering and recording findings from knowledge sources, including disagreements and gaps.
