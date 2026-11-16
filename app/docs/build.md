# Building and Testing the OctoCAT Supply Chain Application

This guide provides instructions for building, running, and testing the OctoCAT Supply Chain Management application, which consists of an API component and a React Frontend.

Run commands from the **repository root**, which contains `app` and `labs`. Keep that root open in VS Code. For the participant setup, including Windows certificate guidance, see the [setup guide](../../labs/01-setup.md). In Windows PowerShell use `npm.cmd` instead of `npm`; the Make commands below are for Codespaces or another Bash environment with Make.

## Prerequisites



- Node.js 24.x (npm included)
- npm (latest version recommended)

- Docker/Podman (optional, for containerization)

## Installation

1. Clone the repository
2. Install dependencies:

```bash
make -C app install
```

## Building the Application

### Using Make Commands

You can build the entire application or its individual components using the following commands:

```bash
# Build both API and Frontend components
make -C app build

# Build only the API component
make -C app build-api

# Build only the Frontend component
make -C app build-frontend
```



Alternatively, if you're in the `app/api/` or `app/frontend/` directories, you can use npm commands directly:

```bash
npm --prefix app/api run build
npm --prefix app/frontend run build
```





### Database management

```bash
# Apply pending migrations
make -C app db-init

# Run migrations only
npm --prefix app/api run db:migrate

# Apply migrations and seed sample data
make -C app db-seed
```



Alternatively, if you're in the `app/api/` directory, you can use npm commands directly:

```bash
npm --prefix app/api run db:init
npm --prefix app/api run db:migrate
npm --prefix app/api run db:seed
```





Environment variables:

- DB_FILE: path to SQLite database file (default: `app/api/data/app.db`)
- DB_ENABLE_WAL: enable WAL mode (default: true)
- DB_FOREIGN_KEYS: enforce foreign keys (default: true)
- DB_TIMEOUT: busy timeout in ms (default: 30000)

### Using VS Code Tasks

VS Code tasks have been configured to streamline the build process:

1. Press `Ctrl+Shift+P` (or `Cmd+Shift+P` on macOS) to open the Command Palette
2. Type `Tasks: Run Task` and select it
3. Choose from the following tasks:
   - `Build All`: Builds both API and Frontend components
   - `Build API`: Builds only the API component
   - `Build Frontend`: Builds only the Frontend component


Alternatively, you can press `Ctrl+Shift+B` (or `Cmd+Shift+B` on macOS) to run the default build task (`Build All`).


## Running the Application

### Using Make Commands

```bash
# Start both API and Frontend in development mode with hot reloading
make -C app dev

# Start only the API in development mode
make -C app dev-api

# Start only the Frontend in development mode
make -C app dev-frontend

# Start the application in production mode
make -C app start
```



Alternatively, if you're in the `app/api/` or `app/frontend/` directories, you can use npm commands directly:

```bash
# Terminal 1
npm --prefix app/api run dev
# Terminal 2
npm --prefix app/frontend run dev
```





### Using VS Code Debugger

1. Open the Debug panel (`Ctrl+Shift+D` or `Cmd+Shift+D` on macOS)
2. Select `Start API & Frontend` from the dropdown menu
3. Click the green play button or press F5

This will start both the API and Frontend in development mode with the integrated terminal, allowing you to see the console output.

## Testing the Application

### Running Tests

```bash
# Run API tests only
make -C app test-api
```



Alternatively, if you're in the `app/api/` or `app/frontend/` directories, you can use npm commands directly:

```bash
npm --prefix app/api test -- --run
```





### Linting

```bash
# Run linting checks
make -C app lint
```



Alternatively, if you're in the `app/api/` or `app/frontend/` directory, you can use npm commands directly:

```bash
npm --prefix app/frontend run lint
```

```bash
npm --prefix app/api run lint
```





## Additional Information

### Port Configuration

The API runs on port 3000 by default, and the Frontend runs on port 5137. When running in a Codespace environment, ensure that the API port visibility is set to `public` to avoid CORS errors when the Frontend tries to communicate with the API.

For Docker, the sample compose maps API to 3000 and frontend to 3001.

### Docker Deployment

The project includes Dockerfiles for both API and Frontend components and a docker-compose.yml file for easy containerized deployment:

```bash
# Build and start using Docker Compose
docker compose -f app/docker-compose.yml up --build
```
