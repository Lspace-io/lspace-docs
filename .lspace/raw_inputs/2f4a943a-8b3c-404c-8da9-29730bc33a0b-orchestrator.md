# Orchestrator Service

The Orchestrator Service is a key component of the Lspace API that provides intelligent document processing, organization, and management capabilities using Large Language Models (LLMs).

## Overview

The Orchestrator Service acts as a bridge between raw document content (stored in `/.lspace/raw_inputs/`) and the structured knowledge base (the repository root, with `/README.md` as its entry point), handling tasks like:

- Document classification and categorization (of content from `/.lspace/raw_inputs/`)
- Content structuring and formatting (for the knowledge base in the root)
- Duplicate detection (across source documents or within the KB)
- Repository organization (e.g., of `/.lspace/raw_inputs/` or assisting LLM with root KB structure)
- Content summarization (primarily for `/README.md` and other KB articles)
- Timeline tracking of source document operations (in `/.lspace/timeline.json` based on `/.lspace/raw_inputs/` changes)

## Architecture

The Orchestrator Service consists of two main components:

1. **LLM Service** - Provides AI-powered utilities for document processing:
   - Document classification
   - Content structuring
   - Duplicate detection
   - Content organization
   - Repository summarization

2. **Orchestrator Service** - Coordinates repository operations and LLM functionality:
   - Document processing workflow
   - Repository organization
   - Content pruning and cleanup
   - Summary generation

## Configuration

The Orchestrator Service requires an OpenAI API key for LLM functionality:

```
OPENAI_API_KEY=your_openai_api_key
OPENAI_MODEL=gpt-4o (default)
OPENAI_ENDPOINT=https://api.openai.com/v1/chat/completions (optional)
```

## API Endpoints

### Process Document

```
POST /api/orchestrator/process-document
```

Processes a document (ingested into `/.lspace/raw_inputs/`) and triggers its integration into the knowledge base in the repository root. Handles classification, structuring, and duplicate detection. Also records the source document addition in the repository timeline.

**Request Body:**
```json
{
  "repositoryId": "uuid-of-repository",
  "content": "# Document Title

Document content...", // This content will be saved to /.lspace/raw_inputs/
  "user": "optional-user-identifier"
}
```

**Response:**
```json
{
  "rawInputPath": ".lspace/raw_inputs/some-unique-name.md", // Path where the input content was stored
  "knowledgeBasePath": "topic/machine-learning-basics.md", // Example path of a KB article generated/updated in root
  "readmeUpdated": true, // Indicates if /README.md was updated
  "category": "technical",
  "subcategory": "machine-learning",
  "title": "Machine Learning Basics",
  "tags": ["machine-learning", "ai", "tutorial"],
  "timelineEntry": {
    "timestamp": "2023-05-10T15:30:45Z",
    "operation": "add",
    "user": "user-identifier"
  }
}
```

### Organize Repository

```
POST /api/orchestrator/organize-repository/:repositoryId
```

Reorganizes the `/.lspace/raw_inputs/` directory content for better structure and discoverability (or assists LLM in organizing the root KB, TBD based on final design of this feature).

**Response:**
```json
{
  "moved": 2,
  "updated": 3,
  "created": 1,
  "unchanged": 5
}
```

### Generate Repository Summary

```
GET /api/orchestrator/repository-summary/:repositoryId
```

Generates a summary of the repository's knowledge base content (primarily from `/README.md` and other content in the root).

**Response:**
```json
{
  "title": "AI/ML Knowledge Base",
  "description": "A collection of notes on artificial intelligence and machine learning.",
  "topics": ["ai", "machine-learning"],
  "fileCount": 12,
  "mainCategories": ["ai", "ml"],
  "lastUpdated": "2023-05-10T15:30:45Z"
}
```

### Prune Repository

```
POST /api/orchestrator/prune-repository/:repositoryId
```

Removes obsolete or redundant content from the repository.

**Response:**
```json
{
  "deleted": 3,
  "merged": 2,
  "unchanged": 7
}
```

## Usage Example

```javascript
// Process a new document
const result = await axios.post('http://localhost:3000/api/orchestrator/process-document', {
  repositoryId: 'repository-uuid',
  content: '# Machine Learning Basics\n\nThis is an introduction to ML concepts...'
});

// The document is now classified, structured, and added to the repository
console.log(`Document added at: ${result.data.path}`);

// Generate a summary of the repository
const summary = await axios.get(`http://localhost:3000/api/orchestrator/repository-summary/repository-uuid`);
console.log(`Repository contains ${summary.data.fileCount} files on topics: ${summary.data.topics.join(', ')}`);
```

## Benefits

- **Automated Organization** - Intelligently categorize and structure content
- **Consistency** - Maintain consistent document formatting and structure
- **Duplicate Prevention** - Detect and merge similar content
- **Improved Discoverability** - Better organization leads to easier content discovery
- **Content Pruning** - Identify and remove obsolete information
- **Timeline Tracking** - Maintain a record of document operations for visibility and auditing

## Timeline Tracking

The Orchestrator Service maintains a timeline of source document operations in a `/.lspace/timeline.json` file within each repository. This provides a historical record of all document additions, updates, and other operations affecting files in `/.lspace/raw_inputs/`.

### Timeline Structure

```json
{
  "entries": [
    {
      "timestamp": "2023-05-10T15:30:45Z",
      "operation": "add",
      "path": ".lspace/raw_inputs/machine-learning-basics.md",
      "title": "Machine Learning Basics",
      "user": "user-identifier",
      "category": "technical",
      "tags": ["machine-learning", "ai", "tutorial"],
      "url": "/api/repositories/repo-id/files/.lspace/raw_inputs/machine-learning-basics.md"
    },
    {
      "timestamp": "2023-05-11T09:12:33Z",
      "operation": "update",
      "path": ".lspace/raw_inputs/machine-learning-basics.md",
      "title": "Machine Learning Basics",
      "user": "another-user",
      "category": "technical",
      "tags": ["machine-learning", "ai", "tutorial"]
    }
  ]
}
```

### Timeline API

```
GET /api/repositories/:id/timeline
```

Retrieves the timeline data in a paginated format with optional filtering.

**Query Parameters:**
- `limit` - Number of entries to return (default: 20)
- `offset` - Pagination offset (default: 0)
- `operation` - Filter by operation type (add, update, delete, move)
- `user` - Filter by user identifier
- `category` - Filter by document category
- `tag` - Filter by document tag
- `startDate` - Filter entries after this date
- `endDate` - Filter entries before this date

**Response:**
```json
{
  "total": 150,
  "limit": 20,
  "offset": 0,
  "entries": [
    {
      "timestamp": "2023-05-10T15:30:45Z",
      "operation": "add",
      "path": ".lspace/raw_inputs/machine-learning-basics.md",
      "title": "Machine Learning Basics",
      "user": "user-identifier",
      "category": "technical",
      "tags": ["machine-learning", "ai", "tutorial"],
      "url": "/api/repositories/repo-id/files/.lspace/raw_inputs/machine-learning-basics.md"
    },
    // More entries...
  ]
}
```
