# Odoo 20 × Direct Store Delivery (DSD) — Learning Curriculum

Version pinned to **Odoo 20.0** documentation. All URLs are official Odoo docs.
Built for use with LearnHarness (Teach mode) — see "How to use this file" below.

---

## Note for the teaching agent

- This file is the **syllabus**: it defines the learning goal, the learner's background, the
  dependency-ordered topic sequence, and a check question per topic.
- Each topic names its **canonical source URL(s)**. The learner reads them in a browser.
  Do not invent doc content beyond this file — teach from the objectives and facts given here,
  and send the learner to the URL for the primary text.
- The learner comes from an **SAP background** (SD / MM / FI / MD vocabulary). The glossary
  below maps their terms to Odoo terms — use it to anchor explanations.
- Two **pending company decisions** affect some topics: edition (Community vs Enterprise)
  and hosting (Odoo Online vs Odoo.sh vs on-premise). Where it matters, the topic says so.

## How to use this file (for the human)

1. **Best path — Teach session:** start a Teach-mode session in LearnHarness, attach this
   file (paperclip / drag-and-drop), and paste the "Learning goal" paragraph below as your
   opening message. Add: *"Use the attached file as my syllabus; probe my starting point,
   then teach topic by topic in order."*
2. **Optional — knowledge graph:** use "Extract files…" to also import this file so its
   facts land in your persistent graph with provenance. (UTF-8 plain Markdown — supported.)
3. Do the phases **in order** — later phases depend on earlier ones. Open each topic's URL,
   read it, then answer the topic's check question in the session.

## Learning goal (paste into the session)

> I want to go from SAP-style ERP knowledge to fluently designing and running a **Direct
> Store Delivery (DSD)** operation on **Odoo 20**: master data, order-to-cash (presales),
> van sales via Point of Sale, inventory with lots/expiry and van stock, and the finance
> settlement behind cash-on-delivery — plus understanding which DSD pieces Odoo does NOT
> cover natively, because our company will build those as a custom application.

## Learner context

- Background: SAP ERP vocabulary (SD, MM, FI, master data); intermediate IT, new to Odoo.
- Company: adopting Odoo 20 as ERP; planning a DSD application (hybrid presales + van sales
  for consumer-goods distribution to retail stores), later built with AI assistance.
- End target: be able to (a) configure Odoo for a DSD pilot, (b) specify the custom DSD app
  (routes, proof of delivery, empties, driver settlement), (c) train analysts and users.

---

## Phase 0 — Orientation facts (before any topic)

These facts constrain everything below; the agent should verify them in probe questions.

- Odoo docs are split into four books: User (applications), Administration (hosting),
  Developer, Contributing. Index: https://www.odoo.com/documentation/20.0/applications.html
- Odoo has two editions: **Community** (open source) and **Enterprise** (paid). The user
  docs mostly describe **Enterprise** behavior; POS advanced features and full Accounting
  are Enterprise. Decide edition before configuring anything real.
- Hosting options: Odoo Online (no custom modules — Studio only), Odoo.sh or on-premise
  (full custom development). A custom DSD app **requires Odoo.sh or on-premise**.
  Hosting docs: https://www.odoo.com/documentation/20.0/administration.html
- **The central DSD design decision — two patterns:**
  1. **Presales**: rep takes a sales order on the road; warehouse delivers later.
     Standard order-to-cash; needs connectivity at some point.
  2. **Van sales**: truck carries stock; selling happens in Point of Sale **offline mode**
     with cash control; stock and cash reconcile at end of day.
     Only POS has robust offline; the Inventory app does not.
- Odoo has **no native route optimization / visit sequencing** — that is custom-app or
  third-party territory (a core reason our DSD app exists).
- Fiscal localization (country tax pack) should be chosen **before** creating real master
  data: https://www.odoo.com/documentation/20.0/applications/finance/fiscal_localizations.html

---

## Phase 1 — Master data (MD)

### 1.1 Import & export basics
- Why: you will migrate customer and product masters into Odoo.
- Learn to: export any list view to XLSX/CSV; import with the CSV importer and its
  relational-field handling; know when to import vs create manually.
- Source: https://www.odoo.com/documentation/20.0/applications/essentials/export_import_data.html
- Check: "Given a spreadsheet of 500 stores, what must you map before a clean Contacts
  import, and how do you handle a missing external ID?"

