# Diagram 2: C4 Container, Campus Meal Guide

**Type:** C4 Level 2 (Container) · **Scope:** every separately deployable unit, its technology, and how the units talk.

```mermaid
---
title: "C4 Container diagram: Campus Meal Guide"
config:
  flowchart:
    wrappingWidth: 340
    nodeSpacing: 55
    rankSpacing: 80
    padding: 18
---
flowchart LR
    student["<b>Student</b><br/><br/>Guest, no login"]:::person
    admin["<b>Admin (Verifier)</b><br/><br/>Signs in with Google"]:::person

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

**Next.js is drawn as one container:** the pages and the REST API are built and deployed together as a single web application.

## Key

| Shape / colour | Meaning |
|---|---|
| Dark blue box | Person |
| Light blue box `[Container: technology]` | A separately deployable unit and its technology |
| Cylinder | Data store |
| Grey box | External system |
| Dashed frame | System boundary |
| Arrow label | Intent, then `[protocol]` |

## Notes

- **Audience:** developers and whoever will deploy the system.
- **Risk reduced:** hidden deployable units, and unclear technology or protocol choices.
