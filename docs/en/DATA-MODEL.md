# DATA-MODEL

## Objective
This document describes the **functional data model used from the service layer** (`src/services/*.js`) and UI flows.

---

## 1) Model Principles

### 1.1 Logical Multi-tenant
Isolation between clients (clubs) is primarily implemented using `clubId` in domain collections:
- `students`
- `payments`
- `groups`
- `trainers`
- `trainingPlaces`
- `enrollments`
- `tenantPayments` (platform billing)

### 1.2 Identity and Ownership
- Authenticated user in Firebase Auth (`uid`)
- Application profile in `users/{uid}`
- Main relation: `users.clubId -> clubs/{clubId}`

### 1.3 Temporal Traceability
Most entities include timestamp markers with `serverTimestamp()` (`createdAt`, `updatedAt`, state events).

---

## 2) Entities and Fields

### 2.1 `clubs`
Represents the tenant/club.

Fields:
- `name: string`
- `ownerId: string` (Firebase uid)
- `active: boolean`
- `plan: "free" | "pro"`
- `planStatus: "active" | "trial" | "suspended"`
- `trainer: string`
- `createdAt: Timestamp`
- `updatedAt?: Timestamp`
- `proSince?: Timestamp`
- `trialStartedAt?: Timestamp`
- `trialEndsAt?: Timestamp`
- `suspendedAt?: Timestamp`
- `termsAccepted?: boolean`
- `termsAcceptedAt?: Timestamp`
- `trainerPhone?: string`

Relations:
- 1 club -> N users (usually 1 owner + potential future roles)
- 1 club -> N students/payments/groups/trainers/enrollments/trainingPlaces

---

### 2.2 `users`
Application profile by authenticated `uid`.

Fields:
- `uid: string`
- `name: string`
- `email: string`
- `role: "owner" | "superadmin"`
- `clubId: string`
- `createdAt: Timestamp`
- `lastLoginAt: Timestamp`
- `active: boolean`
- `phone?: string`
- `termsAccepted?: boolean`
- `termsAcceptedAt?: Timestamp`

Relations:
- `users.uid` references Firebase Auth identity.
- `users.clubId` links to the tenant.

---

### 2.3 `students`
Student registered manually, via import, or by enrollment approval.

Fields:
- `clubId: string`
- `student: object`
  - `fullName?: string`
  - `documentNumber?: string`
  - `phone?: string`
  - `address?: string`
  - `belt?: string`
  - `weightKg?: number | ""`
  - `birthDate?: string`
- `health: object`
- `guardians: object`
- `training: object`
  - `groupId?: string`
- `status: "active" | "inactive"`
- `source: "import" | "enrollment"`
- `createdAt: Timestamp`
- `updatedAt?: Timestamp`

Relations:
- N students -> 1 club
- N students -> 0..1 group (via `training.groupId`)
- 1 student -> N payments (`payments.studentId`)

---

### 2.4 `payments`
Student monthly payment.

Fields:
- `clubId: string`
- `studentId: string`
- `amount: number` (COP parsed)
- `method: string`
- `receiptNumber: string` (`REC-YYYY-####`)
- `year: number`
- `paidAt: Timestamp`
- `validFrom: Timestamp`
- `validUntil: Timestamp`
- `notes: string`
- `status: "paid"`
- `createdAt: Timestamp`

Functional rules:
- Free Clubs have a limit for recent payments (`FREE_PLAN_PAYMENT_LIMIT`).
- Validity is calculated as `paidAt + 30 days`.

---

### 2.5 `groups`
Operational definition of groups/classes.

Fields:
- `clubId: string`
- `name: string`
- `discipline: string`
- `trainerIds: string[]`
- `days: string[]`
- `startTime: string`
- `endTime: string`
- `color: string`
- `notes: string`
- `active: boolean`
- `visibleInForm: boolean`
- `createdAt: Timestamp`
- `updatedAt?: Timestamp`

Relations:
- N groups -> 1 club
- N groups <-> N trainers (via `trainerIds`)
- Functional sync with `trainingPlaces` by `groupId`.

---

### 2.6 `trainers`
Club trainers.

Fields:
- `clubId: string`
- `firstName: string`
- `lastName: string`
- `fullName: string`
- `dni: string`
- `email: string`
- `phone: string`
- `active: boolean`
- `createdAt: Timestamp`
- `updatedAt?: Timestamp`

---

### 2.7 `trainingPlaces`
Projection of groups for public enrollment use (name, trainers, visibility, status).

Fields:
- `clubId: string`
- `groupId: string`
- `name: string`
- `trainerIds: string[]`
- `active: boolean`
- `visibleInForm: boolean`
- `createdAt: Timestamp`
- `updatedAt?: Timestamp`

---

### 2.8 `enrollments`
Enrollments received from public forms.

Fields:
- `clubId: string`
- `student: object`
- `health: object`
- `guardians: object`
- `training: object`
- `source: string` (e.g. `public_form`)
- `status: "pending" | "approved" | "rejected"`
- `createdAt: Timestamp`
- `approvedAt?: Timestamp`
- `rejectedAt?: Timestamp`

State transitions:
- Initial state: `pending`
- Approval: creates `students` + marks `approved`
- Rejection: marks `rejected`

---

### 2.9 `tenantPayments`
SaaS subscription charges per tenant (superadmin use).

Fields:
- `clubId: string`
- `amount: number`
- `method: string`
- `paidAt: Timestamp`
- `validFrom: Timestamp`
- `validUntil: Timestamp`
- `receiptNumber: string` (`CMP-YYYY-####`)
- `year: number`
- `notes?: string`
- `createdAt: Timestamp`

---

## 3) Relationships (Text-Diagram)

- `users (uid)` -> `clubs (clubId)`
- `clubs (id)` -> `students.clubId`
- `clubs (id)` -> `groups.clubId`
- `clubs (id)` -> `trainers.clubId`
- `clubs (id)` -> `trainingPlaces.clubId`
- `groups (id)` -> `trainingPlaces.groupId`
- `students (id)` -> `payments.studentId`
- `clubs (id)` -> `payments.clubId`
- `clubs (id)` -> `enrollments.clubId`
- `clubs (id)` -> `tenantPayments.clubId`

---

## 4) Relevant Indexes/Queries (Derived)
The code executes common compound filters that typically require Firestore indexes:
- `payments`: `clubId + year`
- `payments`: `clubId + paidAt`
- `payments`: `clubId + validUntil`
- `enrollments`: `clubId + orderBy(createdAt desc)`
- `tenantPayments`: `clubId + year`

> Missing Firebase indexes will cause these queries to fail at runtime with index creation messages.

---

## 5) Integrity Observations

Strengths:
- Consistent separation by `clubId` in core modules
- Server-side timestamps for basic auditing
- Explicit state in entities with lifecycle (`enrollments`, `groups`, `students`)

Current risks:
- Non-typed or non-validated schema server-side in repo
- Sequential number generation via counting (`size + 1`) is not safe under high concurrency
- Semantic duplication between `groups` and `trainingPlaces` requires synchronization

---

## 6) Sensitive Fields and Publication
This document **does not publish actual configuration values** or user data. Any potentially sensitive data will be presented using a placeholder.
