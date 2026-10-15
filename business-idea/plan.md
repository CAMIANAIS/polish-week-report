# Business idea plan

## My assumption

I believe accountants lose a lot of time checking invoices for crédito fiscal and uploading entries one by one, and would pay for a tool that validates invoices automatically and uploads them in bulk.
I'll know I'm right if 3 of 5 accountants say it costs them hours, and 3 of 5 already pay for a tool.

## How my plan changed

First I thought the problem of small business owners was fear of SUNAT. Then I learned that some owners are not formal businesses, and they care more about client debts and sales. So I talked to an accountant, and I learned that their pain is checking invoices for crédito fiscal and uploading entries one by one.

## 1. Who it's for, the problem, and how they solve it today

Aldía is for accountants in Peru who check invoices by hand. They reject many invoices, and they upload entries one by one. Today they use Concar, SIRE and Excel, but validation is still manual.

## 2. Competitors and why someone would choose my product

- **CumpliTax:** claims automatic validation with 140+ rules (vendor claim, not proven).
- **NubeCont:** accepts XML and compares with SIRE. S/ 49/month + S/ 15 per extra company.
- **ContaUno:** made for accountants with many companies (S/ 139 for up to 50). The accountant still reviews.

**Why Aldía:** Aldía flags doubtful invoices and the accountant approves them. It saves the flagged information, so when SUNAT audits, the accountant can show why each invoice was accepted. It's an add-on next to their current tool, not a replacement.

Full research: `research/Aldía_ closest competitors and adjacent tools.md` and `research/aldia_competitor_matrix.csv`.

## 3. My experiment, what I learned, and what I'm still assuming

**Experiment:**
- I talked about my idea in an auditorium at a tech event.
- I made an informative webpage with a demo and showed it at the event. Analytics: 9 visits, only 1 left their contact, plus 1 warm contact.
- I did 2 live interviews with possible users (small business owners).
- I contacted 3 business incubators. Only 1 replied: they mostly have student ideas, not running businesses registered with SUNAT.
- By message, I asked 1 entrepreneur and 1 accountant. The accountant told me they still verify data by hand.
- Next: 2 calls with accountants to validate this.

**What I learned:**
- Fear of SUNAT is not what owners worry about most.
- Many businesses are informal, not registered with SUNAT.
- Accountants have many tools, but they still need to verify what the system does.

**Still assuming:**
- Accountants would pay for it.
- Validation can be automated.
- My worry: every workflow should have a human in the loop. Maybe this validation step is that human step. I will test it.

## 4. Smallest useful version, mockup, how I'd build and test it

- **Smallest version:** Aldía alerts the accountant: "you need to review these specific invoices." It checks 3 rules: the supplier's RUC is not active/habido, the CPE is not valid in SUNAT, or the invoice is missing data.
- **Mockup:** the accountant's Excel with a new review column (e.g. "⚠️ Revisar: RUC no habido").
- **Build:** by hand first (concierge MVP). I check each invoice myself with SUNAT's RUC and CPE lookup tools, to test if the idea helps before I write code.
- **Test:** An accountant I interviewed gives me 3 invoices she rejected and 3 good ones. It works if Aldía flags all 3 rejected and none of the good ones.

**Flow** (the validation step is where people had the most opinions):

```
Accountant uploads Excel
        ↓
Aldía checks the 3 rules
        ↓
   Invoice flagged?
   │
   ├─ no  → Goes to Concar ✅
   │
   └─ yes → Accountant reviews it
                 ↓
            Approve or reject?
            │
            ├─ approve → Record saved → Goes to Concar ✅
            └─ reject  → Record saved → Back to the client ❌
```

## 5. Pricing, marketing, first paying customers

- **Price:** S/ 5 per RUC per month, cheaper than NubeCont's extra company. With ~37 clients per accountant, that's about S/ 185 per month.
- **Market:** about 1.5 million taxpayers in RMT and RER can use crédito fiscal (SUNAT 2025). NRUS can't. There are more than 63,000 registered accountants (CCPL, 2023).
- **First customers:** my accountant friends first. Then the accountants' club on the outskirts of the city, which holds events for all accountants (volleyball tournaments, dances). I'd also go to my university to talk to some teachers about the idea.

## 6. What I'd test next, and what result would make me change direction

- Next test: calls with 5 accountants. I'm right if 3 of 5 say it costs them hours, and 3 of 5 already pay for a tool.
- I'll also test if accountants want a human in the loop for validation.
- If accountants say no, I'll go back to owners and focus on client debts.

https://emprender.sunat.gob.pe/ruc/regimenes-tributarios-mype/nuevo-regimen-unico-simplificado-nuevo-rus

https://www.sunat.gob.pe/legislacion/rus/regla/

- Group 1 moved to MYPE, so they knew they passed.
- Group 2 stayed, so they knew too.
