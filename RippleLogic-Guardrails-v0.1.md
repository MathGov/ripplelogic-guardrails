# RippleLogic Guardrails v0.1 (Spec Pack)

Author: James (RippleLogic)
Version: v0.1
Status: Public Draft
Scope: Platform-agnostic guardrails for tool-using AI agents

## 1) Purpose
Tool-using AI agents can read, write, execute, message, publish, and transact. Guardrails are the minimum governance layer that ensures agents remain:
- rights-bounded
- tail-risk bounded
- containment-aware
- auditable

This spec defines a portable “minimum viable guardrails layer” that can be implemented in any agent system.

## 2) Core outputs (the three decisions)
Every intended tool action MUST produce one of:

1) ALLOW  
Action may proceed.

2) ALLOW_WITH_CONSTRAINTS  
Action may proceed only if listed constraints are satisfied.

3) BLOCK  
Action must not proceed. Provide a safe alternative.

## 3) Definitions

### 3.1 Action
An Action is any step that could invoke a tool, change state, access data, or affect external systems.

Examples:
- run a command
- write a file
- send a message
- post publicly
- log into a website
- make a purchase
- call an API

### 3.2 Capability class (required)
Each Action MUST be classified by capability class (one or more):
- filesystem_read
- filesystem_write
- shell_exec
- browser_control
- network_call
- credentials_access
- publish_share_upload
- messaging_send
- payments_transactions
- local_compute
- unknown

### 3.3 Data sensitivity (required)
Each Action MUST be classified by the highest data sensitivity involved:
- public
- personal
- secrets
- regulated
- unknown

Notes:
- secrets includes passwords, API keys, tokens, private keys, session cookies
- regulated includes health, finance, minors data, government IDs, or locally regulated categories

### 3.4 Blast radius (required)
Estimate the worst-case scope of harm if the action goes wrong:
- low (localized, easily contained)
- medium (affects multiple files/accounts/people)
- high (system-wide, public exposure, irreversible distribution, or major financial harm)
- unknown

### 3.5 Reversibility (required)
Estimate how easy it is to undo:
- easy (simple undo/rollback)
- hard (requires complex recovery)
- irreversible (cannot reliably be undone once done)
- unknown

### 3.6 Consent and authorization
Actions that affect other people, their accounts, their data, or their autonomy require explicit consent and proper authorization. Lack of consent or authorization triggers BLOCK.

## 4) RippleLogic Guardrails Cascade (minimal)
This is a minimal version of a rights-first cascade.

### Step A: Rights floor (non-compensatory)
BLOCK if the action would:
- exfiltrate secrets or credentials
- violate privacy or consent
- enable impersonation, surveillance, or coercion
- create unauthorized access, escalation, or bypass authentication
- cause physical harm or credible safety risk

### Step B: Tail-risk bound (high-blast or irreversible)
If blast_radius is high OR reversibility is irreversible:
- default to ALLOW_WITH_CONSTRAINTS
- require containment and stepwise execution
- require explicit confirmation for any state change

### Step C: Containment requirements
For high-risk capabilities (shell_exec, browser_control, credentials_access, payments_transactions):
- require sandbox or restricted environment when possible
- require explicit confirmation
- require previews or dry runs when possible

### Step D: Auditability requirement
Any Action that triggers tools MUST create an audit entry. If the Action is BLOCKED, an audit entry MUST still be created when the request was high risk.

### Step E: Unknowns
If classification is unknown in any required dimension:
- default to ALLOW_WITH_CONSTRAINTS
- ask a clarifying question
- do not execute irreversible actions

## 5) Standard constraints library (v0.1)
When returning ALLOW_WITH_CONSTRAINTS, constraints MUST be explicit. Use these canonical strings:

Consent and confirmation
- explicit_user_confirmation_required
- confirm_destination_and_scope
- confirm_identity_and_authorization

Containment
- sandbox_required
- restricted_permissions_only
- no_credential_access

Scope control
- narrow_scope
- stepwise_execution
- limit_to_allowlisted_paths

Previews
- show_commands_before_exec
- dry_run_first_if_possible
- preview_diff_before_write
- summarize_changes_before_apply

Data protection
- run_sensitive_data_check
- redact_secrets
- do_not_log_sensitive_content
- do_not_publish_private_data

Commerce and accounts
- no_payments_without_confirmation
- no_login_without_confirmation
- no_account_changes_without_confirmation

## 6) Required gate decision format (portable)
Any implementation MUST be able to output:

GATE_ACTION: <one sentence description>  
CLASSIFICATION:  
  capability: [..]  
  data_sensitivity: <..>  
  blast_radius: <..>  
  reversibility: <..>  
GATE_DECISION: <ALLOW | ALLOW_WITH_CONSTRAINTS | BLOCK>  
REASON: <short>  
CONSTRAINTS: <list or NONE>  
NEXT: <one question or next step>

## 7) Hard-block list (v0.1)
Always BLOCK:
- sharing or transmitting credentials/secrets
- disabling authentication or exposing a private control plane publicly
- installing unknown code in a privileged context without containment and review
- impersonation, unauthorized surveillance, stalking, coercion
- actions that are illegal or clearly violate rights or safety

## 8) Minimal implementation contract
Any “Guardrails-compliant” system MUST implement:

1) classify(action) -> classification object  
2) gate(action, policy) -> decision object  
3) enforce_constraints(decision, action) -> execution plan  
4) audit(event) -> append-only record  

Optional:
5) label_skill(skill_metadata) -> risk label

## 9) Claim of compliance (what you may claim)
If your system:
- classifies actions,
- gates every tool call,
- blocks hard-block items,
- constrains high-blast or irreversible actions,
- and writes audit logs,

then you may claim:
"Implements RippleLogic Guardrails v0.1 (portable spec)"

## 10) Non-goals (v0.1)
This spec does not attempt to:
- prove full security against adversaries
- replace OS sandboxing, secret vaults, and network segmentation
- define a full legal compliance system
- guarantee perfect ethical reasoning

It defines the minimum governance layer that makes agent actions reviewable, bounded, and safer by default.
