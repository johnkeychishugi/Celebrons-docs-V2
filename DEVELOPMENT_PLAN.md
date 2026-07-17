# Celebrons V2 — Development Plan

## 1. Delivery strategy

Celebrons V2 will be delivered in testable waves. Each wave advances the same parallel tracks and ends with an integrated staging increment that can be reviewed by Celebrons. A wave is accepted only when its backend, frontend, quality, documentation, and operating needs work together; unfinished work does not accumulate until launch.

Task convention: keep unfinished work as `- [ ]` and change it to `- [x]` only after the task satisfies the Definition of Done and its evidence is linked from the task or wave review.

The primary acceptance journey is:

```text
Public inquiry
→ negotiation
→ confirmed event
→ invitations and RSVP
→ floor plan and seating
→ logistics, personnel, and vendors
→ installments and expenses
→ event-day check-in
→ closeout and reports
```

## 2. Team assumptions

Recommended minimum delivery team:

- [ ] 1 product owner from Celebrons;
- [ ] 1 UI/UX designer;
- [ ] 1 frontend engineer;
- [ ] 1 backend engineer;
- [ ] 1 QA engineer shared throughout delivery;
- [ ] part-time DevOps support.

With fewer people, preserve the wave order and reduce parallel work rather than dropping testing, security, or migration activities.

## 3. Definition of ready

A feature is ready for development when it has:

- [ ] an identified user and business outcome;
- [ ] written acceptance criteria;
- [ ] approved interaction design;
- [ ] known role and permission requirements;
- [ ] defined data fields and validation;
- [ ] identified reporting and audit needs;
- [ ] test data or realistic examples.

## 4. Definition of done

A feature is complete when:

- [ ] acceptance criteria pass;
- [ ] permissions are enforced in the UI and API;
- [ ] responsive behavior is verified;
- [ ] unit and integration tests pass;
- [ ] important user paths have end-to-end coverage;
- [ ] empty, loading, validation, and failure states exist;
- [ ] audit logging and analytics are included where required;
- [ ] API and operating documentation are updated;
- [ ] the product owner accepts the feature in staging.

## 5. Delivery waves and tracks

The tracks are continuous delivery lanes, not separate products:

- **Track A — Product, process, and UI/UX:** workflows, French terminology, interaction design, content, accessibility, and stakeholder acceptance.
- **Track B — Backend and data:** NestJS modules, TypeORM entities/migrations, PostgreSQL constraints/transactions, authorization, audit, OpenAPI, and integrations.
- **Track C — Frontend:** the independent Next.js application covering the public website, staff operations, client portal, and responsive event-day tools.
- **Track D — Quality and security:** acceptance criteria, automated tests, permission checks, performance, accessibility, and release evidence.
- **Track E — Platform, content, and adoption:** environments, CI/CD, object storage, monitoring, migration, documentation, training, and rollout.

### Wave 0 — Discovery, scope, and experience design

**Goal:** confirm workflows and create a coherent visual language before feature implementation.

**Track A — Product, process, and UI/UX**

- [ ] interview Celebrons administration, event managers, finance, logistics, and protocol staff;
- [ ] map the current event workflow and identify handoffs, spreadsheets, and paper records;
- [ ] define French product terminology and remove ambiguous terms such as “owner”;
- [ ] validate roles and the permission matrix;
- [ ] inventory and classify V1 content and media;
- [ ] create information architecture for public, staff, and client areas;
- [ ] design the core flows: inquiry, event creation, invitations, floor plan, logistics, payment, and event closeout;
- [ ] approve desktop and mobile prototypes;
- [ ] define success metrics and baseline current performance.

**Track B — Backend and data**

- [ ] define domain boundaries, core entities, state transitions, audit requirements, and critical data invariants;
- [ ] agree the REST/OpenAPI conventions and the rule that frontend and backend share contracts through generated API types rather than source imports.

**Track C — Frontend**

- [ ] validate the information architecture and responsive navigation for public, staff, and client areas;
- [ ] annotate the HTML prototype and identify reusable components, page states, drawers, dialogs, and event-workspace modules.

**Track D — Quality and security**

- [ ] write acceptance journeys for every scope area and misuse cases for role/event isolation;
- [ ] define accessibility, browser, device, performance, and test-data targets.

