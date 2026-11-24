---
title: Workspace intake
parent: Workspace
grand_parent: Core
nav_order: 1
permalink: /docs/apps/workspace/intake/
---

# Workspace intake

Bring a source into Workspace, then read its extracted text, inspect the original pages and link to the passage your project depends on. Intake supports PowerPoint, Word, Excel, PDFs, saved email and links to web pages or documents. It keeps the uploaded original or link alongside the material it generates.

[Back to Workspace]({{ site.baseurl }}{% link docs/apps/workspace.md %})

## Add files or links in the browser

1. Open the destination workspace. In the navigator, choose **Upload files**, or open the destination folder’s menu and choose **Upload files here…**.
2. Drop files or a folder into the dialog, or select **Choose files…**. Check the destination and the list, then select **Upload**. If a file fails, correct the error shown beside it and retry that file.
3. For a web page, SharePoint or OneDrive document, or an Atlas file or folder, choose **Add link** in the navigator or **Add link here…** in the folder menu. Paste the address, name the link and choose how to keep it.

| Option | When to use it |
| --- | --- |
| **Keep link only** | Keep a reference and open its address on demand. |
| **Keep preview updated** | Keep a copy of the text and available pictures from the linked source. Use **Refresh** to update it. |
| **Open live** | Open the current Atlas target with your own access. This option applies to Workspace links. |
| **Import copy** | Store a file from your computer in the workspace. |

Workspace saves the item before processing it. Open the item to see when its preview is **Ready**. **Needs input** means there is a question on the item’s page, such as a sign-in request or approval to copy into a workspace with a broader audience. **Deferred** means a desktop worker is needed. **Failed** shows the last attempt’s error. Use **Open original** on a link to go to its source.

## Intake from the command line

With the Workspace CLI installed, run `ws-tool login` and sign in to an account that can change the destination. The examples use the `main` ref of a workspace named `quarterly-review`. Replace the example project, folder and file names with yours.

From outside a checkout, give `--to` the full Workspace address to upload and process a local file. A local folder also works as a source for processing its supported files.

```powershell
ws-tool intake ./brief.pptx --to atlas://workspace/quarterly-review/main/inbox
ws-tool intake --link https://example.org/report.pdf --to atlas://workspace/quarterly-review/main/inbox
```

To generate or refresh the extracted material for a workspace file, copy its Atlas link and pass it to intake. Use `--render none` for text and outline only.

```powershell
ws-tool intake atlas://workspace/quarterly-review/main/inbox/brief.pptx
```

Inside a [local checkout]({{ site.baseurl }}{% link docs/apps/workspace.md %}#work-from-a-local-checkout), intake prepares and stages files locally. Commit and push to send them to the workspace. For example, run this from the checkout root with the source file outside the checkout.

```powershell
ws-tool intake ../brief.pptx --to inbox
ws-tool git commit -m "Add the project brief"
ws-tool git push
```

## Render and refresh previews

The server can extract text from supported files and render PDF pages. PowerPoint, Word and Excel page images need a desktop with Microsoft Office or LibreOffice. A desktop worker can also handle jobs that require an interactive browser.

- For a **Deferred** job, sign in on a suitably equipped computer and run `ws-tool intake --worker`. Add `--once` to work through the available queue and exit.
- To inspect an ingested file, open it and switch between **Text**, **Original** and **Both**. Choose a slide, page, sheet or heading in the section menu. The picture and the extracted text follow that selection when both are available.
- Use **Refresh** on the item to process it again, or **Refresh with options…** to change how a link is kept. If a refresh fails, the previous preview stays available.

Intake also reads saved `.eml` messages and extracts supported attachments. The message and extracted attachment text are available as text.

## Make the extracted text editable

1. Open the processed file in Workspace and select **Make an editable copy**. This requires write access. A file named `brief.pptx` gets a separate Markdown copy named `brief.pptx.md`.
2. Select **Edited** and use the visual or source editor to correct the text or add notes. Keep the original-reference links at the start of each section so the text stays tied to its page, slide, sheet or heading. Reviewers can comment on the editable copy.
3. Switch back to **Extracted** to read the generated text. After refreshing the source, use **Compare with the current extraction** to review differences. Workspace keeps your edited copy across refreshes.

## Link to a page, slide or section

1. Open the file and select a position in the section menu or preview strip.
2. Select **Copy link to this slide**, **Copy link to this page**, **Copy link to this sheet** or **Copy link to this section**, as shown for the file.
3. Paste the link into a workspace note or project reference. Opening it takes the reader to the position you selected.

For example, `atlas://workspace/quarterly-review/main/inbox/brief.pptx#slide=7` opens slide seven. In a Markdown file beside the deck, you can show the rendered slide directly with `![Revenue summary](brief.pptx#slide=7)`. A positional link such as `#slide=7` follows the slide number if the deck is reordered. Use a heading link when referring to a named Word section.
