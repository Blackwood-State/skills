---
name: blackwood-webpage-markdown
description: Use this when you need to scrape a web page, extract content from a url or read an article as markdown for an LLM. Returns JSON with the clean Markdown of a public web page plus its title, description, language, canonical URL and links for 0.01 USDC per call. Do not use for pages that need JavaScript to render, sit behind a login or paywall, or are not HTML (PDFs go to blackwood_pdf_extract). Typical response time 2500 ms (p95). Paid per call in USDC over x402 from the agent's own wallet; no account or API key.
license: MIT
metadata:
  service: web-markdown
  price: 0.01 USDC per call
  network: eip155:8453
  mcp: blackwood_webpage_markdown
---

# Web page to Markdown

Use this when you need to scrape a web page, extract content from a url or read an article as markdown for an LLM. Returns JSON with the clean Markdown of a public web page plus its title, description, language, canonical URL and links for 0.01 USDC per call. Do not use for pages that need JavaScript to render, sit behind a login or paywall, or are not HTML (PDFs go to blackwood_pdf_extract). Typical response time 2500 ms (p95).

**Price:** 0.01 USDC per call, paid in USDC on Base mainnet over x402. Failed calls and buyer errors (HTTP 400) are not charged.

## When to use

Use this when you need to scrape a web page, extract content from a url or read an article as markdown for an LLM. Do not use for pages that need JavaScript to render, sit behind a login or paywall, or are not HTML (PDFs go to blackwood_pdf_extract).

## Endpoint

`POST https://x402-gateway.blackwoodstate8.workers.dev/v1/web-markdown`

Parameters (JSON body):
- `url` (required): Public http(s) URL of the web page to read, e.g. https://example.com/article
- `maxChars`: Maximum length of the returned markdown (500-100000 characters, 50000 when omitted)

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
const res = await fetchWithPayment("https://x402-gateway.blackwoodstate8.workers.dev/v1/web-markdown", {
  method: "POST",
  headers: { "content-type": "application/json" },
  body: JSON.stringify({"url":"https://httpbin.org/html","maxChars":500}),
});
console.log(await res.json());
```

With the Coinbase Agentic Wallet CLI: `npx awal x402 pay https://x402-gateway.blackwoodstate8.workers.dev/v1/web-markdown --method POST --body '{"url":"https://httpbin.org/html","maxChars":500}'`.

Over MCP (Streamable HTTP at `https://x402-gateway.blackwoodstate8.workers.dev/mcp`, payment via `@x402/mcp`): call the tool `blackwood_webpage_markdown` with the same arguments. Add `"response_format": "detailed"` for the full JSON.

## Example

Request:

```json
{
  "url": "https://httpbin.org/html",
  "maxChars": 500
}
```

Response (real output recorded from the service):

```json
{
  "url": "https://httpbin.org/html",
  "finalUrl": "https://httpbin.org/html",
  "status": 200,
  "title": null,
  "description": null,
  "lang": null,
  "canonical": null,
  "markdown": "# Herman Melville - Moby-Dick\n\nAvailing himself of the mild, summer-cool weather that now reigned in these latitudes, and in preparation for the peculiarly active pursuits shortly to be anticipated, Perth, the begrimed, blistered old blacksmith, had not removed his portable forge to the hold again, after concluding his contributory work for Ahab's leg, but still retained it on deck, fast lashed to ringbolts by the foremast; being now almost incessantly invoked by the headsmen, and harpooneers, a",
  "links": [],
  "wordCount": 78,
  "truncated": true
}
```

More: https://x402-gateway.blackwoodstate8.workers.dev/services/web-markdown · OpenAPI https://x402-gateway.blackwoodstate8.workers.dev/openapi.json · llms.txt https://x402-gateway.blackwoodstate8.workers.dev/llms.txt
