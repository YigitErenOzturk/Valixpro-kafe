# ValixPro V2 Review

## Applied
- Validated required environment variables; replaced real `.env` with `.env.example`.
- Added safe centralized localStorage session parsing.
- Removed global navigation state from `window`.
- Added route-level lazy loading.
- Improved mobile containment, touch targets, focus states, and reduced-motion support.
- Hardened VIN decoding with validation, normalization, timeout, and caching.
- Added an RLS starter note for Supabase tenant isolation.

## Critical backend checks
- Enable and test RLS for every business table and storage bucket. Browser filters are not security.
- Enforce delete/update permissions in RLS or Edge Functions.
- Edge Functions must verify JWT, derive shop and role server-side, validate input, and rate-limit.
- Keep customer/vehicle documents in private buckets with signed URLs.
- Add foreign keys, not-null/unique/check constraints and tenant-aware indexes.

## Performance
- Replace remaining `select('*')` queries with explicit columns.
- Add server pagination/search for large lists.
- Aggregate dashboard/report data in SQL views or RPCs.
- Consolidate remote fonts.

## Feature notes
- AI fault triage should run in an Edge Function and return uncertainty, not a definitive diagnosis.
- OBD support is best offered as optional Web Bluetooth/Web Serial progressive enhancement.
- VIN decoding exists and is now hardened; EU history/equipment requires a licensed provider.
