# RippleLogic Guardrails v0.1

**Portable minimum governance layer for tool-using AI agents**

## What This Is

RippleLogic Guardrails v0.1 is a platform-agnostic specification that defines the minimum governance layer for AI agents with tool access. It ensures agents remain:

- **Rights-bounded** – respects privacy, consent, and authorization
- **Tail-risk bounded** – constrains high-blast and irreversible actions
- **Containment-aware** – sandboxes high-risk capabilities
- **Auditable** – logs all tool usage and decisions

Every intended tool action produces one of three gate decisions:
- **ALLOW** – action may proceed
- **ALLOW_WITH_CONSTRAINTS** – action may proceed only if constraints are satisfied
- **BLOCK** – action must not proceed

## Included Files

This spec pack contains:

- **RippleLogic-Guardrails-v0.1.md** – Complete specification with classification schema, guardrails cascade, constraints library, and compliance requirements
- **OnePageQuickStart.md** – Four-step implementation guide (1 page)
- **AuditLogSchema.json** – JSON schema for audit log entries
- **SkillRiskLabelSchema.json** – JSON schema for skill risk labels
- **TestScenarios.md** – Test scenarios covering low-risk, constrained, high-risk, and blocked actions
- **SHA256SUMS.txt** – Integrity checksums for all files

## Download

### GitHub Release (Canonical)
- **Latest Release**: [v0.1](https://github.com/MathGov/ripplelogic-guardrails/releases/tag/v0.1)
- **ZIP Package**: Download from GitHub Releases

### Website Mirror
- **WordPress Pack**: [https://ripplelogic.org/wp-content/uploads/2026/02/RippleLogic-Guardrails-v0.1.zip](https://ripplelogic.org/wp-content/uploads/2026/02/RippleLogic-Guardrails-v0.1.zip)

## Integrity Verification

All files include SHA-256 checksums in `SHA256SUMS.txt`.

### Verify ZIP Package (Windows PowerShell)
```powershell
Get-FileHash .\RippleLogic-Guardrails-v0.1.zip -Algorithm SHA256
```

### Verify ZIP Package (macOS/Linux)
```bash
shasum -a 256 RippleLogic-Guardrails-v0.1.zip
```

Compare the output to the corresponding line in `SHA256SUMS.txt`. The hash should match exactly.

### Verify Individual Files
```bash
# macOS/Linux
shasum -a 256 -c SHA256SUMS.txt

# Windows PowerShell
Get-Content SHA256SUMS.txt | ForEach-Object {
    $hash, $file = $_ -split '  '
    $computed = (Get-FileHash $file -Algorithm SHA256).Hash
    if ($computed -eq $hash) { "$file OK" } else { "$file FAILED" }
}
```

## Quick Start

If you are building a tool-using agent:

### Step 1: Classify the intended action
Before invoking tools, classify by:
- **capability class** (filesystem_write, shell_exec, publish_share_upload, etc.)
- **data_sensitivity** (public, personal, secrets, regulated)
- **blast_radius** (low, medium, high)
- **reversibility** (easy, hard, irreversible)

### Step 2: Gate it (three decisions)
Return one of: **ALLOW**, **ALLOW_WITH_CONSTRAINTS**, or **BLOCK**

Apply:
- **A)** Rights floor hard-blocks (no credential sharing, no unauthorized access, no surveillance)
- **B)** Tail-risk constraints for high blast or irreversible actions
- **C)** Containment for shell/browser/credentials/payments
- **D)** Default unknowns to constraints + ask clarifying question

### Step 3: Enforce constraints
If ALLOW_WITH_CONSTRAINTS:
- Ask for explicit confirmation if required
- Sandbox high-risk capabilities
- Preview changes before applying
- Run sensitive-data check before publishing

### Step 4: Write an audit entry
If tools were used, append an audit JSON object to a JSONL log.  
If a high-risk request was blocked, log that too.  
**Never log secrets.**

## Compliance Claim

If your system:
- Classifies actions
- Gates every tool call
- Blocks hard-block items (credential sharing, unauthorized access, surveillance, etc.)
- Constrains high-blast or irreversible actions
- Writes audit logs

Then you may claim:

**"Implements RippleLogic Guardrails v0.1 (portable spec)"**

## Versioning

- **v0.1** – Initial public release (February 2026)
- Future updates will follow semantic versioning: v0.1.1, v0.2, v1.0, etc.

## Non-Goals

This spec does not attempt to:
- Prove full security against adversaries
- Replace OS sandboxing, secret vaults, and network segmentation
- Define a full legal compliance system
- Guarantee perfect ethical reasoning

It defines the **minimum governance layer** that makes agent actions reviewable, bounded, and safer by default.

## Links

- **RippleLogic**: [https://ripplelogic.org](https://ripplelogic.org)
- **MathGov**: [https://mathgov.org](https://mathgov.org)
- **GitHub Repository**: [https://github.com/MathGov/ripplelogic-guardrails](https://github.com/MathGov/ripplelogic-guardrails)

## License

MIT License

Copyright (c) 2026 James (RippleLogic)

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.

---

**Author**: James (RippleLogic)  
**Version**: v0.1  
**Status**: Public Draft  
**Scope**: Platform-agnostic guardrails for tool-using AI agents
