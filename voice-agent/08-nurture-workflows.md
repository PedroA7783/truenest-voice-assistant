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

**Superseded, pending real input:** the client's own current process is
near-daily text + call follow-up for 8-10 days, then a handoff into a
separate longer-term nurture — already built and running today in
LeadSimple. The cadence above was a reasonable-guess starting point before
that was known; screenshots of the actual LeadSimple workflow are coming,
and the real redesign will replace this table rather than tweak it —
keeping this version here only until that lands.

**Explicit goals for the redesign, per the client:** work in real sales
psychology / mental-triggers, be "aggressive yet professional," and
optimize for converting to a booked call as fast as possible — while
staying inside a compliant SMS/call cadence (a working opt-out, and
honoring it immediately — see the TCPA note below).

**TCPA consideration — build this in, don't bolt it on later.** Near-daily
text and call contact for over a week is squarely in TCPA territory
(federal law governing repeated marketing contact), and violations carry
per-message statutory damages. Whatever the redesigned cadence ends up
being needs: a real "reply STOP to opt out" mechanism that's actually
wired to stop the workflow (not just printed as a courtesy line), and
immediate compliance if a lead says stop contacting them in any form
(text, call, or verbally to Pedro) — not just via the literal STOP
keyword. This constrains "aggressive," it doesn't rule it out.

## Workflow 2 — Onboarding Data Collection (owner confirmed, cadence TBD)

**Owner: Neser Gumana** — neser@truenestpm.com, +1 954-787-7078. Contacts
new owners once they sign on to collect account, mailing address, HOA,
and contact information. Note: his number is one digit off Diego
Binetti's (+1 954-787-7077) — be precise when this gets built so nothing
gets crossed between the two.

**Why:** Same underlying problem as Workflow 1 — new clients are often
slow to respond, so this needs persistent multi-touch follow-up rather
than a single request that goes unanswered.

**Not fully designed yet** — the person is confirmed, but exact cadence,
what specifically needs collecting (beyond account/mailing/HOA/contact
info), and what channel Neser prefers (text, call, email) still need your
input before this is buildable. Reasonable next step once you're ready:
same treatment as Workflow 1 — either describe how Neser currently
handles this, or share it if it's also captured in LeadSimple.

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
