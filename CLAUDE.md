# TextilWorld Operations CRM — Project Brief

## What This Is
A single-file React prototype CRM/operations tool built for TextilWorld Poland (textilworld.pl), a fabric import and e-commerce company based in Sękocin Nowy, near Warsaw. Built by intern Abhinay Pratap Singh at the request of CEO Praveer Lakhani.

**No build step. No backend. No database.** Everything runs in the browser from one HTML file using React 18 (CDN), Babel standalone (JSX), Tailwind CSS (CDN), and localStorage for persistence.

## Run the App
```bash
cd /home/user/project-xs
python3 -m http.server 8765
# Open: http://127.0.0.1:8765/index.html
```

## Tech Stack
| Layer | Tech |
|---|---|
| UI Framework | React 18 (CDN: unpkg.com/react@18) |
| JSX Compiler | Babel Standalone (CDN) |
| CSS | Tailwind CSS (CDN) |
| Persistence | localStorage (useLocalStorage hook) |
| Entry point | `/index.html` (1,318 lines, single file) |

## Brand Colors
- Navy: `#1B3A6B` (sidebar, primary buttons)
- Gold: `#C9A84C` (active nav highlight, gold button)
- Background: `#F1F5F9`

## File Structure
```
project-xs/
└── index.html    ← entire app (HTML + React + data + all components)
```

---

## The 12 App Modules (Tabs)

### 1. Dashboard (`dashboard`)
- Alert banner: open courier disputes + critical website bugs
- 4 stat cards: Orders, Open Disputes (PLN), Zero Stock, Active B2B
- Website issues summary + Marketplace Presence grid
- Recent disputes list
- Website bugs list (Pre-Order/No Buy Button products)

### 2. Orders (`orders`)
- Filter by channel: Shopify / Allegro / Amazon / Etsy / B2B Direct
- Search by order ID or customer name
- Red row highlight for open disputes
- Columns: ID, Date, Customer, Channel, Courier badge, Status, Dispute

### 3. Courier Disputes (`disputes`)
- Stats: Open / Submitted / Won count + PLN amounts
- Table: declared vs billed weight, difference (🚨 if overcharged), dispute amount
- Edit modal: update weights, amount, status, notes

### 4. Courier Stats (`couriers`)
- Per-courier: total shipments, disputes, open, won
- Win rate % and avg overcharge per dispute
- Color-coded border: red if overcharges exist, green if clean

### 5. Inventory (`inventory`)
- Sorted by units sold in 30 days (best-sellers first)
- Status badges: ✅ OK / ⚠️ LOW STOCK / 🐛 PRE-ORDER BUG / 🐛 BRAK PRZYCISKU
- China lead time reminder: order before 20 Nov 2026 for Chinese New Year

### 6. B2B Clients (`b2b`)
- Add/view company clients with type, status, monthly spend
- Status: Active / Pending / Prospect / Inactive
- Types: Fashion School, Medical/Scrubs, Army/Military, Furniture/Upholstery, Events/Film, Industrial, Workwear

### 7. Package Sizes (`packages`)
- Reference table from Furkan's notes
- Folio A/B/C + 6 roll sizes (20×20×150 to 60×60×150)
- GLS currently blocked due to disputes

### 8. Evidence & Invoice (`evidence`)
- Compare declared vs courier-billed weights
- 6-item evidence checklist per dispute (checkable)
- Progress bar per dispute
- "Generate Evidence Summary" → copyable text block for couriers
- Click row to expand detail panel

### 9. Website Issues (`website`)
- Track bugs: Pre-Order Bug / No Buy Button / Translation / Currency / SEO
- Priority: Critical (red) / High (orange) / Medium (yellow)
- Mark Fixed / In Progress / Reopen buttons
- Add new issues form

### 10. Allegro Monitor (`allegro`)
- Price vs competitor min price (green = cheaper, red = more expensive)
- Listing status: Active / Paused / Out of Stock
- Views and orders (30 days)
- Flagged issues per listing

### 11. Warehouse Labels (`warehouse`)
- Label every fabric roll: ID, SKU, fabric, color, width, length
- Location: Row (A/B/C/D) + Shelf (1–5) + Position
- Color-coded by row: A=blue, B=green, C=yellow, D=purple
- Search by label ID or fabric name
- Print label modal (formatted card)
- Add label form

### 12. Customer Segments B2C (`segments`)
- 5 segments from CEO notes
- Strategy cards, channels, avg order size, buying frequency, potential PLN/month
- Editable notes per segment
- Marketplace Presence grid (Allegro ✓, Eobuwie ✓, Etsy ✓, Emag –, Amazon ✓, Cdiscount –)

