# Changelog

All notable changes to RippleLogic Guardrails will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [0.1] - 2026-02-01

### Added
- Initial public release of RippleLogic Guardrails spec pack
- Complete specification document (RippleLogic-Guardrails-v0.1.md) including:
  - Classification schema (capability class, data sensitivity, blast radius, reversibility)
  - Guardrails cascade (rights floor, tail-risk bounds, containment, auditability)
  - Standard constraints library
  - Gate decision format
  - Hard-block list
  - Minimal implementation contract
  - Compliance claim requirements
- OnePageQuickStart.md – Four-step implementation guide
- AuditLogSchema.json – JSON schema for audit log entries
- SkillRiskLabelSchema.json – JSON schema for skill risk labels
- TestScenarios.md – Comprehensive test scenarios covering low-risk, constrained, high-risk, and blocked actions
- SHA256SUMS.txt – Integrity checksums for all files
- MIT License
- Documentation and README

### Notes
- This is the initial version defining the minimum viable guardrails layer
- Designed to be platform-agnostic and portable across any agent system
- Focus on rights-bounded, tail-risk bounded, containment-aware, and auditable operations
