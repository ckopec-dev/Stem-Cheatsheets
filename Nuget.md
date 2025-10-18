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

# How to Release Updates to NuGet Packages

Releasing updates to your NuGet package is straightforward, but requires careful version management and testing. Here's how to do it properly.

## Step 1: Update Your Version Number

Edit your `.csproj` file and increment the version number according to semantic versioning:

```xml
<PropertyGroup>
  <Version>1.0.1</Version>  <!-- Changed from 1.0.0 -->
</PropertyGroup>
```

### Semantic Versioning Guidelines

- **Patch (1.0.0 → 1.0.1)**: Bug fixes, no breaking changes
- **Minor (1.0.0 → 1.1.0)**: New features, backward compatible
- **Major (1.0.0 → 2.0.0)**: Breaking changes

## Step 2: Document Your Changes

Create or update a `CHANGELOG.md` file to track what changed:

```markdown
# Changelog

## [1.0.1] - 2025-10-18
### Fixed
- Fixed null reference exception in StringHelper.Reverse
- Corrected documentation typos

### Changed
- Improved performance by 20%

## [1.0.0] - 2025-10-15
### Added
- Initial release with StringHelper class
```

You can also add release notes directly in your `.csproj`:

```xml
<PropertyGroup>
  <Version>1.0.1</Version>
  <PackageReleaseNotes>
    Fixed null reference exception and improved performance.
    See CHANGELOG.md for full details.
  </PackageReleaseNotes>
</PropertyGroup>
```

## Step 3: Make Your Code Changes

Update your code with bug fixes, new features, or improvements. Ensure backward compatibility for minor/patch releases.

## Step 4: Test Thoroughly

Before releasing, test your changes:

```bash
# Run unit tests
dotnet test

# Build in release mode
dotnet build --configuration Release
```

Test the updated package locally in a test project:

```bash
# Pack the new version
dotnet pack --configuration Release

# In your test project
dotnet add package MyAwesomeLibrary --version 1.0.1 --source ../MyAwesomeLibrary/bin/Release
```

## Step 5: Pack and Publish

Once testing is complete, publish the new version:

```bash
# Create the package
dotnet pack --configuration Release

# Push to NuGet.org
dotnet nuget push bin/Release/MyAwesomeLibrary.1.0.1.nupkg --api-key YOUR_API_KEY --source https://api.nuget.org/v3/index.json
```

## Step 6: Verify the Update

After publishing:

1. Wait a few minutes for NuGet.org to index your package
2. Visit your package page: `https://www.nuget.org/packages/MyAwesomeLibrary`
3. Verify the new version appears
4. Test installing it in a fresh project:

```bash
dotnet add package MyAwesomeLibrary --version 1.0.1
```

## Best Practices for Updates

### Always Increment the Version

NuGet versions are immutable. Once published, you cannot replace version 1.0.0 with different code. You must publish a new version number.

### Use Pre-release Versions for Testing

Test major changes with pre-release versions first:

```xml
<Version>2.0.0-beta.1</Version>
```

Users can opt into pre-releases:

```bash
dotnet add package MyAwesomeLibrary --version 2.0.0-beta.1
```

Pre-release suffixes:
- `-alpha`: Very early, unstable
- `-beta`: Feature complete, needs testing
- `-rc` (release candidate): Nearly ready for production

### Consider Deprecation for Breaking Changes

If you're making breaking changes, consider:

1. Mark old methods as obsolete first (in a minor version):

```csharp
[Obsolete("Use NewMethod instead. This will be removed in v2.0.0")]
public static string OldMethod(string input)
{
    return NewMethod(input);
}

public static string NewMethod(string input)
{
    // New implementation
}
```

2. Remove deprecated code in the next major version

### Maintain Multiple Major Versions

You can maintain bug fixes for older major versions:

```bash
# Branch for v1.x maintenance
git checkout -b v1.x
# Update version to 1.0.2
dotnet pack --configuration Release
dotnet nuget push bin/Release/MyAwesomeLibrary.1.0.2.nupkg --api-key YOUR_API_KEY --source https://api.nuget.org/v3/index.json
```

## Unlisting vs Deleting Packages

### Unlisting a Package

If you published a bad version but users already downloaded it:

1. Go to NuGet.org
2. Navigate to your package version
3. Click "Unlist"

Unlisted packages:
- Don't appear in search results
- Don't show as the latest version
- Can still be installed by exact version number

### Deleting a Package (Avoid This)

You can only delete within 72 hours of publishing. Deletion should be rare and only for:
- Accidentally published secrets
- Legal issues
- Critical security vulnerabilities

## Automation with GitHub Actions

Automate releases with CI/CD. Create `.github/workflows/publish.yml`:

```yaml
name: Publish NuGet Package

on:
  push:
    tags:
      - 'v*'

jobs:
  publish:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      
      - name: Setup .NET
        uses: actions/setup-dotnet@v3
        with:
          dotnet-version: '8.0.x'
      
      - name: Restore dependencies
        run: dotnet restore
      
      - name: Build
        run: dotnet build --configuration Release --no-restore
      
      - name: Test
        run: dotnet test --no-restore --verbosity normal
      
      - name: Pack
        run: dotnet pack --configuration Release --no-build
      
      - name: Publish to NuGet
        run: dotnet nuget push **/*.nupkg --api-key ${{ secrets.NUGET_API_KEY }} --source https://api.nuget.org/v3/index.json
```

Store your API key in GitHub Secrets as `NUGET_API_KEY`.

## Quick Release Checklist

- [ ] Update version number in `.csproj`
- [ ] Update `CHANGELOG.md` or release notes
- [ ] Make and test code changes
- [ ] Run all unit tests
- [ ] Test package locally
- [ ] Build and pack: `dotnet pack --configuration Release`
- [ ] Push to NuGet: `dotnet nuget push`
- [ ] Verify on NuGet.org
- [ ] Tag release in Git: `git tag v1.0.1 && git push --tags`
- [ ] Create GitHub release with notes (optional)

## Troubleshooting

**Error: Package already exists**  
You're trying to push a version that's already published. Increment the version number.

**Error: API key is invalid**  
Your API key expired or is incorrect. Generate a new one from NuGet.org account settings.

**Package not showing up**  
Give it 5-10 minutes for indexing. Check the validation status on your NuGet.org package page.

**Users not seeing the update**  
They may need to clear their local package cache: `dotnet nuget locals all --clear`

Regular updates keep your package relevant and show active maintenance. Version responsibly and communicate changes clearly to your users!
