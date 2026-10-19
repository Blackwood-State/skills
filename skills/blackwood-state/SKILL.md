---
name: blackwood-state
description: Use when an agent needs live crypto and onchain data (token prices on Base, wallet balances and portfolio value, ENS names, gas fees, new token pools), currency exchange rates from the ECB, or quick AI text utilities (summarize, translate, sentiment), paid per call in USDC with a free starting credit. Installs the Blackwood State MCP server with payment and a spend cap built in.
---

# Blackwood State tools (MCP)

33 pay-per-call tools for agents, served by the Blackwood State API and paid in USDC on Base over x402. The first calls are free: the server asks for a small starting credit (US$0.05) on first use. After that, payment is built in, and capped per call and per day.

## Install

Claude Code:

```bash
claude mcp add blackwood -- npx -y @blackwood-state/mcp
```

Any MCP client (stdio):

```json
{ "mcpServers": { "blackwood": { "command": "npx", "args": ["-y", "@blackwood-state/mcp"] } } }
```

## When to use

- **Onchain data:**
  - `blackwood_token_price`: token price on Base.
  - `blackwood_wallet_balances`, `blackwood_wallet_portfolio`, `blackwood_wallet_value`: what a wallet holds and what it is worth, on Ethereum, Base and Solana.
  - `blackwood_ens_lookup`: ENS names.
  - `blackwood_gas_fees`: gas price.
  - `blackwood_altcoin_discovery`: new tokens with liquidity on Base.
- **Currency rates:** `blackwood_fx_rates` and `blackwood_fx_history`, official ECB reference rates.
- **Text:** `blackwood_text_summarize`, `blackwood_text_translate`, `blackwood_text_sentiment`.
- **Account:** `blackwood_account` (free) shows the credit left, the paying wallet, today's spend and the caps.

Prices run from US$0.002 to US$0.05 per call, and every tool description states its price. A call that fails or rejects the input is not charged.

## Paying after the free credit

Choose one:
- **Wallet:** set `BLACKWOOD_PRIVATE_KEY` to a Base wallet that holds USDC, and each call is paid over x402.
  - `BLACKWOOD_MAX_USD_PER_CALL` caps each call (default 0.05).
  - `BLACKWOOD_MAX_USD_PER_DAY` caps each UTC day (default 1).
- **Prepaid credit:** buy credit at `https://x402-gateway.blackwoodstate8.workers.dev/v1/credits/5` (US$5) and set `BLACKWOOD_CREDITS_KEY` to the `bwk_` key you get back.

The wallet key never leaves the machine; only signed payments do. The credit key and today's spend live in `~/.blackwood/state.json`, readable only by the user.
