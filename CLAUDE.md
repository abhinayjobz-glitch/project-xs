# TextilWorld Operations CRM — Complete Project Reference

> **Every piece of data, context, business knowledge, and technical spec for this project.**
> Last updated: June 2026 | Built by: Abhinay Pratap Singh (Intern, Erasmus+)
> CEO: Praveer Lakhani | Operations Manager: Furkan (on leave)

---

## 1. HOW TO RUN

```bash
cd /home/user/project-xs
python3 -m http.server 8765
# Open in browser: http://127.0.0.1:8765/index.html
```

No npm, no build step, no backend needed. Pure browser app.

---

## 2. TECH STACK

| Layer | Technology |
|---|---|
| UI Framework | React 18 via CDN (`unpkg.com/react@18/umd/react.development.js`) |
| JSX Compiler | Babel Standalone (`unpkg.com/@babel/standalone/babel.min.js`) |
| CSS Framework | Tailwind CSS via CDN (`cdn.tailwindcss.com`) |
| Data Persistence | `localStorage` (browser) via `useLocalStorage` hook |
| Entry Point | `/index.html` — single file, ~1,318 lines, ~92KB |
| State Management | React `useState` + `useCallback` (no Redux, no Context) |
| No Backend | All data is initialized in JS constants and saved to localStorage |

### Brand Colors
```
Navy (primary):   #1B3A6B  — sidebar background, btn-primary
Gold (accent):    #C9A84C  — active nav border, btn-gold
Background:       #F1F5F9  — main content area
White cards:      #FFFFFF  — stat-card, table backgrounds
```

### CSS Classes (custom, defined in <style>)
```
.nav-item         — sidebar nav buttons
.nav-item.active  — active tab (gold left border)
.flag-red         — red left border row (open dispute)
.flag-yellow      — yellow left border row (submitted dispute)
.badge-dpd        — red pill
.badge-inpost     — yellow pill
.badge-gls        — blue pill
.badge-dhl        — yellow-dark pill
.badge-ups        — brown pill
.badge-orlen      — green pill
.stat-card        — white rounded stat box
.table-row        — hover effect on table rows
.btn-primary      — navy button
.btn-secondary    — gray button
.btn-danger       — red button
.btn-gold         — gold button
.modal-overlay    — full-screen modal backdrop
.modal            — modal content box (640px wide)
.row-a            — blue left border (warehouse row A)
.row-b            — green left border (warehouse row B)
.row-c            — yellow-dark left border (warehouse row C)
.row-d            — purple left border (warehouse row D)
.dispute-open     — red bold text
.dispute-submitted— orange bold text
.dispute-won      — green bold text
.dispute-lost     — gray bold text
```

---

## 3. FILE STRUCTURE

```
project-xs/
├── index.html       ← ENTIRE APP: all React components, all data, all styles
├── CLAUDE.md        ← this file
└── .git/            ← git repo, branch: claude/beautiful-keller-bWKEg
```

### Git Info
- **Repo:** `abhinayjobz-glitch/project-xs` (GitHub)
- **Branch:** `claude/beautiful-keller-bWKEg`
- **Local commits:** 3 (v1.0 prototype → v2.0 expansion → CLAUDE.md)
- **Push status:** Blocked — GitHub App lacks write permission to this repo
- **Fix:** Go to github.com/settings/installations → Claude Code app → add `project-xs`

---

## 4. APP ARCHITECTURE — ALL COMPONENTS

### Component Tree
```
App
├── Sidebar                     — left nav, 12 tabs, branding footer
├── Dashboard                   — overview, alerts, stats, recent activity
├── Orders                      — all orders, filter by channel, search
├── Disputes                    — courier dispute table, edit modal
├── CourierStats                — per-courier performance, win rates
├── Inventory                   — stock levels, reorder alerts, website bugs
├── B2B                         — B2B client management, add form
├── PackageSizes                — static reference table (Furkan's notes)
├── Evidence                    — invoice comparison, evidence checklist
├── WebsiteIssues               — bug tracker (priority, status, fix action)
├── AllegroMonitor              — listing status, price vs competition
├── WarehouseLabels             — warehouse location system, print labels
└── CustomerSegments            — B2C segments, strategy, marketplace grid

Shared:
├── CourierBadge({ courier })   — colored pill badge for each courier
└── StatusBadge({ status })     — colored text for dispute status

Hook:
└── useLocalStorage(key, init)  — useState + localStorage sync
```

### localStorage Keys
```
tw_orders          — orders array
tw_inventory       — inventory array
tw_b2b             — B2B clients array
tw_evidence        — evidence/invoice array
tw_website_issues  — website bugs array
tw_allegro         — Allegro listings array
tw_warehouse_labels— warehouse labels array
tw_segments        — B2C segments array
```

### Sidebar Tabs (in order)
```
dashboard   📊  Dashboard
orders      📦  Zamówienia / Orders
disputes    ⚠️  Spory Kurierskie
couriers    🚚  Statystyki Kurierów
inventory   🏭  Stany Magazynowe
b2b         🏢  Klienci B2B
packages    📐  Rozmiary Opakowań
evidence    🧾  Faktury / Evidence
website     🌐  Błędy Strony
allegro     🛒  Allegro Monitor
warehouse   🏷️  Etykiety Magazyn
segments    👥  Segmenty B2C
```

