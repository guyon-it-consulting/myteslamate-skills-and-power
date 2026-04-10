# Tesla Skill for Kiro CLI & Claude Code

Control your Tesla vehicles and energy products (Powerwall, Solar) through natural language using the [MyTeslaMate](https://myteslamate.com) MCP server.

**100+ tools** — vehicle control, charging, energy management, navigation, security, analytics. No local scripts, no setup beyond auth.

## What is MyTeslaMate?

[MyTeslaMate](https://myteslamate.com) is the hosted version of TeslaMate, the most popular open-source data logger for Tesla vehicles. 
It continuously records every drive, charge, sleep cycle, and software update into a PostgreSQL database, and exposes rich Grafana dashboards — battery degradation, charging curves, trip history, lifetime stats, vampire drain, efficiency trends, and more. 
TeslaMate + Grafana in the cloud, no self-hosting required. 
It adds premium features like supercharger cost import, automations, fleet management, and an API/MQTT layer.

The MCP server at `https://mcp.myteslamate.com/mcp` wraps both the Tesla Fleet API (98 tools — vehicle commands, energy control) and the TeslaMate API (9 tools — drive stats, charging analytics, efficiency data) into a single endpoint that any MCP-compatible AI assistant can use. Authentication is handled via OAuth SSO with Tesla's authorization server — your credentials never touch the MCP server directly.

## What you can do

```
"What's my battery level?"
"Lock my car — I'm at the airport"
"Preheat the car at 7:45am"
"Set charge limit to 90%, road trip tomorrow"
"Should I charge now or wait for solar?"
"Set Powerwall reserve to 20%"
"How much did I spend on charging this month?"
"Road trip to Lyon — do I need a Supercharger stop?"
"Turn on Sentry mode"
```

## Install — Kiro IDE (Power)

In Kiro IDE (0.7+), open the Powers panel and select **Add power from GitHub**. Paste this repo URL:

```
https://github.com/guyon-it-consulting/myteslamate-skills-and-power
```

Kiro will auto-detect the `power/` directory, install the MCP server config, and load the steering instructions. On first use, follow the OAuth prompt to sign in with your Tesla account.

The power activates automatically when you mention Tesla-related keywords in conversation.

---

## Install — Kiro CLI

### 1. Copy the skill and agent config

```bash
# Clone the repo
git clone https://github.com/guyon-it-consulting/myteslamate-skills-and-power.git
cd tesla-kiro-skill

# Copy skill
cp -r kiro/skills/tesla-commands ~/.kiro/skills/

# Copy agent
cp kiro/agents/tesla.json ~/.kiro/agents/
```

### 2. Add the MCP server (one-time)

```bash
kiro-cli mcp add --name tesla-mcp --url "https://mcp.myteslamate.com/mcp" --scope global
```

### 3. Authenticate

Start a new session and run `/mcp`. The tesla-mcp server will show `⚠ auth-required`. Press Enter to copy the OAuth URL, open it in your browser, and sign in with your Tesla account.

### 4. Use it

```
/agent swap tesla
```

Or press `Ctrl+Shift+T` to toggle the Tesla agent. Then just talk naturally.

---

## Install — Claude Code

> ⚠️ **Untested** — This Claude Code integration has not been validated yet. Contributions and feedback welcome.

### 1. Register the MCP server

Add to `~/.claude/mcp.json`:

```json
{
  "mcpServers": {
    "tesla-mcp": {
      "type": "http",
      "url": "https://mcp.myteslamate.com/mcp",
      "headers": { "Authorization": "Bearer ${MTM_TOKEN}" }
    }
  }
}
```

### 2. Set your token

Get your API token from [MyTeslaMate](https://app.myteslamate.com/fleet), then:

```bash
export MTM_TOKEN=<your_token>
```

Add to `~/.zshrc` or `~/.bashrc` to persist.

### 3. Copy the slash command

```bash
cp claude-code/tesla.md ~/.claude/commands/tesla.md
```

### 4. Use it

```
/tesla what is my battery level?
/tesla lock my car
/tesla set the AC to 22°C
```

---

## Repo structure

```
tesla-kiro-skill/
├── README.md
├── LICENSE
├── power/                            # Kiro IDE Power
│   ├── POWER.md                      # Power definition (frontmatter + onboarding + steering)
│   └── mcp.json                      # MCP server config
├── kiro/                             # Kiro CLI
│   ├── skills/tesla-commands/
│   │   └── SKILL.md                  # Skill definition
│   └── agents/
│       └── tesla.json                # Agent config
└── claude-code/                      # Claude Code
    └── tesla.md                      # /tesla slash command
```

## Capabilities

| Category            | Tools | Examples                                                              |
|---------------------|-------|-----------------------------------------------------------------------|
| Vehicle Control     | 64    | Lock/unlock, climate, trunk, windows, lights, horn, media, navigation |
| Vehicle Data        | 29    | Status, battery, location, nearby chargers, alerts, release notes     |
| Energy / Powerwall  | 12    | Live solar/grid status, backup reserve, storm mode, time-of-use       |
| Charging History    | 3     | Session history, invoices, pricing data                               |
| TeslaMate Analytics | 9     | Drive stats, trip history, efficiency, energy costs                   |

## Auth options

| Method       | Best for                     | Setup                                          |
|--------------|------------------------------|------------------------------------------------|
| OAuth SSO    | Kiro CLI, Claude.ai, ChatGPT | Automatic — follow the auth prompt             |
| Manual token | Claude Code, VS Code, Cursor | Get token from myteslamate.com, set as env var |

## Powered by

- [MyTeslaMate MCP Server](https://github.com/MyTeslaMate/mcp-tesla) — 107 tools wrapping Tesla Fleet API + TeslaMate API

## License

MIT
