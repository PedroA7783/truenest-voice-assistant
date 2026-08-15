# GHL Actions checklist — what the current prompt (02) assumes exists

Checked against a screenshot of the live builder's Actions tab (2026-08-15).

## Already configured
- Appointment Booking — confirm it's pointed at the calendar Pedro actually
  uses for new-owner consultations; if showings (Prospective Tenant flow)
  use a separate calendar, that needs its own Appointment Booking action too
- Update contact field — Extract First Name
- Update contact field — Extract Last Name
- Update contact field — Extract Email
- Update contact field — Extract Phone

## Missing — referenced by name in the prompt, not yet visible in the builder

| Action to add | Type | Used by |
|---|---|---|
| Transfer to Pedro | Transfer Call | Owner Lead (§1) — only when the caller insists on talking to him live, not the default path |
| Transfer to Roy | Transfer Call | Maintenance (§3) |
| Transfer to Christine | Transfer Call | Screening (§5) |
| Emergency Transfer | Transfer Call | Universal Emergency Protocol — should ring a different/priority destination than "Transfer to Roy," not reuse it |
| Emergency Escalation Webhook/SMS alert | Custom Webhook or internal notification | Emergency fallback when the transfer above fails — needs to carry the property address and issue description captured right before it fires |
| Send SMS – Process & Consultation links | Send SMS | Owner Lead fallback, Pricing & Fees compliance rule |
| Send SMS – Tenant Portal link | Send SMS | Maintenance fallback, all of Tenant Support (§4) |
| Send SMS – Showing/Application link | Send SMS | Prospective Tenant (§6) |
| Update Contact / Tag by intent | Update Contact / Workflow trigger | Intent Classification & Tagging — distinct from the field-extraction actions already configured; nothing currently applies the `Owner Lead` / `Current Owner` / `Maintenance` / `Tenant Support` / `Screening` / `Prospective Tenant` tags |

## Design notes
- **Emergency Transfer should not point at the same number as "Transfer to
  Roy."** If it does, a failed emergency transfer and a failed routine
  maintenance transfer look identical to the fallback logic, and the
  escalation webhook won't know to treat it as urgent.
- The three SMS actions could be one action with dynamic link content
  instead of three separate ones, depending on how the GHL plan handles
  variables in SMS templates — either works, it's a builder-convenience
  choice, not a behavior difference.
- There's no dedicated 24/7 emergency line today (confirmed by the client),
  so the Emergency Escalation Webhook/SMS alert is doing the job a real
  after-hours line would otherwise do — see `02-system-prompt.md`'s
  "Still needs a setup step" section.
