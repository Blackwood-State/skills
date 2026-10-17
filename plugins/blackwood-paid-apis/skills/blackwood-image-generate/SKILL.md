---
name: blackwood-image-generate
description: Use this when you need image generation or text to image from a prompt for illustrations or mockups. Returns JSON with a 1024x1024 JPEG generated from a text prompt by FLUX.1 schnell on Workers AI, as base64 for 0.03 USDC per call. Do not use for image edits, specific sizes, photos of real people or more than one image per call. Result in the same call (about 10000 ms); a status tool polls for free if it takes longer. Category: Media. Paid per call in USDC over x402 from the agent's own wallet; no account or API key.
license: MIT
metadata:
  service: image-gen
  price: 0.03 USDC per call
  network: eip155:8453
  mcp: blackwood_image_generate
---

# Text to image

Use this when you need image generation or text to image from a prompt for illustrations or mockups. Returns JSON with a 1024x1024 JPEG generated from a text prompt by FLUX.1 schnell on Workers AI, as base64 for 0.03 USDC per call. Do not use for image edits, specific sizes, photos of real people or more than one image per call. Result in the same call (about 10000 ms); a status tool polls for free if it takes longer. Category: Media.

**Price:** 0.03 USDC per call, paid in USDC on Base mainnet over x402. Failed calls and buyer errors (HTTP 400) are not charged.

## When to use

Use this when you need image generation or text to image from a prompt for illustrations or mockups. Do not use for image edits, specific sizes, photos of real people or more than one image per call.

## Endpoint

`POST https://x402-gateway.blackwoodstate8.workers.dev/v1/image-gen`

Parameters (JSON body):
- `prompt` (required): What the image should show, in English, e.g. a lighthouse on a cliff at sunset, watercolor
- `steps`: Diffusion steps (1-8, 4 when omitted); more steps, more detail

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
const res = await fetchWithPayment("https://x402-gateway.blackwoodstate8.workers.dev/v1/image-gen", {
  method: "POST",
  headers: { "content-type": "application/json" },
  body: JSON.stringify({"prompt":"A brass lighthouse on a dark navy sea at dusk, minimalist flat illustration","steps":4}),
});
console.log(await res.json());
```

With the Coinbase Agentic Wallet CLI: `npx awal x402 pay https://x402-gateway.blackwoodstate8.workers.dev/v1/image-gen --method POST --body '{"prompt":"A brass lighthouse on a dark navy sea at dusk, minimalist flat illustration","steps":4}'`.

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
