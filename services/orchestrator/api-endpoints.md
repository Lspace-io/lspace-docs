# API Endpoints for Orchestrator Service

## Process Document

```
POST /api/orchestrator/process-document
```

Processes and integrates document content into the knowledge base. Handles classification, structuring, and duplicate detection.

## Organize Repository

```
POST /api/orchestrator/organize-repository/:repositoryId
```

Reorganizes content for better structure and discoverability.

## Generate Repository Summary

```
GET /api/orchestrator/repository-summary/:repositoryId
```

Generates a summary of the repository's knowledge base content.

## Prune Repository

```
POST /api/orchestrator/prune-repository/:repositoryId
```

Removes obsolete or redundant content from the repository.

---

*Source: [Orchestrator Document](/.lspace/raw_inputs/2f4a943a-8b3c-404c-8da9-29730bc33a0b-orchestrator.md)*