**Track E — Platform, content, and adoption**

- [ ] inventory V1 content/media and existing operational spreadsheets/documents;
- [ ] confirm environments, ownership, review rhythm, training groups, and migration responsibilities.

Exit criteria:

- [ ] approved scope and glossary;
- [ ] approved role/permission matrix;
- [ ] signed-off core screens and design system;
- [ ] prioritized acceptance criteria for every module and track.

### Wave 1 — Engineering foundation

**Goal:** establish a secure, deployable development platform.

**Track A — Product, process, and UI/UX**

- [ ] establish design tokens, Celebrons bordeaux/white/gold palette, typography, spacing, forms, tables, cards, status language, and responsive behavior.

**Track B — Backend and data**

- [ ] create the independent `backend/` NestJS application;
- [ ] configure PostgreSQL and TypeORM entities, repositories, reviewed migrations, transactions, seeds, and test database;
- [ ] establish validation, standard errors, structured logs, request IDs, health endpoints, audit infrastructure, and OpenAPI generation;
- [ ] use NestJS scheduling plus PostgreSQL task records only where durable scheduled work is required; Redis is not used.

**Track C — Frontend**

- [ ] create the independent `frontend/` Next.js application with public, auth, staff, and client route groups;
- [ ] establish the component library, loading/empty/error states, responsive shell, and generated OpenAPI client under the frontend;
- [ ] ensure the frontend never imports backend source or accesses PostgreSQL directly.

**Track D — Quality and security**

- [ ] configure linting, formatting, type checking, unit/integration runners, Playwright, pre-commit checks, and initial smoke tests;
- [ ] add migration validation and API contract compatibility checks to CI.

**Track E — Platform, content, and adoption**

- [ ] configure local, test, staging, and production conventions, S3-compatible storage, secrets, backups, and separate frontend/backend pipelines;
- [ ] create representative demo data and document setup, deployment, TypeORM migration, and rollback procedures.

Exit criteria:

- [ ] one command starts the local platform;
- [ ] CI produces independently deployable frontend and backend builds;
- [ ] staging is accessible with health monitoring;
- [ ] migration and rollback procedures are tested.

### Wave 2 — Authentication, users, and access

**Goal:** give staff and clients secure, controlled access.

**Track A — Product, process, and UI/UX**

- [ ] approve the role/permission matrix and flows for invitation, Google login, activation, suspension, expiry, and access recovery;
- [ ] define role-aware navigation and clear forbidden/expired/invitation states.

**Track B — Backend and data**

- [ ] implement Google OAuth, secure HTTP-only sessions persisted/revoked through PostgreSQL, logout, and expiry;
- [ ] implement users, invitations, activation, suspension/reactivation, roles, permissions, event memberships, guards, and audit records;
- [ ] support Super Admin, Admin, Event Manager, Finance, Logistics, Protocol, and Client.

**Track C — Frontend**

- [ ] build Google login, invited-account onboarding, profile/session screens, forbidden states, and staff user/role administration;
- [ ] implement protected routes, role-aware menus/actions, and event-scoped client access.

**Track D — Quality and security**

- [ ] test every protected endpoint and frontend route by role and event membership;
- [ ] verify session revocation, OAuth state/CSRF controls, client isolation, direct-URL attacks, and audit completeness.

**Track E — Platform, content, and adoption**

- [ ] configure OAuth environments, callback URLs, secrets, account-invitation templates, and access-administration runbooks.

Exit criteria:

- [ ] every protected API endpoint has authorization tests;
- [ ] clients cannot discover or access unrelated events;
- [ ] suspended users lose access immediately;
- [ ] administrators can review security activity.

### Wave 3 — Public website, inquiry, and master data

**Goal:** replace V1 with a conversion-focused public presence and reusable operational catalogs.

**Track A — Product, process, and UI/UX**

