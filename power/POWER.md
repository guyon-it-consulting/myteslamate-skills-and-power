---
name: "tesla"
displayName: "Tesla Vehicle & Energy Control"
description: "Control Tesla vehicles and energy products via MyTeslaMate MCP server — lock/unlock, climate, charging, Powerwall, solar, Sentry mode, navigation, drive stats, and more"
keywords: ["tesla", "ev", "car", "vehicle", "charging", "powerwall", "solar", "battery", "sentry", "climate", "supercharger", "road trip", "energy", "lock", "unlock", "drive"]
---

# Onboarding

## Step 1: Authenticate with Tesla

The `tesla-mcp` MCP server uses OAuth SSO via Tesla's authorization server. On first use, Kiro will prompt you to authenticate:

1. Check MCP status — the server will show `⚠ auth-required`
2. Follow the OAuth URL to sign in with your Tesla account
3. Authorize the MyTeslaMate app
4. The server initializes automatically after auth completes

> **Note:** OAuth requires a MyTeslaMate account. The server validates your Tesla token and links it to your account automatically. Your Tesla credentials never touch the MCP server.

## Step 2: Verify connection

After authentication, confirm the server is connected by asking:

- "List my Tesla vehicles"
- "What's my battery level?"

If the server shows tools available, you're ready.

# Steering

The `tesla-mcp` server exposes **107 tools** (98 Tesla Fleet API + 9 TeslaMate API). No local scripts needed — all commands go through the remote MCP server.

## Capabilities

### Vehicle Control (64 commands)
- **Doors & Access**: lock, unlock, open/close trunk, open frunk
- **Climate**: start/stop HVAC, set temps, bioweapon mode, cabin overheat protection, seat heaters/coolers, steering wheel heater
- **Charging**: start/stop charge, open/close charge port, set charge limit, set amps, schedule charging, add/remove charge schedules
- **Windows & Sunroof**: open/close windows, vent/close sunroof
- **Lights & Horn**: flash lights, honk horn
- **Media**: volume, play/pause, next/prev track
- **Navigation**: send destination, send GPS coords, navigate to Supercharger
- **Security**: Sentry mode, valet mode, speed limit, PIN to drive
- **Software**: schedule/cancel software updates
- **Misc**: remote start, trigger HomeLink, boombox, set vehicle name

### Vehicle Data (29 endpoints)
- List vehicles, get vehicle metadata, live vehicle data
- Wake vehicle, fleet status, mobile enabled check
- Nearby charging sites, service data, release notes, recent alerts
- Vehicle options, eligible upgrades/subscriptions
- Driver management, share invitations
- Fleet telemetry config

### Energy / Powerwall / Solar (12 endpoints)
- List energy products, site info, live status (real-time power data)
- Energy history, backup history, charge history
- Set operation mode (autonomous/self_consumption), set backup reserve %
- Storm mode, grid import/export settings, time-of-use settings

### Charging History (3 endpoints)
- Charging history with filters, download invoices, session pricing data

### TeslaMate Analytics (9 tools)
- Drive statistics, trip history, efficiency data
- Charging session analytics, energy cost analysis

## Workflow

1. Use `@tesla-mcp` tools directly for all Tesla operations.
2. Check current state before making changes (e.g. get vehicle_data before toggling climate).
3. Wake the vehicle before sending action commands if the car is asleep.
4. Present data in a human-readable format with units (%, °C/°F, km/mi).
5. For energy optimization, cross-reference solar production, Powerwall state, and vehicle battery level.

## Safety

- Confirm with the user before executing security-sensitive commands: unlock, disable Sentry, remote start, erase data.
- Always show current state before making changes.
- If a command fails, show the error and suggest next steps.
