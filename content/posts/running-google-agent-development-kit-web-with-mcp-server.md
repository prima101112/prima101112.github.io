---
title: "Running Google Agent Development Kit Web With Mcp Server"
date: 2025-04-22T12:04:54+07:00
draft: false
author: "prima adi"
categories: ['technology', 'blog','ai']
tags: ['technology', 'MCP','ai']
languageCode: 'en-us'
---

### This is a 3 days story

In this AI world right now, people are easily creating tools and what we now call "agents". As a person who opened this page, you will already be familiar with MCP or Model Context Protocol. MCP is great, but suddenly Google is releasing ADK, which stands for Agent Development Kit. It's a little bit confusing here, actually, while the Langchain platform is also expanding. Whatever.

In this ADK, there is a command that interests me: it's `adk web`, which will launch an agent in a web-based interface. I usually use Claude desktop for interacting with MCP or a VSCode extension. But for the sake of learning, ADK said there's a way of connecting MCPTools to ADK so all tools will be listed in ADK.

First, I coded myself as a standard Python engineer to logically get tools from MCPToolset and return them to the agent. Following this documentation [mcp with adk web](https://google.github.io/adk-docs/tools/mcp-tools/#mcp-with-adk-web), there were so many errors. First, adk web is running its own `asyncio`, so when we call an async function to generate an agent, it will fail.

```python

async def create_agent():
  """Gets tools from MCP Server."""
  tools, exit_stack = await get_tools_async()

  agent = LlmAgent(
      model='gemini-2.0-flash', # Adjust model name if needed based on availability
      name='filesystem_assistant',
      instruction='Help user interact with the local filesystem using available tools.',
      tools=tools, # Provide the MCP tools to the ADK agent
  )
  return agent, exit_stack


root_agent = create_agent()
```

after runnig that we will get

```
pydantic_core._pydantic_core.ValidationError: 1 validation error for InvocationContext

agent

Input should be a valid dictionary or instance of BaseAgent [type=model_type, input_value=<function create_agent at 0x11df0cae0>, input_type=function]

For further information visit
```

That seems legit *from the example*, but the problem is Pydantic not allowing that. root_agent SHOULD BE an Agent definition. So try to call it.

So, as a Python dev, OK, will call it using `asyncio.run()`.

And then...

```bash
asyncio.run() cannot be called from a running event loop
```

Yeah, it's a little bit frustrating here. I don't know, I am starting to blame why there is already an MCP client and MCP server. Why am I trying this? Why should ADK be working with MCP?

> 2 days gone by

In this early morning, I am still curious about all of that. It should work because calling it manually through main and really waiting for the get_tools function will work. [(This actually works)](https://google.github.io/adk-docs/tools/mcp-tools/#1-using-mcp-servers-with-adk-agents-adk-as-an-mcp-client) But using ADK web **IS NOT**.

So now I have the full power of all LLM, free and paid. And still, LLM is just going back and forth giving bad examples.

Until.....

---

### the power of understanding your code so you could write a clear prompt

in some point i write a prompt, i really carefull to explain what i understand.

```
this code is running on adk web. that this adk web already have his own asyncio running. i cannot wait it like using asyncio.run etc.
how could i still wait asyncio inside already running asyncio

```

But I don't know if this is legit or not, since asyncio should prevent this kind of nested event loop. It could easily crash if not handled properly.

And this is the final example of integrating MCP with ADK web. `agent.py`

```python
import asyncio
import atexit
import os
from contextlib import AsyncExitStack
from dotenv import load_dotenv
from google.adk.agents import Agent, LlmAgent
from google.adk.tools.mcp_tool.mcp_toolset import MCPToolset, StdioServerParameters
# MUST HERE. this is what make me stuck with docs for almost 3 days
import nest_asyncio

# Global exit stack for managing async resources
_exit_stack = AsyncExitStack()

# Initialize tools and agent synchronously during module import
try:
    # Get or create an event loop
    try:
        print("Getting event loop...and use old")
        loop = asyncio.get_event_loop()
    except RuntimeError:
        print("ther is no old so create new event loop")
        # Create a new event loop if one doesn't exist
        loop = asyncio.new_event_loop()
        asyncio.set_event_loop(loop)
    
    # Define async initialization function
    async def _initialize():
        print("Attempting to connect to MCP Filesystem server...")
        # Get tools from the MCP Server
        mcp_toolset, tools_exit_stack = await MCPToolset.from_server(
            connection_params=StdioServerParameters(
                command='npx',
                args=["-y", 
                      "@modelcontextprotocol/server-filesystem",
                      "/Users/prima/work/agent-dev-kit-try/test-agent/"], #replace with your folder
            )
        )
        print("MCP Toolset created successfully.")
        
        # Add the tools exit stack to our main exit stack
        await _exit_stack.enter_async_context(tools_exit_stack)
        
        # Extract the actual tools from the toolset
        # This is critical - we need to get the tool objects to register with the agent
        tools = mcp_toolset
        
        print(f"Found {len(tools)} tools: {', '.join([tool.name for tool in tools])}")
        
        # Create and return the agent with explicit tools
        agent = LlmAgent(
            model='gemini-2.0-flash',
            name='filesystem_assistant',
            instruction='''
            You are a helpful assistant that can interact with the filesystem.
            Use the provided tools to help users manage their files and directories.
            Be sure to demonstrate the tools when appropriate.
            ''',
            tools=tools,  # Pass the list of tools, not the toolset
        )
        print("Agent initialized successfully.")
        return agent
    
  
    nest_asyncio.apply()
    root_agent = loop.run_until_complete(_initialize())
    
    # Register cleanup handler
    def _cleanup():
        try:
            if not loop.is_closed():
                loop.run_until_complete(_exit_stack.aclose())
        except Exception as e:
            print(f"Error during cleanup: {e}")
    
    atexit.register(_cleanup)

except Exception as e:
    print(f"Error initializing agent: {e}")
    # Provide a fallback agent if initialization fails
    root_agent = LlmAgent(
        model='gemini-2.0-flash',
        name='filesystem_assistant',
        instruction='Help user interact with the local filesystem. Note: Tools initialization failed.',
    )

```

Put that file in this folder structure:

![adk x mcp](/img/adkxmcp.png)

And run:

`adk web`

in the root folder of test-agent.

## Conclusion

The Google Agent Development Kit (ADK) Web is a valuable addition to the agent development ecosystem, complementing existing tools like the Model Context Protocol (MCP) server. This article demonstrated how ADK Web can be integrated with an MCP server, showcasing the potential for combining these tools.

It's important to note that ADK is not a replacement for MCP, but rather another tool in the developer's arsenal. While we focused on a specific use case here, ADK offers a much broader range of features and capabilities that developers should explore.

For developers, this integration serves as a learning opportunity to understand how different agent development frameworks can work together. As we continue this journey in this field, remember to stay curious, experiment with various tools and combinations, and remain open to new approaches.

Happy coding. 
