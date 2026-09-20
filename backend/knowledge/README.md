# NeoBank Knowledge Base

This directory contains documents that are automatically ingested into the
miniVoxSetu RAG (Retrieval-Augmented Generation) pipeline at startup.

## Supported Formats
- `.txt` — Plain text
- `.md` — Markdown (formatting is stripped)
- `.pdf` — PDF documents (requires pymupdf)

## How to Add Documents
1. Place your files in this directory
2. Restart the backend server
3. Documents are automatically chunked, embedded, and indexed

## Chunking Details
- **Chunk size**: ~500 characters (optimized for all-MiniLM-L6-v2's 256-token window)
- **Overlap**: 50 characters between chunks (preserves cross-boundary context)
- **Deduplication**: SHA256 hash prevents re-indexing identical content

## Current Documents
- `neobank_faq.txt` — Core NeoBank FAQ (21 topics: accounts, cards, loans, UPI, etc.)
