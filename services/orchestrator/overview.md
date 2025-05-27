# Orchestrator Service

The Orchestrator Service is a key component of the Lspace API that provides intelligent document processing, organization, and management capabilities using Large Language Models (LLMs).

## Overview

This service acts as a bridge between raw document content and the structured knowledge base:

- **Document classification and categorization**
- **Content structuring and formatting**
- **Duplicate detection**
- **Repository organization**
- **Content summarization**
- **Timeline tracking**

## Architecture

The service consists of two main components:

1. **LLM Service** - Supports AI-powered document processing.
2. **Orchestrator Service** - Coordinates repository operations and LLM tasks.

## Configuration

Requires OpenAI API key:
```
OPENAI_API_KEY=your_openai_api_key
OPENAI_MODEL=gpt-4o (default)
OPENAI_ENDPOINT=https://api.openai.com/v1/chat/completions (optional)
```

---

*Source: [Orchestrator Document](/.lspace/raw_inputs/2f4a943a-8b3c-404c-8da9-29730bc33a0b-orchestrator.md)*