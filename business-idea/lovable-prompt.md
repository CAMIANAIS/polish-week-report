# Lovable prompt: Aldía simulated demo

Build a simple one-page website for Aldía. Front end only: no backend, no login, no database, no real upload, no AI calls. Everything is fake data in the code. Language: English.

Aldía flags doubtful invoices so accountants review only those.

## Design: Vello design system
Use these tokens. Warm, calm, simple.
- Background: #F6F2E7 (cream). Section band: #EFEADB. Cards: #FFFFFF with border #E5E4D6.
- Text: #1B1C18 (titles), #3D3F37 (body), #6E7064 (secondary).
- Primary button: #557E26 (olive green), pressed #466621, white text. Dark hero band: #16462F.
- OK / success: #557E26 on #EBF1DB. Warning (flagged rows): #C5421F text on #FCE3D9. Danger: #D64545 on #FBE3E3.
- Fonts (Google Fonts): titles "Bricolage Grotesque", body "Hanken Grotesk", numbers and codes "JetBrains Mono".
- Radius: 14px buttons and inputs, 20px cards. Spacing based on 8px.
- Must work on mobile: on small screens the table scrolls sideways inside its card.

## Editable content
Put the report text and the video link in ONE file, `src/content.ts`, with two constants:
- `REPORT_MARKDOWN` (string, rendered as Markdown). For now: "Report coming soon."
- `VIDEO_URL` (string). For now empty. If empty, show "Video coming soon." If it has a Google Drive or YouTube link, show it embedded (Drive links: use the `/preview` URL).
I will edit this file by hand later, so I don't need another prompt.

## Header
Title "Aldía". Subtitle: "Aldía flags doubtful invoices so accountants review only those."

## 3 tabs
1. **Report**: renders `REPORT_MARKDOWN`.
2. **Video**: renders `VIDEO_URL`.
3. **MVP demo**: the simulated demo below, then a "Next steps" section.

## Tab 3: MVP demo
Under the demo, always show: "Demo with sample data. The reading is simulated."

Show a small info card first:
- Client: Transportes Andinos SAC (transport business)
- Business line: road freight transport
- Company plates: ABC-123, DEF-456
- Bancarización vouchers uploaded: none

Then 4 steps, like a stepper (Step 1 → 4). Only one main button is active at a time.

**Step 1. Button "Upload SIRE + invoice photos"**
Show a short "Reading invoices…" loading state (1 second), then this table:

| Date | Series-Number | Supplier RUC | Supplier | Detail (from the photo) | IGV | Total | Currency | Source |
|---|---|---|---|---|---|---|---|---|
| 03/10/2026 | F001-000245 | 20512345671 | Grifo Los Andes SAC | Fuel, plate ABC-123 | 22.88 | 150.00 | PEN | SIRE + photo |
| 05/10/2026 | F002-001120 | 20498765432 | Llantas del Sur EIRL | 2 tires | 183.05 | 1,200.00 | PEN | SIRE + photo |
| 08/10/2026 | E001-000087 | 10456789012 | Taller Mecánico Ruiz | Truck maintenance | 61.02 | 400.00 | PEN | SIRE + photo |
| 10/10/2026 | F001-000311 | 20512345671 | Grifo Los Andes SAC | Fuel, plate XYZ-789 | 30.51 | 200.00 | PEN | SIRE + photo |
| 12/10/2026 | F003-004521 | 20587654321 | Electro Hogar SAC | 1 TV | 228.81 | 1,500.00 | PEN | SIRE + photo |
| 14/10/2026 | F001-000098 | 20534567890 | Repuestos Lima SAC | Engine parts | 450.00 | 2,950.00 | PEN | SIRE + photo |

Note under the table: "SIRE doesn't show the plate or what was bought. That detail comes from the invoice photo."

**Step 2. Button "Check with Aldía"**
Show the 3 rules as a short list:
- Is the expense for the business? (compare with the business line)
- Is the fuel plate a company vehicle? (compare with the company plates)
- Paid through a bank? (S/ 2,000 or more needs a bancarización voucher)

Then add a "Review" column to the table. Two rules must really run in code on the data (not hard-coded results):
- **Plate rule (real logic):** if the detail has a plate (format like "ABC-123") and that plate is NOT in the company plates list → "⚠️ Plate is not a company vehicle"
- **Bancarización rule (real logic):** if the total is 2,000 or more AND the invoice has no voucher in the uploaded vouchers list → "⚠️ No bancarización (S/ 2,000 or more)"
- **Expense rule (simulated):** this needs judgment, so mark it in the data: only row 5 (1 TV) is not for the business → "⚠️ Expense is not for the business"
- If no rule fails → "✅ OK". If more than one fails, show all reasons.

Keep the company plates and the vouchers list as editable data in the info card's code, so the rules use them. Expected result with this data: rows 1, 2, 3 OK; row 4 plate; row 5 expense; row 6 bancarización.

Add a small note under the rules: "Plate and bancarización rules run on the data. The expense rule is simulated."

Highlight flagged rows with the warning colors. Show a summary: "3 of 6 invoices to review. The other 3 passed all rules."

**Step 3. The accountant decides**
Each flagged row gets 2 buttons: "Approve" and "Reject". Show this text: "Aldía doesn't decide. The accountant decides."

**Step 4. Record saved**
After each decision, replace the buttons with a badge: Approved → "Record saved · Goes to Concar ✓". Rejected → "Record saved · Back to the client ✗".
When all 3 are decided, show: "Done. You reviewed 3 invoices instead of 6."

A "Restart demo" button resets everything to Step 1.

## Next steps (end of tab 3)
- Ask accountants about the last 3 invoices they rejected. My rules work if they catch 2 of 3.
- Send what the AI MVP does to my accountants, so they can try it and give me early feedback.
- If the rules fail, I change the rules first (detracción).

## Footer
"Prototype by Camila Mamani · Fake data · October 2026"
