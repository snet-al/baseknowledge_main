# 🤖 AI Agents – Complete Learning Path

A structured roadmap to learn how to build intelligent agents using LLMs and modern frameworks.

---

## 1. LLM Providers & API Integration

Learn to interact with major LLM providers through their APIs.

### Providers to Master

- **OpenAI**

  - API authentication & setup
  - Chat completions API
  - Streaming responses
  - Rate limits & error handling
  - Model selection (GPT-4, GPT-3.5, etc.)

- **Google Gemini**

  - Google AI Studio setup
  - Gemini API integration
  - Multimodal capabilities
  - Safety settings & filters

- **Claude (Anthropic)**
  - Anthropic API setup
  - Claude models (Opus, Sonnet, Haiku)
  - System prompts & messages
  - Token limits & usage

### Key Concepts

- API keys & environment variables
- Request/response formats
- Error handling & retries
- Cost management & token counting
- Model comparison & selection

---

## 2. Prompt Engineering for Specific Problems

Master the art of crafting effective prompts to solve real-world problems.

### Topics

- **Prompt Structure**

  - System vs user messages
  - Role definition
  - Context setting
  - Output formatting

- **Prompt Patterns**

  - Zero-shot prompting
  - Few-shot prompting
  - Chain-of-thought reasoning
  - Self-consistency
  - Tree of thoughts

- **Problem-Specific Prompts**

  - Text generation
  - Code generation
  - Data extraction
  - Summarization
  - Translation
  - Question answering

- **Prompt Optimization**
  - Iterative refinement
  - A/B testing prompts
  - Temperature & sampling parameters
  - Token efficiency

---

## 3. Tool Calls & Function Calling

Enable LLMs to interact with external tools and APIs.

### Concepts

- **Function Calling**

  - Defining tool schemas
  - JSON schema for functions
  - Tool selection by LLM
  - Parsing tool responses

- **Tool Types**

  - Web search APIs
  - Database queries
  - Calculator functions
  - Code execution
  - File operations
  - Custom APIs

- **Implementation**

  - OpenAI function calling
  - Anthropic tool use
  - Gemini function calling
  - Error handling in tool execution
  - Tool result formatting

- **Best Practices**
  - Tool description clarity
  - Parameter validation
  - Security considerations
  - Tool chaining

---

## 4. Chaining LLM Calls

Learn to chain multiple LLM calls together to solve complex problems step by step.

### Chain Types

- **Sequential Chains**

  - Output of one call feeds into the next
  - Breaking down complex tasks
  - Passing context between calls
  - Intermediate result processing

- **Parallel Chains**

  - Concurrent API requests
  - Result aggregation
  - Fan-out / fan-in patterns
  - Independent subtask processing

- **Conditional Chains**

  - Branching based on LLM output
  - Dynamic routing
  - Fallback chains
  - Error handling paths

- **Iterative Chains**
  - Self-correction loops
  - Refinement until criteria met
  - Validation & retry patterns
  - Convergence strategies

### Chain Patterns

- **Prompt Chaining**

  - Decompose complex prompts
  - Step-by-step reasoning
  - Context accumulation
  - Result synthesis

- **Transform Chains**

  - Input → Process → Output
  - Data extraction pipelines
  - Multi-stage summarization
  - Translation chains

- **Router Chains**
  - Classify input type
  - Route to specialized chains
  - Expert selection
  - Dynamic chain selection

### Implementation

- Passing state between calls
- Context window management
- Async/await patterns
- Caching intermediate results
- Cost optimization
- Progress tracking & logging

---

## 5. Vector Databases & RAG (Retrieval-Augmented Generation)

Enable agents to access and retrieve information from knowledge bases using vector databases.

### Core Concepts

- **Embeddings**

  - What are embeddings
  - Embedding models (OpenAI, Cohere, Sentence Transformers)
  - Vector dimensions & similarity
  - Embedding generation

- **Vector Databases**
  - What are vector databases
  - When to use them
  - Similarity search (cosine, dot product, euclidean)
  - Indexing strategies

### Popular Vector Databases

- **Pinecone**

  - Managed vector database
  - Setup & configuration
  - Index management
  - Querying & filtering

- **Chroma**

  - Open-source vector store
  - Local & server deployment
  - Collection management
  - Persistence

