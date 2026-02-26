# STACK

## Languages
- **JavaScript (ESM)**: main language for frontend and service layer.
- **CSS (Tailwind + custom utilities)**: styling and visual system.

## Main frameworks and libraries
- **React 18**: declarative UI building and components.
- **React Router DOM 7**: SPA navigation and public/private segmentation.
- **Firebase SDK 12**:
  - Auth for authentication,
  - Firestore for data,
  - Storage for files/assets.
- **Chart.js + react-chartjs-2**: metrics visualization.
- **html2canvas**: support for visual receipt capture/download.

## Build toolchain
- **Vite 3**: fast dev server and optimized build for SPA React.
- **@vitejs/plugin-react**: React/Fast Refresh integration.

## UI and styles
- **TailwindCSS 3**: atomic utilities for productivity and consistency.
- **PostCSS + Autoprefixer**: cross-browser CSS compatibility.
- **Component classes (`.btn-*`) in `index.css`** to normalize actions.

## Database
- **Cloud Firestore (NoSQL, document-based)**.

Why it fits here:
- allows for rapid MVP iteration,
- simplifies logical multi-tenancy with `clubId`,
- reduces the need for a backend in early stages.

## Infrastructure and deployment
- **Netlify** with global rewrite to `index.html` for SPA routes.
- Lightweight setup, suitable for frontend decoupled from its own backend.

## DevOps
- Basic npm scripts: `dev`, `build`, `preview`.

## Testing
- Current validation is primarily manual and through production builds.

## Code quality
- Centralized form validations in utilities.
- Service layer separated from the UI.
- Error handling with toasts and global error boundaries.

## Versioning
- **Git** with active history and merge commits from feature branches.
- PR evidence in merge messages (`#8`, `#9`, `#25`).

## Dependency management
- **npm** with lockfile (`package-lock.json`) committed for reproducibility.