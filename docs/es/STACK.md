# STACK

## Lenguajes
- **JavaScript (ESM)**: lenguaje principal del frontend y capa de servicios.
- **CSS (Tailwind + utilidades custom)**: estilo y sistema visual.

## Frameworks y librerías principales
- **React 18**: construcción de UI declarativa y componentes.
- **React Router DOM 7**: navegación SPA y segmentación pública/privada.
- **Firebase SDK 12**:
  - Auth para autenticación,
  - Firestore para datos,
  - Storage para archivos/activos.
- **Chart.js + react-chartjs-2**: visualización de métricas.
- **html2canvas**: soporte de captura/descarga de comprobantes visuales.

## Build toolchain
- **Vite 3**: dev server rápido y build optimizado para SPA React.
- **@vitejs/plugin-react**: integración React/Fast Refresh.

## UI y estilos
- **TailwindCSS 3**: utilidades atómicas para productividad y consistencia.
- **PostCSS + Autoprefixer**: compatibilidad CSS cross-browser.
- **Clases de componentes (`.btn-*`) en `index.css`** para normalizar acciones.

## Base de datos
- **Cloud Firestore (NoSQL, documental)**.

Por qué encaja aquí:
- permite iterar rápido en MVP,
- simplifica multi-tenant lógico con `clubId`,
- reduce necesidad de backend propio en fase temprana.

## Infraestructura y despliegue
- **Netlify** con rewrite global a `index.html` para rutas SPA.
- Configuración ligera, adecuada para frontend desacoplado de backend propio.

## DevOps
- Scripts npm básicos: `dev`, `build`, `preview`.

## Testing
- Validación actual principalmente manual y por build de producción.

## Calidad de código
- Validaciones de formularios centralizadas en utilidades.
- Capa de servicios separada de la UI.
- Manejo de errores con toasts y boundary global.

## Versionado
- **Git** con historial activo y merge commits desde ramas feature.
- Evidencia de PRs en mensajes de merge (`#8`, `#9`, `#25`).

## Gestión de dependencias
- **npm** con lockfile (`package-lock.json`) committeado para reproducibilidad.