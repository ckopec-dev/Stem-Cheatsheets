# Asynchronous Programming in .NET 8 Tutorial

## Introduction

Asynchronous programming allows your application to remain responsive while performing long-running operations like file I/O, database queries, or web requests. In .NET 8, async/await makes this easier than ever.

## Understanding async/await

The `async` and `await` keywords are the foundation of asynchronous programming in .NET. They allow you to write asynchronous code that looks and behaves like synchronous code.

**Key Concepts:**
- **async**: A modifier that marks a method as asynchronous
- **await**: An operator that suspends execution until an awaited task completes
- **Task**: Represents an asynchronous operation
- **Task<T>**: Represents an asynchronous operation that returns a value

## Basic Syntax

Here's a simple example comparing synchronous and asynchronous methods:

```csharp
// Synchronous method
public string DownloadData(string url)
{
    using var client = new HttpClient();
    return client.GetStringAsync(url).Result; // Blocking!
}

// Asynchronous method
public async Task<string> DownloadDataAsync(string url)
{
    using var client = new HttpClient();
    return await client.GetStringAsync(url); // Non-blocking!
}
```

## Practical Examples

### Example 1: Making HTTP Requests

```csharp
public class WeatherService
{
    private readonly HttpClient _httpClient;

    public WeatherService()
    {
        _httpClient = new HttpClient();
    }

    public async Task<string> GetWeatherAsync(string city)
    {
        try
        {
            string url = $"https://api.example.com/weather?city={city}";
            string response = await _httpClient.GetStringAsync(url);
            return response;
        }
        catch (HttpRequestException ex)
        {
            Console.WriteLine($"Error fetching weather: {ex.Message}");
            return null;
        }
    }
}
```

### Example 2: File Operations

```csharp
public class FileService
{
    public async Task<string> ReadFileAsync(string filePath)
    {
        // ReadAllTextAsync is available in .NET
        return await File.ReadAllTextAsync(filePath);
    }

    public async Task WriteFileAsync(string filePath, string content)
    {
        await File.WriteAllTextAsync(filePath, content);
    }

    public async Task<List<string>> ReadLargeFileAsync(string filePath)
    {
        var lines = new List<string>();
        
        // Using async streams for efficient memory usage
        await foreach (string line in File.ReadLinesAsync(filePath))
        {
            lines.Add(line);
        }
        
        return lines;
    }
}
```

### Example 3: Running Multiple Tasks Concurrently

```csharp
public class DataAggregator
{
    public async Task<(string weather, string news, string stocks)> 
        GetAllDataAsync()
    {
        // Start all tasks simultaneously
        Task<string> weatherTask = GetWeatherAsync();
        Task<string> newsTask = GetNewsAsync();
        Task<string> stocksTask = GetStocksAsync();

        // Wait for all to complete
        await Task.WhenAll(weatherTask, newsTask, stocksTask);

        // Return results
        return (
            await weatherTask,
            await newsTask,
            await stocksTask
        );
    }

    private async Task<string> GetWeatherAsync()
    {
        await Task.Delay(1000); // Simulate API call
        return "Sunny, 75°F";
    }

    private async Task<string> GetNewsAsync()
    {
        await Task.Delay(1500); // Simulate API call
        return "Latest headlines...";
    }

    private async Task<string> GetStocksAsync()
    {
        await Task.Delay(800); // Simulate API call
        return "Market up 2%";
    }
}
```

### Example 4: Async Streams (IAsyncEnumerable)

.NET 8 supports async streams, which allow you to asynchronously iterate over data:

```csharp
public class DataGenerator
{
    public async IAsyncEnumerable<int> GenerateNumbersAsync(int count)
    {
        for (int i = 0; i < count; i++)
        {
            // Simulate async work
            await Task.Delay(100);
            yield return i;
        }
    }

    public async Task ProcessNumbersAsync()
    {
        await foreach (int number in GenerateNumbersAsync(10))
        {
            Console.WriteLine($"Processing: {number}");
        }
    }
}
```

### Example 5: Cancellation Tokens

Cancellation tokens allow you to cancel long-running operations:

```csharp
public class DownloadService
{
    public async Task<byte[]> DownloadFileAsync(
        string url, 
        CancellationToken cancellationToken)
    {
        using var client = new HttpClient();
        
        try
        {
            return await client.GetByteArrayAsync(url, cancellationToken);
        }
        catch (OperationCanceledException)
        {
            Console.WriteLine("Download was cancelled");
            throw;
        }
    }

    public async Task DownloadWithTimeoutAsync(string url)
    {
        using var cts = new CancellationTokenSource(TimeSpan.FromSeconds(5));
        
        try
        {
            var data = await DownloadFileAsync(url, cts.Token);
            Console.WriteLine($"Downloaded {data.Length} bytes");
        }
        catch (OperationCanceledException)
        {
            Console.WriteLine("Download timed out after 5 seconds");
        }
    }
}
```

## Best Practices

**Use async all the way down:** Once you start using async, continue using it throughout your call stack. Don't mix async and synchronous code with `.Result` or `.Wait()` as this can cause deadlocks.

**Avoid async void:** Only use `async void` for event handlers. For all other methods, return `Task` or `Task<T>` so errors can be properly handled.

**Use ConfigureAwait(false) in libraries:** When writing library code, use `ConfigureAwait(false)` to avoid capturing the synchronization context unnecessarily, which improves performance.

**Name methods with Async suffix:** Convention is to name async methods with an "Async" suffix, like `GetDataAsync()`.

**Don't block on async code:** Never use `.Result` or `.Wait()` on a Task in UI or ASP.NET applications, as this can cause deadlocks.

## Common Patterns

### Pattern 1: Retry Logic

```csharp
public async Task<T> RetryAsync<T>(
    Func<Task<T>> operation, 
    int maxRetries = 3)
{
    for (int i = 0; i < maxRetries; i++)
    {
        try
        {
            return await operation();
        }
        catch (Exception ex) when (i < maxRetries - 1)
        {
            await Task.Delay(TimeSpan.FromSeconds(Math.Pow(2, i)));
        }
    }
    
    throw new Exception($"Failed after {maxRetries} attempts");
}
```

### Pattern 2: Task.WhenAny for Timeout

```csharp
public async Task<string> GetDataWithTimeoutAsync(string url, TimeSpan timeout)
{
    using var client = new HttpClient();
    var dataTask = client.GetStringAsync(url);
    var timeoutTask = Task.Delay(timeout);

    var completedTask = await Task.WhenAny(dataTask, timeoutTask);

    if (completedTask == timeoutTask)
    {
        throw new TimeoutException("Request timed out");
    }

    return await dataTask;
}
```

## Console Application Example

```csharp
class Program
{
    static async Task Main(string[] args)
    {
        Console.WriteLine("Starting async operations...");

        var service = new DataAggregator();
        var (weather, news, stocks) = await service.GetAllDataAsync();

        Console.WriteLine($"Weather: {weather}");
        Console.WriteLine($"News: {news}");
        Console.WriteLine($"Stocks: {stocks}");

        Console.WriteLine("All operations completed!");
    }
}
```

## Performance Benefits

Asynchronous programming provides significant benefits in scenarios where operations involve waiting for external resources. Your application threads aren't blocked during the wait, allowing them to handle other work. This is particularly important in web applications where you want to maximize request throughput, and in UI applications where you want to keep the interface responsive.

By following these patterns and practices, you'll be able to write efficient, responsive applications that make the best use of system resources in .NET 8.
