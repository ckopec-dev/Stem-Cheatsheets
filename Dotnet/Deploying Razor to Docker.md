# Deploying an ASP.NET Core Razor Application as a Docker Container

This tutorial walks you through containerizing and deploying an ASP.NET Core Razor Pages application using Docker.

## Prerequisites

- Docker Desktop installed on your machine
- .NET SDK (6.0 or later)
- Basic familiarity with command line
- A Razor Pages application (or create a new one)

## Step 1: Create a Razor Pages Application

If you don't have an existing application, create one:

```bash
dotnet new webapp -o MyRazorApp
cd MyRazorApp
```

Test that it works locally:

```bash
dotnet run
```

Visit `https://localhost:5001` to verify the application runs correctly.

To make `dotnet run` listen on all network interfaces in Linux, you need to bind to `0.0.0.0` instead of `localhost`. Here are the main ways to do this:

**Quick method - command line:**
```bash
dotnet run --urls "http://0.0.0.0:5000"
```

**Set it in `launchSettings.json`:**
Edit `Properties/launchSettings.json` in your project:
```json
{
  "profiles": {
    "YourProjectName": {
      "commandName": "Project",
      "applicationUrl": "http://0.0.0.0:5000",
      "environmentVariables": {
        "ASPNETCORE_ENVIRONMENT": "Development"
      }
    }
  }
}
```

**Environment variable:**
```bash
export ASPNETCORE_URLS="http://0.0.0.0:5000"
dotnet run
```

**In Program.cs (for more control):**
```csharp
var builder = WebApplication.CreateBuilder(args);
builder.WebHost.UseUrls("http://0.0.0.0:5000");
```

After starting, you can access the site from another PC using `http://YOUR_LINUX_IP:5000`.

**Important notes:**
- Make sure your firewall allows incoming connections on port 5000 (or whatever port you choose)
- `0.0.0.0` means "listen on all network interfaces"
- For HTTPS, use `https://0.0.0.0:5001` (you'll need to handle certificates)
- In production, consider using a reverse proxy like nginx
  
## Step 2: Create a Dockerfile

In your project root directory, create a file named `Dockerfile` (no extension):

```dockerfile
# Use the official .NET SDK image for building
FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
WORKDIR /app

# Copy csproj and restore dependencies
COPY *.csproj ./
RUN dotnet restore

# Copy everything else and build
COPY . ./
RUN dotnet publish -c Release -o out

# Build runtime image
FROM mcr.microsoft.com/dotnet/aspnet:8.0
WORKDIR /app
COPY --from=build /app/out .

# Expose port 8080
EXPOSE 8080

# Set environment variable for ASP.NET Core
ENV ASPNETCORE_URLS=http://+:8080

# Run the application
ENTRYPOINT ["dotnet", "MyRazorApp.dll"]
```

**Important:** Replace `MyRazorApp.dll` with your actual project name.

## Step 3: Create a .dockerignore File

Create a `.dockerignore` file in your project root to exclude unnecessary files:

```
bin/
obj/
*.md
.git/
.gitignore
.vs/
.vscode/
```

## Step 4: Build the Docker Image

Build your Docker image with a tag:

```bash
docker build -t myrazorapp:1.0 .
```

This command:
- `-t myrazorapp:1.0` tags the image with name and version
- `.` specifies the current directory as build context

## Step 5: Run the Container

Run your containerized application:

```bash
docker run -d -p 8080:8080 --name myrazorapp-container myrazorapp:1.0
```

Parameters explained:
- `-d` runs in detached mode (background)
- `-p 8080:8080` maps host port 8080 to container port 8080
- `--name` gives the container a friendly name
- `myrazorapp:1.0` is the image to run

## Step 6: Verify the Deployment

Visit `http://localhost:8080` in your browser. Your Razor application should be running!

Check container logs:

```bash
docker logs myrazorapp-container
```

## Step 7: Manage Your Container

### Stop the container
```bash
docker stop myrazorapp-container
```

### Start the container
```bash
docker start myrazorapp-container
```

### Remove the container
```bash
docker rm myrazorapp-container
```

### View running containers
```bash
docker ps
```

## Step 8: Push to Docker Hub (Optional)

To share your image or deploy to production:

1. Create an account on [Docker Hub](https://hub.docker.com)

2. Log in via CLI:
```bash
docker login
```

3. Tag your image with your Docker Hub username:
```bash
docker tag myrazorapp:1.0 yourusername/myrazorapp:1.0
```

4. Push to Docker Hub:
```bash
docker push yourusername/myrazorapp:1.0
```

## Using Docker Compose (Recommended for Development)

Create a `docker-compose.yml` file in your project root:

```yaml
version: '3.8'

services:
  web:
    build: .
    ports:
      - "8080:8080"
    environment:
      - ASPNETCORE_ENVIRONMENT=Development
    volumes:
      - ./appsettings.json:/app/appsettings.json:ro
```

Run with Docker Compose:

```bash
docker-compose up -d
```

Stop with:

```bash
docker-compose down
```

## Production Considerations

### Environment Variables
Set production environment variables:

```bash
docker run -d -p 8080:8080 \
  -e ASPNETCORE_ENVIRONMENT=Production \
  -e ConnectionStrings__DefaultConnection="your-connection-string" \
  myrazorapp:1.0
```

### Health Checks
Add health checks to your Dockerfile:

```dockerfile
HEALTHCHECK --interval=30s --timeout=3s --start-period=5s --retries=3 \
  CMD curl -f http://localhost:8080/health || exit 1
```

### HTTPS Support
For HTTPS in production, you'll need to configure certificates. Mount certificate files as volumes or use a reverse proxy like Nginx.

## Troubleshooting

### Container won't start
Check logs: `docker logs myrazorapp-container`

### Port already in use
Change the host port: `docker run -p 8081:8080 ...`

### Permission issues
Run Docker commands with appropriate permissions or add your user to the docker group.

## Next Steps

- Set up CI/CD pipeline for automatic builds
- Deploy to cloud platforms (Azure, AWS, GCP)
- Configure orchestration with Kubernetes
- Implement monitoring and logging solutions

Your ASP.NET Core Razor application is now containerized and ready for deployment to any Docker-compatible environment!
