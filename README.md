# Epublys MCP Server

Remote [MCP](https://modelcontextprotocol.io) server for EPUB and PDF ebook manipulation — merge, split, compress, convert, edit metadata, validate, and generate AI covers. Backed by [epublys.com](https://epublys.com), running on Cloudflare Workers.

This is a **hosted server** (Streamable HTTP). There is nothing to install — this repository holds the public manifests, configuration examples, and documentation.

- **Endpoint:** `https://epublys.com/mcp` (Streamable HTTP)
- **Auth:** Bearer API key — free at <https://epublys.com/account>
- **Server card:** <https://epublys.com/.well-known/mcp/server-card.json>
- **Full agent docs:** <https://epublys.com/llms-full.txt>
- **Registry:** `com.epublys/epublys` on the [official MCP registry](https://registry.modelcontextprotocol.io)

## Quickstart

1. Sign up at <https://epublys.com/account> (free tier is permanent, not a trial) and create an API key (`epl_…` — shown once).
2. Add to your MCP client config (`claude_desktop_config.json`, `cline_mcp_settings.json`, etc.):

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

## Tools

| Tool | Description |
|------|-------------|
| `merge_epub` | Merge multiple EPUB files into one |
| `split_epub` | Split an EPUB by chapter ranges |
| `compress_epub` | Compress an EPUB to reduce file size |
| `epub_to_pdf` | Convert EPUB to PDF |
| `pdf_to_epub` | Convert PDF to EPUB |
| `read_epub_metadata` | Read EPUB metadata |
| `edit_epub_metadata` | Edit EPUB metadata and cover |
| `validate_epub` | Validate EPUB structure (with optional auto-fix) |
| `generate_cover` | Generate an AI book cover from a description (Pro) |

Files are passed as base64-encoded strings in tool arguments and results.

## Limits

| | Free | Pro ($9/mo or $72/yr) |
|---|---|---|
| File size | 10 MB (25 MB `split_epub`, 20 MB `pdf_to_epub`) | 100 MB |
| Merge | 10 files | 500 files |
| Rate | 15 req/min | 60 req/min |
| Output branding | Epublys colophon | none |
| `generate_cover` | — | 30/month |

## Privacy

Files are processed in memory and never stored. There is no upload bucket, no retention, no content logging. Anonymous usage metrics only (tool name, status, sizes).

## REST API

The same tools are available over plain REST with an OpenAPI spec — see <https://epublys.com/for-ai-agents>.

## About this repository

The Epublys service itself is closed-source. This repository contains only the public MCP manifests (`server.json`, `smithery.yaml`) and documentation, and is the canonical public home for the `com.epublys/epublys` registry entry. Issues are welcome for MCP integration problems; for product support use <https://epublys.com/about>.
