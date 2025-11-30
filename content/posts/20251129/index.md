---
title: Seting up the new Uno Platform's MCP Servers
date: 2025-11-29T22:00:00-05:00
draft: false
---

As I write this, .NET Conf is just behind us. As it's mandatory every year, there is an endless string of announcements around .NET 10, C\# 14, AI building blocks, and a revamped and rebranded Aspire, among many other topics. I'll be covering some of that in the next few articles, but in today's post, I'd like to take a closer look at a different announcement from the Uno Platform team: [Uno Platform 6.4](https://platform.uno/blog/uno-platform-6-4/). This release introduces (among many other things) two new MCP Servers, which promise to make it a lot easier to create awesome stuff by pairing Uno Platform with your favorite agentic AI tool.

## Using AI to overcome .NET project complexity

.NET cross-platform frameworks can be very complex. There are often a lot of different moving pieces that need to work well in sync, and they require a good understanding of both mobile platforms and .NET patterns. Even knowing what to do and where to look when you don't, frequent changes in the .NET architecture and tooling can make things difficult.

The Uno Platform team has come up with a number of tools that aim to leverage the power of AI to overcome these obstacles.

  - **Hot Design Agent**: It's a built-in agent baked into the powerful Hot Design feature to have AI agents make changes to the UI of your app WHILE IT RUNS.
  - **App MCP**: Gives AI agents control over the app surface to allow interactions with the app's actual UI and further extend the functionality already available as part of Hot Design. IDE-integrated AI agents such as VS Code and Visual Studio, as well as CLI-based agents (Claude Code, Gemini CLI, etc.), are supported.
  - **Agentic MCP Server**: Extends the knowledge base of AI agents to create apps using the latest Uno Platform improvements available and provide a better understanding to resolve potential issues.

[In my previous article](../20251104/), I covered MCP servers and I talked about how easy it is to create your own MCP server in C\#. MCP servers are a great way to extend the abilities of your AI agent by giving them access to functionality not necessarily available as part of the training data of whatever model they're powered by. You can easily tap into Uno Platform's agentic MCP server to make your AI agent:

  - Verify your system to make sure it's ready to develop Uno Platform applications.
  - Create new apps or add features to existing apps by deciding which features are more convenient.
  - Tap into the Uno Platform documentation to provide access for agentic AI tools to the latest updated information about Uno Platform development.

This is a game-changer for AI-driven Uno Platform developers. Tapping into the Uno MCP server means that you can easily give your AI agent access to the latest documentation to more easily debug and diagnose issues with your application. Furthermore, using the [Model Context Protocol](https://modelcontextprotocol.io/docs/getting-started/intro) means that you're not limited to a specific agentic tool, so whatever your choice of agentic tool is, you can use it.

## Adding the Uno Platform MCP Server

The Uno Platform MCP Server provides a set of tools to query the Uno Platform's Knowledge base in order to give your AI agent access to the latest up-to-date documentation and help it make the best decision. It also includes tools to create new projects and initialize them with the recommended best practices. More information about the available tools can be found in the [Uno Platform's docs](https://platform.uno/docs/articles/features/using-the-uno-mcps.html#mcp-remote)

A great thing about the Uno Platform MCP server is that it's HTTP-based; this means no installation is necessary to use it. However, adding it to your agent involves different steps depending on which one you use.

### GitHub Copilot (VS Code, Visual Studio)

The easiest way to install the Uno Platform MCP Server in VS Code's integrated GitHub Copilot is by using VS Code's Command Palette.

1.  Press `Cmd+Shift+P` (Mac) or `Ctrl+Shift+P` (Windows) to open the Command Palette.
2.  In the search box, start typing "MCP" and pick the option "Add Server..."
3.  In the next prompt, pick "HTTP (HTTP Or Server-Sent Events)"
4.  Enter the URL `https://mcp.platform.uno/v1`

This will create (or update if it exists) a file called `mcp.json` in the `.vscode` folder with something that looks like this:

```json
{
  "servers": {
    "uno": {
      "url": "https://mcp.platform.uno/v1"
    }
  }
}
```

If you use Visual Studio 2022 or 2026 as your preferred IDE, this setup will also make the Uno Platform MCP server available in your project. Alternatively, you can place the `mcp.json` file in the `.vs` folder instead, and it will be available to both VS Code and Visual Studio.

### Claude Code

If Claude Code is your preferred AI Agent, you can easily add the Uno Platform MCP Server by executing the following in the project directory:

```bash
claude mcp add --transport http uno https://mcp.platform.uno/v1
```

Alternatively, you can manually create or edit the `.mcp.json` file in the directory root to look like this:

```json
{
  "mcpServers": {
    "api-server": {
      "type": "http",
      "url": "https://mcp.platform.uno/v1"
    }
  }
}
```

### Gemini CLI

For Gemini CLI, the process is fairly similar to Claude Code:

```bash
gemini mcp add uno https://mcp.platform.uno/v1
```

If you prefer to add the MCP manually, open the `.gemini\settings.json` file in the directory root using your favorite code editor and make sure this is added to the file:

```json
{
  "mcpServers": {
    "uno": {
      "httpUrl": "https://mcp.platform.uno/v1",
    }
  }
}
```

## Uno Platform's App MCP Server

Uno Platform 6.4 also introduces an App MCP Server. This runs locally within the context of the app and provides tools to support testing and debugging. You can find more info about the Uno Platform's App MCP in the [Uno Platform Docs](https://platform.uno/docs/articles/features/using-the-uno-mcps.html#app-mcp-local).

Setting up the Uno Platform's App MCP Server is also implemented as an HTTP MCP Server, but it runs locally instead of remotely. Configuration for the App MCP Server usually looks like this:

```json
{
  "servers": {
    "uno": {
      "url": "http://localhost:65392/mcp"
    }
  }
}
```

## Conclusion

The introduction of MCP servers in Uno Platform 6.4 marks a significant step forward in bridging the gap between complex cross-platform development and modern AI capabilities. By exposing the latest documentation and system verification tools directly to your AI agent—whether you prefer GitHub Copilot, Claude, or Gemini—Uno Platform allows you to spend less time wrestling with configuration and more time building features.

When you combine the remote Agentic MCP server for knowledge retrieval with the local App MCP server for runtime debugging, you get a robust, AI-assisted workflow that stays up-to-date with the latest framework changes. If you haven't configured these servers for your preferred agent yet, give it a try; it’s a small setup step that can yield a massive productivity boost.