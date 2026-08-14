# TrueNest Voice Assistant — GoHighLevel Voice AI Audit & Redesign

This repo holds a property-management-specific audit and rebuild of the
GoHighLevel Voice AI agent for TrueNest Property Management
(truenestpm.com), a residential property manager in Broward County, FL.

**Read this first:** this session had no way to log into the GoHighLevel
voice agent builder (it needs your authenticated GHL session; no API
credentials or connector were available here), so nothing in the live agent
was changed directly. What's here is a complete, ready-to-paste replacement
built from public research on the business — apply it yourself in the
builder using `voice-agent/04-ghl-setup-steps.md`, or paste your current
agent's prompt/actions into a message and I'll diff against this instead.

## Contents

- `voice-agent/00-business-profile.md` — researched facts about TrueNest,
  with confidence flags on anything unverified (phone number, exact pricing)
- `voice-agent/01-audit-findings.md` — what's wrong with a generic voice
  agent for a property-management use case, and why
- `voice-agent/02-system-prompt.md` — the full recommended Personality/Prompt
  field, ready to paste into GHL
- `voice-agent/03-actions-and-call-flows.md` — the Actions to configure and
  the per-caller-type call flow logic
- `voice-agent/04-ghl-setup-steps.md` — step-by-step instructions for
  applying this in the actual GHL builder
- `voice-agent/05-qa-test-script.md` — test calls to run before publishing,
  including Fair Housing compliance traps and emergency-maintenance handling

Start with `01-audit-findings.md`.
