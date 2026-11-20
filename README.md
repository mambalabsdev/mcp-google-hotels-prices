# Google Hotels Prices MCP Server

[![MCP Registry](https://img.shields.io/badge/dynamic/json?url=https%3A%2F%2Fregistry.modelcontextprotocol.io%2Fv0%2Fservers%3Fsearch%3Dcom.mambabuilt%252Fmcp-google-hotels-prices%26limit%3D1&query=%24.servers%5B0%5D._meta%5B%22io.modelcontextprotocol.registry%2Fofficial%22%5D.status&label=mcp%20registry&color=blue)](https://registry.modelcontextprotocol.io/v0/servers?search=com.mambabuilt/mcp-google-hotels-prices&limit=1) [![npm version](https://img.shields.io/npm/v/@mambalabsdev/mcp-google-hotels-prices)](https://www.npmjs.com/package/@mambalabsdev/mcp-google-hotels-prices) [![npm downloads](https://img.shields.io/npm/dm/@mambalabsdev/mcp-google-hotels-prices)](https://www.npmjs.com/package/@mambalabsdev/mcp-google-hotels-prices) [![license](https://img.shields.io/github/license/mambalabsdev/mcp-google-hotels-prices)](https://github.com/mambalabsdev/mcp-google-hotels-prices/blob/main/LICENSE)

An MCP server for the Mamba Labs Google Hotels Prices actor on Apify. Google Hotels prices as rows.

## What's Inside

- [What it does](#what-it-does)
- [Quick start](#quick-start)
- [Prerequisites](#prerequisites)
- [Example prompts](#example-prompts)
- [Tool](#tool)
- [Inputs](#inputs)
- [Pricing](#pricing)
- [Full actor documentation](#full-actor-documentation)
- [License](#license)

## What it does

Takes destinations and stay dates and returns one row per property from Google Hotels with nightly price, stay total with taxes and fees, star class, rating, review count, deal label, and amenities.

All of the work runs on Apify. This package is a thin client that starts the actor run, waits for it, and hands back the dataset rows.

## Quick start

You need Node.js 18 or newer and an Apify account with an API token.

Add this to your Claude Desktop config (`claude_desktop_config.json`), Cursor, or any MCP client:

```json
{
  "mcpServers": {
    "mamba-google-hotels-prices": {
      "command": "npx",
      "args": [
        "-y",
        "@mambalabsdev/mcp-google-hotels-prices"
      ],
      "env": {
        "APIFY_TOKEN": "your-apify-token"
      }
    }
  }
}
```

Restart the client. The `get_google_hotels_prices` tool appears.

## Prerequisites

- Node.js 18 or newer.
- An Apify API token from [console.apify.com/account/integrations](https://console.apify.com/account/integrations), set as `APIFY_TOKEN`. The server lists its tool without a token; a call needs one.

## Example prompts

- "What do hotels in Lisbon cost for March 3 to March 6?"
- "Compare 4-star hotel prices in Miami for the next three weekends."

## Tool

`get_google_hotels_prices`. One tool. Example arguments:

```json
{
  "destinations": [
    "Lisbon"
  ],
  "check_in": [
    "2027-03-03"
  ],
  "nights": 3,
  "max_hotels_per_search": 10
}
```

## Inputs

Every buyer facing input of the actor. Omitted inputs take the actor's own defaults.

| Input | Type | Required | Description |
|---|---|---|---|
| `destinations` | array | no | Cities, areas, or landmarks, one per line, for example Miami, Paris 1st arrondissement, or near Times Square. The actor searches "hotels in <destination>". A destination that already says hotels, resorts, hostels, or vacation rentals is sent as written. From Clay, put the column inside a one item list. |
| `check_in` | array | no | One or more check in dates, YYYY-MM-DD, up to 330 days ahead. Each date is its own search. Empty: 14 days from today. |
| `nights` | number | no | Length of each stay, 1 to 30. Default 2. Ignored when check_out is set. |
| `check_out` | string | no | Optional. YYYY-MM-DD. Works with one check in date only; for several dates use nights. |
| `max_hotels_per_search` | number | no | 1 to 40. Default 20. Google Hotels shows about 18 to 20 properties on its first page, and this actor reads that page only. |
| `hotels_only` | boolean | no | true: keep only properties Google gives a star class, and drop vacation rentals and unclassified properties (free). Default false. |
| `priced_only` | boolean | no | true: leave out properties Google shows with no price for your dates. Default false, which returns them as free rows. |
| `dedupe` | boolean | no | true (default): a property already returned for the same dates in this run is skipped and not charged. |
| `max_charge_usd` | number | no | Optional. The run stops before a charge that would pass this amount. Summary rows are always returned. |

## Pricing

Pay per event on Apify, billed to your Apify account. Prices in USD.

| Event | Free plan | Bronze | Silver | Gold |
|---|---|---|---|---|
| `property-row`, one priced property | $0.002 | $0.0019 | $0.0018 | $0.0017 |
| `apify-actor-start`, per run start | $0.005 | $0.005 | $0.005 | $0.005 |

Properties with no price, repeats, summaries, and refused input are free. Mamba Labs pays the platform costs of the run, Apify proxy traffic included.

## Full actor documentation

[apify.com/mambalabs/google-hotels-prices](https://apify.com/mambalabs/google-hotels-prices)

## License

MIT. See [LICENSE](LICENSE).
