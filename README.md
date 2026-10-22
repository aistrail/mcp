# AIS Trail MCP server

Ship data for AI assistants: live AIS positions, vessel history, port calls, vessel events and sanctions flags. A hosted, read-only MCP server; there is nothing to install.

- **Endpoint:** `https://api.aistrail.com/mcp` (Streamable HTTP)
- **Registry name:** `com.aistrail/ais`
- **Docs:** https://aistrail.com/docs/mcp/

This repository holds the documentation for the hosted server. The server runs at AIS Trail.

## Connect

The live tools work without a key. The archive tools need an AIS Trail API key, sent as `Authorization: Bearer <key>`. You can get one at https://aistrail.com (the Free plan includes one).

Claude Code:

```bash
claude mcp add --transport http aistrail https://api.aistrail.com/mcp \
  --header "Authorization: Bearer $AIS_TRAIL_KEY"
```

Clients that take a JSON config (VS Code, Cursor and others):

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

Leave out `headers` to use only the live tools.

## Tools

| Tool | What it answers | Key |
|---|---|---|
| `search_vessels` | Find ships by name, MMSI or IMO number | No |
| `get_vessel` | One ship now: identity, flag, size, position, speed, course, destination and ETA | No |
| `vessels_in_area` | Ships heard in the last hour inside a box | No |
| `vessels_near` | Ships heard in the last hour near a point or another ship | No |
| `find_port` | Ports by name, UN/LOCODE or country | No |
| `data_status` | Live ship count, archive coverage and data freshness | No |
| `vessel_track` | Where a ship has been between two times | Yes |
| `vessel_port_calls` | A ship's port calls with draught and cargo estimate | Yes |
| `port_calls` | Arrivals and departures at one port | Yes |
| `vessel_events` | Port arrivals and departures, stops, changes of name, IMO, destination or flag, AIS gaps and close encounters | Yes |
| `lookup_vessels` | Records of up to 20 ships at once, with sanctions list flags (OFAC, UK, EU) | Yes |

Archive tools use the credits of your AIS Trail plan, the same way as the REST API. See the [docs](https://aistrail.com/docs/mcp/) for the details.

## Data

- **Coverage:** [coverage map](https://map.aistrail.com/coverage) and [data sources](https://aistrail.com/docs/sources/).
- **REST API:** https://aistrail.com/docs/
- Not for navigation.

## Support

Open an issue in this repository or write to hello@aistrail.com.
