---
name: blackwood-email-check
description: Use this when you need email validation or to verify email addresses (domain, MX records) before sending, signup or lead scoring. Returns JSON with an email address check: syntax, domain and MX records, disposable, free and role account flags, SPF and DMARC, and a verdict on whether the domain can receive mail for 0.005 USDC per call. Do not use for confirming that the mailbox itself exists (no SMTP probe is done) or sending email. Typical response time 800 ms (p95). Category: Data. Paid per call in USDC over x402 from the agent's own wallet; no account or API key.
license: MIT
metadata:
  service: email-check
  price: 0.005 USDC per call
  network: eip155:8453
  mcp: blackwood_email_check
---

# Email address check

Use this when you need email validation or to verify email addresses (domain, MX records) before sending, signup or lead scoring. Returns JSON with an email address check: syntax, domain and MX records, disposable, free and role account flags, SPF and DMARC, and a verdict on whether the domain can receive mail for 0.005 USDC per call. Do not use for confirming that the mailbox itself exists (no SMTP probe is done) or sending email. Typical response time 800 ms (p95). Category: Data.

**Price:** 0.005 USDC per call, paid in USDC on Base mainnet over x402. Failed calls and buyer errors (HTTP 400) are not charged.

## When to use

Use this when you need email validation or to verify email addresses (domain, MX records) before sending, signup or lead scoring. Do not use for confirming that the mailbox itself exists (no SMTP probe is done) or sending email.

## Endpoint

`POST https://x402-gateway.blackwoodstate8.workers.dev/v1/email-check`

Parameters (JSON body):
- `email` (required): Email address to check, e.g. jane@example.com

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
const res = await fetchWithPayment("https://x402-gateway.blackwoodstate8.workers.dev/v1/email-check", {
  method: "POST",
  headers: { "content-type": "application/json" },
  body: JSON.stringify({"email":"postmaster@ietf.org"}),
});
console.log(await res.json());
```

With the Coinbase Agentic Wallet CLI: `npx awal x402 pay https://x402-gateway.blackwoodstate8.workers.dev/v1/email-check --method POST --body '{"email":"postmaster@ietf.org"}'`.

Over MCP (Streamable HTTP at `https://x402-gateway.blackwoodstate8.workers.dev/mcp`, payment via `@x402/mcp`): call the tool `blackwood_email_check` with the same arguments. Add `"response_format": "detailed"` for the full JSON.

## Example

Request:

```json
{
  "email": "postmaster@ietf.org"
}
```

Response (real output recorded from the service):

```json
{
  "email": "postmaster@ietf.org",
  "normalized": "postmaster@ietf.org",
  "syntaxValid": true,
  "domain": "ietf.org",
  "disposable": false,
  "freeProvider": false,
  "roleAccount": true,
  "domainExists": true,
  "mx": [
    {
      "priority": 0,
      "host": "mx.ietf.org"
    }
  ],
  "acceptsMail": true,
  "spf": "v=spf1 ip4:166.84.6.31 ip4:166.84.7.238 ip6:2602:f977:800:f7f6::/64 ip4:166.84.7.34 ip6:2602:f977:800::e276:63ff:fe66:3400 include:_spf.google.com include:spf.hostedrt.com ~all",
  "dmarc": "v=DMARC1; p=none; rua=mailto:dmarc_agg@vali.email,mailto:dmarc-report@ietf.org",
  "dmarcPolicy": "none",
  "verdict": "deliverable_domain",
  "reasons": [
    "role_account"
  ],
  "smtpChecked": false
}
```

More: https://x402-gateway.blackwoodstate8.workers.dev/services/email-check · OpenAPI https://x402-gateway.blackwoodstate8.workers.dev/openapi.json · llms.txt https://x402-gateway.blackwoodstate8.workers.dev/llms.txt
