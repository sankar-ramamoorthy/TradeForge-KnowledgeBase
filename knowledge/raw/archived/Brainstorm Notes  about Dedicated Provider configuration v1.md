Yes — fix the credential UX/design first. 

I would design this as a **dedicated Provider Configuration workspace**, not a right-rail form.

Core design:

```text
Settings / Provider Configuration

1. Provider Registry
   - yfinance
   - fmp
   - alpha_vantage
   - polygon
   - alpaca
   - litellm

2. Capability Matrix
   Provider        Price   Fundamentals   Broker/Paper   AI Models
   yfinance        yes     partial         no             no
   fmp             no/yes  yes             no             no
   alpaca          yes     no              yes            no
   litellm         no      no              no             yes

3. Credentials
   - Provider name
   - Credential status: configured / missing / invalid / untested
   - Add / rotate / delete key
   - Test connection
   - Last tested timestamp
   - Error message if failed

4. Provider Selection
   Per capability:
   - Price provider: yfinance
   - Fundamentals provider: fmp
   - Fundamentals fallback: alpha_vantage
   - Paper broker: alpaca
   - AI gateway: litellm

5. LiteLLM Model Routing
   Model Provider     Model Alias       API Key Source       Enabled
   OpenAI             gpt-4.1            openai_key          yes
   Anthropic          claude-sonnet      anthropic_key       yes
   Groq               llama/qwen         groq_key            yes
   Ollama             local models       no key              yes
```

The key distinction:

```text
Credential != Provider != Capability != Model
```

That matters a lot.

For example:

```text
LiteLLM is not one provider.
LiteLLM is a gateway.

Behind LiteLLM you may have:
- OpenAI key
- Anthropic key
- Groq key
- Gemini key
- local Ollama with no key
```

So TradeForge should not treat `litellm` like `fmp`.

Better mental model:

```text
Provider Configuration
├── Market Data Providers
│   ├── yfinance
│   ├── fmp
│   ├── alpha_vantage
│   └── polygon
├── Broker Providers
│   └── alpaca
└── AI Gateway Providers
    └── litellm
        ├── openai
        ├── anthropic
        ├── groq
        ├── gemini
        └── ollama
```

For LiteLLM specifically, I would want:

```text
LiteLLM Configuration

Base URL:
http://localhost:4000

Health:
reachable / unreachable

Available model aliases:
- tradeforge-fast
- tradeforge-reasoning
- tradeforge-local
- tradeforge-cheap

Routes:
tradeforge-fast      -> groq / llama-3.3
tradeforge-reasoning -> openai / gpt-4.1 or claude
tradeforge-local     -> ollama / qwen
tradeforge-cheap     -> gemini/groq/etc

Usage role:
- summarization
- thesis critique
- risk critique
- evidence extraction
- plan review assistant
```

TradeForge should choose **roles**, not raw model names everywhere.

Example:

```text
AI Role: Thesis Critic
Current route: tradeforge-reasoning
Gateway: LiteLLM
Underlying provider: OpenAI / Anthropic / Groq
```

That lets you change providers without changing TradeForge logic.

For the UI, the right rail should only show status:

```text
Provider Status

Fundamentals:
FMP selected
Credential configured
Last test: success

AI:
LiteLLM reachable
Default role: tradeforge-reasoning
Health: ok

Warnings:
Alpha Vantage fallback missing key
```

Clicking “Configure” should take you to the full Provider Configuration workspace.

And for your immediate bug: when “Add credential” does nothing, I would create an issue before code:

```text
TF-F0xx: Implement dedicated Provider Configuration workspace

Problem:
Credential entry is currently embedded in the contextual rail. Saving a provider API key gives no visible confirmation, failure, validation state, or test result. LiteLLM also requires multi-provider/model routing support that does not fit the current one-key-per-provider mental model.

Scope:
- Add Provider Configuration workspace
- Separate credentials, provider selection, capability routing, and LiteLLM model routing
- Add save/test feedback
- Show configured/missing/invalid states
- Keep right rail read-only/contextual except for link to configure
- Support multiple API keys behind LiteLLM
- Support model aliases/roles rather than hard-coded raw models

Out of scope:
- Actual AI advisory behavior
- Trading automation
- Broker order placement
```

My honest view: this is not a small UI tweak. This is a real architectural UX boundary. You found the right gap.
