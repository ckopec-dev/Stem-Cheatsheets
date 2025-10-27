# Complete .NET Core appsettings Tutorial

## Table of Contents

1. [Introduction](#introduction)
2. [Basic Configuration](#basic-configuration)
3. [Configuration File Hierarchy](#configuration-file-hierarchy)
4. [Reading Configuration Values](#reading-configuration-values)
5. [Strongly Typed Configuration](#strongly-typed-configuration)
6. [Environment-Specific Configuration](#environment-specific-configuration)
7. [Configuration Sources](#configuration-sources)
8. [Advanced Scenarios](#advanced-scenarios)
9. [Security Best Practices](#security-best-practices)
10. [Common Patterns and Examples](#common-patterns-and-examples)

## Introduction

The `appsettings.json` file is the primary configuration file in .NET Core applications. It provides a centralized location for storing application settings, connection strings, logging configurations, and other runtime parameters. .NET Core's configuration system is flexible, hierarchical, and supports multiple sources.

## Basic Configuration

### Creating appsettings.json

Create an `appsettings.json` file in your project root:

```json
{
  "Logging": {
    "LogLevel": {
      "Default": "Information",
      "Microsoft.AspNetCore": "Warning"
    }
  },
  "ConnectionStrings": {
    "DefaultConnection": "Server=localhost;Database=MyApp;Trusted_Connection=true;"
  },
  "AppSettings": {
    "ApplicationName": "My Application",
    "Version": "1.0.0",
    "MaxRetryAttempts": 3,
    "EnableFeatureX": true
  }
}
```

### Project Configuration

Ensure the file is included in your project and copied to output:

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">
  <PropertyGroup>
    <TargetFramework>net8.0</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <Content Include="appsettings.json">
      <CopyToOutputDirectory>PreserveNewest</CopyToOutputDirectory>
    </Content>
  </ItemGroup>
</Project>
```

## Configuration File Hierarchy

.NET Core follows a specific order when loading configuration files:

1. `appsettings.json`
2. `appsettings.{Environment}.json`
3. Environment variables
4. Command-line arguments
5. User secrets (in development)

Later sources override earlier ones, creating a hierarchical configuration system.

## Reading Configuration Values

### Method 1: IConfiguration Interface

```csharp
public class HomeController : Controller
{
    private readonly IConfiguration _configuration;

    public HomeController(IConfiguration configuration)
    {
        _configuration = configuration;
    }

    public IActionResult Index()
    {
        var appName = _configuration["AppSettings:ApplicationName"];
        var connectionString = _configuration.GetConnectionString("DefaultConnection");
        var maxRetries = _configuration.GetValue<int>("AppSettings:MaxRetryAttempts");
        var isFeatureEnabled = _configuration.GetValue<bool>("AppSettings:EnableFeatureX");

        return View();
    }
}
```

### Method 2: Configuration Sections

```csharp
public IActionResult GetSettings()
{
    var appSettingsSection = _configuration.GetSection("AppSettings");
    var appName = appSettingsSection["ApplicationName"];
    var version = appSettingsSection["Version"];

    var loggingSection = _configuration.GetSection("Logging:LogLevel");
    var defaultLogLevel = loggingSection["Default"];

    return Ok(new { appName, version, defaultLogLevel });
}
```

## Strongly Typed Configuration

### Creating Configuration Classes

```csharp
public class AppSettings
{
    public string ApplicationName { get; set; }
    public string Version { get; set; }
    public int MaxRetryAttempts { get; set; }
    public bool EnableFeatureX { get; set; }
    public DatabaseSettings Database { get; set; }
    public EmailSettings Email { get; set; }
}

public class DatabaseSettings
{
    public string ConnectionString { get; set; }
    public int CommandTimeout { get; set; }
    public bool EnableRetry { get; set; }
}

public class EmailSettings
{
    public string SmtpServer { get; set; }
    public int Port { get; set; }
    public string Username { get; set; }
    public string Password { get; set; }
    public bool UseSSL { get; set; }
}
```

### Corresponding appsettings.json

```json
{
  "AppSettings": {
    "ApplicationName": "My Application",
    "Version": "1.0.0",
    "MaxRetryAttempts": 3,
    "EnableFeatureX": true,
    "Database": {
      "ConnectionString": "Server=localhost;Database=MyApp;Trusted_Connection=true;",
      "CommandTimeout": 30,
      "EnableRetry": true
    },
    "Email": {
      "SmtpServer": "smtp.gmail.com",
      "Port": 587,
      "Username": "your-email@gmail.com",
      "Password": "your-password",
      "UseSSL": true
    }
  }
}
```

### Registering Configuration Classes

In `Program.cs` (.NET 6+):

```csharp
var builder = WebApplication.CreateBuilder(args);

// Register strongly typed configuration
builder.Services.Configure<AppSettings>(builder.Configuration.GetSection("AppSettings"));

// Alternative: Register individual sections
builder.Services.Configure<DatabaseSettings>(builder.Configuration.GetSection("AppSettings:Database"));
builder.Services.Configure<EmailSettings>(builder.Configuration.GetSection("AppSettings:Email"));

var app = builder.Build();
```

### Using Strongly Typed Configuration

```csharp
public class EmailService
{
    private readonly EmailSettings _emailSettings;

    public EmailService(IOptions<EmailSettings> emailSettings)
    {
        _emailSettings = emailSettings.Value;
    }

    public async Task SendEmailAsync(string to, string subject, string body)
    {
        // Use _emailSettings.SmtpServer, _emailSettings.Port, etc.
        using var client = new SmtpClient(_emailSettings.SmtpServer, _emailSettings.Port);
        client.EnableSsl = _emailSettings.UseSSL;
        client.Credentials = new NetworkCredential(_emailSettings.Username, _emailSettings.Password);
        
        var message = new MailMessage("from@example.com", to, subject, body);
        await client.SendMailAsync(message);
    }
}
```

### IOptionsMonitor for Dynamic Updates

```csharp
public class ConfigurableService
{
    private readonly IOptionsMonitor<AppSettings> _optionsMonitor;

    public ConfigurableService(IOptionsMonitor<AppSettings> optionsMonitor)
    {
        _optionsMonitor = optionsMonitor;
        
        // React to configuration changes
        _optionsMonitor.OnChange((settings, name) =>
        {
            // Handle configuration changes
            Console.WriteLine($"Configuration changed: {settings.ApplicationName}");
        });
    }

    public void DoWork()
    {
        // Always get current configuration
        var currentSettings = _optionsMonitor.CurrentValue;
        Console.WriteLine($"Current app name: {currentSettings.ApplicationName}");
    }
}
```

## Environment-Specific Configuration

### Multiple Configuration Files

Create environment-specific files:

- `appsettings.json` (base configuration)
- `appsettings.Development.json`
- `appsettings.Staging.json`
- `appsettings.Production.json`

### appsettings.Development.json

```json
{
  "Logging": {
    "LogLevel": {
      "Default": "Debug",
      "Microsoft.AspNetCore": "Information"
    }
  },
  "ConnectionStrings": {
    "DefaultConnection": "Server=localhost;Database=MyApp_Dev;Trusted_Connection=true;"
  },
  "AppSettings": {
    "ApplicationName": "My Application (Development)",
    "EnableFeatureX": false
  }
}
```

### appsettings.Production.json

```json
{
  "Logging": {
    "LogLevel": {
      "Default": "Warning",
      "Microsoft.AspNetCore": "Error"
    }
  },
  "ConnectionStrings": {
    "DefaultConnection": "Server=prod-server;Database=MyApp_Prod;User Id=appuser;Password=secure_password;"
  },
  "AppSettings": {
    "ApplicationName": "My Application",
    "EnableFeatureX": true,
    "MaxRetryAttempts": 5
  }
}
```

### Setting Environment

Set the `ASPNETCORE_ENVIRONMENT` variable:

```bash
# Development
export ASPNETCORE_ENVIRONMENT=Development

# Production
export ASPNETCORE_ENVIRONMENT=Production

# Or in launchSettings.json
{
  "profiles": {
    "Development": {
      "commandName": "Project",
      "environmentVariables": {
        "ASPNETCORE_ENVIRONMENT": "Development"
      }
    }
  }
}
```

## Configuration Sources

### Environment Variables

Override any configuration setting using environment variables with double underscore notation:

```bash
# Override AppSettings:ApplicationName
export AppSettings__ApplicationName="Overridden App Name"

# Override nested settings
export AppSettings__Database__ConnectionString="New Connection String"

# Connection strings have a special prefix
export ConnectionStrings__DefaultConnection="Environment Connection String"
```

### Command Line Arguments

```bash
dotnet run --AppSettings:ApplicationName="Command Line App" --ConnectionStrings:DefaultConnection="Command Line Connection"
```

### User Secrets (Development Only)

Initialize user secrets:

```bash
dotnet user-secrets init
dotnet user-secrets set "AppSettings:Email:Password" "secret-password"
dotnet user-secrets set "ConnectionStrings:DefaultConnection" "secret-connection-string"
```

### Custom Configuration Providers

```csharp
public class DatabaseConfigurationProvider : ConfigurationProvider
{
    private readonly string _connectionString;

    public DatabaseConfigurationProvider(string connectionString)
    {
        _connectionString = connectionString;
    }

    public override void Load()
    {
        // Load configuration from database
        var configurations = LoadFromDatabase();
        
        foreach (var config in configurations)
        {
            Data[config.Key] = config.Value;
        }
    }

    private Dictionary<string, string> LoadFromDatabase()
    {
        // Implementation to load from database
        return new Dictionary<string, string>
        {
            ["AppSettings:ApplicationName"] = "Database App Name",
            ["AppSettings:MaxRetryAttempts"] = "10"
        };
    }
}

public class DatabaseConfigurationSource : IConfigurationSource
{
    private readonly string _connectionString;

    public DatabaseConfigurationSource(string connectionString)
    {
        _connectionString = connectionString;
    }

    public IConfigurationProvider Build(IConfigurationBuilder builder)
    {
        return new DatabaseConfigurationProvider(_connectionString);
    }
}

// Register in Program.cs
var builder = WebApplication.CreateBuilder(args);

builder.Configuration.Add(new DatabaseConfigurationSource("connection-string"));
```

## Advanced Scenarios

### Configuration Validation

```csharp
public class AppSettings
{
    public const string SectionName = "AppSettings";
    
    [Required]
    [MinLength(3)]
    public string ApplicationName { get; set; }
    
    [Range(1, 10)]
    public int MaxRetryAttempts { get; set; }
    
    [Required]
    [EmailAddress]
    public string AdminEmail { get; set; }
    
    [Url]
    public string BaseUrl { get; set; }
}

// In Program.cs
builder.Services.Configure<AppSettings>(builder.Configuration.GetSection(AppSettings.SectionName));
builder.Services.AddSingleton<IValidateOptions<AppSettings>, ValidateOptionsService>();

public class ValidateOptionsService : IValidateOptions<AppSettings>
{
    public ValidateOptionsResult Validate(string name, AppSettings options)
    {
        var results = new List<ValidationResult>();
        var context = new ValidationContext(options);
        
        if (!Validator.TryValidateObject(options, context, results, true))
        {
            var errors = results.Select(r => r.ErrorMessage).ToList();
            return ValidateOptionsResult.Fail(errors);
        }
        
        return ValidateOptionsResult.Success;
    }
}
```

### Configuration Binding with Custom Converters

```csharp
public class CustomTypeConverter : TypeConverter
{
    public override bool CanConvertFrom(ITypeDescriptorContext context, Type sourceType)
    {
        return sourceType == typeof(string) || base.CanConvertFrom(context, sourceType);
    }

    public override object ConvertFrom(ITypeDescriptorContext context, CultureInfo culture, object value)
    {
        if (value is string stringValue)
        {
            // Custom conversion logic
            return new CustomType(stringValue);
        }
        return base.ConvertFrom(context, culture, value);
    }
}

// Register converter
TypeDescriptor.AddAttributes(typeof(CustomType), new TypeConverterAttribute(typeof(CustomTypeConverter)));
```

### Conditional Configuration Loading

```csharp
var builder = WebApplication.CreateBuilder(args);

// Load different configurations based on conditions
if (builder.Environment.IsDevelopment())
{
    builder.Configuration.AddJsonFile("appsettings.local.json", optional: true);
}

if (builder.Configuration.GetValue<bool>("UseAzureKeyVault"))
{
    var keyVaultUrl = builder.Configuration["KeyVaultUrl"];
    // Add Azure Key Vault configuration
}
```

### Configuration Caching and Performance

```csharp
public class CachedConfigurationService
{
    private readonly IOptionsMonitor<AppSettings> _options;
    private readonly IMemoryCache _cache;
    private readonly TimeSpan _cacheExpiry = TimeSpan.FromMinutes(5);

    public CachedConfigurationService(IOptionsMonitor<AppSettings> options, IMemoryCache cache)
    {
        _options = options;
        _cache = cache;
    }

    public AppSettings GetSettings()
    {
        return _cache.GetOrCreate("AppSettings", factory =>
        {
            factory.AbsoluteExpirationRelativeToNow = _cacheExpiry;
            return _options.CurrentValue;
        });
    }
}
```

## Security Best Practices

### Protecting Sensitive Configuration

1. **Never store secrets in appsettings.json**
2. **Use User Secrets for development**
3. **Use environment variables or Key Vault for production**
4. **Implement configuration encryption**

```csharp
public class EncryptedConfigurationProvider : ConfigurationProvider
{
    public override void Load()
    {
        // Load encrypted configuration and decrypt
        var encryptedConfig = File.ReadAllText("encrypted-config.json");
        var decryptedConfig = DecryptConfiguration(encryptedConfig);
        
        var configData = JsonSerializer.Deserialize<Dictionary<string, object>>(decryptedConfig);
        Data = FlattenDictionary(configData);
    }

    private string DecryptConfiguration(string encrypted)
    {
        // Implement your decryption logic
        return encrypted; // Placeholder
    }
}
```

### Azure Key Vault Integration

```csharp
// Install: Microsoft.Extensions.Configuration.AzureKeyVault
var builder = WebApplication.CreateBuilder(args);

if (builder.Environment.IsProduction())
{
    var keyVaultUrl = builder.Configuration["KeyVaultUrl"];
    builder.Configuration.AddAzureKeyVault(new Uri(keyVaultUrl), new DefaultAzureCredential());
}
```

### Environment Variable Security

```bash
# Secure way to set sensitive environment variables
export ConnectionStrings__DefaultConnection="$(cat /run/secrets/db_connection)"
export AppSettings__ApiKey="$(cat /run/secrets/api_key)"
```

## Common Patterns and Examples

### Feature Flags Configuration

```json
{
  "FeatureFlags": {
    "EnableNewUI": true,
    "EnableBetaFeatures": false,
    "MaintenanceMode": false,
    "Features": {
      "UserRegistration": {
        "Enabled": true,
        "RequireEmailConfirmation": true
      },
      "PaymentProcessing": {
        "Enabled": true,
        "Provider": "Stripe",
        "TestMode": false
      }
    }
  }
}
```

```csharp
public class FeatureFlags
{
    public bool EnableNewUI { get; set; }
    public bool EnableBetaFeatures { get; set; }
    public bool MaintenanceMode { get; set; }
    public Dictionary<string, FeatureConfig> Features { get; set; }
}

public class FeatureConfig
{
    public bool Enabled { get; set; }
    public Dictionary<string, object> Properties { get; set; }
}
```

### Multi-tenant Configuration

```json
{
  "Tenants": {
    "tenant1": {
      "DatabaseConnection": "Server=db1;Database=Tenant1;",
      "StorageAccount": "tenant1storage",
      "Features": {
        "AdvancedReporting": true
      }
    },
    "tenant2": {
      "DatabaseConnection": "Server=db2;Database=Tenant2;",
      "StorageAccount": "tenant2storage",
      "Features": {
        "AdvancedReporting": false
      }
    }
  }
}
```

### Logging Configuration

```json
{
  "Logging": {
    "LogLevel": {
      "Default": "Information",
      "Microsoft": "Warning",
      "Microsoft.Hosting.Lifetime": "Information"
    },
    "Console": {
      "IncludeScopes": false,
      "TimestampFormat": "yyyy-MM-dd HH:mm:ss "
    },
    "File": {
      "Path": "logs/app-{Date}.log",
      "MinLevel": "Information",
      "OutputTemplate": "{Timestamp:yyyy-MM-dd HH:mm:ss.fff} [{Level:u3}] {Message:lj}{NewLine}{Exception}"
    }
  }
}
```

### API Configuration

```json
{
  "ApiSettings": {
    "BaseUrl": "https://api.example.com",
    "Timeout": "00:00:30",
    "RetryPolicy": {
      "MaxRetries": 3,
      "DelayBetweenRetries": "00:00:02",
      "ExponentialBackoff": true
    },
    "Endpoints": {
      "Users": "/api/v1/users",
      "Orders": "/api/v1/orders",
      "Products": "/api/v1/products"
    },
    "Authentication": {
      "Type": "Bearer",
      "TokenEndpoint": "/oauth/token"
    }
  }
}
```

### Complete Example: E-commerce Application

```json
{
  "Logging": {
    "LogLevel": {
      "Default": "Information"
    }
  },
  "ConnectionStrings": {
    "DefaultConnection": "Server=localhost;Database=ECommerceDb;Trusted_Connection=true;",
    "RedisCache": "localhost:6379"
  },
  "AppSettings": {
    "ApplicationName": "E-Commerce Platform",
    "Version": "2.1.0",
    "Environment": "Production",
    "Database": {
      "CommandTimeout": 30,
      "EnableRetry": true,
      "MaxRetryCount": 3
    },
    "Cache": {
      "DefaultExpiration": "01:00:00",
      "SlidingExpiration": "00:15:00",
      "EnableDistributedCache": true
    },
    "Email": {
      "SmtpServer": "smtp.company.com",
      "Port": 587,
      "UseSSL": true,
      "FromAddress": "noreply@company.com",
      "FromName": "E-Commerce Platform"
    },
    "Payment": {
      "Provider": "Stripe",
      "PublishableKey": "pk_live_...",
      "WebhookSecret": "whsec_...",
      "Currency": "USD"
    },
    "Storage": {
      "Provider": "Azure",
      "ConnectionString": "DefaultEndpointsProtocol=https;...",
      "ContainerName": "product-images"
    },
    "Security": {
      "JwtSecret": "your-secret-key",
      "JwtExpiration": "24:00:00",
      "RequireHttps": true,
      "EnableCors": true,
      "AllowedOrigins": ["https://www.company.com", "https://admin.company.com"]
    },
    "Features": {
      "EnableProductRecommendations": true,
      "EnableRealTimeInventory": true,
      "EnableAdvancedSearch": false,
      "MaintenanceMode": false
    }
  }
}
```

This comprehensive tutorial covers all aspects of working with appsettings in .NET Core, from basic configuration reading to advanced scenarios like custom providers, validation, and security best practices. Use these patterns and examples as a foundation for building robust, configurable .NET Core applications.
