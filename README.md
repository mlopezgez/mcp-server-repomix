# Repomix MCP Server for Zed

A [Zed](https://zed.dev) extension that integrates [Repomix](https://repomix.com) as a Model Context Protocol (MCP) server, enabling AI assistants to pack and analyze entire codebases.

## Features

- **Repository Packing**: Pack local or remote repositories into a single AI-friendly file
- **MCP Integration**: Seamlessly integrates with Zed's Assistant through the Model Context Protocol
- **Flexible Configuration**: Support for custom Repomix paths and settings
- **AI-Optimized Output**: Generates comprehensive codebase context for AI analysis

## What is Repomix?

[Repomix](https://repomix.com) is a powerful tool that packs your entire repository into a single file optimized for AI consumption. It's particularly useful for:

- Providing comprehensive codebase context to AI assistants
- Analyzing repository structure and dependencies
- Creating shareable codebase snapshots for AI analysis
- Supporting multiple output formats (XML, Markdown, Plain text)

## Prerequisites

Before using this extension, you need to have:

1. **Zed Editor**: Version with MCP support
2. **Node.js**: Required for running Repomix
3. **Repomix**: Install globally with:
   ```bash
   npm install -g repomix
   ```

## Installation

### From Zed Extension Marketplace

1. Open Zed
2. Press `Cmd+Shift+P` (macOS) or `Ctrl+Shift+P` (Linux/Windows)
3. Type "zed: extensions" and select it
4. Search for "Repomix MCP Server"
5. Click "Install"

### From Source

1. Clone this repository:
   ```bash
   git clone https://github.com/mlopezgez/zed-mcp-server-repomix.git
   cd zed-mcp-server-repomix
   ```

2. Build the extension:
   ```bash
   cargo build --release
   ```

3. Install in Zed:
   ```bash
   # Copy to Zed extensions directory
   mkdir -p ~/.config/zed/extensions
   cp -r . ~/.config/zed/extensions/mcp-server-repomix
   ```

## Configuration

### Basic Setup

Add the following to your Zed settings (`~/.config/zed/settings.json`):

```json
{
  "context_servers": {
    "mcp-server-repomix": {}
  }
}
```

### Custom Repomix Path (Optional)

If you have Repomix installed in a custom location:

```json
{
  "context_servers": {
    "mcp-server-repomix": {
      "settings": {
        "repomix_path": "/custom/path/to/repomix"
      }
    }
  }
}
```

## Accessing Private Repositories

Repomix uses your **locally installed Git** for authentication. To access private repositories, configure Git authentication using standard methods:

### Quick Setup

**Option 1: SSH Keys (Recommended)**
```bash
# Generate SSH key
ssh-keygen -t ed25519 -C "your_email@example.com"

# Add to GitHub: https://github.com/settings/keys
# Add to GitLab: https://gitlab.com/-/profile/keys

# Test connection
ssh -T git@github.com
```

**Option 2: Git Credential Manager**
```bash
# macOS
git config --global credential.helper osxkeychain

# Windows
git config --global credential.helper manager

# Linux
git config --global credential.helper store
```

**Option 3: Personal Access Tokens (HTTPS)**
- GitHub: Generate at https://github.com/settings/tokens (scope: `repo`)
- GitLab: Generate at https://gitlab.com/-/profile/personal_access_tokens (scopes: `read_api`, `read_repository`)

### Verification

Test your Git authentication:
```bash
git clone <private-repo-url>
```

If Git authentication works, Repomix will automatically use the same credentials.

For detailed setup instructions, see the [installation guide](configuration/installation_instructions.md).

## Usage

Once installed and configured, the Repomix MCP server will be available in your Zed Assistant. The assistant can use Repomix tools to:

1. **Pack repositories**: Convert entire codebases into AI-friendly format
2. **Analyze code structure**: Understand repository organization and dependencies
3. **Search packed output**: Find specific patterns and code elements
4. **Read packed content**: Extract specific files or sections from packed repositories

### Example Prompts

Try these prompts with your Zed Assistant:

- "Pack the current repository and show me an overview"
- "Analyze the structure of repository https://github.com/yamadashy/repomix"
- "Pack only the TypeScript files in the src directory"
- "Find all authentication-related code in this repository"

## Available MCP Tools

The extension provides the following MCP tools to the Zed Assistant:

- `pack_repository`: Pack a local or remote repository
- `read_packed_output`: Read and analyze packed repository output
- `search_packed_output`: Search for specific patterns in packed output

## Development

### Building from Source

```bash
# Install Rust if you haven't already
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh

# Clone and build
git clone https://github.com/mlopezgez/zed-mcp-server-repomix.git
cd zed-mcp-server-repomix
cargo build --release
```

### Project Structure

```
zed-mcp-server-repomix/
├── src/
│   └── mcp_server_repomix.rs    # Main extension implementation
├── configuration/
│   ├── installation_instructions.md
│   └── default_settings.jsonc
├── Cargo.toml                    # Rust dependencies
├── extension.toml                # Zed extension metadata
└── README.md
```

## Troubleshooting

### Server Not Starting

If the MCP server fails to start:

1. Verify Repomix is installed: `repomix --version`
2. Check Node.js is installed: `node --version`
3. Ensure the extension is enabled in Zed settings
4. Restart Zed after configuration changes

### Permission Issues

If you encounter permission errors:

1. Ensure you have write permissions in the project directory
2. Verify Repomix can execute: `which repomix`
3. Check Node.js permissions

### Debug Mode

Enable debug logging in Zed to see detailed MCP server output:

```json
{
  "assistant": {
    "debug": true
  }
}
```

## Related Projects

- [Repomix](https://github.com/yamadashy/repomix) - The core repository packing tool
- [Zed](https://github.com/zed-industries/zed) - The editor this extension runs in
- [Model Context Protocol](https://modelcontextprotocol.io) - The protocol specification

## Contributing

Contributions are welcome! Please feel free to submit issues or pull requests.

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add some amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Acknowledgments

- [Repomix](https://repomix.com) by [@yamadashy](https://github.com/yamadashy) for the excellent repository packing tool
- [Zed](https://zed.dev) team for the extensible editor and MCP support
- [Context7 MCP Server](https://github.com/akbxr/zed-mcp-server-context7) for reference implementation

## Links

- [Documentation](https://github.com/mlopezgez/zed-mcp-server-repomix)
- [Repomix Website](https://repomix.com)
- [Zed Extensions Guide](https://zed.dev/docs/extensions)
- [Report Issues](https://github.com/mlopezgez/zed-mcp-server-repomix/issues)
