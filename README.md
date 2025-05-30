# Local LLM Stack: Your Complete AI Workbench

*A comprehensive Docker-based environment for working with Large Language Models locally*

![Local LLM Stack Banner](https://example.com/banner.jpg)

## Introduction

In today's rapidly evolving AI landscape, having a reliable local environment for experimenting with Large Language Models (LLMs) has become essential for developers, researchers, and AI enthusiasts. The Local LLM Stack provides exactly that - a complete ecosystem for document management, workflow automation, vector search, and LLM inference capabilities, all running locally on your machine.

Inspired by setups like [Peter Nhan's Ollama-AnythingLLM integration](https://peter-nhan.github.io/posts/Ollama-AnythingLLM/), this stack takes the concept further by incorporating additional services and enhanced configuration options to create a more robust and versatile environment.

## What Makes This Stack Special?

The Local LLM Stack integrates several powerful open-source tools that work together seamlessly:

- **Document processing and RAG capabilities** with AnythingLLM
- **AI workflow automation** with Flowise
- **Direct LLM interaction** through Open WebUI
- **Text generation pipeline** with Open WebUI Pipelines
- **Advanced workflow automation** with n8n
- **Vector storage** with Qdrant
- **Seamless integration** with Ollama for LLM inference

Unlike simpler setups, this stack is designed with flexibility in mind, allowing you to use either a locally installed Ollama instance or run Ollama as a containerized service. All components are configured to work together out of the box, with sensible defaults that can be customized to suit your specific needs.

## The Services: A Closer Look

### AnythingLLM
AnythingLLM serves as your document management and AI interaction platform. It allows you to chat with any document, such as PDFs or Word files, using various LLMs. In our setup, AnythingLLM is configured to use:
- Qdrant as the vector database for efficient document embeddings
- Ollama for LLM capabilities
- Port 3002 for web access (http://localhost:3002)

### Flowise
Flowise provides a visual programming interface for creating AI workflows:
- Build complex AI applications without coding
- Connect to various AI services and data sources
- Port 3001 for web access (http://localhost:3001)

### Open WebUI
Open WebUI offers a clean interface for direct interaction with AI models:
- Chat with models hosted on Ollama
- Manage and switch between different models
- Port 11500 for web access (http://localhost:11500)

### Open WebUI Pipelines
Pipelines provides optimized text generation capabilities:
- High-performance inference server for LLMs
- OpenAI-compatible API for easy integration
- Works seamlessly with Open WebUI
- Accessible through Open WebUI or directly via API

### n8n
n8n is a powerful workflow automation platform:
- Create automated workflows connecting various services
- Includes automatic import of workflows and credentials from backup directory
- Port 5678 for web access (http://localhost:5678)

### Qdrant
Qdrant serves as our vector database for AI applications:
- Stores and retrieves vector embeddings for semantic search
- Used by AnythingLLM for document embeddings
- Port 6333 for API access (http://localhost:6333)

### PostgreSQL
PostgreSQL provides database services for n8n:
- Reliable and robust SQL database
- Port 5432 for database connections

### Ollama (Optional)
Ollama is our LLM inference engine:
- Run models like Llama, Mistral, and others locally
- Can be run natively (default) or as a containerized service
- Used by AnythingLLM and can be used by Flowise
- Port 11434 for API access (http://localhost:11434)

## Getting Started

### Prerequisites
Before diving in, make sure you have:
- Docker and Docker Compose installed on your system
- Basic understanding of Docker containers and networking
- Ollama installed locally (or optionally run as a container by uncommenting the Ollama service in docker-compose.yml)

### Setup Instructions

1. **Clone the repository:**
   ```bash
   git clone https://github.com/yourusername/local-llm-stack.git
   cd local-llm-stack
   ```

2. **Create your environment file:**
   ```bash
   cp .env.sample .env
   ```

3. **Customize your environment:**
   Open the `.env` file in your favorite editor and modify the credentials and settings to your liking.

4. **Configure Ollama:**
   By default, the setup is configured to use a locally installed Ollama instance. If you prefer to run Ollama as a container:
   - Uncomment the `ollama_storage` volume in the volumes section of docker-compose.yml
   - Uncomment the entire `ollama` service definition
   - Update the OLLAMA_BASE_PATH and EMBEDDING_BASE_PATH in .env to use http://ollama:11434

5. **Create the data directory structure:**
   ```bash
   mkdir -p ./data/{n8n,postgres,qdrant,openwebui,flowise,anythingllm,pipelines}
   ```

6. **Launch the stack:**
   ```bash
   docker-compose up -d
   ```

7. **Access your services:**
   - Flowise: http://localhost:3001
   - AnythingLLM: http://localhost:3002
   - Open WebUI: http://localhost:11500
   - n8n: http://localhost:5678
   - Qdrant: http://localhost:6333

## Data Persistence

Your data is persisted in the local `./data` directory with separate subdirectories for each service:

- `./data/n8n`: n8n data
- `./data/postgres`: PostgreSQL database
- `./data/qdrant`: Qdrant vector database
- `./data/openwebui`: Open WebUI data
- `./data/flowise`: Flowise data
- `./data/anythingllm`: AnythingLLM data
- `./data/pipelines`: Open WebUI Pipelines data
- `./data/ollama`: Ollama data (when using containerized Ollama)

This structure ensures your data persists across container restarts and makes it easy to backup or transfer your data.

## Network Configuration

All services are connected to the `ai-network` Docker network for internal communication, ensuring they can seamlessly work together while remaining isolated from your host network.

## Customization Options

### AnythingLLM Configuration

AnythingLLM is highly configurable through environment variables. In our setup, it's configured to use:
- Qdrant as the vector database for document embeddings
- Ollama for LLM capabilities and embeddings
- Various settings that can be adjusted in the .env file

### Open WebUI Pipelines

To connect Open WebUI with the Pipelines service:
1. In Open WebUI, go to Settings > Connections > OpenAI API
2. Set the API URL to `http://pipelines:9099`
3. Set the API key to `0p3n-w3bu!` (or your custom key if modified)

### Ollama Configuration

Ollama can be configured in two ways:

1. **Native Installation (Default)**: 
   - Install Ollama directly on your host machine
   - The services are configured to access Ollama via host.docker.internal
   - Requires Ollama to be running on your host (`ollama serve`)

2. **Containerized Installation**:
   - Uncomment the Ollama service in docker-compose.yml
   - Uncomment the ollama_storage volume
   - Update the OLLAMA_BASE_PATH and EMBEDDING_BASE_PATH in .env to http://ollama:11434
   - No need to run Ollama separately on your host

## Use Cases

### Document Analysis with RAG
Upload documents to AnythingLLM and chat with them using the power of Retrieval-Augmented Generation (RAG). The system will:
1. Process and embed your documents using Ollama's embedding models
2. Store the embeddings in Qdrant
3. Retrieve relevant context when you ask questions
4. Generate responses using the LLM with the retrieved context

### Workflow Automation
Use n8n to create workflows that:
- Monitor folders for new documents
- Trigger document processing in AnythingLLM
- Send notifications when processing is complete
- Extract and process information from documents

### AI Application Development
Use Flowise to build AI applications with a visual interface:
- Create chatbots with specific knowledge domains
- Build document processing pipelines
- Develop custom AI assistants for specific tasks

### High-Performance Inference
Use Open WebUI Pipelines for optimized text generation:
- Faster response times compared to standard Ollama API
- OpenAI-compatible API for easy integration with various tools
- Connect directly from Open WebUI for an enhanced experience

## Troubleshooting

If you encounter any issues:

1. **Check the logs:**
   ```bash
   docker-compose logs [service-name]
   ```

2. **Ensure port availability:**
   Make sure all required ports are available on your host system.

3. **Verify environment variables:**
   Check that the `.env` file contains all necessary variables.

4. **Ollama status:**
   If using native Ollama, ensure it's running on your host machine:
   ```bash
   ollama serve
   ```

5. **Connection issues:**
   If you're having connection issues between containers and your host machine, check that the `extra_hosts` configuration is correctly set in docker-compose.yml.

6. **Permission issues with data directories:**
   If you encounter permission errors with the data directories, you may need to adjust permissions:
   ```bash
   sudo chmod -R 777 ./data  # Use with caution in production environments
   ```

## Conclusion

The Local LLM Stack provides a powerful, flexible environment for working with AI and LLMs locally. By combining the best open-source tools available, it offers capabilities that were previously only available through cloud services, all while keeping your data private and under your control.

Whether you're a developer looking to build AI applications, a researcher experimenting with document analysis, or an enthusiast exploring the capabilities of LLMs, this stack provides the foundation you need to get started quickly and efficiently.

---

## References

- [Peter Nhan's Ollama-AnythingLLM Integration](https://peter-nhan.github.io/posts/Ollama-AnythingLLM/)
- [AnythingLLM Documentation](https://docs.anythingllm.com/)
- [Ollama GitHub Repository](https://github.com/ollama/ollama)
- [Qdrant Vector Database](https://qdrant.tech/)
- [n8n Workflow Automation](https://n8n.io/)
- [Flowise AI](https://flowiseai.com/)
- [Open WebUI](https://github.com/open-webui/open-webui)
- [Open WebUI Pipelines](https://github.com/open-webui/pipelines)

