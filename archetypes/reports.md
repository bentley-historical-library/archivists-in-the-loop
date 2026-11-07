---
title: '{{ replace .File.ContentBaseName `-` ` ` | title }}'
description: '' # short summary; shown on the report page, the Reports list, and the home page callout
date: '{{ .Date }}' # issue date
draft: true
authors:
  - '' # one name per line
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
