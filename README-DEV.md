# Local Development with Docker

This is the **proper way** to run Coinos for local development - using Docker Compose.

## Prerequisites

- Docker Desktop installed and running
- Your codebase cloned locally

## Quick Start

1. **Ensure you have the config files:**
   ```bash
   cp config.ts.sample config.ts  # If not exists
   cp compose.yml.sample compose.yml  # If not exists
   ```

2. **Ensure data directory exists:**
   ```bash
   if [ ! -d data ]; then cp -r sampledata data; fi
   ```

3. **Start all services with Docker Compose:**
   ```bash
   docker compose up -d
   ```

4. **Check that services are running:**
   ```bash
   docker compose ps
   ```

5. **View logs (optional):**
   ```bash
   docker compose logs -f app
   ```

6. **Access the app:**
   - Backend API: http://localhost:3119
   - Frontend UI: http://localhost:5173 (run separately)

## How It Works

The `compose.yml` sets up:

- **app** container: Your server code runs here
  - Mounts your local code: `./:/home/bun/app`
  - Hot-reload via nodemon
  - Port 3119 exposed to host
  
- **Dependencies** (all in Docker):
  - `db` - Redis/KeyDB (port 6379)
  - `bc` - Bitcoin node (port 18443)
  - `cl`, `clb`, `clc` - Lightning nodes
  - `lq` - Liquid node (port 7043)
  - `archive` - Archive Redis (port 6380)
  - `nostr` - Nostr relay (port 8082)

## Development Workflow

1. **Make code changes** in your local files
2. **Nodemon automatically restarts** the app container
3. **Changes are reflected** immediately
4. **Access services** via localhost ports

## Common Commands

```bash
# Start everything
docker compose up -d

# Stop everything
docker compose down

# View logs
docker compose logs -f app

# Restart a service
docker compose restart app

# Rebuild if needed
docker compose up -d --build

# Check status
docker compose ps
```

## Troubleshooting

**If app container keeps restarting:**
- Check logs: `docker compose logs app`
- Ensure all dependencies are running: `docker compose ps`

**If database connection fails:**
- Ensure `db` container is running: `docker compose ps db`
- Check Redis connection: `docker compose logs db`

**Port conflicts:**
- Stop conflicting services on your machine
- Or change ports in `compose.yml`

