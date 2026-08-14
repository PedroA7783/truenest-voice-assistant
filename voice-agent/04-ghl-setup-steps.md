# Applying this in the GoHighLevel builder

Steps, in order, for the location `KyQITFKPi0R84dwIN85b` agent
`6a7f87cfaf818819a894e817`:

1. **Fill in the placeholders first.** Before touching the builder, confirm
   the ⚠️ items in `00-business-profile.md` (phone number, address, service
   area boundary, current fee structure) with the client or your CRM records.
   Don't paste guessed values into a live agent.

2. **Personality/Prompt field:** replace the existing prompt with
   `02-system-prompt.md`, substituting confirmed values for the
   `{{placeholders}}`.

3. **Actions tab:** create/update the actions listed in
   `03-actions-and-call-flows.md`'s table. For the two `Transfer Call`
   actions, place a real test call to each destination number before saving —
   confirm it's staffed and picks up, don't trust a saved number.

4. **Calendars:** if `Book Showing` and `Book Owner Consult` don't already
   have GHL calendars behind them, create those first (Calendars tab) — the
   voice agent's Book Appointment action needs a calendar to point at.

5. **SMS templates:** for each `Send SMS – *` action, write the actual message
   text (application link, rate sheet, portal link) using your client's real
   URLs — the flows only specify *when* to send each, not the literal link.

6. **CRM/webhook action:** confirm what pipeline/opportunity stages exist in
   this GHL sub-account and map `Update Contact / Create Opportunity` to tag
   by track (Prospective Tenant / Maintenance Request / Prospective Owner /
   Owner Inquiry / Tenant Inquiry) so each lands in the right place for a
   human to follow up.

7. **Voice/language:** if the plan supports it, enable Spanish as a fallback
   language given the Broward/Miami-Dade market — check GHL's current voice
   model support for language switching before scripting around it.

8. **Test before publishing.** Run every scenario in `05-qa-test-script.md`
   as real test calls against the draft agent. Fix anything that breaks
   before flipping it live on the client's main number.

9. **Re-audit after major site/pricing changes.** This whole package was
   built from what's publicly indexed about truenestpm.com — if the client
   changes their fee structure, service area, or policies, the prompt and
   flows need a refresh, since the agent can't know about it otherwise.
