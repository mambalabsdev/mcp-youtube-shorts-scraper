# YouTube Shorts Scraper MCP Server

[![npm version](https://img.shields.io/npm/v/@mambalabsdev/mcp-youtube-shorts-scraper)](https://www.npmjs.com/package/@mambalabsdev/mcp-youtube-shorts-scraper) [![license](https://img.shields.io/github/license/mambalabsdev/mcp-youtube-shorts-scraper)](https://github.com/mambalabsdev/mcp-youtube-shorts-scraper/blob/main/LICENSE)

An MCP server for the Mamba Labs YouTube Shorts Scraper actor on Apify. YouTube Shorts as rows, with views, likes, and comments.

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

Takes YouTube channels, search terms, or Short links and returns one row per Short with exact views, likes, comment count, publish date, hashtags, and channel, plus a free report row per source.

All of the work runs on Apify. This package is a thin client that starts the actor run, waits for it, and hands back the dataset rows.

## Quick start

You need Node.js 18 or newer and an Apify account with an API token.

Add this to your Claude Desktop config (`claude_desktop_config.json`), Cursor, or any MCP client:

```json
{
  "mcpServers": {
    "mamba-youtube-shorts-scraper": {
      "command": "npx",
      "args": [
        "-y",
        "@mambalabsdev/mcp-youtube-shorts-scraper"
      ],
      "env": {
        "APIFY_TOKEN": "your-apify-token"
      }
    }
  }
}
```

Restart the client. The `get_youtube_shorts` tool appears.

## Prerequisites

- Node.js 18 or newer.
- An Apify API token from [console.apify.com/account/integrations](https://console.apify.com/account/integrations), set as `APIFY_TOKEN`. The server lists its tool without a token; a call needs one.

## Example prompts

- "Get the 20 most popular YouTube Shorts from @MrBeast with their like and comment counts."
- "Find YouTube Shorts about iced coffee recipes and list the ones with the most views."

## Tool

`get_youtube_shorts`. One tool. Example arguments:

```json
{
  "channels": [
    "@MrBeast"
  ],
  "max_shorts_per_source": 5
}
```

## Inputs

Every buyer facing input of the actor. Omitted inputs take the actor's own defaults.

| Input | Type | Required | Description |
|---|---|---|---|
| `channels` | array | no | Channel handles (@MrBeast or MrBeast), channel links (https://www.youtube.com/@MrBeast), or UC channel IDs. Each channel's Shorts tab is read. |
| `search_terms` | array | no | Words to search on YouTube with the Shorts filter, one term per line, for example iced coffee recipe. YouTube orders the results. |
| `shorts` | array | no | Single Shorts: links such as https://www.youtube.com/shorts/sHpEqeSByPI, watch links, youtu.be links, or 11 character video IDs. Each one gets a row with its details. |
| `max_shorts_per_source` | number | no | Shorts to return from each channel or search. YouTube shows about 48 a page. Default 30, maximum 1000. |
| `sort` | string | no | Order of a channel's Shorts tab: latest (default), popular (most viewed), or oldest. Searches keep YouTube's own order. |
| `details` | boolean | no | yes (default): read each Short for exact views, likes, comment count, and publish date, priced per Short with details. no: return only what the list shows (ID, title, rounded views, thumbnail) at the lower list price. Short links always get details. |
| `max_charge_usd` | number | no | Optional. The run stops before a charge that would pass this amount. Report rows are always returned. |

## Pricing

Pay per event on Apify, billed to your Apify account. Prices in USD.

| Event | Free plan | Bronze | Silver | Gold |
|---|---|---|---|---|
| `short-detail-row`, one Short with details | $0.002 | $0.0019 | $0.0018 | $0.0017 |
| `short-row`, one Short from the list only (details off) | $0.001 | $0.00095 | $0.0009 | $0.00085 |
| `apify-actor-start`, per run start | $0.004 | $0.004 | $0.004 | $0.004 |

Source reports, Shorts YouTube says are not available, refused reads, repeats, and refused input are free. Mamba Labs pays the platform costs of the run, Apify proxy traffic included.

## Full actor documentation

[apify.com/mambalabs/youtube-shorts-scraper](https://apify.com/mambalabs/youtube-shorts-scraper)

## License

MIT. See [LICENSE](LICENSE).