---

## Sample Data (Pre-loaded)

### Orders (8 records)
| ID | Customer | Channel | Courier | Dispute |
|---|---|---|---|---|
| BL-10421 | Anna Kowalska | Shopify | DPD | Open 4.20 PLN |
| BL-10420 | Fashion School Kraków | B2B Direct | GLS | Submitted 42.00 PLN |
| BL-10419 | Tomasz Nowak | Allegro | InPost | None |
| BL-10418 | Magdalena Wiśniewska | Shopify | DPD | Open 6.30 PLN |
| BL-10417 | MedScrubs Polska | B2B Direct | DHL | Won |
| BL-10416 | Katarzyna Zielińska | Etsy | InPost | None |
| BL-10415 | Armia Polska | B2B Direct | GLS | Open 98.00 PLN |
| BL-10414 | Piotr Lewandowski | Amazon | DPD | None |

### Inventory (8 SKUs)
| SKU | Product | Stock | Issue |
|---|---|---|---|
| TW-VIS-BLK-150 | Wiskoza Czarna 150cm | 48 | OK |
| TW-AKS-BORDO-150 | Aksamit Bordo | 0 | 🐛 PRE-ORDER BUG |
| TW-VIS-ZOL-150 | Wiskoza Żółta 150cm | 0 | 🐛 NO BUY BUTTON |
| TW-AKS-NIEBIESKI-150 | Aksamit Niebieski | 0 | 🐛 NO BUY BUTTON |
| TW-KREPA-ROZA-150 | Krepa Różowa | 22 | OK |
| TW-DZIAN-GRANAT-150 | Dzianina Jersey Granatowa | 35 | OK |
| TW-SZY-BIAL-150 | Szyfon Biały | 6 | ⚠️ LOW STOCK |
| TW-BAW-BIAL-150 | Bawełna Biała | 55 | OK |

### B2B Clients (5)
- KSA Kraków (Fashion School) — Active — 3,200 PLN/mth
- MedScrubs Polska (Medical) — Active — 4,800 PLN/mth
- Armia Polska (Military) — Pending — 12,000 PLN/mth
- Meble Artystyczne Wrocław (Furniture) — Prospect
- VIAMODA Warsaw (Fashion School) — Prospect

### Evidence Disputes (3)
- EV-001: DPD / BL-10421 — +0.6 kg — 4.20 PLN — Open
- EV-002: GLS / BL-10420 — +6.5 kg — 42.00 PLN — Submitted
- EV-003: GLS / BL-10415 — +14.0 kg — 98.00 PLN — Open

### Website Issues (7)
- CRITICAL: Aksamit Bordo — Pre-Order Bug
- CRITICAL: Wiskoza Żółta — No Buy Button
- HIGH: Aksamit Niebieski — No Buy Button
- CRITICAL: All 17 stores — Currency stuck in PLN (fix: Shopify Markets)
- HIGH: Product pages — Not translated (fix: Shopify Translate & Adapt)
- MEDIUM: Footer — Garbled UTF-8 encoding (fix: footer.liquid)
- MEDIUM: All pages — No meta descriptions (fix: Shopify SEO bulk editor)

### Allegro Monitor (6 listings)
- Wiskoza Czarna — 14.90 PLN — Competitor 16.50 — ✓ Cheaper
- Aksamit Bordo — 24.90 PLN — Competitor 22.00 — ✗ Dearer + Out of Stock
- Dzianina Jersey — 12.50 PLN — Competitor 13.80 — ✓ Cheaper
- Szyfon Biały — 11.90 PLN — Competitor 10.50 — ✗ Dearer
- Krepa Różowa — 16.90 PLN — Competitor 17.50 — ✓ Cheaper
- Bawełna Biała — 9.90 PLN — Paused listing, missing photos

### Warehouse Labels (8)
- TW-A1-001/002: Wiskoza Czarna — Row A, Shelf 1
- TW-A2-001: Dzianina Jersey — Row A, Shelf 2
- TW-B1-001: Krepa Różowa — Row B, Shelf 1
- TW-B2-001/002: Bawełna Biała — Row B, Shelf 2
- TW-C1-001: Szyfon Biały — Row C, Shelf 1 (LOW)
- TW-C2-001: Aksamit Bordo — Row C, Shelf 2 (OUT OF STOCK)

