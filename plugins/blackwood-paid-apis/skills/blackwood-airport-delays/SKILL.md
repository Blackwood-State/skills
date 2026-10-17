---
name: blackwood-airport-delays
description: Use this when you need airport delays or flight status context for a US airport, such as ground stops or departure delays right now. Returns JSON with current US airport delays from the FAA: ground stops, ground delay programs with average and maximum delay, arrival and departure delays and closures, for all airports or one for 0.007 USDC per call. Do not use for the status of a flight number, airports outside the US or historical delays. Typical response time 1500 ms (p95). Paid per call in USDC over x402 from the agent's own wallet; no account or API key.
license: MIT
metadata:
  service: airport-delays
  price: 0.007 USDC per call
  network: eip155:8453
  mcp: blackwood_airport_delays
---

# Airport delays

Use this when you need airport delays or flight status context for a US airport, such as ground stops or departure delays right now. Returns JSON with current US airport delays from the FAA: ground stops, ground delay programs with average and maximum delay, arrival and departure delays and closures, for all airports or one for 0.007 USDC per call. Do not use for the status of a flight number, airports outside the US or historical delays. Typical response time 1500 ms (p95).

**Price:** 0.007 USDC per call, paid in USDC on Base mainnet over x402. Failed calls and buyer errors (HTTP 400) are not charged.

## When to use

Use this when you need airport delays or flight status context for a US airport, such as ground stops or departure delays right now. Do not use for the status of a flight number, airports outside the US or historical delays.

## Endpoint

`POST https://x402-gateway.blackwoodstate8.workers.dev/v1/airport-delays`

Parameters (JSON body):
- `airport`: US airport code to filter, e.g. SFO or KSFO (all airports with delays when omitted)

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
const res = await fetchWithPayment("https://x402-gateway.blackwoodstate8.workers.dev/v1/airport-delays", {
  method: "POST",
  headers: { "content-type": "application/json" },
  body: JSON.stringify({}),
});
console.log(await res.json());
```

With the Coinbase Agentic Wallet CLI: `npx awal x402 pay https://x402-gateway.blackwoodstate8.workers.dev/v1/airport-delays --method POST --body '{}'`.

Over MCP (Streamable HTTP at `https://x402-gateway.blackwoodstate8.workers.dev/mcp`, payment via `@x402/mcp`): call the tool `blackwood_airport_delays` with the same arguments. Add `"response_format": "detailed"` for the full JSON.

## Example

Request:

```json
{}
```

Response (real output recorded from the service):

```json
{
  "updatedAt": "Sat Oct 17 16:47:47 2026 GMT",
  "airport": null,
  "total": 8,
  "groundStops": [],
  "groundDelays": [
    {
      "airport": "SAN",
      "reason": "airport volume",
      "average": "53 minutes",
      "maximum": "1 hour and 50 minutes"
    },
    {
      "airport": "YVR",
      "reason": "other",
      "average": "15 minutes",
      "maximum": "43 minutes"
    }
  ],
  "arrivalDepartureDelays": [
    {
      "airport": "MCO",
      "reason": "TM Initiatives:MIT:VOL",
      "type": "Departure",
      "minimum": "16 minutes",
      "maximum": "30 minutes",
      "trend": "Increasing"
    },
    {
      "airport": "MIA",
      "reason": "RWY:Construction",
      "type": "Departure",
      "minimum": "16 minutes",
      "maximum": "30 minutes",
      "trend": "Increasing"
    },
    {
      "airport": "PHX",
      "reason": "VOL:Multi-taxi",
      "type": "Departure",
      "minimum": "16 minutes",
      "maximum": "30 minutes",
      "trend": "Increasing"
    },
    {
      "airport": "RSW",
      "reason": "WX:Thunderstorms",
      "type": "Departure",
      "minimum": "16 minutes",
      "maximum": "30 minutes",
      "trend": "Increasing"
    }
  ],
  "closures": [
    {
      "airport": "LAX",
      "reason": "!LAX 05/277 LAX AD AP CLSD TO NON SKED TRANSIENT GA ACFT EXC 24HR PPR CTC ATLANTIC AVIATION 310-258-9884 OR SIGNATURE AVIATION 310-410-9605 2605271826-2705281600",
      "start": "May 27 at 18:26 UTC.",
      "reopen": "May 28 at 16:00 UTC."
    },
    {
      "airport": "SAN",
      "reason": "!SAN 10/004 SAN AD AP CLSD TO NON SKED TRANSIENT GA ACFT EXC PPR 619-298-7704 2610011633-2701010800",
      "start": "Oct 01 at 16:33 UTC.",
      "reopen": "Jan 01 at 08:00 UTC."
    }
  ],
  "attribution": "Source: FAA National Airspace System Status (nasstatus.faa.gov), U.S. public domain"
}
```

More: https://x402-gateway.blackwoodstate8.workers.dev/services/airport-delays · OpenAPI https://x402-gateway.blackwoodstate8.workers.dev/openapi.json · llms.txt https://x402-gateway.blackwoodstate8.workers.dev/llms.txt
