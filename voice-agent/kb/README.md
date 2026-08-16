# Knowledge Base documents

Upload these to the "Truenest Knowledge Data" knowledge base in the GHL
Voice AI builder (the panel shown in the screenshot from 2026-08-16).

## Maintenance troubleshooting (added first)
- `maintenance-ac-not-cooling.md`
- `maintenance-clogged-toilet.md`
- `maintenance-thermostat-unresponsive.md`

These mirror the three approved troubleshooting scripts that are also still
inline in `02-system-prompt.md`'s Maintenance flow (§3). That's intentional
for now, not a mistake — per the plan discussed with the client: keep the
content in both places for one round of real test calls to confirm Nora
actually retrieves the right document mid-call before removing it from the
prompt. Once that's confirmed, trim the inline copies out of the prompt so
future troubleshooting scripts can be added as new KB uploads instead of
prompt edits.

## Still to add
- Owner Pitch Talking Points (sales content for the Owner Lead flow)
- Any additional troubleshooting scripts as they're approved (garbage
  disposal, tripped breaker/dead outlet, smoke detector chirping, no hot
  water, garage/gate remote, fridge not cooling — see
  `06-actions-checklist.md` for the full candidate list)
