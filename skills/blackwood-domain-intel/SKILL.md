---
name: blackwood-domain-intel
description: Use this when you need a whois or domain lookup, dns records, email security checks or certificate transparency data for osint or due diligence. Returns JSON with a domain's registration (RDAP: registrar, dates, status, DNSSEC), DNS records, SPF and DMARC, and recent TLS certificates for 0.02 USDC per call. Do not use for historical whois, registrant personal data (redacted by registries) or port and vulnerability scans. Typical response time 4000 ms (p95). Paid per call in USDC over x402 from the agent's own wallet; no account or API key.
license: MIT
metadata:
  service: domain-intel
  price: 0.02 USDC per call
  network: eip155:8453
  mcp: blackwood_domain_intel
---

# Domain intelligence

Use this when you need a whois or domain lookup, dns records, email security checks or certificate transparency data for osint or due diligence. Returns JSON with a domain's registration (RDAP: registrar, dates, status, DNSSEC), DNS records, SPF and DMARC, and recent TLS certificates for 0.02 USDC per call. Do not use for historical whois, registrant personal data (redacted by registries) or port and vulnerability scans. Typical response time 4000 ms (p95).

**Price:** 0.02 USDC per call, paid in USDC on Base mainnet over x402. Failed calls and buyer errors (HTTP 400) are not charged.

## When to use

Use this when you need a whois or domain lookup, dns records, email security checks or certificate transparency data for osint or due diligence. Do not use for historical whois, registrant personal data (redacted by registries) or port and vulnerability scans.

## Endpoint

`POST https://x402-gateway.blackwoodstate8.workers.dev/v1/domain-intel`

Parameters (JSON body):
- `domain` (required): Domain name in ASCII (punycode for internationalized names), e.g. example.com
- `certificates`: Include issued TLS certificates from Certificate Transparency (crt.sh); true when omitted

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
const res = await fetchWithPayment("https://x402-gateway.blackwoodstate8.workers.dev/v1/domain-intel", {
  method: "POST",
  headers: { "content-type": "application/json" },
  body: JSON.stringify({"domain":"example.com","certificates":false}),
});
console.log(await res.json());
```

With the Coinbase Agentic Wallet CLI: `npx awal x402 pay https://x402-gateway.blackwoodstate8.workers.dev/v1/domain-intel --method POST --body '{"domain":"example.com","certificates":false}'`.

Over MCP (Streamable HTTP at `https://x402-gateway.blackwoodstate8.workers.dev/mcp`, payment via `@x402/mcp`): call the tool `blackwood_domain_intel` with the same arguments. Add `"response_format": "detailed"` for the full JSON.

## Example

Request:

```json
{
  "domain": "example.com",
  "certificates": false
}
```

Response (real output recorded from the service):

```json
{
  "domain": "example.com",
  "registered": true,
  "registration": {
    "registrar": "RESERVED-Internet Assigned Numbers Authority",
    "created": "1995-08-14T04:00:00Z",
    "updated": "2026-08-14T08:01:43Z",
    "expires": "2027-08-13T04:00:00Z",
    "status": [
      "client delete prohibited",
      "client transfer prohibited",
      "client update prohibited"
    ],
    "nameservers": [
      "elliott.ns.cloudflare.com",
      "hera.ns.cloudflare.com"
    ],
    "dnssec": true,
    "rdapServer": "rdap.verisign.com"
  },
  "dns": {
    "a": [
      "104.20.23.154",
      "172.66.147.243"
    ],
    "aaaa": [
      "2606:4700:10::6814:179a",
      "2606:4700:10::ac42:93f3"
    ],
    "mx": [],
    "ns": [
      "elliott.ns.cloudflare.com",
      "hera.ns.cloudflare.com"
    ],
    "txt": [
      "v=spf1 -all",
      "_k2n1y4vw3qtb4skdx9e7dxt97qrmmq9"
    ],
    "caa": []
  },
  "email": {
    "spf": "v=spf1 -all",
    "dmarc": "v=DMARC1;p=reject;sp=reject;adkim=s;aspf=s",
    "dmarcPolicy": "reject"
  },
  "certificates": null,
  "partial": []
}
```

More: https://x402-gateway.blackwoodstate8.workers.dev/services/domain-intel · OpenAPI https://x402-gateway.blackwoodstate8.workers.dev/openapi.json · llms.txt https://x402-gateway.blackwoodstate8.workers.dev/llms.txt
