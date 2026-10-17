---
name: blackwood-text-translate
description: Use this when you need to translate text, a language translation of a sentence or paragraph between two languages. Returns JSON with your text translated between about 100 languages by Meta M2M100 1.2B on Workers AI, with the source and target languages used for 0.012 USDC per call. Do not use for formatting that must be preserved, certified or legal translation, or speech (transcribe first). Typical response time 5000 ms (p95). Category: Inference. Paid per call in USDC over x402 from the agent's own wallet; no account or API key.
license: MIT
metadata:
  service: translate
  price: 0.012 USDC per call
  network: eip155:8453
  mcp: blackwood_text_translate
---

# Translate text

Use this when you need to translate text, a language translation of a sentence or paragraph between two languages. Returns JSON with your text translated between about 100 languages by Meta M2M100 1.2B on Workers AI, with the source and target languages used for 0.012 USDC per call. Do not use for formatting that must be preserved, certified or legal translation, or speech (transcribe first). Typical response time 5000 ms (p95). Category: Inference.

**Price:** 0.012 USDC per call, paid in USDC on Base mainnet over x402. Failed calls and buyer errors (HTTP 400) are not charged.

## When to use

Use this when you need to translate text, a language translation of a sentence or paragraph between two languages. Do not use for formatting that must be preserved, certified or legal translation, or speech (transcribe first).

## Endpoint

`POST https://x402-gateway.blackwoodstate8.workers.dev/v1/translate`

Parameters (JSON body):
- `text` (required): Text to translate, e.g. Pay only for the requests you make (up to 2000 characters)
- `to` (required): Target language as an ISO 639-1 code, e.g. es, fr, pt, de, ja, zh
- `from`: Source language as an ISO 639-1 code (default en)

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
const res = await fetchWithPayment("https://x402-gateway.blackwoodstate8.workers.dev/v1/translate", {
  method: "POST",
  headers: { "content-type": "application/json" },
  body: JSON.stringify({"text":"Pay only for the requests you make.","from":"en","to":"pt"}),
});
console.log(await res.json());
```

With the Coinbase Agentic Wallet CLI: `npx awal x402 pay https://x402-gateway.blackwoodstate8.workers.dev/v1/translate --method POST --body '{"text":"Pay only for the requests you make.","from":"en","to":"pt"}'`.

Over MCP (Streamable HTTP at `https://x402-gateway.blackwoodstate8.workers.dev/mcp`, payment via `@x402/mcp`): call the tool `blackwood_text_translate` with the same arguments. Add `"response_format": "detailed"` for the full JSON.

## Example

Request:

```json
{
  "text": "Pay only for the requests you make.",
  "from": "en",
  "to": "pt"
}
```

Response (real output recorded from the service):

```json
{
  "translatedText": "Pague apenas pelos pedidos que você faz.",
  "from": "en",
  "to": "pt",
  "model": "@cf/meta/m2m100-1.2b"
}
```

More: https://x402-gateway.blackwoodstate8.workers.dev/services/translate · OpenAPI https://x402-gateway.blackwoodstate8.workers.dev/openapi.json · llms.txt https://x402-gateway.blackwoodstate8.workers.dev/llms.txt
