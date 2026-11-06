<p align="center">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="https://raw.githubusercontent.com/aistrail/.github/main/profile/aistrail-logo-on-dark.png">
    <img alt="AIS Trail" src="https://raw.githubusercontent.com/aistrail/.github/main/profile/aistrail-logo-on-light.png" width="260">
  </picture>
</p>

# AIS Trail MCP Server

Ship data for AI assistants. The AIS Trail MCP server gives Claude, ChatGPT, Cursor and any other
[Model Context Protocol](https://modelcontextprotocol.io) client read-only access to live AIS positions, vessel history,
port calls, vessel events and sanctions list flags.

The server is hosted by AIS Trail, so there is nothing to install: point your client at the endpoint below. This
repository holds its public documentation.

[Website](https://aistrail.com) · [Documentation](https://aistrail.com/docs/mcp/) · [Live map](https://map.aistrail.com) · [Coverage](https://map.aistrail.com/coverage) · [Pricing](https://aistrail.com/pricing/)

## Server

| | |
|---|---|
| Endpoint | `https://api.aistrail.com/mcp` |
| Transport | Streamable HTTP, stateless (one JSON response per request) |
| Protocol versions | 2026-07-28, 2025-11-25, 2025-06-18, 2025-03-26 |
| Authentication | `Authorization: Bearer <API key>`, required only for the archive tools |
| Registry | `com.aistrail/ais` in the official MCP Registry; also on [Smithery](https://smithery.ai/servers/aistrail/ais) |

## Connect

**Claude Code**

```bash
claude mcp add --transport http aistrail https://api.aistrail.com/mcp \
  --header "Authorization: Bearer $AIS_TRAIL_KEY"
```

Run `/mcp` in Claude Code to confirm that `aistrail` is connected.

**Other clients.** Add a remote (Streamable HTTP) server with the same URL and header. For clients configured with JSON,
such as VS Code and Cursor:

```json
{
  "servers": {
    "aistrail": {
      "type": "http",
      "url": "https://api.aistrail.com/mcp",
      "headers": { "Authorization": "Bearer YOUR_AIS_TRAIL_KEY" }
    }
  }
}
```

Leave out the header to use the live tools only; clients that cannot send a header (for example connectors that offer
only OAuth) work the same way. API keys are created in the [AIS Trail dashboard](https://app.aistrail.com/signup), and
the Free plan includes one. Give each client its own key so it can be revoked separately.

## Example questions

Ask in plain words; the assistant picks the tools.

- "Which tankers are off Helsinki right now?"
- "Find the ship MYSTAR and show where it sailed three days ago."
- "Which ships called at Turku last week?"
- "Is the ship with IMO 1234567 on a sanctions list?"
- "What happened to MMSI 276859000 last week? Include AIS gaps and encounters."

## Tools

All tools are read-only.

| Tool | Answers | API key |
|---|---|---|
| `search_vessels` | Ships by name, MMSI or IMO number, live ships first | No |
| `get_vessel` | One ship now: identity, flag, size, position, speed, course, destination, ETA and its recent track | No |
| `vessels_in_area` | Ships heard in the last hour inside a box, largest first, optionally by ship type | No |
| `vessels_near` | Ships heard in the last hour near a point or another ship, nearest first | No |
| `find_port` | Ports by name, UN/LOCODE or country | No |
| `data_status` | Live ship count, archive coverage per source and service status | No |
| `vessel_track` | Where a ship has been between two times | Yes |
| `vessel_port_calls` | A ship's port calls with draught, cargo estimate and sanctions flags | Yes |
| `port_calls` | Arrivals and departures at one port, with the berth where the port's own records give it | Yes |
| `expected_arrivals` | Ships expected at one port, from the port's own records (Finnish ports; Pro and Business plans) | Yes |
| `vessel_events` | Port and anchorage calls, stops, identity and destination changes, AIS gaps and encounters | Yes |
| `lookup_vessels` | Records of up to 20 ships at once, with the sanctions lists that name them (OFAC SDN, UK, EU) | Yes |

Limits and parameters for each tool are in the [documentation](https://aistrail.com/docs/mcp/#tools).

## API key and credits

The live tools use no credits. The archive tools call the AIS Trail REST API with your key, so your plan's history
depth, credits and rate limits apply exactly as they do in the API:

| Tool | Credits |
|---|---|
| `vessel_track` | 1 per vessel-day |
| `vessel_port_calls`, `port_calls` | 1 per port call returned |
| `expected_arrivals` | 1 per ship returned |
| `vessel_events` | 1 per event returned |
| `lookup_vessels` | 1 per vessel found |

See [Pricing](https://aistrail.com/pricing/) for plans and the [REST API documentation](https://aistrail.com/docs/) for
the underlying endpoints.

## Data

- Positions are what ships broadcast over AIS; all times are UTC.
- Every answer with ship data names the sources to credit. Coverage and sources: [coverage map](https://map.aistrail.com/coverage),
  [data sources](https://aistrail.com/docs/sources/).
- Private vessels (pleasure craft, sailing vessels and Class B transponders without a valid IMO number) are never listed
  or looked up; area and nearby results only count them.
- Not for navigation.

## Support

Open an issue in this repository or write to [hello@aistrail.com](mailto:hello@aistrail.com).

Use of the server is subject to the [Terms of Service](https://aistrail.com/legal/terms/) and the
[Privacy Policy](https://aistrail.com/legal/privacy/).
