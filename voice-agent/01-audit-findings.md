# Voice AI Agent Audit — TrueNest Property Management

## Scope and a limitation you should know about

I could not log into the GoHighLevel voice agent builder — that link
(app.gohighlevel.com/v2/location/.../ai-agents/voice-ai/builder/...) requires your
authenticated session, and this environment has no GoHighLevel credentials, API
key, or connector attached, and no browser to drive your login. So this isn't a
line-by-line diff of your current prompt against a new one — it's a from-scratch,
best-practice build for a property-management voice agent, sized to TrueNest using
what's public about the business (see `00-business-profile.md`).

Two ways to close that gap, your call:
1. **Fastest:** copy your current agent's Personality/Prompt field and Actions list
   into a message here — I'll diff it against the recommendations below and tell
   you exactly what to change.
2. **Or:** treat `02-system-prompt.md` and `03-actions-and-call-flows.md` as a
   drop-in replacement and paste them into the builder yourself (10-15 minutes).
   `05-qa-test-script.md` gives you call scenarios to run before it goes live.

## Why a property-management voice agent is a different animal

Most GHL voice agent templates are built for generic lead-gen (real estate,
home services, roofing). Property management has three properties that a
generic template gets wrong if untouched:

1. **Two completely different callers share one phone number.** A prospective
   tenant asking about a listing and a current tenant with a burst pipe need
   opposite handling — one is a sales conversation, the other is an operational
   emergency triage. An agent that doesn't branch on this in the first two turns
   will mishandle both.
2. **Some inbound calls are genuine emergencies.** Flooding, no A/C in a Florida
   summer, gas smell, no working smoke detector, lockout, sewage backup — these
   need immediate live transfer or an explicit "this is being escalated right
   now," not "someone will call you back within 24 hours."
3. **Fair Housing Act exposure is real and specific to this industry.** A voice
   agent that answers "do you accept families with kids" or "is this a good
   neighborhood for [group]" with anything other than a neutral, policy-based
   answer creates legal risk for your client. This has to be a hard guardrail in
   the prompt, not an afterthought.

## High-impact findings (apply regardless of what's currently configured)

### 1. Missing caller-intent triage at the top of the call
**Risk if absent:** the agent either assumes every caller is a lead (annoys
existing tenants/owners) or assumes every caller is a tenant (loses leasing
opportunities). **Fix:** first substantive question routes into one of four
branches — prospective tenant, current tenant, prospective owner, current owner
— before anything else happens. Built into `02-system-prompt.md` and
`03-actions-and-call-flows.md`.

### 2. No emergency-maintenance fast path
**Risk if absent:** a tenant with a real emergency gets the same "I'll log a
ticket, someone will follow up" treatment as a leaky faucet — bad for the tenant,
liability exposure for the owner and for TrueNest. **Fix:** an explicit keyword/
symptom check (flooding, fire/smoke, gas smell, no A/C above a heat-index
threshold, no heat, sewage backup, broken exterior lock/lockout, no working smoke
or CO detector) that triggers immediate live transfer, with a scripted line that
tells the caller escalation is happening now, not "eventually."

### 3. Fair Housing guardrails not enforced at the prompt level
**Risk if absent:** the model will, if asked, cheerfully speculate about
neighborhood safety, school quality "for families," or make judgment calls about
an applicant's suitability — all Fair Housing violations if said by or on behalf
of a housing provider. **Fix:** explicit "never do X" list in the system prompt
(see `02-system-prompt.md` §Compliance), plus scripted redirects to give only
objective, published screening criteria.

### 4. Pricing/fee numbers likely to be stated with false confidence
Public info on TrueNest's fee structure was inconsistent (see business profile).
A voice agent that confidently states a management-fee percentage that's wrong,
outdated, or for the wrong property type is a credibility hit your client will
hear about from an owner prospect. **Fix:** agent captures property details and
offers to text the current rate sheet or book time with a leasing specialist,
never states a number itself unless you paste in the confirmed current rates.

### 5. No language for the South Florida market
Broward/Miami-Dade has a large Spanish-speaking population. If the underlying
GHL voice model/number supports Spanish, offering an immediate language switch
("¿Prefiere continuar en español?") measurably reduces drop-off. Flagged as a
recommended action, not force-built, since it depends on your GHL plan's voice/
language support — check before enabling.

### 6. No clear "what happens next" close on every call type
**Risk if absent:** callers hang up unsure if anything actually happened.
**Fix:** every branch in `03-actions-and-call-flows.md` ends with an explicit
confirmation of the next step and a timeframe (e.g., "You'll get a text with the
application link in the next minute; our leasing team follows up within one
business day").

### 7. Voicemail/overflow behavior undefined
If no human is reachable for a live transfer (after hours, all lines busy), the
agent needs a defined fallback: take a structured message, confirm callback
window, and — for anything flagged as an emergency — give the caller the after-
hours emergency-line number/instructions rather than a generic "we'll call you
back."

### 8. Generic, over-long turns
Voice agents built from web-chat prompts tend to produce paragraph-length
responses, which sound robotic and cause callers to talk over the agent.
`02-system-prompt.md` enforces 1–2 sentence turns and no reading back of lists
longer than 3 items.

## Checklist to run against your live GHL builder

Use this once you have the builder open — each item maps to a section of this
audit:

- [ ] Personality/Prompt field matches the structure in `02-system-prompt.md`
      (identity, tone, intent-routing, compliance guardrails, escalation rules)
- [ ] "Actions" list includes: Transfer Call (at least 2 destinations — general
      leasing/PM line and emergency maintenance line), Book Appointment
      (showings), Send SMS (application link, portal link, rate sheet), and a
      CRM-update/webhook action that writes structured lead data back into your
      pipeline stage, plus End Call
- [ ] Transfer destinations are the correct, currently-live phone numbers/queues
      — verify by placing a real test call, don't trust what's saved
- [ ] Knowledge base (if GHL exposes one for this agent) has the service-area
      list, application requirements, and screening-criteria language pulled
      from `00-business-profile.md`, not the model's guess
- [ ] No fee/pricing numbers are hardcoded unless you've confirmed them against
      the current live rate sheet
- [ ] A test call for each scenario in `05-qa-test-script.md` behaves as
      expected, including the Fair Housing trap questions and the emergency-
      maintenance fast path
