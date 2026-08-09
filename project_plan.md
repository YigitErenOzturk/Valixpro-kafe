# AutoFix Pro - Car Repair Shop Management Panel

## 1. Project Description
An internal management dashboard for car repair shops. Each shop owner registers an account and gets their own isolated management panel. Shop owners and mechanics can log in to manage daily operations — track appointments, maintain customer and vehicle records, create invoices, manage parts inventory, and oversee staff. The goal is to replace paper records and scattered spreadsheets with one clean, unified system. **Multi-tenant: each shop's data is completely separate via RLS.**

## 2. Page Structure
- `/register` - New shop registration (shop name, owner info, email, password)
- `/login` - Staff login page (Supabase Auth)
- `/` - Dashboard overview (KPIs, today's appointments, revenue summary, quick actions)
- `/appointments` - Appointment calendar & list management
- `/customers` - Customer directory with search and detail view
- `/customers/:id` - Individual customer profile with service history
- `/vehicles` - Vehicle service records
- `/invoices` - Invoice list with status tracking
- `/inventory` - Parts inventory management
- `/staff` - Staff & mechanic management
- `/supply` - Parts supply management (suppliers, price comparison, auto order list)
- `/notifications` - Customer notification & email sending
- `/reports` - Financial & performance reports

## 3. Core Features
- [x] Multi-tenant shop registration (each account = different shop, RLS-separated data)
- [x] Staff authentication (login via Supabase Auth, role-based access)
- [x] Dashboard with KPIs (today's appointments, revenue, pending invoices, low stock alerts)
- [x] Appointment booking & calendar management
- [x] Customer records with contact info and service history
- [x] Vehicle service tracking (repair history per vehicle)
- [x] Invoicing & billing with status tracking (pending/paid/overdue)
- [x] Parts & inventory management with low-stock alerts
- [x] Staff / mechanic management with role assignment
- [x] Staff invite system (shop owners can invite staff via email + temporary password, creates real login accounts)
- [x] Parts supply management — supplier tracking, price comparison, auto purchase order generation
- [x] Customer email notifications — send status updates from vehicle detail or notifications page
- [x] AI Fault Prediction — OpenAI GPT-4 powered fault analysis from repair notes, saved to vehicle, displayed as service record card
- [x] AI Supplier Finder — OpenAI GPT-4 powered part sourcing, supplier recommendations and price research
- [x] VIN Lookup — NHTSA vPIC integration for automatic vehicle info population
- [x] Vehicle Photo Management — upload, label, mark problem areas, lightbox viewer
- [x] Maintenance Reminders — periodic maintenance tracking per vehicle
- [x] Bulk VIN Import — Excel/CSV multi-vehicle registration
- [x] Financial Reports — revenue trends, category distribution, technician productivity, profit/loss
- [x] Dark Mode support

## 8. Multi-Tenant Architecture
- Each shop owner registers via `/register` → creates a Supabase Auth user + shop record + staff record (owner role)
- All business tables (customers, vehicles, appointments, invoices, inventory, staff) have `shop_id` column
- RLS policies filter all queries by `shop_id = (SELECT staff.shop_id FROM staff WHERE staff.user_id = auth.uid())`
- Edge function `register-shop` handles atomic shop creation (auth user + shop + staff record via service_role)
- Edge function `invite-staff` handles staff invitations (creates auth user + staff record, only owner/admin/manager can invite)
- Shop name shown in sidebar — each shop sees only their own data

## 4. Data Model Design
*(Implemented with Supabase; RLS policies active for multi-tenant isolation)*

Core tables:
- **staff** - id, name, email, role, phone, active, shop_id, user_id
- **customers** - id, name, email, phone, address, shop_id, created_at
- **vehicles** - id, customer_id, make, model, year, plate_number, vin, color, notes, images, ai_prediction (JSONB), shop_id, created_at
- **appointments** - id, customer_id, vehicle_id, staff_id, date, time, status, service_description, notes, shop_id
- **invoices** - id, customer_id, appointment_id, invoice_number, amount, tax_amount, status, due_date, shop_id, created_at
- **inventory** - id, part_name, part_number, quantity, min_quantity, price, category, supplier, shop_id
- **suppliers** - id, name, contact_name, email, phone, address, notes, shop_id
- **supplier_parts** - id, supplier_id, inventory_id, supplier_price, lead_time_days, is_preferred, status, shop_id
- **purchase_orders** - id, supplier_id, status, total, shop_id, created_at
- **maintenance_reminders** - id, vehicle_id, reminder_type, interval_km, interval_days, last_done_at, shop_id
- **transactions** - id, type (income/expense), amount, category, description, date, shop_id

## 5. Backend / Third-party Integration Plan
- **Supabase**: ✅ Connected — database tables created, demo data seeded, RLS policies active, staff-based login wired to real queries.
- **Stripe**: Not needed for initial version (internal invoicing only).
- **Shopify**: Not applicable.

## 6. Development Phase Plan

### Phase 1: Login + Dashboard Shell
- Goal: Staff can see a login page and the main dashboard layout with sidebar navigation
- Deliverable: Login page UI, dashboard layout with sidebar, top bar, and a stats overview page with KPI cards
- Status: ✅ Complete

### Phase 2: Appointment Management
- Goal: Staff can view, create, and manage repair appointments
- Deliverable: Appointment calendar view, appointment list, create/edit appointment form
- Status: ✅ Complete

### Phase 3: Customer Records
- Goal: Staff can manage customer information and view service history
- Deliverable: Customer list with search, customer detail page with vehicle & service history
- Status: ✅ Complete

### Phase 4: Vehicle Service Tracking
- Goal: Track all vehicles and their service/repair history
- Deliverable: Vehicle list, vehicle detail with full service timeline, VIN lookup, AI fault prediction, photo management
- Status: ✅ Complete

### Phase 5: Invoicing & Billing
- Goal: Create and track invoices for completed services
- Deliverable: Invoice list with status filters, create invoice form, invoice detail view
- Status: ✅ Complete

### Phase 6: Parts Inventory
- Goal: Track parts stock levels with low-stock alerts
- Deliverable: Inventory list, add/edit part form, low-stock warning indicators
- Status: ✅ Complete

### Phase 7: Staff Management
- Goal: Manage mechanic and staff accounts
- Deliverable: Staff list, add/edit staff form, role assignment
- Status: ✅ Complete

### Phase 8: Supabase Integration
- Goal: Connect real authentication and database
- Deliverable: Real login, real data storage replacing mock data across all pages
- Status: ✅ Complete (database + login + dashboard done)

### Phase 9: Supply Chain & AI Features
- Goal: Complete supply management with AI-powered part sourcing, AI fault prediction, maintenance reminders
- Deliverable: Supplier management, purchase order tracking, AI supplier finder, AI fault prediction, VIN lookup, bulk import, reports, notifications
- Status: ✅ Complete