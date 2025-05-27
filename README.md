# Lspace Official Docs

## Overview

This repository contains a structured knowledge base designed to synthesize information from raw input documents stored securely in the `/.lspace/raw_inputs/` directory, while the model-generated knowledge base resides in the repository root.

## Repository Structure

```
repository/
├── .lspace/            # System-level raw inputs and metadata
│   ├── timeline.json    # Operation history for source files
│   └── raw_inputs/      # Storage for raw documents
├── README.md           # Main entry point & overview of the knowledge base
├── topic1/             # Example content directory
│   └── concept1.md     # Article synthesized from raw data
├── topic2/             # Example content directory
│   └── concept2.md     
└── services/           # Services directory
    └── orchestrator/   # Orchestrator Service documentation
        ├── overview.md
        └── api-endpoints.md
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
- Updated the structure based on insights from `5c08683c-23cb-4174-bc58-98f9b39ff139-knowledge-base-structure.md`.
- Added Orchestrator Service documentation based on `2f4a943a-8b3c-404c-8da9-29730bc33a0b-orchestrator.md`.
