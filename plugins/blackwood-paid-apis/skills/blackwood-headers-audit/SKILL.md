---
name: blackwood-headers-audit
description: Use this when you need an http security headers audit of a website or URL, with a grade and what to fix. Returns JSON auditing the HTTP security headers of a public URL (HSTS, CSP, X-Frame-Options, X-Content-Type-Options, Referrer-Policy, Permissions-Policy, cookies) with a grade and fixes for 0.007 USDC per call. Do not use for vulnerability scans, TLS cipher analysis or pages behind a login. Typical response time 2500 ms (p95). Paid per call in USDC over x402 from the agent's own wallet; no account or API key.
license: MIT
metadata:
  service: http-headers
  price: 0.007 USDC per call
  network: eip155:8453
  mcp: blackwood_headers_audit
---

# Security headers

Use this when you need an http security headers audit of a website or URL, with a grade and what to fix. Returns JSON auditing the HTTP security headers of a public URL (HSTS, CSP, X-Frame-Options, X-Content-Type-Options, Referrer-Policy, Permissions-Policy, cookies) with a grade and fixes for 0.007 USDC per call. Do not use for vulnerability scans, TLS cipher analysis or pages behind a login. Typical response time 2500 ms (p95).

**Price:** 0.007 USDC per call, paid in USDC on Base mainnet over x402. Failed calls and buyer errors (HTTP 400) are not charged.

## When to use

Use this when you need an http security headers audit of a website or URL, with a grade and what to fix. Do not use for vulnerability scans, TLS cipher analysis or pages behind a login.

## Endpoint

`POST https://x402-gateway.blackwoodstate8.workers.dev/v1/http-headers`

Parameters (JSON body):
- `url` (required): Public http(s) URL to audit, e.g. https://example.com

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
const res = await fetchWithPayment("https://x402-gateway.blackwoodstate8.workers.dev/v1/http-headers", {
  method: "POST",
  headers: { "content-type": "application/json" },
  body: JSON.stringify({"url":"https://example.com"}),
});
console.log(await res.json());
```

With the Coinbase Agentic Wallet CLI: `npx awal x402 pay https://x402-gateway.blackwoodstate8.workers.dev/v1/http-headers --method POST --body '{"url":"https://example.com"}'`.

Over MCP (Streamable HTTP at `https://x402-gateway.blackwoodstate8.workers.dev/mcp`, payment via `@x402/mcp`): call the tool `blackwood_headers_audit` with the same arguments. Add `"response_format": "detailed"` for the full JSON.

## Example

Request:

```json
{
  "url": "https://example.com"
}
```

Response (real output recorded from the service):

```json
{
  "url": "https://example.com",
  "finalUrl": "https://example.com/",
  "status": 200,
  "redirects": 0,
  "grade": "F",
  "score": 10,
  "checks": [
    {
      "header": "strict-transport-security",
      "result": "fail",
      "value": null,
      "advice": "Add Strict-Transport-Security: max-age=31536000; includeSubDomains"
    },
    {
      "header": "content-security-policy",
      "result": "fail",
      "value": null,
      "advice": "Add a Content-Security-Policy; start with default-src 'self' and tighten from there."
    },
    {
      "header": "x-frame-options",
      "result": "fail",
      "value": null,
      "advice": "Add X-Frame-Options: DENY (or CSP frame-ancestors 'none') against clickjacking."
    },
    {
      "header": "x-content-type-options",
      "result": "fail",
      "value": null,
      "advice": "Add X-Content-Type-Options: nosniff."
    },
    {
      "header": "referrer-policy",
      "result": "warn",
      "value": null,
      "advice": "Add Referrer-Policy: strict-origin-when-cross-origin."
    },
    {
      "header": "permissions-policy",
      "result": "warn",
      "value": null,
      "advice": "Add a Permissions-Policy that turns off features you do not use (camera, microphone, geolocation)."
    }
  ],
  "cookies": [],
  "checkedAt": "2026-10-17T16:49:19.296Z"
}
```

More: https://x402-gateway.blackwoodstate8.workers.dev/services/http-headers · OpenAPI https://x402-gateway.blackwoodstate8.workers.dev/openapi.json · llms.txt https://x402-gateway.blackwoodstate8.workers.dev/llms.txt
