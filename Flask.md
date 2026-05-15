# Dockerize a Flask App

> **5 steps · from zero to running container**

---

## Step 1 — Project Structure

Create a project folder and set up three files. That's all Docker needs to containerize your app.

```bash
mkdir flask-docker && cd flask-docker
touch app.py requirements.txt Dockerfile
```

> **Tip:** Keep everything in the same directory — Docker's build context will pick up all three files.

---

## Step 2 — Write the Flask App

A minimal Flask app with one route. Notice `host="0.0.0.0"` — this is required so Flask listens on all interfaces inside the container, not just localhost.

**`app.py`**

```python
from flask import Flask

app = Flask(__name__)

@app.route("/")
def hello():
    return "Hello from Docker! 🐳"

if __name__ == "__main__":
    app.run(host="0.0.0.0", port=5000)
```

---

## Step 3 — Pin Your Dependencies

List every Python package your app needs. Pinning exact versions guarantees reproducible builds across any machine.

**`requirements.txt`**

```
flask==3.0.3
```

> **Tip:** Run `pip freeze > requirements.txt` in your local venv to capture all dependencies from an existing project.

---

## Step 4 — Write the Dockerfile

The Dockerfile is a recipe for your image. Each instruction adds a layer — Docker caches layers, so ordering matters for fast rebuilds.

**`Dockerfile`**

```dockerfile
# Use an official slim Python base image
FROM python:3.12-slim

# Set working directory inside the container
WORKDIR /app

# Copy and install dependencies first (better layer caching)
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# Copy the rest of the application code
COPY . .

# Expose the port Flask listens on
EXPOSE 5000

# Run the app
CMD ["python", "app.py"]
```

> **Tip:** `python:3.12-slim` is ~50 MB vs ~1 GB for the full image — always prefer slim or alpine for production.

---

## Step 5 — Build & Run

Two commands and you're live. The `-p` flag maps port 5000 on your machine to port 5000 inside the container.

```bash
# Build the image (run from project root)
docker build -t flask-docker .

# Run a container from the image
docker run -p 5000:5000 flask-docker
```

Open **http://localhost:5000** in your browser — you should see the hello message.

> **Tip:** Add `-d` to run in detached (background) mode:
> ```bash
> docker run -d -p 5000:5000 flask-docker
> ```

---

## ✅ Your Flask App is Containerized!

### Docker compose (docker-compose.yml)

~~~yaml
services:
  web:
    build: .
    ports:
      - "5000:5000"
    volumes:
      - .:/app
    environment:
      - FLASK_ENV=development
      - FLASK_DEBUG=1
    depends_on:
      - db
    restart: unless-stopped

  db:
    image: postgres:16-alpine
    ports:
      - "5432:5432"
    environment:
      - POSTGRES_USER=flask_user
      - POSTGRES_PASSWORD=flask_pass
      - POSTGRES_DB=flask_db
    volumes:
      - postgres_data:/var/lib/postgresql/data
    restart: unless-stopped

volumes:
  postgres_data:
~~~

### What's next?

- **Add a `.dockerignore`** — exclude `__pycache__`, `.env`, and `.git` to keep your image lean
- **Deploy to the cloud** — try Fly.io, Railway, or AWS ECS for hosting your container
