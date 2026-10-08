# Diagram 1: C4 System Context, Campus Meal Guide

**Type:** C4 Level 1 (System Context) · **Scope:** the whole system as one box, every user role, and every external system.

```mermaid
---
title: "C4 System Context diagram: Campus Meal Guide"
config:
  flowchart:
    wrappingWidth: 340
    nodeSpacing: 55
    rankSpacing: 80
    padding: 18
---
flowchart LR
    student["<b>Student</b><br/><br/>Guest, no login"]:::person
    admin["<b>Admin (Verifier)<br/><br/>Signs in with Google"]:::person

    subgraph boundary["Campus Meal Guide [Software System]"]
        direction LR
        web["<b>Web Application</b><br/>[Container: Next.js]<br/><br/>Serves the mobile-friendly pages and the REST API."]:::container
        db[("<b>Database</b><br/>[Container: PostgreSQL]<br/><br/>Stores meals, stalls, ingredients, reports and choices.")]:::container
    end

    google["<b>Google Identity</b><br/>[External System]<br/><br/>Authenticates admins."]:::external

    student -->|"Browses meals, reports errors<br/>[HTTPS]"| web
    admin -->|"Manages and verifies listings<br/>[HTTPS]"| web
    web -->|"Reads and writes meal data<br/>[SQL over TCP]"| db
    web -->|"Verifies admin sign-in<br/>[OpenID Connect over HTTPS]"| google

    classDef person fill:#08427b,stroke:#052e56,color:#ffffff
    classDef container fill:#438dd5,stroke:#2e6295,color:#ffffff
    classDef external fill:#8a8a8a,stroke:#5e5e5e,color:#ffffff
    style boundary fill:none,stroke:#555,stroke-dasharray:5 5
```

## Key

| Shape / colour | Meaning |
|---|---|
| Dark blue box `[Person]` | A human user role |
| Blue box `[Software System]` | The system we are building |
| Grey box `[External System]` | A system we depend on but do not build |
| Arrow | Direction of the request; the label states the intent |

## Notes

- **Vendors are not drawn.** In the MVP they never touch the system. The Admin collects data from them offline.
- **Audience:** instructor, campus partners, and non-technical stakeholders.
- **Risk reduced:** building for the wrong users, or missing an external dependency.
