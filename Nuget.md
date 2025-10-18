# How to Create NuGet Packages

NuGet is the package manager for .NET, allowing you to share reusable code libraries with other developers. This tutorial will walk you through creating your first NuGet package.

## Prerequisites

- .NET SDK installed (6.0 or later recommended)
- A code editor (Visual Studio, VS Code, or Rider)
- Basic understanding of C# and .NET

## Step 1: Create Your Library Project

First, create a new class library project:

```bash
dotnet new classlib -n MyAwesomeLibrary
cd MyAwesomeLibrary
```

This creates a basic class library with a `Class1.cs` file. Replace it with your actual code. For example:

```csharp
namespace MyAwesomeLibrary
{
    public class StringHelper
    {
        public static string Reverse(string input)
        {
            if (string.IsNullOrEmpty(input))
                return input;
            
            char[] charArray = input.ToCharArray();
            Array.Reverse(charArray);
            return new string(charArray);
        }
    }
}
```

## Step 2: Configure Package Metadata

Edit your `.csproj` file to include package metadata. This information will appear on NuGet.org and help users understand your package:

```xml
<Project Sdk="Microsoft.NET.Sdk">
  <PropertyGroup>
    <TargetFramework>net8.0</TargetFramework>
    <ImplicitUsings>enable</ImplicitUsings>
    <Nullable>enable</Nullable>
    
    <!-- Package Information -->
    <PackageId>MyAwesomeLibrary</PackageId>
    <Version>1.0.0</Version>
    <Authors>Your Name</Authors>
    <Company>Your Company</Company>
    <Description>A helpful library for string manipulation</Description>
    <PackageTags>strings;utilities;helpers</PackageTags>
    <PackageProjectUrl>https://github.com/yourusername/myawesomelibrary</PackageProjectUrl>
    <RepositoryUrl>https://github.com/yourusername/myawesomelibrary</RepositoryUrl>
    <PackageLicenseExpression>MIT</PackageLicenseExpression>
    <PackageReadmeFile>README.md</PackageReadmeFile>
  </PropertyGroup>

  <ItemGroup>
    <None Include="README.md" Pack="true" PackagePath="\" />
  </ItemGroup>
</Project>
```

### Key Metadata Properties

- **PackageId**: Unique identifier for your package
- **Version**: Semantic version (Major.Minor.Patch)
- **Authors**: Package creator(s)
- **Description**: What your package does
- **PackageTags**: Searchable keywords
- **PackageLicenseExpression**: License type (MIT, Apache-2.0, etc.)

## Step 3: Create a README

Create a `README.md` file in your project root to document your package:

```markdown
# MyAwesomeLibrary

A helpful library for string manipulation.

## Installation

dotnet add package MyAwesomeLibrary

## Usage

string reversed = StringHelper.Reverse("Hello");
// Output: "olleH"
```

## Step 4: Build and Pack Your Package

Build your project and create a NuGet package:

```bash
dotnet build
dotnet pack --configuration Release
```

This creates a `.nupkg` file in `bin/Release/`. The file will be named something like `MyAwesomeLibrary.1.0.0.nupkg`.

## Step 5: Test Your Package Locally

Before publishing, test your package locally. Create a test project:

```bash
cd ..
dotnet new console -n TestApp
cd TestApp
dotnet add package MyAwesomeLibrary --source ../MyAwesomeLibrary/bin/Release
```

Use your library in the test app to verify it works correctly.

## Step 6: Publish to NuGet.org

To publish your package publicly:

1. Create an account at [NuGet.org](https://www.nuget.org)
2. Generate an API key from your account settings
3. Push your package:

```bash
dotnet nuget push MyAwesomeLibrary.1.0.0.nupkg --api-key YOUR_API_KEY --source https://api.nuget.org/v3/index.json
```

Your package will be available on NuGet.org after validation (usually takes a few minutes).

## Advanced Topics

### Multi-Targeting

To support multiple .NET versions, modify your `.csproj`:

```xml
<TargetFrameworks>net6.0;net8.0;netstandard2.0</TargetFrameworks>
```

### Including Dependencies

If your library uses other NuGet packages, they'll automatically be included as dependencies when you pack. Just reference them normally:

```bash
dotnet add package Newtonsoft.Json
```

### Adding Package Icons

Include a package icon by adding to your `.csproj`:

```xml
<PropertyGroup>
  <PackageIcon>icon.png</PackageIcon>
</PropertyGroup>

<ItemGroup>
  <None Include="icon.png" Pack="true" PackagePath="\" />
</ItemGroup>
```

### Versioning Best Practices

Follow semantic versioning:
- **Major**: Breaking changes (2.0.0)
- **Minor**: New features, backward compatible (1.1.0)
- **Patch**: Bug fixes (1.0.1)

### Creating Symbol Packages

For better debugging experience, include symbols:

```bash
dotnet pack --include-symbols --include-source
```

## Common Issues

**Issue**: Package ID already exists  
**Solution**: Choose a unique PackageId. Check NuGet.org first.

**Issue**: Missing metadata warnings  
**Solution**: Ensure all required properties are set in your `.csproj`.

**Issue**: Package won't install in target project  
**Solution**: Verify target framework compatibility and dependencies.

## Resources

- [Official NuGet Documentation](https://docs.microsoft.com/nuget)
- [Semantic Versioning](https://semver.org)
- [Package Icon Guidelines](https://docs.microsoft.com/nuget/reference/nuspec#icon)

Creating NuGet packages is a great way to share your code with the .NET community. Start small, follow best practices, and iterate based on user feedback!