### 1.2 Contacts (customers = stores)
- Why: every store, chain parent, and delivery address is a Contact in DSD.
- Learn to: create individual vs company contacts; parent/child structure for chains
  (billing parent, delivery child addresses); merge duplicates; set a price list and
  payment terms on a customer.
- Source: https://www.odoo.com/documentation/20.0/applications/essentials/contacts.html
  (merging: https://www.odoo.com/documentation/20.0/applications/essentials/contacts/merge.html )
- Check: "Model 'Carrefour HQ' with 12 stores that get delivered separately but invoiced
  centrally — which contact fields carry that?"

### 1.3 Products, variants, UoM, packaging
- Why: DSD sells the same goods in units, cases, pallets; expiry-critical SKUs.
- Learn to: create products (goods vs service); enable and use **variants** (flavor/size);
  configure **units of measure** and UoM conversion (unit ↔ case); set **packaging**
  (buy case, sell unit); set sales/commodity fields needed later (tracking, expiry).
- Sources:
  - https://www.odoo.com/documentation/20.0/applications/essentials/product_catalog.html
  - https://www.odoo.com/documentation/20.0/applications/sales/sales/products_prices/products/variants.html
  - https://www.odoo.com/documentation/20.0/applications/inventory_and_mrp/inventory/product_management/configure/uom.html
  - https://www.odoo.com/documentation/20.0/applications/inventory_and_mrp/inventory/product_management/configure/packaging.html
- Check: "A crate = 24 bottles. Customer orders 30 bottles. How do UoM and packaging
  make picking and invoicing show the right quantities?"

### 1.4 Price lists and discounts
- Why: DSD lives on per-store/per-segment pricing, deal discounts, and promotions.
- Learn to: create price lists (per customer group), price rules (fixed/discount/formula),
  optional pricelist selection per customer; discount policies.
- Sources:
  - https://www.odoo.com/documentation/20.0/applications/sales/sales/products_prices/prices/pricing.html
  - https://www.odoo.com/documentation/20.0/applications/sales/sales/products_prices/prices/discounts.html
- Check: "Key accounts get −8%, independents get list price. Design the minimal pricelist
  setup and say where each customer gets tagged."

---

## Phase 2 — Sales flow / order-to-cash (SD)

### 2.1 Quotations and sales orders
- Why: the presales backbone — order today, deliver and invoice later.
- Learn to: quotation → confirmed sales order lifecycle; order lines, margins, deadlines;
  delivery addresses; what a confirmed order triggers in Inventory.
- Source: https://www.odoo.com/documentation/20.0/applications/sales/sales/sales_quotations.html
- Check: "Walk the document flow: what exact objects exist after confirming a sales order
  for a delivery customer, and what status changes?"

### 2.2 Invoicing policy
- Why: DSD often invoices on delivered quantities, not ordered ones.
- Learn to: choose invoice on ordered vs delivered quantities; down payments; how the
  invoice relates to the delivery (stock moves) — the FI hinge.
- Source: https://www.odoo.com/documentation/20.0/applications/sales/sales/invoicing/invoicing_policy.html
- Check: "Driver delivers 28 of 30 ordered bottles (2 damaged). Which invoicing policy
  invoices 28 without manual edits, and why?"

### 2.3 Promotions, loyalty, eWallet/gift cards
- Why: trade promotions and consumer promos are daily DSD currency.
- Learn to: loyalty programs (points, coupons, immediate discounts); when a promotion
  belongs in a pricelist vs the loyalty engine.
- Source: https://www.odoo.com/documentation/20.0/applications/sales/sales/products_prices/loyalty_discount.html
- Check: "'Buy 10 get 2 free' for a store — pricelist rule or loyalty program? Argue both
  and pick one."

### 2.4 Returns and refunds
- Why: unsold/expired goods and empties flow back through the truck.
- Learn to: create returns from a sales order/POS; refund options (credit note vs cash);
  restock or scrap decision.
- Source: https://www.odoo.com/documentation/20.0/applications/sales/sales/products_prices/returns.html
- Check: "Driver brings back 5 expired crates. Which document chain takes them into stock
  AND issues a credit note?"

---

## Phase 3 — Van sales: Point of Sale

### 3.1 POS setup and configuration
- Why: the van is a shop on wheels — POS is the selling surface.
- Learn to: create a POS per van; assign warehouse/stock location (van location!);
  payment methods (cash, card); customers in POS; receipts.
- Source: https://www.odoo.com/documentation/20.0/applications/sales/point_of_sale.html
- Check: "What binds a POS to a van's stock location, and what happens to inventory when
  a POS order is paid?"

### 3.2 POS workflow, sessions, and cash control
- Why: one session per van per day; cash control = the COD discipline.
- Learn to: open/close sessions; cash in/out; opening/closing balances and discrepancy
  handling; what journal entries a closed session produces.
- Source: https://www.odoo.com/documentation/20.0/applications/sales/point_of_sale/use.html
- Check: "Driver's drawer is 40 short at close. Walk the closing flow and the accounting
  impact of the difference."

### 3.3 Offline mode — capabilities and limits
- Why: trucks lose connectivity; know exactly what survives.
- Learn to: what POS offline mode syncs when reconnected; what breaks (payment terminals?
  concurrent stock?); general Odoo offline mode is only for short interruptions.
- Source: https://www.odoo.com/documentation/20.0/applications/general/offline_mode.html
- Check: "Van sells offline for 3 hours. What is guaranteed synced later, and what two
  risks would you design around in the custom app?"

### 3.4 Odoo mobile app (context)
- Why: reps/drivers use phones — know which apps work on mobile.
- Source: https://www.odoo.com/documentation/20.0/administration/mobile.html
- Check: "Which of our DSD surfaces (presales order, van sale, stock count, delivery
  proof) does the standard mobile app cover today?"

