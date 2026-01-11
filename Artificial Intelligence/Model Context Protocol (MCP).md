https://www.youtube.com/watch?v=SB1dFNogZKc

https://modelcontextprotocol.io/docs/getting-started/intro

<img width="1510" height="410" alt="image" src="https://github.com/user-attachments/assets/800b8752-b48c-47cf-8878-fe413f3a12f6" />

Model Context Protocol (MCP) is an AI framework that lets your LLM model to call external tools or function to actually execute a user prompt as a real action. For eg, "Add one iPhone 16 pro max to my Amazon shopping cart".

Suppose you ask the model "Book a flight for me from Bangalore to Bagdogra on date 25th August 2025 also check for some better discount". The prompt goes to a Large Language Model (for eg, Claude LLM). The model will use Natural Language Processing (NLP) to figure out your intent & extract the important details from your input. Suppose the flight service provider is Amadeus, how can the LLM model connect to Amadeus service? MCP will act as a bridge between the LLM model and the service provider. The service provider needs to expose a set of tools (fucntions) through their MCP server. Behind the scenes, LLM will convert the plain text to JSON-RPC format and call the correct tool. 

<img width="1167" height="620" alt="image" src="https://github.com/user-attachments/assets/715991af-2394-4bfb-8909-8f12479d810b" />

While designing AI tools, we need to follow some rules / contracts so that AI can understand and use them correctly, for how our AI model and external tool can talk to each other. That contract is called MCP.

Why MCP and not simple REST API?
- In REST APIs, its easy to mess up with APIs. For eg, we can perform update or delete operations inside a GET method. The code will work but thats not the standard format.

<img width="600" height="300" alt="image" src="https://github.com/user-attachments/assets/b3e0e888-b83a-4cf5-bd6c-da1beb6c5beb" />

<img width="1000" height="500" alt="image" src="https://github.com/user-attachments/assets/f46eff7f-3bc9-4d99-bd17-5ba52c95b664" />

---

MCP allows you to retrieve data from a source and do something with it. It's a protocol that provides context for language models to interact with resources, run tools, etc.

Installation:
- https://claude.ai/download
- https://docs.astral.sh/uv/getting-started/installation/#standalone-installer

```
 set Path=C:\Users\Biswajit\.local\bin;%Path%   (cmd)
 $env:Path = "C:\Users\Biswajit\.local\bin;$env:Path"   (powershell)
```

https://github.com/modelcontextprotocol/servers

Terminologies:
- MCP Host: any application (Claude Desktop, Cursor) that wants to access data through an MCP.
- MCP Client: an extension to your application that maintains the protocol connection between the application and the MCP server.
- MCP Server: or simply MCP, is a package of programs that expose some resource either to retrieve data or to do something with that data that the MCP Client can interact with
- Models means Anthropic's Claude models.

What can MCP's do?
1. Resources: providing direct access to specific data
2. Prompts: providing the language model with customized prompt to interact with that data
3. Tools: performing actions on or with the data
4. Sampling: allows the MCP Server to request completions from the language model
5. Roots: define the boundaries inside which the MCP server can operate
