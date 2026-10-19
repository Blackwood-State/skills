# Blackwood State skills

Skills for 33 pay-per-call APIs for AI agents, paid in USDC on Base over [x402](https://x402.org). No account, no API key: the agent pays each call from its own wallet. Generated from the service specifications; do not edit by hand.

- Install everything: `npx skills add Blackwood-State/skills` (or from the gateway: `npx skills add https://x402-gateway.blackwoodstate8.workers.dev`)
- Claude Code: `claude plugin marketplace add Blackwood-State/skills` then `claude plugin install blackwood-paid-apis@blackwood-state-skills` (skills plus the MCP server for prepaid-credit keys; see [the plugin README](plugins/blackwood-paid-apis/README.md))
- Any MCP client (stdio, free starting credit, spend caps per call and per day): `npx -y @blackwood-state/mcp` ([npm](https://www.npmjs.com/package/@blackwood-state/mcp))
- OpenClaw: `openclaw plugins install git:github.com/Blackwood-State/skills@main` · Hermes: `hermes plugins install Blackwood-State/skills` (the server spends money, so Hermes installs it as untrusted)
- MCP server (Streamable HTTP): `https://x402-gateway.blackwoodstate8.workers.dev/mcp`
- OpenAPI: `https://x402-gateway.blackwoodstate8.workers.dev/openapi.json` · llms.txt: `https://x402-gateway.blackwoodstate8.workers.dev/llms.txt`

| Skill | Use this when | Price |
|---|---|---|
| [blackwood-webpage-markdown](skills/blackwood-webpage-markdown/SKILL.md) | you need web page to markdown, url to markdown, to scrape a web page or extract content from a url for an LLM | 0.01 USDC |
| [blackwood-sec-facts](skills/blackwood-sec-facts/SKILL.md) | you need company financials from SEC filings, earnings or fundamentals of a US company by ticker for the last two years | 0.02 USDC |
| [blackwood-pdf-extract](skills/blackwood-pdf-extract/SKILL.md) | you need to extract text from a pdf, pdf to text or document parsing for an LLM | up to 0.05 USDC |
| [blackwood-domain-intel](skills/blackwood-domain-intel/SKILL.md) | you need a domain lookup, whois, dns records or certificate transparency data for osint, security or due diligence | 0.02 USDC |
| [blackwood-weather-forecast](skills/blackwood-weather-forecast/SKILL.md) | you need a weather forecast, the current weather, temperature, rain or wind for the next hours or days at given coordinates | 0.003 USDC |
| [blackwood-email-check](skills/blackwood-email-check/SKILL.md) | you need email validation, to verify email addresses or validate email syntax and MX records before sending or signup | 0.005 USDC |
| [blackwood-llm-chat](skills/blackwood-llm-chat/SKILL.md) | you need an llm chat completion for summaries, extraction or classification from a cheap open model | up to 0.03 USDC |
| [blackwood-audio-transcribe](skills/blackwood-audio-transcribe/SKILL.md) | you need speech to text transcription or translation of an audio file at a public URL | up to 0.05 USDC |
| [blackwood-image-generate](skills/blackwood-image-generate/SKILL.md) | you need image generation, an AI image generator or text to image from a prompt for illustrations, mockups or social posts | 0.03 USDC |
| [blackwood-token-price](skills/blackwood-token-price/SKILL.md) | you need a crypto token price or market data for a Base token, quoted from on-chain liquidity | 0.003 USDC |
| [blackwood-wallet-balances](skills/blackwood-wallet-balances/SKILL.md) | you need a wallet balance, token balances or the ERC20 balance of an address on Base, read live from the chain | 0.004 USDC |
| [blackwood-news-search](skills/blackwood-news-search/SKILL.md) | you need news articles or headlines from the last 72 hours about a topic, company or person | 0.008 USDC |
| [blackwood-crypto-news](skills/blackwood-crypto-news/SKILL.md) | you need crypto news or crypto market news headlines about a token or the market from recent articles | 0.005 USDC |
| [blackwood-random-generate](skills/blackwood-random-generate/SKILL.md) | you need a random number generator, dice rolls, a coin flip, a uuid or random bytes that no one can predict | 0.005 USDC |
| [blackwood-timezone-convert](skills/blackwood-timezone-convert/SKILL.md) | you need timezone conversion between time zones, the current time in a zone or a utc offset with daylight saving | 0.003 USDC |
| [blackwood-text-summarize](skills/blackwood-text-summarize/SKILL.md) | you need to summarize text such as an article, notes or a transcript into a short summary | 0.008 USDC |
| [blackwood-airport-delays](skills/blackwood-airport-delays/SKILL.md) | you need airport delays, FAA ground stops or the airport status of a US airport right now, such as departure delays or closures | 0.007 USDC |
| [blackwood-speech-synthesize](skills/blackwood-speech-synthesize/SKILL.md) | you need text to speech, tts, a voice audio file or spoken narration of a short text | 0.023 USDC |
| [blackwood-headers-audit](skills/blackwood-headers-audit/SKILL.md) | you need an http security headers audit of a website or URL, with a grade and what to fix | 0.007 USDC |
| [blackwood-qrcode-generate](skills/blackwood-qrcode-generate/SKILL.md) | you need a qr code generator for a URL, text, Wi-Fi or payment link as an SVG image | 0.004 USDC |
| [blackwood-text-translate](skills/blackwood-text-translate/SKILL.md) | you need to translate text, a language translation or a text translation of a sentence or paragraph between two languages | 0.012 USDC |
| [blackwood-fx-rates](skills/blackwood-fx-rates/SKILL.md) | you need currency exchange rates, fx rates or forex conversion of an amount between two currencies like EUR and GBP | 0.005 USDC |
| [blackwood-x402-check](skills/blackwood-x402-check/SKILL.md) | you need an x402 endpoint check or x402 endpoint test, or to validate x402 endpoint listings before paying or integrating | 0.007 USDC |
| [blackwood-text-sentiment](skills/blackwood-text-sentiment/SKILL.md) | you need sentiment analysis, text sentiment or opinion mining of reviews, comments or headlines in English | 0.004 USDC |
| [blackwood-fx-history](skills/blackwood-fx-history/SKILL.md) | you need historical exchange rates, a historical FX rate series, forex history or a currency trend | 0.005 USDC |
| [blackwood-email-batch](skills/blackwood-email-batch/SKILL.md) | you need bulk email validation, email list verification or to verify email lists before a send or import | 0.02 USDC |
| [blackwood-ens-lookup](skills/blackwood-ens-lookup/SKILL.md) | you need to resolve an ENS name to an address, reverse-resolve an address to its ENS name, or read an ENS avatar and profile records | 0.005 USDC |
| [blackwood-gas-fees](skills/blackwood-gas-fees/SKILL.md) | you need the current gas price, an ethereum gas fee estimate before a transaction, or to compare gas fees across chains | 0.003 USDC |
| [blackwood-wallet-portfolio](skills/blackwood-wallet-portfolio/SKILL.md) | you need a multi-chain wallet balance, a wallet portfolio or wallet transaction history on Ethereum, Base or Solana | 0.005 USDC |
| [blackwood-news-briefing](skills/blackwood-news-briefing/SKILL.md) | you need a news briefing, a research summary of recent coverage or an analysis of what the press says about a topic or company | 0.04 USDC |
| [blackwood-company-dossier](skills/blackwood-company-dossier/SKILL.md) | you need company research, due diligence or a quick analysis of a business from its website domain and optional ticker | 0.03 USDC |
| [blackwood-wallet-value](skills/blackwood-wallet-value/SKILL.md) | you need a wallet portfolio value in USD, a wallet net worth or token balances with USD value for a 0x address | 0.005 USDC |
| [blackwood-altcoin-discovery](skills/blackwood-altcoin-discovery/SKILL.md) | you need altcoin discovery, new token listings or fresh Base tokens with real liquidity to research | 0.005 USDC |

License: MIT. Contact: blackwoodstate8@gmail.com.
