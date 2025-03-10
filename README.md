# mcp-server-qdrant: A Qdrant MCP server
[![smithery badge](https://smithery.ai/badge/mcp-server-qdrant)](https://smithery.ai/protocol/mcp-server-qdrant)

> The [Model Context Protocol (MCP)](https://modelcontextprotocol.io/introduction) is an open protocol that enables seamless integration between LLM applications and external data sources and tools. Whether you’re building an AI-powered IDE, enhancing a chat interface, or creating custom AI workflows, MCP provides a standardized way to connect LLMs with the context they need.

This repository is an example of how to create a MCP server for [Qdrant](https://qdrant.tech/), a vector search engine.

<a href="https://glama.ai/mcp/servers/9ejy5scw5i"><img width="380" height="200" src="https://glama.ai/mcp/servers/9ejy5scw5i/badge" alt="mcp-server-qdrant MCP server" /></a>

## Overview

A basic Model Context Protocol server for keeping and retrieving memories in the Qdrant vector search engine.
It acts as a semantic memory layer on top of the Qdrant database.

## Components

### Tools

1. `qdrant-store-memory`
   - Store a memory in the Qdrant database
   - Input:
     - `information` (string): Memory to store
   - Returns: Confirmation message
2. `qdrant-find-memories`
   - Retrieve a memory from the Qdrant database
   - Input:
     - `query` (string): Query to retrieve a memory
   - Returns: Memories stored in the Qdrant database as separate messages

## Installation

### Using uv (recommended)

When using [`uv`](https://docs.astral.sh/uv/) no specific installation is needed to directly run *mcp-server-qdrant*.

```shell
uv run mcp-server-qdrant \
  --qdrant-url "http://localhost:6333" \
  --qdrant-api-key "your_api_key" \
  --collection-name "my_collection" \
  --embedding-model "sentence-transformers/all-MiniLM-L6-v2"
```

### Installing via Smithery

To install Qdrant MCP Server for Claude Desktop automatically via [Smithery](https://smithery.ai/protocol/mcp-server-qdrant):

```bash
npx @smithery/cli install mcp-server-qdrant --client claude
```

## Usage with Claude Desktop

To use this server with the Claude Desktop app, add the following configuration to the "mcpServers" section of your `claude_desktop_config.json`:

```json
{
  "qdrant": {
    "command": "uvx",
    "args": [
      "mcp-server-qdrant",
      "--qdrant-url",
      "http://localhost:6333",
      "--qdrant-api-key",
      "your_api_key",
      "--collection-name",
      "your_collection_name"
    ]
  }
}
```

Replace `http://localhost:6333`, `your_api_key` and `your_collection_name` with your Qdrant server URL, Qdrant API key
and collection name, respectively. The use of API key is optional, but recommended for security reasons, and depends on
the Qdrant server configuration.

This MCP server will automatically create a collection with the specified name if it doesn't exist.

By default, the server will use the `sentence-transformers/all-MiniLM-L6-v2` embedding model to encode memories.
For the time being, only [FastEmbed](https://qdrant.github.io/fastembed/) models are supported, and you can change it
by passing the `--embedding-model` argument to the server.

### Using the local mode of Qdrant

To use a local mode of Qdrant, you can specify the path to the database using the `--qdrant-local-path` argument:

```json
{
  "qdrant": {
    "command": "uvx",
    "args": [
      "mcp-server-qdrant",
      "--qdrant-local-path",
      "/path/to/qdrant/database",
      "--collection-name",
      "your_collection_name"
    ]
  }
}
```

It will run Qdrant local mode inside the same process as the MCP server. Although it is not recommended for production.

## Environment Variables

The configuration of the server can be also done using environment variables:

- `QDRANT_URL`: URL of the Qdrant server, e.g. `http://localhost:6333`
- `QDRANT_API_KEY`: API key for the Qdrant server
- `COLLECTION_NAME`: Name of the collection to use
- `EMBEDDING_MODEL`: Name of the embedding model to use
- `EMBEDDING_PROVIDER`: Embedding provider to use (currently only "fastembed" is supported)
- `QDRANT_LOCAL_PATH`: Path to the local Qdrant database

You cannot provide `QDRANT_URL` and `QDRANT_LOCAL_PATH` at the same time.

## Contributing

If you have suggestions for how mcp-server-qdrant could be improved, or want to report a bug, open an issue!
We'd love all and any contributions.

### Testing `mcp-server-qdrant` locally

The [MCP inspector](https://github.com/modelcontextprotocol/inspector) is a developer tool for testing and debugging MCP
servers. It runs both a client UI (default port 5173) and an MCP proxy server (default port 3000). Open the client UI in
your browser to use the inspector.

```shell
npx @modelcontextprotocol/inspector uv run mcp-server-qdrant \
  --collection-name test \
  --qdrant-local-path /tmp/qdrant-local-test
```

Once started, open your browser to http://localhost:5173 to access the inspector interface.

## License

This MCP server is licensed under the Apache License 2.0. This means you are free to use, modify, and distribute the
software, subject to the terms and conditions of the Apache License 2.0. For more details, please see the LICENSE file
in the project repository.
