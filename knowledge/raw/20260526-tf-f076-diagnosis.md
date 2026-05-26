---
title: TF-F076 LiteLLM Healthcheck Route-Probing Diagnosis
type: raw
status: raw
tags: [tradeforge, tf-f076, m13b, litellm, healthcheck, advisory-runtime, diagnostics]
created: 2026-05-26
---

# TF-F076 LiteLLM Healthcheck Route-Probing Diagnosis

## Trigger

Runtime logs from the LiteLLM container showed provider attempts against Groq, Gemini, OpenAI, and Anthropic routes while the managed advisory path was being validated through NVIDIA NIM.

## Finding

The Docker Compose healthcheck for LiteLLM called `/health`. LiteLLM documentation describes `/health` as model health monitoring for all configured models and states that it makes real LLM API calls. The same documentation describes `/health/readiness` as the endpoint for checking whether the proxy is ready to accept requests.

Because TradeForge uses stateless request-time credential composition, Docker readiness must not validate every configured provider route. Provider/model validation should occur only through explicit TradeForge-mediated diagnostics, smoke tests, or advisory requests.

## Architectural Interpretation

This is an operational infrastructure defect, not a semantic or domain-model defect. The problem sits at the Docker healthcheck boundary. It does not require changes to lifecycle state, event schema, credential schema, provider model selection, or frontend behavior.

## Constraint

Do not add provider API keys to LiteLLM environment merely to satisfy health checks. That would hide the route-probing behavior and weaken the managed advisory credential boundary.
