# SignalR Tutorial

## Table of Contents
1. Introduction to SignalR
2. Setting Up Your Environment
3. Creating Your First SignalR Application
4. Understanding Hubs
5. Client-Side Implementation
6. Advanced Features
7. Best Practices
8. Troubleshooting

---

## 1. Introduction to SignalR

SignalR is an open-source library for ASP.NET that enables real-time web functionality. It allows server-side code to push content to connected clients instantly, rather than having the server wait for a client to request new data.

### What Can You Build with SignalR?

- Chat applications
- Live dashboards and monitoring systems
- Collaborative applications (like Google Docs)
- Real-time gaming
- Live notifications
- Stock tickers and financial applications
- Live auctions and bidding systems

### How SignalR Works

SignalR uses various transport mechanisms to establish the best possible connection between client and server:

1. **WebSockets** (preferred, if available)
2. **Server-Sent Events**
3. **Long Polling** (fallback)

SignalR automatically negotiates the best transport method based on server and client capabilities.

---

## 2. Setting Up Your Environment

### Prerequisites

- .NET 6.0 SDK or later
- Visual Studio 2022, VS Code, or your preferred IDE
- Basic knowledge of C# and JavaScript

### Creating a New Project

```bash
# Create a new ASP.NET Core Web App
dotnet new web -n SignalRDemo
cd SignalRDemo

# Add SignalR (included in ASP.NET Core by default)
```

### Installing Client Libraries

For JavaScript clients:
```bash
npm install @microsoft/signalr
```

Or use a CDN:
```html
<script src="https://cdnjs.cloudflare.com/ajax/libs/microsoft-signalr/7.0.0/signalr.min.js"></script>
```

---

## 3. Creating Your First SignalR Application

### Step 1: Create a Hub

Create a new file called `ChatHub.cs`:

```csharp
using Microsoft.AspNetCore.SignalR;

namespace SignalRDemo.Hubs
{
    public class ChatHub : Hub
    {
        public async Task SendMessage(string user, string message)
        {
            // Send message to all connected clients
            await Clients.All.SendAsync("ReceiveMessage", user, message);
        }
    }
}
```

### Step 2: Configure SignalR in Program.cs

```csharp
using SignalRDemo.Hubs;

var builder = WebApplication.CreateBuilder(args);

// Add SignalR services
builder.Services.AddSignalR();

var app = builder.Build();

// Enable static files
app.UseDefaultFiles();
app.UseStaticFiles();

// Map the hub endpoint
app.MapHub<ChatHub>("/chatHub");

app.Run();
```

### Step 3: Create the HTML Client

Create `wwwroot/index.html`:

```html
<!DOCTYPE html>
<html>
<head>
    <title>SignalR Chat</title>
    <style>
        body { font-family: Arial, sans-serif; max-width: 800px; margin: 50px auto; }
        #messagesList { list-style-type: none; padding: 0; }
        #messagesList li { padding: 8px; margin: 4px 0; background: #f0f0f0; border-radius: 4px; }
        input, button { padding: 8px; margin: 4px; }
        #messageInput { width: 300px; }
    </style>
</head>
<body>
    <h1>SignalR Chat Demo</h1>
    
    <div>
        <label>User: </label>
        <input type="text" id="userInput" />
        <label>Message: </label>
        <input type="text" id="messageInput" />
        <button id="sendButton">Send</button>
    </div>
    
    <hr />
    
    <ul id="messagesList"></ul>
    
    <script src="https://cdnjs.cloudflare.com/ajax/libs/microsoft-signalr/7.0.0/signalr.min.js"></script>
    <script src="js/chat.js"></script>
</body>
</html>
```

### Step 4: Create the JavaScript Client

Create `wwwroot/js/chat.js`:

```javascript
// Create connection
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withAutomaticReconnect()
    .build();

// Disable send button until connection is established
document.getElementById("sendButton").disabled = true;

// Receive messages from server
connection.on("ReceiveMessage", (user, message) => {
    const li = document.createElement("li");
    li.textContent = `${user}: ${message}`;
    document.getElementById("messagesList").appendChild(li);
});

// Start the connection
connection.start()
    .then(() => {
        document.getElementById("sendButton").disabled = false;
        console.log("Connected to SignalR hub");
    })
    .catch(err => console.error(err.toString()));

// Send message to server
document.getElementById("sendButton").addEventListener("click", () => {
    const user = document.getElementById("userInput").value;
    const message = document.getElementById("messageInput").value;
    
    connection.invoke("SendMessage", user, message)
        .catch(err => console.error(err.toString()));
    
    document.getElementById("messageInput").value = "";
});

// Allow Enter key to send message
document.getElementById("messageInput").addEventListener("keypress", (e) => {
    if (e.key === "Enter") {
        document.getElementById("sendButton").click();
    }
});
```

### Running the Application

```bash
dotnet run
```

