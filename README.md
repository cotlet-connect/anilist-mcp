# AniList API and MCP server for ChatGPT and Claude

AniList has a public GraphQL API and no official MCP server according to the live Cotlet.dev page, checked 17 October 2026. Its API covers title searches, list entries, activity, favourites, follows and forum tasks. Cotlet.dev offers a hosted MCP connection to your own signed-in AniList account with 14 workflow tools.

## What you can ask

> Show my completed anime list, following every page of results.

> Find manga titles to add to my Planning list.

> Draft updates to my anime progress and scores for my approval.

> Read my notifications without marking them as read.

> Draft replies to activity mentions and ask me before publishing.

> Show my profile totals and highest-rated genres.

## Connect in 2 minutes

Get your API key at https://cotlet.dev/app and export it as `COTLET_API_KEY`.

### ChatGPT

Settings > Apps & Connectors > Create. Use `https://cotlet.dev/mcp/v1/connect` with OAuth.

### Claude

Settings > Connectors > Add custom connector. Use `https://cotlet.dev/mcp/v1/connect`.

### Claude Code

```bash
claude mcp add --transport http cotlet https://cotlet.dev/mcp/v1 --header "Authorization: Bearer $COTLET_API_KEY"
```

### Cursor

Copy `.cursor/mcp.json` from this repo into your project:

```json
{
  "mcpServers": {
    "cotlet": {
      "url": "https://cotlet.dev/mcp/v1",
      "headers": {
        "Authorization": "Bearer ${COTLET_API_KEY}"
      }
    }
  }
}
```

## Run it locally (stdio)

This repo is a real MCP server. It speaks MCP over stdio, lists its AniList tools without any key, and forwards every tool call to the hosted Cotlet.dev server. The hosted URL above stays the recommended way. Use this when your client only starts local servers.

Clone this repository and run `python3 anilist_agent.py serve` with `COTLET_API_KEY` set.

Or with Docker:

```bash
docker build -t anilist-mcp .
docker run -i --rm -e COTLET_API_KEY anilist-mcp
```

The local server exposes seven browser helper tools, distinct from the 14 hosted workflow tools described on the live page.

Local tools: `anilist_guide`, `anilist_status`, `anilist_open`, `anilist_read`, `anilist_click`, `anilist_type`, `anilist_close`. Without `COTLET_API_KEY` a tool call answers: "Set COTLET_API_KEY. Get a key at https://cotlet.dev/app".

## Command line

Python 3.10+, nothing else to install. Run from this repo:

```bash
export COTLET_API_KEY="your-key"
python3 anilist_agent.py connect   # ready config for Claude Code, Cursor, ChatGPT
python3 anilist_agent.py status    # is AniList ready in your Cotlet.dev account
python3 anilist_agent.py guide     # AniList screens and sign-in link
python3 anilist_agent.py read      # open AniList, read the page, close the browser
python3 anilist_agent.py tools     # list Cotlet.dev tools
python3 anilist_agent.py serve     # start the stdio MCP server (same as no argument)
```

## Questions from AniList users

**Does AniList have an API or official MCP server?**

The live Cotlet.dev page, checked 17 October 2026, describes a public GraphQL API and no official MCP server. Use AniList's own API first when it covers your task. Cotlet.dev offers an independent hosted MCP connection.

**Which account do I connect?**

Connect your own AniList account in its Cotlet Cloud Browser, sign in yourself and enable it for agents.

**Which actions ask for approval?**

Saving or deleting list entries, posting activity, replying, following users and posting forum comments ask for approval by default. Favourites can be toggled directly. Approval settings can be changed per tool.

**Do notifications become read automatically?**

No. The list_notifications operation resets the unread count only when mark_read is true; its default is false.

**Is this affiliated with AniList?**

No. Cotlet.dev is independent and is not affiliated with AniList.

## Live integration

[AniList tools and connection guide](https://cotlet.dev/integrations/anilist-api-mcp-server?utm_source=github&utm_medium=readme&utm_campaign=anilist)

---

More: https://cotlet.dev/integrations/anilist-api-mcp-server?utm_source=github&utm_medium=readme&utm_campaign=anilist

Start free: https://cotlet.dev/signup?app=anilist&utm_source=github&utm_medium=readme&utm_campaign=anilist
