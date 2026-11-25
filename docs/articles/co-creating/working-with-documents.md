---
title: "Working with documents beyond Markdown"
parent: Co-creating with AI
grand_parent: Articles
nav_order: 3
---

# Working with documents beyond Markdown

[Co-creating with AI]({{ site.baseurl }}{% link docs/articles/co-creating/index.md %}) · Part 3 of 6

Real projects rarely start with neatly written Markdown. You may receive a PDF, a PowerPoint presentation, a spreadsheet, a Word document or an email with attachments. People need to see what the source says and how it is presented; an agent needs accessible content it can interpret.

Workspace intake helps bridge those needs without discarding the original.

## Bring in the sources

For our exhibition, suppose the organiser provides an old survey as a PDF, a spreadsheet of dated market records and a Word document containing interview notes. These are illustrative sources, not an actual historical archive.

Create an `inputs/` folder. Use **Upload files** or **Upload files here…** from the folder menu to bring in your files. For an online reference, choose **Add link** and decide whether to keep only its address or an available preview.

Workspace saves the source or link before processing it. Open the item to see its progress. A job can require additional input or a desktop worker; a link may require authentication. Do not treat a saved link or a queued job as evidence that the agent has read its content.

Keep the originals as received. Corrected transcripts and research interpretations belong in separate working material.

## One source, several ways to read it

![Original material and derived representations]({{ site.baseurl }}{% link assets/images/articles/intake-and-bundles.svg %})

An imported file may have extracted text, page or slide fragments and rendered previews. Workspace presents them through the source item instead of requiring you to navigate the generated files individually.

Open a processed document and switch between **Text**, **Original** and **Both**. Select a page, slide, sheet or heading. The extracted text and preview follow the selection where both are available.

This is useful for humans too: a table may be easier to inspect visually, while a long passage is easier to search and quote as text.

PDF rendering is available for supported files; Office page images may need a suitably equipped desktop worker. Scanned pages, complex layouts, charts and formulas can require additional interpretation. Do not assume that text extraction has captured everything visible in a document.

## Ask the agent to interpret, not just summarise

Instead of “summarise all the files”, give the agent a purpose and an output:

> Inspect the available originals, extracted text and relevant document fragments in inputs/. Create research/source-inventory.md listing each source, its date, what it covers, and any extraction limitations. Then identify passages relevant to the uses of the market square. Keep facts, interpretations and unanswered questions separate. Cite the relevant page, sheet or section.

The agent can use targeted fragments rather than rereading every file in full when only a few pages matter. It should report any source or fragment it cannot access and must not invent material to fill gaps.

Use the relevant page or section's **Copy link** command to refer directly to it in your own notes or comments. A positional link refers to that position, so check it after a document is reorganised. A named heading may be more useful for a section that moves.

## Correct an extraction without changing the original

If the extracted interview notes need correction or annotation, open the processed file and choose **Make an editable copy**. Workspace creates a separate Markdown file alongside the original. You can edit it and add review comments.

Keep the source-reference links in the copy. This allows readers to distinguish a correction to extracted text from a new interpretation of what the source means.

The **Extracted** view continues to represent the generated material. Refreshing a source does not overwrite the edited copy; use **Compare with the current extraction** to inspect differences.

For example, if a date was extracted incorrectly, correct it after inspecting the original and record the reason. If two sources disagree on the date, preserve both claims in the research notes rather than silently choosing one.

## Know which representation you are editing

| Representation | Purpose |
| --- | --- |
| Original file or link | Preserve the source and its provenance |
| Generated extraction and previews | Make the source accessible |
| Editable Markdown copy | Correct or annotate the extracted material |
| Research notes | Interpret and compare evidence |
| Final exhibition text | Communicate the reviewed findings |

Making an editable copy does not turn Workspace into a native Word or Excel editor. The copy does not automatically write changes back into the original Office file.

For the exhibition, we now have accessible source material, an inventory and a place to record evidence. Next we will organise a growing collection of related working documents into a coherent artifact.

See [Workspace intake]({{ site.baseurl }}{% link docs/apps/workspace/intake.md %}) for supported processing options, preview rendering, editable copies and fragment links.

---

[Previous: Continuing work across conversations and agents]({{ site.baseurl }}{% link docs/articles/co-creating/continuing-work.md %}) · [Next: Organising complex documents into bundles]({{ site.baseurl }}{% link docs/articles/co-creating/bundles.md %})
