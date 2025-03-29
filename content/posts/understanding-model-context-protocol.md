---
title: "Understanding Model Context Protocol"
date: 2025-03-26T21:42:38+07:00
draft: false
author: "prima adi"
categories: ['technology', 'blog','ai']
tags: ['technology', 'MCP','ai']
languageCode: 'en-us'
---
## Journey understanding it

### What is Model Context Protocol

in this recent days like 2 weeks back i was stumbling upon an agentic AI, what is agentic we all does not really know it.
agentic is the AI act like agent they understand what you need and doing certain action also choose tools to do what you need, thats whats my understanding of agentic AI. but for more prcise Agentic AI refers to artificial intelligence that acts like an agent capable of understanding user needs, **selecting appropriate tools**, and **performing actions autonomously**.

for supporting this Anthropic are creating a standard called Model Context Protocol
this is from their site

> MCP is an open protocol that standardizes how applications provide context to LLMs. Think of MCP like a USB-C port for AI applications. Just as USB-C provides a standardized way to connect your devices to various peripherals and accessories, MCP provides a standardized way to connect AI models to different data sources and tools.

i am already try this model context protocol using to communicate with k8s or getting insight from sqlite database small helloworld to MCP. but how its actually work.

### Why Most MCP are run on local

when we use claude desktop or other mcp client mostly the mcp config look like this

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
    }
  }
}
```

if we see this, there is command that run npx and run the server localy.
This launches a local MCP server to access files on your Desktop and Downloads folders. Why local? The protocol’s standards for remote authentication and security are still in development. For example, if you hosted an MCP server for a cloud-based PostgreSQL database, the server might connect to the database fine, but the client-to-server link would need manual security (e.g., behind an authentication layer) to be safe.

lets say we have postgresql in a cloud and we deploy MCP server postgress and connect to it.
mcp to postgress are have a limited access but MCP client to MCP server are still no security or auth, we should manualy put a server behind auth to make this actually work.

to actually understanding this MCP client and MCP server relation in local and if we want to do it in remote server as follows

### MCP server in local

you may understand it better if you could see this sequence diagram. all of this state are happend in local. even whe call it mcp server its actually running on local and note.txt also in local

{{% mermaid %}}
sequenceDiagram
    participant U as User
    participant C as MCP Client
    participant S as MCP Server
    participant A as AI Model
    U->>C: "Summarize notes.txt"
    C->>A: Sends user request
    C->>S: MCP Request (stdio)
    S-->>S: Accesses local file (e.g., notes.txt)
    S->>C: Returns content (stdio)
    C->>A: Forwards content to AI
    A-->>A: Processes into summary
    A->>C: Sends summary
    C->>U: Displays summary
{{% /mermaid %}}

### MCP remote server

for MCP remote server, auth is essentials to actually make it secure to connect to resource we want and in this case note.txt is actually on remote server alongside with MCP server.

{{% mermaid %}}
sequenceDiagram
    participant U as User
    participant C as MCP Client
    participant S as MCP Server (Remote)
    participant A as AI Model
    participant Auth as Auth Service
    U->>C: "Summarize notes.txt (remote)"
    C->>Auth: Sends credentials (e.g., SSH key/API token)
    Auth-->>C: Auth success/failure
    alt Auth success
        C->>S: MCP Request (over SSH/HTTP with auth)
        S-->>S: Accesses remote file (notes.txt)
        S->>C: Returns content (over secure channel)
        C->>A: Forwards content to AI
        A-->>A: Processes into summary
        A->>C: Sends summary
        C->>U: Displays summary
    else Auth failure
        C->>U: "Authentication failed"
    end
{{% /mermaid %}}

next we should try to learn how to build one. 
Thanks

