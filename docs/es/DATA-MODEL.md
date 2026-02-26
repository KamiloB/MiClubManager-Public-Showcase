# DATA-MODEL

## Objetivo
Este documento describe el **modelo de datos funcional usado desde la capa de servicios** (`src/services/*.js`) y los flujos de UI.

---

## 1) Principios del modelo

### 1.1 Multi-tenant lógico
El aislamiento entre clientes (clubes) se implementa principalmente con `clubId` en colecciones de dominio:
- `students`
- `payments`
- `groups`
- `trainers`
- `trainingPlaces`
- `enrollments`
- `tenantPayments` (facturación de la plataforma)

### 1.2 Identidad y pertenencia
- Usuario autenticado en Firebase Auth (`uid`)
- Perfil de aplicación en `users/{uid}`
- Relación principal: `users.clubId -> clubs/{clubId}`

### 1.3 Trazabilidad temporal
La mayoría de entidades incluyen marcas temporales con `serverTimestamp()` (`createdAt`, `updatedAt`, eventos de estado).

---

## 2) Entidades y campos

### 2.1 `clubs`
Representa el tenant/club.

Campos:
- `name: string`
- `ownerId: string` (uid Firebase)
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

Relaciones:
- 1 club -> N users (aunque normalmente 1 owner + posibles roles futuros)
- 1 club -> N students/payments/groups/trainers/enrollments/trainingPlaces

---

### 2.2 `users`
Perfil de aplicación por `uid` autenticado.

Campos:
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

Relaciones:
- `users.uid` referencia identidad Firebase Auth.
- `users.clubId` enlaza al tenant.

---

### 2.3 `students`
Alumno registrado manualmente, por importación o por aprobación de inscripción.

Campos:
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

Relaciones:
- N students -> 1 club
- N students -> 0..1 group (vía `training.groupId`)
- 1 student -> N payments (`payments.studentId`)

---

### 2.4 `payments`
Pago de mensualidad de estudiante.

Campos:
- `clubId: string`
- `studentId: string`
- `amount: number` (COP parseado)
- `method: string`
- `receiptNumber: string` (`REC-YYYY-####`)
- `year: number`
- `paidAt: Timestamp`
- `validFrom: Timestamp`
- `validUntil: Timestamp`
- `notes: string`
- `status: "paid"`
- `createdAt: Timestamp`

Reglas funcionales:
- Clubs Free tienen límite de pagos recientes (`FREE_PLAN_PAYMENT_LIMIT`).
- Vigencia calculada como `paidAt + 30 días`.

---

### 2.5 `groups`
Definición operativa de grupos/clases.

Campos:
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

Relaciones:
- N groups -> 1 club
- N groups <-> N trainers (por `trainerIds`)
- Sincronización funcional con `trainingPlaces` por `groupId`.

---

### 2.6 `trainers`
Entrenadores del club.

Campos:
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
Proyección de grupos para uso en inscripción pública (nombre, entrenadores, visibilidad, estado).

Campos:
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
Inscripciones recibidas desde formulario público.

Campos:
- `clubId: string`
- `student: object`
- `health: object`
- `guardians: object`
- `training: object`
- `source: string` (ej. `public_form`)
- `status: "pending" | "approved" | "rejected"`
- `createdAt: Timestamp`
- `approvedAt?: Timestamp`
- `rejectedAt?: Timestamp`

Transición de estados:
- Alta inicial: `pending`
- Aprobación: crea `students` + marca `approved`
- Rechazo: marca `rejected`

---

### 2.9 `tenantPayments`
Cobros de suscripción del SaaS a cada tenant (uso superadmin).

Campos:
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

## 3) Relaciones (texto-diagrama)

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

## 4) Índices/consultas relevantes (derivadas)
El código ejecuta filtros compuestos frecuentes que típicamente requieren índices Firestore:
- `payments`: `clubId + year`
- `payments`: `clubId + paidAt`
- `payments`: `clubId + validUntil`
- `enrollments`: `clubId + orderBy(createdAt desc)`
- `tenantPayments`: `clubId + year`

> Si faltan índices en Firebase, estas queries fallarán en runtime con mensajes de creación de índice.

---

## 5) Observaciones de integridad

Fortalezas:
- separación por `clubId` consistente en módulos core,
- timestamps server-side para auditoría básica,
- estado explícito en entidades con ciclo de vida (`enrollments`, `groups`, `students`).

Riesgos actuales:
- esquema no tipado ni validado server-side en repo,
- generación de consecutivos por conteo (`size + 1`) no es segura bajo alta concurrencia,
- existencia de duplicidad semántica entre `groups` y `trainingPlaces` que exige sincronización.

---

## 6) Campos sensibles y publicación
Este documento **no publica valores reales de configuración** ni datos de usuarios. Cualquier dato potencialmente sensible se presentara mediante un placeholder.
