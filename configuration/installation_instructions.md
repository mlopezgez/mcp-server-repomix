# Repomix MCP Server Installation

This extension integrates Repomix as a Model Context Protocol (MCP) server for Zed's Assistant, allowing you to pack and analyze codebases for AI consumption.

## What is Repomix?

Repomix is a tool that packs your entire repository into a single AI-friendly file. It's particularly useful for:
- Providing comprehensive codebase context to AI assistants
- Analyzing repository structure and dependencies
- Creating shareable codebase snapshots for AI analysis

## Installation

1. **Install the extension** from the Zed extension marketplace
2. **Install Repomix globally** (required):
   ```bash
   npm install -g repomix
   ```

## Configuration

### Basic Setup

After installing the extension, enable it in your Zed settings:

1. Open Zed settings (Cmd+, on macOS or Ctrl+, on Linux/Windows)
2. Add the following to your settings:

```json
{
  "context_servers": {
    "mcp-server-repomix": {}
  }
}
```

### Custom Repomix Path (Optional)

If you have Repomix installed in a custom location, you can specify the path:

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

Repomix uses your **locally installed Git** for authentication when accessing remote repositories. This means it automatically uses whatever authentication method you've already configured with Git.

### Authentication Methods

To access private repositories, configure Git authentication using one of these standard methods:

#### Option 1: SSH Keys (Recommended)

1. Generate an SSH key if you don't have one:
   ```bash
   ssh-keygen -t ed25519 -C "your_email@example.com"
   ```

2. Add your SSH key to GitHub/GitLab:
   - **GitHub**: https://github.com/settings/keys
   - **GitLab**: https://gitlab.com/-/profile/keys

3. Test your connection:
   ```bash
   ssh -T git@github.com
   # or
   ssh -T git@gitlab.com
   ```

4. Use SSH URLs with Repomix:
   ```bash
   repomix --remote git@github.com:user/private-repo.git
   ```

#### Option 2: Git Credential Manager

Git Credential Manager securely stores your credentials:

**macOS:**
```bash
git config --global credential.helper osxkeychain
```

**Windows:**
```bash
git config --global credential.helper manager
```

**Linux:**
```bash
git config --global credential.helper store
```

When you clone or access a private repository via HTTPS for the first time, Git will prompt for credentials and store them.

#### Option 3: Personal Access Tokens (HTTPS)

For HTTPS access with tokens:

**GitHub:**
1. Generate a token at: https://github.com/settings/tokens
2. Required scopes: `repo` (for private repos)
3. Clone with token in URL:
   ```bash
   git clone https://TOKEN@github.com/user/private-repo.git
   ```

**GitLab:**
1. Generate a token at: https://gitlab.com/-/profile/personal_access_tokens
2. Required scopes: `read_api`, `read_repository`
3. Clone with token in URL:
   ```bash
   git clone https://oauth2:TOKEN@gitlab.com/user/private-repo.git
   ```

### Verifying Authentication

To verify your Git authentication is working:

```bash
# Test cloning a private repository
git clone <private-repo-url>

# If successful, Repomix will also be able to access it
repomix --remote <private-repo-url>
```

### Security Best Practices

⚠️ **Important Notes:**

- **SSH keys are recommended** for better security
- Never commit credentials to version control
- Use tokens with minimal required scopes
- Rotate tokens regularly
- Store credentials securely using Git credential helpers
- Repomix inherits all Git credentials automatically

## Usage

### Agent Mode

To use Repomix in agent mode:

1. Enable the MCP server in your assistant settings
2. The assistant will have access to Repomix tools for packing repositories

### Available Tools

The MCP server provides the following tools:

- **pack_repository**: Pack a local or remote repository into a single file
- **read_packed_output**: Read and analyze the packed repository output
- **search_packed_output**: Search for specific patterns in the packed output

## Verification

To verify the extension is working:

1. Open the Zed Assistant panel
2. Check that "Repomix MCP Server" appears in the list of available context servers
3. Try using the Repomix tools in your conversation

## Troubleshooting

### Server Not Starting

If the server fails to start:

1. Ensure Repomix is installed globally: `npm list -g repomix`
2. Check that Node.js is installed: `node --version`
3. Verify the extension is enabled in your settings
4. Restart Zed after making configuration changes

### Permission Issues

If you encounter permission errors:

1. Ensure you have write permissions in the project directory
2. Check that Repomix can execute: `which repomix`
3. Verify Node.js permissions

### Private Repository Access Issues

If you can't access private repositories:

1. Verify Git authentication works: `git clone <private-repo-url>`
2. Check your SSH keys are added: `ssh -T git@github.com`
3. Verify credential helper is configured: `git config --global credential.helper`
4. For HTTPS, ensure credentials are stored correctly
5. Check repository permissions (you need at least read access)

## Learn More

- [Repomix Documentation](https://repomix.com)
- [Repomix GitHub Repository](https://github.com/yamadashy/repomix)
- [Repomix Remote Repository Processing](https://repomix.com/guide/remote-repository-processing)
- [MCP Documentation](https://modelcontextprotocol.io)
- [Zed Extensions Guide](https://zed.dev/docs/extensions)

## Support

For issues and questions:
- Extension issues: [Report on GitHub](https://github.com/mlopezgez/zed-mcp-server-repomix/issues)
- Repomix issues: [Report on Repomix GitHub](https://github.com/yamadashy/repomix/issues)