---

## Phase 4 — Inventory & logistics (MM/WM)

### 4.1 Warehouses and locations — modeling the van
- Why: THE structural decision: van as its own warehouse vs a location in the main WH.
- Learn to: create warehouses and locations; internal transfers between them; operation
  types; think in stock moves (Odoo's "material documents").
- Sources:
  - https://www.odoo.com/documentation/20.0/applications/inventory_and_mrp/inventory/warehouses_storage/inventory_management/warehouses.html
  - https://www.odoo.com/documentation/20.0/applications/inventory_and_mrp/inventory/warehouses_storage/inventory_management/use_locations.html
- Check: "Compare 'VAN1 as warehouse' vs 'WH/VAN1 as location' for 8 trucks and one DC.
  Name one thing each design makes easy."

### 4.2 Delivery flows (1/2/3-step) and routes
- Why: store deliveries and WH→van replenishment are just configured routes.
- Learn to: pick/delivery steps per warehouse; routes and pull/push rules basics.
- Sources:
  - https://www.odoo.com/documentation/20.0/applications/inventory_and_mrp/inventory/shipping_receiving/daily_operations/receipts_delivery_one_step.html
  - https://www.odoo.com/documentation/20.0/applications/inventory_and_mrp/inventory/shipping_receiving/daily_operations/receipts_delivery_two_steps.html
  - https://www.odoo.com/documentation/20.0/applications/inventory_and_mrp/inventory/shipping_receiving/daily_operations/delivery_three_steps.html
  - https://www.odoo.com/documentation/20.0/applications/inventory_and_mrp/inventory/shipping_receiving/daily_operations/use_routes.html
- Check: "Morning van loading = which operation type, and why would 2-step delivery NOT
  fit a store drop?"

### 4.3 Stock counting and cycle counts
- Why: end-of-day van reconciliation and shrinkage control.
- Learn to: inventory adjustments; counting lists; cycle counts by ABC.
- Sources:
  - https://www.odoo.com/documentation/20.0/applications/inventory_and_mrp/inventory/warehouses_storage/inventory_management/count_products.html
  - https://www.odoo.com/documentation/20.0/applications/inventory_and_mrp/inventory/warehouses_storage/inventory_management/cycle_counts.html
- Check: "Van session closed with POS saying 40 units sold but the truck counts 3 missing.
  Which document fixes stock, and does it touch accounting?"

### 4.4 Lots, serials, expiration
- Why: FMCG freshness — FEFO picking, expiry claims, traceability.
- Learn to: enable tracking per product; create/assign lots; removal strategy (FEFO);
  expiration dates and alerts.
- Sources:
  - https://www.odoo.com/documentation/20.0/applications/inventory_and_mrp/inventory/product_management/product_tracking/lots.html
  - https://www.odoo.com/documentation/20.0/applications/inventory_and_mrp/inventory/product_management/product_tracking/serial_numbers.html
  - https://www.odoo.com/documentation/20.0/applications/inventory_and_mrp/inventory/product_tracking/expiration_dates.html
- Check: "Two lots in the van: expiring tomorrow and next month. What makes tomorrow's
  stops ship the old lot, and where is that configured?"

### 4.5 Barcode app
- Why: load the truck, pick by scan, count by scan — error-proofing on the road.
- Learn to: barcode interface for picking, transfers, counts; what it needs on mobile.
- Source: https://www.odoo.com/documentation/20.0/applications/inventory_and_mrp/barcode.html
- Check: "Which two DSD daily tasks would you barcode first for the highest error-cost
  reduction, and why?"

### 4.6 Purchase (optional) and Manufacturing (optional)
- Why: only if you buy from producers or produce (dairy/bakery) in scope.
- Sources:
  - https://www.odoo.com/documentation/20.0/applications/inventory_and_mrp/purchase.html
  - https://www.odoo.com/documentation/20.0/applications/inventory_and_mrp/manufacturing.html
- Check: "Reorder a SKU when the DC falls below a week of demand: which app owns the
  trigger?"

---

## Phase 5 — Financials (FI)

### 5.1 Customer invoices and payment terms
- Why: the AR side of DSD — and invoice *timing* rules.
- Learn to: create/register invoices from SO or POS; payment terms (instant, end of
  month); credit notes.
- Sources:
  - https://www.odoo.com/documentation/20.0/applications/finance/accounting/customer_invoices.html
  - https://www.odoo.com/documentation/20.0/applications/finance/accounting/customer_invoices/payment_terms.html
- Check: "Chain customer pays in 30 days, corner stores pay the driver cash. How do the
  two flows differ in documents from sale to cleared payment?"

### 5.2 Journals and payments
- Why: a cash journal per van makes COD auditable.
- Learn to: journal types (sale, cash, bank); register payments; outstanding payments.
- Sources:
  - https://www.odoo.com/documentation/20.0/applications/finance/accounting/payments.html
  - https://www.odoo.com/documentation/20.0/applications/finance/accounting/get_started/journals.html
- Check: "Design the journal layout for 8 vans: which journals, what names, and where
  POS session closings land."

### 5.3 Bank and cash reconciliation
- Why: end-of-day cash and the bank statement must match the road.
- Learn to: reconcile bank lines with payments; match POS session cash; handle
  discrepancies.
- Source: https://www.odoo.com/documentation/20.0/applications/finance/accounting/bank/reconciliation.html
- Check: "Driver declares 1,200; envelope contains 1,150. From session close to write-off,
  walk the entries."

### 5.4 Taxes and fiscal localization
- Why: do this FIRST in any real database — taxes ride on everything.
- Learn to: your country's fiscal pack; tax grids on products and invoices.
- Sources:
  - https://www.odoo.com/documentation/20.0/applications/finance/accounting/taxes.html
  - https://www.odoo.com/documentation/20.0/applications/finance/fiscal_localizations.html
- Check: "Why is installing the localization pack a step-zero, not a month-two task?"

---

## Phase 6 — Support apps

### 6.1 Fleet (trucks)
- Source: https://www.odoo.com/documentation/20.0/applications/hr/fleet.html
- Check: "Which van facts live in Fleet vs Inventory vs POS, and why keeping them
  separate is correct?"

### 6.2 Planning (driver shifts)
- Why: shift/visit scheduling — but NOT route optimization (gap!).
- Source: https://www.odoo.com/documentation/20.0/applications/services/planning.html
- Check: "Planning can schedule a driver's day of visits; what exactly can it NOT decide
  for DSD?"

---

## Phase 7 — The DSD gaps → your custom application

### 7.1 Build vs buy first
- Check apps.odoo.com for "van sales" / "direct store delivery" modules (e.g. VanGO and
  similar) and the community discussion:
  https://www.odoo.com/forum/help-1/is-there-full-functionality-mobile-app-solution-for-van-sales-field-sales-integrated-with-odoo-278151
- Check: "List the four DSD gaps below; for each, would you buy or build? Defend one."

### 7.2 Studio (no-code changes)
- Why: first stop for small tweaks (fields, views, automation rules).
- Source: Studio section of https://www.odoo.com/documentation/20.0/applications.html
- Check: "Add a 'shelf placement photo' field to store visits: Studio or module? When
  does Studio stop being enough?"

### 7.3 Developer path (the real app)
- Learn to: read the dev docs map; follow Server Framework 101 (build a first module);
  use the ORM reference; respect coding guidelines; expose data to AI via the MCP server.
- Sources:
  - https://www.odoo.com/documentation/20.0/developer.html
  - https://www.odoo.com/documentation/20.0/developer/tutorials.html
  - https://www.odoo.com/documentation/20.0/developer/reference/backend/orm.html
  - https://www.odoo.com/documentation/20.0/contributing.html
  - https://www.odoo.com/documentation/20.0/applications/productivity/ai/mcp_server.html
- Check: "Your DSD app needs a `dsd.visit` model linking store, van, driver, and route.
  Which ORM pieces will it build on?"

### 7.4 Capstone — the DSD blueprint (Practice mode)
- Produce a written design answering, using only what you learned:
  1. Van modeling: warehouse vs location — your pick and why.
  2. Document chain for a full day: load → 10 store visits (7 POS sales, 2 returns,
     1 expired swap) → cash settle → stock reconcile.
  3. Journal and session design per van.
  4. Which parts standard Odoo covers; which four your app must build
     (visit sequencing/routes, proof of delivery, empty-container tracking,
     driver settlement).
- Practice prompt for LearnHarness: "Drill me on the DSD daily cycle: give me scenarios
  (returns, shortages, offline gaps, price disputes) and grade my document choices."

---

## Glossary — SAP speak → Odoo speak

| SAP term | Odoo equivalent |
|---|---|
| SD (Sales & Distribution) | Sales app (+ Point of Sale for cash sales) |
| MM / WM | Inventory app (+ Purchase) |
| FI / FI-AR | Accounting app |
| MD master data | Contacts, Product catalog, Pricelists |
| Customer master / BP | Contact |
| Material master | Product |
| Pricing procedure | Pricelist (+ loyalty/promotions) |
| Sales order | Sales order |
| Outbound delivery / delivery note | Delivery order (picking) |
| Goods receipt | Receipt |
| Stock transfer | Internal transfer / operation type |
| Movement type | Operation type / stock move |
| Batch | Lot |
| Physical inventory doc | Inventory adjustment / count |
| Storage location | Location |
| Invoice document | Customer invoice (journal entry) |
| Incoming/outgoing payment | Payment + reconciliation |
| Cash journal | Journal (type: cash) |
| MRP / PP | Manufacturing |

---

## Source index (flat list)

- Docs root (all books): https://www.odoo.com/documentation/20.0/
- Applications (user): https://www.odoo.com/documentation/20.0/applications.html
- Administration (hosting): https://www.odoo.com/documentation/20.0/administration.html
- Developer: https://www.odoo.com/documentation/20.0/developer.html
- Contributing (coding guidelines): https://www.odoo.com/documentation/20.0/contributing.html
- Import/export: https://www.odoo.com/documentation/20.0/applications/essentials/export_import_data.html
- Contacts: https://www.odoo.com/documentation/20.0/applications/essentials/contacts.html
- Merge contacts: https://www.odoo.com/documentation/20.0/applications/essentials/contacts/merge.html
- Product catalog: https://www.odoo.com/documentation/20.0/applications/essentials/product_catalog.html
- Variants: https://www.odoo.com/documentation/20.0/applications/sales/sales/products_prices/products/variants.html
- UoM: https://www.odoo.com/documentation/20.0/applications/inventory_and_mrp/inventory/product_management/configure/uom.html
- Packaging: https://www.odoo.com/documentation/20.0/applications/inventory_and_mrp/inventory/product_management/configure/packaging.html
- Pricelists: https://www.odoo.com/documentation/20.0/applications/sales/sales/products_prices/prices/pricing.html
- Discounts: https://www.odoo.com/documentation/20.0/applications/sales/sales/products_prices/prices/discounts.html
- Quotations/orders: https://www.odoo.com/documentation/20.0/applications/sales/sales/sales_quotations.html
- Invoicing policy: https://www.odoo.com/documentation/20.0/applications/sales/sales/invoicing/invoicing_policy.html
- Loyalty/promotions: https://www.odoo.com/documentation/20.0/applications/sales/sales/products_prices/loyalty_discount.html
- Returns/refunds: https://www.odoo.com/documentation/20.0/applications/sales/sales/products_prices/returns.html
- Point of Sale: https://www.odoo.com/documentation/20.0/applications/sales/point_of_sale.html
- POS workflow/sessions: https://www.odoo.com/documentation/20.0/applications/sales/point_of_sale/use.html
- Offline mode: https://www.odoo.com/documentation/20.0/applications/general/offline_mode.html
- Mobile app: https://www.odoo.com/documentation/20.0/administration/mobile.html
- Warehouses: https://www.odoo.com/documentation/20.0/applications/inventory_and_mrp/inventory/warehouses_storage/inventory_management/warehouses.html
- Locations: https://www.odoo.com/documentation/20.0/applications/inventory_and_mrp/inventory/warehouses_storage/inventory_management/use_locations.html
- 1-step delivery: https://www.odoo.com/documentation/20.0/applications/inventory_and_mrp/inventory/shipping_receiving/daily_operations/receipts_delivery_one_step.html
- 2-step: https://www.odoo.com/documentation/20.0/applications/inventory_and_mrp/inventory/shipping_receiving/daily_operations/receipts_delivery_two_steps.html
- 3-step delivery: https://www.odoo.com/documentation/20.0/applications/inventory_and_mrp/inventory/shipping_receiving/daily_operations/delivery_three_steps.html
- Routes: https://www.odoo.com/documentation/20.0/applications/inventory_and_mrp/inventory/shipping_receiving/daily_operations/use_routes.html
- Count products: https://www.odoo.com/documentation/20.0/applications/inventory_and_mrp/inventory/warehouses_storage/inventory_management/count_products.html
- Cycle counts: https://www.odoo.com/documentation/20.0/applications/inventory_and_mrp/inventory/warehouses_storage/inventory_management/cycle_counts.html
- Lots: https://www.odoo.com/documentation/20.0/applications/inventory_and_mrp/inventory/product_management/product_tracking/lots.html
- Serials: https://www.odoo.com/documentation/20.0/applications/inventory_and_mrp/inventory/product_management/product_tracking/serial_numbers.html
- Expiration: https://www.odoo.com/documentation/20.0/applications/inventory_and_mrp/inventory/product_management/product_tracking/expiration_dates.html
- Barcode: https://www.odoo.com/documentation/20.0/applications/inventory_and_mrp/barcode.html
- Purchase: https://www.odoo.com/documentation/20.0/applications/inventory_and_mrp/purchase.html
- Manufacturing: https://www.odoo.com/documentation/20.0/applications/inventory_and_mrp/manufacturing.html
- Customer invoices: https://www.odoo.com/documentation/20.0/applications/finance/accounting/customer_invoices.html
- Payment terms: https://www.odoo.com/documentation/20.0/applications/finance/accounting/customer_invoices/payment_terms.html
- Payments: https://www.odoo.com/documentation/20.0/applications/finance/accounting/payments.html
- Journals: https://www.odoo.com/documentation/20.0/applications/finance/accounting/get_started/journals.html
- Reconciliation: https://www.odoo.com/documentation/20.0/applications/finance/accounting/bank/reconciliation.html
- Taxes: https://www.odoo.com/documentation/20.0/applications/finance/accounting/taxes.html
- Fiscal localizations: https://www.odoo.com/documentation/20.0/applications/finance/fiscal_localizations.html
- Fleet: https://www.odoo.com/documentation/20.0/applications/hr/fleet.html
- Planning: https://www.odoo.com/documentation/20.0/applications/services/planning.html
- Dev tutorials: https://www.odoo.com/documentation/20.0/developer/tutorials.html
- ORM reference: https://www.odoo.com/documentation/20.0/developer/reference/backend/orm.html
- MCP server: https://www.odoo.com/documentation/20.0/applications/productivity/ai/mcp_server.html
- Apps store: https://apps.odoo.com
- Van sales discussion: https://www.odoo.com/forum/help-1/is-there-full-functionality-mobile-app-solution-for-van-sales-field-sales-integrated-with-odoo-278151