---

## 5. ALL INITIAL DATA (as loaded in the app)

### 5.1 ORDERS — INITIAL_ORDERS (8 records)

```javascript
BL-10421 | 2026-06-05 | Anna Kowalska        | Shopify    | DPD    | Wysłane      | dispute: Open      | +0.6kg | 4.20 PLN
BL-10420 | 2026-06-05 | Fashion School Kraków | B2B Direct | GLS    | Wysłane      | dispute: Submitted | +6.5kg | 42.00 PLN
BL-10419 | 2026-06-04 | Tomasz Nowak          | Allegro    | InPost | Dostarczono  | no dispute
BL-10418 | 2026-06-04 | Magdalena Wiśniewska  | Shopify    | DPD    | W transporcie| dispute: Open      | +0.9kg | 6.30 PLN
BL-10417 | 2026-06-03 | MedScrubs Polska      | B2B Direct | DHL    | Dostarczono  | dispute: Won       | 0kg    | 0 PLN
BL-10416 | 2026-06-03 | Katarzyna Zielińska   | Etsy       | InPost | Dostarczono  | no dispute
BL-10415 | 2026-06-02 | Armia Polska          | B2B Direct | GLS    | Wysłane      | dispute: Open      | +14kg  | 98.00 PLN
BL-10414 | 2026-06-02 | Piotr Lewandowski     | Amazon     | DPD    | Dostarczono  | no dispute
```

**Fields per order:**
`id, date, customer, channel, products, courier, tracking, status, weight_declared, dims_declared, weight_billed, dims_billed, dispute_status, dispute_amount, photos[], notes`

**Order statuses:** Wysłane / W transporcie / Dostarczono
**Dispute statuses:** Open / Submitted / Won / Lost / null

### 5.2 INVENTORY — INITIAL_INVENTORY (8 SKUs)

```
SKU                   | Name                              | Stock | Reorder | Sold30d | Status
TW-VIS-BLK-150        | Wiskoza Gładka Czarna 150cm       | 48    | 20      | 31      | OK
TW-AKS-BORDO-150      | Aksamit Cuba Welur Ciemny Bordo   | 0     | 10      | 12      | PRE_ORDER_BUG
TW-VIS-ZOL-150        | Wiskoza Gładka Żółta 150cm        | 0     | 8       | 8       | NO_BUY_BUTTON
TW-AKS-NIEBIESKI-150  | Aksamit Cuba Welur Niebieski      | 0     | 8       | 7       | NO_BUY_BUTTON
TW-KREPA-ROZA-150     | Krepa Amerykańska Różowa          | 22    | 15      | 18      | OK
TW-DZIAN-GRANAT-150   | Dzianina Jersey Granatowa 150cm   | 35    | 25      | 29      | OK
TW-SZY-BIAL-150       | Szyfon Biały 150cm                | 6     | 12      | 22      | LOW_STOCK
TW-BAW-BIAL-150       | Bawełna Gładka Biała 150cm        | 55    | 20      | 15      | OK
```

**Status badge meanings:**
- `OK` → green ✅
- `LOW_STOCK` → yellow ⚠️ (stock < reorder_point)
- `PRE_ORDER_BUG` → red 🐛 (shows as pre-order on website — customers CANNOT buy)
- `NO_BUY_BUTTON` → red 🐛 (no add-to-cart on website)

### 5.3 B2B CLIENTS — INITIAL_B2B (5 records)

```
ID | Company                              | Type              | City     | Status  | Monthly PLN | Last Order
1  | KSA Cracow School Art & Fashion      | Fashion School    | Kraków   | Active  | 3,200       | 2026-06-05
2  | MedScrubs Polska Sp. z o.o.          | Medical / Scrubs  | Warsaw   | Active  | 4,800       | 2026-06-03
3  | Armia Polska — Dostawca Mundurów     | Army / Military   | Warsaw   | Pending | 12,000      | 2026-06-02
4  | Meble Artystyczne Wrocław            | Furniture/Upholst | Wrocław  | Prospect| 0           | —
5  | VIAMODA University Warsaw            | Fashion School    | Warsaw   | Prospect| 0           | —
```

**Notes:**
- KSA: orders jersey and wiskoza in bulk monthly, 64 students
- MedScrubs: requires OEKO-TEX certification with every order
- Armia Polska: GLS dispute open on last shipment, large technical fabric order
- Meble Artystyczne: interested in velour and boucle for furniture upholstery
- VIAMODA: outreach sent 2026-06-01, no response yet

### 5.4 EVIDENCE / INVOICES — INITIAL_EVIDENCE (3 records)

