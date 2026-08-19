# Post-Call Nurture Workflows

These are GHL Workflow automations — multi-day sequences with wait steps
and conditions — not something Nora says live on a call. They pick up
where the voice agent's single call leaves off, for leads/processes that
realistically take multiple touches over days or weeks to close. Same
"design it here, build it in GHL" pattern as the rest of this repo.

## Workflow 1 — Owner Lead Sales Nurture (designed, ready to build)

**Why:** Per the client — homeowners shopping for a property manager
rarely decide on the first call. They compare other companies, need to
loop in a spouse, or just need time. A single call with no follow-up
loses winnable deals to whoever follows up better.

**Trigger:** Contact tagged `Owner Lead` by Nora during the call, AND no
appointment currently booked on the "Property Management Consultation -
TrueNest" calendar for that contact. Check this ~1-2 hours after the call
ends (not immediately) — gives room for a same-call booking or someone
using the SMS fallback link right after hanging up, so the workflow
doesn't fire on a lead that already converted.

**Stop conditions (check before every step, not just at the start):**
exit immediately if an appointment gets booked, or if the contact
explicitly says not interested / asks to stop.

**Sequence (SMS-primary; add email too if one was captured):**

| When | Message |
|---|---|
| ~2 hrs after call | "Hi {{contact.first_name}}, thanks for chatting with Nora about TrueNest managing your property! Whenever you're ready, here's Pedro's calendar: https://calendly.com/truenestpm/consultation — happy to answer anything else too." |
| Day 2 | A value-reinforcing touch, not just a repeated link — pull a differentiator from the Owner Sales KB doc that fits (e.g., "One thing owners often don't realize: we don't profit a cent on maintenance or repairs — no kickbacks, no upcharging. Happy to walk through the rest whenever works for you: [link]") |
| Day 5 | An FAQ-style touch — pick one from `sales-owner-faq.md` likely to matter to a still-deciding owner (e.g., the no-lock-in / 30-day-notice point, since "what if it doesn't work out" is a common hesitation) + link |
| Day 10 | Soft close, not a hard push — "Just want to make sure this doesn't get lost — let us know if you'd like to pick this back up, or if now's not the right time we'll check in again down the road." |
| Day ~75 (re-engagement) | One longer-horizon touch — "Circling back in case timing's better now: [link]" — then exit the workflow either way |

**Open items before building this:**
- Confirm the cadence above (2hrs/2d/5d/10d/75d) matches how aggressively
  vs. gently you want to pursue these — easy to adjust once it's live.
- Decide if Days 2 and 5's message content should rotate through a fixed
  set you write once, or if you want me to draft several variants to
  rotate through so repeat leads don't get the identical text on a second
  pass through the funnel.

## Workflow 2 — Onboarding Data Collection (pending: who runs this?)

**Why:** Once an owner signs on, someone needs to collect account/mailing
address/HOA/contact details — and owners are often slow to respond, so
this needs the same kind of persistent multi-touch follow-up as Workflow 1.

**Blocked on:** confirming who this is assigned to — the client referenced
a name ("Nessid") that doesn't match anyone established in this build
(Pedro, Roy, Christine, Diego, Scharisse, Noel). Not designing the
trigger/sequence until that's confirmed, since assigning this to the
wrong person is a real error, not just a placeholder to fix later.

## Workflow 3 — Screening Follow-Up During the 5-Business-Day Window (not yet scoped)

**Why:** Once an application is submitted, TrueNest has up to 5 business
days to complete verification. The client raised this as a "maybe" — an
idea, not a confirmed requirement, and isn't yet sure whether a formal
workflow is needed here versus Christine just handling it manually.

**Not designed yet** — needs more detail on the actual screening process
(what triggers a follow-up: waiting on a document from the applicant?
waiting on a previous landlord's callback? just an internal reminder for
Christine to check status?) before there's enough to build a real
workflow instead of a guess.
