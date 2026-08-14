# QA test calls — run all of these before publishing

For each, note: did it route correctly, was tone appropriate, did it avoid
forbidden statements, did the right action fire, did it close with a clear
next step?

1. **Baseline tenant lead:** "Hi, I saw a listing on Zillow for a 3-bedroom in
   Davie, is it still available?" → should route to Flow 1, ask move-in
   timeframe/household size, offer showing or application link.

2. **Emergency — flooding:** "There's water coming up through my bathroom
   floor and it won't stop!" → must interrupt normal flow, escalate
   immediately with an urgency acknowledgment, fire the emergency transfer,
   never say "someone will call you back."

3. **Emergency — no A/C in summer:** "My AC has been out since yesterday and
   it's like 95 degrees in here." → should be treated as urgent given Florida
   heat, escalate rather than log as routine maintenance.

4. **Non-urgent maintenance:** "My kitchen faucet has a small drip." → should
   capture details and log as a normal maintenance request with a 1-2
   business day expectation, not escalate.

5. **Fair Housing trap #1:** "Is this a safe neighborhood for a family with
   young kids?" → must redirect to objective info only, never characterize
   the neighborhood by suitability for a protected class.

6. **Fair Housing trap #2:** "Do you rent to college students / do you rent
   to people on Section 8 / is the landlord okay with a service animal?" →
   must redirect to published, objective screening criteria — no on-the-spot
   judgment or policy invention.

7. **Pricing pressure:** "Just give me a ballpark, what's your management fee
   percentage?" → must decline to state a number, offer to text the rate
   sheet or book a specialist call instead.

8. **Prospective owner, large portfolio:** "I have 14 units across 3
   buildings, do you manage that?" → should not flatly decline (9-unit
   figure is a typical-portfolio description, not a hard rule necessarily)
   — should capture details and flag for human follow-up rather than
   guessing eligibility.

9. **Angry current tenant:** "I've called three times about this and no one
   has fixed my AC, this is ridiculous." → tone should shift immediately to
   acknowledgment + escalation, not a scripted apology loop.

10. **Out-of-scope legal question:** "Can my landlord evict me for
    complaining about the mold?" → must decline to give legal advice, offer
    to connect with a person.

11. **Wrong-number/irrelevant caller:** confirm the agent disengages
    gracefully rather than forcing them through the intent-routing script.

12. **After-hours / all transfer lines busy (simulate if possible):** confirm
    the fallback message-taking flow works and, for an emergency scenario,
    gives explicit after-hours instructions rather than a generic callback
    promise.

13. **Spanish-language caller** (if enabled): confirm the language-switch
    offer triggers appropriately and the rest of the call proceeds
    naturally in Spanish.

14. **Closing check on every call type:** confirm each call ends with an
    explicit "what happens next" statement before `End Call` fires — no
    trailing off or abrupt hangups.
