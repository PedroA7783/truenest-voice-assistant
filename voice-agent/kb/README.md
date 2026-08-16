# Knowledge Base documents

Upload these to the "Truenest Knowledge Data" knowledge base in the GHL
Voice AI builder (the panel shown in the screenshot from 2026-08-16).

## Maintenance troubleshooting
- `maintenance-ac-not-cooling.md`
- `maintenance-clogged-toilet.md`
- `maintenance-thermostat-unresponsive.md`
- `maintenance-garbage-disposal.md`
- `maintenance-breaker-dead-outlet.md`
- `maintenance-smoke-detector-chirping.md`
- `maintenance-no-hot-water.md`

**Retrieval confirmed working** (2026-08-16 test call) for the first three
docs. The first three still have matching inline copies in
`02-system-prompt.md`'s Maintenance flow (§3) as a safety net — trim those
out once you've confirmed the four new docs retrieve correctly too. The
four new docs don't have inline copies in the prompt; they exist only here.

## Owner sales
- `sales-owner-process-and-fees.md` — full process walkthrough and fees for
  all three tiers (Basic 8%, Full 9.9%, Premium 11.9%, each with its own
  min/max), plus Premium's Landlord Protection Plan coverage. Pricing
  disclosure is now approved for this document specifically (client's
  call — it's published on the website); see `02-system-prompt.md`'s
  Compliance section for the exact scoping. Remaining open gap: Basic and
  Premium's setup/leasing/renewal fees weren't provided (only their
  management fee rates were) — noted inline as something to defer to Pedro
  or the website until supplied.
- `sales-owner-faq.md` — the 9 standard owner questions/answers.

## Property listings (Prospective Tenant flow)
- `_property-listing-template.md` — the format for every active listing.
  Scharisse, Noel, and Pedro (the three leasing agents) each own their own
  listings and are responsible for adding a doc when a property goes
  active and removing/updating it the same day it's rented — see the
  template's notes for why that matters so much for a voice agent.
- Actual listing docs get added here as `listing-[address-slug].md`, one
  per property, once agents start uploading them.

## Still to add
- Any further troubleshooting scripts as they come up (garage/gate remote,
  fridge not cooling, washer/dryer issues, etc.)
- Property listing docs, as agents add them
