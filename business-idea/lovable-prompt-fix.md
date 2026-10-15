Fix only the Report tab. Don't change other tabs, the stats strip or the demo.

1. Text: in `REPORT_MARKDOWN` (src/content.ts), make exactly these 2 replacements:
- Replace "checking invoices for crédito fiscal, and would pay" with "checking invoices for crédito fiscal (input VAT credit: the sales tax paid on purchases, which a company subtracts from the sales tax it owes), and would pay"
- Replace "check invoices for crédito fiscal (tax credit) by hand." with "check invoices for crédito fiscal by hand."

2. Styling of the rendered markdown:
- Lists: show bullets for unordered lists (list-style: disc) and numbers for ordered lists (list-style: decimal), with left padding 1.5rem and 0.4rem between items.
- Headings: h2 at 24px (Bricolage Grotesque), margin-top 2.5rem, margin-bottom 0.75rem. h3 at 18px, margin-top 1.5rem.
- Paragraphs: margin-bottom 1rem. Bold text in #1B1C18.
- Add a thin divider line (#E5E4D6) above each h2, except the first one.
- Links: olive green (#466621), underlined, open in a new tab.
Tip: use Tailwind Typography ("prose") or explicit CSS for the markdown container.
