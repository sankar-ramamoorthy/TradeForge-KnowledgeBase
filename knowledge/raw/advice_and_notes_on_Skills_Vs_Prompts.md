Notes on Skills vs prompts


skills/
    behavioral/domain rules

prompts/
    executable operational workflows
	

skills/

These are:

behavior modifiers
reasoning constraints
cognitive overlays
domain operating rules

Examples:

decision-lifecycle
event-sourcing
execution-discipline
market-interpreting
scenario-ranking
workspace-querying

These are NOT task prompts.

They are:

semantic operating behaviors
architectural reasoning constraints
cognition modifiers

That is excellent.

Keep them in:

knowledge-base/TradeForge/skills/	


prompts/

These are:

task-oriented operational entrypoints

Examples:

GENERATE_ADRS
build-vertical-slice
replay-analysis

These are:

reusable workflows
operational procedures
structured Codex tasks

These ARE prompts.

So they belong in:

knowledge-base/TradeForge/prompts/


The Important Realization

You do NOT want to merge them.

Because conceptually they are different layers:

SKILLS
    = how Codex should think

PROMPTS
    = what Codex should do

That is actually a very strong separation.

Notice:

skills/
    reusable thinking constraints

prompts/
    reusable operational workflows
	
	