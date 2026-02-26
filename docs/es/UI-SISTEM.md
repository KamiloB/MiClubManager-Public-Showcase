# UI-SYSTEM

## 1) Filosofía de diseño inferida
El sistema prioriza **claridad operativa** y **rapidez de ejecución de tareas** sobre ornamentación. Podemos encontrar:
- vistas administrativas utilitarias (tablas, cards, modales),
- landing comercial expresiva para adquisición.

La intención dominante es reducir fricción en operaciones repetitivas (registrar, filtrar, aprobar, cobrar).

## 2) Paleta de colores
Paleta inferida por clases Tailwind predominantes:
- **Primario**: índigo (`indigo-600/500`) para acciones principales.
- **Peligro/cancelación**: rojo (`red-600/500`).
- **Éxito**: verde (`green-600`) en toasts.
- **Advertencia**: amarillo/ámbar (`yellow-500`, `amber-*`).
- **Neutros**: escala de grises (`gray-*`, `slate-*`) para texto y fondos.

Racional: semántica consistente entre acción primaria, riesgo y feedback de estado.

## 3) Tipografía
- Tipografía por defecto del stack web
- Jerarquía basada en peso (`font-bold`, `font-medium`) y tamaño (`text-sm` a `text-6xl` en landing).

## 4) Sistema de espaciado
- Basado en escala utilitaria Tailwind (`p-4`, `p-6`, `gap-2/3/4/6`, `mb-4/6`, etc.).
- Layout centrado con contenedores `max-w-*` en landing y formularios.
- Buena separación vertical entre bloques funcionales.

## 5) Componentes principales
- Botones normalizados (`btn`, `btn-primary`, `btn-danger`, `btn-outline`, `btn-icon`).
- Modales reutilizables para creación/edición y comprobantes.
- Cards para estudiantes, grupos y entrenadores.
- Tablas para módulos administrativos de alto volumen.
- Formularios wizard para inscripción.
- Toasts globales para feedback no intrusivo.
- Gráficas para métricas de negocio.

## 6) Estados de UI
- **Hover**: variantes visuales en botones, links y cards.
- **Loading**: textos de carga y skeletons en algunos listados.
- **Error**:
  - mensajes toast,
  - warning boxes en contexto,
  - boundary global para errores de render.
- **Empty**: mensajes explícitos (“No hay ... para mostrar”).
- **Disabled**: botones desactivados en acciones no disponibles o dependencias no cumplidas.

## 7) Accesibilidad
Se Utilizaron prácticas como:
- uso de `aria-label`, `aria-expanded`, `aria-controls` en navegación móvil,
- foco visual en botones con `focus:ring`,
- textos de estado legibles.

## 8) Consistencia visual
La consistencia es alta en zona admin por:
- clases utilitarias repetibles,
- sistema de botones compartido,
- estructura recurrente (header + controles + contenido + modal).

La landing usa más gradientes e ilustración, pero mantiene coherencia de branding con primario índigo.

## 9) Principios UX aplicados
- **Feedback inmediato**: toasts y mensajes de confirmación.
- **Prevención de errores**: validaciones por paso y límites de plan.
- **Progressive disclosure**: wizard multi-step y acciones avanzadas en modales.
- **Responsive-first pragmático**: sidebar móvil, tablas con overflow, ajustes de calendario.
- **Orientación a tarea**: acciones CTA claras en cada módulo.