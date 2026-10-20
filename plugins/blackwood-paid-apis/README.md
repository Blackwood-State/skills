# Blackwood State paid APIs

Plugin with 33 pay-per-call APIs for AI agents: weather, news, email checks, token prices and wallet balances, PDF and web extraction, translation, summaries, speech, images, QR codes, time zones, FX rates and more. Each call is paid in USDC on Base mainnet over [x402](https://x402.org) from the agent's own wallet, with no account and no API key. Generated from the service specifications; do not edit by hand.

## What it contains

- `skills/`: one skill per service (when to use it, price, endpoint, parameters, payment snippet and a real recorded example) and the catalog skill `blackwood-paid-apis`, which lists every service with its price.
- `.claude-plugin/plugin.json`: the manifest, with the MCP server described below and its one option.

No hooks, commands, agents, scripts or executables: the plugin runs nothing on your machine.

## How to pay per call

The endpoints answer HTTP 402 with the price. An x402 client signs the USDC payment with the agent's wallet and repeats the call: `@x402/fetch` in Node (snippet in each skill) or a wallet CLI the agent already has: `npx agentcash fetch <url> -m POST -b '<json>'` or `npx awal x402 pay <url> -X POST -d '<json>'`. Buyer errors (HTTP 400) and failed calls are not charged.

## MCP server (prepaid credits)

The plugin also declares the remote MCP server `blackwood-state` (Streamable HTTP at `https://x402-gateway.blackwoodstate8.workers.dev/mcp`): one `blackwood_*` tool per service, the free `blackwood_credits_balance` tool and a free status tool for each long-running service. Claude Code's MCP client does not sign x402 payments, so the paid tools need a prepaid credits key:

1. Buy a pack with one x402 payment from an agent wallet: `POST https://x402-gateway.blackwoodstate8.workers.dev/v1/credits/5` (US$5 for US$5 of calls), `POST https://x402-gateway.blackwoodstate8.workers.dev/v1/credits/20` (US$20 for US$22.222222 of calls), `POST https://x402-gateway.blackwoodstate8.workers.dev/v1/credits/100` (US$100 for US$125 of calls). For example `npx awal x402 pay https://x402-gateway.blackwoodstate8.workers.dev/v1/credits/5 -X POST --max-amount 5000000` (the cap is in USDC atomic units). Default buyer caps refuse more than US$1 per payment, so raise the cap for that one call.
2. Copy the `apiKey` (`bwk_...`) from the first top-up response into the plugin option `credits_key`: run `/plugin configure blackwood-paid-apis@blackwood-state-skills` in a session, or pipe `{"credits_key": "bwk_..."}` to `claude plugin configure blackwood-paid-apis@blackwood-state-skills --values-stdin`. It is a sensitive option, kept in the system credential store and never written to a file.
3. Every tool call debits the balance; failed calls are not charged. `blackwood_credits_balance` shows the balance and recent debits.

With the option empty the server still connects and lists its tools, and a paid tool answers with the x402 payment terms instead of a result, without charging anything.

## What the plugin sends, and where

- Skills are text. When Claude follows one, the request goes from your x402 client to `https://x402-gateway.blackwoodstate8.workers.dev` with the arguments of that call.
- The MCP server connects to `https://x402-gateway.blackwoodstate8.workers.dev/mcp` when a session starts, to list the tools, and sends the tool arguments when Claude calls a tool. With a credits key it also sends `Authorization: Bearer <key>`.
- No telemetry and no other endpoint: the skills and the MCP server only call `https://x402-gateway.blackwoodstate8.workers.dev`. The x402 client you pick signs the payment with your wallet.

## Install

```
claude plugin marketplace add Blackwood-State/skills
claude plugin install blackwood-paid-apis@blackwood-state-skills
```

The version rises with every published change; run `claude plugin update blackwood-paid-apis@blackwood-state-skills` or turn on auto-update for the marketplace in `/plugin`.

Catalog: https://x402-gateway.blackwoodstate8.workers.dev · OpenAPI: https://x402-gateway.blackwoodstate8.workers.dev/openapi.json · llms.txt: https://x402-gateway.blackwoodstate8.workers.dev/llms.txt · Issues: https://github.com/Blackwood-State/skills/issues · Contact: blackwoodstate8@gmail.com. License: MIT.
