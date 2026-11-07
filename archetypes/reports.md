---
title: '{{ replace .File.ContentBaseName `-` ` ` | title }}'
description: '' # short summary; shown on the report page, the Reports list, and the home page callout
date: '{{ .Date }}' # issue date
draft: true
report_fulltitle: '' # optional full formal title (title is the short page title). Shown as "Full title" on the report page and in the Reports list; used as the JSON-LD name and headline, and in link previews
authors: # use authors, editors, or both; delete any block you don't need
  - name: '' # one author per entry
    orcid: '' # optional. Numbers only, e.g. 0000-0002-1825-0097 (no https://orcid.org/ prefix; the URL is added automatically)
editors:
  - name: '' # one editor per entry
    orcid: '' # optional. Numbers only, e.g. 0000-0002-1825-0097 (the URL is added automatically)
publisher: 'University of Michigan'
doi: '' # e.g. 10.xxxx/xxxxx
license: '' # Creative Commons license as CC-<terms>-<version>, hyphens, no spaces: CC-BY-4.0, CC-BY-SA-4.0, CC-BY-NC-4.0, CC-BY-NC-SA-4.0, CC-BY-ND-4.0, CC-BY-NC-ND-4.0, or CC0-1.0. Displays as 'CC BY NC SA 4.0', linked to the creativecommons.org license page (the URL is built automatically). Any other text is shown as-is, unlinked.
reportPDF: '' # e.g. /documents/my-report.pdf (file goes in static/documents/)
coverImage: '' # optional cover graphic (jpg/png). Filename ONLY, e.g. my-report-cover.png. The file MUST be in assets/images/ (NOT static/images/); the build fails if it isn't found there.
coverAlt: '' # alt text for the cover; defaults to "Cover of <title>"
pdfSize: '' # e.g. 2.4 MB
citation: '' # suggested citation text
---

Optional longer description, key findings, or acknowledgments go here.
