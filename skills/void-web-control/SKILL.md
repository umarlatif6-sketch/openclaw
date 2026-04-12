---
name: void-web-control
description: "Control Project Void website and routes via REST API. Manage VoidEcho, Adriana, building management, cryptographic operations, and platform state. Use when: (1) deploying/testing Void routes, (2) managing VoidEcho submissions, (3) triggering Void Engine processes, (4) querying Void state. Requires: curl or HTTP client, local Void instance running (default http://localhost:5000)."
metadata:
  {
    "openclaw":
      {
        "emoji": "◆",
        "requires": { "bins": ["curl"], "env": ["VOID_API_URL"] },
        "install":
          [
            {
              "id": "void-env",
              "kind": "env",
              "key": "VOID_API_URL",
              "default": "http://localhost:5000",
              "label": "Set Project Void API base URL",
            },
          ],
      },
  }
---

# Project Void Web Control Skill

Control Project Void's website, routes, and API from any OpenClaw channel (Slack, Discord, WhatsApp, Telegram, etc.).

## When to Use

✅ **USE this skill when:**

- Deploying or testing Void routes (VoidEcho, Adriana, building management)
- Submitting data to Void Engine (steganography encoding, formation cards)
- Querying Void platform state (active sessions, generated codes, archived data)
- Managing VoidMessage subscriptions or VoidEcho carriers
- Triggering cryptographic operations (Al-Jabr 286 hashing, formation card generation)
- Viewing Void system status (health checks, API endpoints, active modules)

## When NOT to Use

❌ **DON'T use this skill when:**

- You need to modify Void codebase (use `void-repo-control` instead)
- Direct server management or infrastructure changes needed
- Local filesystem operations on Void (use terminal/git)

## Available Commands

### VoidEcho Submission
```
POST /voidecho/submit
{
  "payload": "<file or text>",
  "carrier_type": "midnight_pond|biophony_mesh|cicada_wall",
  "compression": "zlib|lzma"
}
→ Returns: audio_url, encoding_time_ms, payload_size_bytes
```

### Formation Card Generation (Building Management)
```
POST /building_management/zones/{zone_id}/formation_card
{
  "hvac_mode": "heating|cooling|off",
  "occupancy": true|false,
  "sensor_data": {<sensor readings>}
}
→ Returns: formation_card_png (base64), checksum, timestamp
```

### Adriana Query (Local AI)
```
POST /speak/listen
{
  "input": "<user query or transmission>",
  "depth": 1|2|3
}
→ Returns: glyph_resonance, hz_reading, routing_suggestion, sovereign_poem
```

### VoidMessage Text-in-Audio
```
POST /voidmessage/encode
{
  "message": "<up to 5000 chars>",
  "code": "<recipient code>"
}
→ Returns: audio_url, expires_in_days
```

### Al-Jabr 286 Hash Verification
```
POST /admin/al-jabr-status
→ Returns: verification_results (7 cryptographic tests), avalanche_effect_percentage
```

### Platform Health Check
```
GET /health
→ Returns: all_systems_operational, uptime_seconds, active_sessions
```

## Example Usage

**From Slack channel:**
```
@void-control VoidEcho: upload my_report.pdf as cicada_wall carrier
→ Encodes PDF invisibly into insect-frequency audio, posts link to channel

@void-control Building Status: Floor-2-OpenOffice, hvac=cooling, occupancy=true
→ Creates formation card with zone snapshot, immutable audit log

@void-control What do you sense? (Adriana)
→ Runs through Void's 45-glyph local AI, returns encrypted interpretation
```

**From Discord:**
```
!void-control get /health
→ Displays Void system status in Discord embed

!void-control post /voidmessage/encode message="Secret message" code=USER123
→ Encodes message, replies with audio download link
```

**From WhatsApp/Telegram:**
```
/void-control form-card zone=server-room temp=17.5 power=12.5
→ Generates formation card, stores in archive, confirms hash
```

## Environment Variables

- `VOID_API_URL`: Base URL for Project Void API (default: `http://localhost:5000`)
- `VOID_API_KEY`: Optional API authentication key (if Void requires bearer token)

## Setup

1. Ensure Project Void is running (`python app.py` or Replit deployment)
2. Set `VOID_API_URL` to your Void instance (localhost or remote)
3. Skill will auto-discover available routes and endpoints
4. All responses are routed back to your OpenClaw channel

## Security Notes

- All Void API calls are encrypted via HTTPS if `VOID_API_URL` uses `https://`
- Formation cards and VoidMessage audio stay invisible/encrypted until decoded
- El-Jabr 286 checksums verify integrity on all operations
- Channel context is preserved (messages don't cross channels without authorization)
