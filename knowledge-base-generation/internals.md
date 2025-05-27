# Knowledge Base Generation Internals

## Introduction
This document provides a look under the hood at how Lspace, particularly its Management and Control Plane (MCP) Server and integrated Large Language Models (LLMs), transforms raw input documents into a structured and interconnected knowledge base. Understanding this process can help developers and advanced users optimize their inputs and comprehend Lspace's behavior.

## Core Components

1. **Lspace MCP Server:** Manages repositories, receives new content, and coordinates the KB generation pipeline.
2. **Raw Input Storage (`/.lspace/raw_inputs/`):** Stores original content with unique identifiers to preserve the pristine source.
3. **Timeline (`/.lspace/timeline.json`):** Provides an audit trail for all operations performed on raw inputs.
4. **LLM Engine:** Performs core tasks of understanding, synthesizing, and structuring information.
5. **Prompts (`src/config/prompts.ts`):** Guides the LLM’s behavior during KB construction. It includes directions on information synthesis, contradiction handling, content organization, file management, and README.md maintenance.
6. **Git Adapter:** Manages interactions with the remote Git repository, performing cloning, pulling, committing, and pushing actions.

## The Generation Pipeline

### 1. Content Ingestion & Storage
   - The MCP Server receives and saves raw content to `/.lspace/raw_inputs/` with a unique filename.
   - Entries are made in `/.lspace/timeline.json`.

### 2. LLM Processing Cycle
   - The MCP Server invokes the LLM with the new content and read/write permissions (excluding `/.lspace/`).
   - It analyzes if the information fits into existing KB files or new ones need creation.
   - Prefer updating existing files to maintain cohesive topics.
   - Attempts to resolve contradictions, favoring newer data.
   - Updates `/README.md` with summaries, links, and logs changes in "Recent Updates".
   - Uses tools for file system operations and creates relative Markdown links and source citations.

### 3. Summary of Operations
   - Once processing is complete, the LLM provides a summary used by the MCP Server.

### 4. Git Commit and Push
   - The MCP Server stages changes, generates a human-readable commit message and pushes updates.

## Key Principles
- **Synthesis over Storage**
- **Intelligent Aggregation**
- **Hierarchical Organization**
- **README.md as Central Hub**
- **Robust Linking**
- **Clear Source Citation**

## Customization and Evolution
Modifying system prompts is the primary method to evolve Lspace’s KB generation behavior, allowing more sophisticated management.

(Source: "Knowledge Base Generation Internals" from raw inputs)