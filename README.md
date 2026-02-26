# Mi Club Manager

Plataforma SaaS multitenant para la gestión operativa de clubes deportivos.

Mi Club Manager permite a academias y entrenadores centralizar alumnos, pagos, entrenamientos e inscripciones en un solo sistema, eliminando procesos manuales y desordenados.

---

## 🚀 Public Showcase

Este repositorio está estructurado como un **public showcase** del proyecto.

Incluye:

- Documentación técnica
- Arquitectura
- Decisiones de ingeniería
- Sistema de UI
- Modelo de datos
- Seguridad
- Stack tecnológico

Todo organizado en **dos idiomas**:

📁 `/docs/es` → documentación en español  
📁 `/docs/en` → documentation in english

---

## 📚 Documentación

### 🇪🇸 Español

- [Readme](./docs/es/README.md)
- [Arquitectura](./docs/es/ARCHITECTURE.md)
- [Modelo de datos](./docs/es/DATAMODEL.md)
- [Funcionalidades](./docs/es/FEATURES.md)
- [Stack tecnológico](./docs/es/STACK.md)
- [Sistema de UI](./docs/es/UI-SYSTEM.md)
- [Decisiones técnicas](./docs/es/DECISIONS.md)

---

### 🇺🇸 English

- [Readme](./docs/en/README.md)
- [Architecture](./docs/en/ARCHITECTURE.md)
- [Data Model](./docs/en/DATAMODEL.md)
- [Features](./docs/en/FEATURES.md)
- [Tech Stack](./docs/en/STACK.md)
- [UI System](./docs/en/UI-SYSTEM.md)
- [Technical Decisions](./docs/en/DECISIONS.md)

---

## 🧩 ¿Qué incluye el sistema?

Mi Club Manager actualmente permite:

- Gestión de estudiantes
- Control de pagos y mensualidades
- Organización de entrenadores y sedes
- Inscripciones públicas online
- Dashboard de métricas operativas
- Administración multitenant (modo SaaS)

---

## 🏗 Arquitectura general

- Frontend SPA en React
- Backend BaaS con Firebase:
  - Authentication
  - Firestore
  - Storage
- Modelo multitenant basado en `clubId`
- Service layer para acceso a datos

Para más detalles ver:
👉 `docs/es/ARCHITECTURE.md`

---

## 🎯 Estado del proyecto

Actualmente el sistema se encuentra en:

**MVP avanzado listo para clientes reales**

- Flujo end-to-end funcional
- Primeros usuarios en adquisición
- Iteración continua sobre feedback real

---

## 🌎 Objetivo del producto

Convertirse en el sistema operativo de gestión para clubes deportivos en LATAM:

- desde academias pequeñas
- hasta ligas y organizaciones deportivas completas

---

## 🧑‍💻 Autor

**Kamilo Blandon**  
Software Developer & Taekwondo Coach

> “Estoy construyendo el sistema que yo mismo necesitaba como entrenador.”

---

## 📬 Contacto

📧 Email: [Kamilob1224@gmail.com](mailto:kamilob1224@gmail.com?subject=Work%20Opportunity&body=Hi%20Kamilo,%20I%20saw%20your%20portfolio...)  
📸 Instagram: [@Kamilo_Blandon](www.instagram.com/Kamilo_Blandon)  
🌐 Web: [Mi Club Manager](https://www.miclubmanager.com)  

---

## 🧠 Nota

Este repositorio no incluye variables sensibles ni configuración privada de Firebase.

La seguridad del sistema se basa en reglas de Firestore y Storage configuradas en el proyecto productivo.
