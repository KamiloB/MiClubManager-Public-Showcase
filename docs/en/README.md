# Mi Club Manager

Mi Club Manager is a **multitenant SaaS** web platform designed for the operational management of sports clubs, focused on small and medium academies that currently manage students, payments, and groups manually.

---

## 🚀 What is Mi Club Manager?

It’s a web application built with **React + Firebase** that centralizes in one place:

- Student management
- Monthly fee control
- Trainer and training location organization
- Online public registrations
- Operational dashboard with key metrics
- Multi-club administration (superadmin mode)

---

## ❌ The Problem It Solves

Many sports clubs rely on spreadsheets, WhatsApp, and manual controls to manage their operations. This leads to:

- 💸 Loss of revenue from overdue payments not detected
- 📉 Lack of control over student status
- 📋 Manual and disorganized enrollment processes
- 🔍 Low administrative traceability
- 📈 Difficulty scaling as the club grows

---

## 🎯 Who is it for?

- Sports academy owners
- Trainers managing their own club
- Administrators of multiple locations or disciplines
- Leagues or sports organizations with multiple clubs

---

## 💡 Value Proposition

- **Everything in one place:** students, payments, locations, trainers, and registrations
- **Clear payment flow:** real-time payment status and validity control
- **Digital enrollment:** eliminates paper and manual processes
- **Fast onboarding:** automatic club registration ready to use
- **Scalable:** multitenant model for multiple clubs
- **Practical focus:** interface designed for trainers' everyday use

---

## 🧠 Project Status

**Advanced MVP / Functional pre-production**

- End-to-end functionality for main cases
- Product currently usable by real clubs
- Active iteration based on user feedback
- In the process of commercial expansion

---

## ⚙️ Main Features

### 🔐 Authentication
- Account registration + automatic club creation
- Login with Firebase Auth
- User roles (owner / superadmin)

### 📊 Dashboard
- Active students
- Students up to date
- Overdue payments
- Income in the last 30 days

### 👨‍🎓 Students
- Full CRUD
- Personal, health, and guardian information
- Profile photo
- Bulk import from Excel
- Data export

### 🏟️ Training Locations and Trainers
- Management of training locations
- Trainer management
- Activation/deactivation
- Relationship between locations and trainers

### 💰 Payments
- Monthly fee registration
- Automatic validity calculation
- Payment history per student
- Downloadable receipt
- WhatsApp sending

### 📝 Enrollments
- Multi-step public form
- Step-by-step validation
- Approval or rejection flow
- Automatic creation of approved student

### 🧾 SaaS Administration (Superadmin)
- Club management
- Free / Pro plan
- Trial activation
- Subscription billing
- Tenant payment history

---

## 🧱 Architecture

- **Frontend:** React + React Router
- **Backend:** Firebase (Auth, Firestore, Storage)
- **Model:** multitenant by `clubId`
- **Service Layer:** intermediate layer for business logic
- **React SPA → Services → Firebase SDK → Firestore/Auth/Storage:**

---

## 📚 Technical Documentation

- [📐 Architecture](./ARCHITECTURE.md)
- [🧩 Features](./FEATURES.md)
- [🧠 Technical Decisions](./DECISIONS.md)
- [🎨 UI System](./UI-SYSTEM.md)
- [🛠️ Tech Stack](./STACK.md)
- [💾 Data Models](./DATA-MODEL.md)

---

## 🧪 Roadmap

- 🤾‍♂️ Full integration of multiple sports
- 🔔 Push notifications (payments / birthdays)
- 📅 Sports event module
- 📊 Advanced analytics
- 🧑‍💼 Multi-user roles per club
- 📱 External integrations (leagues, federations)

🧑‍💻 Author

Developed by Kamilo Blandon  
Web Developer  
Taekwondo Instructor

**“What I needed for my club… now I’m giving it to everyone.”**

📄 License

Private project — all rights reserved.