```
EV-001 | DPD | BL-10421 | DPD-INV-2026-4421 | 2026-06-06
  Declared: 1.8kg / 30x20x5cm  →  Billed: 2.4kg / 30x20x5cm
  Overcharge: 4.20 PLN | Status: Open
  Checklist: 0/6 done

EV-002 | GLS | BL-10420 | GLS-INV-2026-8877 | 2026-06-06
  Declared: 18.0kg / 40x40x150cm  →  Billed: 24.5kg / 50x50x150cm
  Overcharge: 42.00 PLN | Status: Submitted
  Checklist: 4/6 done (photo+dims+label+invoice uploaded)
  Note: Wrong box size category — declared 40x40x150 billed as 50x50x150

EV-003 | GLS | BL-10415 | GLS-INV-2026-3344 | 2026-06-03
  Declared: 48.0kg / 60x60x150cm  →  Billed: 62.0kg / 60x60x150cm
  Overcharge: 98.00 PLN | Status: Open
  Checklist: 1/6 done (photo on scale only)
  Note: Army order, largest dispute, preparing evidence package
```

**Evidence Checklist Items (same 6 for every dispute):**
1. Photo of package on scale
2. Photo of dimensions with tape measure
3. Courier label photo
4. Invoice PDF uploaded
5. Shipment confirmation screenshot
6. BaseLinker order screenshot

### 5.5 WEBSITE ISSUES — INITIAL_WEBSITE_ISSUES (7 records)

```
ID | Type          | Product/Page            | Priority | Status | Fix Action
1  | Pre-Order Bug | Aksamit Cuba Welur Bordo| Critical | Open   | Set "Continue Selling When OOS" in Shopify
2  | No Buy Button | Wiskoza Gładka Żółta    | Critical | Open   | Check physical stock, update Shopify inventory
3  | No Buy Button | Aksamit Cuba Welur Niebieski | High | Open  | Check Shopify inventory settings
4  | Currency      | All 17 country stores   | Critical | Open   | Shopify Markets — enable EUR/GBP/NOK/CZK/HUF per market
5  | Translation   | Product pages           | High     | Open   | Install Shopify Translate & Adapt app
6  | SEO/Encoding  | Footer text             | Medium   | Open   | Fix UTF-8 encoding in theme footer.liquid
7  | SEO           | All product pages       | Medium   | Open   | Add meta descriptions via Shopify SEO bulk editor
```

### 5.6 ALLEGRO LISTINGS — INITIAL_ALLEGRO (6 records)

```
ID | Title                          | Our Price | Competitor | Diff      | Status       | Views30d | Orders30d | Issues
1  | Wiskoza Gładka Czarna 150cm    | 14.90 PLN | 16.50 PLN  | -1.60 PLN | Active       | 2340     | 87        | none
2  | Aksamit Cuba Welur Bordo 150cm | 24.90 PLN | 22.00 PLN  | +2.90 PLN | Out of Stock | 1890     | 34        | out-of-stock active + price higher
3  | Dzianina Jersey Granatowa      | 12.50 PLN | 13.80 PLN  | -1.30 PLN | Active       | 1450     | 62        | none
4  | Szyfon Biały 150cm             | 11.90 PLN | 10.50 PLN  | +1.40 PLN | Active       | 890      | 18        | price higher than competitor
5  | Krepa Amerykańska Różowa       | 16.90 PLN | 17.50 PLN  | -0.60 PLN | Active       | 1120     | 41        | none
6  | Bawełna Gładka Biała 150cm     | 9.90 PLN  | 9.90 PLN   | 0.00 PLN  | Paused       | 340      | 5         | listing paused + missing photos
```

### 5.7 WAREHOUSE LABELS — INITIAL_WAREHOUSE_LABELS (8 labels)

```
Label ID    | SKU                  | Fabric                    | Color        | Width | Length | Row | Shelf | Pos | Notes
TW-A1-001   | TW-VIS-BLK-150       | Wiskoza Gładka Czarna     | Czarna       | 150cm | 50m    | A   | 1     | 01  | Full roll
TW-A1-002   | TW-VIS-BLK-150       | Wiskoza Gładka Czarna     | Czarna       | 150cm | 32m    | A   | 1     | 02  | Partial roll
TW-A2-001   | TW-DZIAN-GRANAT-150  | Dzianina Jersey Granatowa | Granatowa    | 150cm | 60m    | A   | 2     | 01  |
TW-B1-001   | TW-KREPA-ROZA-150    | Krepa Amerykańska Różowa  | Różowa       | 150cm | 45m    | B   | 1     | 01  |
TW-B2-001   | TW-BAW-BIAL-150      | Bawełna Gładka Biała      | Biała        | 150cm | 80m    | B   | 2     | 01  | Medical grade
TW-B2-002   | TW-BAW-BIAL-150      | Bawełna Gładka Biała      | Biała        | 150cm | 55m    | B   | 2     | 02  |
TW-C1-001   | TW-SZY-BIAL-150      | Szyfon Biały              | Biała        | 150cm | 20m    | C   | 1     | 01  | Low — reorder soon
TW-C2-001   | TW-AKS-BORDO-150     | Aksamit Cuba Welur Bordo  | Ciemny Bordo | 150cm | 0m     | C   | 2     | 01  | OUT OF STOCK — awaiting delivery
```

**Row color coding:**
- Row A = Blue border (`#2563EB`)
- Row B = Green border (`#16A34A`)
- Row C = Yellow/amber border (`#D97706`)
- Row D = Purple border (`#7C3AED`)

### 5.8 B2C SEGMENTS — INITIAL_SEGMENTS (5 segments)

