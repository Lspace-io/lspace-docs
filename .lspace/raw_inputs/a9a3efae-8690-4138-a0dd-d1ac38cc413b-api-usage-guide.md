# Using the Lspace API

## Introduction

The Lspace API provides a comprehensive set of tools to manage and interact with your knowledge bases. This guide outlines the primary API functionalities, including adding content, searching, browsing, and managing repository history. All interactions typically require a `repositoryId`, which can be obtained by listing available repositories (see MCP Server Setup documentation for configuration).

## Core API Operations

### 1. Listing Repositories

Before interacting with a specific knowledge base, you might need to know its `repositoryId`. Lspace provides tools to list all configured repositories.

*   **Tool:** `mcp_lspace_lspace_list_repositories`
*   **Purpose:** Retrieves a list of all repositories managed by the Lspace instance, including their names, IDs, and types.
*   **Example Usage (Conceptual):**
    ```
    GET /api/lspace/repositories 
    ```
    *(The actual API endpoint might vary based on your Lspace server's routing setup. The tool name reflects the action performed).*
*   **Response:** A list of repository objects, each containing at least `id`, `name`, and `type`.

### 2. Adding Content

Lspace can ingest content from various sources.

*   **Tool:** `mcp_lspace_lspace_add_content`
*   **Purpose:** Adds new content to the specified repository, triggering knowledge base processing and generation.
*   **Key Parameters:**
    *   `repositoryId` (string, required): The ID of the target Lspace repository.
    *   `inputType` (enum, required): The type of content being added.
        *   `'text_snippet'`: For short pieces of text.
        *   `'file_upload'`: For uploading complete files.
        *   `'web_url'`: For ingesting content directly from a URL.
    *   `content` (string, conditional): The actual text content (for `text_snippet`) or the base64 encoded file content (for `file_upload`).
    *   `fileName` (string, conditional): Required if `inputType` is `file_upload`. The name of the file being uploaded (e.g., "my-document.md").
    *   `url` (string, conditional): Required if `inputType` is `web_url`. The URL to fetch content from.
    *   `title` (string, optional): A descriptive title for the content item (e.g., "Project Alpha Specification", "Meeting Notes 2023-10-26"). This title can be used by Lspace for better organization and commit messages.
    *   `metadata` (object, optional): A JSON object for any additional metadata (e.g., tags, categories).
*   **Example (File Upload):**
    *   Tool Call: `mcp_lspace_lspace_add_content(repositoryId="b3fcb584...", inputType="file_upload", fileName="api_guide.md", content="...", title="API Usage Guide")`
*   **Outcome:** The content is saved to the internal `/.lspace/raw_inputs/` directory, and the knowledge base generation process is triggered for the repository. Changes are typically committed and pushed to the remote Git repository if configured.

### 3. Searching the Knowledge Base

Lspace allows for natural language queries against the generated knowledge base.

*   **Tool:** `mcp_lspace_lspace_search_knowledge_base`
*   **Purpose:** Searches the synthesized knowledge base content (not the raw inputs directly).
*   **Key Parameters:**
    *   `repositoryId` (string, required): The ID of the repository to search within.
    *   `queryText` (string, required): The natural language search query (e.g., "How to configure the Lspace server?", "What are the testing procedures?").
*   **Response:** A list of search results, typically including snippets of relevant content and paths to the knowledge base files.

### 4. Browsing the Knowledge Base

You can explore the file and directory structure of the generated knowledge base.

*   **Tool:** `mcp_lspace_lspace_browse_knowledge_base`
*   **Purpose:** Lists directory contents or reads specific files from the generated knowledge base in the repository root.
*   **Key Parameters:**
    *   `repositoryId` (string, required): The ID of the repository.
    *   `operation` (enum, required):
        *   `'list_directory'`: To see files and folders within a path.
        *   `'read_file'`: To get the content of a specific file.
    *   `path` (string, required): The path relative to the repository root (e.g., `.` for the root, `services/orchestrator/` for a directory, `README.md` for a file).
*   **Example (List Root Directory):**
    *   Tool Call: `mcp_lspace_lspace_browse_knowledge_base(repositoryId="b3fcb584...", operation="list_directory", path=".")`
*   **Example (Read a File):**
    *   Tool Call: `mcp_lspace_lspace_browse_knowledge_base(repositoryId="b3fcb584...", operation="read_file", path="mcp-server/setup-and-configuration.md")`

### 5. Viewing Knowledge Base History

Lspace tracks changes made to the knowledge base.

*   **Tool:** `mcp_lspace_lspace_list_knowledge_base_history`
*   **Purpose:** Retrieves a log of changes, including file uploads and knowledge base generations.
*   **Key Parameters:**
    *   `repositoryId` (string, required): The ID of the repository.
    *   `limit` (number, optional): Maximum number of history entries to return (default: 20).
    *   `changeType` (enum, optional): Filter by type of change: `'file_upload'`, `'knowledge_base_generation'`, or `'both'`.
*   **Response:** A list of history entries, detailing the changes.

### 6. Undoing Knowledge Base Changes

Lspace provides functionality to revert changes.

*   **Tool:** `mcp_lspace_lspace_undo_knowledge_base_changes`
*   **Purpose:** Reverts file uploads or knowledge base generations.
*   **Key Parameters:**
    *   `repositoryId` (string, required): The ID of the repository.
    *   `changeId` (string, optional): Specific ID of a change from the history to undo.
    *   `filename` (string, optional): Target a specific file to undo its addition/changes.
    *   `lastNChanges` (number, optional): Undo the last N changes.
    *   `revertType` (enum, optional): Specify what to revert: `'file_upload'`, `'knowledge_base_generation'`, or `'both'`.
*   **Caution:** Use with care, as this modifies the repository history.

## Workflow Example

1.  **Identify Repository:** Use `mcp_lspace_lspace_list_repositories` to find the `repositoryId` for "Lspace Official Docs".
2.  **Add New Documentation:**
    ```
    mcp_lspace_lspace_add_content(
        repositoryId="b3fcb584-5fd9-4098-83b8-8c5d773d86eb",
        inputType="file_upload",
        fileName="new-feature-guide.md",
        content="# Guide to New Feature X...",
        title="New Feature X Guide"
    )
    ```
3.  **Verify Addition (Optional):**
    *   Browse: `mcp_lspace_lspace_browse_knowledge_base(repositoryId="...", operation="list_directory", path=".")` to see if a new file/directory related to "New Feature X" appears.
    *   Read README: `mcp_lspace_lspace_browse_knowledge_base(repositoryId="...", operation="read_file", path="README.md")` to see if it's mentioned.
4.  **Search:**
    ```
    mcp_lspace_lspace_search_knowledge_base(
        repositoryId="b3fcb584-5fd9-4098-83b8-8c5d773d86eb",
        queryText="How to use Feature X?"
    )
    ```
5.  **Check History:**
    ```
    mcp_lspace_lspace_list_knowledge_base_history(
        repositoryId="b3fcb584-5fd9-4098-83b8-8c5d773d86eb",
        limit=5
    )
    ```

This guide provides an overview of the main Lspace API tools. Refer to specific tool definitions or further documentation for more detailed parameter options and behaviors.
