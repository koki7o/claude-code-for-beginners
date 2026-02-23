# Module 11: MCP Servers - Extending Claude Code

**Goal:** Learn to use and create Model Context Protocol (MCP) servers to extend Claude Code's capabilities

**Estimated Time:** 45-55 minutes

---

## What You'll Learn

MCP is how Claude Code goes from a general-purpose coding assistant to something tailored specifically to your stack, your tools, your workflow. This module covers what MCP actually is, how to install and configure servers, which ones are worth your time (and which aren't), and how to build your own from scratch.

---

## Lesson 1: Understanding MCP

### What is MCP?

**Model Context Protocol (MCP)** is a standard way to give Claude Code access to external data and services. Here's the deal: Claude Code is already good at a lot of things out of the box, but MCP is how you make it great at *your* specific things.

Think of MCP servers as plugins. Each one gives Claude a new capability -- maybe it's the ability to query your database, maybe it's pulling live documentation, maybe it's driving a browser. They all follow the same protocol, so once you understand how one works, you understand how they all work.

---

### Why Use MCP?

Without MCP, you're stuck with Claude Code's built-in tools. That's fine for general coding work, but the moment you need to hit your company's database, check a deployment status, or pull docs for some niche library, you're back to copy-pasting context manually.

With MCP, Claude can reach directly into your databases, talk to your APIs, pull live documentation, and integrate with tools like GitHub and Notion -- all without you playing middleman. Once you start using MCP servers, working without them feels painful.

---

## Lesson 2: Installing MCP Servers

### Configuration Files

MCP servers live in simple JSON config files. There are two places they can go:

Project-level (shared with your team via version control):
```
.mcp.json    # in your project root
```

User-level (your personal global config):
```
~/.claude/settings.json    # under the "mcpServers" key
```

Most of the time you want project-level config. That way everyone on the team gets the same MCP servers when they work on the project. Use the user-level config for servers you want available everywhere, regardless of which project you're in.

Here's what each looks like in practice:

Project-level (`.mcp.json`):
```json
{
  "mcpServers": {
    "filesystem": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-filesystem", "/path/to/allowed/directory"]
    }
  }
}
```

User-level (`~/.claude/settings.json`):
```json
{
  "mcpServers": {
    "filesystem": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-filesystem", "/path/to/allowed/directory"]
    }
  }
}
```

---

### Installing Your First MCP Server

The easiest way to get started is to just ask Claude Code for help:

```
Help me install the filesystem MCP server.
I want to give you access to my /home/user/projects directory.
```

Claude Code will show you exactly what configuration to add, help you edit the right file, and verify that everything is wired up correctly. No need to fumble through docs on your own.

---

### MCP Server Settings

A couple of settings worth knowing about:

`enableAllProjectMcpServers` -- Set this to `true` in your user settings if you're tired of Claude Code asking you to approve project MCP servers every time. Only do this if you trust the projects you work on.

You can also allowlist or blocklist specific MCP servers so you have fine-grained control over which ones are permitted to run. This is handy if your team adds servers to `.mcp.json` but you don't want all of them active on your machine.

---

## Lesson 3: Common MCP Servers

Here are the servers you'll see mentioned most often. These are the "official" ones from the MCP project itself.

### Filesystem MCP Server

This one gives Claude direct access to files and directories outside the current project. Useful when you need Claude to work across multiple directories or access files that aren't in your repo.

```json
{
  "mcpServers": {
    "filesystem": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-filesystem", "/allowed/path"]
    }
  }
}
```

Then just ask naturally:

```
Using the filesystem MCP server, list all Python files in my projects directory
```

---

### PostgreSQL MCP Server

If your app talks to Postgres, this server lets Claude query it directly. No more dumping query results into the chat -- Claude can just go look.

```json
{
  "mcpServers": {
    "postgres": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-postgres"],
      "env": {
        "POSTGRES_URL": "postgresql://user:pass@localhost:5432/dbname"
      }
    }
  }
}
```

```
Using the postgres MCP server, show me all tables in the database
Query the users table and show me the schema
```

---

### SQLite MCP Server

Same idea as the Postgres server, but for SQLite. Great for local dev databases or smaller projects where Postgres would be overkill.

```json
{
  "mcpServers": {
    "sqlite": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-sqlite", "/path/to/database.db"]
    }
  }
}
```

```
Using the sqlite MCP server, analyze the database schema
Find all tables with user data
```

---

### GitHub MCP Server

This one connects Claude to the GitHub API. Listing repos, reading issues, creating PRs -- all without leaving your terminal.

```json
{
  "mcpServers": {
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_TOKEN": "your-github-token"
      }
    }
  }
}
```

```
Using the GitHub MCP server:
- List my repositories
- Show open issues in my-repo
- Create an issue for the bug I just found
```

---

## Lesson 4: The MCP Servers That Actually Matter

There are hundreds of MCP servers floating around on npm and GitHub. Most of them you'll never need. Don't waste time installing every shiny new one you see on social media. Focus on the ones that solve real, daily problems.

### Context7 - Live Documentation Lookup

This matters more than you think. Context7 pulls live, version-specific documentation so Claude stops hallucinating API signatures. You know that moment when Claude confidently gives you a function call that doesn't exist in the version you're using? Context7 fixes that.

```json
{
  "mcpServers": {
    "context7": {
      "command": "npx",
      "args": ["-y", "@context7/mcp-server"]
    }
  }
}
```

Trust me on this one -- install it first, thank me later.

---

### Playwright - Browser Automation

Need to test a web app? Scrape a page? Verify that your frontend changes actually work? Playwright gives Claude the ability to drive a real browser. It can click buttons, fill forms, take screenshots, and assert on page content.

```json
{
  "mcpServers": {
    "playwright": {
      "command": "npx",
      "args": ["-y", "@anthropic/mcp-server-playwright"]
    }
  }
}
```

If you do any frontend work at all, this one should be on your short list.

---

### Claude in Chrome - Real Browser Debugging

This is different from Playwright, and most people skip it but shouldn't. Instead of automating a headless browser, Claude in Chrome gives Claude access to your *actual* browser context -- the page you're looking at right now, the console errors, the network requests. When you're debugging a frontend issue and you want Claude to see exactly what you see, this is the tool.

```json
{
  "mcpServers": {
    "chrome": {
      "command": "npx",
      "args": ["-y", "@anthropic/claude-in-chrome-mcp-server"]
    }
  }
}
```

---

### DeepWiki - GitHub Repo Documentation

Ever needed to understand how some open-source library works internally? DeepWiki gets you documentation for any GitHub repo without leaving your session. Instead of tab-switching and reading through READMEs, just ask Claude and let DeepWiki fetch the details.

```json
{
  "mcpServers": {
    "deepwiki": {
      "command": "npx",
      "args": ["-y", "@anthropic/mcp-server-deepwiki"]
    }
  }
}
```

---

### Quality Over Quantity

Resist the urge to install 20 MCP servers at once. Each one adds startup time and cognitive overhead. Start with Context7 -- accurate docs matter more than anything -- add Playwright or Chrome when you're doing frontend work, and bring in others as you actually need them. Three well-chosen servers beat fifteen that just sit there.

---

## Lesson 5: Creating a Custom MCP Server

### Why Create Custom Servers?

The pre-built servers cover a lot of ground, but eventually you'll hit something specific to your world -- your company's internal APIs, a custom database, some domain-specific workflow that no open-source server is going to handle. That's when you build your own.

The good news: it's not hard. An MCP server is just a small program that speaks the MCP protocol over standard I/O. If you can write a Node script, you can write an MCP server.

---

### MCP Server Basics

Every MCP server can provide three things:

- **Tools** -- Functions that Claude can call. This is the one you'll use most.
- **Resources** -- Data that Claude can read on demand.
- **Prompts** -- Predefined prompt templates for common workflows.

You don't have to implement all three. Most custom servers start with just a tool or two and grow from there.

---

### Creating a Simple MCP Server

Here's a real example. Say you want Claude to be able to check the weather. This is what the full server looks like:

```javascript
// weather-mcp-server.js
import { McpServer } from "@modelcontextprotocol/sdk/server/mcp.js";
import { StdioServerTransport } from "@modelcontextprotocol/sdk/server/stdio.js";
import { z } from "zod";

const server = new McpServer({
  name: "weather",
  version: "1.0.0"
});

// Define a tool
server.tool(
  "get_weather",
  { city: z.string().describe("City name") },
  async ({ city }) => {
    // Call weather API
    const response = await fetch(
      `https://api.weather.com/...?city=${encodeURIComponent(city)}`
    );
    const data = await response.json();

    return {
      content: [{
        type: "text",
        text: JSON.stringify({
          temperature: data.temp,
          conditions: data.conditions,
          humidity: data.humidity
        }, null, 2)
      }]
    };
  }
);