```
1. Small Clothing Brands
   Description: Small fashion brands making clothing lines, regular buyers
   Avg order: 2–10m per fabric | Frequency: Bi-weekly
   Channels: Allegro, Shopify
   Strategy: Bulk discount tiers — 5m=5%, 10m=10%, 20m=15%. Loyalty program.
   Potential: 8,000 PLN/month

2. Hobby Sewing & Hobby Brands (e.g. Scrunchied)
   Description: Hobby sewers, small brands, high variety, price sensitive
   Avg order: 0.5–2m per fabric | Frequency: Weekly
   Channels: Allegro, Etsy, Shopify
   Strategy: Variety packs — 5 fabrics × 0.5m sample box. Bundle deals.
   Potential: 4,500 PLN/month | Note: Scrunchied is key account — follow up

3. Sewing Schools & Community (KSA, VIAMODA)
   Description: Fashion schools, sewing clubs, institutional buyers
   Avg order: 10–30m per month | Frequency: Monthly
   Channels: Shopify, B2B Direct
   Strategy: School program — dedicated account manager, NET30 payment, free delivery >500 PLN
   Potential: 15,000 PLN/month | Note: KSA active, VIAMODA follow up pending

4. Events, Decor & Film Sets
   Description: Event companies, decorators, film/theatre production
   Avg order: Seasonal bulk 20–100m | Frequency: Seasonal
   Channels: Shopify, B2B Direct
   Strategy: Event packages with fast turnaround guarantee. Custom cutting.
   Potential: 6,000 PLN/month | Note: Target Warsaw film production companies

5. Textile Retailers
   Description: Small shops buying to resell, buy 10m packing rolls
   Avg order: 10m rolls | Frequency: Monthly
   Channels: Allegro, B2B Direct
   Strategy: Wholesale price list, min 10m per SKU, reseller discount 20%
   Potential: 12,000 PLN/month
```

### 5.9 PACKAGE SIZES — PACKAGE_SIZES (9 entries, from Furkan's handwritten notes)

```
Type               | Dimensions (cm)     | Weight (kg) | Fabrics / Use
Folio A            | ≤35×25×5           | ≤1           | Samples, small orders
Folio B            | ≤40×30×10          | 1–3          | Chiffon, light crepe
Folio C            | ≤60×40×15          | 3–10         | Viscose, cotton, satin
Roll 20×20×150     | 20×20×150          | 10–15        | Muslin, Satin, Silk
Roll 30×30×150     | 30×30×150          | 15–25        | Barbie, Skom, Muslin 150cm
Roll 30×30×180     | 30×30×180          | 25–31        | Single Jersey, Dresówka, Boucle, Zamsz, Tencel, Pikówka
Roll 40×40×150     | 40×40×150          | 20–31        | Up to 11 rolls
Roll 50×50×150     | 50×50×150          | 20–31        | Up to 14 rolls
Roll 60×60×150     | 60×60×150          | 20–31        | Up to 14 rolls (heavy/bulky)
```

**Rule:** Always declare exact size and weight. If unsure — round UP, never down. GLS currently BLOCKED.

---

## 6. BUSINESS CONTEXT — TEXTILWORLD POLAND

### Company Profile
- **Full name:** TextilWorld
- **Website:** textilworld.pl (Shopify)
- **Location:** Sękocin Nowy, near Warsaw, Poland
- **Type:** Fabric import & e-commerce
- **Business model:** Import fabrics from China, sell by the meter online
- **CEO:** Praveer Lakhani
- **Operations Manager:** Furkan (on leave June 2026)
- **Intern:** Abhinay Pratap Singh (Erasmus+ program)

### Sales Channels
| Channel | Platform | Notes |
|---|---|---|
| Own website | Shopify (textilworld.pl) | Multiple country stores (17 markets) |
| Allegro | Poland's #1 marketplace | Primary marketplace channel |
| Etsy | International handmade | Good for EU customers |
| Amazon | Amazon.de (Germany) | Active |
| Eobuwie | Polish marketplace | Active |
| Emag | Romanian marketplace | Not yet active |
| Cdiscount | French marketplace | Not yet active |
| B2B Direct | Direct sales | Schools, medical, military, furniture |

### Product Categories (fabrics sold by the meter)
```
Wiskoza (Viscose)        — most popular, comes in many colors
Aksamit / Welur          — velvet/velour, Cuba velour is bestseller
Szyfon (Chiffon)         — light, used for dresses, bridal
Krepa (Crepe)            — Krepa Amerykańska is popular
Dzianina / Jersey        — Jersey knit, used for clothing
Bawełna (Cotton)         — plain cotton, medical grade available
Satyna (Satin)           — shiny, used for events/decor
Muslin                   — lightweight, used by photographers/events
Polar (Fleece)           — used for army/outdoor
Tencel                   — premium sustainable fabric
Boucle                   — textured, used for furniture/fashion
Zamsz (Suede-look)       — faux suede
Dresówka                 — sweatshirt fabric
Pikówka                  — quilted fabric
Jedwab (Silk)            — premium
Tkanina Techniczna       — technical fabric (army/workwear)
```

