# DECISIONS

## 1) Decisiones arquitectónicas clave

### 1.1 Firebase como backend gestionado
**Decisión:** usar Auth + Firestore (+ Storage) sin backend propio en el repo.

**Motivo inferido:** acelerar entrega del MVP y reducir carga operativa de infraestructura.

**Trade-off:**
- ✅ alta velocidad de implementación,
- ❌ lógica crítica y seguridad dependen de configuración externa (reglas/funciones no visibles aquí).

### 1.2 Multitenancy lógico por `clubId`
**Decisión:** modelar aislamiento de datos vía campo `clubId` en colecciones de dominio.

**Trade-off:**
- ✅ simple y funcional para etapa temprana,
- ❌ requiere disciplina estricta de queries/reglas para evitar fugas entre tenants.

### 1.3 Capa de servicios separada de páginas
**Decisión:** encapsular Firestore en `src/services/*`.

**Trade-off:**
- ✅ mejor mantenibilidad y centralización de reglas,
- ❌ sin tipado estático, errores de contrato pueden emerger en runtime.

---

## 2) Decisiones de producto/negocio visibles en código

### 2.1 Estrategia Free/Pro embebida en flujos
**Decisión:** aplicar límites Free (estudiantes/pagos) directamente en operaciones.

**Impacto:**
- ✅ soporta monetización desde el producto,
- ❌ convierte limitaciones en palanca de upgrade.

### 2.2 Superadmin multitenant
**Decisión:** incluir módulo de administración SaaS (planes, trial, suspensión, mensualidades).

**Impacto:**
- ✅ habilita operación comercial B2B desde la misma app,
- ❌ incrementa complejidad funcional y de permisos.

---

## 3) Decisiones de performance

### 3.1 Consultas agregadas en cliente
**Decisión:** cargar colecciones y derivar métricas en frontend.

**Trade-off:**
- ✅ implementación rápida,
- ❌ mayor latencia/costo en datasets grandes.

### 3.2 Numeración de comprobantes por conteo
**Decisión:** consecutivos `size + 1` por año/club.

**Trade-off:**
- ✅ simple de implementar,
- ❌ riesgo de colisiones en concurrencia alta.

---

## 4) Decisiones de diseño UI

### 4.1 Tailwind + clases de botones compartidas
**Decisión:** utilidades atómicas con pequeña capa de componentes CSS (`btn-*`).

**Trade-off:**
- ✅ velocidad y consistencia visual,
- ❌ posible verbosidad en JSX y deuda de diseño tokenizado formal.

### 4.2 Landing comercial separada del panel operativo
**Decisión:** coexistencia de UX de marketing y UX administrativa en la misma SPA.

**Trade-off:**
- ✅ onboarding y conversión en un mismo producto,
- ❌ bundle más pesado si no hay code splitting por ruta.

---

## 5) Decisiones de estructura del proyecto
- Organización por tipo (`pages`, `components`, `services`, `utils`, `context`).
- Favor de simplicidad para equipo pequeño.
- Escalable a mediano plazo, aunque con crecimiento conviene evolucionar a módulos por dominio.

---

## 6) Decisiones de seguridad

### Evidencia
- Guardas de rutas por estado de auth y rol.
- Verificaciones de permisos en algunos servicios (`ensureClubAccess`, `requireSuperAdmin`).

### Observaciones
- Este documento omite valores sensibles deliberadamente para publicación.

---

## 7) Decisiones de escalabilidad
- Se privilegió rapidez de producto sobre sofisticación de backend.
- La arquitectura actual es apropiada para MVP activo, pero requerirá:
  - operaciones transaccionales para facturación/consecutivos,
  - agregaciones server-side,
  - observabilidad y testing automatizado.

---

## 8) Alternativas descartadas

- Backend propio (Node) + DB SQL: descartado por velocidad en el desarrollo del producto y flexibilidad en la db.
- State manager global (Redux/Zustand): evitado en favor de estado local + contexts mínimos.
- Diseño sin multitenancy: abandonado al crecer el proyecto y ver la necesidad de introducir superadmin y gestión de tenants.