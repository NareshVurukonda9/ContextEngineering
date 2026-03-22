# Context Engineering

A comprehensive guide and implementation of context engineering techniques for Large Language Models (LLMs). This project demonstrates how to effectively construct, manage, and optimize contextual information to improve LLM outputs for real-world applications.

## Overview

Context Engineering focuses on the strategic preparation and delivery of contextual information to language models. Rather than relying solely on the model's base knowledge, context engineering enables more accurate, domain-specific, and reliable responses by providing relevant information at inference time.

## Project Structure

```
ContextEngineering/
├── Day1/
│   ├── Segment1/
│   │   └── CodeDemo.ipynb          # LLM Initialization & Prompting
│   ├── Segment2/
│   │   ├── CodeDemo.ipynb          # Document Ingestion Pipeline
│   │   └── data/
│   │       ├── api_records.json
│   │       └── release_notes.md
│   ├── Segment3/
│   │   ├── CodeDemo.ipynb          # Vector Embeddings & Retrieval
│   │   └── Exercise.ipynb
│   └── Segment4/
│       ├── CodeDemo.ipynb          # Context Routing & Ranking
│       └── Exercise.ipynb
└── Day2/
    ├── Segment1/
    │   ├── CodeDemo.ipynb          # RAG Pipeline Implementation
    │   ├── Exercise.ipynb
    │   └── data/
    ├── Segment2/
    │   ├── CodeDemo.ipynb          # Token Management & Context Compression
    │   └── Exercise.ipynb
    ├── Segment3/
    │   ├── CodeDemo.ipynb          # Multi-Source Context Integration
    │   └── Exercise.ipynb
    └── Segment4/
        ├── CodeDemo.ipynb          # Advanced RAG Techniques
        └── Exercise.ipynb
```

## Key Topics

### Day 1: Fundamentals of Context Engineering

**Segment 1: LLM Initialization & Prompting**
- Initialize OpenAI models (GPT-4o)
- Craft effective system and domain-level context
- Understand role-based prompting strategies

**Segment 2: Document Ingestion & Processing**
- Load documents from multiple formats (JSON, Markdown, PDF)
- Validate and clean document content
- Enrich metadata for better retrieval
- Build a document ingestion pipeline

**Segment 3: Vector Embeddings & Retrieval**
- Generate embeddings using OpenAI's embedding models
- Store documents in vector databases (Chroma)
- Implement semantic similarity search
- Create retriever interfaces for flexible document access

**Segment 4: Context Routing & Ranking**
- Retrieve candidate documents based on query similarity
- Rank documents by multiple factors (similarity, priority, recency)
- Resolve topic conflicts to avoid redundant context
- Route queries to the most relevant documents

### Day 2: Advanced Applications

**Segment 1: RAG Pipeline Implementation**
- Build complete Retrieval-Augmented Generation (RAG) pipelines
- Load and chunk documents for optimal retrieval
- Create and query vector stores with embeddings
- Generate context-aware responses with LLMs

**Segment 2: Token Management & Context Compression**
- Count tokens in prompts and responses using tiktoken
- Implement sliding window memory management
- Compress context while preserving key information
- Manage conversation history with token limits
- Optimize context window usage for cost and performance

**Segment 3: Multi-Source Context Integration**
- Integrate context from databases, documents, and APIs
- Implement multi-factor ranking systems
- Weight different context sources appropriately
- Handle conflicts between multiple context sources
- Build adaptive context selection strategies

**Segment 4: Advanced RAG Techniques**
- Hybrid retrieval strategies combining multiple approaches
- Query expansion and rewriting techniques
- Cross-encoder re-ranking for improved precision
- Dynamic context window management
- Production-ready RAG system design

## Installation

### Prerequisites
- Python 3.9 or higher
- OpenAI API key (see section below for setup instructions)
- pip or conda package manager

### Getting Your OpenAI API Key

