# Lspace Official Docs

## Overview

This repository contains a structured knowledge base designed to synthesize information from raw input documents stored securely in the `/.lspace/raw_inputs/` directory, while the model-generated knowledge base resides in the repository root.

## Repository Structure

```
repository/
[0m├── .lspace/            # System-level raw inputs and metadata
[0m│   ├── timeline.json    # Operation history for source files
[0m│   └── raw_inputs/      # Storage for raw documents
[0m├── README.md           # Main entry point & overview of the knowledge base
[0m├── mcp-server/         # MCP Server Setup and Configuration
[0m│   └── setup-and-configuration.md # MCP server setup guide
[0m├── services/           # Services directory
[0m│   └── orchestrator/   # Orchestrator Service documentation
[0m│       ├── overview.md
[0m│       └── api-endpoints.md
[0m└── api-guide/         # API Usage documentation
│   └── usage.md
```

## Knowledge Base Architecture

### 1. Raw Input Storage (`/.lspace/raw_inputs/`)
- Stores original documents and tracks operations in `timeline.json`
- Raw files here are preserved as the ultimate source for KB

### 2. Synthesis Layer (Repository Root)
- Knowledge base entries synthesized from raw inputs
- Main `/README.md` as the repository's homepage, overviewing the KB
- Cross-referencing and hierarchical organization in root directories

## Knowledge Base Components

### Entry Page (`/README.md`)
The `/README.md` serves as the main entry point, outlining:
- A description of the knowledge scope
- Links to major sections in the root
- Recent updates, key topics, and concept overviews

### Topic Pages & Directories
Located within the root and offer detailed insights on topics:
- Synthesized from raw documents with links to the source
- May include visual aids and hierarchical structure for subtopics

### API Guide
- A new section for the Lspace API, which allows users to add, search, browse, and manage the repository content programmatically.
- Detailed usage and tool descriptions can be found in [API Guide](api-guide/usage.md).

## Processes

### Document Ingestion
1. Store document in `/.lspace/raw_inputs/`
2. Trigger KB update in root, focusing on synthesized coherence
3. Update `/README.md` for overall KB summary

### Regeneration & Updates
- Full or partial rebuilds as documents change or are reorganized
- LLM synthesizes updates, enhancing contents and summaries

## API and User Interface

- Provides endpoints for interacting with the knowledge base and viewing its structure.
- `/README.md` offers an intuitive start for user navigation with source tracking and search capabilities.

## Recent Updates
- Integrated MCP Server setup details from the document `b431ef7b-63da-4166-8dae-949c778ff3b1-mcp-server-setup.md`.
- Updated the structure based on insights from `5c08683c-23cb-4174-bc58-98f9b39ff139-knowledge-base-structure.md`.
- Added Orchestrator Service documentation based on `2f4a943a-8b3c-404c-8da9-29730bc33a0b-orchestrator.md`.
- Added API Usage Guide from the document `a9a3efae-8690-4138-a0dd-d1ac38cc413b-api-usage-guide.md`.