- [ ] homepage, about, services, venues, portfolio/gallery, and contact pages;
- [ ] Celebrons visual system using rouge bordeaux, white, and gold, with the photographic and cinematic character of V1 refined for accessibility and modern responsive use;
- [ ] public-first navigation with a clearly identified secure portal connection in the header and footer;
- [ ] value proposition, event-type tagline, “why Celebrons”, key figures, process, testimonials, conversion calls to action, and rich footer;
- [ ] responsive French content and SEO metadata;
- [ ] social links, phone, email, map/location, and clear calls to action;
- [ ] approve staff catalog forms and preview states for every master-data area.

**Track B — Backend and data**

- [ ] implement public content, gallery, venue, service, inquiry, and contact APIs;
- [ ] implement venue, two-layout-template, table-type, event-type, invitation-category, inventory, service, personnel-role, and vendor master data;
- [ ] enforce venue price/capacity/layout constraints and maintain master-data history where operational records depend on it.

**Track C — Frontend**

- [ ] build all public pages, venue/service/gallery details, Google-authenticated portal entry, and inquiry/quote form;
- [ ] content and image administration;
- [ ] build staff CRUD, search, filter, status, preview, and media workflows for all master data;
- [ ] basic inquiry attribution and conversion analytics.

Master-data coverage includes:

- [ ] venue records with indoor/outdoor type, name, location, weekday/weekend price, capacity, table allowance, description, images, kitchen availability, and floor configuration;
- [ ] exactly two default floor-plan templates per active venue;
- [ ] table types with shape, dimensions, capacity, and image;
- [ ] event types and invitation categories;
- [ ] inventory catalog with category, unit, quantity, condition, and reorder threshold;
- [ ] staff/protocol roles and service catalog;
- [ ] vendor directory for photographers, MCs, caterers, decorators, and other providers.

**Track D — Quality and security**

- [ ] verify accessibility, responsive behavior, SEO/social metadata, inquiry abuse/rate limits, upload validation, and all catalog validation;
- [ ] test that only authorized staff can publish content or alter operational catalogs.

**Track E — Platform, content, and adoption**

- [ ] optimize/import approved V1 media and copy, configure analytics and contact delivery, and prepare URL redirects;
- [ ] document catalog ownership and content-publishing workflow.

Exit criteria:

- [ ] staff can publish public content without a deployment;
- [ ] visitors always enter through the informational website and authorized users can reach Google login without confusing the portal with a public registration form;
- [ ] a visitor can submit an inquiry and staff can view it;
- [ ] capacity and pricing validation prevent invalid venue records;
- [ ] venue layouts can be previewed.

### Wave 4 — CRM, event creation, launch, and command center

**Goal:** convert a prospect into a properly configured event.

**Track A — Product, process, and UI/UX**

- [ ] approve inquiry pipeline, event wizard, lifecycle transition rules, launch checklist, readiness scoring, alert rules, and event command-center information hierarchy;
- [ ] define event identity, primary client/event owners, family contacts, responsible manager, date/time, venue, guests, commercial details, notes, and client-access behavior.

**Track B — Backend and data**

- [ ] implement inquiry pipeline and conversion without duplicate contact entry;
- [ ] implement event creation, Lead → Negotiating → Confirmed → Planning → In Progress → Completed/Cancelled lifecycle, venue overlap control, price snapshots, agreement history, tasks, timeline, dashboard/calendar read models, notifications, and global event search;
- [ ] implement an authorized, audited, idempotent launch transaction for complete Confirmed events that copies a venue template, creates the event checklist/module records/installments/memberships, and publishes calendar/dashboard entries;
- [ ] expose the event command-center aggregate for readiness, days remaining, guests, tasks, balance, alerts, timeline, and client-portal state.

**Track C — Frontend**

- [ ] build inquiry pipeline, conversion, event wizard, calendar, statistics, five upcoming/recent lists, filters, notifications, and search results;
- [ ] build the dedicated `/dashboard/events/:eventId` command center with header, overview cards, alerts, timeline, next actions, client status, and role-aware quick actions;
- [ ] provide navigation to details, guests/invitations, floor plan, gallery/menu, logistics, personnel/vendors, finance, reports, and closeout;
- [ ] build client invitation/access controls from the event.

**Track D — Quality and security**

- [ ] test venue conflicts, lifecycle permissions, required launch data, transaction rollback, retry idempotency, price history, command-center accuracy, event isolation, and role-aware actions;
- [ ] automate inquiry → confirmed event → launched workspace as the first complete vertical journey.

