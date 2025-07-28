# MCP-Hub-MCP Usage Rules for AI Models

## CRITICAL: THIS IS YOUR CENTRAL TOOL HUB

**YOU ARE WORKING WITH A HUB-OF-HUBS.** This MCP server is your unified gateway to ALL available tools from multiple MCP servers. It prevents context pollution and manages tool discovery intelligently.

## MANDATORY WORKFLOW - NEVER DEVIATE

### Step 1: TOOL DISCOVERY (ALWAYS FIRST)
**NEVER call tools blindly. ALWAYS discover first.**

**🎯 PRIMARY METHOD: Smart Search**
```
Tool: find-tools
Parameters: { "pattern": "specific_keyword_related_to_task" }
Examples:
- find-tools with pattern="file" → finds file-related tools
- find-tools with pattern="web|http" → finds web/API tools  
- find-tools with pattern="database|sql" → finds database tools
```

**📋 ALTERNATIVE: Server-Specific Discovery**
```
Tool: list-tools-in-server  
Parameters: { "serverName": "known_server_name" }
Use when you know the specific server name
```

**⚠️ LAST RESORT: Complete Inventory**
```
Tool: list-all-tools
Parameters: {} (empty)
WARNING: Only use when find-tools fails to find what you need
Returns ALL tools from ALL servers - can overwhelm context
```

### Step 2: SCHEMA VALIDATION (MANDATORY)
**Before calling ANY tool, get its complete schema:**
```
Tool: get-tool
Parameters: { 
  "serverName": "exact_server_name_from_discovery",
  "toolName": "exact_tool_name_from_discovery" 
}
This reveals required parameters, types, and constraints
```

### Step 3: TOOL EXECUTION
```
Tool: call-tool
Parameters: {
  "serverName": "exact_server_name", 
  "toolName": "exact_tool_name",
  "toolArgs": { "param1": "value1", "param2": "value2" }
}
```

## NON-NEGOTIABLE RULES

1. **🔍 DISCOVERY FIRST**: Never execute without discovering via find-tools
2. **📝 EXACT NAMES**: Use EXACT serverName and toolName from discovery - NO GUESSING
3. **🎯 SPECIFIC PATTERNS**: Use targeted keywords in find-tools, not generic terms
4. **✅ SCHEMA CHECK**: Always validate with get-tool before execution
5. **🧠 CONTEXT EFFICIENCY**: This hub prevents pollution - use it properly

## USAGE PATTERNS BY TASK TYPE

### File Operations
```
1. find-tools → pattern="file|read|write|directory"
2. get-tool → validate file path requirements  
3. call-tool → execute with proper paths
```

### Web/API Operations  
```
1. find-tools → pattern="web|http|api|request|url"
2. get-tool → check URL and header requirements
3. call-tool → execute with proper endpoints
```

### Database Operations
```
1. find-tools → pattern="database|sql|query|connection"  
2. get-tool → validate connection parameters
3. call-tool → execute with proper credentials
```

### Search/Information Retrieval
```
1. find-tools → pattern="search|find|lookup|retrieve"
2. get-tool → check query format requirements
3. call-tool → execute with targeted queries
```

## PERFORMANCE OPTIMIZATION

1. **🎯 TARGETED SEARCH**: Use specific, relevant keywords in patterns
2. **🔄 REUSE DISCOVERY**: Remember tool locations within conversation
3. **📊 BATCH PROCESSING**: Prefer tools handling multiple operations
4. **🧹 FILTERED RESPONSES**: Hub shows only name+description for discovery

## ERROR HANDLING PROTOCOLS

### When Tool Discovery Fails:
```
1. Try different keyword patterns in find-tools
2. Use broader terms if too specific
3. Use list-all-tools as absolute last resort
4. Verify the required MCP server is actually connected
```

### When Tool Execution Fails:
```
1. Verify serverName and toolName are EXACT matches from discovery
2. Re-check schema with get-tool
3. Validate all required parameters are provided
4. Ensure correct data types for all parameters
5. Check for missing environment variables or permissions
```

## COMMON MISTAKES - AVOID THESE

❌ **NEVER**: Call tools without prior discovery
❌ **NEVER**: Guess or assume server/tool names  
❌ **NEVER**: Skip schema validation with get-tool
❌ **NEVER**: Use list-all-tools as first option
❌ **NEVER**: Assume parameter formats or types

✅ **ALWAYS**: Start with find-tools using specific patterns
✅ **ALWAYS**: Use exact names from discovery results
✅ **ALWAYS**: Validate schemas before execution  
✅ **ALWAYS**: Provide all required parameters
✅ **ALWAYS**: Use appropriate data types

## EXAMPLE: PROPER WORKFLOW

**Task**: "Read a configuration file and update a web API"

**Correct Approach**:
```
1. find-tools with pattern="file|read"
   → Discovers: { serverName: "filesystem", toolName: "readFile" }

2. get-tool with serverName="filesystem", toolName="readFile"  
   → Schema: { "path": "string (required)" }

3. call-tool with serverName="filesystem", toolName="readFile", 
   toolArgs={"path": "/path/to/config.json"}
   → Reads file content

4. find-tools with pattern="web|http|api"
   → Discovers: { serverName: "web-client", toolName: "httpRequest" }

5. get-tool with serverName="web-client", toolName="httpRequest"
   → Schema: { "url": "string", "method": "string", "data": "object" }

6. call-tool with serverName="web-client", toolName="httpRequest",
   toolArgs={"url": "https://api.example.com", "method": "POST", "data": {...}}
   → Updates API
```

## ADVANCED TECHNIQUES

1. **🔗 CHAIN OPERATIONS**: Use output from one tool as input to next
2. **📚 PATTERN LEARNING**: Remember successful keyword combinations  
3. **🎯 CONTEXT OPTIMIZATION**: Leverage hub's noise reduction
4. **🛠️ ERROR RECOVERY**: Have fallback discovery strategies
5. **📊 EFFICIENCY**: Prefer tools with batch capabilities

## DEBUGGING CHECKLIST

When things go wrong:
- [ ] Did I discover the tool first with find-tools?
- [ ] Are serverName and toolName exact matches from discovery?
- [ ] Did I validate the schema with get-tool?
- [ ] Are all required parameters provided?
- [ ] Are parameter types correct (string vs number vs object)?
- [ ] Is the target MCP server actually running and connected?

## REMEMBER

This hub exists to make you **more efficient and accurate**. It prevents context pollution by managing tool discovery intelligently. Use its discovery features actively, validate everything, and never assume. Your success depends on following this workflow religiously.

**The hub is your tool discovery engine - use it properly and it will make you unstoppable.** 