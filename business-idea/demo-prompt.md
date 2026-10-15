# Demo prompt (AI-assisted concierge MVP)

Paste everything below into claude.ai. All data is fake.

---

## 1. Context
You help an accountant in Peru check purchase invoices for crédito fiscal.
The client is Transportes Andinos SAC, a transport business.
Aldía flags doubtful invoices so the accountant reviews only those.

## 2. Inputs
- The SIRE purchase list (with the detail from the invoice photos, because SIRE doesn't show the plate or what was bought). I or AI match each photo to its SIRE row by serie-número and RUC.
- The list of company plates
- The business line
- The bancarización vouchers the client uploaded

## 3. Rules
1. **Expense:** compare what was bought with the business line. If it's not for the business → "⚠️ Revisar: gasto no es del giro"
2. **Plate:** compare the fuel plate with the list of company plates. If it's not there → "⚠️ Revisar: placa no es de la empresa"
3. **Bancarización:** if the invoice is S/ 2,000 or more, check the voucher the client uploaded. If there's no voucher → "⚠️ Revisar: falta bancarización"

If no rule fails → "✅ OK"

## 4. Output
The same table, plus a "Revisar" column. Then one line for each flagged invoice saying why.

## 5. Limits
Don't approve or reject invoices. The accountant decides.

---

## Data

**Business line:** transporte de carga por carretera

**Company plates:** ABC-123, DEF-456

**Bancarización vouchers uploaded:** none

**SIRE purchases + detail from photos:**

| Fecha | Serie-Número | RUC proveedor | Proveedor | Detalle (de la foto) | IGV | Total | Moneda |
|---|---|---|---|---|---|---|---|
| 03/10/2026 | F001-000245 | 20512345671 | Grifo Los Andes SAC | Combustible placa ABC-123 | 22.88 | 150.00 | PEN |
| 05/10/2026 | F002-001120 | 20498765432 | Llantas del Sur EIRL | 2 llantas | 183.05 | 1200.00 | PEN |
| 08/10/2026 | E001-000087 | 10456789012 | Taller Mecánico Ruiz | Mantenimiento de camión | 61.02 | 400.00 | PEN |
| 10/10/2026 | F001-000311 | 20512345671 | Grifo Los Andes SAC | Combustible placa XYZ-789 | 30.51 | 200.00 | PEN |
| 12/10/2026 | F003-004521 | 20587654321 | Electro Hogar SAC | 1 televisor | 228.81 | 1500.00 | PEN |
| 14/10/2026 | F001-000098 | 20534567890 | Repuestos Lima SAC | Repuestos de motor | 450.00 | 2950.00 | PEN |
