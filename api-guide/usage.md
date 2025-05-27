# Lspace API: Usage Guide

## Introduction

The Lspace API provides a comprehensive set of tools to manage and interact with your knowledge bases. This guide outlines the primary API functionalities, including adding content, searching, browsing, and managing repository history. All interactions typically require a `repositoryId`, which can be obtained by listing available repositories (see MCP Server Setup documentation for configuration).

## Core API Operations

### Listing Repositories

Before interacting with a specific knowledge base, you might need to know its `repositoryId`. Lspace provides tools to list all configured repositories.

* **Tool:** `mcp_lspace_lspace_list_repositories`
* **Purpose:** Retrieves a list of all repositories managed by the Lspace instance, including names, IDs, and types.
* **Response:** A list of repository objects, each containing at least `id`, `name`, and `type`.

### Adding Content

Lspace can ingest content from various sources.

* **Tool:** `mcp_lspace_lspace_add_content`
* **Purpose:** Adds new content to the specified repository, triggering knowledge base processing and generation.
* **Outcome:** The content is saved to the internal `/.lspace/raw_inputs/` directory, triggering processing.

### Searching the Knowledge Base

Lspace allows for natural language queries against the generated knowledge base.

* **Tool:** `mcp_lspace_lspace_search_knowledge_base`
* **Purpose:** Searches the synthesized knowledge base content.
* **Response:** A list of search results with relevant content snippets and file paths.

### Browsing the Knowledge Base

You can explore the file and directory structure of the generated knowledge base.

* **Tool:** `mcp_lspace_lspace_browse_knowledge_base`
* **Purpose:** Lists directory contents or reads specific files from the generated knowledge base.

### Viewing Knowledge Base History

Lspace tracks changes to the knowledge base.

* **Tool:** `mcp_lspace_lspace_list_knowledge_base_history`
* **Purpose:** Retrieves a log of changes, including file uploads and knowledge base generations.

### Undoing Knowledge Base Changes

Lspace provides functionality to revert changes.

* **Tool:** `mcp_lspace_lspace_undo_knowledge_base_changes`
* **Purpose:** Reverts certain changes regarding file uploads or knowledge base generations.

## Workflow Example

1. **Identify Repository:** Use repository tools to find a `repositoryId`.
2. **Add Documentation:** Upload content and trigger processing.
3. **Verify Addition (Optional):** Browse and check for updates.
4. **Search:** Query for relevant information.
5. **Check History:** Validate changes logged.

This guide provides an overview of main Lspace API tools. Refer to further documentation for detailed parameters and behaviors.

---

(Source: "a9a3efae-8690-4138-a0dd-d1ac38cc413b-api-usage-guide.md" from input documents)