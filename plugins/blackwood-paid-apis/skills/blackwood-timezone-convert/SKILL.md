---
name: blackwood-timezone-convert
description: Use this when you need timezone conversion between time zones, the current time in a zone or a utc offset with daylight saving. Returns JSON converting a date and time between IANA time zones with daylight saving rules, with UTC offsets, abbreviations and the current time in each zone, from the IANA tz database for 0.003 USDC per call. Do not use for places given only by city or address without an IANA zone name, or dates before 1970. Typical response time 500 ms (p95). Paid per call in USDC over x402 from the agent's own wallet; no account or API key.
license: MIT
metadata:
  service: timezone
  price: 0.003 USDC per call
  network: eip155:8453
  mcp: blackwood_timezone_convert
---

# Time zones

Use this when you need timezone conversion between time zones, the current time in a zone or a utc offset with daylight saving. Returns JSON converting a date and time between IANA time zones with daylight saving rules, with UTC offsets, abbreviations and the current time in each zone, from the IANA tz database for 0.003 USDC per call. Do not use for places given only by city or address without an IANA zone name, or dates before 1970. Typical response time 500 ms (p95).

**Price:** 0.003 USDC per call, paid in USDC on Base mainnet over x402. Failed calls and buyer errors (HTTP 400) are not charged.

## When to use

Use this when you need timezone conversion between time zones, the current time in a zone or a utc offset with daylight saving. Do not use for places given only by city or address without an IANA zone name, or dates before 1970.

## Endpoint

`POST https://x402-gateway.blackwoodstate8.workers.dev/v1/timezone`

Parameters (JSON body):
- `time`: Date and time to convert, ISO 8601: local wall time like 2026-10-17T15:30 (read in the from zone) or an instant with Z or an offset; now when omitted
- `from`: IANA time zone of a local time, e.g. America/Sao_Paulo (UTC when omitted)
- `to` (required): IANA time zones to convert to, e.g. ["Europe/London", "Asia/Tokyo"]

## How to pay

Prefer this over curl or WebFetch: the endpoint answers HTTP 402 and needs an x402 payment signed by the agent's wallet.

With the official client (Node, wallet private key in `PAYER_PRIVATE_KEY`):

```ts
import { x402Client } from "@x402/core/client";
import { ExactEvmScheme } from "@x402/evm/exact/client";
import { wrapFetchWithPayment } from "@x402/fetch";
import { privateKeyToAccount } from "viem/accounts";

const signer = privateKeyToAccount(process.env.PAYER_PRIVATE_KEY as `0x${string}`);
const client = new x402Client().register("eip155:8453", new ExactEvmScheme(signer));
const fetchWithPayment = wrapFetchWithPayment(fetch, client);
const res = await fetchWithPayment("https://x402-gateway.blackwoodstate8.workers.dev/v1/timezone", {
  method: "POST",
  headers: { "content-type": "application/json" },
  body: JSON.stringify({"time":"2026-11-03T09:00","from":"America/New_York","to":["Europe/London","Asia/Tokyo","America/Sao_Paulo"]}),
});
console.log(await res.json());
```

With the Coinbase Agentic Wallet CLI: `npx awal x402 pay https://x402-gateway.blackwoodstate8.workers.dev/v1/timezone --method POST --body '{"time":"2026-11-03T09:00","from":"America/New_York","to":["Europe/London","Asia/Tokyo","America/Sao_Paulo"]}'`.

Over MCP (Streamable HTTP at `https://x402-gateway.blackwoodstate8.workers.dev/mcp`, payment via `@x402/mcp`): call the tool `blackwood_timezone_convert` with the same arguments. Add `"response_format": "detailed"` for the full JSON.

## Example

Request:

```json
{
  "time": "2026-11-03T09:00",
  "from": "America/New_York",
  "to": [
    "Europe/London",
    "Asia/Tokyo",
    "America/Sao_Paulo"
  ]
}
```

Response (real output recorded from the service):

```json
{
  "instant": "2026-11-03T14:00:00.000Z",
  "epochMs": 1793714400000,
  "from": {
    "zone": "America/New_York",
    "localTime": "2026-11-03T09:00:00",
    "offset": "-05:00",
    "offsetMinutes": -300,
    "abbreviation": "EST",
    "dst": false
  },
  "to": [
    {
      "zone": "Europe/London",
      "localTime": "2026-11-03T14:00:00",
      "offset": "+00:00",
      "offsetMinutes": 0,
      "abbreviation": "GMT",
      "dst": false
    },
    {
      "zone": "Asia/Tokyo",
      "localTime": "2026-11-03T23:00:00",
      "offset": "+09:00",
      "offsetMinutes": 540,
      "abbreviation": "GMT+9",
      "dst": false
    },
    {
      "zone": "America/Sao_Paulo",
      "localTime": "2026-11-03T11:00:00",
      "offset": "-03:00",
      "offsetMinutes": -180,
      "abbreviation": "GMT-3",
      "dst": false
    }
  ],
  "source": "IANA Time Zone Database (public domain), via Intl in the Cloudflare Workers runtime"
}
```

More: https://x402-gateway.blackwoodstate8.workers.dev/services/timezone · OpenAPI https://x402-gateway.blackwoodstate8.workers.dev/openapi.json · llms.txt https://x402-gateway.blackwoodstate8.workers.dev/llms.txt
