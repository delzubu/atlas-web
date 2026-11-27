---
title: "Search beyond documents"
parent: Articles
nav_order: 11
description: "Atlas Search 1.2.0 brings document indexing into Atlas and explores semantic retrieval across Python code and existing graphs."
---

# Search beyond documents

*27 November 2026 · Atlas Core 1.2.0*

Search has its origins in a fairly simple problem: I have a large collection of material that I want to be able to use when working with AI. My historical library alone contains hundreds of PDFs. I know roughly what is in the collection, but neither I nor an agent should have to navigate and reread all those documents every time a new question comes up.

The original Search was a front end to a separate document-indexing service, doc-indexer. It gave me a way to query that collection, and gradually became part of Atlas's research workflow. With the 1.2.0 release, I have brought the search and indexing logic into Atlas itself. Search now connects directly to Neo4j, owns the way information is indexed and retrieved, and exposes those capabilities to people and agents.

The change also gave me an opportunity to explore a question that had been on my mind for some time: why should the information we search always have the structure of a document?

## Documents remain the starting point

Reports, presentations, articles and books have an inherent hierarchy. A book has chapters and sections; a report has headings and paragraphs. Cutting everything into arbitrary blocks of text loses some of that structure.

The document intake tool extracts material from supported sources, preserves its hierarchy and divides the text into smaller searchable passages. It can index local documents, web material, Confluence sources and selected files from an Atlas workspace. A text-based PDF ebook can be treated just like another document; intake is not a promise to support every ebook format or scanned document.

The resulting graph preserves the relationship between a passage and its containing sections and document. Text embeddings allow Search to retrieve material by meaning, not merely by matching words. Metadata and relationships add another dimension: the index can contain tags, semantic entities and similarity links. Optional AI-assisted enrichment can assign semantic metadata to passages using a shared vocabulary.

![Document indexing and retrieval]({{ site.baseurl }}{% link assets/images/articles/search-representations.svg %})

When a question arrives, Search combines several retrieval signals. Exact words help locate names and terminology, embeddings help find related concepts, and graph connections can bring other relevant material into consideration. Optional reranking helps refine the result. The goal is to identify useful evidence at the right level of detail, while retaining the link to the original material.

For example, when researching how a medieval town financed public works, I might ask about taxation, market privileges and municipal income. The useful passages might not contain the exact wording of my question. They may be spread across several books and reports. Search helps the agent find them without reading the library from beginning to end.

The heavy work of extracting, chunking and preparing the index runs through the dedicated `search-intake` command-line tool or its worker. The index is published through Atlas using the user's authorised connection. Neo4j and compatible embedding services are still infrastructure requirements; moving the logic into Atlas does not eliminate the cost of building or maintaining an index.

## An experiment: searching source code

A source tree is not really a collection of prose documents. It contains modules, classes, functions, declarations and references. A useful index should reflect those structures rather than simply treat every file as a long string of text.

In 1.2.0 I introduced a separate representation for Python code. Intake parses the source files without importing or executing them. It identifies modules, classes and functions, records signatures, docstrings and relevant source text, and connects symbols through statically resolvable imports and calls. Symbols can also be embedded for semantic retrieval.

This makes a different kind of question possible. Suppose an agent needs to understand how a project handles revision conflicts. It might start by searching for "prevent concurrent edits from overwriting each other", identify relevant functions and follow indexed call or import relationships to related symbols. Search returns source references, including line ranges where available, so the agent can inspect the actual code.

The experiment is deliberately limited. Python's dynamic behaviour means a static index cannot resolve every call or determine what happens at runtime. It is not a replacement for reading the source, running tests or using a proper debugger. Nor does it automatically stay current as code changes: the indexed source needs to be refreshed.

What interests me is not just code search itself, but the fact that the same Search tool can work with different information structures. A document collection and a codebase need different intake and retrieval strategies. They can nevertheless share the same user experience: select the relevant datasets, ask a question and inspect the returned evidence.

## Working with existing graphs

There is also a more direct route. An existing Neo4j database does not have to adopt Atlas's document or code representation just to be queried.

The **Any graph** connection type allows a user or agent to inspect the graph's available schema and indexes and run native queries. Where the necessary indexes and compatible embedding configuration exist, keyword and vector queries are available too. Administrators can define logical datasets for shared generic connections, rather than always working with the entire graph.

This is useful when the information already exists in a graph maintained elsewhere. An agent can discover its structure and formulate a read-only Cypher query. The generic connection does not automatically acquire the document-specific retrieval, intake or semantic enrichment behaviour; those require knowledge of the representation.

## How I use Search

Most of the time I use Search through an agent rather than open its interface myself. It is one of the tools available while investigating a question, building a model or reviewing an existing body of work.

A typical instruction might be:

> Search the historical library for evidence about municipal market rights and revenues. Identify the most relevant sources, inspect their supporting passages and record the findings in the research folder with original source references. Flag conflicting accounts rather than constructing a conclusion unsupported by the evidence.

The agent can refine its queries, retrieve more evidence and compare what it finds. Since the collection is maintained in Atlas rather than in a particular conversation, I can use another compatible agent or subscription to continue the same research.

But sometimes I just want to find something quickly. The Search web app lets me select one or more configured indexes, enter a question, inspect the ranked results and refine the search without starting a conversation. Statistics help me understand what has been indexed; native queries offer more direct access to compatible graphs.

Search is not intended to replace the lightweight Knowledge base. I still use KB to organise and navigate knowledge that I actively maintain. Search comes into its own when the amount of material makes navigation and repeated reading inefficient. The two approaches can also complement one another when a KB is indexed in Search.

See [KB versus Search — which one to use?]({{ site.baseurl }}{% link docs/articles/kb-versus-search.md %}) for that distinction, and the [Search app guide]({{ site.baseurl }}{% link docs/apps/search.md %}) for connection settings, indexing and query instructions.

## Where this could go next

The Python experiment is a useful first step, not the beginning of a commitment to index every conceivable kind of information.

I may extend source intake to other programming languages, and explore better ways of using comments and documentation when generating embeddings. A function's purpose is not necessarily apparent from its implementation alone; a richer description of intent could make semantic retrieval more useful. The current Python index already uses docstrings, signatures and source text, but there is room to experiment with what makes a good search representation.

Other possibilities include specialised retrieval strategies for knowledge graphs or structured tables. A table, for example, should not necessarily be reduced to chunks of text if its rows, columns and relationships are what give the information meaning. Those would be additional representations or, where justified, database integrations. Neo4j is the currently implemented database backend; these possibilities are not shipped features or a promised roadmap.

The direction I want to explore is fairly straightforward: let the structure of the information guide how it is indexed and retrieved, while keeping the interaction simple for people and agents. I would rather add a representation in response to a real use case than build a universal indexing framework in anticipation of one.

For the details of this release, see the [1.2.0 release notes]({{ site.baseurl }}{% link docs/changelog/2026-11-27.md %}).
