---
title: Core playbooks
parent: Playbooks
nav_order: 1
permalink: /docs/playbooks/core/
---

# Core playbooks

Use Core playbooks with an agent to set up a workspace knowledge base or answer questions from your own Neo4j graph.

## Knowledge bases

### Set up a knowledge base
{: #setup-knowledge-base }

Give the agent the subject, audience and any starter sources. It sets up the workspace knowledge base, agrees on folders and tags, records conventions, seeds the first articles and checks the indexes.

Playbook ID `setup-knowledge-base`.

## Search

### Query your own graph database
{: #query-your-graph }

Give the agent the question you want to answer and identify a database you already connected in Search, or ask it to connect your database over HTTPS. The agent inspects the schema and available indexes, then uses keyword, vector or read-only Cypher queries as the question requires. It cites the nodes behind the answer and lets you decide whether to disconnect the database or leave it to expire.

Playbook ID `query-your-graph`. See [Search]({{ site.baseurl }}{% link docs/apps/search.md %}#query-your-own-neo4j-database) for connection requirements.
