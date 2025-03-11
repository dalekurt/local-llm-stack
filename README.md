# Docker Local AI LLM Environment

This repository contains a Docker Compose setup for running a local AI and automation environment with multiple services.

## Services

### Flowise
- AI workflow automation tool
- Port: 3001
- Web interface: http://localhost:3001

### AnythingLLM
- Document management and AI interaction platform
- Port: 3002
- Web interface: http://localhost:3002
- Uses Qdrant as vector database
- Connects to Ollama for LLM capabilities

### Open WebUI
- Web interface for interacting with AI models
- Port: 11500
- Web interface: http://localhost:11500

### n8n
- Workflow automation platform
- Port: 5678
- Web interface: http://localhost:5678
- Includes automatic import of workflows and credentials from backup directory

### Qdrant
- Vector database for AI applications
- Port: 6333
- API endpoint: http://localhost:6333
- Used by AnythingLLM for document embeddings

### PostgreSQL
- Database used by n8n
- Port: 5432

## Prerequisites

- Docker and Docker Compose installed on your system
- Basic understanding of Docker containers and networking
- Ollama installed locally (or optionally run as a container by uncommenting the Ollama service in docker-compose.yml)

## Setup Instructions

1. Clone this repository:
   ```
   git clone https://github.com/yourusername/docker-local-ai-llm.git
   cd docker-local-ai-llm
   ```

2. Create a `.env` file based on the provided `.env.sample` file:
   ```
   cp .env.sample .env
   ```

3. Modify the `.env` file with your preferred credentials and secrets.

4. Ollama Configuration:
   - By default, the setup is configured to use a locally installed Ollama instance
   - If you want to run Ollama as a container, uncomment the Ollama service in docker-compose.yml and update the OLLAMA_BASE_PATH and EMBEDDING_BASE_PATH in .env to use http://ollama:11434

5. Start the services:
   ```
   docker-compose up -d
   ```

6. Access the services through their respective ports:
   - Flowise: http://localhost:3001
   - AnythingLLM: http://localhost:3002
   - Open WebUI: http://localhost:11500
   - n8n: http://localhost:5678
   - Qdrant: http://localhost:6333

## Data Persistence

The following Docker volumes are used for data persistence:

- `n8n_storage`: n8n data
- `postgres_storage`: PostgreSQL database
- `qdrant_storage`: Qdrant vector database
- `open-webui`: Open WebUI data
- `flowise`: Flowise data
- `anythingllm_storage`: AnythingLLM data
- `ollama_storage`: Ollama data (when using containerized Ollama)

## Network Configuration

All services are connected to the `ai-network` Docker network for internal communication.

## Customization

You can customize the services by modifying the `docker-compose.yml` and `.env` files according to your needs.

### AnythingLLM Configuration

AnythingLLM is configured to use:
- Qdrant as the vector database
- Ollama for LLM capabilities and embeddings
- The configuration can be adjusted in the .env file

## Troubleshooting

If you encounter any issues:

1. Check the logs of the specific service:
   ```
   docker-compose logs [service-name]
   ```

2. Ensure all required ports are available on your host system.

3. Verify that the `.env` file contains all necessary variables.

4. If using native Ollama, ensure it's running on your host machine:
   ```
   ollama serve
   ```

5. If you're having connection issues between containers and your host machine, check that the `extra_hosts` configuration is correctly set in docker-compose.yml.

## License

[Specify your license here] 