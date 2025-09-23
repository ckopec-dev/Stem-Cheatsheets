# Plugin Tutorial 

* a simple attribute + plugin interface
* a loader that finds plugins in the current AppDomain or a folder
* an example plugin
* an advanced variant using `AssemblyLoadContext` (unloadable plugins)
* notes on safety, DI, versioning, and improvements

# 1) Core contract & attribute

```csharp
// IPlugin.cs
public interface IPlugin
{
    string Name { get; }
    void Initialize(IServiceProvider? services = null);
    void Execute();
}

// PluginAttribute.cs
[AttributeUsage(AttributeTargets.Class, AllowMultiple = false, Inherited = false)]
public sealed class PluginAttribute : Attribute
{
    public string? Id { get; }
    public string? Version { get; }

    public PluginAttribute(string? id = null, string? version = null)
    {
        Id = id;
        Version = version;
    }
}
```

* `IPlugin` is the runtime contract.
* `PluginAttribute` marks classes as plugins and optionally carries metadata (id/version).

# 2) A basic plugin implementation (example)

```csharp
// HelloWorldPlugin.cs (in a plugin assembly)
[Plugin(id: "HelloWorld", version: "1.0.0")]
public class HelloWorldPlugin : IPlugin
{
    public string Name => "Hello World Plugin";

    public void Initialize(IServiceProvider? services = null)
    {
        // optional init (resolve services if host provides them)
    }

    public void Execute()
    {
        Console.WriteLine("Hello from the plugin!");
    }
}
```

# 3) Simple reflection-based plugin loader (search loaded assemblies)

```csharp
using System.Reflection;

public class PluginDescriptor
{
    public Type Type { get; init; }
    public PluginAttribute Attribute { get; init; }
    public IPlugin? Instance { get; set; }
}

public class ReflectionPluginLoader
{
    public IEnumerable<PluginDescriptor> DiscoverFromAssemblies(IEnumerable<Assembly> assemblies)
    {
        foreach (var asm in assemblies)
        {
            Type[] types;
            try { types = asm.GetTypes(); }
            catch (ReflectionTypeLoadException ex) { types = ex.Types.Where(t => t != null).ToArray()!; }

            foreach (var t in types)
            {
                if (t == null) continue;
                if (!typeof(IPlugin).IsAssignableFrom(t)) continue; // must implement IPlugin
                var attr = t.GetCustomAttribute<PluginAttribute>(inherit: false);
                if (attr == null) continue;

                yield return new PluginDescriptor { Type = t, Attribute = attr };
            }
        }
    }

    public IEnumerable<IPlugin> InstantiatePlugins(IEnumerable<PluginDescriptor> descriptors, IServiceProvider? services = null)
    {
        foreach (var desc in descriptors)
        {
            var ctor = desc.Type.GetConstructors()
                        .OrderBy(c => c.GetParameters().Length) // pick simplest ctor (example)
                        .FirstOrDefault();

            if (ctor == null) continue;

            var parameters = ctor.GetParameters();
            object? instance;
            if (parameters.Length == 0)
            {
                instance = Activator.CreateInstance(desc.Type);
            }
            else
            {
                // naive: if IServiceProvider injected, pass it
                var args = parameters.Select(p =>
                {
                    if (p.ParameterType == typeof(IServiceProvider) || p.ParameterType == typeof(IServiceProvider?))
                        return services;
                    return p.HasDefaultValue ? p.DefaultValue : null;
                }).ToArray();

                instance = Activator.CreateInstance(desc.Type, args);
            }

            if (instance is IPlugin plugin)
            {
                plugin.Initialize(services);
                desc.Instance = plugin;
                yield return plugin;
            }
        }
    }
}
```

Usage (discover plugins from currently loaded assemblies — typically your host and any referenced plugin dlls loaded at startup):

```csharp
var loader = new ReflectionPluginLoader();
var descriptors = loader.DiscoverFromAssemblies(AppDomain.CurrentDomain.GetAssemblies()).ToList();
var plugins = loader.InstantiatePlugins(descriptors).ToList();

foreach(var p in plugins)
    p.Execute();
```

# 4) Loading plugin assemblies from a folder

If you want drop-in plugins (DLLs in `./plugins`), load them and discover:

```csharp
string folder = Path.Combine(AppContext.BaseDirectory, "plugins");
Directory.CreateDirectory(folder);

var dlls = Directory.EnumerateFiles(folder, "*.dll", SearchOption.TopDirectoryOnly);
var assemblies = new List<Assembly>();
foreach (var dll in dlls)
{
    try { assemblies.Add(Assembly.LoadFrom(dll)); }
    catch (Exception ex) { Console.WriteLine($"Failed to load {dll}: {ex.Message}"); }
}

var descriptors = loader.DiscoverFromAssemblies(assemblies);
var plugins = loader.InstantiatePlugins(descriptors);
```

