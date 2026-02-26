# FEATURES

## 1) Módulo de autenticación y acceso
- Inicio de sesión con Firebase Auth.
- Registro de owner con creación/recuperación automática de club.
- Enriquecimiento de perfil app (`users`) con rol y `clubId`.
- Ruteo privado para operación interna y ruta exclusiva superadmin.

## 2) Landing pública (marketing)
- Página principal comercial con:
  - hero orientado a valor.
  - lista de problemas/soluciones.
  - pricing y CTA.
  - carrusel de screenshots.
  - enlaces a términos y política de datos.

## 3) Dashboard operativo
- KPIs principales por club:
  - estudiantes activos,
  - estudiantes al día,
  - ingresos últimos 30 días,
  - cantidad de pagos vencidos.
- Gráfica de ingresos.
- Segmentación por estado de pago.

## 4) Gestión de estudiantes
- Listado con búsqueda y orden.
- Alta manual de estudiante.
- Edición de ficha completa (datos personales, salud, acudientes, entrenamiento).
- Activar/desactivar estudiante.
- Asignación a grupo.
- Exportación de datos.
- Importación masiva desde Excel con validación de campos, duplicados y normalización en campos.
- Restricción por plan Free (límite de estudiantes).

## 5) Gestión de grupos
- CRUD de grupos con horario, disciplina, color, entrenadores y notas.
- Estado activo/inactivo.
- Visibilidad en formulario público (`visibleInForm`).
- Vista calendario de grupos.
- Sincronización con `trainingPlaces` para estado y visibilidad.

## 6) Gestión de entrenadores
- Crear/editar entrenador.
- Búsqueda y orden.
- Activar/desactivar.
- Asociación de entrenadores a grupos.

## 7) Pagos de estudiantes
- Registro de mensualidades con método de pago.
- Cálculo de vigencia automática (+30 días).
- Numeración de comprobante por año y club.
- Historial de pagos.
- Estados de vigencia (activos/vencidos).
- Comprobante imprimible/descargable y envío automático a whatsapp con mensaje preestablecido y descarga automática de la imagen.
- Restricción de cantidad de pagos en plan Free.

## 8) Inscripciones
### Flujo público
- Formulario multi-step:
  1. entrenamiento,
  2. alumno,
  3. salud,
  4. acudientes,
  5. reglamento/consentimiento.
- Validación progresiva por paso.
- Registro `pending` en `enrollments`.

### Flujo interno
- Listado de inscripciones.
- Aprobación (crea estudiante y marca enrollment `approved`).
- Rechazo (`rejected`).

## 9) Configuración
- Ajustes del club (nombre, branding, datos de contacto según vista disponible).
- Generación/uso de enlace de inscripción pública por `clubId`.

## 10) Administración de tenants (superadmin)
- Vista consolidada de tenants con plan, estado y volumen de estudiantes.
- Cambio de plan Free/Pro.
- Suspensión de tenant.
- Activación de trial Pro.
- Registro de mensualidades de suscripción.
- Historial de cobros de tenant + comprobantes.

## Flujos de usuario
- **Owner nuevo**: register → auto-creación de club → dashboard.
- **Owner operativo**: estudiantes/grupos/entrenadores → pagos → seguimiento en dashboard.
- **Aspirante**: formulario público → inscripción pendiente.
- **Admin interno**: revisión de inscripciones → aprobación/rechazo.
- **Superadmin SaaS**: panel tenants → planes/pruebas/cobros.

## Permisos
- No autenticado: landing + inscripción pública + términos/política.
- Usuario autenticado: módulos internos del club.
- Superadmin: acceso adicional a `/admin/tenants`.

## Casos edge relevantes
- Club inexistente o enlace sin `clubId` en inscripción pública.
- Límite plan Free alcanzado para estudiantes/pagos.
- Duplicados en importación masiva por documento.
- Entidades relacionadas inactivas (grupos/entrenadores ocultos en formulario público).

## Limitaciones actuales
- Sin backend propio para operaciones críticas concurrentes.

## Funcionalidades futuras inferidas
- Módulo “Eventos” ya funcional para crear eventos directamente en la plataforma y gestionar deportistas, pirámides y tickets de competencia.
- Mayor robustez de analítica y facturación multitenant.
- Endurecimiento de seguridad server-side (reglas/funciones).