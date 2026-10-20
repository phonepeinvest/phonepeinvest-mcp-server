# PhonePe Invest MCP Server

Connect your PhonePe Invest account to AI assistants such as Claude, Cursor and VS Code using the [Model Context Protocol (MCP)](https://modelcontextprotocol.io).

> **Read-only.** The assistant can view your holdings and market data. It **cannot** place, modify or cancel orders, or move funds.

## Quick start

Add this server URL to any MCP client that supports remote servers:

```
mcp.phonepeinvest.com/mcp
```

The first time you connect, your browser opens the PhonePe Invest login page. Log in with your phone number and OTP. You don't need an API key, and your password is never shared with the AI app.

## Setup

### Claude Code

```bash
claude mcp add --transport http phonepe-invest mcp.phonepeinvest.com/mcp
```

Then run `/mcp` in Claude Code and choose **Authenticate**.

### Cursor

Add to `~/.cursor/mcp.json`:

```json
{
  "mcpServers": {
    "phonepe-invest": {
      "url": "mcp.phonepeinvest.com/mcp"
    }
  }
}
```

### VS Code

Add to `.vscode/mcp.json`:

```json
{
  "servers": {
    "phonepe-invest": {
      "type": "http",
      "url": "mcp.phonepeinvest.com/mcp"
    }
  }
}
```

### Other clients (via `mcp-remote`)

For clients that only support local (stdio) servers, such as Claude Desktop, use [`mcp-remote`](https://www.npmjs.com/package/mcp-remote) (requires Node.js 18+):

```json
{
  "mcpServers": {
    "phonepe-invest": {
      "command": "npx",
      "args": ["-y", "mcp-remote", "mcp.phonepeinvest.com/mcp"]
    }
  }
}
```

## Available tools

| Tool | What it does |
|---|---|
| `get_holdings` | Your current holdings with latest prices |
| `search-instruments` | Find a stock, ETF, index, future or option by name or symbol |
| `get_ohlcv` | Price candles (open, high, low, close, volume) for a time range |
| `get_closing_prices` | Closing prices for a time range |
| `get_open_interest` | Current and historical open interest for futures and options |
| `analyze_open_interest` | Open interest summary, levels and trend |

## Example prompts

- "Show my holdings and today's change"
- "Which of my holdings are down more than 10%?"
- "Get daily candles for RELIANCE for the last 6 months"
- "Compare the closing prices of TCS and INFY over the last year"
- "What is the open interest trend for NIFTY this expiry?"

## Login and access

- You log in on the official PhonePe Invest login page. The AI app never sees your phone number, OTP or PIN.
- Access lasts up to **24 hours**. After that, you will be asked to log in again.
- To stop access, remove the server from your AI app.

## Your data

- Holdings data is shared only with the AI app you connect, and only when you ask it a question that needs it.
- The AI app you choose processes this data under its own terms and privacy policy.
- Market data (prices, open interest) is public information.

See the PhonePe Invest [Terms and Conditions](<https://www.phonepeinvest.com/terms-conditions>)
