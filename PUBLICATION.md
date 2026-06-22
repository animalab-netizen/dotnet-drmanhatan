# Publication Guide

## Current State

`dotnet-drmanhatan` is structured as a standalone NuGet package.

Current coordinates:

- package: `dotnet-drmanhatan`
- version: `0.1.0`
- repository: `github.com/animalab-netizen/dotnet-drmanhatan`

## Distribution Model

The package is intended for:

- direct NuGet distribution as the public .NET DrManhatan runtime
- consumption by validation projects and backend examples
- installation without any private feed requirement

## Installation

```bash
dotnet add package dotnet-drmanhatan
```

## Release Checklist

1. Run `dotnet build`
2. Run `dotnet run --project tests/DotNetDrManhatan.Validation/DotNetDrManhatan.Validation.csproj`
3. Update `CHANGELOG.md`
4. Confirm version in `src/DotNetDrManhatan/DotNetDrManhatan.csproj`
5. Commit release metadata
6. Create and push tag `v0.1.0`