### Pricing Model
- Sold per meter (per metr)
- Minimum order: 0.5m (half meter)
- 1 unit in Shopify = 0.5m of fabric
- Standard fabric width: 150cm
- Price range: ~9.90 PLN (cotton) to ~24.90 PLN (velvet) per meter

---

## 7. OPERATIONAL PROCEDURES

### Order Fulfillment Workflow
1. New order arrives in BaseLinker (syncs from Shopify + Allegro + Amazon + Etsy)
2. Find order in BaseLinker by order ID or customer username (Allegro uses login name)
3. Pick the fabric from warehouse — use label system to find location
4. Cut to correct length (1 unit = 0.5m, so qty 6 = 3 meters)
5. Wrap in foil if needed
6. Choose correct courier (never GLS currently — blocked)
7. Create shipment in BaseLinker — select courier, enter weight and dimensions
8. Print label, attach to package
9. Place package in DHL/DPD/InPost pickup area by entrance
10. Mark order as fulfilled (auto-syncs from BaseLinker to Shopify; Allegro may need manual update)

### Courier Dispute Procedure
**BEFORE shipping (do every time):**
- Photo of package on scale (weight must be visible)
- Photo of dimensions with tape measure (length × width × height)
- Save PDF of shipment confirmation from courier portal

**AFTER receiving invoice:**
- Compare declared weight/dims vs billed weight/dims
- If billed > declared → open dispute in courier portal within 7 days
- Upload: scale photo + dimension photo + label photo + invoice + confirmation PDF + BaseLinker screenshot

**Couriers with known issues:**
- GLS: 2 major overcharges in June 2026 — BLOCKED by Furkan
- DPD: regular small overcharges — monitor carefully

### Warehouse Location System
Format: `TW-[ROW][SHELF]-[POSITION]`
- Row A: blue — premium/fast-moving fabrics
- Row B: green — medium stock fabrics
- Row C: yellow — low stock / pending reorder
- Row D: purple — reserved / special orders

**Finding fabric fast:** Look up label ID in Warehouse Labels tab → go to Row/Shelf/Position → cut required length.

### Stock Management
**China lead time:** 50–70 days (standard)
**Chinese New Year 2027:** 29 January 2027 → must order by 20 November 2026

**Safety stock formula (Excel):**
```
=1.65 * STDEV(B2:B92) * SQRT(65)
```
Where B2:B92 = 90 days of daily sales data, 65 = lead time in days

**Reorder point:** `avg_daily_sales × 65 + safety_stock`

**ABC Analysis:**
- A items (top 20% SKUs = 80% revenue): Wiskoza Czarna, Dzianina Jersey, Krepa Różowa
- These should never hit zero stock — keep 3 months of safety stock

### Customer Service Script (from Praveer)
When there's a delayed or missing product:
1. Call customer proactively before they complain
2. Acknowledge the delay honestly
3. Offer: replacement item + free extra fabric (same category)
4. Give discount code for next order
5. If substitution: find similar pattern/color, ask for approval
6. Never let customer wait without information

Example phrase (Polish): *"Mam informację że pakiet był już dawno przygotowany, ale niestety widzę notatkę że czekają na informację o [produkcie]. Nie chcę żeby Pani czekała — chciałem zaproponować wymianę i dodać coś gratis z naszej strony za opóźnienie."*

---

## 8. COURIER REFERENCE

| Courier | Color | Integration | Notes |
|---|---|---|---|
| DPD | Red | BaseLinker | Most used, some weight disputes |
| InPost | Yellow | BaseLinker | Paczkomaty (lockers) — very popular in Poland |
| GLS | Blue | BaseLinker | **CURRENTLY BLOCKED** — ongoing disputes |
| DHL | Yellow-red | BaseLinker (Allegro native) | Use for Allegro orders (cheaper via Allegro deal) |
| UPS | Brown | BaseLinker | Available |
| ORLEN Paczka | Green | BaseLinker | Available |

**Important:** Allegro has special courier pricing deals (cheaper than direct). For Allegro orders, always use the Allegro-integrated courier option in BaseLinker — do NOT use direct DPD/GLS/DHL rates.

---

## 9. PLATFORM INTEGRATIONS

### BaseLinker (Order Management Hub)
- Syncs orders from: Shopify + Allegro + Amazon + Etsy
- Used for: fulfillment, courier selection, label printing, order status updates
- Allegro orders: must use Allegro-specific courier method in BaseLinker (not generic)
- Status sync: Shopify auto-updates, Allegro may need manual "refresh" after dispatch

### Shopify (Website Platform)
- Main store: textilworld.pl
- 17 country markets (but all showing PLN — needs Shopify Markets fix)
- Product inventory tracked in Shopify (syncs to BaseLinker)
- Known issues: 3 broken products (pre-order bug, no buy button × 2)
- Apps needed: Shopify Markets (currency), Shopify Translate & Adapt (translations)

### Allegro (Polish Marketplace)
- Poland's largest marketplace
- Separate account/login from Shopify
- Orders identified by customer username (login name), not email
- Has its own courier deal (usually DPD or DHL at lower rate)
- Products: some out-of-stock listings still showing as active (needs fixing)

---

## 10. CEO'S PRIORITY LIST (from Praveer's handwritten notes)

