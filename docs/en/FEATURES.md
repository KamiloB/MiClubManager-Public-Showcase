# FEATURES

## 1) Authentication and Access Module
- Login with Firebase Auth.
- Owner registration with automatic club creation/recovery.
- Enrichment of app profile (`users`) with role and `clubId`.
- Private routing for internal operations and exclusive superadmin route.

## 2) Public Landing (Marketing)
- Main commercial page with:
  - value-oriented hero.
  - list of problems/solutions.
  - pricing and CTA.
  - screenshot carousel.
  - links to terms and privacy policy.

## 3) Operational Dashboard
- Main KPIs per club:
  - active students,
  - students up to date,
  - income in the last 30 days,
  - number of overdue payments.
- Income graph.
- Segmentation by payment status.

## 4) Student Management
- List with search and sorting.
- Manual student creation.
- Complete profile editing (personal data, health, guardians, training).
- Activate/deactivate student.
- Group assignment.
- Data export.
- Bulk import from Excel with field validation, duplicates check, and field normalization.
- Free plan restriction (student limit).

## 5) Group Management
- CRUD for groups with schedule, discipline, color, trainers, and notes.
- Active/inactive status.
- Visibility in public form (`visibleInForm`).
- Group calendar view.
- Synchronization with `trainingPlaces` for status and visibility.

## 6) Trainer Management
- Create/edit trainer.
- Search and sorting.
- Activate/deactivate.
- Assign trainers to groups.

## 7) Student Payments
- Monthly fee registration with payment method.
- Automatic validity calculation (+30 days).
- Receipt numbering by year and club.
- Payment history.
- Validity status (active/expired).
- Printable/downloadable receipt and automatic WhatsApp sending with a pre-set message and auto-download of the image.
- Payment limit for Free plan.

## 8) Enrollments
### Public Flow
- Multi-step form:
  1. training,
  2. student,
  3. health,
  4. guardians,
  5. terms/consent.
- Progressive validation by step.
- `pending` registration in `enrollments`.

### Internal Flow
- Enrollment list.
- Approval (creates student and marks enrollment `approved`).
- Rejection (`rejected`).

## 9) Settings
- Club settings (name, branding, contact info based on available view).
- Public enrollment link generation/use by `clubId`.

## 10) Tenant Management (Superadmin)
- Consolidated view of tenants with plan, status, and student volume.
- Change plan Free/Pro.
- Suspend tenant.
- Activate Pro trial.
- Subscription billing records.
- Tenant payment history + receipts.

## User Flows
- **New Owner**: register → auto-create club → dashboard.
- **Operational Owner**: students/groups/trainers → payments → follow-up on dashboard.
- **Applicant**: public form → pending enrollment.
- **Internal Admin**: review enrollments → approve/reject.
- **SaaS Superadmin**: tenants panel → plans/trials/billing.

## Permissions
- Non-authenticated: landing + public enrollment + terms/privacy policy.
- Authenticated user: internal club modules.
- Superadmin: additional access to `/admin/tenants`.

## Relevant Edge Cases
- Non-existent club or link without `clubId` in public enrollment.
- Free plan limit reached for students/payments.
- Duplicates in bulk import by document.
- Inactive related entities (groups/trainers hidden in public form).

## Current Limitations
- No own backend for concurrent critical operations.

## Inferred Future Features
- "Events" module now functional to create events directly on the platform and manage athletes, pyramids, and competition tickets.
- More robust analytics and multitenant billing.
- Strengthening of server-side security (rules/functions).