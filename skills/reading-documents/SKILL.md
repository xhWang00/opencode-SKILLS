---
name: reading-documents
description: Use when needing to read or extract text content from document files (PDF, DOCX, PPTX, XLSX, EPUB, HTML, CSV, JSON, images with OCR, audio transcripts, YouTube). Before reading, ask the user whether they want to use markitdown or direct per-format libraries (python-docx, pdfminer, openpyxl, etc.).
---

# Reading Documents with MarkItDown

## Overview

Microsoft MarkItDown converts PDF, DOCX, PPTX, XLSX, images (OCR), EPUB, HTML, CSV, JSON, XML, audio transcripts, YouTube captions, RSS feeds, and more to Markdown. Single consistent CLI or Python API.

## When to Use Each Approach

When asked to read a document, **first ask the user** which approach they prefer:

| Approach | Pros | Cons |
|----------|------|------|
| **markitdown** | Single tool for all formats, consistent markdown output, handles OCR/transcription | Adds a dependency, less control over format-specific details |
| **Direct library** (python-docx, pdfminer, openpyxl, etc.) | More control, may already be installed, no extra dependency | Different API for each format, manual markdown rendering |

If the user has no preference, default to markitdown.

## Installation

```bash
pip install markitdown          # core only (txt, html, csv, json, xml)
pip install "markitdown[pdf]"   # PDF support
pip install "markitdown[docx]"  # DOCX support
pip install "markitdown[pptx]"  # PPTX support
pip install "markitdown[xlsx]"  # XLSX support
pip install "markitdown[all]"   # everything
```

Verify with `pip show markitdown`. If CLI isn't on PATH, use `python3 -m markitdown` or find it at `~/.local/bin/markitdown`.

## CLI Usage

```bash
markitdown file.pdf              # stdout
markitdown file.docx -o out.md   # to file
markitdown < file.pdf            # stdin
cat file.pdf | markitdown        # pipe
markitdown -x pdf < stream       # hint extension for stdin
```

## Python API

```python
from markitdown import MarkItDown

md = MarkItDown()
result = md.convert("path/to/file.pdf")
print(result.text_content)

# From URL
result = md.convert_url("https://example.com/doc.pdf")

# From stream
with open("file.pdf", "rb") as f:
    result = md.convert_stream(f)
```

## Key Options

| Flag | Purpose |
|------|---------|
| `-o FILE` | Write to file instead of stdout |
| `-x EXT` | Hint file extension (useful for stdin) |
| `-m MIME` | Hint MIME type |
| `--keep-data-uris` | Preserve base64 images in output |
| `-p` | Enable third-party plugins |
| `-d` | Use Azure Document Intelligence (requires endpoint) |

## Supported Formats

txt, pdf, docx, pptx, xlsx, html, csv, json, xml, epub, images (OCR), audio (transcription), YouTube, RSS, Outlook MSG, Jupyter notebooks, ZIP archives.
