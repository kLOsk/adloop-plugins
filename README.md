# AdLoop Cloud plugins

Google Ads, Reddit Ads and Google Analytics in your AI assistant, with a preview and your approval before every change.

This repository packages [AdLoop Cloud](https://getadloop.com) as a plugin for Claude, Cursor, Gemini CLI, and ChatGPT and Codex. Each plugin does two things:

1. **Connects the AdLoop Cloud MCP server** at `https://mcp.getadloop.com/mcp`. It gives your assistant tools for Google Ads, Reddit Ads, Google Analytics 4, Search Console, Tag Manager and Merchant Center.
2. **Adds AdLoop's orchestration rules.** These rules tell the assistant which tool to use for which question and which checks to run before a change. They also cover how to read the data, for example why Ads clicks and GA4 sessions differ under cookie consent. The rules ship as a skill for Claude and Codex, as a rule for Cursor and as context for Gemini CLI.

The plugins contain no executable code. No hooks, no local servers, nothing that runs on your machine. The only network connection they make is to `https://mcp.getadloop.com/mcp`.

## What you need

- A free AdLoop Cloud account: sign up at [getadloop.com](https://getadloop.com).
- Your Google account connected in the AdLoop Cloud dashboard. Connecting Reddit Ads is optional.

The first time your assistant uses AdLoop, it opens a browser window. Sign in to AdLoop Cloud there and approve the connection. You don't paste an API key anywhere.

## Install

### Claude (claude.ai, Cowork, Claude Code)

Once the plugin is listed, add **AdLoop Cloud** from Claude's plugin directory.

To try it from source, clone this repository and start Claude Code with its `--plugin-dir` option pointing at the folder.

### Any other MCP client

Add `https://mcp.getadloop.com/mcp` as a custom connector or remote MCP server (Streamable HTTP). The server supports OAuth with dynamic client registration, so most clients only need the URL. For a client without OAuth support, create an API key under **Settings → API keys** in the dashboard and send it as a header:

```
Authorization: Bearer alc_YOUR_KEY
```

API keys can also be restricted to read-only access or to selected toolsets. See the [connection guides](https://docs.getadloop.com) for per-client instructions.

## Safety model

AdLoop enforces these rules on its server, so they apply even to an assistant that ignores its instructions:

- **Preview and approval.** Every change starts as a preview. Nothing is applied until you approve it.
- **Google validation test run.** Before a change is applied for real, Google checks it in a test run that changes nothing. A change Google would reject never reaches your account.
- **Budget and bid caps.** Budgets above the maximum daily budget you set in the dashboard are refused, and so are bid raises beyond the per-change limit.
- **New campaigns start paused.** New campaigns and ads are created paused. You enable them after you've reviewed them.

Every change, including test runs, is recorded in the change log in your dashboard. More in the [safety model](https://docs.getadloop.com/concepts/safety-model) docs.

## About this branch

This branch holds the Claude plugin only: the manifest, the MCP server entry and the skill with AdLoop's orchestration rules. It is built from the [main branch](https://github.com/kLOsk/adloop-plugins), which also packages AdLoop Cloud for Cursor, Gemini CLI, and ChatGPT and Codex.

## Links

- Website: [getadloop.com](https://getadloop.com)
- Documentation: [docs.getadloop.com](https://docs.getadloop.com)
- Privacy policy: [getadloop.com/privacy](https://getadloop.com/privacy)
- Terms of service: [getadloop.com/terms](https://getadloop.com/terms)
- Support: [hello@getadloop.com](mailto:hello@getadloop.com)

## License

[MIT](LICENSE)
