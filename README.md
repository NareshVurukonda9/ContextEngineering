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
    │   ├── CodeDemo.ipynb
    │   └── data/
    ├── Segment2/
    ├── Segment3/
    └── Segment4/
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
(Under development)

## Installation

### Prerequisites
- Python 3.9 or higher
- OpenAI API key

### Setup

1. **Clone the repository:**
   ```bash
   git clone https://github.com/NareshVurukonda9/ContextEngineering.git
   cd ContextEngineering
   ```

2. **Create environment file:**
   Create a `.env` file in the workspace root:
   ```
   OPENAI_API_KEY=your_api_key_here
   ```

3. **Install dependencies:**
   The required packages will be installed automatically when you run the notebooks:
   - `langchain-core`
   - `langchain-openai`
   - `langchain-community`
   - `python-dotenv`
   - `chromadb`
   - `pypdf`

## Running the Notebooks

Each notebook is standalone and builds on concepts from previous segments. Start with Day1/Segment1 and progress through in order.

1. Open a notebook in VS Code
2. Configure the notebook kernel (if prompted)
3. Run cells sequentially from top to bottom
4. Follow along with the code comments and exercises

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
- Set `OPENAI_API_KEY` environment variable
- Verify API key is valid and has sufficient credits

### Missing Dependencies
- Run notebook cells to auto-install packages
- Or manually: `pip install langchain-openai langchain-community chromadb pypdf python-dotenv`

### Vector Database Issues
- Chroma stores data in-memory by default
- For persistent storage, specify a directory path
- Restart kernel to clear in-memory database

## Architecture Pattern: RAG (Retrieval-Augmented Generation)

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

## Resources

- [LangChain Documentation](https://python.langchain.com/)
- [OpenAI API Reference](https://platform.openai.com/docs/api-reference)
- [Chroma Vector Database](https://www.trychroma.com/)

## Author

Created by Naresh Vurukonda

## License

This project is provided as-is for educational purposes.

---

**Start with Day1/Segment1/CodeDemo.ipynb to begin learning context engineering!**