Caveat: `Assembly.LoadFrom` will lock files and keep them loaded in the default context so you can't easily update/unload them. For unloadable plugins see the next section.

# 5) Advanced: unloadable plugins using AssemblyLoadContext (for .NET Core/.NET 5+)

This pattern lets you load plugin assemblies into a collectible `AssemblyLoadContext`, then unload them. Short version:

```csharp
using System.Runtime.Loader;

public class CollectiblePluginLoadContext : AssemblyLoadContext
{
    private AssemblyDependencyResolver _resolver;

    public CollectiblePluginLoadContext(string pluginPath) : base(isCollectible: true)
    {
        _resolver = new AssemblyDependencyResolver(pluginPath);
    }

    protected override Assembly? Load(AssemblyName assemblyName)
    {
        string? path = _resolver.ResolveAssemblyToPath(assemblyName);
        if (path != null) return LoadFromAssemblyPath(path);
        return null;
    }
}

public class UnloadablePluginHost
{
    public (IEnumerable<IPlugin> plugins, AssemblyLoadContext alc) LoadPluginsFrom(string pluginAssemblyPath)
    {
        var alc = new CollectiblePluginLoadContext(pluginAssemblyPath);
        var assembly = alc.LoadFromAssemblyPath(pluginAssemblyPath);

        var loader = new ReflectionPluginLoader();
        var descriptors = loader.DiscoverFromAssemblies(new[] { assembly }).ToList();
        var plugins = loader.InstantiatePlugins(descriptors).ToList();
        return (plugins, alc);
    }

    public static async Task UnloadAsync(AssemblyLoadContext alc)
    {
        alc.Unload();
        // give the GC a chance to collect the context
        for (int i = 0; i < 10 && GC.GetGeneration(alc) >= 0; i++)
        {
            GC.Collect();
            GC.WaitForPendingFinalizers();
            await Task.Delay(100);
        }
    }
}
```

Usage:

```csharp
var host = new UnloadablePluginHost();
var (plugins, alc) = host.LoadPluginsFrom(@"C:\path\to\MyPlugin.dll");

// run plugins
foreach(var p in plugins) p.Execute();

// when ready to remove:
await UnloadablePluginHost.UnloadAsync(alc);
// now the assembly file can be replaced/deleted
```

Important: unloading only succeeds when no managed references to types from the plugin assembly remain (including static events, timers, unresolved delegates). Clean your references.

# 6) Suggestions & best practices

* **Security**: plugins mean executing third-party code. Run untrusted plugins in sandbox processes or with restricted permissions. Consider process isolation.
* **Versioning / compatibility**: include a `HostVersion` requirement in the attribute or an interface method for `bool IsCompatibleWith(string hostVersion)` to avoid breaking changes.
* **Dependency Injection**: instead of `Activator.CreateInstance`, use a DI container to construct plugin instances and resolve services. Provide a minimal `IServiceProvider` to plugins for controlled access.
* **Metadata registry**: build a `PluginCatalog` that stores metadata (id, version, author, description) — extend `PluginAttribute` to contain those fields.
* **Logging & Exception handling**: isolate plugin failures: a plugin should not crash the host. Catch exceptions and consider run-time timeouts.
* **Testing**: keep plugin APIs small and well-documented; unit test both plugins and the loader.
* **Hot-reload**: use `AssemblyLoadContext` + reload pattern to update plugins at runtime.

# 7) Simple host program (complete quick example)

```csharp
class Program
{
    static void Main()
    {
        var loader = new ReflectionPluginLoader();

        // option 1: discover in current AppDomain
        var descriptors = loader.DiscoverFromAssemblies(AppDomain.CurrentDomain.GetAssemblies()).ToList();
        var plugins = loader.InstantiatePlugins(descriptors).ToList();

        // option 2: from plugins folder (uncomment if you have plugin DLLs there)
        /*
        var assemblies = Directory.EnumerateFiles("plugins", "*.dll").Select(path => Assembly.LoadFrom(path));
        descriptors = loader.DiscoverFromAssemblies(assemblies).ToList();
        plugins = loader.InstantiatePlugins(descriptors).ToList();
        */

        foreach (var p in plugins)
        {
            Console.WriteLine($"Running {p.Name}");
            try { p.Execute(); }
            catch (Exception ex) { Console.WriteLine($"Plugin {p.Name} threw: {ex.Message}"); }
        }
    }
}
```

# 8) Quick checklist to get this running

1. Create three projects (Recommended):

   * `Host` (console app)
   * `Plugin.Abstractions` (class library: `IPlugin`, `PluginAttribute`)
   * `Plugins` (class library referencing `Plugin.Abstractions`) — put your plugin classes here.
2. Host references `Plugin.Abstractions` but NOT the plugin assemblies (so host doesn't statically link plugins).
3. Build plugin DLL(s) and put them in `Host/plugins` or load them via `AssemblyLoadContext`.
4. Run the host — it will discover & execute plugins.
