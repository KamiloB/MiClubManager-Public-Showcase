# UI-SYSTEM

## 1) Inferred design philosophy
The system prioritizes **operational clarity** and **task execution speed** over ornamentation. You will find:
- utilitarian administrative views (tables, cards, modals),
- expressive commercial landing page for acquisition.

The dominant intention is to reduce friction in repetitive operations (register, filter, approve, charge).

## 2) Color Palette
Palette inferred from predominant Tailwind classes:
- **Primary**: indigo (`indigo-600/500`) for main actions.
- **Danger/cancel**: red (`red-600/500`).
- **Success**: green (`green-600`) in toasts.
- **Warning**: yellow/amber (`yellow-500`, `amber-*`).
- **Neutrals**: gray scale (`gray-*`, `slate-*`) for text and backgrounds.

Rationale: consistent semantics between primary action, risk, and state feedback.

## 3) Typography
- Default font from the web stack
- Hierarchy based on weight (`font-bold`, `font-medium`) and size (`text-sm` to `text-6xl` on landing).

## 4) Spacing System
- Based on Tailwind utility scale (`p-4`, `p-6`, `gap-2/3/4/6`, `mb-4/6`, etc.).
- Centered layout with `max-w-*` containers in landing and forms.
- Good vertical separation between functional blocks.

## 5) Main Components
- Normalized buttons (`btn`, `btn-primary`, `btn-danger`, `btn-outline`, `btn-icon`).
- Reusable modals for creation/editing and receipts.
- Cards for students, groups, and trainers.
- Tables for high-volume administrative modules.
- Wizard forms for registration.
- Global toasts for non-intrusive feedback.
- Graphs for business metrics.

## 6) UI States
- **Hover**: visual variants on buttons, links, and cards.
- **Loading**: loading texts and skeletons in some listings.
- **Error**:
  - toast messages,
  - warning boxes in context,
  - global boundary for render errors.
- **Empty**: explicit messages (“No data to display”).
- **Disabled**: disabled buttons on unavailable actions or unmet dependencies.

## 7) Accessibility
Practices used include:
- use of `aria-label`, `aria-expanded`, `aria-controls` in mobile navigation,
- visual focus on buttons with `focus:ring`,
- legible status texts.

## 8) Visual Consistency
Consistency is high in the admin area due to:
- repeatable utility classes,
- shared button system,
- recurring structure (header + controls + content + modal).

The landing uses more gradients and illustrations but maintains brand consistency with primary indigo.

## 9) Applied UX Principles
- **Immediate feedback**: toasts and confirmation messages.
- **Error prevention**: step-by-step validations and plan limits.
- **Progressive disclosure**: multi-step wizard and advanced actions in modals.
- **Pragmatic responsive-first**: mobile sidebar, tables with overflow, calendar adjustments.
- **Task-oriented**: clear CTA actions in each module.