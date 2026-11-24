---
title: Stakeholders
parent: Work
grand_parent: Apps
nav_order: 3
---

# Stakeholders

Use Stakeholders to link people and organisations from a shared registry to a project register. Record roles, responsibilities, power, interest, and engagement. Use those records to review stakeholder relationships and prepare communication briefs.

## How to use Stakeholders

1. Open or create a project register. When adding a person, search the shared registry if you have access and reuse the existing record.
2. Set the person’s role, responsibilities, power, interest and engagement for this project. Add reporting lines and dated observations as needed.
3. Use the matrix, org chart and engagement views to review the project’s relationships. Use survey responses and observations when preparing a communication brief.

The shared registry holds people and organisations. A project register holds what you know about their involvement in that project.

## Import people from a CSV

Add people from a CSV takes a file (or pasted text) with a header row. Columns are matched by name,
in any case: `name` (required), `emails`, `phones`, `organization`, `title`, `aliases`, `timezone`, `location`. Several values in one
cell are separated by `;`.

**Check the file** first. It lists every row as *will be added*, *already added* or *skipped* (an email that is already in the register
under someone else), and every problem by row number. Nothing is written until you press **Add**, and a bad row never stops the good ones.
Adding the same file twice adds no one twice. Files are limited to 2 MB and 2000 rows.

Export people as CSV gives the roster with each person's current role, RACI, power, interest, engagement levels, who they report to and last review. It
contains no notes, observations or survey responses. Cells that a spreadsheet would run as a formula (starting `=`, `+`, `-` or `@`)
are written with a leading apostrophe, and read back without it.

## Playbooks supporting work

[Map stakeholders]({{ site.baseurl }}{% link docs/playbooks/work.md %}#map-stakeholders) develops engagement actions. [Prepare or debrief a meeting]({{ site.baseurl }}{% link docs/playbooks/work.md %}#prepare-meeting) can use that context to plan the next conversation and follow-up.
