---
name: request-info
description: Put the user in touch with a licensed Dubai real-estate agent about a building, off-plan project or villa community — available units, prices, payment plans, a viewing or an offer — using the Dubai Wealth Index request_info tool. Use when the user wants to buy, rent, view, make an offer, or talk to an agent about a Dubai property.
---

# Request info from an agent

Explicit user instructions take priority over this workflow.

1. **Pin the property.** Use the page url from an earlier result if you have
   one. Otherwise `search` the name and confirm the match with the user when
   it is fuzzy or there are several.
2. **Ask what they want, briefly.** Unit type, buy or rent, and anything that
   matters to them (budget, timing). Write it as the
   `message` in their words. Don't interrogate: the property alone is enough.
3. **Ask how to reach them.** The account's name and email are always sent.
   Ask whether they also want a call or WhatsApp, and on which number.
4. **Confirm, then send.** Say what will be sent and to whom (Dubai Wealth
   Index, which replies and introduces a licensed agent), get a yes, then call
   `request_info` with
   `consent: true`. Never call it on your own initiative.
5. **Report back.** Give the reference, who it went to (`sentTo`), and the
   WhatsApp link if one is returned. Follow the result's `next` line.

If `request_info` returns `received: false`, fix the named fields with the
user and call again. If it returns `found: false`, offer its suggestions or
send it as a general enquiry without `property`.
