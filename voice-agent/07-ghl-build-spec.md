# GHL Build Spec — exact configuration for every action

Matched against the current prompt (`02-system-prompt.md`) and the 9
Knowledge Base docs in `voice-agent/kb/`. `{{ }}` marks a value still
needed.

## Team contacts (confirmed)

| Name | Role | Phone | Email |
|---|---|---|---|
| Pedro | Business Development (BDM) | 954-406-0400 | pedro@truenestpm.com |
| Roy Maldonado | Maintenance Lead | +1 954-278-7193 | roy@truenestpm.com |
| Christine | Screening | +1 954-361-9361 | christine@truenestpm.com |
| Diego Binetti | Operations | +1 954-787-7077 | diego@truenestpm.com |

Note on spelling: written as "Cristine" once and "Christine" every other
time across our conversation — using "Christine" throughout since that's
been consistent; flag if that's actually wrong.

**Diego isn't wired into any flow yet** — he was given as a contact but no
role in the call routing was specified. Is he meant to be a backup/escalation
contact somewhere (e.g. Operations backstop if Roy doesn't answer an
emergency), or just FYI contact info for now? Not assuming either way.

**Hard rule, confirmed by the client: Pedro is never looped into
maintenance or repairs — no notifications, no reports, no escalation path
through him for anything maintenance-related.** This overrides my earlier
suggestion of "Roy + Pedro" for the emergency escalation alert — corrected
below to Roy only.

## 0. Knowledge Base — verify first

Confirm all 9 docs are uploaded to "Truenest Knowledge Data" and none are
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

Per-property listing docs are a planned addition — see the note at the
bottom of this file before building those.

## 1. Transfer Call actions (4)

| Action name | Destination | Notes |
|---|---|---|
| Transfer to Pedro | 954-406-0400 | Owner Lead §1 — only fires when caller explicitly insists on talking to Pedro live; default path is booking, not this |
| Transfer to Roy | +1 954-278-7193 | Maintenance §3, after troubleshooting doesn't resolve it |
| Transfer to Christine | +1 954-361-9361 | Screening §5 |
| Emergency Transfer | +1 954-278-7193 (Roy) | Same number as "Transfer to Roy," configured as its own separate action so a failed emergency transfer stays distinguishable from a failed routine one in reporting/logs |

## 2. Internal Notification actions (3) — not caller-facing SMS

| Action name | Destination | Fires | Suggested message template |
|---|---|---|---|
| Internal Notification – Roy | +1 954-278-7193 (SMS) or roy@truenestpm.com | Alongside every Transfer to Roy, regardless of pickup | `Maintenance call from {{contact.first_name}} {{contact.last_name}} ({{contact.phone}}) — [issue description]. Troubleshooting already tried: [what was attempted, or "none"].` |
| Internal Notification – Christine | +1 954-361-9361 (SMS) or christine@truenestpm.com | Alongside every Transfer to Christine, regardless of pickup | `Screening inquiry from {{contact.first_name}} {{contact.last_name}} ({{contact.phone}}) about [property applied for].` |
| Emergency Escalation Webhook/SMS Alert | **Roy only** — +1 954-278-7193 and/or roy@truenestpm.com. Not Pedro, per the client's hard rule above. | Only when Emergency Transfer (above) fails to connect | `URGENT — unreached emergency call. {{contact.first_name}} {{contact.last_name}} ({{contact.phone}}) at [property address]: [issue description]. Not next-business-day — needs immediate callback.` |

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
| Send SMS – Showing & Application | Prospective Tenant §6 | Per-property — see below, this is being redesigned around per-listing Knowledge Base docs rather than a single static link |

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

## 5. Appointment Booking — confirm, don't rebuild

One Appointment Booking action already exists. Confirm it's pointed at
the calendar Pedro actually uses for new-owner consultations.

## 6. Prospective Tenant flow — being redesigned, not final yet

The client's proposal: add each active property as its own Knowledge Base
document (so Nora can answer property-specific questions and pull that
property's actual showing link, since every listing has its own), then
transfer to a leasing agent once a caller is ready to move forward and
rent. See the chat response for the design discussion and open questions
(who the leasing agent is, and how listing docs get kept in sync when a
property is rented or a new one comes on). Don't build §6's actions until
that's settled — reusing the current single static "Showing & Application"
link would work against a design that's about to change.

## Still open before this is fully wired up
- Diego's role, if any, in the call routing
- Leasing agent's name and direct number, once decided (see §6)
- How per-property Knowledge Base docs will be created and kept current
  (see §6 / chat discussion)
