---
name: blackwood-random-generate
description: Use this when you need a random number generator, dice rolls, a coin flip, a uuid or random bytes that no one can predict. Returns JSON with cryptographically secure random values: integers in a range, dice rolls, coin flips, UUIDs or hex bytes, from the Workers Web Crypto CSPRNG for 0.005 USDC per call. Do not use for verifiable on-chain randomness (VRF) or reproducible seeded sequences. Typical response time 500 ms (p95). Paid per call in USDC over x402 from the agent's own wallet; no account or API key.
license: MIT
metadata:
  service: random
  price: 0.005 USDC per call
  network: eip155:8453
  mcp: blackwood_random_generate
---

# Random numbers

Use this when you need a random number generator, dice rolls, a coin flip, a uuid or random bytes that no one can predict. Returns JSON with cryptographically secure random values: integers in a range, dice rolls, coin flips, UUIDs or hex bytes, from the Workers Web Crypto CSPRNG for 0.005 USDC per call. Do not use for verifiable on-chain randomness (VRF) or reproducible seeded sequences. Typical response time 500 ms (p95).

**Price:** 0.005 USDC per call, paid in USDC on Base mainnet over x402. Failed calls and buyer errors (HTTP 400) are not charged.

## When to use

Use this when you need a random number generator, dice rolls, a coin flip, a uuid or random bytes that no one can predict. Do not use for verifiable on-chain randomness (VRF) or reproducible seeded sequences.

## Endpoint

`POST https://x402-gateway.blackwoodstate8.workers.dev/v1/random`

Parameters (JSON body):
- `kind` (required): integer (count values between min and max), dice (count rolls of a die with sides faces), coin (count flips), uuid (count UUID v4) or bytes (length random bytes as hex)
- `count`: How many values (1-100, default 1); ignored for bytes
- `min`: integer: lowest value, inclusive (default 1)
- `max`: integer: highest value, inclusive (default 100)
- `sides`: dice: faces of the die (default 6)
- `length`: bytes: how many random bytes (1-256, default 32)

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
const res = await fetchWithPayment("https://x402-gateway.blackwoodstate8.workers.dev/v1/random", {
  method: "POST",
  headers: { "content-type": "application/json" },
  body: JSON.stringify({"kind":"dice","count":2,"sides":6}),
});
console.log(await res.json());
```

With the Coinbase Agentic Wallet CLI: `npx awal x402 pay https://x402-gateway.blackwoodstate8.workers.dev/v1/random --method POST --body '{"kind":"dice","count":2,"sides":6}'`.

Over MCP (Streamable HTTP at `https://x402-gateway.blackwoodstate8.workers.dev/mcp`, payment via `@x402/mcp`): call the tool `blackwood_random_generate` with the same arguments. Add `"response_format": "detailed"` for the full JSON.

## Example

Request:

```json
{
  "kind": "dice",
  "count": 2,
  "sides": 6
}
```

Response (real output recorded from the service):

```json
{
  "kind": "dice",
  "values": [
    4,
    1
  ],
  "sum": 5,
  "generatedAt": "2026-10-17T16:48:06.750Z",
  "source": "crypto.getRandomValues (Cloudflare Workers CSPRNG)"
}
```

More: https://x402-gateway.blackwoodstate8.workers.dev/services/random · OpenAPI https://x402-gateway.blackwoodstate8.workers.dev/openapi.json · llms.txt https://x402-gateway.blackwoodstate8.workers.dev/llms.txt