const transport = new StdioServerTransport();
await server.connect(transport);
```

Notice the pattern: you create a `McpServer`, register tools on it with `server.tool()`, set up a `StdioServerTransport`, and connect. That's the whole structure. Everything else is just your business logic inside the tool handler.

---

### Configuring Your Custom Server

Once your server file exists, point Claude Code at it in your `.mcp.json`:

```json
{
  "mcpServers": {
    "weather": {
      "command": "node",
      "args": ["/path/to/weather-mcp-server.js"],
      "env": {
        "WEATHER_API_KEY": "your-api-key"
      }
    }
  }
}
```

---

### Using Your Custom Server

Now just talk to Claude like normal:

```
Using the weather MCP server, what's the weather in San Francisco?
```

Claude sees the tool, calls it, gets the result, and gives you an answer. No special syntax required.

---

## Lesson 6: Advanced MCP Features

### Resources

Resources let you expose data that Claude can pull in on demand -- think of them as read-only endpoints. Here's how you register one with `server.resource()`:

```javascript
server.resource(
  "company://employees",
  async (uri) => {
    const employees = await db.query('SELECT * FROM employees');
    return {
      contents: [{
        uri: uri.href,
        text: JSON.stringify(employees)
      }]
    };
  }
);
```

This is particularly useful for data that doesn't change often -- employee directories, configuration docs, reference tables. Claude can pull it in when needed without you having to paste it into the conversation.

---

### Prompts

Prompts are predefined templates that standardize how Claude approaches certain tasks. If your team has a specific code review process, for example, you can bake it right into an MCP server:

```javascript
server.prompt(
  "code_review",
  { filename: z.string() },
  async ({ filename }) => {
    const standards = await loadCompanyStandards();
    return {
      messages: [{
        role: "user",
        content: {
          type: "text",
          text: `Review ${filename} according to these standards:\n${standards}`
        }
      }]
    };
  }
);
```

This way, everyone on the team gets the same review criteria, and nobody has to remember the full prompt each time.

---

## Lesson 7: Real-World MCP Examples

### Example 1: Company Database MCP

Say you want Claude to query your company's customer database. Here's a full working server:

```javascript
// company-db-mcp.js
import { McpServer } from "@modelcontextprotocol/sdk/server/mcp.js";
import { StdioServerTransport } from "@modelcontextprotocol/sdk/server/stdio.js";
import { z } from "zod";
import pg from "pg";

