# Knowledge Base Generation Internals

## Introduction

This document provides a look under the hood at how Lspace, particularly its Management and Control Plane (MCP) Server and integrated Large Language Models (LLMs), transforms raw input documents into a structured and interconnected knowledge base. Understanding this process can help developers and advanced users optimize their inputs and comprehend Lspace's behavior.

## Core Components

The knowledge base (KB) generation process involves several key components:

1.  **Lspace MCP Server:** The central orchestrator that manages repositories, receives new content, and coordinates the KB generation pipeline.
2.  **Raw Input Storage (`/.lspace/raw_inputs/`):** When content is added (e.g., via file upload, text snippet), the original, unaltered content is stored here with a unique identifier. This ensures the pristine source is always preserved.
3.  **Timeline (`/.lspace/timeline.json`):** Records all operations performed on the raw inputs, providing an audit trail.
4.  **LLM Engine:** A Large Language Model (e.g., GPT-4, Claude) that performs the core intelligent tasks of understanding, synthesizing, and structuring information.
5.  **Prompts (`src/config/prompts.ts` in the MCP Server codebase):** A crucial set of instructions and guidelines that direct the LLM's behavior during KB construction. These prompts define how the LLM should:
    *   Synthesize information rather than just storing it.
    *   Handle contradictions.
    *   Organize content hierarchically.
    *   Manage file creation and updates.
    *   Critically, how it should build and maintain the main `/README.md` as the KB's entry point and summary.
    *   Implement internal linking and source citations.
6.  **Git Adapter (e.g., `GitHubAdapter.ts`):** Manages interactions with the remote Git repository (e.g., GitHub), including cloning, pulling, committing, and pushing the generated KB.

## The Generation Pipeline

When new content is added to a repository via `mcp_lspace_lspace_add_content`:

1.  **Content Ingestion & Storage:**
    *   The MCP Server receives the content.
    *   The raw content is saved to `/.lspace/raw_inputs/` with a unique filename (often UUID-prefixed).
    *   An entry is made in `/.lspace/timeline.json`.

2.  **LLM Processing Cycle (Iterative):**
    *   The MCP Server invokes the LLM, providing it with:
        *   The content of the newly added raw input file.
        *   Access to tools for interacting with the repository's file system (read, write, list files/directories in the KB, but *never* in `/.lspace/`).
        *   The system prompt from `prompts.ts` that guides its overall behavior.
    *   **Initial Analysis & README Check:** The LLM first typically checks the state of the root `/README.md`. If it doesn't exist, it creates one.
    *   **Content Synthesis:** Based on the prompts, the LLM analyzes the new input:
        *   It determines if the information belongs in existing KB files or requires new files/directories.
        *   It prioritizes updating existing files to keep topics cohesive (Principle of Intelligent Aggregation).
        *   It may create new topical files or directories in the repository root.
    *   **Contradiction Resolution:** The LLM attempts to identify and resolve contradictions between the new information and existing KB content, typically favoring newer information.
    *   **README.md Update:** CRITICALLY, the LLM updates the root `/README.md`. As per the prompts, this involves:
        *   Integrating a summary of the new information into the README's main narrative sections (Overview, Key Concepts, etc.), especially if the new document provides foundational details.
        *   Adding links to new or significantly updated KB files.
        *   Logging the change in a "Recent Updates" section, with proper Markdown links.
    *   **File System Operations:** The LLM uses its tools (`write_file`, `edit_file`, `create_directory`, etc.) to implement the changes in the repository root.
    *   **Linking & Citations:** The LLM is instructed to create relative Markdown links for internal KB navigation and to cite sources from `/.lspace/raw_inputs/` textually.

3.  **Summary of Operations:**
    *   Once the LLM considers the processing of the input file complete, it signals this to the MCP server.
    *   It provides a summary of its operations (files created/updated, main changes). This summary is used by the MCP server.

4.  **Git Commit and Push:**
    *   The MCP Server, using the Git Adapter, stages all changes in the repository root (new files, modified files like `/README.md`).
    *   It generates a commit message (often using the LLM's summary of operations as input, with an emphasis on using human-readable titles/filenames for the commit subject).
    *   It commits the changes and pushes them to the configured remote repository (e.g., GitHub).

## Key Principles Guiding the LLM (from `prompts.ts`)

*   **Synthesis over Storage:** Create cohesive articles, not just collections of raw files.
*   **Intelligent Aggregation:** Merge related info; avoid redundant files.
*   **Hierarchical Organization:** Build a logical directory structure in the root.
*   **README.md as Central Hub:** Ensure it's a comprehensive, well-linked entry point that truly summarizes the KB.
*   **Robust Linking:** All internal file mentions should be active Markdown links.
*   **Clear Source Citation:** Attribute information to its raw input document.

## Customization and Evolution

The behavior of the KB generation process is heavily influenced by the system prompts in `prompts.ts`. Modifying these prompts is the primary way to evolve how Lspace organizes information, synthesizes content, and structures the knowledge base. As Lspace develops, these prompts will likely be further refined for even more sophisticated and nuanced knowledge management.
