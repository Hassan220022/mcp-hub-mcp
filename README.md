<p align="center">
  <img src="https://imgur.com/DgWxkmv.png" width="200" height="200">
</p>

[![Install MCP Server](https://cursor.com/deeplink/mcp-install-dark.svg)](https://cursor.com/install-mcp?name=mcp-hub&config=eyJjb21tYW5kIjoibnB4IC15IG1jcC1odWItbWNwIC0tY29uZmlnLXBhdGggfi8uY3Vyc29yL21jcC1odWIuanNvbiJ9)

[![Install in VS Code](https://img.shields.io/badge/VS_Code-Install_Server-0098FF?style=flat-square&logo=visualstudiocode&logoColor=white)](https://insiders.vscode.dev/redirect?url=vscode:mcp/install?%7B%22name%22%3A%22mcp-hub%22%2C%22command%22%3A%22npx%22%2C%22args%22%3A%5B%22-y%22%2C%22mcp-hub-mcp%40latest%22%2C%22--config-path%22%2C%22~%2Fmcp-hub.json%22%5D%7D)

# MCP-Hub-MCP Server

A hub server that connects to and manages other MCP (Model Context Protocol) servers.

## Overview

This project builds an MCP hub server that connects to and manages multiple MCP (Model Context Protocol) servers through a single interface.
It helps prevent excessive context usage and pollution from infrequently used MCPs (e.g., Atlassian MCP, Playwright MCP) by allowing you to connect them only when needed.
This reduces AI mistakes and improves performance by keeping the active tool set focused and manageable.

## Key Features

- Automatic connection to other MCP servers via configuration file
- List available tools on connected servers
- Call tools on connected servers and return results

## Configuration

Add this to your `mcp.json`:

#### Using npx

```json
{
  "mcpServers": {
    "other-tools": {
      "command": "npx",
      "args": [
        "-y",
        "mcp-hub-mcp",
        "--config-path",
        "/Users/username/mcp.json"
      ]
    }
  }
}
```


## Installation and Running

### Requirements

- Node.js 18.0.0 or higher
- npm, yarn, or pnpm

### Installation

```bash
# Clone repository
git clone <repository-url>
cd mcp-hub-mcp

# Install dependencies
npm install
# or
yarn install
# or
pnpm install
```

### Build

```bash
npm run build
# or
yarn build
# or
pnpm build
```

### Run

```bash
npm start
# or
yarn start
# or
pnpm start
```

### Development Mode

```bash
npm run dev
# or
yarn dev
# or
pnpm dev
```

## Configuration File

The MCP-Hub-MCP server uses a Claude Desktop format configuration file to automatically connect to other MCP servers.
You can specify the configuration file in the following ways:

1. Environment variable: Set the `MCP_CONFIG_PATH` environment variable to the configuration file path
2. Command line argument: Use the `--config-path` option to specify the configuration file path
3. Default path: Use `mcp-config.json` file in the current directory

Configuration file format:

```json
{
  "mcpServers": {
    "serverName1": {
      "command": "command",
      "args": ["arg1", "arg2", ...],
      "env": { "ENV_VAR1": "value1", ... }
    },
    "serverName2": {
      "command": "anotherCommand",
      "args": ["arg1", "arg2", ...]
    }
  }
}
```

Example:

```json
{
  "mcpServers": {
    "filesystem": {
      "command": "npx",
      "args": [
        "-y",
        "@modelcontextprotocol/server-filesystem",
        "/Users/username/Desktop",
        "/Users/username/Downloads"
      ]
    },
    "other-server": {
      "command": "node",
      "args": ["path/to/other-mcp-server.js"]
    }
  }
}
```

## 🤖 AI Model Usage Rules for Windsurf & Cursor

### **CRITICAL: Read This Before Using MCP-Hub-MCP**

**THIS IS YOUR CENTRAL HUB FOR ALL TOOLS.** This MCP server acts as a unified gateway to access ALL available tools from multiple MCP servers. Think of it as a "server of servers" that prevents context pollution and improves performance by managing tool discovery intelligently.

### **📋 Essential Usage Workflow**

#### **Step 1: ALWAYS Start with Tool Discovery**
**NEVER** call tools blindly. **ALWAYS** discover tools first using these methods:

**🎯 Method A: Smart Search (RECOMMENDED)**
```
Use: find-tools
With pattern: "keyword related to what you want to do"
Example: find-tools with pattern="file" to find file-related tools
```

**📋 Method B: Server-Specific Discovery**
```
Use: list-tools-in-server
With serverName: "known_server_name"
Example: list-tools-in-server with serverName="filesystem"
```

**⚠️ Method C: Complete Inventory (USE SPARINGLY)**
```
Use: list-all-tools
WARNING: Only use when find-tools doesn't find what you need
This returns ALL tools from ALL servers - can be overwhelming
```

#### **Step 2: Get Tool Details**
Before calling any tool, get its complete schema:
```
Use: get-tool
With: serverName="server_name" and toolName="exact_tool_name"
This shows you the exact parameters and their types
```

#### **Step 3: Execute Tools**
```
Use: call-tool
With: 
- serverName: "exact_server_name" (from discovery)
- toolName: "exact_tool_name" (from discovery) 
- toolArgs: {"param1": "value1", "param2": "value2"}
```

### **🚨 CRITICAL RULES - NEVER VIOLATE THESE**

1. **🔍 DISCOVERY FIRST**: Never call a tool without discovering it first via `find-tools` or similar
2. **📝 EXACT NAMES**: Use EXACT serverName and toolName from discovery results - no guessing
3. **🎯 BE SPECIFIC**: Use `find-tools` with specific patterns rather than `list-all-tools`
4. **✅ VALIDATE SCHEMA**: Always use `get-tool` to understand parameters before calling
5. **🧠 CONTEXT AWARENESS**: This hub prevents context pollution - use it efficiently

### **💡 Smart Usage Patterns**

#### **Pattern 1: File Operations**
```
1. find-tools with pattern="file|read|write"
2. get-tool for the specific file tool you need
3. call-tool with proper file paths and parameters
```

#### **Pattern 2: Web/HTTP Operations**
```
1. find-tools with pattern="web|http|api|request"
2. get-tool to understand the HTTP tool parameters
3. call-tool with proper URL and headers
```

#### **Pattern 3: Database Operations**
```
1. find-tools with pattern="database|sql|query"
2. get-tool to understand connection parameters
3. call-tool with proper connection strings and queries
```

### **⚡ Performance Optimization Rules**

1. **🎯 Targeted Search**: Use specific keywords in `find-tools` rather than generic terms
2. **🔄 Reuse Discovery**: Remember tool locations within the same conversation
3. **📊 Batch Operations**: When possible, use tools that can handle multiple operations
4. **🧹 Clean Responses**: The hub filters responses to show only name + description for discovery

### **🛡️ Error Handling Guidelines**

#### **When Tool Discovery Fails:**
```
1. Try different search patterns with find-tools
2. Use list-all-tools as last resort
3. Check if the required MCP server is configured
```

#### **When Tool Execution Fails:**
```
1. Verify serverName and toolName are exact matches
2. Check tool schema with get-tool
3. Validate all required parameters are provided
4. Ensure proper data types for parameters
```

### **🔧 Configuration for Windsurf**

Add to your `~/.codeium/mcp_config.json`:
```json
{
  "mcpServers": {
    "mcp-hub": {
      "command": "npx",
      "args": [
        "-y",
        "mcp-hub-mcp@latest",
        "--config-path",
        "~/.codeium/mcp-servers.json"
      ]
    }
  }
}
```

Create `~/.codeium/mcp-servers.json` with your actual MCP servers:
```json
{
  "mcpServers": {
    "filesystem": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-filesystem", "/path/to/files"]
    },
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_PERSONAL_ACCESS_TOKEN": "your_token"
      }
    }
  }
}
```

### **🔧 Configuration for Cursor**

Add to your Cursor `mcp.json`:
```json
{
  "mcpServers": {
    "mcp-hub": {
      "command": "npx",
      "args": [
        "-y",
        "mcp-hub-mcp@latest",
        "--config-path",
        "~/.cursor/mcp-servers.json"
      ]
    }
  }
}
```

Create `~/.cursor/mcp-servers.json` with your actual MCP servers (same format as above).

### **🎯 Example: Complete Workflow**

```
User: "I want to read a file and then search for some information online"

AI Model Response:
1. find-tools with pattern="file|read" 
   → Discovers file reading tools
2. find-tools with pattern="web|search|http"
   → Discovers web search tools
3. get-tool for file reading tool
   → Gets exact parameters needed
4. call-tool to read the file
   → Executes file read
5. get-tool for web search tool  
   → Gets search parameters
6. call-tool to search web
   → Executes web search
```

### **⚠️ Common Mistakes to Avoid**

❌ **DON'T**: Call tools without discovery  
❌ **DON'T**: Guess server names or tool names  
❌ **DON'T**: Use `list-all-tools` as first option  
❌ **DON'T**: Ignore tool schemas from `get-tool`  
❌ **DON'T**: Assume parameter formats  

✅ **DO**: Always discover first with `find-tools`  
✅ **DO**: Use exact names from discovery results  
✅ **DO**: Check schemas with `get-tool`  
✅ **DO**: Provide all required parameters  
✅ **DO**: Use specific search patterns  

### **🏆 Advanced Usage Tips**

1. **🔄 Chain Operations**: Use results from one tool as input to another
2. **📚 Learn Patterns**: Remember successful tool combinations
3. **🎯 Context Optimization**: Use the hub's filtering to reduce noise
4. **🛠️ Error Recovery**: Have fallback strategies for failed tool calls
5. **📊 Batch Processing**: Prefer tools that can handle multiple items

**Remember: This hub exists to make you more efficient and accurate. Use its discovery features actively, and always validate before executing.**

## Usage

The MCP-Hub-MCP server provides the following tools:

### 1. `list-all-tools`

Returns a list of tools from all connected servers.

```json
{
  "name": "list-all-tools",
  "arguments": {}
}
```

### 2. `call-tool`

Calls a tool on a specific server.

- `serverName`: Name of the MCP server to call the tool from
- `toolName`: Name of the tool to call
- `toolArgs`: Arguments to pass to the tool

```json
{
  "name": "call-tool",
  "arguments": {
    "serverName": "filesystem",
    "toolName": "readFile",
    "toolArgs": {
      "path": "/Users/username/Desktop/example.txt"
    }
  }
}
```

### 3. `find-tools`

Find tools matching a regex pattern across all connected servers (grep-like functionality).

- `pattern`: Regex pattern to search for in tool names and descriptions
- `searchIn`: Where to search: "name", "description", or "both" (default: "both")
- `caseSensitive`: Whether the search should be case-sensitive (default: false)

```json
{
  "name": "find-tools",
  "arguments": {
    "pattern": "file",
    "searchIn": "both",
    "caseSensitive": false
  }
}
```

Example patterns:
- `"file"` - Find all tools containing "file"
- `"^read"` - Find all tools starting with "read"
- `"(read|write).*file"` - Find tools for reading or writing files
- `"config$"` - Find tools ending with "config"

Example output:
```json
{
  "filesystem": [
    {
      "name": "readFile",
      "description": "Read the contents of a file",
      "inputSchema": {
        "type": "object",
        "properties": {
          "path": {
            "type": "string",
            "description": "Path to the file to read"
          }
        },
        "required": ["path"]
      }
    },
    {
      "name": "writeFile",
      "description": "Write content to a file",
      "inputSchema": {
        "type": "object",
        "properties": {
          "path": {
            "type": "string",
            "description": "Path to the file to write"
          },
          "content": {
            "type": "string",
            "description": "Content to write to the file"
          }
        },
        "required": ["path", "content"]
      }
    }
  ]
}
```

## Commit Message Convention

This project follows [Conventional Commits](https://www.conventionalcommits.org/) for automatic versioning and CHANGELOG generation.

Format: `<type>(<scope>): <description>`

Examples:

- `feat: add new hub connection feature`
- `fix: resolve issue with server timeout`
- `docs: update API documentation`
- `chore: update dependencies`

Types:

- `feat`: New feature (MINOR version bump)
- `fix`: Bug fix (PATCH version bump)
- `docs`: Documentation only changes
- `style`: Changes that do not affect the meaning of the code
- `refactor`: Code change that neither fixes a bug nor adds a feature
- `perf`: Code change that improves performance
- `test`: Adding missing tests or correcting existing tests
- `chore`: Changes to the build process or auxiliary tools

Breaking Changes:
Add `BREAKING CHANGE:` in the commit footer to trigger a MAJOR version bump.

## Other Links

- [MCP Reviews](https://mcpreview.com/mcp-servers/warpdev/mcp-hub-mcp)

## License

MIT
