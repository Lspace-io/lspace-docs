# Lspace MCP Server Setup and Configuration

## Introduction

The Lspace Management and Control Plane (MCP) Server is the backend application that powers Lspace. It handles repository management, knowledge base generation, interaction with Git services (like GitHub), and exposes an API for clients to use. Proper setup and configuration are crucial for Lspace to function correctly.

## Prerequisites

Before you begin, ensure you have the following installed:
- **Node.js**: A recent LTS version is recommended.
- **npm** (comes with Node.js) or **yarn**: For managing project dependencies.
- **Git**: Required for interacting with Git repositories, especially if you plan to use the Git CLI-based GitHub adapter.

## Installation

1.  **Clone the Repository** (if you haven't already):
    ```bash
    git clone <repository_url>
    cd <repository_directory> 
    ```
    (Replace `<repository_url>` and `<repository_directory>` with the actual URL and path for the `lspace-mcp-server` or `bee-context-api` project).

2.  **Install Dependencies**:
    Navigate to the server's root directory (e.g., where `package.json` is located) and run:
    ```bash
    npm install
    ```
    or if you prefer yarn:
    ```bash
    yarn install
    ```

## Configuration (`config.local.json`)

The MCP server uses a `config.local.json` file (typically located in the project root or a `config/` subdirectory, check your project's structure) to manage sensitive information and repository configurations. This file should **not** be committed to version control. Create it if it doesn't exist.

Here's an example structure:

```json
{
  "credentials": {
    "github_pats": [
      {
        "alias": "my_github_pat",
        "token": "ghp_xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
      }
    ]
  },
  "repositories": [
    {
      "id": "your-unique-repo-id-1",
      "name": "My Local Documentation",
      "type": "local",
      "path": "/path/to/your/local/docs/repo"
    },
    {
      "id": "your-unique-repo-id-2",
      "name": "My Project on GitHub",
      "type": "github",
      "owner": "your_github_username_or_org",
      "repo": "your_repository_name",
      "branch": "main",
      "pat_alias": "my_github_pat",
      "path_to_kb": "."
    }
  ]
}
```

### `credentials` Section

-   **`github_pats`**: An array of GitHub Personal Access Tokens (PATs).
    -   `alias`: A unique, human-readable name for the PAT. This alias is referenced in the `repositories` configuration.
    -   `token`: The actual GitHub PAT. Ensure it has the necessary permissions (e.g., `repo` scope for full control of private and public repositories).

### `repositories` Section

An array of repository objects Lspace will manage.

**Common Fields:**
-   `id`: A unique UUID for the repository. You can generate one using an online UUID generator.
-   `name`: A human-readable name for the repository.
-   `type`: Can be `"local"` or `"github"`.

**For `type: "local"`:**
-   `path`: The absolute file system path to the local Git repository.

**For `type: "github"`:**
-   `owner`: The GitHub username or organization that owns the repository.
-   `repo`: The name of the repository on GitHub.
-   `branch`: The default branch to use (e.g., "main", "master").
-   `pat_alias`: The alias of the GitHub PAT (from the `credentials` section) to use for this repository.
-   `path_to_kb`: Optional. A relative path within the repository to treat as the root of the knowledge base. Defaults to `.` (the repository root).

## Environment Variables

The MCP server might also use environment variables for certain configurations, especially for services it integrates with (like OpenAI).

Common environment variables:
-   `OPENAI_API_KEY`: Your API key for OpenAI, required for LLM-based knowledge base generation.
-   `OPENAI_MODEL`: (Optional) The specific OpenAI model to use (e.g., `gpt-4o`, `gpt-3.5-turbo`). Defaults to a model specified in the server code if not set.
-   `OPENAI_ENDPOINT`: (Optional) If you need to use a custom OpenAI API endpoint.

These can typically be set in a `.env` file in the project root (ensure `.env` is in your `.gitignore`) or directly in your shell environment.

Example `.env` file:
```
OPENAI_API_KEY=sk-xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
OPENAI_MODEL=gpt-4o
```

## Running the Server

-   **Development Mode** (often with hot-reloading):
    ```bash
    npm run dev
    ```
    (Check your `package.json` for the exact script name, it might also be `start:dev` or similar).

-   **Production Mode**:
    First, build the application:
    ```bash
    npm run build
    ```
    Then, start the server (the command might vary based on your project's setup, often it's):
    ```bash
    npm start
    ```
    or
    ```bash
    node dist/server.js 
    ```
    (or similar, pointing to the main built JavaScript file).

## Restarting the Server

You'll need to restart the MCP server if you:
-   Make changes to `config.local.json`.
-   Modify environment variables.
-   Update server-side code (unless you are running in a development mode with automatic restarts/hot-reloading).

Simply stop the running server (usually `Ctrl+C` in the terminal) and start it again using the appropriate run command.
