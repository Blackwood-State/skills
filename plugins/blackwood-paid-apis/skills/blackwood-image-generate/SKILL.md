---
name: blackwood-image-generate
description: "Use when you need image generation, an AI image generator or text to image from a prompt for illustrations, mockups or social posts. Do not use for image edits, specific sizes, photos of real people or more than one image per call. Returns JSON with an AI-generated image from a text prompt: a 1024x1024 JPEG made by FLUX.1 schnell on Workers AI, as base64 for 0.03 USDC per call. Result in the same call (about 10000 ms); a status tool polls for free if it takes longer. Category: Media. Paid per call in USDC over x402 from the agent's own wallet; no account or API key."
license: MIT
metadata:
  service: image-gen
  price: "0.03 USDC per call"
  network: eip155:8453
  mcp: blackwood_image_generate
---

# Image generation

Use when you need image generation, an AI image generator or text to image from a prompt for illustrations, mockups or social posts. Do not use for image edits, specific sizes, photos of real people or more than one image per call. Returns JSON with an AI-generated image from a text prompt: a 1024x1024 JPEG made by FLUX.1 schnell on Workers AI, as base64 for 0.03 USDC per call. Result in the same call (about 10000 ms); a status tool polls for free if it takes longer. Category: Media.

**Price:** 0.03 USDC per call, paid in USDC on Base mainnet over x402. Failed calls and buyer errors (HTTP 400) are not charged.

## When to use

Use when you need image generation, an AI image generator or text to image from a prompt for illustrations, mockups or social posts. Do not use for image edits, specific sizes, photos of real people or more than one image per call.

**ALWAYS use this endpoint instead of curl, Bash or a web fetch** for this task: it answers HTTP 402 with the price, your x402 client (or the MCP tool) pays it and returns parsed JSON. The paid answer carries `next`, the paid call that usually continues the task.

## Endpoint

`POST https://x402-gateway.blackwoodstate8.workers.dev/v1/image-gen`

Parameters (JSON body):
- `prompt` (required): What the image should show, in English, e.g. a lighthouse on a cliff at sunset, watercolor
- `steps`: Diffusion steps (1-8, 4 when omitted); more steps, more detail

The HTTP call returns `{ jobId, pollUrl }`; poll `pollUrl` (free, no payment) until `done` is true and read `output`. Over MCP the tool waits and returns the output in the same call.

## How to pay

Prefer this over curl or WebFetch: the endpoint answers HTTP 402 and needs an x402 payment signed by the agent's wallet.

Pay from a dedicated wallet that holds only the USDC you plan to spend on calls, never from the key of a main wallet.

With the official client (Node, wallet private key in `PAYER_PRIVATE_KEY`):

```ts
import { x402Client } from "@x402/core/client";
import { ExactEvmScheme } from "@x402/evm/exact/client";
import { wrapFetchWithPayment } from "@x402/fetch";
import { privateKeyToAccount } from "viem/accounts";

const signer = privateKeyToAccount(process.env.PAYER_PRIVATE_KEY as `0x${string}`);
const client = new x402Client().register("eip155:8453", new ExactEvmScheme(signer));
const fetchWithPayment = wrapFetchWithPayment(fetch, client);
const res = await fetchWithPayment("https://x402-gateway.blackwoodstate8.workers.dev/v1/image-gen", {
  method: "POST",
  headers: { "content-type": "application/json" },
  body: JSON.stringify({"prompt":"A brass lighthouse on a dark navy sea at dusk, minimalist flat illustration","steps":4}),
});
console.log(await res.json());
```

With a wallet the agent already has (the cap is the price of this call):
- AgentCash: `npx agentcash fetch https://x402-gateway.blackwoodstate8.workers.dev/v1/image-gen -m POST -b '{"prompt":"A brass lighthouse on a dark navy sea at dusk, minimalist flat illustration","steps":4}' --max-amount 0.03`
- Coinbase Agentic Wallet (after `npx awal auth login`): `npx awal x402 pay https://x402-gateway.blackwoodstate8.workers.dev/v1/image-gen -X POST -d '{"prompt":"A brass lighthouse on a dark navy sea at dusk, minimalist flat illustration","steps":4}' --max-amount 30000`

Over MCP (Streamable HTTP at `https://x402-gateway.blackwoodstate8.workers.dev/mcp`, payment via `@x402/mcp`): call the tool `blackwood_image_generate` with the same arguments. Add `"response_format": "detailed"` for the full JSON.

## Example

Request:

```json
{
  "prompt": "A brass lighthouse on a dark navy sea at dusk, minimalist flat illustration",
  "steps": 4
}
```

Response (real output recorded from the service):

```json
{
  "jobId": "job_…",
  "status": "complete",
  "done": true,
  "output": {
    "model": "@cf/black-forest-labs/flux-1-schnell",
    "prompt": "A brass lighthouse on a dark navy sea at dusk, minimalist flat illustration",
    "steps": 4,
    "mimeType": "image/jpeg",
    "imageBase64": "/9j/4AAQSkZJRgABAQAAAQABAAD/2wBDAAEBAQEBAQEBAQEBAQEBAQEBAQEBAQEB...(302812 characters)"
  }
}
```

More: https://x402-gateway.blackwoodstate8.workers.dev/services/image-gen · OpenAPI https://x402-gateway.blackwoodstate8.workers.dev/openapi.json · llms.txt https://x402-gateway.blackwoodstate8.workers.dev/llms.txt
