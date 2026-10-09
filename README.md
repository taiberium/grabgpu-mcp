# GrabGPU MCP server

Find a cloud GPU you can actually rent. GrabGPU checks 22 GPU clouds every day (RunPod, Vast.ai, Lium, the providers behind Shadeform and more) and records, for 71 GPU models, the price per GPU hour and whether the provider reports it in stock.

This repository documents the hosted MCP server. There is nothing to install or run: the server is remote.

- URL: `https://grabgpu.com/mcp`
- Transport: Streamable HTTP
- Auth: none (no account, no API key)
- Tool: `find_gpu` (read only)
- Website: https://grabgpu.com

## Add it to your client

Cline (`cline_mcp_settings.json`), Cursor (`~/.cursor/mcp.json`) and most other clients:

```json
{
  "mcpServers": {
    "grabgpu": {
      "type": "streamableHttp",
      "url": "https://grabgpu.com/mcp"
    }
  }
}
```

Claude Code:

```bash
claude mcp add --transport http grabgpu https://grabgpu.com/mcp
```

Claude and ChatGPT: add a custom connector with the URL `https://grabgpu.com/mcp`, or install GrabGPU from the Claude connector directory.

## The tool

`find_gpu` returns offers from the latest daily snapshot. Rentable offers come first, then cheapest.

| Argument | Type | Meaning |
|---|---|---|
| `gpu` | string | GPU model, e.g. `H100`, `B300`, `RTX 4090`. Omit for every model. |
| `in_stock` | boolean | Default `true`: only offers in stock. `false`: also offers out of stock or with unknown stock. |
| `min_gpus` | integer | Smallest node size, e.g. `8` for an 8x node. |
| `max_price` | number | Highest USD per GPU hour. |
| `limit` | integer | How many offers, default 20, at most 100. |

Each offer has `gpu_name`, `provider_name`, `usd_per_gpu_hour`, `availability` (`in_stock`, `unknown`, `out_of_stock`), `max_gpus`, `rent_url`, `direct_url` and `is_referral`.

Example prompts:

- "Where can I rent an H100 right now, and what is the cheapest in-stock offer?"
- "I need an 8x H100 node. Who has one in stock?"
- "Show me every GPU in stock under $1 an hour."

Check it from a terminal:

```bash
curl -s https://grabgpu.com/mcp -H 'content-type: application/json' \
  -d '{"jsonrpc":"2.0","id":1,"method":"tools/call","params":{"name":"find_gpu","arguments":{"gpu":"H100","limit":3}}}'
```

## Referral links and privacy

Some `rent_url` links are provider referral links: the provider may pay GrabGPU a commission, the price for you does not change, and the order is always by stock and price, never by commission. Each offer says whether its link is a referral (`is_referral`).

MCP calls are not stored. Privacy policy: https://grabgpu.com/privacy/

Support: info@grabgpu.com
