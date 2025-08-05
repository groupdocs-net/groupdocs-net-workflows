# 🚀 GroupDocs .NET Workflows

This repository contains GitHub Actions workflows for managing GroupDocs .NET library examples and API references. It provides automated tools to update code examples across multiple language repositories and maintain API documentation when new library versions are released.

## 📦 Features

### 🔄 Code Examples Management
- **Automated version updates** across C#, F#, and VB.NET example repositories
- **NuGet package version tracking** with automatic latest version detection
- **Multi-language support** for GroupDocs library examples
- **Git tagging** for version releases in example repositories
- **Selective updates** - only processes repositories with actual changes

### 📚 API References Management
- **Automated API reference generation** using Docker-based tooling
- **Version-specific documentation** for different GroupDocs libraries
- **Optional deployment** to QA or production environments
- **Change detection** - only commits when there are actual updates

## 🛠️ Supported Libraries

Currently configured to work with:
- `GroupDocs.Conversion.LowCode`
- `GroupDocs.Merger.LowCode` *(commented out)*
- `GroupDocs.Metadata.LowCode` *(commented out)*

Library support can be extended by updating the workflow choice options and `nuget-packages.json`.

## 🔧 Repository Structure

```
groupdocs.net-workflows/
├── .github/workflows/
│   ├── update_code_examples.yml    # Updates library examples
│   └── update_api_refs.yml         # Updates API references
├── nuget-packages.json             # Tracks current library versions
├── README.md
└── LICENSE
```

## 🚀 Workflows

### 1. Update Code Examples

**Workflow:** `update_code_examples.yml`  
**Trigger:** Manual dispatch with library selection

**What it does:**
1. **Version Detection** - Compares current version (from `nuget-packages.json`) with latest NuGet release
2. **Multi-Repository Updates** - Updates PackageReference versions in:
   - `groupdocs-net/{library}-csharp` repository
   - `groupdocs-net/{library}-fsharp` repository  
   - `groupdocs-net/{library}-vbnet` repository
3. **Project File Updates** - Modifies `.csproj`, `.fsproj`, and `.vbproj` files automatically
4. **Version Tagging** - Creates and pushes git tags (e.g., `25.7.0`) to updated repositories
5. **Configuration Update** - Updates `nuget-packages.json` with the new version
6. **Smart Processing** - Only updates repositories that have actual changes

**Usage:**
1. Go to **Actions** → **Update Code Examples**
2. Click **Run workflow**
3. Select the library to update
4. The workflow will automatically detect and apply updates if available

### 2. Update API References

**Workflow:** `update_api_refs.yml`  
**Trigger:** Manual dispatch with library and deployment options

**What it does:**
1. **Reference Generation** - Uses Docker-based tooling to generate API documentation
2. **Content Updates** - Updates documentation in the `groupdocs-net/groupdocs-net` repository
3. **Change Detection** - Only commits when there are actual documentation changes
4. **Optional Deployment** - Can trigger site deployment to QA or production

**Usage:**
1. Go to **Actions** → **Update API References**
2. Click **Run workflow**  
3. Select the library and deployment target
4. The workflow will generate and optionally deploy updated references

## 🔐 Required Secrets

The following repository secrets must be configured:

| Secret Name | Description | Usage |
|-------------|-------------|-------|
| `READ_PAT` | Personal Access Token with `repo` scope | Access to private GroupDocs repositories |

## 📋 Configuration

### `nuget-packages.json`

Tracks current versions of supported libraries:

```json
{
    "GroupDocs.Conversion.LowCode": "25.7.0",
    "GroupDocs.Merger.LowCode": "25.9.0", 
    "GroupDocs.Metadata.LowCode": "25.9.0"
}
```

This file is automatically updated when new versions are deployed to example repositories.

## 🎯 Target Repositories

The workflows operate on these repository patterns:

### Code Examples
- `groupdocs-net/groupdocs-{product}-lowcode-examples-csharp`
- `groupdocs-net/groupdocs-{product}-lowcode-examples-fsharp`
- `groupdocs-net/groupdocs-{product}-lowcode-examples-vbnet`

### API References
- `groupdocs-net/groupdocs-net` (main documentation repository)

## 🔄 Workflow Logic

### Version Update Process
1. Query NuGet API for latest package version
2. Compare with version stored in `nuget-packages.json`  
3. If update available:
   - Clone each language-specific example repository
   - Update PackageReference versions in project files
   - Commit changes and push to main branch
   - Create and push version tag (e.g., `25.7.0`)
   - Update `nuget-packages.json` with new version

### Smart Change Detection
- Only processes repositories when actual updates are needed
- Skips repositories with no PackageReference changes
- Provides detailed logging for troubleshooting

## 🚦 Workflow Status

- ✅ **Update Code Examples** - Fully operational with tagging support
- ✅ **Update API References** - Operational with optional deployment
- 🔄 **Multi-library Support** - Ready to enable additional libraries

## 🤝 Contributing

To add support for new GroupDocs libraries:

1. Add the library to workflow choice options in both `.yml` files
2. Add the library and current version to `nuget-packages.json`
3. Ensure corresponding example repositories exist with the expected naming pattern
4. Test the workflow with the new library

## 📝 Notes

- All workflows use Ubuntu latest runners
- Git operations are performed with automated user credentials
- Docker is used for API reference generation
- Workflows provide detailed logging for debugging
- Tags follow semantic versioning (e.g., `25.7.0`)