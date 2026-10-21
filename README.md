# Epublys MCP Server

Remote [MCP](https://modelcontextprotocol.io) server for EPUB and PDF ebook manipulation — merge, split, compress, convert, edit metadata, validate, and generate AI covers. Backed by [epublys.com](https://epublys.com), running on Cloudflare Workers.

This is a **hosted server** (Streamable HTTP). There is nothing to install — this repository holds the public manifests, configuration examples, and documentation.

- **Endpoint:** `https://epublys.com/mcp` (Streamable HTTP)
- **Auth:** none needed for the free tier. An optional Bearer API key (free at <https://epublys.com/account>) carries your account's allowance and Pro.
- **Server card:** <https://epublys.com/.well-known/mcp/server-card.json>
- **Full agent docs:** <https://epublys.com/llms-full.txt>
- **Registry:** `com.epublys/epublys` on the [official MCP registry](https://registry.modelcontextprotocol.io)

## Quickstart

### Without a key (free tier)

Claude Code:

```sh
claude mcp add --transport http epublys https://epublys.com/mcp
```

Any other MCP client (`claude_desktop_config.json`, `cline_mcp_settings.json`, etc.):

```json
{
  "mcpServers": {
    "epublys": {
      "url": "https://epublys.com/mcp"
    }
  }
}
```

The free tier is identified by IP address and shares the website's free daily allowance. It is permanent, not a trial.

### With a key (your account's allowance, Pro)

1. Sign in at <https://epublys.com/account> and create an API key (`epl_…` — shown once).
2. Send it as a Bearer header:

```json
{
  "mcpServers": {
    "epublys": {
      "url": "https://epublys.com/mcp",
      "headers": { "Authorization": "Bearer epl_YOUR_KEY_HERE" }
    }
  }
}
```

Claude Code: `claude mcp add --transport http epublys https://epublys.com/mcp --header "Authorization: Bearer epl_YOUR_KEY_HERE"`

A wrong, malformed or revoked key is a `401`, not a silent fall back to the free tier. The key goes in the `Authorization` header only; a key in the URL is rejected.

## Tools

| Tool | Description |
|------|-------------|
| `merge_epub` | Merge multiple EPUB files into one |
| `split_epub` | Split an EPUB by chapter ranges |
| `compress_epub` | Compress an EPUB to reduce file size |
| `epub_to_pdf` | Convert EPUB to PDF |
| `pdf_to_epub` | Convert PDF to EPUB |
| `epub_to_markdown` | Convert EPUB to Markdown |
| `markdown_to_epub` | Build an EPUB3 from Markdown files (YAML frontmatter, Obsidian wikilinks) |
| `docx_to_epub` | Convert DOCX to EPUB |
| `read_epub_metadata` | Read EPUB metadata |
| `edit_epub_metadata` | Edit EPUB metadata and cover |
| `validate_epub` | Validate EPUB structure (with optional auto-fix) |
| `generate_cover` | Generate an AI book cover from a description (Pro) |
| `upgrade_to_pro` | Return a Stripe checkout link for the account behind the key (keyless calls get the pricing URL) |

Files are passed as base64-encoded strings in tool arguments and results.

## Limits

| | Free | Pro ($9/mo or $72/yr) |
|---|---|---|
| File size | 5 MB (15 MB `split_epub`, 10 MB `pdf_to_epub`) | 30 MB per request |
| Merge | 10 files, 20 MB total | 50 files, 30 MB total |
| Daily operations | 3 per day (reading metadata is never metered) | unlimited |
| Rate | 15 req/min | 60 req/min |
| Output branding | Epublys colophon | none |
| `generate_cover` | — | 30/month |

## Privacy

Files are processed in memory and never stored. There is no upload bucket, no retention, no content logging. Anonymous usage metrics only (tool name, status, sizes).

## REST API

The same tools are available over plain REST with an OpenAPI spec — see <https://epublys.com/for-ai-agents>.

## About this repository

The Epublys service itself is closed-source. This repository contains only the public MCP manifests (`server.json`, `smithery.yaml`) and documentation, and is the canonical public home for the `com.epublys/epublys` registry entry. Issues are welcome for MCP integration problems; for product support use <https://epublys.com/about>.