- **Weaviate**

  - Open-source vector database
  - GraphQL API
  - Schema definition
  - Multi-tenant support

- **Qdrant**

  - High-performance vector search
  - Filtering capabilities
  - Payload management
  - Clustering & replication

- **FAISS (Facebook AI Similarity Search)**

  - In-memory vector search
  - Index types (Flat, IVF, HNSW)
  - GPU acceleration
  - Integration with other systems

- **Milvus**
  - Distributed vector database
  - Scalability features
  - Collection management
  - Production deployment

### RAG Implementation

- **RAG Pipeline**

  - Document ingestion & chunking
  - Embedding generation
  - Vector storage
  - Query processing
  - Context retrieval
  - Response generation

- **Chunking Strategies**

  - Fixed-size chunks
  - Semantic chunking
  - Overlapping chunks
  - Document-aware chunking

- **Retrieval Strategies**

  - Top-k retrieval
  - Re-ranking
  - Hybrid search (vector + keyword)
  - Metadata filtering
  - Query expansion

- **Integration with Agents**
  - RAG as a tool for agents
  - Context window management
  - Source citation
  - Handling retrieval failures

### Best Practices

- Chunk size optimization
- Embedding model selection
- Index configuration
- Query performance tuning
- Cost management
- Accuracy vs speed trade-offs

---

## 6. Agent Frameworks: LangChain, CrewAI, AutoGen

Build reactive agents using popular frameworks. Now that you understand the raw patterns, leverage frameworks to accelerate development.

### LangChain

- **Core Concepts**

  - Chains & agents
  - Memory management
  - Document loaders
  - Vector stores & retrievers
  - Output parsers

- **Agent Types**

  - ReAct agents
  - Plan-and-execute agents
  - Self-ask with search
  - Custom agent creation

- **Tools & Integrations**
  - Built-in tools
  - Custom tool creation
  - Toolkits (SQL, Python, etc.)
  - LangSmith for debugging

### CrewAI

- **Multi-Agent Systems**

  - Crew orchestration
  - Role definition
  - Task delegation
  - Agent collaboration

- **Features**
  - Sequential & hierarchical crews
  - Agent memory & context
  - Tool sharing
  - Process management

### AutoGen

- **Conversation Patterns**

  - Multi-agent conversations
  - Group chat orchestration
  - Human-in-the-loop
  - Code execution agents

- **Advanced Features**
  - Customizable agents
  - Conversation patterns
  - Cost optimization
  - Agent workflows

---

## 7. LangGraph

Build complex, stateful agent workflows with LangGraph.

### Core Concepts

- **Graph-Based Agents**

  - Nodes & edges
  - State management
  - Conditional routing
  - Cycles & loops

- **State Management**

  - State schemas
  - State updates
  - Checkpointing
  - Persistence

- **Workflow Patterns**

  - Linear workflows
  - Conditional branching
  - Parallel execution
  - Human-in-the-loop
  - Error handling & recovery

- **Advanced Features**
  - Streaming
  - Interrupts & resumption
  - Subgraphs
  - Dynamic graph construction
  - Memory & context management

### Use Cases

- Multi-agent collaboration
- Complex reasoning workflows
- RAG pipelines
- Autonomous research agents
- Long-running tasks

---

## Learning Resources

### Recommended Order

1. Start with **LLM Providers** – Get comfortable with APIs
2. Master **Prompt Engineering** – Learn to communicate effectively
3. Add **Tool Calls** – Extend LLM capabilities
4. Learn **Chaining LLM Calls** – Combine calls for complex tasks
5. Explore **Vector Databases & RAG** – Add knowledge retrieval
6. Use **Frameworks** – Now you understand what they abstract
7. Advanced: **LangGraph** – Build production-ready workflows

### Practice Projects

- Simple chatbot with API
- Prompt optimization challenge
- Tool-using agent (calculator, search, etc.)
- Chained summarization pipeline (e.g., document → extract → summarize)
- RAG-powered Q&A agent with vector database
- Multi-agent system with CrewAI (e.g., research team)
- Complex workflow with LangGraph

---

## Next Steps

After mastering agents, explore:

- **Fine-tuning** – Custom models for specific domains
- **Evaluation** – Testing & benchmarking agents
- **Production Deployment** – Scaling agents for real-world use
- **Advanced RAG** – Multi-query, self-RAG, adaptive retrieval

---
