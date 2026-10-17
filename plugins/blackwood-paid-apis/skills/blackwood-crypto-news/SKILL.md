---
name: blackwood-crypto-news
description: Use this when you need crypto news headlines about bitcoin, ethereum, a token or the crypto market from recent articles. Returns JSON with the latest crypto and blockchain news headlines from the last 72 hours (title, link, source, time, tone, people and organizations), from the GDELT global news graph for 0.005 USDC per call. Do not use for prices, full article text, social media posts or news older than 72 hours. Typical response time 1500 ms (p95). Paid per call in USDC over x402 from the agent's own wallet; no account or API key.
license: MIT
metadata:
  service: crypto-news
  price: 0.005 USDC per call
  network: eip155:8453
  mcp: blackwood_crypto_news
---

# Crypto news

Use this when you need crypto news headlines about bitcoin, ethereum, a token or the crypto market from recent articles. Returns JSON with the latest crypto and blockchain news headlines from the last 72 hours (title, link, source, time, tone, people and organizations), from the GDELT global news graph for 0.005 USDC per call. Do not use for prices, full article text, social media posts or news older than 72 hours. Typical response time 1500 ms (p95).

**Price:** 0.005 USDC per call, paid in USDC on Base mainnet over x402. Failed calls and buyer errors (HTTP 400) are not charged.

## When to use

Use this when you need crypto news headlines about bitcoin, ethereum, a token or the crypto market from recent articles. Do not use for prices, full article text, social media posts or news older than 72 hours.

## Endpoint

`POST https://x402-gateway.blackwoodstate8.workers.dev/v1/crypto-news`

Parameters (JSON body):
- `token`: Narrow to one coin, chain or company, e.g. solana, xrp or coinbase (any crypto news when omitted)
- `hours`: How far back to search, in hours (1-72, default 24)
- `limit`: Most articles to return (1-50, default 20), newest first

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
const res = await fetchWithPayment("https://x402-gateway.blackwoodstate8.workers.dev/v1/crypto-news", {
  method: "POST",
  headers: { "content-type": "application/json" },
  body: JSON.stringify({"hours":24,"limit":5}),
});
console.log(await res.json());
```

With the Coinbase Agentic Wallet CLI: `npx awal x402 pay https://x402-gateway.blackwoodstate8.workers.dev/v1/crypto-news --method POST --body '{"hours":24,"limit":5}'`.

Over MCP (Streamable HTTP at `https://x402-gateway.blackwoodstate8.workers.dev/mcp`, payment via `@x402/mcp`): call the tool `blackwood_crypto_news` with the same arguments. Add `"response_format": "detailed"` for the full JSON.

## Example

Request:

```json
{
  "hours": 24,
  "limit": 5
}
```

Response (real output recorded from the service):

```json
{
  "token": null,
  "hours": 24,
  "count": 5,
  "articles": [
    {
      "title": "Hyderabad: 5 arrested in Rs 25 lakh robbery in crypto deal",
      "url": "https://aninews.in/news/national/general-news/hyderabad-5-arrested-in-rs-25-lakh-robbery-in-crypto-deal20261017214202/",
      "domain": "aninews.in",
      "publishedAt": "2026-10-17T16:45:00.000Z",
      "tone": -6.78,
      "image": null,
      "persons": [
        "Bharatiya Nyaya Sanhita",
        "Syed Ajaz",
        "Mohammed Faisal",
        "Guglavath Vijesh Kumar",
        "Syed Zakariya Ahmed",
        "Sumair Khan",
        "Faraz Hussain"
      ],
      "organizations": [
        "Nampally Police",
        "Himalayan Hotel At Bazarghat"
      ]
    },
    {
      "title": "Morning Minute: OKX Raises at $25B From Circle, Ripple, and StanChart",
      "url": "https://decrypt.co/380281/morning-minute-okx-raises-at-25b-from-circle-ripple-and-stanchart&t=Morning%20Minute:%20OKX%20Raises%20at%20$25B%20From%20Circle,%20Ripple,%20and%20StanChart&fnp_t=1792254492.2c1aa61f211d4848",
      "domain": "decrypt.co",
      "publishedAt": "2026-10-17T16:45:00.000Z",
      "tone": -0.72,
      "image": "https://cdn.decrypt.co/resize/1024/height/512/wp-content/uploads/2026/03/okx-decrypt-style-gID_7.jpg",
      "persons": [
        "Tyler Warner",
        "Bitcoin Etfssaw"
      ],
      "organizations": [
        "New York Stock Exchange",
        "York Stock Exchange",
        "Blackrock",
        "Qube Research",
        "Intercontinental Exchange"
      ]
    },
    {
      "title": "Chips Under Control?",
      "url": "https://www.miragenews.com/chips-under-control-1762163/",
      "domain": "miragenews.com",
      "publishedAt": "2026-10-17T16:30:00.000Z",
      "tone": -1.39,
      "image": "https://www.mpg.de/27016264/original-1791212021.jpg?t=ZXlKM2FXUjBhQ0k2T0RRNExDSm9aV2xuYUhRaU9qUTNOeXdpWm1sMElqb2lZM0p2Y0NJc0ltWnBiR1ZmWlhoMFpXNXphVzl1SWpvaWFuQm5JaXdpYjJKcVgybGtJam95TnpBeE5qSTJOSDA9LS1kMGQ3ZTM4ZTI3OTg5ZmJhYzdlMGJmM2U4YzIxY2FiNzI5ZmJlM2Fh",
      "persons": [
        "Hoang Nguyen Hien Pham",
        "Edward Snowden",
        "Thomas Brandstetter",
        "Christof Paar",
        "Julian Speith"
      ],
      "organizations": [
        "Cryptographic Engineering Group Of Peter Schwabe",
        "Technology Nist",
        "Google",
        "Huawei",
        "Max Planck Institute For Security",
        "Microsoft",
        "Us National Institute Of Standards",
        "United States"
      ]
    },
    {
      "title": "This is How AI Gets Physical | The View From North Central Idaho",
      "url": "https://blog.joehuffman.org/2026/10/17/this-is-how-ai-gets-physical/",
      "domain": "joehuffman.org",
      "publishedAt": "2026-10-17T16:30:00.000Z",
      "tone": 1.04,
      "image": null,
      "persons": [
        "Ethereum Sepolia"
      ],
      "organizations": []
    },
    {
      "title": "Who Really Owns the Bitcoin Inside BlackRock's ETF?",
      "url": "https://247wallst.com/investing/cryptocurrency/2026/10/17/who-really-owns-the-bitcoin-inside-blackrocks-etf/",
      "domain": "247wallst.com",
      "publishedAt": "2026-10-17T16:00:00.000Z",
      "tone": 3.16,
      "image": "https://247wallst.com/wp-content/uploads/2025/12/shutterstock-1933377095-huge-licensed-scaled.jpg",
      "persons": [
        "Blackrock Bitcoin",
        "Velishchuk Yevhen Shutterstock"
      ],
      "organizations": [
        "Anchorage Digital Bank",
        "Bitcoin Trust",
        "Blackrock Fund Advisors",
        "Blackrock"
      ]
    }
  ],
  "lastIngestAt": "2026-10-17T16:47:08.395Z",
  "attribution": "Source: The GDELT Project (https://www.gdeltproject.org/)"
}
```

More: https://x402-gateway.blackwoodstate8.workers.dev/services/crypto-news · OpenAPI https://x402-gateway.blackwoodstate8.workers.dev/openapi.json · llms.txt https://x402-gateway.blackwoodstate8.workers.dev/llms.txt
