# DECISIONS

## 1) Key Architectural Decisions

### 1.1 Firebase as Managed Backend
**Decision:** use Auth + Firestore (+ Storage) without a dedicated backend in the repo.

**Reason:** accelerate MVP delivery and reduce infrastructure operational load.

**Trade-off:**
- ✅ high implementation speed,
- ❌ critical logic and security depend on external configuration (rules/functions not visible here).

### 1.2 Logical Multitenancy via `clubId`
**Decision:** model data isolation using the `clubId` field in domain collections.

**Trade-off:**
- ✅ simple and functional for early stage,
- ❌ requires strict discipline in queries/rules to avoid data leakage between tenants.

### 1.3 Service Layer Separate from Pages
**Decision:** encapsulate Firestore in `src/services/*`.

**Trade-off:**
- ✅ better maintainability and rule centralization,
- ❌ no static typing, contract errors may emerge at runtime.

---

## 2) Product/Business Decisions Visible in Code

### 2.1 Embedded Free/Pro Strategy in Flows
**Decision:** apply Free limits (students/payments) directly in operations.

**Impact:**
- ✅ supports monetization from the product,
- ❌ turns limitations into an upgrade lever.

### 2.2 Multitenant Superadmin
**Decision:** include SaaS admin module (plans, trial, suspension, subscriptions).

**Impact:**
- ✅ enables B2B commercial operation within the same app,
- ❌ increases functional complexity and permissions.

---

## 3) Performance Decisions

### 3.1 Aggregated Queries on Client
**Decision:** load collections and derive metrics on the frontend.

**Trade-off:**
- ✅ quick implementation,
- ❌ higher latency/cost with large datasets.

### 3.2 Receipt Numbering by Count
**Decision:** consecutive `size + 1` by year/club.

**Trade-off:**
- ✅ simple to implement,
- ❌ risk of collisions under high concurrency.

---

## 4) UI Design Decisions

### 4.1 Tailwind + Shared Button Classes
**Decision:** atomic utilities with a small layer of CSS components (`btn-*`).

**Trade-off:**
- ✅ speed and visual consistency,
- ❌ potential verbosity in JSX and formal tokenized design debt.

### 4.2 Commercial Landing Separate from Operational Panel
**Decision:** coexistence of marketing UX and admin UX in the same SPA.

**Trade-off:**
- ✅ onboarding and conversion in a single product,
- ❌ larger bundle size if no route-based code splitting.

---

## 5) Project Structure Decisions
- Organization by type (`pages`, `components`, `services`, `utils`, `context`).
- Favor simplicity for a small team.
- Scalable in the medium term, although with growth, it’s better to evolve to domain-based modules.

---

## 6) Security Decisions

### Evidence
- Route guards based on auth state and role.
- Permission checks in some services (`ensureClubAccess`, `requireSuperAdmin`).

### Observations
- This document deliberately omits sensitive values for publication.

---

## 7) Scalability Decisions
- Speed of product development was prioritized over backend sophistication.
- The current architecture is suitable for an active MVP, but will require:
  - transactional operations for billing/serial numbers,
  - server-side aggregations,
  - observability and automated testing.

---

## 8) Discarded Alternatives

- Custom Backend (Node) + SQL DB: discarded due to speed in product development and flexibility in the database.
- Global State Manager (Redux/Zustand): avoided in favor of local state + minimal contexts.
- Non-multitenancy design: abandoned as the project grew and the need for superadmin and tenant management became clear.