# GHL Build Spec — exact configuration for every action

Matched against the current prompt (`02-system-prompt.md`) and the
Knowledge Base docs in `voice-agent/kb/`. `{{ }}` marks a value still
needed.

## Team contacts (confirmed)

| Name | Role | Phone | Email |
|---|---|---|---|
| Pedro Andrade | Business Development (Owner Lead calls) **and** one of three leasing agents (Prospective Tenant calls) | 954-406-0400 | pedro@truenestpm.com |
| Roy Maldonado | Maintenance Lead (sole emergency contact) | +1 954-278-7193 | roy@truenestpm.com |
| Christine | Screening | +1 954-361-9361 | christine@truenestpm.com |
| Diego Binetti | Owner of TrueNest — handles Current Owner payment/ops questions | +1 954-787-7077 | diego@truenestpm.com |
| Scharisse Moreno | Leasing agent | +1 754-316-9815 | scharisse@truenestpm.com |
| Noel Cuarezma | Leasing agent | +1 754-777-0353 | noelsellsrealestate@gmail.com |

Note on spelling: "Christine" used throughout (was written "Cristine"
once) — flag if that's actually wrong.

**Hard rule, confirmed by the client: Pedro is never looped into
maintenance or repairs** — no notifications, no reports, no escalation
path through him for anything maintenance-related. Emergency escalation
goes to Roy only.

**Diego's role, confirmed:** transferred (or offered his check-in link,
https://calendly.com/truenest/check-in, as fallback) specifically when
existing owners call about payments or operations questions on their
properties. Not wired into anything else.

**Leasing agents, confirmed:** Scharisse, Noel, and Pedro each manage
their own listings and are responsible for adding/removing their own
Knowledge Base docs as availability changes (see §6 below and
`kb/_property-listing-template.md`). Which agent Nora transfers to for a
given property is read from that property's specific listing document,
not fixed per call type.

## 0. Knowledge Base — verify first

Confirm these are uploaded to "Truenest Knowledge Data" and none are
stale duplicates:
- `maintenance-ac-not-cooling.md`
- `maintenance-clogged-toilet.md`
- `maintenance-thermostat-unresponsive.md`
- `maintenance-garbage-disposal.md`
- `maintenance-breaker-dead-outlet.md`
- `maintenance-smoke-detector-chirping.md`
- `maintenance-no-hot-water.md`
- `sales-owner-process-and-fees.md` — **if you uploaded the earlier
  single-tier version, delete that upload first**, then upload the current
  3-tier version, so the KB doesn't have two conflicting fee documents.
- `sales-owner-faq.md`
- Property listing docs, as Scharisse/Noel/Pedro add them, following
  `_property-listing-template.md`'s format

## 1. Transfer Call actions (7)

| Action name | Destination | Notes |
|---|---|---|
| Transfer to Pedro | 954-406-0400 | Owner Lead §1 (only if caller insists on talking to Pedro live) — reused for Prospective Tenant §6 when Pedro is the listing's assigned agent |
| Transfer to Roy | +1 954-278-7193 | Maintenance §3, after troubleshooting doesn't resolve it |
| Transfer to Christine | +1 954-361-9361 | Screening §5 |
| Transfer to Diego | +1 954-787-7077 | Current Owner §2, payment/ops questions |
| Transfer to Scharisse | +1 754-316-9815 | Prospective Tenant §6, when Scharisse is the listing's assigned agent |
| Transfer to Noel | +1 754-777-0353 | Prospective Tenant §6, when Noel is the listing's assigned agent |
| Emergency Transfer | +1 954-278-7193 (Roy) | Same number as "Transfer to Roy," configured as its own separate action so a failed emergency transfer stays distinguishable from a failed routine one in reporting/logs |

## 2. Internal Notification actions (3) — not caller-facing SMS

| Action name | Destination | Fires | Suggested message template |
|---|---|---|---|
| Internal Notification – Roy | +1 954-278-7193 (SMS) or roy@truenestpm.com | Alongside every Transfer to Roy, regardless of pickup | `Maintenance call from {{contact.first_name}} {{contact.last_name}} ({{contact.phone}}) — [issue description]. Troubleshooting already tried: [what was attempted, or "none"].` |
| Internal Notification – Christine | +1 954-361-9361 (SMS) or christine@truenestpm.com | Alongside every Transfer to Christine, regardless of pickup | `Screening inquiry from {{contact.first_name}} {{contact.last_name}} ({{contact.phone}}) about [property applied for].` |
| Emergency Escalation Webhook/SMS Alert | **Roy only** — +1 954-278-7193 and/or roy@truenestpm.com. Not Pedro. | Only when Emergency Transfer (above) fails to connect | `URGENT — unreached emergency call. {{contact.first_name}} {{contact.last_name}} ({{contact.phone}}) at [property address]: [issue description]. Not next-business-day — needs immediate callback.` |

