<div align="center">

![Module 11](https://img.shields.io/badge/Module_11-6A0DAD?style=for-the-badge&labelColor=1a1a2e)
![Time](https://img.shields.io/badge/⏱_45_min-555555?style=for-the-badge&labelColor=1a1a2e)
![Difficulty](https://img.shields.io/badge/Advanced-FF6B35?style=for-the-badge&labelColor=1a1a2e)

# MCP Servers - Extending Claude Code

**Learn to use Model Context Protocol servers to give Claude Code new capabilities**

[← Previous](module-10-workflow-best-practices.md) · [🏠 Home](README.md) · [Next →](module-12-skills-and-hooks.md)

</div>

---

## What You'll Learn

MCP is how Claude Code goes from a general-purpose coding assistant to something shaped around *your* stack, *your* tools, *your* workflow. What MCP actually is, how to install and configure servers, which ones are worth your time (and which aren't), and how to build your own from scratch.

---

## Lesson 1: Understanding MCP

### What is MCP?

**Model Context Protocol (MCP)** is a standard way to give Claude Code access to external data and services. Claude Code is already good at a lot out of the box - MCP is how you make it great at *your* specific things.

Think of MCP servers as plugins. Each gives Claude a new capability - querying your database, pulling live docs, driving a browser. They all speak the same protocol, so once you understand one, you understand all of them.

---

### Why Use MCP?

Without MCP you're limited to Claude Code's built-in tools. Fine for general work - but the moment you need to hit your company's database, check a deployment, or pull docs for some niche library, you're back to copy-pasting context by hand.

With MCP, Claude reaches straight into your databases, talks to your APIs, pulls live documentation, and plugs into tools like GitHub and Notion - no middleman. Start using MCP servers and working without them starts to feel painful.

---

## Lesson 2: Installing MCP Servers

### Configuration Files

MCP servers live in simple JSON config files. Two places they can go:

Project-level (shared with your team via version control):
```text
.mcp.json    # in your project root
```

User-level (your personal global config):
```text
~/.claude/settings.json    # under the "mcpServers" key
```

Most of the time you want project-level config, so everyone on the team gets the same servers when they open the project. Use user-level for servers you want everywhere, no matter which project you're in.

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

Easiest way to start is to just ask Claude Code:

```text
Help me install the filesystem MCP server.
I want to give you access to my /home/user/projects directory.
```

Claude Code shows you exactly what config to add, helps you edit the right file, and checks it's wired up correctly. No fumbling through docs on your own.

---

### MCP Server Settings

A couple of settings worth knowing:

`enableAllProjectMcpServers` -- set this to `true` in your user settings if you're tired of approving project MCP servers every time. Only do this if you trust the projects you work on.

You can also allowlist or blocklist specific servers, so you've got fine-grained control over which ones are allowed to run. Handy when your team drops servers into `.mcp.json` but you don't want all of them active on your machine.

---

## Lesson 3: Common MCP Servers

The servers you'll see mentioned most. These are the "official" ones from the MCP project itself.

### Filesystem MCP Server

Gives Claude direct access to files and directories outside the current project. Useful when you need Claude working across multiple directories or touching files that aren't in your repo.

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

```text
Using the filesystem MCP server, list all Python files in my projects directory
```

---

### PostgreSQL MCP Server

If your app talks to Postgres, this server lets Claude query it directly. No more dumping query results into the chat - Claude just goes and looks.

```json
{
  "mcpServers": {
    "postgres": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-postgres", "postgresql://user:pass@localhost:5432/dbname"]
    }
  }
}
```

```text
Using the postgres MCP server, show me all tables in the database
Query the users table and show me the schema
```

---

### SQLite MCP Server

Same idea as the Postgres server, for SQLite. Great for local dev databases or smaller projects where Postgres would be overkill.

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

```text
Using the sqlite MCP server, analyze the database schema
Find all tables with user data
```

---

### GitHub MCP Server

Connects Claude to the GitHub API. Listing repos, reading issues, creating PRs - all without leaving your terminal.

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

```text
Using the GitHub MCP server:
- List my repositories
- Show open issues in my-repo
- Create an issue for the bug I just found
```

---

## Lesson 4: The MCP Servers That Actually Matter

There are hundreds of MCP servers floating around npm and GitHub. Most you'll never need. Don't burn time installing every shiny one you see on social media - focus on the ones that solve real, daily problems.

### Context7 - Live Documentation Lookup

This one matters more than it sounds. Context7 pulls live, version-specific documentation so Claude stops hallucinating API signatures. You know that moment when Claude confidently hands you a function call that doesn't exist in your version? Context7 fixes that.

```json
{
  "mcpServers": {
    "context7": {
      "command": "npx",
      "args": ["-y", "@upstash/context7-mcp"]
    }
  }
}
```

Install it first, thank me later.

---

### Playwright - Browser Automation

Need to test a web app? Scrape a page? Confirm your frontend changes actually work? Playwright gives Claude a real browser to drive. It can click buttons, fill forms, take screenshots, and assert on page content.

```json
{
  "mcpServers": {
    "playwright": {
      "command": "npx",
      "args": ["-y", "@playwright/mcp"]
    }
  }
}
```

If you do any frontend work at all, put this one on the short list.

---

### Claude in Chrome - Real Browser Debugging

Different from Playwright, and most people skip it when they shouldn't. Instead of automating a headless browser, Claude in Chrome gives Claude your *actual* browser context - the page you're looking at right now, the console errors, the network requests. When you're debugging a frontend issue and you want Claude to see exactly what you see, this is the tool.

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

> **Note:** MCP server package names change frequently. Check the [Claude Code docs](https://docs.anthropic.com/en/docs/claude-code/mcp) for current package names.

---

### DeepWiki - GitHub Repo Documentation

Ever needed to understand how some open-source library works under the hood? DeepWiki gets you documentation for any GitHub repo without leaving your session. Instead of tab-switching through READMEs, just ask Claude and let DeepWiki fetch the details.

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

Resist the urge to install 20 servers at once. Each one adds startup time and mental overhead. Start with Context7 - accurate docs beat everything - add Playwright or Chrome when you're doing frontend work, and pull in others as you actually need them. Three well-chosen servers beat fifteen that just sit there.

---

## Lesson 5: Creating a Custom MCP Server

### Why Create Custom Servers?

The pre-built servers cover a lot, but eventually you hit something specific to your world - your company's internal APIs, a custom database, some domain workflow no open-source server is going to handle. That's when you build your own.

Good news: it's not hard. An MCP server is just a small program that speaks the MCP protocol over standard I/O. If you can write a Node script, you can write an MCP server.

---

### MCP Server Basics

Every MCP server can provide three things:

- **Tools** -- functions Claude can call. The one you'll use most.
- **Resources** -- data Claude can read on demand.
- **Prompts** -- predefined prompt templates for common workflows.

You don't have to implement all three. Most custom servers start with a tool or two and grow from there.

---

### Creating a Simple MCP Server

A real example. Say you want Claude to check the weather. Here's the whole server:

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
    // Using OpenWeatherMap as an example -- sign up at openweathermap.org for a free API key
    const response = await fetch(
      `https://api.openweathermap.org/data/2.5/weather?q=${encodeURIComponent(city)}&appid=${process.env.WEATHER_API_KEY}`
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

Notice the pattern: create a `McpServer`, register tools on it with `server.tool()`, set up a `StdioServerTransport`, connect. That's the whole skeleton. Everything else is just your business logic inside the tool handler.

---

### Configuring Your Custom Server

Once the server file exists, point Claude Code at it in your `.mcp.json`:

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

```text
Using the weather MCP server, what's the weather in San Francisco?
```

Claude sees the tool, calls it, gets the result, answers. No special syntax.

---

## Lesson 6: Advanced MCP Features

### Resources

Resources let you expose data Claude can pull in on demand - read-only endpoints, basically. Register one with `server.resource()`:

```javascript
server.resource(
  "employees",
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

Great for data that doesn't change often - employee directories, config docs, reference tables. Claude pulls it in when it needs it, without you pasting it into the conversation.

---

### Prompts

Prompts are predefined templates that standardize how Claude approaches a task. If your team has a specific code review process, bake it right into an MCP server:

```javascript
server.prompt(
  "code_review",
  "Review code against company standards",
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

Now everyone on the team gets the same review criteria, and nobody has to remember the full prompt each time.

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

One server, one tool, and now Claude can look up customer data whenever you need it. You can see how easy it'd be to add more - `query_orders`, `get_customer_by_id`, whatever your workflow needs.

---

### Example 2: Internal API MCP

A more ambitious one. Say your company has an internal deployment API and you want Claude to kick off deploys:

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

Now you can say "deploy the auth service version 2.3.1" and Claude handles the API call. Powerful - but get proper guardrails in place before you hand Claude the keys to production deploys. Which brings us to...

---

## Lesson 8: Best Practices

### Security

Fair warning: this is the one section I won't be casual about. Getting security wrong with MCP servers can hurt.

- Use environment variables for secrets. Never hardcode API keys or database passwords in your server code or config files.
- Validate all inputs. Zod schemas help, but think about what happens if someone - or Claude - passes an unexpected value.
- Limit access to sensitive operations. Just because you *can* give Claude write access to production doesn't mean you should.
- Use authentication and authorization. If your server talks to internal APIs, use proper auth tokens with minimal permissions.
- Audit log everything. When Claude takes action through your server, you want a record of what happened and when.

---

### Error Handling

Things will go wrong - APIs go down, databases time out, inputs get weird. Don't let your server crash. Wrap tool handlers in try/catch and return useful error messages so Claude can tell the user what happened:

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

The `isError: true` flag tells Claude the call failed, so it responds sensibly instead of trying to parse garbage output.

---

### Performance

Nobody wants Claude spinning for 30 seconds while your server chokes. Keep it snappy:

- Cache frequently accessed data. Pulling the same reference table every time? Cache it.
- Use connection pooling for databases. Don't open a fresh connection on every tool call.
- Implement timeouts. If an API call hangs, fail fast instead of blocking forever.
- Limit result sizes. Returning 10,000 rows to Claude helps no one. Paginate or summarize.

---

## Hands-On Practice

### Exercise 1: Install and Use MCP Servers

**Task:** Get your first MCP servers running

```text
1. Install the filesystem MCP server by adding it to .mcp.json
2. Configure it for your projects directory
3. Use it to analyze your code
4. Install the Context7 MCP server
5. Ask Claude to look up docs for a library you use
```

About 10 minutes, and it shows you the value of MCP immediately. If Context7 saves you from even one hallucinated API call, it's already paid for itself.

---

### Exercise 2: Create a Simple MCP Server

**Task:** Build your first custom MCP server

```text
Create an MCP server that:
- Connects to a JSON file of notes
- Provides tools to:
  * List all notes
  * Search notes by keyword
  * Add new note
  * Delete note

Test it with Claude Code
```

A great starter because it's simple enough to finish in one sitting but complete enough to teach the full pattern - setting up the server, registering multiple tools, handling reads and writes, and wiring it into Claude Code.

---

### Exercise 3: Build a Practical MCP Server

**Task:** Create something you'll actually use

Pick one, or invent your own:
- A todo list manager that connects to a file or database
- A git helper with shortcuts for common git operations
- A project template generator for spinning up new repos
- A code snippet library so Claude can pull from your team's patterns

The goal isn't just practice - it's to build something that makes your real workflow better. Pick the one that solves a problem you actually have.

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
- @upstash/context7-mcp -- live documentation
- @playwright/mcp -- browser automation
- @anthropic/claude-in-chrome-mcp-server -- browser debugging
- @anthropic/mcp-server-deepwiki -- GitHub repo docs

You'll find many more on npm and GitHub - search "mcp-server" - but remember: quality over quantity. Install what you need, not what looks cool on Twitter.

---

## What's Next?

You now know how to extend Claude Code with MCP servers, which means you're no longer boxed into what Claude does out of the box. Connect it to any database, wire it to any API, build custom tools for your exact workflow.

Next up: Module 12 -- Customizing Claude Code. CLAUDE.md, Skills, Commands, Hooks, and Agents - where you really make Claude Code your own.

> **Ready to build production-grade MCP servers?** The [Advanced Modules](https://payhip.com/b/8E107) dedicate an entire module (Module 18) to custom MCP server development -- protocol internals, testing, performance tuning, and publishing to npm.

---

<div align="center">

[← Previous Module](module-10-workflow-best-practices.md) · [🏠 Home](README.md) · [Next Module →](module-12-skills-and-hooks.md)

</div>