### Long-Term Plan
1. **Logistic Center** — Build own logistics center
2. **Brands** → Expand to: Textile, Pet Products, Home Electronics, Hair Care, Fitness
3. **Manage Systems** — Online systems, logistics system, operations

### Immediate Priorities (what this CRM solves)
1. **Website Changes** — fix translations, currency, broken products
2. **Price Management & Product Management** — competitive pricing on all channels
3. **Stock Availability Management:**
   - Pre-order — why? Fix or communicate properly
   - Sold out → reorder or not? Decision system needed
   - Best-selling analysis → regular (ABC analysis)
4. **Logistics App/CRM** — this app!
   - DPD, InPost, GLS, UPS, ORLEN, DHL
   - Package sizes: Folio A/B/C + rolls with weight ranges
   - Evidence docs platform — BaseLinker tracking
   - Upload invoice / mistakes / compare etc.

### Customer Strategy (CEO's notes)

**B2B Customers:**
- Printing Factories / Brands that need printing
- Children's Products
- Army Products
- Workwear
- Medical Brands / Institutes / Scrubs
- Fashion Brands
- Furniture Brands
- Industrial — Ducting, etc.
- Textile Retailers → Fashion, Kids, Medical, Furniture
- Events, Decoration, Film Sets

**B2C Customers (Marketplaces):**
- Allegro ✓, Eobuwie ✓, Etsy ✓, Emag ✗, Amazon ✓, Cdiscount ✗

**B2C Segments:**
- Small Clothing Brands
- Hobby Sewing & Hobby Brands (ex: Scrouchied)
- Sewing Schools → Sewing Community
- Events, Decor, Film Sets
- Textile Retailers → Buying many 10m packing roles

**Extra (strategy work needed):**
- Strategy planning
- Event campaigns
- Competition analysis
- Potential customer identification
- Product knowledge database

---

## 11. KNOWN WEBSITE PROBLEMS (from live audit of textilworld.pl)

### Critical — Losing Sales Now
1. **Aksamit Cuba Welur Bordo** — showing as "Pre-Order" even though it's a bestseller. Customers cannot buy. Fix: in Shopify product settings → "Continue selling when out of stock" OR restock.

2. **Wiskoza Gładka Żółta** — no "Add to Cart" button. Stock = 0 in Shopify but may be in warehouse. Fix: physically check warehouse, update Shopify inventory.

3. **Aksamit Cuba Welur Niebieski** — same issue, no buy button. Fix: check inventory.

4. **Currency locked to PLN** — All 17 country stores only show Polish Złoty. German customers see PLN, Czech customers see PLN, etc. Major conversion killer. Fix: Shopify Markets → enable local currencies (EUR for DE/AT/FR, GBP for UK, NOK for Norway, CZK for Czech, HUF for Hungary).

### High Priority
5. **No translations** — Product descriptions only in Polish. German, Czech, Romanian markets get Polish text. Fix: Install Shopify "Translate & Adapt" app (free from Shopify App Store).

### Medium Priority
6. **Footer garbled text** — UTF-8 encoding error causes Polish characters to show as symbols in footer. Fix: open `footer.liquid` in Shopify theme editor, ensure file is saved as UTF-8.

7. **No meta descriptions** — Every product page has empty meta description field. Hurts Google ranking significantly. Fix: Shopify Admin → Products → use bulk editor to add meta descriptions, or use SEO app.

---

## 12. FURKAN'S OPERATIONAL NOTES (from handwritten documents)

### Package Sizes (exact from notes)
- **Folio sizes:** A, B, C — roughly +/- weights (0.5–10kg)
- **Roll ①:** 20×20×150 (10–15kg) — Muslin, Satyna, Silk
- **Roll ②a:** 30×30×150 (15–25kg) — Barbie, Skom, Muslin 150cm
- **Roll ②b:** 30×30×180 (25–31kg) — Single Jersey, Dresówka, Boucle, Zamsz, Tencel, Pikówka
- **Roll ②c:** 30×30×150 (25–31kg) — Boucle, Zamsz, Tencel, Pikówka (variant)
- **Roll ②d:** 30×30×180 (25–31kg) — Single Jersey, Dresówka
- **Roll ②e:** 40×40×150 (20–31kg) — Boucle, Sherpa, Big Teddy, Small Teddy — "11" rolls
- **Roll ③:** 50×50×150 (20–31kg) — "14" rolls
- **Roll ④:** 60×60×150 (20–31kg) — "14" rolls

### Evidence / Tracking Notes
- Evidence docs: Platform → BaseLinker → tracking
- Upload invoice / mistakes / compare etc.

---

## 13. RESEARCH FINDINGS — 10 STRATEGIC IMPROVEMENTS

### 1. Multi-Currency with Shopify Markets
- **Problem:** All 17 country stores show PLN. German/Czech/UK customers leave immediately.
- **Solution:** Shopify Markets + currency conversion
- **Currencies needed:** EUR (DE/AT/NL/FR/BE), GBP (UK), NOK (Norway), CZK (Czech), HUF (Hungary), SEK (Sweden), DKK (Denmark), RON (Romania)
- **Impact:** 15–30% conversion rate improvement for non-Polish customers
- **Cost:** Free (built into Shopify)
- **Fix in:** Shopify Admin → Markets → enable currencies per market

