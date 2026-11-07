---
title: '{{ replace .File.ContentBaseName `-` ` ` | title }}'
description: '' # short summary; shown on the report page, the Reports list, and the home page callout
date: '{{ .Date }}' # issue date
draft: true
report_title: '' # full formal title of the report (may differ from the short page title); shown under the page heading and used as the JSON-LD headline
authors:
  - name: '' # one author per entry
    orcid: '' # optional. Numbers only, e.g. 0000-0002-1825-0097 (no https://orcid.org/ prefix; the URL is added automatically)
publisher: 'University of Michigan'
doi: '' # e.g. 10.xxxx/xxxxx
license: '' # e.g. CC BY 4.0
reportPDF: '' # e.g. /documents/my-report.pdf (file goes in static/documents/)
coverImage: '' # optional cover graphic (jpg/png). Filename ONLY, e.g. my-report-cover.png. The file MUST be in assets/images/ (NOT static/images/); the build fails if it isn't found there.
coverAlt: '' # alt text for the cover; defaults to "Cover of <title>"
pdfSize: '' # e.g. 2.4 MB
citation: '' # suggested citation text
---

Optional longer description, key findings, or acknowledgments go here.
