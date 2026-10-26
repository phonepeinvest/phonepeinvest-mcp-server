# PhonePe Invest MCP Server

Connect your PhonePe Invest account to AI assistants such as Claude, Cursor and VS Code using the [Model Context Protocol (MCP)](https://modelcontextprotocol.io).

> **Read-only.** The assistant can view your holdings and market data. It **cannot** place, modify or cancel orders, or move funds.

## Quick start

Add this server URL to your MCP client:

```
https://mcp.phonepeinvest.com/mcp
```

The first time you connect, your browser opens the PhonePe Invest login page. Log in with your phone number and OTP. You don't need an API key, and your password is never shared with the AI app.

## Setup

### Claude Code

```bash
claude mcp add --transport http phonepe-invest https://mcp.phonepeinvest.com/mcp
```

Then run `/mcp` in Claude Code and choose **Authenticate**.

### Cursor

Add to `~/.cursor/mcp.json`:

```json
{
  "mcpServers": {
    "phonepe-invest": {
      "command": "npx",
      "args": ["-y", "mcp-remote", "https://mcp.phonepeinvest.com/mcp", "--transport", "http-only"]
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
      "type": "stdio",
      "command": "npx",
      "args": ["-y", "mcp-remote", "https://mcp.phonepeinvest.com/mcp", "--transport", "http-only"]
    }
  }
}
```

### Claude Desktop and other clients

Use [`mcp-remote`](https://www.npmjs.com/package/mcp-remote) (requires Node.js 18+):

```json
{
  "mcpServers": {
    "phonepe-invest": {
      "command": "npx",
      "args": ["-y", "mcp-remote", "https://mcp.phonepeinvest.com/mcp", "--transport", "http-only"]
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
- To stop access, see [Stop access](#stop-access).

## Troubleshooting

**Your app keeps waiting or times out right after you log in (first time only)**

Restart the MCP server in your app, or restart the app. Your login is already saved, so it connects straight away. This comes from a known issue in `mcp-remote` during the first login.

**The login page keeps opening again after you log in**

Close your AI app (or press `Ctrl+C` in the terminal). Repeated logins can temporarily block your account. [Reset your login](#reset-your-login) and try once more.

**The tools don't appear**

Restart your AI app after adding the server, and check that the server is enabled in your app's MCP settings.

**The login page doesn't open**

Copy the login link printed in your app's MCP logs and open it in your browser.

**claude.ai or ChatGPT web**

Web connectors are not supported yet. Use a desktop or command-line app.

## Reset your login

Use this if login keeps failing, or to remove PhonePe Invest access from your computer.

1. Close your AI app.
2. Remove the saved login.

   **Cursor, VS Code, Claude Desktop and other `mcp-remote` setups**

   ```bash
   node -e "const c=require('crypto'),fs=require('fs'),p=require('path'),os=require('os');const h=c.createHash('md5').update(process.argv[1]).digest('hex');const d=p.join(os.homedir(),'.mcp-auth');let n=0;if(fs.existsSync(d))for(const v of fs.readdirSync(d)){const vd=p.join(d,v);if(!fs.statSync(vd).isDirectory())continue;for(const f of fs.readdirSync(vd))if(f.startsWith(h+'_')){fs.unlinkSync(p.join(vd,f));n++}}console.log('Removed '+n+' PhonePe Invest login file(s)')" "https://mcp.phonepeinvest.com/mcp"
   ```

   This removes only the PhonePe Invest login. Other MCP servers are not affected. To remove **all** saved MCP logins instead, run `rm -rf ~/.mcp-auth`.

   **Claude Code:** run `/mcp`, select `phonepe-invest` and choose **Clear authentication**.

3. Open your app again and log in.

## Stop access

- Remove the server from your AI app's MCP settings, then [reset your login](#reset-your-login).
- Access also ends automatically within 24 hours. After that, you need to log in again.

## Your data

- Holdings data is shared only with the AI app you connect, and only when you ask it a question that needs it.
- The AI app you choose processes this data under its own terms and privacy policy.
- Market data (prices, open interest) is public information.

See the PhonePe Invest [Terms and Conditions](https://www.phonepeinvest.com/terms-conditions).
