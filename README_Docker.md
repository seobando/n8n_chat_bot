# n8n Docker Setup

This setup allows you to run n8n in a Docker container with your existing workflow folders (Apps, DataIntegration, Training) properly mounted.

## Quick Start

1. **Copy the environment file:**
   ```bash
   cp env.example .env
   ```

2. **Edit the `.env` file** and change the default password:
   ```
   N8N_BASIC_AUTH_PASSWORD=your_secure_password_here
   ```

3. **Start n8n:**
   ```bash
   docker-compose up -d
   ```

4. **Access n8n:**
   - Open your browser and go to: http://localhost:5678
   - Login with the credentials from your `.env` file

## How It Works

The Docker Compose configuration:

- **Maps your folders**: Your `Apps/`, `DataIntegration/`, and `Training/` folders are mounted into the n8n container
- **Persistent data**: n8n's internal data (workflows, credentials, etc.) is stored in a Docker volume
- **Port mapping**: n8n runs on port 5678 (accessible at http://localhost:5678)

## Folder Structure in Container

Your folders are mounted as follows:
- `./Apps` → `/home/node/.n8n/workflows/Apps`
- `./DataIntegration` → `/home/node/.n8n/workflows/DataIntegration`
- `./Training` → `/home/node/.n8n/workflows/Training`

## Useful Commands

- **Start n8n**: `docker-compose up -d`
- **Stop n8n**: `docker-compose down`
- **View logs**: `docker-compose logs -f n8n`
- **Restart n8n**: `docker-compose restart n8n`
- **Update n8n**: `docker-compose pull && docker-compose up -d`

## Environment Variables

Key environment variables you can customize in your `.env` file:

- `N8N_BASIC_AUTH_USER`: Username for n8n login
- `N8N_BASIC_AUTH_PASSWORD`: Password for n8n login
- `N8N_HOST`: Host where n8n is accessible
- `WEBHOOK_URL`: Base URL for webhooks
- `GENERIC_TIMEZONE`: Timezone for n8n

## Notes

- Your workflow JSON files in the Apps, DataIntegration, and Training folders will be accessible within n8n
- Any changes you make to workflows in n8n will be saved to the persistent volume
- The container automatically restarts unless manually stopped
- All n8n data persists between container restarts

## Troubleshooting

If you encounter issues:

1. Check if port 5678 is already in use: `netstat -an | grep 5678`
2. View container logs: `docker-compose logs n8n`
3. Ensure Docker and Docker Compose are properly installed
4. Make sure your `.env` file exists and has the correct format

