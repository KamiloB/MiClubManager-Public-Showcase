# ARCHITECTURE

## 1) System Overview
Mi Club Manager implements a **frontend-centric** architecture with a backend managed by Firebase.
The React client handles:

- UI rendering  
- Navigation  
- Form validations  
- Invocation of domain services  

Firestore acts as the main storage and database for:

- Users  
- Clubs  
- Students  
- Payments  
- Groups  
- Trainers  
- Enrollments  

---

## 2) How It Is Structured

### Frontend

Main layers:

1. **Pages (`src/pages`)**  
   Each screen organizes the required actions so everything works correctly.

2. **Components (`src/components`)**  
   Reusable UI elements, modals, cards, charts, enrollment steps.

3. **Context (`src/context`)**

   - `AuthContext`: session + app profile (`appUser`)  
   - `ToastContext`: global feedback and runtime error capture  

4. **Services (`src/services`)**  
   Firebase access + application business rules.

5. **Utils (`src/utils`)**  
   Formatting, validations, dates, and specific helpers.

---

### Backend (BaaS)

There is no dedicated server in the repository.

The project uses:

- **Firebase Auth** for identity  
- **Cloud Firestore** for persistence  
- **Firebase Storage** for assets/files  

---

### Database (Firestore)

Collections from services:

- `users`
- `clubs`
- `students`
- `payments`
- `groups`
- `trainers`
- `trainingPlaces`
- `enrollments`
- `tenantPayments` (SaaS billing to tenants)

---

### External Services

- Firebase (Auth, Firestore, Storage)  
- Netlify (SPA deployment, rewrite to `index.html`)  

---

## 3) Architectural Patterns Used

- **Service Layer Pattern**  
  Pages do not access Firestore directly; they delegate to `*.service.js`.

- **Context-Based State Sharing**  
  Global auth/toasts without an external state manager.

- **Client-Side Route Guarding**  
  Public/private routes and superadmin control.

- **No BFF by Design**  
  Business logic lives in the client + Firebase rules.

---

## 4) Main Data Flows

### A. Login + Provisioning

1. Firebase Auth emits `onAuthStateChanged`.
2. `ensureUserAndClub` ensures the existence of `users/{uid}` and `clubs/{clubId}`.
3. The client stores `user` and `appUser`; private routes are enabled.

---

### B. Public Enrollment

1. External user accesses `/inscripcion?clubId=...`.
2. Completes a multi-step wizard.
3. Data is persisted in `enrollments` with `pending` status.
4. In the internal panel, it is approved/rejected. Approval creates `students`.

---

### C. Payments and Validity

1. Operator creates a payment with a base date.
2. Service generates `receiptNumber`, calculates `validUntil` (+30 days), and saves the payment.
3. Dashboard and listings derive validity status from timestamps.

---

### D. Multitenant Administration

1. Superadmin queries `clubs/users/students` for an overview.
2. Can change plan, suspend, grant trial, and create monthly billing.
3. Monthly charges generate records in `tenantPayments`.

---

## 5) Diagram Explained in Text

`Browser (React SPA)`
→ `AuthContext / ToastContext`
→ `Pages`
→ `Service Layer`
→ `Firebase SDK`
→ `Firestore/Auth/Storage`

The flow is synchronous per screen in UX terms, but asynchronous over the network.

The UI handles:

- Local loading/error per component  
- Global feedback via toast  

---

## 6) State Management

- **Minimal global state**: authentication and notifications  
- **Local per-page state**: filters, modals, tables, forms, loading flags  
- **No advanced caching** or centralized invalidation (each page reloads as needed)

---

## 7) Error Handling

- `try/catch` in asynchronous page/modal actions  
- Global toaster for expected and unexpected errors  
- `GlobalErrorBoundary` for render exceptions (current fallback: `null`)  

---

## 8) Authentication and Security

- Auth via Firebase  
- Client-side permission validations for club/superadmin across multiple services  
- Multitenant model via `clubId` in queries  
- Firestore and Storage rules are the primary permission enforcement layer in production  

---

## 9) Scalability

### Strengths

- BaaS reduces backend operational overhead  
- Collection-based model and `clubId` filters scale functionally for early-stage SaaS  

### Current Risks

- Several operations use full `getDocs` reads + client-side filtering/grouping  
- Sequential number generation via counting (`size + 1`) may collide under high concurrency  
- Lack of a dedicated transactional backend for critical logic  

---

## 10) Performance Considerations

- Large single bundle reported by Vite (>500 KiB minified)  
- Extensive use of full collection reads in analytics modules  

### Recommended Improvements

- Route-based code splitting  
- Paginated queries  
- Server-side aggregations  
- Well-defined Firestore indexes  