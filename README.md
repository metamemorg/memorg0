# Memorg0: Hierarchical Context Management System

Memorg0 is a sophisticated context management system designed to enhance the capabilities of Large Language Models (LLMs) by providing efficient context management, retrieval, and optimization.

## Features

- **Hierarchical Context Storage**: Organizes information in a Session → Conversation → Topic → Exchange hierarchy
- **Intelligent Context Management**: Prioritizes and compresses information based on relevance and importance
- **Efficient Retrieval**: Combines keyword, semantic, and temporal search capabilities
- **Context Window Optimization**: Manages token usage and creates optimized prompts
- **Working Memory Management**: Efficiently allocates and manages token budgets

## Specifications

For detailed specifications, please refer to:
- [Technical Specification](specifications/technial.md) - Core architecture and implementation details
- [Usage Guide](specifications/usage.md) - Detailed usage patterns and examples
- [Analysis](specifications/analysis.md) - System analysis and design decisions

## Installation

1. Clone the repository:
```bash
git clone https://github.com/metamemorg/memorg0.git
cd memorg
```

2. Install using Poetry:
```bash
poetry install
```

3. Set up your OpenAI API key:
```bash
export OPENAI_API_KEY="your-api-key-here"
```

## Library Usage

Memorg0 can be used as a library in your Python projects. Here's how to integrate it:

```python
from app.main import MemorgSystem
from app.storage.sqlite_storage import SQLiteStorageAdapter
from app.vector_store.usearch_vector_store import USearchVectorStore
from openai import AsyncOpenAI

async def setup_memorg():
    # Initialize components
    storage = SQLiteStorageAdapter("memorg.db")
    vector_store = USearchVectorStore("memorg.db")
    openai_client = AsyncOpenAI()
    
    # Create system instance
    system = MemorgSystem(storage, vector_store, openai_client)
    
    # Create a session with token budget
    session = await system.create_session("user123", {"max_tokens": 4096})
    
    # Start a conversation
    conversation = await system.start_conversation(session.id)
    
    # Create a topic
    topic = await system.context_store.create_topic(conversation.id, "Project Discussion")
    
    # Add an exchange (interaction)
    exchange = await system.add_exchange(
        topic.id,
        "What are the key features?",
        "The system provides hierarchical storage, intelligent context management, and efficient retrieval."
    )
    
    # Search through context
    results = await system.search_context("key features")
    
    # Monitor memory usage
    memory_usage = await system.get_memory_usage()
    return system, session, conversation, topic
```

## CLI Exploration

The CLI provides an interactive way to explore and manage your memory system:

```bash
# Start the CLI
poetry run python -m app.cli
```

Available commands in the CLI:
- `help`: Show available commands
- `new`: Start a new conversation
- `search`: Search through conversation history
- `memory`: Show memory usage statistics
- `exit`: Exit the chat

Example CLI session:
```bash
$ poetry run python -m app.cli
Welcome to Memorg CLI Chat!
Type 'help' for available commands or start chatting.

You: help
Available Commands:
- help: Show this help message
- new: Start a new conversation
- search: Search through conversation history
- memory: Show memory usage statistics
- exit: Exit the chat

You: memory
Memory Usage:
Total Tokens: 1,234
Active Items: 50
Compressed Items: 10
Vector Count: 60
Index Size: 2.5 MB

You: search
Enter search query: key features
Score  Type        Content
0.92   SEMANTIC    The system provides hierarchical storage...
0.85   KEYWORD     Intelligent context management and...
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
poetry run pytest
```

### Code Style

The project uses:
- Black for code formatting
- isort for import sorting
- mypy for type checking

Run the formatters:
```bash
poetry run black .
poetry run isort .
poetry run mypy .
```

## Contributing

1. Fork the repository
2. Create a feature branch
3. Commit your changes
4. Push to the branch
5. Create a Pull Request

## License

This project is licensed under the MIT License - see the LICENSE file for details. 
