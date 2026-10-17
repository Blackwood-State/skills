---
name: blackwood-llm-chat
description: Use this when you need an llm chat completion for summaries, extraction or classification from a cheap open model. Returns JSON with a chat completion from IBM Granite 4.0 Micro on Workers AI, with token usage for 0.002 USDC per thousand tokens, up to 0.03 USDC per call (clients without upto pay the cap). Do not use for frontier models, tool calling or streaming. Result in the same call (about 8000 ms); a status tool polls for free if it takes longer. Category: Inference. Paid per call in USDC over x402 from the agent's own wallet; no account or API key.
license: MIT
metadata:
  service: llm-chat
  price: 0.002 USDC per thousand tokens, at most 0.03 USDC per call
  network: eip155:8453
  mcp: blackwood_llm_chat
---

# LLM chat completion

Use this when you need an llm chat completion for summaries, extraction or classification from a cheap open model. Returns JSON with a chat completion from IBM Granite 4.0 Micro on Workers AI, with token usage for 0.002 USDC per thousand tokens, up to 0.03 USDC per call (clients without upto pay the cap). Do not use for frontier models, tool calling or streaming. Result in the same call (about 8000 ms); a status tool polls for free if it takes longer. Category: Inference.

**Price:** 0.002 USDC per thousand tokens, at most 0.03 USDC per call, paid in USDC on Base mainnet over x402. Failed calls and buyer errors (HTTP 400) are not charged.

## When to use

Use this when you need an llm chat completion for summaries, extraction or classification from a cheap open model. Do not use for frontier models, tool calling or streaming.

## Endpoint

`POST https://x402-gateway.blackwoodstate8.workers.dev/v1/llm-chat`

Parameters (JSON body):
- `messages` (required): Conversation so far, oldest first, e.g. [{"role": "user", "content": "Summarize: ..."}]
- `maxTokens`: Maximum tokens to generate (1-4096, 512 when omitted); billed as requested
- `temperature`: Sampling temperature (0-2), e.g. 0.2 for near-deterministic output

The HTTP call returns `{ jobId, pollUrl }`; poll `pollUrl` (free, no payment) until `done` is true and read `output`. Over MCP the tool waits and returns the output in the same call.

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
const res = await fetchWithPayment("https://x402-gateway.blackwoodstate8.workers.dev/v1/llm-chat", {
  method: "POST",
  headers: { "content-type": "application/json" },
  body: JSON.stringify({"messages":[{"role":"user","content":"In one sentence, what is the x402 payment protocol?"}],"maxTokens":80}),
});
console.log(await res.json());
```

With the Coinbase Agentic Wallet CLI: `npx awal x402 pay https://x402-gateway.blackwoodstate8.workers.dev/v1/llm-chat --method POST --body '{"messages":[{"role":"user","content":"In one sentence, what is the x402 payment protocol?"}],"maxTokens":80}'`.

Over MCP (Streamable HTTP at `https://x402-gateway.blackwoodstate8.workers.dev/mcp`, payment via `@x402/mcp`): call the tool `blackwood_llm_chat` with the same arguments. Add `"response_format": "detailed"` for the full JSON.

## Example

Request:

```json
{
  "messages": [
    {
      "role": "user",
      "content": "In one sentence, what is the x402 payment protocol?"
    }
  ],
  "maxTokens": 80
}
```

Response (real output recorded from the service):

```json
{
  "jobId": "job_…",
  "status": "complete",
  "done": true,
  "output": {
    "model": "@cf/ibm-granite/granite-4.0-h-micro",
    "content": "The x402 payment protocol is a standardized system for secure, real-time electronic payments that enables transactions to be processed quickly, reliably, and with a high level of security across different financial institutions.",
    "finishReason": "stop",
    "usage": {
      "promptTokens": 42,
      "completionTokens": 39
    }
  }
}
```

More: https://x402-gateway.blackwoodstate8.workers.dev/services/llm-chat · OpenAPI https://x402-gateway.blackwoodstate8.workers.dev/openapi.json · llms.txt https://x402-gateway.blackwoodstate8.workers.dev/llms.txt