Visit `https://localhost:5001` in multiple browser windows to test the chat functionality.

---

## 4. Understanding Hubs

Hubs are the central component of SignalR. They handle communication between server and clients.

### Hub Methods

```csharp
public class DemoHub : Hub
{
    // Send to all clients
    public async Task BroadcastMessage(string message)
    {
        await Clients.All.SendAsync("ReceiveMessage", message);
    }
    
    // Send to specific client
    public async Task SendToClient(string connectionId, string message)
    {
        await Clients.Client(connectionId).SendAsync("ReceiveMessage", message);
    }
    
    // Send to all except caller
    public async Task SendToOthers(string message)
    {
        await Clients.Others.SendAsync("ReceiveMessage", message);
    }
    
    // Send to caller only
    public async Task SendToCaller(string message)
    {
        await Clients.Caller.SendAsync("ReceiveMessage", message);
    }
    
    // Send to specific group
    public async Task SendToGroup(string groupName, string message)
    {
        await Clients.Group(groupName).SendAsync("ReceiveMessage", message);
    }
}
```

### Connection Lifecycle

```csharp
public class ChatHub : Hub
{
    public override async Task OnConnectedAsync()
    {
        await Clients.All.SendAsync("UserConnected", Context.ConnectionId);
        await base.OnConnectedAsync();
    }
    
    public override async Task OnDisconnectedAsync(Exception exception)
    {
        await Clients.All.SendAsync("UserDisconnected", Context.ConnectionId);
        await base.OnDisconnectedAsync(exception);
    }
}
```

### Groups

Groups allow you to broadcast to subsets of connections:

```csharp
public class ChatHub : Hub
{
    public async Task JoinRoom(string roomName)
    {
        await Groups.AddToGroupAsync(Context.ConnectionId, roomName);
        await Clients.Group(roomName).SendAsync("UserJoined", Context.ConnectionId);
    }
    
    public async Task LeaveRoom(string roomName)
    {
        await Groups.RemoveFromGroupAsync(Context.ConnectionId, roomName);
        await Clients.Group(roomName).SendAsync("UserLeft", Context.ConnectionId);
    }
    
    public async Task SendMessageToRoom(string roomName, string message)
    {
        await Clients.Group(roomName).SendAsync("ReceiveMessage", message);
    }
}
```

---

## 5. Client-Side Implementation

### JavaScript Client

#### Connection Configuration

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/hub", {
        accessTokenFactory: () => "your-jwt-token" // For authentication
    })
    .withAutomaticReconnect([0, 2000, 5000, 10000]) // Custom retry intervals
    .configureLogging(signalR.LogLevel.Information)
    .build();
```

#### Handling Reconnection

```javascript
connection.onreconnecting((error) => {
    console.log("Connection lost. Reconnecting...", error);
    document.getElementById("status").textContent = "Reconnecting...";
});

connection.onreconnected((connectionId) => {
    console.log("Reconnected with ID:", connectionId);
    document.getElementById("status").textContent = "Connected";
});

connection.onclose((error) => {
    console.log("Connection closed", error);
    document.getElementById("status").textContent = "Disconnected";
});
```

#### Invoking Server Methods

```javascript
// Simple invocation
connection.invoke("MethodName", arg1, arg2);

// With promise handling
connection.invoke("MethodName", arg1, arg2)
    .then(result => console.log("Success:", result))
    .catch(err => console.error("Error:", err));

// With return value
const result = await connection.invoke("GetData", id);
```

### .NET Client

```csharp
using Microsoft.AspNetCore.SignalR.Client;

var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/hub")
    .WithAutomaticReconnect()
    .Build();

// Register handler
connection.On<string, string>("ReceiveMessage", (user, message) =>
{
    Console.WriteLine($"{user}: {message}");
});

// Start connection
await connection.StartAsync();

// Invoke method
await connection.InvokeAsync("SendMessage", "User", "Hello!");

// Stop connection
await connection.StopAsync();
```

---

## 6. Advanced Features

### Strongly Typed Hubs

Define an interface for client methods:

```csharp
public interface IChatClient
{
    Task ReceiveMessage(string user, string message);
    Task UserJoined(string connectionId);
    Task UserLeft(string connectionId);
}

public class ChatHub : Hub<IChatClient>
{
    public async Task SendMessage(string user, string message)
    {
        // Now you get IntelliSense and compile-time checking
        await Clients.All.ReceiveMessage(user, message);
    }
}
```

### Authentication and Authorization

```csharp
using Microsoft.AspNetCore.Authorization;

[Authorize]
public class SecureHub : Hub
{
    public async Task SendMessage(string message)
    {
        var userName = Context.User.Identity.Name;
        await Clients.All.SendAsync("ReceiveMessage", userName, message);
    }
    