const server = new McpServer({
  name: "company-db",
  version: "1.0.0"
});

const db = new pg.Client({
  connectionString: process.env.DATABASE_URL
});
await db.connect();

server.tool(
  "query_customers",
  { filter: z.string().describe("SQL WHERE clause filter") },
  async ({ filter }) => {
    const result = await db.query(
      'SELECT * FROM customers WHERE $1',
      [filter]
    );
    return {
      content: [{
        type: "text",
        text: JSON.stringify(result.rows, null, 2)
      }]
    };
  }
);

const transport = new StdioServerTransport();
await server.connect(transport);
```

One server, one tool, and now Claude can look up customer data whenever you need it. You can see how easy it'd be to add more tools -- `query_orders`, `get_customer_by_id`, whatever your workflow needs.

---

### Example 2: Internal API MCP

Here's a more ambitious one. Say your company has an internal deployment API and you want Claude to be able to kick off deploys:

```javascript
server.tool(
  "deploy_service",
  {
    service: z.string().describe("Service name"),
    version: z.string().describe("Version to deploy")
  },
  async ({ service, version }) => {
    const response = await fetch(
      `https://internal-api.company.com/deploy`,
      {
        method: 'POST',
        headers: {
          'Authorization': `Bearer ${process.env.API_TOKEN}`,
          'Content-Type': 'application/json'
        },
        body: JSON.stringify({ service, version })
      }
    );

    const data = await response.json();
    return {
      content: [{
        type: "text",
        text: JSON.stringify(data, null, 2)
      }]
    };
  }
);
```

Now you can say "deploy the auth service version 2.3.1" and Claude handles the API call. Powerful stuff -- but make sure you've got proper guardrails in place before giving Claude access to production deployments. Which brings us to...

---

## Lesson 8: Best Practices

### Security

Fair warning: this is the one section where I won't be casual. Getting security wrong with MCP servers can bite you hard.

- Use environment variables for secrets. Never hardcode API keys or database passwords in your server code or config files.
- Validate all inputs. Zod schemas help, but think about what happens if someone -- or Claude -- passes unexpected values.
- Limit access to sensitive operations. Just because you *can* give Claude access to your production database doesn't mean you should give it write access.
- Use authentication and authorization. If your MCP server talks to internal APIs, make sure it uses proper auth tokens with minimal required permissions.
- Audit log everything. When Claude takes actions through your MCP server, you want a record of what happened and when.

---

### Error Handling

Things will go wrong -- APIs will be down, databases will time out, inputs will be weird. Don't let your server crash. Wrap your tool handlers in try/catch and return useful error messages so Claude can tell the user what went wrong:

```javascript
server.tool(
  "risky_operation",
  { target: z.string() },
  async ({ target }) => {
    try {
      const result = await doSomethingRisky(target);
      return {
        content: [{ type: "text", text: JSON.stringify(result) }]
      };
    } catch (error) {
      return {
        content: [{ type: "text", text: `Failed: ${error.message}` }],
        isError: true
      };
    }
  }
);
```

The `isError: true` flag tells Claude the tool call failed, so it can respond appropriately instead of trying to parse garbage output.

---

### Performance

Nobody wants Claude sitting there spinning for 30 seconds while your MCP server chokes. Keep things snappy:

- Cache frequently accessed data. If you're pulling the same reference table every time, cache it.
- Use connection pooling for databases. Don't open a new connection on every tool call.
- Implement timeouts. If an API call hangs, fail fast rather than blocking forever.
- Limit result sizes. Returning 10,000 rows to Claude doesn't help anyone. Paginate or summarize.

---

## Hands-On Practice

### Exercise 1: Install and Use MCP Servers

**Task:** Get your first MCP servers running

```
1. Install the filesystem MCP server by adding it to .mcp.json
2. Configure it for your projects directory
3. Use it to analyze your code
4. Install the Context7 MCP server
5. Ask Claude to look up docs for a library you use
```

This should take about 10 minutes and will immediately show you the value of MCP. If Context7 saves you from even one hallucinated API call, it's already paid for itself.

---

### Exercise 2: Create a Simple MCP Server

**Task:** Build your first custom MCP server

```
Create an MCP server that:
- Connects to a JSON file of notes
- Provides tools to:
  * List all notes
  * Search notes by keyword
  * Add new note
  * Delete note