### Customer Segments (5)
| Segment | Potential/mth | Channels | Frequency |
|---|---|---|---|
| Small Clothing Brands | 8,000 PLN | Allegro, Shopify | Bi-weekly |
| Hobby Sewing & Brands | 4,500 PLN | Allegro, Etsy, Shopify | Weekly |
| Sewing Schools | 15,000 PLN | Shopify, B2B Direct | Monthly |
| Events, Decor, Film Sets | 6,000 PLN | Shopify, B2B Direct | Seasonal |
| Textile Retailers | 12,000 PLN | Allegro, B2B Direct | Monthly |

---

## Key Business Context

### Company
- **Name:** TextilWorld (textilworld.pl)
- **Location:** Sękocin Nowy, near Warsaw, Poland
- **Products:** Fabric sold by the meter (wiskoza, aksamit, szyfon, krepa, dzianina, bawełna, etc.)
- **Channels:** Shopify website, Allegro, Etsy, Amazon, Eobuwie, B2B direct

### Courier Setup (Poland)
| Courier | Badge Color | Status |
|---|---|---|
| DPD | Red | Active |
| InPost | Yellow | Active (paczkomaty) |
| GLS | Blue | **BLOCKED** (disputes) |
| DHL | Yellow-red | Active |
| UPS | Brown | Active |
| ORLEN Paczka | Green | Active |

### Courier Dispute Process
1. Before shipping: photo of package on scale (weight visible)
2. Before shipping: photo of dimensions with tape measure
3. Keep PDF shipment confirmation (with declared weight + dims)
4. Keep courier label photo
5. After invoice: compare declared vs billed weight
6. If diff > 0 kg → open dispute within 7 days via courier portal

### Package Size Reference (from Furkan's notes)
| Type | Dimensions | Weight | Fabrics |
|---|---|---|---|
| Folio A | 35×25×5 cm | ≤1 kg | Samples |
| Folio B | 40×30×10 cm | 1–3 kg | Chiffon, light crepe |
| Folio C | 60×40×15 cm | 3–10 kg | Viscose, cotton, satin |
| Roll 20×20×150 | 20×20×150 cm | 10–15 kg | Muslin, satin, silk |
| Roll 30×30×150 | 30×30×150 cm | 15–25 kg | Barbie, Skom, Muslin |
| Roll 30×30×180 | 30×30×180 cm | 25–31 kg | Jersey, dresówka, boucle, zamsz, tencel |
| Roll 40×40×150 | 40×40×150 cm | 20–31 kg | Up to 11 rolls |
| Roll 50×50×150 | 50×50×150 cm | 20–31 kg | Up to 14 rolls |
| Roll 60×60×150 | 60×60×150 cm | 20–31 kg | Heavy bulky |

### Inventory Management
- China lead time: 50–70 days
- Safety stock formula: `=1.65*STDEV(sales_90d)*SQRT(65)`
- Chinese New Year 2027: 29 January → order by 20 November 2026
- Reorder point = avg daily sales × 65 days + safety stock

### Known Website Issues (fix in Shopify)
1. 3 products broken (pre-order/no buy button) — fix inventory settings
2. All 17 country stores show PLN only — enable Shopify Markets currencies
3. Translations missing for DE/CZ/RO — install Shopify Translate & Adapt
4. Footer UTF-8 encoding error — fix footer.liquid
5. No meta descriptions on any product page — use SEO bulk editor

---

## Component Map

```
App (App)
├── Sidebar
├── Dashboard
├── Orders
├── Disputes
├── CourierStats
├── Inventory
├── B2B
├── PackageSizes
├── Evidence
├── WebsiteIssues
├── AllegroMonitor
├── WarehouseLabels
└── CustomerSegments
```

### Shared Components
- `CourierBadge` — colored pill for DPD/InPost/GLS/DHL/UPS/ORLEN
- `StatusBadge` — colored text for Open/Submitted/Won/Lost
- `useLocalStorage(key, initial)` — hook for persistent state

---

## Git State
- Branch: `claude/beautiful-keller-bWKEg`
- Commits: 2 (initial + v2.0 expansion)
- Remote: `abhinayjobz-glitch/project-xs` (GitHub push pending — needs repo write permission)

## Next Steps / Future Features
- [ ] Connect to BaseLinker API for live order sync
- [ ] Auto-flag weight mismatches from real courier invoice PDF parsing
- [ ] Photo upload for package weight evidence (requires backend)
- [ ] Real Allegro API integration for live pricing data
- [ ] Shopify webhook for live stock updates
- [ ] Email/SMS alerts for critical disputes
- [ ] Export disputes to PDF for courier submission
- [ ] Multi-user support (different roles: CEO, intern, warehouse)
