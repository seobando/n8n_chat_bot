# N8N Chat Bot - Docker Development Setup

This project contains N8N workflows for building chat bots with various integrations including Facebook, ManyChat, and RAG (Retrieval-Augmented Generation) applications.

## Prerequisites

- Docker and Docker Compose installed on your system
- Git (for version control)
- VS Code with Dev Containers extension (for devcontainer setup)

## Quick Start

### Option 1: DevContainer (Recommended for Development)

1. **Open in VS Code with Dev Containers**
   ```bash
   # Open the project in VS Code
   code C:\Users\sebas\Documents\Projects\n8n_chat_bot
   ```

2. **Reopen in Container**
   - Press `Ctrl+Shift+P` (or `Cmd+Shift+P` on Mac)
   - Type "Dev Containers: Reopen in Container"
   - Select the command and wait for the container to build

3. **Access N8N**
   - **URL**: http://localhost:5678
   - **Username**: admin
   - **Password**: admin123

### Option 2: Traditional Docker Compose

1. **Clone and Setup**
   ```bash
   # Navigate to your project directory
   cd C:\Users\sebas\Documents\Projects\n8n_chat_bot

   # Copy the environment file
   cp .env.example .env

   # Edit the .env file with your preferred settings
   # At minimum, change the N8N_ENCRYPTION_KEY to a secure value
   ```

2. **Start N8N Development Environment**
   ```bash
   # Start N8N with SQLite (recommended for development)
   docker-compose up -d

   # Or start with PostgreSQL and Redis (for production-like setup)
   docker-compose --profile postgres --profile redis up -d
   ```

3. **Access N8N**
   - **URL**: http://localhost:5678
   - **Username**: admin
   - **Password**: admin123

## Configuration

### Environment Variables

The `.env.example` file contains all available configuration options. Key settings include:

- `N8N_BASIC_AUTH_USER` / `N8N_BASIC_AUTH_PASSWORD`: Login credentials
- `N8N_ENCRYPTION_KEY`: Encryption key for sensitive data (CHANGE THIS!)
- `N8N_LOG_LEVEL`: Logging level (debug, info, warn, error)
- `DB_TYPE`: Database type (sqlite, postgresdb)

### Database Options

#### SQLite (Default - Development)
- No additional setup required
- Data persisted in Docker volume
- Good for development and testing

#### PostgreSQL (Production-like)
```bash
# Start with PostgreSQL
docker-compose --profile postgres up -d
```

#### Redis (Queue Management)
```bash
# Start with Redis for queue management
docker-compose --profile redis up -d
```

## Project Structure

```
n8n_chat_bot/
├── Apps/                    # Main application workflows
│   ├── Facebook RAG App.json
│   ├── Facebook Simple LLM.json
│   ├── Generic RAG App.json
│   ├── ManyChat RAG App PROD.json
│   └── ManyChat Simple LLM.json
├── DataIntegration/         # Data processing workflows
│   ├── GoogleDriveFiles.json
│   ├── Manual Scraping.json
│   ├── Single Page Scraping.json
│   └── WebCrawling.json
├── Training/               # Training and development workflows
│   ├── 1 Gold Smit to Pinecode.json
│   ├── 2 Gold Smith RAG App.json
│   └── 3 Scrape Web page RAG.json
├── docker-compose.yml      # Docker Compose configuration
├── .env.example           # Environment variables template
└── README.md              # This file
```

## Development Workflow

### DevContainer Development

When using the devcontainer, you have access to helpful development commands:

```bash
# Check N8N status and access information
check_n8n

# View N8N logs in real-time
view_logs

# Backup all workflows
backup_workflows

# Show all available commands
show_help
```

### 1. Import Workflows

1. Access N8N at http://localhost:5678
2. Go to "Workflows" → "Import from File"
3. Select JSON files from the `Apps/`, `DataIntegration/`, or `Training/` directories

### 2. Workflow Development

- Create new workflows in the N8N interface
- Export workflows as JSON files to version control
- Use the local file system for workflow backup and sharing
- Workflows are automatically synced between container and host

### 3. Data Persistence

- **DevContainer**: Data is stored in Docker volumes and synced with host
- **Docker Compose**: Workflows and credentials are stored in Docker volumes
- Data persists between container restarts
- Backup volumes regularly for important data

## Useful Commands

### DevContainer Commands

```bash
# Check N8N status and access info
check_n8n

# View N8N logs in real-time
view_logs

# Backup workflows
backup_workflows

# Show all available commands
show_help
```

### Docker Compose Commands

```bash
# Start the development environment
docker-compose up -d

# View logs
docker-compose logs -f n8n

# Stop the environment
docker-compose down

# Stop and remove volumes (WARNING: This will delete all data)
docker-compose down -v

# Restart N8N service
docker-compose restart n8n

# Update N8N to latest version
docker-compose pull
docker-compose up -d

# Access N8N container shell
docker-compose exec n8n sh

# View container status
docker-compose ps
```

### DevContainer Management

```bash
# Rebuild devcontainer (if you make changes to .devcontainer files)
# In VS Code: Ctrl+Shift+P → "Dev Containers: Rebuild Container"

# Open new terminal in devcontainer
# In VS Code: Terminal → New Terminal

# Access devcontainer from outside VS Code
docker exec -it n8n-dev-container bash
```

## Troubleshooting

### Common Issues

1. **Port 5678 already in use**
   ```bash
   # Change the port in docker-compose.yml or .devcontainer/docker-compose.dev.yml
   ports:
     - "5679:5678"  # Use port 5679 instead
   ```

2. **Permission issues on Windows**
   ```bash
   # Ensure Docker Desktop has access to your project directory
   # Check Docker Desktop settings → Resources → File Sharing
   ```

3. **DevContainer not starting**
   ```bash
   # Rebuild the devcontainer
   # In VS Code: Ctrl+Shift+P → "Dev Containers: Rebuild Container"
   
   # Check devcontainer logs
   # In VS Code: View → Output → Select "Dev Containers" from dropdown
   ```

4. **Database connection issues**
   ```bash
   # Check if PostgreSQL is running
   docker-compose ps postgres
   
   # View PostgreSQL logs
   docker-compose logs postgres
   ```

5. **Workflow import errors**
   - Ensure JSON files are valid
   - Check N8N version compatibility
   - Verify all required credentials are configured

6. **DevContainer helper commands not working**
   ```bash
   # Source the helper functions manually
   source /workspace/scripts/dev-helpers.sh
   
   # Or restart the terminal in VS Code
   ```

### Logs and Debugging

```bash
# View all logs
docker-compose logs

# View N8N logs only
docker-compose logs n8n

# Follow logs in real-time
docker-compose logs -f n8n

# View logs with timestamps
docker-compose logs -t n8n
```

## Security Considerations

1. **Change default credentials** in production
2. **Use strong encryption keys** for N8N_ENCRYPTION_KEY
3. **Enable HTTPS** in production environments
4. **Restrict network access** as needed
5. **Regular security updates** of Docker images

## Production Deployment

For production deployment:

1. Use PostgreSQL instead of SQLite
2. Enable Redis for queue management
3. Use environment-specific configuration
4. Set up proper backup strategies
5. Configure reverse proxy (nginx/traefik)
6. Enable SSL/TLS certificates
7. Set up monitoring and alerting

## Support

- [N8N Documentation](https://docs.n8n.io/)
- [N8N Community Forum](https://community.n8n.io/)
- [Docker Compose Documentation](https://docs.docker.com/compose/)
