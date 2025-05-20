# Memorg: Hierarchical Context Management System

Memorg is a sophisticated context management system designed to enhance the capabilities of Large Language Models (LLMs) by providing efficient context management, retrieval, and optimization.

## Features

- **Hierarchical Context Storage**: Organizes information in a Session → Conversation → Topic → Exchange hierarchy
- **Intelligent Context Management**: Prioritizes and compresses information based on relevance and importance
- **Efficient Retrieval**: Combines keyword, semantic, and temporal search capabilities
- **Context Window Optimization**: Manages token usage and creates optimized prompts
- **Working Memory Management**: Efficiently allocates and manages token budgets

## Architecture

The system consists of several key components:

1. **Context Store**: Manages the storage and retrieval of context data
2. **Context Manager**: Handles prioritization, compression, and working memory
3. **Retrieval System**: Processes queries and ranks results
4. **Context Window Optimizer**: Optimizes context for LLM consumption

## Installation

1. Clone the repository:
```bash
git clone https://github.com/yourusername/memorg.git
cd memorg
```

2. Create a virtual environment:
```bash
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
```

3. Install dependencies:
```bash
pip install -r requirements.txt
```

## Usage

Basic usage example:

```python
from app.main import MemorgSystem
from app.storage.sqlite_storage import SQLiteStorageAdapter
from app.vector_store.usearch_vector_store import USearchVectorStore
from openai import AsyncOpenAI

async def main():
    # Initialize the system with actual storage and vector store implementations
    storage = SQLiteStorageAdapter("memorg.db")
    vector_store = USearchVectorStore("memorg.db")
    openai_client = AsyncOpenAI()
    
    system = MemorgSystem(storage, vector_store, openai_client)
    
    # Create a session
    session = await system.create_session("user123", {"max_tokens": 4096})
    
    # Start a conversation
    conversation = await system.start_conversation(session.id)
    
    # Create a topic and add exchanges
    topic = await system.context_store.create_topic(conversation.id, "Initial Discussion")
    exchange = await system.add_exchange(
        topic.id,
        "Hello, how can you help me?",
        "I'm here to assist you with any questions or tasks you have."
    )
    
    # Search context
    results = await system.search_context("help")
    
    # Get memory usage
    memory_usage = await system.get_memory_usage()

if __name__ == "__main__":
    import asyncio
    asyncio.run(main())
```

## Components

### Context Store

The Context Store manages the hierarchical storage of context data:
- Sessions: Top-level containers for user interactions
- Conversations: Groups of related exchanges
- Topics: Specific subjects within conversations
- Exchanges: Individual message pairs

### Context Manager

The Context Manager handles:
- Prioritization of information based on recency and importance
- Compression of content while preserving key information
- Working memory allocation and management

### Retrieval System

The Retrieval System provides:
- Query processing with entity recognition
- Multi-factor relevance scoring
- Hybrid search capabilities

### Context Window Optimizer

The Context Window Optimizer:
- Summarizes content while preserving important entities
- Optimizes token usage
- Creates context-aware prompt templates

## Development

### Running Tests

```bash
pytest
```

### Code Style

The project uses:
- Black for code formatting
- isort for import sorting
- mypy for type checking

Run the formatters:
```bash
black .
isort .
mypy .
```

## Contributing

1. Fork the repository
2. Create a feature branch
3. Commit your changes
4. Push to the branch
5. Create a Pull Request

## License

This project is licensed under the MIT License - see the LICENSE file for details. 