The bracketed `[issue description]` / `[what was attempted]` fields are
what the prompt has Nora capture in conversation right before firing each
action — if your builder supports passing a captured variable into the
message template, wire it there; otherwise these need to be fields Nora
fills via an Update Contact Field-style action immediately before the
notification fires.

## 3. Send SMS actions (caller-facing)

| Action name | Fires from | Suggested message |
|---|---|---|
| Send SMS – Owner Process & Consultation | Owner Lead §1 fallback (Pedro unavailable / no time works) | `Hi {{contact.first_name}}, thanks for your interest in TrueNest! Here's more on our process: https://truenestpm.com/property-management-process — and you can grab time with Pedro here: https://calendly.com/truenestpm/consultation` |
| Send SMS – Tenant Portal | Maintenance §3 fallback, all of Tenant Support §4 | `Hi {{contact.first_name}}, here's your TrueNest Resident Portal: https://truenestpm.com/tenants` |
| Send SMS – Diego Check-in Link | Current Owner §2 fallback (Diego unavailable) | `Hi {{contact.first_name}}, here's Diego's check-in link to grab time with him directly: https://calendly.com/truenest/check-in` |
| Send SMS – Property Showing/Application | Prospective Tenant §6 | Content is dynamic per property — pulled from whichever listing document matched the call, not a single fixed link. If your builder needs a static fallback message wrapper, something like: `Hi {{contact.first_name}}, here's the info for [address]: showing — [showing link], apply — [application link]` |

Every SMS action should be preceded by Nora's spoken confirmation per the
prompt's SMS Confirmation Protocol ("Is this mobile number the best place
to text that link right now?") — that's prompt logic, not a builder
setting, so no separate action needed for it.

## 4. Tag / Update Contact actions (Intent Classification)

One action (or one per value, depending on how your builder's contact-tag
action works) applying one of these six tags, matching the prompt's
Intent Classification & Tagging section exactly:
`Owner Lead`, `Current Owner`, `Maintenance`, `Tenant Support`,
`Screening`, `Prospective Tenant`

Distinct from the 4 field-extraction actions already configured
(First Name / Last Name / Email / Phone) — nothing currently applies these
tags.

## 5. Appointment Booking — confirmed working

Calendar: **Property Management Consultation - TrueNest** (30 min, Active,
owned by Pedro Andrade) — confirmed correct via a real test booking
already on it. A second calendar, "Onsite Consultation - Pedro Andrade"
(1 hr), also exists in the same group — TBD whether that's a distinct
later-stage in-person step (no action needed) or a duplicate worth
deactivating to avoid ambiguity.

**Advanced Options, as configured:**
- Collect: Email ✓ (already recommended), Phone number ✓, Full name ✓
- Not collected: Address (this calendar isn't an in-person appointment)
- Notes ✓ — carries the property address/unit count/occupancy status Nora
  already gathered during qualification, so Pedro has context before the
  call (prompt updated to actually populate this)
- Scheduling window: 3 days ahead, 1h buffer — worth confirming with Pedro
  that 3 days usually has open slots; tighten/loosen if his calendar tends
  to fill up
- Cancel appointment ✓, Reschedule appointment ✓ — prompt updated so Nora
  actually uses these for an existing consultation instead of just taking
  a message, which had no coverage before this was discovered
- Auto-confirm appointments ✓ (already correct)

## 6. Prospective Tenant flow — property listing docs

Design is settled now (see prompt §6), adapted from a proven pattern Pedro
already runs on his own HousebyPedro listings:

- **Realtor/Agent callers:** Nora answers property questions from the
  matching listing document, points showing requests to "the MLS listing
  instructions for this property" (not a specific named tool), and routes
  commission questions to that listing's assigned agent.
- **Unrepresented prospective tenants:** Nora runs a one-question-at-a-time
  prequalification interview (move-in date, household size/income, credit
  range, eviction/criminal history, co-signer need, move-in funds, pets),
  compares it against that property's specific criteria from its document,
  and — regardless of fit — never declares approval, denial, or "not a
  match." Good fit → texts that property's self-service showing link. Poor
  fit → neutral self-assessment framing, still invites them to apply.
- Either path, once ready to move forward: transfers to whichever of the
  three agents (Scharisse/Noel/Pedro) is named as that listing's agent.

What's still needed to actually go live on this: Scharisse, Noel, and
Pedro adding their current active listings as Knowledge Base docs using
`kb/_property-listing-template.md`'s format — including the
prequalification-criteria fields, which are new to this template version
and need filling in per property, not left blank. Until at least a few
listings exist in the KB, this flow has nothing to retrieve — worth
testing with a couple of real listings uploaded before relying on it in
production.