    [Authorize(Roles = "Admin")]
    public async Task AdminBroadcast(string message)
    {
        await Clients.All.SendAsync("AdminMessage", message);
    }
}
```

### Streaming

Server to client streaming:

```csharp
public class StreamHub : Hub
{
    public async IAsyncEnumerable<int> Counter(
        int count,
        int delay,
        CancellationToken cancellationToken)
    {
        for (var i = 0; i < count; i++)
        {
            cancellationToken.ThrowIfCancellationRequested();
            yield return i;
            await Task.Delay(delay, cancellationToken);
        }
    }
}
```

Client code:

```javascript
connection.stream("Counter", 10, 1000)
    .subscribe({
        next: (item) => console.log(item),
        complete: () => console.log("Stream completed"),
        error: (err) => console.error(err)
    });
```

### Dependency Injection

```csharp
public class NotificationHub : Hub
{
    private readonly INotificationService _notificationService;
    
    public NotificationHub(INotificationService notificationService)
    {
        _notificationService = notificationService;
    }
    
    public async Task SendNotification(string message)
    {
        await _notificationService.ProcessNotification(message);
        await Clients.All.SendAsync("ReceiveNotification", message);
    }
}
```

### Sending Messages from Outside Hubs

```csharp
public class NotificationService
{
    private readonly IHubContext<NotificationHub> _hubContext;
    
    public NotificationService(IHubContext<NotificationHub> hubContext)
    {
        _hubContext = hubContext;
    }
    
    public async Task SendNotificationToAll(string message)
    {
        await _hubContext.Clients.All.SendAsync("ReceiveNotification", message);
    }
    
    public async Task SendToUser(string userId, string message)
    {
        await _hubContext.Clients.User(userId).SendAsync("ReceiveNotification", message);
    }
}
```

---

## 7. Best Practices

### Performance Optimization

1. **Use Message Pack Protocol** for binary serialization:
```csharp
builder.Services.AddSignalR()
    .AddMessagePackProtocol();
```

2. **Enable Response Compression**:
```csharp
builder.Services.AddResponseCompression(opts =>
{
    opts.MimeTypes = ResponseCompressionDefaults.MimeTypes.Concat(
        new[] { "application/octet-stream" });
});
```

3. **Scale Out with Redis**:
```csharp
builder.Services.AddSignalR()
    .AddStackExchangeRedis("localhost:6379");
```

### Security Best Practices

1. Always use HTTPS in production
2. Implement proper authentication and authorization
3. Validate all input from clients
4. Use CORS policies appropriately:

```csharp
builder.Services.AddCors(options =>
{
    options.AddPolicy("ClientPermission", policy =>
    {
        policy.WithOrigins("https://example.com")
              .AllowAnyHeader()
              .AllowAnyMethod()
              .AllowCredentials();
    });
});

app.UseCors("ClientPermission");
```

### Error Handling

```csharp
public class ChatHub : Hub
{
    public async Task SendMessage(string message)
    {
        try
        {
            if (string.IsNullOrWhiteSpace(message))
            {
                throw new HubException("Message cannot be empty");
            }
            
            await Clients.All.SendAsync("ReceiveMessage", message);
        }
        catch (Exception ex)
        {
            // Log the error
            Console.WriteLine($"Error: {ex.Message}");
            throw new HubException("Failed to send message");
        }
    }
}
```

Client-side error handling:

```javascript
connection.invoke("SendMessage", message)
    .catch(err => {
        if (err.message) {
            alert("Error: " + err.message);
        }
    });
```

---

## 8. Troubleshooting

### Common Issues and Solutions

**Connection fails immediately**
- Check that the hub is properly mapped in Program.cs
- Verify the URL path matches between client and server
- Ensure CORS is configured if needed

**Messages not received**
- Verify method names match exactly (case-sensitive)
- Check that handlers are registered before starting connection
- Ensure connection is started successfully

**Frequent disconnections**
- Check server and network stability
- Implement automatic reconnection
- Consider increasing timeout values

### Debugging Tips

Enable detailed logging:

```csharp
// Server-side
builder.Logging.AddFilter("Microsoft.AspNetCore.SignalR", LogLevel.Debug);

// Client-side
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/hub")
    .configureLogging(signalR.LogLevel.Debug)
    .build();
```

Monitor connections:

```csharp
public class ChatHub : Hub
{
    public override async Task OnConnectedAsync()
    {
        var connectionId = Context.ConnectionId;
        var userId = Context.User?.Identity?.Name;
        Console.WriteLine($"Connected: {connectionId}, User: {userId}");
        await base.OnConnectedAsync();
    }
}
```

---

## Conclusion

SignalR provides a powerful framework for building real-time web applications. This tutorial covered the fundamentals and advanced features to get you started. For more information, visit the official [Microsoft SignalR documentation](https://docs.microsoft.com/aspnet/core/signalr).

### Next Steps

- Build a real-time dashboard
- Implement a notification system
- Create a collaborative editing tool
- Explore scaling with Azure SignalR Service

Happy coding!
