  ---
  title: LiteLLM Advisory Routing Test Gap
  type: brainstorm
  status: raw
  tags: [TradeForge, brainstorm, litellm, advisory, ollama, provider-governance, frontend-proxy]
  created: 2026-05-24
  ---

  # LiteLLM Advisory Routing Test Gap

  ## Trigger

  While testing TradeForge's LiteLLM integration, the operator could navigate through the app and create a new ANET trade idea, but
  could not clearly tell whether Ollama or any LLM route was actually being called. Groq showed no activity for the day, and the Plan
  Review Workspace displayed a JSON parse error in the advisory/non-canonical interpretation panel.

  ## Raw Input

  The operator started the Docker Compose stack with backend, Postgres, and LiteLLM running in containers while the frontend ran
  outside Docker through Vite.

  Local Ollama was verified directly from PowerShell with:

  ```powershell
  Invoke-RestMethod -Method Post `
    -Uri http://localhost:11434/api/generate `
    -ContentType "application/json" `
    -Body '{
      "model": "granite4:350m",
      "prompt": "Why is the sky grey?",
      "stream": false
    }'

  Ollama responded successfully.

  LiteLLM initially rejected a raw model call:

  Invalid model name passed in model=ollama/granite4:350m.
  Call /v1/models to view available models for your key.

  Diagnosis showed LiteLLM only exposed configured aliases. litellm_config.yaml was updated so:

  tradeforge-ollama -> ollama/granite4:350m

  After restarting LiteLLM, a PowerShell call to LiteLLM using model = "tradeforge-ollama" succeeded and returned token usage.

  The TradeForge UI credential was configured as:

  base_url: http://litellm:4000
  api_key: sk-tradeforge-local-dev
  default_model: tradeforge-ollama

  The operator then created a new ANET trade idea, entered a thesis, and reached Plan Review Workspace. The page showed:

  Advisory / Non-canonical
  Evidence influence and caveats
  JSON.parse: unexpected character at line 1 column 1 of the JSON data

  No accepted interpretations are linked to this context yet.

  ## Observations

  - LiteLLM container is running and reachable on host port 4000.
  - LiteLLM readiness endpoint returns healthy.
  - LiteLLM exposes configured aliases including tradeforge-ollama.
  - LiteLLM -> Ollama works when called directly through the alias.
  - TradeForge backend is running in Docker Compose, so its LiteLLM credential must use http://litellm:4000.
  - Frontend runs outside Docker through Vite on port 5173.
  - Creating a trade idea and thesis does not automatically call the LLM.
  - TradeForge logs for the ANET flow showed lifecycle and workspace requests, but no /advisory/thesis-review, /advisory/generate-
    observations, /advisory/replay-summary, or other AI generation endpoint.
  - LiteLLM logs showed no new /v1/chat/completions call attributable to the ANET workflow.
  - The JSON parse error appears to come from the frontend calling /advisory/interpretations, but frontend/vite.config.ts does not
    currently proxy /advisory to the backend.
  - Because /advisory is not proxied, Vite likely returns the React HTML shell, and the frontend attempts to parse HTML as JSON.

  ## Ideas

  - Add /advisory to the Vite dev proxy table so advisory API calls reach the backend during local frontend development.
  - Add a clear operator-facing smoke test for LiteLLM routes, ideally separate from lifecycle workflows.
  - Add a simple "send test prompt" action in provider governance or AI gateway configuration that sends a non-canonical diagnostic
    message through the configured LiteLLM route.
  - Keep the test prompt explicitly operational/diagnostic and not tied to any decision lifecycle artifact.
  - Surface the selected gateway URL, default model alias, and last route test result in the provider governance UI.
  - Consider logging or displaying "last advisory provider call" metadata for diagnostics without promoting it into canonical event
    truth.

  ## Questions

  - Should TradeForge include an explicit AI gateway test endpoint, or should route testing remain outside the app through documented
    PowerShell/curl commands?
  - If a route test endpoint is added, should it call LiteLLM /v1/chat/completions with a tiny fixed diagnostic prompt, or use a
    lighter provider-specific probe?
  - Should successful/failed route tests be session-only operational diagnostics, retained local diagnostic history, or simply
    displayed as immediate UI feedback?
  - Should the Plan Review Workspace include an explicit "Generate thesis review" action if one does not already exist, instead of
    implying advisory content may appear automatically?
  - Should the frontend show non-JSON API failures with a clearer diagnostic message instead of raw JSON.parse failures?

  ## Concerns

  - Route tests must not create canonical decision events.
  - AI output must remain advisory and must not approve, reject, or transition lifecycle state.
  - Provider diagnostics should not become hidden state that changes workflow behavior.
  - Automatic advisory generation during lifecycle transitions would need separate semantic approval and issue scope.
  - The current JSON parse error obscures the real failure mode and makes it look like an LLM failure when it is likely a frontend
    proxy/API response issue.

  ## Possible Next Outputs

  - Issue candidate
  - Frontend proxy fix
  - Provider governance diagnostic workflow issue
  - AI gateway smoke-test endpoint issue
  - Documentation update for local LiteLLM/Ollama testing
  
  # logging
  Test1. from powershell.
  
   PS C:\Users\bosto\dockerstuff\TradeForge> Invoke-RestMethod -Method Post `
  >>   -Uri http://localhost:11434/api/generate `
  >>   -ContentType "application/json" `
  >>   -Body '{
  >>     "model": "granite4:350m",
  >>     "prompt": "Why is the sky grey?",
  >>     "stream": false
  >>   }'


  model                : granite4:350m
  created_at           : 2026-05-24T18:30:45.2576728Z
  response             : The sky appears gray due to several factors:

                         1. Absorption: The Earth's atmosphere absorbs light from the sun, particularly blue light which
                         tends to be more intense than red light. This me



Test 2. 
  PS C:\Users\bosto\dockerstuff\TradeForge> docker compose --profile advisory restart litellm
[+] restart 0/1
 - Container tradeforge-litellm-1 Restarting                                                                        2.7s
PS C:\Users\bosto\dockerstuff\TradeForge>  $env:LITELLM_MASTER_KEY = "sk-tradeforge-local-dev"
PS C:\Users\bosto\dockerstuff\TradeForge>
PS C:\Users\bosto\dockerstuff\TradeForge>   $body = @{
>>     model = "tradeforge-ollama"
>>     messages = @(
>>       @{
>>         role = "user"
>>         content = "Reply with exactly: tradeforge ollama alias ok"
>>       }
>>     )
>>     stream = $false
>>     max_tokens = 50
>>   } | ConvertTo-Json -Depth 5
PS C:\Users\bosto\dockerstuff\TradeForge>
PS C:\Users\bosto\dockerstuff\TradeForge>   Invoke-RestMethod `
>>     -Method Post `
>>     -Uri "http://localhost:4000/v1/chat/completions" `
>>     -Headers @{ Authorization = "Bearer $env:LITELLM_MASTER_KEY" } `
>>     -ContentType "application/json" `
>>     -Body $body


id      : chatcmpl-c8d178bc-bfa0-46b8-ad96-d8e4be6d2251
created : 1779648426
model   : tradeforge-ollama
object  : chat.completion
choices : {@{finish_reason=stop; index=0; message=}}
usage   : @{completion_tokens=50; prompt_tokens=46; total_tokens=96}