# ARCHITECTURE

## 1) Visión general del sistema
Mi Club Manager implementa una arquitectura **frontend-centric** con backend gestionado por Firebase.
El cliente React ejecuta:

- Renderizado de interfaz.
- Navegación.
- Validaciones de formularios.
- Invocación de servicios de dominio.

Firestore actúa como almacenamiento principal y base de datos para:

- usuarios
- clubes 
- estudiantes
- pagos
- grupos
- entrenadores
- inscripciones

---

## 2) Cómo está dividido

### Frontend

Capas principales:

1. **Pages (`src/pages`)**
   Cada pantalla organiza las acciones necesarias para que todo funcione correctamente.

2. **Components (`src/components`)**
   UI reusable, modales, cards, gráficas, pasos de inscripción.

3. **Context (`src/context`)**

   - `AuthContext`: sesión + perfil app (`appUser`).
   - `ToastContext`: feedback global y captura de errores de runtime.

4. **Services (`src/services`)**
   Acceso a Firebase + reglas de negocio de aplicación.

5. **Utils (`src/utils`)**
   Formato, validaciones, fechas y helpers específicos.

---

### Backend (BaaS)

No hay servidor propio en el repositorio. 

Se utiliza:

- **Firebase Auth** para identidad.
- **Cloud Firestore** para persistencia.
- **Firebase Storage** para activos/archivos.

---

### Base de datos (Firestore)

Colecciones por servicios:

- `users`
- `clubs`
- `students`
- `payments`
- `groups`
- `trainers`
- `trainingPlaces`
- `enrollments`
- `tenantPayments` (facturación del SaaS a tenants)

---

### Servicios externos

- Firebase (Auth, Firestore, Storage).
- Netlify (deploy SPA, rewrite a `index.html`).

---

## 3) Patrones arquitectónicos usados

- **Service layer pattern**
   páginas no acceden directo a Firestore; delegan en `*.service.js`.

- **Context-based state sharing**  
   auth/toasts globales sin state manager externo.

- **Route guarding en cliente**
   rutas públicas/privadas y control de superadmin.

- **BFF ausente por diseño**
   lógica de negocio vive en cliente + reglas Firebase.

---

## 4) Flujos de datos principales

### A. Login + provisionamiento

1. Firebase Auth emite `onAuthStateChanged`.
2. `ensureUserAndClub` asegura existencia de `users/{uid}` y `clubs/{clubId}`.
3. El cliente guarda `user` y `appUser`; habilita rutas privadas.

---

### B. Inscripción pública

1. Usuario externo accede `/inscripcion?clubId=...`.
2. Completa wizard multi-step.
3. Se persiste en `enrollments` con estado `pending`.
4. En panel interno, se aprueba/rechaza; aprobación crea `students`.

---

### C. Cobros y vigencias

1. Operador crea pago con fecha base.
2. Servicio genera `receiptNumber`, calcula `validUntil` (+30 días) y guarda pago.
3. Dashboard y listados derivan estado de vigencia desde timestamps.

---

### D. Administración multitenant

1. Superadmin consulta `clubs/users/students` para overview.
2. Puede cambiar plan, suspender, otorgar trial y crear mensualidad.
3. Mensualidades generan comprobantes en `tenantPayments`.

---

## 5) Diagrama explicado en texto

`Browser (React SPA)`
→ `AuthContext / ToastContext`
→ `Pages`
→ `Service Layer`
→ `Firebase SDK`
→ `Firestore/Auth/Storage`

El flujo es síncrono por pantalla en términos de UX, pero asíncrono en red. 

La UI maneja:

- loading/error local por componente
- feedback global por toast.

---

## 6) Manejo de estado

- **Global mínimo**: autenticación y notificaciones.
- **Estado local por página**: filtros, modales, tablas, formularios y loading flags.
- **Sin caché avanzada** ni invalidación centralizada (cada página recarga según necesidad).

---

## 7) Manejo de errores

- try/catch en acciones asíncronas de páginas/modales.
- Toaster global para errores esperados e inesperados.
- `GlobalErrorBoundary` para excepciones de render (fallback actual: `null`).

---

## 8) Autenticación y seguridad

- Auth via Firebase.
- Validaciones de permiso en cliente para club/superadmin en varios servicios.
- Modelo multitenant por `clubId` en queries.
- Las reglas de Firestore y Storage son la capa principal de enforcement de permisos en producción.

---

## 9) Escalabilidad

### Fortalezas:

- BaaS reduce carga operativa de backend.
- Modelo por colecciones y filtros `clubId` escala funcionalmente para SaaS temprano.

### Riesgos actuales:

- varias operaciones usan `getDocs` completos + filtrado/agrupación en cliente,
- generación de consecutivos por conteo (`size + 1`) puede colisionar bajo concurrencia alta,
- ausencia de backend transaccional dedicado para lógica crítica.

---

## 10) Consideraciones de rendimiento

- Bundle único grande reportado por Vite (>500 KiB minificado).
- Uso extensivo de lecturas completas por colección en módulos analíticos.

### Mejoras recomendables

 - code splitting por ruta
 - consultas paginadas
 - agregaciones server-side
 - índices Firestore bien definidos.