# Knowledge Base Structure

## Overview

The Lspace API is designed to support a repository architecture where raw input documents are stored in a protected subdirectory within `/.lspace/`, and a model-generated knowledge base resides in the repository root, with `/README.md` as its main entry point.

## Repository Structure

```
repository/
├── .lspace/            # System metadata and raw inputs directory (LLM must not modify)
│   ├── timeline.json    # Operation history for source files from .lspace/raw_inputs/
│   └── raw_inputs/      # Raw source document storage
│       └── doc1.md      # Original documents with metadata
│
├── README.md           # Main entry point & summary of the knowledge base (LLM-managed)
├── topic1/             # Example KB content directory in root (LLM-managed)
│   └── concept1.md     # Synthesized from .lspace/raw_inputs/ documents
└── topic2/             # Example KB content directory in root (LLM-managed)
    └── concept2.md
```

## Dual-Component Architecture Focus

### 1. Raw Input Storage (`/.lspace/raw_inputs/`)

This component handles the storage and tracking of original source content:

- Documents are ingested and stored within the `/.lspace/raw_inputs/` directory.
- All operations on these source documents are tracked in `/.lspace/timeline.json`.
- Raw files in `/.lspace/raw_inputs/` are preserved as the source material for the knowledge base.
- The LLM is strictly prohibited from modifying the entire `/.lspace/` directory, including `raw_inputs/`.

### 2. Knowledge Base / Synthesis Layer (Repository Root)

This component, managed by the LLM, creates a Wikipedia-like knowledge structure directly in the repository root, using content from `/.lspace/raw_inputs/`:

- **Entry Page (`/README.md`)**: The central landing page in the root, introducing the repository's scope and content. This is the primary summary file managed by the LLM.
- **Synthesized Topics**: Articles and directories in the root that aggregate information across multiple raw documents from `/.lspace/raw_inputs/`.
- **Cross-References**: Links between related concepts and topics within the root content.
- **Source References**: Citations and clear references back to the raw source documents in `/.lspace/raw_inputs/`.
- **Hierarchical Structure**: Topics organized in an intuitive browsable structure within the root (e.g., `/guides/`, `/reference/`).

## Processing Flow

1. When a document is ingested (e.g., uploaded or via stream):
   - The document processing pipeline stores the raw document into `/.lspace/raw_inputs/`.
   - A new knowledge base generation process is triggered:
     - The LLM reads the new document from `/.lspace/raw_inputs/`.
     - It updates relevant knowledge base articles in the repository root (or creates new ones).
     - It critically updates the `/README.md` file to reflect new content and serve as the main summary.
     - It maintains cross-references between articles in the root.

2. When a repository is organized (a future feature that might affect `/.lspace/raw_inputs/`):
   - After raw files in `/.lspace/raw_inputs/` might be reorganized by a system process (not the KB LLM).
   - The knowledge base in the root may need to be regenerated or updated by the LLM to reflect these changes to source material.

## Knowledge Base Components (in Repository Root)

### Entry Page (`/README.md`)

The `/README.md` in the root serves as the "homepage" of the knowledge base, providing:

- A title and description of the knowledge domain.
- An overview of key topics and concepts covered in the repository root.
- Navigation links to main sections/directories within the root.
- Recent updates or changes to the knowledge base.
- Optionally, statistics about the knowledge base.

### Topic Pages & Directories (in Repository Root)

Topic pages and directories within the root provide deeper dives into specific subjects:

- Comprehensive information synthesized from multiple documents in `/.lspace/raw_inputs/`.
- Images, diagrams, or other visual elements when available (e.g., in an `/images/` directory in the root).
- References to source documents in `/.lspace/raw_inputs/`.
- Related topics and concepts, linked within the root content.
- Hierarchical organization of subtopics within directories in the root.

## Implementation Considerations

### Knowledge Base Generation (in Root)

The knowledge base in the root can be generated using several approaches:

1. **Full Regeneration**: Rebuild the entire knowledge base with each new document
   - Pros: Ensures complete consistency
   - Cons: Computationally expensive for large repositories

2. **Incremental Updates**: Update only affected articles when new content is added
   - Pros: More efficient for large repositories
   - Cons: May miss cross-document connections

3. **Hybrid Approach**: Incremental updates with periodic full regeneration
   - Pros: Balances efficiency and completeness
   - Cons: More complex to implement

### LLM Integration

The LLM service needs to support knowledge base generation in the root directory:

1. **Content Extraction**: Identify key information, facts, and concepts from raw documents in `/.lspace/raw_inputs/`.
2. **Synthesis**: Combine information from multiple sources into coherent articles in the repository root.
3. **Summarization**: Create concise summaries, especially for the main `/README.md`.
4. **Cross-Referencing**: Identify relationships between concepts within the root content.
5. **Citation**: Track sources of information from `/.lspace/raw_inputs/` for proper attribution in the root content.

### API Endpoints

API endpoints are needed to support interacting with the knowledge base content in the repository root:

```
GET /api/repositories/:repositoryId/tree # Get file tree of the root (excluding .lspace)
GET /api/repositories/:repositoryId/readme # Get content of /README.md
GET /api/repositories/:repositoryId/file?path=<path_to_file_in_root>
POST /api/repositories/:repositoryId/regenerate # Trigger full KB regeneration in root
```

## User Interface Considerations

While the API focuses on the backend functionality, the eventual frontend (or a user browsing on GitHub) should experience:

1. **Direct KB Access**: The repository root is the knowledge base. `/README.md` is the entry point.
2. **Clear Source Tracking**: Clear references from synthesized content in the root back to source documents in `/.lspace/raw_inputs/` (though `/.lspace/raw_inputs/` itself might not be directly browsable in a simplified UI).
3. **Search**: Unified search across the knowledge base content in the root.

## Benefits

This root-based knowledge base approach provides several advantages:

1. **Preserves Raw Content**: Original documents remain intact and accessible in `/.lspace/raw_inputs/` for auditing and regeneration.
2. **Optimal GitHub UX**: The repository root provides an immediate, user-friendly knowledge base experience starting with `/README.md`.
3. **Enhances Understanding**: Synthesized content in the root provides higher-level understanding.

## Next Steps

To implement the knowledge base component in the repository root:

1. Ensure the repository structure (`/README.md`, `/.lspace/` containing `timeline.json` and `raw_inputs/`) is correctly initialized and managed.
2. Develop LLM prompts and services for knowledge extraction from `/.lspace/raw_inputs/` and synthesis into the repository root, with `/README.md` as the primary summary.
3. Create API endpoints for accessing and managing the knowledge base content in the root.
4. Implement knowledge base generation (populating the root) as part of the document ingestion pipeline.
5. Add regeneration capabilities for updating the knowledge base in the root.
