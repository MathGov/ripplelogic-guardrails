# RippleLogic Guardrails v0.1 Quick Start (1 page)

If you are building a tool-using agent, implement these four steps.

## Step 1: Classify the intended action
Before tools, produce:
- capability class (filesystem_write, shell_exec, publish_share_upload, etc.)
- data_sensitivity (public, personal, secrets, regulated)
- blast_radius (low, medium, high)
- reversibility (easy, hard, irreversible)

## Step 2: Gate it (three decisions)
Return one of:
- ALLOW
- ALLOW_WITH_CONSTRAINTS
- BLOCK

Apply:
A) Rights floor hard-blocks
B) Tail-risk constraints for high blast or irreversible actions
C) Containment for shell/browser/credentials/payments
D) Default unknowns to constraints + ask clarifying question

## Step 3: Enforce constraints
If ALLOW_WITH_CONSTRAINTS:
- ask for explicit confirmation if required
- sandbox high-risk capabilities
- preview changes before applying
- run sensitive-data check before publishing

## Step 4: Write an audit entry
If tools were used, append an audit JSON object to a JSONL log.
If a high-risk request was blocked, log that too.
Never log secrets.

Minimum claim you can make if you do the above:
"Implements RippleLogic Guardrails v0.1 (portable spec)"