### 2. Allegro Optimization
- **Problem:** Some listings paused, out-of-stock still showing, pricing not optimized
- **Current performance:** ~247 orders/30d across 6 monitored SKUs
- **Opportunities:**
  - Reactivate paused Bawełna Biała listing
  - Pause out-of-stock Aksamit Bordo (or add "notify when available")
  - Price match Szyfon Biały (undercut competitor by 0.50 PLN)
  - Add product photos to all listings (Bawełna Biała missing photos)
- **Allegro-specific:** Use Allegro's internal courier deal — cheaper than direct courier contracts

### 3. Amazon.de Expansion
- **Current status:** Active on Amazon (some listings)
- **Opportunity:** Germany is largest EU fabric market
- **Key fabrics for Germany:** Baumwollstoff (cotton), Viskose, Jersey, Chiffon
- **Listing optimization:** German product titles + descriptions, European sizing references

### 4. Etsy Growth
- **Current status:** Active
- **Best-performing:** Chiffon, satin (bridal/event market)
- **Strategy:** Add "handmade-friendly" tags, bundle listings (3 colors), seasonal promotions

### 5. B2B Portal
- **Current problem:** B2B ordering done manually via phone/email — too slow
- **Solution:** Dedicated B2B section on Shopify with wholesale prices visible after login
- **Target customers:** Fashion schools, medical/scrubs manufacturers, furniture brands
- **NET30 payment terms** for verified B2B accounts
- **Potential monthly revenue:** 45,000+ PLN (based on current B2B data)

### 6. FOCO (Central/Eastern Europe) Expansion
- **Emag** (Romania) — not yet active — Romania has growing fabric market
- **Cdiscount** (France) — not yet active — French market values quality fabric
- **Next step:** List top 10 SKUs on Emag first

### 7. TikTok / YouTube Content
- **Opportunity:** Fabric unboxing videos perform well on TikTok
- **Content ideas:** Fabric texture close-ups, DIY project tutorials, "fabric of the week"
- **Target audience:** Hobby sewers, small fashion brands
- **Link to segments:** Drives Hobby Sewing & Small Clothing Brand segments

### 8. Sample Kit / Fabric Sample Box
- **Concept:** Sell 5-fabric sample box (5 × 0.5m) for ~39.90 PLN
- **Value:** Low-risk entry for new customers → converts to repeat full orders
- **Bundle:** "Jersey Starter Pack", "Velvet Collection", "Bridal Fabrics Kit"
- **Channel:** Etsy + Shopify (not Allegro — too price-competitive)

### 9. Inventory & Warehouse Management
- **Current problem:** No labeling system, can't find fabric quickly when order arrives
- **Solution (built):** Warehouse Label system in this app
- **Label format:** `TW-[ROW][SHELF]-[POSITION]` (e.g. TW-A1-001)
- **ABC analysis:** Run monthly — top 20% SKUs get priority reorder attention
- **Lead time:** 50–70 days from China — must plan ahead
- **Chinese New Year 2027:** Order by November 20, 2026

### 10. Loyalty Program & Email Marketing
- **Current:** No loyalty program, no email list management
- **Tools:** Klaviyo (email automation for Shopify), or built-in Shopify Email
- **Segments to target:**
  - Post-purchase: "You bought Wiskoza Czarna — here's what goes with it"
  - Win-back: customers who haven't ordered in 60+ days
  - VIP: customers who spent 500+ PLN — give 10% discount code
- **Discount code system:** After customer service calls (delays/complaints) — give unique code

---

## 14. NEXT FEATURES TO BUILD (Product Roadmap)

### Phase 2 — Medium Priority
- [ ] **Photo upload** for package evidence (needs backend — Node.js + Cloudinary/S3)
- [ ] **PDF export** of evidence summary for courier dispute submissions
- [ ] **Email alerts** when dispute overcharge > threshold (e.g. >20 PLN)
- [ ] **Reorder alert emails** when stock hits reorder point
- [ ] **Bulk import** orders from BaseLinker CSV export

### Phase 3 — API Integrations
- [ ] **BaseLinker API** — live order sync (replaces manual data entry)
  - API docs: app.baselinker.com/api → method `getOrders`
  - Key fields: order_id, products, courier_code, weight, declared_dims
- [ ] **Allegro REST API** — live listing status + price comparison
  - Auth: OAuth2 (allegro.pl/auth/oauth/authorize)
  - Endpoint: GET /sale/offers for listing data
- [ ] **Shopify Admin API** — live inventory sync
  - Webhook: inventory_levels/update → triggers LOW_STOCK alert
- [ ] **DPD/GLS/InPost APIs** — pull invoice data automatically for dispute detection

### Phase 4 — Advanced Features
- [ ] **AI weight verification** — compare photo of package on scale vs courier invoice
- [ ] **Multi-user roles** — CEO view (analytics), intern view (operations), warehouse view (labels)
- [ ] **Mobile app** — React Native version for warehouse scanning
- [ ] **Barcode scanning** — scan label barcode to find location instantly
- [ ] **Demand forecasting** — predict reorder needs based on sales trends

---

## 15. DEVELOPMENT NOTES

