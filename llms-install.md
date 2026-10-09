# Installing the GrabGPU MCP server

GrabGPU is a hosted, remote MCP server. Do not clone or build anything.

1. Add this entry to the MCP settings file:

```json
{
  "mcpServers": {
    "grabgpu": {
      "type": "streamableHttp",
      "url": "https://grabgpu.com/mcp"
    }
  }
}
```

2. No API key, environment variable or sign-in is needed.
3. Check the connection: the server lists one tool, `find_gpu`. Call it with `{"gpu": "H100", "limit": 3}`; it returns in-stock offers with prices.
