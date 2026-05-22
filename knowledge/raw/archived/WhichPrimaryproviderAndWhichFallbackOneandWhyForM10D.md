For TradeForge specifically, I would strongly lean toward:

* Primary: Financial Modeling Prep
* Fallback: Alpha Vantage

Not because they are “the best providers” universally.

But because they are the best fit for *this architectural phase*.

---

# Why FMP Fits M10D Best

## 1. Broad capability surface

FMP naturally reinforces the architectural direction you are moving toward:

* price
* fundamentals
* financial statements
* ratios
* profiles
* earnings
* sector/industry
* insider data
* some news
* screening surfaces

That matters because M10D is fundamentally about:

```text id="8r2ajm"
provider capability differentiation
```

FMP makes that visible immediately.

---

# 2. The provider shape matches future M11 needs

You are not building:

* HFT infra
* institutional tick architecture
* latency-sensitive execution systems

You are building:

* replayable discretionary cognition
* contextual intelligence layers
* advisory overlays

FMP is well aligned with:

* contextual enrichment
* fundamentals overlays
* workflow cognition
* replay snapshots

---

# 3. JSON ergonomics matter

This is underrated.

FMP’s API structure is relatively AI-friendly and developer-friendly for:

* normalization
* typed adapters
* experimentation
* future Codex/Claude workflows

That matters in your ecosystem.

---

# Why Alpha Vantage Works Well As Fallback

Not because it is ideal.

But because it is meaningfully different.

That is useful architecturally.

It gives you:

* provider heterogeneity
* normalization pressure
* capability divergence
* fallback testing
* degraded capability behavior

without introducing massive complexity.

That is exactly what M10D should surface.

---

# Why I Would NOT Start With FinQual First

Not because FinQual is bad.

But because M10D is not yet about:

* premium intelligence quality
* institutional-grade data sophistication
* deep semantic enrichment

It is about:

```text id="qdyquh"
formalizing capability-aware provider architecture
```

You want providers that:

* expose capability diversity clearly
* are easy to integrate
* encourage typed boundaries
* surface normalization challenges early
* are operationally simple enough for M10D

FMP + Alpha Vantage gives you that.

---

# Another Important Point

The first provider pair should optimize for:

```text id="jlwm5z"
architectural learning
```

not:

* ultimate data quality
* maximum market coverage
* institutional completeness

Because later:

* provider replacement
* provider expansion
* capability addition

should become relatively cheap if M10D succeeds.

That is the whole point of the registry/capability architecture.

---

# What I Would Encode Explicitly In ADR-0038

I would add a small doctrine note somewhere like:

```text id="f7b1j0"
Initial provider selection optimizes for architectural capability validation rather than long-term provider finality.
```

That is important psychologically and architecturally.

Because otherwise people later start treating:

* FMP
* Alpha Vantage

as “blessed permanent providers.”

They are not.

They are:

* first capability-shaping providers
* normalization pressure generators
* architecture validation anchors

---

# The Hidden Win Of FMP + Alpha Vantage

This pair naturally forces you to solve:

* missing fields
* divergent semantics
* rate limiting
* inconsistent coverage
* degraded capability states
* fallback provenance
* replay capture

Those are precisely the problems M10D exists to expose *before* M11.

That is why I think it is the right pair.
