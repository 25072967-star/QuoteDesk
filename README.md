# QuoteDesk

**From WhatsApp enquiry to approved quotation.** An automation module concept for the Orbyte business hub, built for the Orbyte Smart SME Challenge (Domain 2).

- **Live demo:** https://25072967-star.github.io/QuoteDesk/
- **Team:** Pink Needoh (Sarah Khadija)

## The problem

Many Malaysian micro and small businesses take orders over WhatsApp. A customer sends a messy message such as "nak 50 box kuih lapis + 20 pulut panggang, Friday, KL", and the owner or staff member has to read it, look up prices, check stock, work out delivery and discounts, then type a quotation by hand. This takes time, often happens late at night, and mistakes creep in: wrong prices, items out of stock, missed delivery rules, forgotten replies.

## What QuoteDesk does

It turns one recurring task, answering a WhatsApp enquiry with a correct quotation, into a short, supervised workflow.

| Step | What happens in the prototype |
|---|---|
| **Trigger** | A customer message arrives in the inbox (8 sample messages, or paste your own). |
| **Connected data** | The system looks up the product catalogue, prices, stock, lead times, delivery zones and the customer's discount tier. |
| **Decision logic** | The message is read into items, quantity, date and location. Rules then check stock, notice time, delivery zone, discounts and total value. Anything uncertain is flagged rather than guessed. |
| **Human approval** | A person with the right role reviews, edits and approves. Nothing is sent without approval. |
| **Visible outcome** | The quotation is shown as a sent WhatsApp message preview, and every action is written to an activity log. |

## People stay in control

Roles are simulated with a dropdown at the top of the page (a full identity system is out of scope).

| Role | Can do |
|---|---|
| **Sales staff** | Edit lines, ask the customer to clarify, reject, and approve quotations that have no warnings. |
| **Owner** | Everything staff can do, plus approve quotations that have warnings (e.g. over RM500, stock shortage, tight lead time). |
| **Viewer** | Read-only. Customer phone numbers are masked. |

Red flags (no catalogue match, missing quantity) block approval for everyone until fixed. The **Activity log** tab records who did what and when.

## Try it

1. Open the live demo.
2. As **Sales staff**, open the **Siti Aminah** enquiry. It is over RM500, so staff cannot approve it.
3. Switch to **Owner** and approve it. Check the sent preview.
4. Open the **Activity log** tab to see the record.
5. Also try: **Pak Din** (stock shortage), **Daniel Raj** (item not in the catalogue), **Aisha Karim** (Penang is outside the delivery zones), **Nurul Huda** (not enough notice for the item).
6. Paste your own message in "Try your own message" and see how it is read.

## How it works (technical)

- A single-file web app (`index.html`: HTML, CSS and JavaScript) with no backend and no build step.
- All data is held in memory and resets on refresh.
- To run locally, download `index.html` and open it in a browser.
- The parsing step lives in the `parse()` function. Pricing and approval rules live in `calc()` and `why()`.
- **Honest note:** in this prototype the message-reading step uses simple rule-based text matching (aliases, typo-tolerant keywords, Malay and English date and location words) as a stand-in for an AI model. In a production version, `parse()` would call a language model that returns the same structured result (items, quantities, date, location, confidence). Pricing, stock and approval rules would stay as plain deterministic code, so AI is used only where it adds value: reading messy text.

## Orbyte fit and reuse

- QuoteDesk reads shared hub data (product catalogue, customer records, user roles) instead of keeping its own silo, and writes its actions to a shared activity record.
- It adds a distinct SME capability (sales quotation handling) and does not duplicate HR or admin functions.
- Any SME with a price list and WhatsApp customers can use it. Only the catalogue and rules change.
- Roadmap: connect to Orbyte invoicing and inventory, add other channels (Instagram, email), add automatic follow-up reminders, and offer the parsing and approval logic as a reusable API.
- Possible business model: a free tier with a monthly quotation limit, then per-seat or per-quotation pricing through the Orbyte ecosystem.

## Hardest risk and fallback

**Risk:** the message is misread (wrong item or quantity) and a wrong quotation goes out.

**Mitigations:**
- Items are matched against the catalogue; low-confidence matches are marked "Check".
- Unmatched items or missing quantities are red-flagged and block approval.
- A human always approves before anything is sent.

**Fallback:** if the reading step fails or the AI service is unavailable, the enquiry still appears in the inbox with the original message beside a blank quotation to fill in by hand. The business is never worse off than today.

## Expected benefit

These will be measured on the test messages rather than assumed:

| Measure | Result |
|---|---|
| Time to prepare a quotation, manual vs QuoteDesk | [fill in after timing 10 of each] |
| Test messages read correctly | [fill in; the app footer shows the parser check] |
| Pricing or stock problems caught before sending | [fill in] |
| New users who completed a quotation with no instructions | [fill in, e.g. 3 of 3] |

## SDG contribution

- **SDG 8 (Decent Work and Economic Growth), targets 8.2 and 8.3:** raises productivity through technology and supports the growth of micro and small businesses.
- **SDG 9 (Industry, Innovation and Infrastructure):** gives small businesses access to automation they could not afford to build.

## Data and privacy (PDPA)

- The prototype uses **only synthetic data**: invented customers, fake phone numbers and made-up prices. No real personal or company records are used.
- In a real deployment, Malaysia's PDPA principles would be applied: collect only the data needed to prepare a quotation, tell customers how their data is used and get consent for WhatsApp contact, restrict access by role (for example masking phone numbers for read-only users), keep an activity log for accountability, set a retention period and delete data after it, and use the official WhatsApp Business API rather than informal workarounds.

## Out of scope

Real payments, statutory filings, and legal or financial advice. The prototype only prepares quotations from synthetic data. It does not connect to WhatsApp or to the live Orbyte platform.

## Limitations

- Roles and login are simulated.
- Data is not saved between page refreshes.
- Message reading is a rule-based stand-in for an AI model, and works best on the sample catalogue.
