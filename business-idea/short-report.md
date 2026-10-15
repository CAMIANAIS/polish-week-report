# Aldía

**Aldía flags doubtful invoices so accountants review only those.**

| 3 of 4 | 3 of 4 | ~2 hours | 3 of 3 |
|---|---|---|---|
| accountants say checking invoices costs hours | already pay for a tool | to check 50–70 invoices, one by one | check the banking channel |

## My biggest assumption

I believe accountants lose a lot of time checking invoices for crédito fiscal (input VAT credit: the sales tax paid on purchases, which a company subtracts from the sales tax it owes), and would pay for a tool that validates invoices automatically.

## How my plan changed

First I thought small business owners feared SUNAT. Then I learned that they care more about client debts, and many businesses are informal. So I changed to accountants: they have the real pain.

## 1. Who it's for, the problem, and how they solve it today

Aldía is for accountants in Peru who check invoices for crédito fiscal by hand.

**How they solve it today:**
1. The client keeps paper invoices by month.
2. The accountant downloads the SIRE .txt and converts it to Excel.
3. She checks each invoice one by one.
4. It takes about 2 hours for 50–70 invoices.

## 2. Competitors and why someone would choose Aldía

- **CumpliTax:** claims automatic validation with 140+ rules (vendor claim, not proven).
- **NubeCont:** accepts XML and compares with SIRE. S/ 49/month + S/ 15 per extra company.
- **ContaUno:** made for accountants with many companies. It also reads photos and checks the banking channel and detracción. The accountant still reviews.

**Why Aldía:** its rules come from real rejection reasons that accountants told me. The expense and plate rules: no competitor I found mentions those. It's an add-on next to their current tool, not a replacement.

## 3. My experiment, what I learned, and what I'm still assuming

**Experiment:** I pitched the idea at a tech event, made a landing page (9 visits, 1 contact), interviewed 4 small business owners, and talked to 4 accountants (calls, voice messages and text).

**My line:** I'm right if 3 of 5 accountants say checking invoices costs them hours, and 3 of 5 already pay for a tool.
**Result:** 3 of 4 say it costs hours, and 3 of 4 already pay for a tool (S/ 450/month, S/ 1,500 install). **Passed.**

**What I learned:**
- The most common rejection reasons: expense not for the business (3 of 3), banking channel, called bancarización in Peru (3 of 3), plate (2 of 3), detracción (2 of 3), wrong company (1 of 3).
- SIRE doesn't show the plate or what was bought. That comes from the paper invoice.
- Most accountants don't fully trust any system. But 1 accountant said she prefers the system does it alone.

**Still assuming:**
- Accountants would pay for Aldía. They already pay for tools, but not yet for Aldía.
- Human in the loop: only 1 of 4 wanted "the system alone." I will test it.
- OCR can read real paper invoices correctly. Not tested yet.

## 4. Smallest useful version, mockup, how I'd build and test it

- **Smallest version:** Aldía checks 3 rules and alerts the accountant: "review these invoices." The rules: the expense is not for the business, the fuel plate is not a company vehicle, or the invoice was not paid through a banking channel (S/ 2,000 or more).
- **Why banking channel and not wrong company:** 3 of 3 accountants mentioned the banking channel, and only 1 mentioned wrong company. The client can upload the bank payment voucher, so I don't need bank credentials.
- **Input:** SIRE + invoice photos + company plates + business line + bank payment vouchers.
- **Mockup:** see the "MVP demo" tab.
- **Build:** concierge MVP with AI help. I or AI match each photo to its SIRE row by serie-número and RUC, AI applies the 3 rules, and I check every row. No code yet.
- **Test:** an accountant gives me invoices she rejected. My rules work if they catch 2 of 3.

## 5. Pricing, marketing, first paying customers

- **Price:** S/ 5 per RUC per month. With ~37 clients, that's about S/ 185 per month, small next to what they already pay.
- **Market:** about 1.5 million taxpayers in RMT and RER can use crédito fiscal (SUNAT 2025). There are more than 63,000 registered accountants (CCPL, 2023).
- **Message:** "Aldía te ayuda a revisar solo las facturas dudosas."
- **First customers:** my accountant friends first. Then the accountants' club, which holds events for all accountants. Then teachers at my university.
- **From trying to paying:** free first month, then S/ 5 per RUC.

## 6. What I'd test next, and what would make me change direction

- **Next test:** ask my accountant friends for the last 3 invoices they rejected, to see if my rules catch real rejected invoices. If 2 of 3 get flagged, I keep going. If not, I change the rules.
- Send what the AI MVP does to my accountants, so they can try it and give me early feedback (real invoices only with their permission).
- **Change direction:** after the free month, 3 of 5 accountants must pay S/ 5 per RUC. If not, I stop.

## How I worked with AI

AI is my partner, but I check what it does.
- **Research:** Manus. When it didn't give a source, I asked for the source.
- **Concierge demo:** after I understood the manual workflow, I wrote a prompt for Claude so it can flag invoices with my 3 rules.
- **Prototype:** Lovable, for the MVP demo.

## Sources

Desk research done with Manus (AI). I checked the competitor links and the SUNAT numbers myself.

- **Interviews:** 4 accountants and 4 small business owners (October 2026). Notes kept private.
- **Market:** [SUNAT, Memoria Anual 2025](https://www.sunat.gob.pe/cuentassunat/planestrategico/memoria/memoria2025.pdf) (Sección III, Inscripción: RMT 940,500 and RER 598,100 taxpayers). Accountants: [El Peruano, "Contadores estratégicos"](https://www.elperuano.pe/noticia/227121-economika-contadores-estrategicos) (Nov 6, 2023), citing CCPL.
- **Competitors:** [CumpliTax](https://www.cumplitax.com/), [NubeCont prices](https://nubecont.com/precios), [ContaUno prices](https://contauno.com/precios-software-contable-peru) (vendor pages, checked October 2026).
