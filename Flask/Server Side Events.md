# Building Server-Sent Events (SSE) with Flask

Server-Sent Events (SSE) let a server push updates to the browser over a single HTTP connection. They’re perfect for:

* Live notifications
* Real-time logs
* Progress updates
* Dashboards
* Streaming AI responses

Unlike WebSockets, SSE is:

* Simpler
* One-way (server → client)
* Built directly into browsers via `EventSource`

This tutorial uses Flask and vanilla JavaScript.

Official docs: [Flask Documentation](https://flask.palletsprojects.com/?utm_source=chatgpt.com)

---

# 1. Install Flask

```bash
pip install flask
```

---

# 2. Basic SSE Server

Create a file named `app.py`.

```python
from flask import Flask, Response
import time

app = Flask(__name__)

def generate_events():
    count = 0

    while True:
        count += 1

        # SSE format
        yield f"data: Message #{count}\n\n"

        time.sleep(1)

@app.route("/stream")
def stream():
    return Response(
        generate_events(),
        mimetype="text/event-stream"
    )

@app.route("/")
def index():
    return """
    <!DOCTYPE html>
    <html>
    <body>
        <h1>Flask SSE Demo</h1>

        <div id="messages"></div>

        <script>
            const eventSource = new EventSource("/stream");

            eventSource.onmessage = function(event) {
                const div = document.getElementById("messages");

                div.innerHTML += `<p>${event.data}</p>`;
            };
        </script>
    </body>
    </html>
    """

if __name__ == "__main__":
    app.run(debug=True, threaded=True)
```

---

# 3. Run the Application

```bash
python app.py
```

Open:

```text
http://127.0.0.1:5000
```

You should see a new message appear every second.

---

# 4. Understanding SSE Format

SSE messages must follow this structure:

```text
data: hello world

```

Important:

* Each message starts with `data:`
* Messages end with a blank line (`\n\n`)

You can also send:

```text
event: update
data: new data

```

---

# 5. Custom Event Types

Server:

```python
def generate_events():
    while True:
        yield "event: notification\n"
        yield "data: New alert!\n\n"

        time.sleep(2)
```

Client:

```javascript
const source = new EventSource("/stream");

source.addEventListener("notification", (event) => {
    console.log("Notification:", event.data);
});
```

---

# 6. Sending JSON Data

Server:

```python
import json

def generate_events():
    count = 0

    while True:
        count += 1

        data = {
            "count": count,
            "status": "running"
        }

        yield f"data: {json.dumps(data)}\n\n"

        time.sleep(1)
```

Client:

```javascript
source.onmessage = (event) => {
    const data = JSON.parse(event.data);

    console.log(data.count);
};
```

---

# 7. Important Response Headers

A production SSE endpoint should include headers like:

```python
return Response(
    generate_events(),
    mimetype="text/event-stream",
    headers={
        "Cache-Control": "no-cache",
        "Connection": "keep-alive",
    }
)
```

---

# 8. Prevent Buffering (Very Important)

Some servers buffer responses and break SSE.

If using Nginx:

```nginx
location /stream {
    proxy_pass http://localhost:5000;
    proxy_buffering off;
}
```

Without this, events may arrive in large batches instead of real time.

---

# 9. Streaming Real Logs Example

```python
from flask import Flask, Response
import random
import time

app = Flask(__name__)

def log_stream():
    levels = ["INFO", "WARNING", "ERROR"]

    while True:
        level = random.choice(levels)

        yield f"data: [{level}] Something happened\n\n"

        time.sleep(1)

@app.route("/logs")
def logs():
    return Response(log_stream(), mimetype="text/event-stream")
```

---

# 10. Auto-Reconnect

`EventSource` automatically reconnects if the connection drops.

You can control retry timing:

```python
yield "retry: 5000\n"
yield "data: reconnecting enabled\n\n"
```

This tells the browser to retry after 5 seconds.

---

# 11. Common Problems

## Problem: Events arrive all at once

Cause:

* Response buffering

Fix:

* Disable buffering
* Use streaming-friendly hosting

---

## Problem: Flask blocks other requests

Fix:

* Enable threading

```python
app.run(threaded=True)
```

Or use a production server like:

* Gunicorn
* Gevent
* Uvicorn

---

## Problem: Connection closes unexpectedly

Possible causes:

* Reverse proxy timeout
* Cloud provider timeout
* Missing keep-alive headers

---

# 12. Production Setup with Gunicorn + Gevent

Install:

```bash
pip install gunicorn gevent
```

Run:

```bash
gunicorn -k gevent -w 1 app:app
```

This works much better for long-lived streaming connections.

Official project pages:

* [Gunicorn](https://gunicorn.org/?utm_source=chatgpt.com)
* [Gevent](https://www.gevent.org/?utm_source=chatgpt.com)

---

# 13. When to Use SSE vs WebSockets

| Feature            | SSE                               | WebSockets   |
| ------------------ | --------------------------------- | ------------ |
| Direction          | Server → Client                   | Two-way      |
| Simplicity         | Easy                              | More complex |
| Built into browser | Yes                               | Yes          |
| Auto reconnect     | Yes                               | Manual       |
| Binary data        | No                                | Yes          |
| Best for           | Notifications, logs, AI streaming | Chat, games  |

Use SSE when:

* The client only needs updates
* You want simpler infrastructure
* You need HTTP compatibility

Use WebSockets when:

* Clients must send frequent real-time data
* You need bidirectional communication

---

# 14. Complete Minimal Example

```python
from flask import Flask, Response
import time

app = Flask(__name__)

@app.route("/stream")
def stream():

    def event_stream():
        i = 0

        while True:
            i += 1
            yield f"data: {i}\n\n"
            time.sleep(1)

    return Response(
        event_stream(),
        mimetype="text/event-stream"
    )

if __name__ == "__main__":
    app.run(threaded=True)
```

Frontend:

```html
<script>
    const source = new EventSource("/stream");

    source.onmessage = (event) => {
        console.log(event.data);
    };
</script>
```

---

# 15. Next Improvements

You can extend this by:

* Streaming database updates
* Broadcasting Redis messages
* Streaming AI responses token-by-token
* Real-time monitoring dashboards
* Progress bars for long tasks

For larger systems, developers often combine:

* Redis
* Celery
* Flask SSE endpoints

to build scalable streaming architectures.