1. **Create an OpenAI Account:**
   - Go to [OpenAI Platform](https://platform.openai.com)
   - Click "Sign up" if you don't have an account
   - Complete the registration and verification process

2. **Generate API Key:**
   - Navigate to [API Keys](https://platform.openai.com/account/api-keys)
   - Click "Create new secret key"
   - Copy the key immediately (you won't be able to see it again)
   - Store it securely

3. **Set Up Billing:**
   - Go to [Billing Settings](https://platform.openai.com/account/billing/overview)
   - Add a payment method
   - Set usage limits to prevent unexpected charges
   - Monitor your usage in the Usage dashboard

4. **Add to Your Project:**
   - Create a `.env` file in your workspace root:
     ```
     OPENAI_API_KEY=sk-your-key-here
     ```
   - Keep this file secure and never commit it to version control
   - Add `.env` to your `.gitignore` file

### Setup

1. **Clone the repository:**
   ```bash
   git clone https://github.com/NareshVurukonda9/ContextEngineering.git
   cd ContextEngineering
   ```

2. **Create environment file:**
   Create a `.env` file in the workspace root (see API key section above):
   ```
   OPENAI_API_KEY=your_api_key_here
   ```

3. **Install dependencies:**
   The required packages will be installed automatically when you run the notebooks. Manual installation:
   ```bash
   pip install langchain-core langchain-openai langchain-community python-dotenv chromadb pypdf tiktoken
   ```
   
   **Package descriptions:**
   - `langchain-core`: Core LangChain framework
   - `langchain-openai`: OpenAI integration for LangChain
   - `langchain-community`: Community integrations (loaders, vectorstores, etc.)
   - `python-dotenv`: Environment variable loading
   - `chromadb`: Vector database for embeddings
   - `pypdf`: PDF document processing
   - `tiktoken`: Token counting for OpenAI models

## Learning Path

### Getting Started (Recommended)
1. **Day1/Segment1**: Learn LLM basics and prompting patterns
2. **Day1/Segment2**: Understand document ingestion and processing
3. **Day1/Segment3**: Master vector embeddings and semantic search
4. **Day1/Segment4**: Implement context routing and ranking

### Building RAG Systems
5. **Day2/Segment1**: Create complete RAG pipelines
6. **Day2/Segment2**: Optimize context with token management
7. **Day2/Segment3**: Integrate multiple context sources

### Advanced Topics
8. **Day2/Segment4**: Advanced RAG techniques and optimization

### Tips
- Complete each segment's exercises before moving to the next
- Experiment with different parameters and thresholds
- Test with your own documents and queries
- Review comments in code for detailed explanations

## Key Concepts

### 1. Role-Based Prompting
Define a system message that establishes the model's role and responsibilities:
```python
system_context = """You are a senior product strategist.
Provide structured, concise, and practical recommendations."""
```

### 2. Domain Context
Provide relevant background information about your domain:
```python
domain_context = """
The product is a B2B AI SaaS platform sold to mid-size enterprises.
Competitors use usage-based and tiered pricing."""
```

### 3. Document Pipeline
A complete workflow for ingesting, validating, and enriching documents:
- **Ingest**: Load from multiple file formats
- **Validate**: Filter out low-quality or incomplete documents
- **Enrich**: Add semantic metadata (topics, lengths, etc.)

### 4. Vector Retrieval
Convert documents to embeddings and retrieve semantically similar content:
- Uses OpenAI's `text-embedding-3-small` model
- Stores vectors in Chroma vector database
- Supports k-nearest neighbor similarity search

### 5. Context Ranking & Routing
Multi-factor ranking system considering:
- **Semantic Similarity**: How relevant is the document to the query?
- **Priority**: How important is this document?
- **Recency**: How fresh is the information?

Weighted formula:
```
final_score = (0.5 × similarity) + (0.3 × priority) + (0.2 × recency)
```

### 6. Conflict Resolution
Avoid redundant context by limiting results to one document per topic:
- Retrieve candidates by similarity
- Rank by combined factors
- Select top result per unique topic

## Best Practices

1. **System Messages**: Establish clear roles and guidelines
2. **Document Quality**: Ensure source documents are high-quality and relevant
3. **Metadata**: Rich metadata enables better filtering and ranking
4. **Testing**: Validate context quality against sample queries
5. **Iteration**: Refine prompts and ranking weights based on results

## Common Issues & Solutions

### Missing API Key
- Ensure `.env` file exists in workspace root
- Set `OPENAI_API_KEY` environment variable with valid key
- Verify API key is valid at [OpenAI API Keys](https://platform.openai.com/account/api-keys)
- Check that your OpenAI account has available credits
- API keys start with `sk-` prefix

### Missing Dependencies
- Run notebook cells to auto-install packages
- Or manually: `pip install langchain-openai langchain-community chromadb pypdf python-dotenv tiktoken`
- Use `pip list` to verify all packages are installed
- Consider using a virtual environment: `python -m venv venv && source venv/bin/activate`

### Vector Database Issues
- Chroma stores data in-memory by default (temporary)
- For persistent storage, specify a directory: `Chroma(persist_directory="./chroma_db")`
- Restart kernel to clear in-memory database
- Delete `__pycache__` and `.chroma` directories if experiencing corruption

### Token/Rate Limits
- Monitor token usage at [OpenAI Usage Dashboard](https://platform.openai.com/account/usage/overview)
- Set rate limits and billing limits in account settings
- Use `tiktoken` to count tokens before API calls
- Implement retry logic with exponential backoff for rate limit errors

### Environment Variable Issues
- Ensure `.env` file is in the correct directory (workspace root)
- Add `.env` to `.gitignore` to prevent accidental commits
- Restart VS Code or kernel after creating/modifying `.env`
- Use `load_dotenv(override=True)` to force reload

## Architecture Patterns

### RAG (Retrieval-Augmented Generation)
The core pattern used throughout this course:
```
Query Input
    ↓
[Retrieval] → Get relevant documents from vector store
    ↓
[Ranking]   → Score documents by multiple factors
    ↓
[Conflict Resolution] → Select diverse, high-quality context
    ↓
[LLM Prompt] → Combine system message + context + query
    ↓
[Response] → Model-generated answer with grounded information
```

### Token Management Pattern
```
User Query + System Prompt
    ↓
[Count Tokens] → Check token count
    ↓
[Within Limit?] → Yes: Proceed | No: Compress
    ↓
[Compression] → Summarize less important context
    ↓
[LLM Call] → Send optimized context to model
```

### Multi-Source Integration Pattern
```
Query
    ↓
[Database Query] → Fetch structured data
[Document Search] → Retrieve from vectors
[API Call] → Get real-time information
    ↓
[Ranking] → Score by relevance, priority, recency
    ↓
[Selection] → Pick top results, avoid duplicates
    ↓
[Fusion] → Combine sources intelligently
    ↓
[LLM Response] → Generate answer with integrated context
```

## Resources

- [OpenAI Platform](https://platform.openai.com) - API keys and billing
- [LangChain Documentation](https://python.langchain.com/) - Framework documentation
- [OpenAI API Reference](https://platform.openai.com/docs/api-reference) - Complete API docs
- [Chroma Vector Database](https://www.trychroma.com/) - Vector store documentation
- [tiktoken](https://github.com/openai/tiktoken) - Token counting library

## Troubleshooting Guide

### API Key Issues
```python
# Verify your API key is loaded correctly
import os
from dotenv import load_dotenv
load_dotenv()
print("API Key loaded:", bool(os.getenv("OPENAI_API_KEY")))
print("Key starts with sk-:", os.getenv("OPENAI_API_KEY", "").startswith("sk-"))
```

### Token Counting
```python
import tiktoken
encoding = tiktoken.encoding_for_model("gpt-4o")
tokens = len(encoding.encode("your text here"))
print(f"Token count: {tokens}")
```

### Verify Chroma Installation
```python
from langchain_community.vectorstores import Chroma
print("Chroma imported successfully")
```

## Author

Created by Naresh Vurukonda; Shivendra Srivastava

## Contributing

Contributions are welcome! Feel free to submit issues and enhancements.

## License

This project is provided as-is for educational purposes.

## Acknowledgments

- OpenAI for GPT models and embeddings API
- LangChain community for the framework
- Chroma for the vector database

---

**Start with Day1/Segment1/CodeDemo.ipynb to begin learning context engineering!**

**Last Updated:** March 21, 2026
