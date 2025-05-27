# Lspace MCP Server Setup and Configuration

## Introduction

The Lspace Management and Control Plane (MCP) Server is the backend application that powers Lspace. It handles repository management, knowledge base generation, interaction with Git services (like GitHub), and exposes an API for clients to use. Proper setup and configuration are crucial for Lspace to function correctly.

## Prerequisites

- **Node.js**: A recent LTS version is recommended.
- **npm** or **yarn**: For managing project dependencies.
- **Git**: Required for interacting with Git repositories especially if you use the Git CLI-based GitHub adapter.

## Installation

1. Clone the repository and navigate into it.
2. Install dependencies using `npm install` or `yarn install`.

## Configuration

Setup `config.local.json` for sensitive info and config details. This file must remain outside version control.

**Example Configuration**:
```json
{
  "credentials": {
    "github_pats": [{
      "alias": "my_github_pat",
      "token": "ghp_xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
    }]
  },
  "repositories": [{
    "id": "your-unique-repo-id-1",
    "name": "My Local Documentation",
    "type": "local",
    "path": "/path/to/your/local/docs/repo"
  }, {
    "id": "your-unique-repo-id-2",
    "name": "My Project on GitHub",
    "type": "github",
    "owner": "your_github_username",
    "repo": "your_repository_name",
    "branch": "main",
    "pat_alias": "my_github_pat",
    "path_to_kb": "."
  }]
}
```

## Environment Variables

Environment variables may be set in a `.env` file and must include:
- `OPENAI_API_KEY`: for LLM-based KB generation.
- `OPENAI_MODEL`: Optional, specifies OpenAI model.
- `OPENAI_ENDPOINT`: Optional, for custom API endpoint.

## Running the Server

- Development Mode: `npm run dev`
- Production Mode:
  1. `npm run build`
  2. `npm start`

## Restarting the Server

After changes to the config, env variables, or code, restart the server by stopping it and starting again.

---

*Source: [./lspace/raw_inputs/b431ef7b-63da-4166-8dae-949c778ff3b1-mcp-server-setup.md]*