Test it with Claude Code
```

This is a great starter project because it's simple enough to finish in one sitting but complex enough to teach you the full pattern -- setting up the server, registering multiple tools, handling reads and writes, and wiring it all into Claude Code.

---

### Exercise 3: Build a Practical MCP Server

**Task:** Create something you'll actually use

Pick one of these, or come up with your own:
- A todo list manager that connects to a file or database
- A git helper with shortcuts for common git operations
- A project template generator for spinning up new repos
- A code snippet library so Claude can pull from your team's patterns

The goal here isn't just practice -- it's to build something that makes your real workflow better. Pick the one that solves a problem you actually have.

---

## Module 11 Checklist

- [ ] Understand what MCP is
- [ ] Know how to configure MCP servers (`.mcp.json` and `~/.claude/settings.json`)
- [ ] Can install common MCP servers
- [ ] Know which MCP servers are essential daily drivers
- [ ] Can create custom MCP servers with the current SDK API
- [ ] Understand MCP security considerations
- [ ] Can use MCP to extend Claude Code

---

## Available MCP Servers

Official servers:
- @modelcontextprotocol/server-filesystem
- @modelcontextprotocol/server-postgres
- @modelcontextprotocol/server-sqlite
- @modelcontextprotocol/server-github
- @modelcontextprotocol/server-slack

Essential community servers:
- @context7/mcp-server -- live documentation
- @anthropic/mcp-server-playwright -- browser automation
- @anthropic/claude-in-chrome-mcp-server -- browser debugging
- @anthropic/mcp-server-deepwiki -- GitHub repo docs

You'll find many more on npm and GitHub -- search for "mcp-server" -- but remember, quality over quantity. Install what you need, not what looks cool on Twitter.

---

## What's Next?

You now know how to extend Claude Code with MCP servers, which means you're no longer limited to what Claude can do out of the box. You can connect it to any database, wire it up to any API, and build custom tools for your exact workflow.

Next up: Module 12 -- Customizing Claude Code. That one covers CLAUDE.md, Skills, Commands, Hooks, and Agents, which is where you really make Claude Code your own.