**Track E — Platform, content, and adoption**

- [ ] prepare event-type checklist templates, readiness definitions, realistic event seeds, notifications, and staff operating guidance;
- [ ] instrument inquiry conversion, event setup time, launch failures, and overdue tasks.

Exit criteria:

- [ ] one inquiry can become an event without re-entering contact data;
- [ ] conflicting confirmed venue bookings are blocked;
- [ ] lifecycle and price changes are auditable;
- [ ] dashboard and calendar reflect updates correctly;
- [ ] an incomplete or unconfirmed event cannot be launched;
- [ ] retrying a launch cannot duplicate plans, tasks, installments, or permissions;
- [ ] opening a launched event always leads to its dedicated command center;
- [ ] each role sees only the event modules and actions allowed by its permissions;
- [ ] the command-center readiness and alerts agree with the underlying module data.

### Wave 5 — Invitations, guests, RSVP, and check-in

**Goal:** manage the complete guest journey.

**Track A — Product, process, and UI/UX**

- [ ] approve invitation party versus individual guest terminology, Single/Couple/Custom rules, Digital QR/Printed/Both formats, name-based/alphanumeric display, RSVP states, delivery states, and event-day entry flow;
- [ ] design batch creation, import correction, invitation detail, guest detail, client editing, scanner, manual lookup, and live-attendance states.

**Track B — Backend and data**

- [ ] implement secure unique invitation codes/QR payloads, invitation batches, party capacity, named guests, RSVP, dietary/allergy/contact data, distribution, and reminders;
- [ ] implement validated spreadsheet import with preview and row-level errors;
- [ ] implement atomic QR/manual check-in, entry points, attendance totals, and duplicate prevention.

**Track C — Frontend**

- [ ] build staff invitation/guest lists, filters, metadata, batch generation, import correction, QR/print export, delivery tracking, and attendance dashboard;
- [ ] build client portal invitation/guest/RSVP editing within event permissions;
- [ ] build phone-first QR scanner plus code/name/manual lookup and duplicate warnings.

**Track D — Quality and security**

- [ ] test code entropy/uniqueness, party limits, import atomicity, event isolation, QR tampering, concurrent duplicate check-in, accessibility, and realistic mobile event load.

**Track E — Platform, content, and adoption**

- [ ] prepare invitation/print templates, import template, reminder content, check-in device guidance, fallback procedure, and protocol training.

Exit criteria:

- [ ] generated codes are unique and non-guessable;
- [ ] invitation party limits cannot be exceeded;
- [ ] imports report row-specific errors without partial silent failures;
- [ ] check-in remains usable on a phone under realistic event load.

### Wave 6 — Professional floor-plan and seating studio

**Goal:** create a practical visual plan for venues, tables, and guests.

**Track A — Product, process, and UI/UX**

- [ ] validate the editor against the interaction language of `plan.celebrons.org` while preserving the Celebrons V2 design system;
- [ ] define object library, toolbar, table-group colors/filters, inspector, occupancy/conflict indicators, history, publish, print, and mobile read-only behavior.

**Track B — Backend and data**

- [ ] copy one of exactly two venue templates into an event-owned versioned plan;
- [ ] persist tables, chairs, stage, door/entrance, aisle, buffet/service, kitchen, dance floor, and custom zones with coordinates, dimensions, rotation, layer, group, and capacity;
- [ ] implement guest/table/seat assignment, capacity/conflict rules, autosave revisions, optimistic concurrency, named versions, restore, and publish snapshots.

**Track C — Frontend**

- [ ] build the professional canvas with select, drag, pan, zoom in/out/fit, rotate, duplicate, align, delete, snap-to-grid, undo, and redo;
- [ ] build the object library and inspector for name, group, dimensions, capacity, rotation, and assigned guests;
- [ ] implement group A/B/C… filters/colors, occupancy, unassigned guests/chairs, shared tables, conflict resolution, history, restore, publish, client preview, mobile operational view, print, and PDF.

**Track D — Quality and security**

- [ ] test all capacity layers, conflicting assignments, two-user edit conflicts, autosave/reconnect, undo/redo, restore, published immutability, browser performance, and print/mobile fidelity.

