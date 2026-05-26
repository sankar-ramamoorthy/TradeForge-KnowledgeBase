
● TF-F069 is logged. Here's a summary of what was recorded:

  - ID: TF-F069
  - Title: Provider Governance UI Broken — JSON Parse Failure On Load
  - Status: Planned / M14
  - Branch: fix/tf-f069-provider-governance-json-parse
  - Root cause (suspected): The provider governance API endpoint is returning a non-JSON response (HTML error page,
  empty body, or unmounted route) — confirmed by the JSON.parse: unexpected character at line 1 column 1 console error.
  The UI degrades silently to a skeleton state ("three lines").
  - Acceptance criteria: JSON parse error gone, content loads, explicit error state if API fails, Content-Type:
  application/json on all governance endpoints, existing tests still pass.