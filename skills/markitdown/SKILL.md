---
name: markitdown
description: Use when you need the text/content of a binary or rich document - PDF, Word (.docx), PowerPoint (.pptx), Excel (.xlsx/.xls), Outlook (.msg), EPub, images (OCR/EXIF), audio (transcription/EXIF), HTML, CSV/JSON/XML, or ZIP archives - converted to clean Markdown for reading, search, or analysis.
---

# MarkItDown

Microsoft's `markitdown` CLI converts files and Office documents to Markdown for LLM consumption. It preserves structure (headings, lists, tables, links) rather than high-fidelity formatting. Installed globally as an isolated CLI via `uv tool` (Python 3.12); the `markitdown` executable is on PATH.

## When to use

Reach for `markitdown` when you need the **textual content** of a file that is not plain text/source code:

- Office: `.docx`, `.pptx`, `.xlsx`, `.xls`
- PDF: `.pdf`
- Mail: `.msg` (Outlook)
- Books: `.epub`
- Web/data: `.html`, `.htm`, `.csv`, `.json`, `.xml`
- Media: images (EXIF + OCR), audio (EXIF + speech transcription)
- Archives: `.zip` (iterates contents)
- YouTube URLs (transcript)

**Do NOT use for** plain-text or source files you intend to *edit* - use the `read` tool there, because it provides line anchors that `edit` needs. `markitdown` output is content-only, with no line numbers.

## Quick reference

```bash
# Convert to stdout
markitdown path/to/file.pdf

# Write to a file
markitdown path/to/file.docx -o out.md

# Pipe content in (specify type hint when reading from stdin)
cat file.pdf | markitdown

# Enable 3rd-party plugins (off by default)
markitdown --list-plugins
markitdown --use-plugins path/to/file.pdf
```

For large outputs, redirect to a temp `.md` and then `read` that file with line ranges:

```bash
markitdown big-report.pdf -o /tmp/report.md   # then: read /tmp/report.md
```

## Notes

- First invocation per session is slower (loads the magika type-detection model); later calls are fast.
- Reinstall / upgrade: `uv tool install --python 3.12 --force 'markitdown[all]'`. Uninstall: `uv tool uninstall markitdown`.
- `markitdown[all]` (installed) bundles all optional format extras. Individual extras exist (`[pdf]`, `[docx]`, `[pptx]`, `[xlsx]`, `[xls]`, `[outlook]`, `[audio-transcription]`, `[youtube-transcription]`, ...).
- Image OCR and richer audio/video need an LLM client or Azure backend; basic EXIF/transcription work without one. Plain images may yield only metadata.
- Conversion is best-effort and lossy for complex layouts. If a table/structure looks garbled, fall back to the `read` tool (this harness's `read` also extracts PDF/Office/EPub) or inspect the raw bytes.
- Security: `markitdown` performs I/O with the current process's privileges. Only convert files you trust.