**Track E — Platform, content, and adoption**

- [ ] prepare realistic venue templates and table assets, export rendering, editor telemetry, event-manager guidance, and version-recovery runbook.

Exit criteria:

- [ ] two users cannot silently overwrite each other’s plan;
- [ ] a published plan can always be restored;
- [ ] all capacity conflicts are visible before publishing;
- [ ] printed and mobile plans match the published version.

### Wave 7 — Event operations, media, personnel, and vendors

**Goal:** coordinate everything Celebrons must prepare, dispatch, and supervise.

**Track A — Product, process, and UI/UX**

- [ ] approve menus/service times, inventory dispatch/return, drink reconciliation, gift chain-of-custody, personnel briefing/attendance, vendor participation, gallery categories, incident, and client-approval workflows;
- [ ] define which operational details are internal, client-visible, client-editable, or require approval.

**Track B — Backend and data**

- [ ] implement menu/foods/drinks/dietary/service-time records and approval history;
- [ ] implement reservation, allocation, checkout, delivery, return, condition, damage, loss, reconciliation, and immutable inventory movements;
- [ ] implement client-owned drink intake/usage/return and gift receipt/holder handover/release/acknowledgement;
- [ ] implement staff/protocol roles, required headcount, assignment, zone, shift, confirmation, attendance, notes, payout basis, and staffing-gap alerts;
- [ ] implement vendor service/contact/contract/schedule/status/attachments, checklists, incidents, albums/media ordering, visibility, and publication.

**Track C — Frontend**

- [ ] build gallery/media, menu, logistics movement, drinks, gift custody, personnel assignment, vendor, checklist, and incident modules inside the event command center;
- [ ] include inventory availability, loading/return documents, custody timeline/signatures, staffing gaps, day plan, vendor status, upload/order/publish controls, and client approval/comment screens.

**Track D — Quality and security**

- [ ] test stock double-allocation, movement reconciliation, drink variance, custody attribution, staffing readiness, media privacy/signed access, upload validation, client visibility, and audit completeness.

**Track E — Platform, content, and adoption**

- [ ] configure private media storage/lifecycle, operational document templates, movement labels, custody forms, briefing output, and logistics/protocol training.

Exit criteria:

- [ ] stock movement history explains every quantity change;
- [ ] unavailable items cannot be allocated twice without authorization;
- [ ] every gift holder change is attributable and timestamped;
- [ ] event managers can see operational readiness from one workspace.

### Wave 8 — Finance, reporting, client documents, and closeout

**Goal:** provide reliable balances, costs, operational results, and management insight.

**Track A — Product, process, and UI/UX**

- [ ] approve payment/installment, receipt, expense approval, correction, report calculation, client-document visibility, and closeout policies;
- [ ] define finance-only versus management versus client-visible fields and report/filter/export needs.

**Track B — Backend and data**

- [ ] implement contract total/payment mode, installment schedule, due/overdue state, and manual cash/bank/mobile-money payments with reference/evidence;
- [ ] implement categorized expenses, transport, protocol/staff payouts, supplier costs, approval, and append-only adjustment/reversal;
- [ ] implement balances, profitability, cash position, RSVP, attendance, seating, personnel, vendor, and inventory-variance reports;
- [ ] implement PDF/CSV data contracts, printable receipt, closeout checklist, frozen operational snapshot, and authorized post-close corrections.

**Track C — Frontend**

- [ ] build finance overview, installments, payment dialog, receipts, expense entry/approval, filters, balance/overdue indicators, and profitability views;
- [ ] build reporting dashboards/filters/exports and the event closeout workflow;
- [ ] expose only approved contracts, quotes, receipts, installment/balance information, documents, decisions, and validations in the client portal.

**Track D — Quality and security**

- [ ] reconcile sample ledgers and report calculations; test overpayment, reversal, concurrent entry, closeout locking, export accuracy, finance permissions, client redaction, and full audit history.

**Track E — Platform, content, and adoption**

- [ ] prepare receipt/report/closeout templates, retention rules, financial operating procedures, sample reconciliations, and finance/administrator training.

Exit criteria:

