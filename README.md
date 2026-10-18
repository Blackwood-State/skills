# Blackwood State skills

Skills for 32 pay-per-call APIs for AI agents, paid in USDC on Base over [x402](https://x402.org). No account, no API key: the agent pays each call from its own wallet. Generated from the service specifications; do not edit by hand.

- Install everything: `npx skills add Blackwood-State/skills` (or from the gateway: `npx skills add https://x402-gateway.blackwoodstate8.workers.dev`)
- Claude Code: `claude plugin marketplace add Blackwood-State/skills` then `claude plugin install blackwood-paid-apis@blackwood-state-skills`
- MCP server (Streamable HTTP): `https://x402-gateway.blackwoodstate8.workers.dev/mcp`
- OpenAPI: `https://x402-gateway.blackwoodstate8.workers.dev/openapi.json` · llms.txt: `https://x402-gateway.blackwoodstate8.workers.dev/llms.txt`

| Skill | Use this when | Price |
|---|---|---|
| [blackwood-webpage-markdown](skills/blackwood-webpage-markdown/SKILL.md) | you need to scrape a web page, extract content from a url or read an article as markdown for an LLM | 0.01 USDC |
| [blackwood-sec-facts](skills/blackwood-sec-facts/SKILL.md) | you need sec filings company financials, earnings or fundamentals for a stock ticker for the last two fiscal years | 0.02 USDC |
| [blackwood-pdf-extract](skills/blackwood-pdf-extract/SKILL.md) | you need to extract text from a pdf for document parsing, search or an LLM | up to 0.05 USDC |
| [blackwood-domain-intel](skills/blackwood-domain-intel/SKILL.md) | you need a whois or domain lookup, dns records, email security checks or certificate transparency data for osint or due diligence | 0.02 USDC |
| [blackwood-weather-forecast](skills/blackwood-weather-forecast/SKILL.md) | you need a weather forecast, temperature or precipitation for the next hours or days at a place given by coordinates | 0.003 USDC |
| [blackwood-email-check](skills/blackwood-email-check/SKILL.md) | you need email validation or to verify email addresses (domain, MX records) before sending, signup or lead scoring | 0.005 USDC |
| [blackwood-llm-chat](skills/blackwood-llm-chat/SKILL.md) | you need an llm chat completion for summaries, extraction or classification from a cheap open model | up to 0.03 USDC |
| [blackwood-audio-transcribe](skills/blackwood-audio-transcribe/SKILL.md) | you need speech to text transcription or translation of an audio file at a public URL | up to 0.05 USDC |
| [blackwood-image-generate](skills/blackwood-image-generate/SKILL.md) | you need image generation or text to image from a prompt for illustrations or mockups | 0.03 USDC |
| [blackwood-token-price](skills/blackwood-token-price/SKILL.md) | you need a crypto token price or market data for a Base token, quoted from on-chain liquidity | 0.003 USDC |
| [blackwood-wallet-balances](skills/blackwood-wallet-balances/SKILL.md) | you need the ERC20 token balance or wallet balance of an address on Base for onchain data checks | 0.004 USDC |
| [blackwood-news-search](skills/blackwood-news-search/SKILL.md) | you need news articles or headlines from the last 72 hours about a topic, company or person | 0.008 USDC |
| [blackwood-crypto-news](skills/blackwood-crypto-news/SKILL.md) | you need crypto news headlines about bitcoin, ethereum, a token or the crypto market from recent articles | 0.005 USDC |
| [blackwood-random-generate](skills/blackwood-random-generate/SKILL.md) | you need a random number generator, dice rolls, a coin flip, a uuid or random bytes that no one can predict | 0.005 USDC |
| [blackwood-timezone-convert](skills/blackwood-timezone-convert/SKILL.md) | you need timezone conversion between time zones, the current time in a zone or a utc offset with daylight saving | 0.003 USDC |
| [blackwood-text-summarize](skills/blackwood-text-summarize/SKILL.md) | you need to summarize text such as an article, notes or a transcript into a short summary | 0.008 USDC |
| [blackwood-airport-delays](skills/blackwood-airport-delays/SKILL.md) | you need airport delays or flight status context for a US airport, such as ground stops or departure delays right now | 0.007 USDC |
| [blackwood-speech-synthesize](skills/blackwood-speech-synthesize/SKILL.md) | you need text to speech, a voice audio file or narration of a short text | 0.023 USDC |
| [blackwood-headers-audit](skills/blackwood-headers-audit/SKILL.md) | you need an http security headers audit of a website or URL, with a grade and what to fix | 0.007 USDC |
| [blackwood-qrcode-generate](skills/blackwood-qrcode-generate/SKILL.md) | you need a qr code generator for a URL, text, Wi-Fi or payment link as an SVG image | 0.004 USDC |
| [blackwood-text-translate](skills/blackwood-text-translate/SKILL.md) | you need to translate text, a language translation of a sentence or paragraph between two languages | 0.012 USDC |
| [blackwood-fx-rates](skills/blackwood-fx-rates/SKILL.md) | you need currency exchange rates, forex conversion of an amount or a reference rate between two currencies | 0.005 USDC |
| [blackwood-x402-check](skills/blackwood-x402-check/SKILL.md) | you need an x402 endpoint test or validation of a paid API before integrating or listing it | 0.007 USDC |
| [blackwood-text-sentiment](skills/blackwood-text-sentiment/SKILL.md) | you need sentiment analysis of text such as reviews, comments or headlines in English | 0.004 USDC |
| [blackwood-fx-history](skills/blackwood-fx-history/SKILL.md) | you need historical exchange rates, forex history or the recent trend of a currency | 0.005 USDC |
| [blackwood-email-batch](skills/blackwood-email-batch/SKILL.md) | you need bulk email validation, email list verification or to verify email lists before a send or import | 0.02 USDC |
| [blackwood-ens-lookup](skills/blackwood-ens-lookup/SKILL.md) | you need to resolve an ENS name to an address, reverse-resolve an address to its ENS name, or read an ENS avatar and profile records | 0.005 USDC |
| [blackwood-gas-fees](skills/blackwood-gas-fees/SKILL.md) | you need a gas price or fee estimate before a transaction on Ethereum, Base or Solana, or to compare gas fees across chains | 0.003 USDC |
| [blackwood-wallet-portfolio](skills/blackwood-wallet-portfolio/SKILL.md) | you need a multi-chain wallet balance, a wallet portfolio or the recent transaction history of an address on Ethereum, Base or Solana | 0.005 USDC |
| [blackwood-news-briefing](skills/blackwood-news-briefing/SKILL.md) | you need a news briefing, a research summary of recent coverage or an analysis of what the press says about a topic or company | 0.04 USDC |
| [blackwood-company-dossier](skills/blackwood-company-dossier/SKILL.md) | you need company research, due diligence or a quick analysis of a business from its website domain and optional ticker | 0.03 USDC |
| [blackwood-wallet-value](skills/blackwood-wallet-value/SKILL.md) | you need a wallet portfolio value in USD, a wallet net worth or token balances with USD value for a 0x address | 0.005 USDC |

License: MIT. Contact: blackwoodstate8@gmail.com.
