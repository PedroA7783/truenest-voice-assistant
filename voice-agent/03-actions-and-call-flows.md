# Actions & Call Flows (GHL Voice AI "Actions" tab)

This maps each caller track from `02-system-prompt.md` to the specific fields
to capture, the GHL action to fire, and the closing line. Configure the
**Actions** list first (below), then reference them by name in each flow.

## Actions to configure in GHL

| Action name | Type | Purpose | Notes |
|---|---|---|---|
| `Transfer – Leasing/PM` | Transfer Call | General leasing & owner-services line | Verify live number before publishing |
| `Transfer – Emergency Maintenance` | Transfer Call | 24/7 emergency line | Must be a number that's actually staffed 24/7 — confirm with client |
| `Book Showing` | Book Appointment | Schedule a property showing on the leasing calendar | Requires a GHL calendar for showings to exist first |
| `Book Owner Consult` | Book Appointment | Schedule a call with a BD/leasing specialist for a prospective owner | Separate calendar from showings |
| `Send SMS – Application Link` | Send SMS | Texts the rental application link | |
| `Send SMS – Rate Sheet` | Send SMS | Texts current management fee/pricing info | Avoids the agent stating numbers verbally |
| `Send SMS – Tenant Portal` | Send SMS | Texts the resident portal login link | |
| `Update Contact / Create Opportunity` | Custom Webhook or native CRM action | Writes structured lead data (track, property, contact info, notes) into the pipeline | Tag by track so leasing vs. maintenance vs. owner-BD land in the right pipeline stage |
| `End Call` | End Call | Graceful close | Always preceded by the "what happens next" line |

## Flow 1 — Prospective tenant

1. Confirm which listing/area they're interested in and desired move-in
   timeframe.
2. Ask household size and whether they have pets (for screening/pet-policy
   context only — never phrase as a suitability judgment).
3. Explain screening applies to every occupant 18+: credit, criminal, rental
   history, employment verification. State typical time-to-lease is 2-4 weeks,
   varies by property.
4. Offer next step: `Book Showing` if a specific property/time is known, else
   `Send SMS – Application Link` plus availability listing link if one exists.
5. Fire `Update Contact / Create Opportunity` tagged "Prospective Tenant."
6. Close: confirm what was sent/booked and that the leasing team follows up
   within 1 business day if anything's outstanding. `End Call`.

## Flow 2 — Current tenant

1. **Emergency check first** (see system prompt). If emergency criteria met:
   say escalation is happening now → `Transfer – Emergency Maintenance`
   immediately, skip the rest of this flow.
2. If not an emergency, identify the request type: maintenance (non-urgent),
   payment/portal question, lease question, or general.
3. **Maintenance (non-urgent):** capture issue description, location in unit,
   access instructions (pets, gate codes), and preferred contact method. Fire
   `Update Contact / Create Opportunity` tagged "Maintenance Request" with
   those fields in the notes. Tell them the maintenance coordinator will
   follow up, typical window 1-2 business days for non-urgent items.
4. **Payment/portal:** `Send SMS – Tenant Portal`. If they describe a payment
   dispute or hardship, don't attempt to resolve it — offer `Transfer –
   Leasing/PM` or take a message for the property manager.
5. **Lease question / anything needing judgment:** take a brief note of the
   question, offer `Transfer – Leasing/PM` if available now, otherwise log via
   `Update Contact / Create Opportunity` tagged "Tenant Inquiry" for callback
   within 1 business day.
6. Close with the "what happens next" line. `End Call`.

## Flow 3 — Prospective owner

1. Capture property basics: address or area, property type (single-family,
   townhome, condo, small multifamily — flag if over 9 units, since that may
   be outside their typical portfolio and should be confirmed by a human, not
   declined by the agent), current occupancy status (vacant, tenanted,
   owner-occupied), and timeline.
2. Briefly state the core service (full-service management: marketing,
   screening, rent collection, 24/7 maintenance coordination, inspections,
   financial reporting, eviction assistance if needed) in one or two
   sentences — don't recite the whole list, offer to send it.
3. Never state a fee percentage. Offer `Send SMS – Rate Sheet` and/or
   `Book Owner Consult`.
4. Fire `Update Contact / Create Opportunity` tagged "Prospective Owner" with
   all captured property details — this is the highest-value lead type, make
   sure nothing is dropped.
5. Close confirming what was sent/booked and the callback window. `End Call`.

## Flow 4 — Current owner

1. Identify the topic: financial statement/portal question, maintenance
   update on their property, a new request (e.g., rent adjustment, lease
   renewal decision), or general.
2. Portal/statement questions: confirm they have portal access; if not,
   `Send SMS – Tenant Portal` (or owner-portal equivalent link if a separate
   one exists — confirm with client) and note it for follow-up.
3. Anything requiring a decision or that references a specific tenant/lease:
   don't improvise — offer `Transfer – Leasing/PM` if available, otherwise
   log via `Update Contact / Create Opportunity` tagged "Owner Inquiry" for
   callback within 1 business day.
4. Close with the "what happens next" line. `End Call`.

## Fallback — no live transfer available (after hours / all lines busy)

- If the call was flagged emergency and no transfer connects: give the
  caller explicit after-hours emergency instructions/number rather than a
  generic callback promise. (Requires the client to confirm this number
  exists and is monitored — don't fabricate one.)
- Otherwise: take a structured message (name, number, property/unit if
  applicable, reason, urgency) via `Update Contact / Create Opportunity`,
  confirm a callback window, `End Call`.