- [ ] balances reconcile from the transaction ledger;
- [ ] finance access is restricted and audited;
- [ ] reports match agreed sample calculations;
- [ ] a completed event produces a full closeout report.

### Wave 9 — Hardening, migration, training, and launch

**Goal:** release a stable product and safely replace the public V1 site.

**Track A — Product, process, and UI/UX**

- [ ] complete French copy, responsive, accessibility, usability, public conversion, client-portal, and operational workflow acceptance;
- [ ] resolve scope gaps against the prototype/brainstorm inventory and obtain final product-owner approval.

**Track B — Backend and data**

- [ ] freeze/review TypeORM migrations, optimize critical queries, validate data constraints, rehearse imports, and resolve security/performance findings;
- [ ] verify scheduled PostgreSQL tasks, audit retention, report reproducibility, and all closeout rules.

**Track C — Frontend**

- [ ] complete cross-browser/device validation, performance optimization, error/offline-safe event-day behavior, analytics, and production configuration;
- [ ] verify every public, staff, event command-center, floor-plan, and client-portal route.

**Track D — Quality and security**

- [ ] run the full regression, authorization, penetration, rate-limit, upload, dependency, accessibility, and recovery suites;
- [ ] performance-test invitation generation, imports, floor-plan usage, reports, and concurrent check-in under realistic event load.

**Track E — Platform, content, and adoption**

- [ ] rehearse backup/restore, production deployment, rollback, monitoring, alerts, runbooks, and incident contacts;
- [ ] migrate V1 content/media, activate redirects, train administrators/event managers/finance/logistics/protocol/client-support users, and execute staged launch/support.

Exit criteria:

- [ ] all critical end-to-end journeys pass in production-like staging;
- [ ] no unresolved critical or high-severity defect;
- [ ] restore rehearsal meets the agreed recovery target;
- [ ] Celebrons product owner approves launch.

## 6. Scope coverage map

The following map prevents prototype or brainstorm features from disappearing between waves:

| Functional area | Included capabilities | Primary wave / tracks |
| --- | --- | --- |
| Public experience | Home, about, services, venues, gallery, process, testimonials, contact, quote, SEO, analytics, rich footer, portal entry | Wave 3 / A, B, C, D, E |
| Authentication and administration | Google login, invitations, sessions, users, roles, permissions, event access, audit, notifications, global search | Waves 2–4 / B, C, D |
| Master data | Tables, venues, two default layouts, inventory, services, event/invitation types, personnel roles, vendors | Wave 3 / A, B, C, D |
| CRM and event lifecycle | Inquiry, negotiation, conversion, event wizard, contacts/families, venue conflict, pricing snapshot, lifecycle, launch | Wave 4 / A, B, C, D |
| Dashboard and command center | Calendar, statistics, five upcoming/recent events, readiness, tasks, alerts, timeline, client status, module navigation | Wave 4 / B, C, D |
| Invitations and guests | Batches, categories/formats/codes, parties, guests, import, RSVP, dietary data, distribution, print/QR, check-in | Wave 5 / A, B, C, D, E |
| Floor plan and seating | Venue maquette, library, table groups A/B/C…, zones, drag/pan/zoom/rotate/duplicate/align/grid/undo, inspector, seating, versions, publish/print | Wave 6 / A, B, C, D, E |
| Gallery and menu | Albums, media, ordering, visibility/publication, menu/service times, dietary needs, client approvals/comments | Wave 7 / A, B, C, D |
| Logistics and custody | Reservation, dispatch/delivery/return, damage/loss, drinks, gifts, transport, documents, incidents | Wave 7 / A, B, C, D, E |
| Personnel and vendors | Required roles, assignments, zones/shifts, attendance, payout basis, staffing alerts, provider contracts/schedules/status | Wave 7 / A, B, C, D |
| Finance | Contract, installments, manual cash/bank/mobile-money receipts, evidence, expenses, approvals, reversals, balances, profitability | Wave 8 / A, B, C, D |
| Reports and closeout | RSVP/attendance/seating/inventory/staff/vendor/finance reports, PDF/CSV, final checklist, frozen snapshot | Wave 8 / A, B, C, D, E |
| Client portal | Event progress, invitations/guests, plan preview, gallery/menu, approvals/comments, documents/contract/receipts/balance | Waves 4–8 / A, B, C, D |

