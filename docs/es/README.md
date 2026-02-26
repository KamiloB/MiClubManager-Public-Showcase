# Mi Club Manager

Mi Club Manager es una plataforma web **SaaS multitenant** diseñada para la gestión operativa de clubes deportivos, enfocada en academias pequeñas y medianas que actualmente administran alumnos, pagos y grupos de forma manual.

---

## 🚀 ¿Qué es Mi Club Manager?

Es una aplicación web construida con **React + Firebase** que centraliza en un solo lugar:

- Gestión de estudiantes
- Control de mensualidades
- Organización de entrenadores y sedes de entrenamiento
- Inscripciones públicas online
- Dashboard operativo con métricas clave
- Administración de múltiples clubes (modo superadmin)

---

## ❌ Problema que resuelve

Muchos clubes deportivos dependen de hojas de cálculo, WhatsApp y controles manuales para llevar su operación. Esto genera:

- 💸 Pérdida de ingresos por pagos vencidos no detectados
- 📉 Falta de control de vigencias de estudiantes
- 📋 Procesos manuales y desorganizados de inscripción
- 🔍 Baja trazabilidad administrativa
- 📈 Dificultad para escalar cuando el club crece

---

## 🎯 ¿Para quién está hecho?

- Dueños de academias deportivas
- Entrenadores que gestionan su propio club
- Administradores de múltiples sedes o disciplinas
- Ligas o entidades deportivas con varios clubes

---

## 💡 Propuesta de valor

- **Todo en un solo lugar:** alumnos, pagos, sedes, entrenadores e inscripciones
- **Flujo de cobros claro:** control de vigencias y estado de pagos en tiempo real
- **Inscripción digital:** elimina papel y procesos manuales
- **Onboarding rápido:** registro automático de club listo para usar
- **Escalable:** modelo multitenant para múltiples clubes
- **Enfoque práctico:** interfaz diseñada para el día a día del entrenador

---

## 🧠 Estado del proyecto

**MVP avanzado / pre-producción funcional**

- Funcionalidad end-to-end para casos principales
- Producto actualmente usable por clubes reales
- Iteración activa basada en feedback de usuarios
- En proceso de expansión comercial

---

## ⚙️ Funcionalidades principales

### 🔐 Autenticación
- Registro de cuenta + creación automática de club
- Inicio de sesión con Firebase Auth
- Roles de usuario (owner / superadmin)

### 📊 Dashboard
- Estudiantes activos
- Estudiantes al día
- Pagos vencidos
- Ingresos últimos 30 días

### 👨‍🎓 Estudiantes
- CRUD completo
- Ficha personal, médica y de acudientes
- Foto de perfil
- Importación masiva desde Excel
- Exportación de datos

### 🏟️ Sedes y entrenadores
- Gestión de lugares de entrenamiento
- Gestión de entrenadores
- Activación/desactivación
- Relación entre sedes y entrenadores

### 💰 Pagos
- Registro de mensualidades
- Cálculo automático de vigencia
- Historial de pagos por estudiante
- Comprobante descargable
- Envío por WhatsApp

### 📝 Inscripciones
- Formulario público multi-step
- Validación por pasos
- Flujo de aprobación o rechazo
- Creación automática de estudiante aprobado

### 🧾 Administración SaaS (Superadmin)
- Gestión de clubes
- Plan Free / Pro
- Activación de trial
- Cobros de suscripción
- Historial de pagos de tenants

---

## 🧱 Arquitectura

- **Frontend:** React + React Router
- **Backend:** Firebase (Auth, Firestore, Storage)
- **Modelo:** multitenant por `clubId`
- **Service Layer:** capa intermedia para lógica de negocio
- **React SPA → Services → Firebase SDK → Firestore/Auth/Storage:**

---

## 📚 Documentación técnica

- [📐 Arquitectura](./ARCHITECTURE.md)
- [🧩 Funcionalidades](./FEATURES.md)
- [🧠 Decisiones técnicas](./DECISIONS.md)
- [🎨 UI System](./UI-SYSTEM.md)
- [🛠️ Stack técnico](./STACK.md)
- [💾 Modelo de datos](./DATA-MODEL.md)

---

## 🧪 Roadmap

- 🤾‍♂️ Integracion total multideporte
- 🔔 Notificaciones push (pagos / cumpleaños)
- 📅 Módulo de eventos deportivos
- 📊 Analítica avanzada
- 🧑‍💼 Roles multiusuario por club
- 📱 Integraciones externas (ligas, federaciones)

🧑‍💻 Autor

Desarrollado por Kamilo Blandon  
Desarrollador Web  
Profesor de Taekwondo

**“Lo que necesitaba para mi club… ahora se lo estoy dando a todos.”**

📄 Licencia

Proyecto privado — todos los derechos reservados.