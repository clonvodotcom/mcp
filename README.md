# Clonvo MCP server

[Clonvo](https://clonvo.com)'s remote [Model Context Protocol](https://modelcontextprotocol.io) server gives agents web search, page reading, site crawling, structured extraction, research reports, a browser agent, and company and people lookups.

Calls run on your own Firecrawl, Tavily, Exa, Parallel or TinyFish keys when you've added them to Clonvo, otherwise on Clonvo credits. Every call is listed in Clonvo's Activity with its cost.

| Setting | Value |
| --- | --- |
| URL | `https://api.clonvo.com/mcp` |
| Transport | Streamable HTTP |
| Authentication | OAuth: add the URL and sign in. An API key also works, for agents and scripts. |

This repository holds the server's documentation and its [MCP Registry](https://registry.modelcontextprotocol.io) entry ([`server.json`](server.json)). The server itself is hosted by Clonvo; there's nothing to install or run.

## Before you connect

[Create a Clonvo account](https://clonvo.com) and buy credits once. That moves your organization to Pay as you go, which the tools need. To run tools on your own provider keys, add them under **Provider keys → Tools**.

## Connect with OAuth

Add `https://api.clonvo.com/mcp` to any app that supports remote MCP servers, then sign in to Clonvo when it asks. You choose the organization and project the app works in. There's no key to create or paste.

- **ChatGPT, Claude and other apps with custom connectors:** add a custom connector with the URL above.
- **Claude Code, Cursor, VS Code and Codex:** see the setup for each below.

An app connected this way can call the tools and nothing else. Its calls are billed to the organization you chose, and appear in Activity under the app's name. To see or disconnect your apps, go to **Settings → Connected apps** in Clonvo.

## Connect with an API key

For agents and scripts that run without a person to sign in, create a key under **API keys**, keep it in an environment variable, and send it as `Authorization: Bearer <key>`:

```bash
export CLONVO_API_KEY="sk-cv-v1-…"
```

## Tools

| Tool | What it does |
| --- | --- |
| `web_search` | Search the web for ranked pages with snippets. Narrow by category (news, companies, people, research papers, GitHub, PDFs, financial reports), domains or time range, and optionally get each page's content. |
| `web_scrape` | Read one page as clean markdown or plain text. |
| `web_map` | List a website's URLs, optionally ranked by a topic. |
| `web_extract` | Read one page and get typed JSON back, shaped by a JSON Schema you give. |
| `web_answer` | Get a short answer to a question from current web sources, with citations. |
| `find_similar` | Find pages similar to a URL: competitors, other coverage, related papers. |
| `company_search` | Find companies by what they do, their industry or the technology they use. |
| `company_enrich` | Look up one company's profile by its domain, LinkedIn URL or name. |
| `company_news` | Get recent news and announcements about a company. |
| `company_jobs` | List a company's open job postings. |
| `company_similar` | Find competitors and lookalikes of a company. |
| `people_search` | Find professional profiles by role, company, name or a description. |
| `people_enrich` | Look up one person's professional profile from their LinkedIn URL. |
| `web_crawl` | Read many pages of one website as markdown. Runs as a job. |
| `web_research` | Research a question across many sources and get a cited report, or data in a shape you give. Runs as a job. |
| `browser_task` | Have a browser agent work through a page to reach a goal you describe. Runs as a job. |
| `job_status` | Check a job and get its result once it's done. Free. |

Long-running tools return an MCP task to clients that support tasks. Other clients get the result directly when the job ends within about 25 seconds, otherwise a job id to check with `job_status`.

Prices for each tool are on [clonvo.com/docs/tools](https://clonvo.com/docs/tools#billing).

## Claude Code

```bash
claude mcp add --transport http clonvo https://api.clonvo.com/mcp
```

Then run `/mcp` in Claude Code and choose **Authenticate** for `clonvo`.

To use an API key instead, for example in a `.mcp.json` committed for your team where each person sets `CLONVO_API_KEY`:

```json
{
  "mcpServers": {
    "clonvo": {
      "type": "http",
      "url": "https://api.clonvo.com/mcp",
      "headers": {
        "Authorization": "Bearer ${CLONVO_API_KEY}"
      }
    }
  }
}
```

The [Clonvo skills](https://github.com/clonvodotcom/skills) plugin for Claude Code connects this server too, along with skills for building on the Clonvo API.

## Cursor

Add Clonvo to `.cursor/mcp.json` in your project, or to `~/.cursor/mcp.json` for every project. Cursor asks you to sign in when it first connects:

```json
{
  "mcpServers": {
    "clonvo": {
      "url": "https://api.clonvo.com/mcp"
    }
  }
}
```

To use an API key instead, add `"headers": { "Authorization": "Bearer ${env:CLONVO_API_KEY}" }`.

## VS Code

Add Clonvo to `.vscode/mcp.json`. VS Code asks you to sign in when it first connects:

```json
{
  "servers": {
    "clonvo": {
      "type": "http",
      "url": "https://api.clonvo.com/mcp"
    }
  }
}
```

## Codex

Add Clonvo and sign in:

```bash
codex mcp add clonvo --url https://api.clonvo.com/mcp
codex mcp login clonvo
```

To use an API key instead, add `bearer_token_env_var = "CLONVO_API_KEY"` under `[mcp_servers.clonvo]` in `~/.codex/config.toml`.

## Other clients

Any client that supports remote MCP servers over Streamable HTTP can connect: by signing in with OAuth, or with the header `Authorization: Bearer <your Clonvo API key>`.

Clonvo's [MCP docs](https://clonvo.com/docs/mcp) cover the OpenAI Agents SDK, the Vercel AI SDK, OAuth details, routing between providers, and what a call returns.

## Support

Email [support@clonvo.com](mailto:support@clonvo.com), or open an issue in this repository.

## License

[MIT](LICENSE)