Any new prototype screen or approved brainstorm item must be added to this map, assigned to a wave/track, and supplied with acceptance criteria before implementation.

## 7. Workstreams

The waves are delivered through coordinated workstreams aligned with Tracks A–E:

### Product and design

- [ ] discovery, glossary, flow design, usability testing, design system, and copy;
- [ ] weekly review with the Celebrons product owner;
- [ ] accessibility and mobile behavior included in initial design.

### Backend

- [ ] TypeORM entities/migrations, domain services, authorization, API, PostgreSQL-backed scheduled tasks, reports, and audit;
- [ ] database constraints enforce critical invariants in addition to application checks.

### Frontend

- [ ] public routes, staff workspace, client portal, guided workflows, floor-plan editor, and event-day tools;
- [ ] shared components and consistent loading/error/empty states.

### Quality

- [ ] test strategy and fixtures begin in Wave 1;
- [ ] feature tests are delivered with each wave;
- [ ] regression suite grows continuously and runs in CI.

### Platform and operations

- [ ] deployment, secret management, backups, observability, security scanning, and incident readiness;
- [ ] production data is never copied into local development.

## 8. Test strategy

### Unit tests

Prioritize:

- [ ] venue and table capacity;
- [ ] venue pricing and event snapshots;
- [ ] invitation code generation;
- [ ] role and permission decisions;
- [ ] payment balance and adjustment rules;
- [ ] inventory availability and variance;
- [ ] event lifecycle transitions.

### Integration tests

Cover:

- [ ] Google identity-to-user linking;
- [ ] event membership boundaries;
- [ ] transactional payment and stock movement writes;
- [ ] audit persistence;
- [ ] floor-plan versioning and concurrency;
- [ ] signed media access;
- [ ] PostgreSQL-backed scheduled-task retries and idempotency.

### End-to-end tests

Automate:

- [ ] Public inquiry to confirmed event.
- [ ] Client invitation and portal access.
- [ ] Guest import, RSVP, QR generation, and check-in.
- [ ] Floor plan, seating assignment, publish, and print.
- [ ] Inventory checkout and return.
- [ ] Installment, expense, balance, and closeout report.
- [ ] Cross-event access-denial attempts.

## 9. Data migration

V1 is primarily static content and media, so migration will:

- [ ] inventory usable text, logos, images, contact details, and portfolio media;
- [ ] correct outdated details and French copy before import;
- [ ] optimize images and record consent/usage status;
- [ ] import selected content into V2 master data and public pages;
- [ ] map existing URLs to V2 destinations;
- [ ] retain an offline V1 archive.

No operational records should be inferred from V1 HTML. Existing operational spreadsheets or paper records require a separate mapping and validation exercise.

## 10. Project controls

- [ ] Weekly product demo using staging.
- [ ] Wave acceptance recorded by the Celebrons product owner.
- [ ] Architecture decisions recorded as short ADR documents.
- [ ] Schema changes reviewed for migration and reporting impact.
- [ ] Scope changes evaluated against workflow, permission, test, and launch impact.
- [ ] Critical defects block wave acceptance.

## 11. Product success metrics

Track after launch:

- [ ] inquiry-to-confirmed-event conversion rate;
- [ ] average time to create and configure an event;
- [ ] percentage of guest RSVPs completed digitally;
- [ ] average guest check-in time;
- [ ] seating conflicts detected before event day;
- [ ] inventory variance and loss per event;
- [ ] overdue client balance;
- [ ] event gross margin;
- [ ] task completion before event day;
- [ ] weekly active staff and client portal adoption.

## 12. Immediate next actions

- [ ] Appoint one Celebrons product owner with final workflow authority.
- [ ] Run discovery sessions with administration, finance, logistics, and protocol staff.
- [ ] Confirm the French domain glossary and role/permission matrix.
- [ ] Gather real examples: event agreement, invitation list, seating plan, inventory sheet, payment receipt, expense sheet, and closeout report.
- [ ] Review and annotate the HTML prototype.
- [ ] Convert accepted prototype flows into implementation-ready user stories.
- [ ] Begin Wave 1 only after the Wave 0 exit criteria are accepted.
