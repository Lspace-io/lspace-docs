# Lspace Official Docs

## Overview

This repository contains a structured knowledge base designed to synthesize information from raw input documents stored securely in the `/.lspace/raw_inputs/` directory, while the model-generated knowledge base resides in the repository root. Leveraging Lspace, this platform transforms dispersed documents into a unified, searchable repository, tackling challenges like information silos and outdated documentation while promoting collaboration.

The latest addition to our documentation is the **Lspace Server's Getting Started Guide**, providing detailed instructions on setting up and leveraging the intelligent capabilities of Lspace Server. It enables developers to integrate comprehensive knowledge management and AI orchestration into their workflows with ease.

## Repository Structure

```
repository/
├── .lspace/            # System-level raw inputs and metadata
│   ├── timeline.json    # Operation history for source files
│   └── raw_inputs/      # Storage for raw documents
├── README.md           # Main entry point & overview of the knowledge base
├── getting-started/    # Getting Started Guides
│   └── README.md       # Lspace Server Getting Started Guide
├── protocols-and-standards/ # Protocols and Standards Overview
│   └── understanding-mcp.md # Description of the Model Context Protocol and its integration
├── lspace-introduction/ # Lspace introduction and use cases
│   └── lspace-benefits-use-cases.md # Details on Lspace's benefits and use cases
├── mcp-server/         # MCP Server Setup and Configuration
│   └── setup-and-configuration.md # MCP server setup guide
├── services/           # Services directory
│   └── orchestrator/   # Orchestrator Service documentation
│       ├── overview.md
│       └── api-endpoints.md
├── api-guide/         # API Usage documentation
│   └── usage.md
├── knowledge-base-generation/ # KB Generation Internals
│   └── internals.md
└── licensing/         # Licensing strategy documentation
    └── licensing-strategy.md
```

## Knowledge Base Architecture
...

## Recent Updates
- Integrated the Licensing Strategy details from `ed184976-5677-4d36-b26d-1ee3849aae16-LSPACELicensingStrategy.txt`.
- Integrated MCP Server setup details from the document `b431ef7b-63da-4166-8dae-949c778ff3b1-mcp-server-setup.md`.
- Updated the structure based on insights from `5c08683c-23cb-4174-bc58-98f9b39ff139-knowledge-base-structure.md`.
- Added Orchestrator Service documentation based on `2f4a943a-8b3c-404c-8da9-29730bc33a0b-orchestrator.md`.
- Added API Usage Guide from the document `a9a3efae-8690-4138-a0dd-d1ac38cc413b-api-usage-guide.md`.
- Added new introductory section about Lspace benefits and use cases based on `lspace-benefits-use-cases.md`.
- Processed [Knowledge Base Generation Internals](./knowledge-base-generation/internals.md) detailing internal mechanisms.
- **Integrated Understanding of the Model Context Protocol (MCP) from `cadfcf2b-f72f-4218-a167-846123e40cee-UnderstandingtheModelContextProtocolMCP.txt`.**
- **Added Getting Started Guide for Lspace Server from `fc4b0887-95c3-4136-aab8-ffe75afe235d-LspaceServer-GettingStartedGuide.txt`.**