### Adding a New Tab
1. Add to `TABS` array: `{ id: "newtab", label: "Label", icon: "🔧" }`
2. Create component: `function NewTab({ data, setData }) { ... }`
3. Add data constant: `const INITIAL_NEWTAB = [...]`
4. Add localStorage hook in `App()`: `const [newtab, setNewtab] = useLocalStorage("tw_newtab", INITIAL_NEWTAB)`
5. Add case in `renderTab()` switch: `case "newtab": return <NewTab data={newtab} setData={setNewtab} />`

### Modifying Courier Colors
All in `<style>` block:
```css
.badge-dpd    { background: #DC2626; color: white; }   /* red */
.badge-inpost { background: #FBBF24; color: #1a1a1a; } /* yellow */
.badge-gls    { background: #2563EB; color: white; }   /* blue */
.badge-dhl    { background: #FCD34D; color: #1a1a1a; } /* yellow-dark */
.badge-ups    { background: #92400E; color: white; }   /* brown */
.badge-orlen  { background: #16A34A; color: white; }   /* green */
```

### Adding a New Courier
In `CourierBadge`:
```javascript
const map = { ..., "ORLEN Paczka": "badge-orlen", "NEW_COURIER": "badge-new" };
```
Add CSS: `.badge-new { background: #COLOR; color: white; }`

### localStorage Reset (for testing)
Open browser console and run:
```javascript
Object.keys(localStorage).filter(k => k.startsWith('tw_')).forEach(k => localStorage.removeItem(k));
location.reload();
```

### Adding Sample Orders
Add to `INITIAL_ORDERS` array. Key fields:
```javascript
{
  id: "BL-10422",                    // BaseLinker order ID
  date: "2026-06-06",
  customer: "Customer Name",
  channel: "Shopify",               // Shopify | Allegro | Amazon | Etsy | B2B Direct
  products: "Product description",
  courier: "DPD",                   // DPD | InPost | GLS | DHL | UPS | ORLEN Paczka
  tracking: "TRACKING_NUMBER",
  status: "Wysłane",               // Wysłane | W transporcie | Dostarczono
  weight_declared: 1.5,            // kg (what you told the courier)
  dims_declared: "30x20x5",        // cm LxWxH
  weight_billed: 2.1,              // kg (what the courier charged)
  dims_billed: "30x20x5",
  dispute_status: "Open",          // Open | Submitted | Won | Lost | null
  dispute_amount: 4.80,            // PLN overcharge
  photos: [],                      // future: base64 images
  notes: "Reason for dispute"
}
```

---

## 16. PEOPLE & CONTACTS

| Person | Role | Notes |
|---|---|---|
| Praveer Lakhani | CEO | Requested this CRM prototype. Makes final decisions. Based in Warsaw. |
| Furkan | Operations Manager | On family leave June 2026. Wrote the package size notes. Manages couriers. GLS blocked on his instruction. |
| Abhinay Pratap Singh | Intern (Erasmus+) | Built this app. E-commerce strategy + operations. |
| Polish Lady (name unknown) | Customer service | On maternity leave — key gap in the team |

---

## 17. IMPORTANT DATES

| Date | Event |
|---|---|
| June 2026 | Current period — Furkan on leave, ads paused ("holiday mode") |
| 4 June 2026 | BL-10419 Allegro order was supposed to be delivered — not delivered |
| 20 Nov 2026 | DEADLINE to order from China (before Chinese New Year 2027) |
| 29 Jan 2027 | Chinese New Year 2027 — factories close 4–6 weeks |

---

## 18. GLOSSARY

| Term | Meaning |
|---|---|
| BaseLinker | Multi-channel order management software. Syncs Shopify+Allegro+Amazon+Etsy. Used for fulfillment. |
| Allegro | Poland's largest e-commerce marketplace (~80% of Polish online shopping) |
| Shopify Markets | Shopify feature to manage multiple country stores with local currencies/languages |
| Folio | Flat poly bag for small/folded fabric orders |
| Rola / Roll | Cardboard tube roll for large fabric shipments |
| Wiskoza | Viscose / rayon fabric |
| Aksamit/Welur | Velvet/velour fabric |
| Szyfon | Chiffon fabric |
| Krepa | Crepe fabric |
| Dzianina Jersey | Jersey knit fabric |
| Bawełna | Cotton fabric |
| Dresówka | French terry / sweatshirt fabric |
| Pikówka | Quilted fabric |
| Boucle | Loopy textured fabric (used in furniture) |
| Zamsz | Faux suede fabric |
| Tencel | Sustainable lyocell fabric (premium) |
| OEKO-TEX | Textile safety certification (required by MedScrubs) |
| OSS VAT | EU One-Stop-Shop VAT system — threshold €10,000/PLN 42,000 |
| Spór kurierski | Courier dispute (weight/dimension overcharge) |
| Nadpłata | Overcharge |
| Zadeklarowana waga | Declared weight |
| Naliczona waga | Billed/charged weight |
| Wysłane | Sent/dispatched |
| Dostarczono | Delivered |
| W transporcie | In transit |
| Rząd | Row (warehouse) |
| Półka | Shelf (warehouse) |
| Etykieta | Label |
