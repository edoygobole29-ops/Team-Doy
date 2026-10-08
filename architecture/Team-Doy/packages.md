# Diagram 8: UML Package, source folder structure

**Type:** UML package · **Scope:** the folders under `src/` that the team will create, and the allowed dependencies between them. The folder names are **proposed** and should be confirmed by the team.

```mermaid
---
title: "UML Package diagram: Source folder structure and allowed dependencies"
config:
  flowchart:
    wrappingWidth: 340
    nodeSpacing: 55
    rankSpacing: 80
    padding: 18
---
flowchart TB
    subgraph src["UML Package diagram: src/ folder structure and allowed dependencies"]
        direction TB
        subgraph ui["User interface"]
            direction LR
            guide["<b>app/(guide)</b><br/>Student pages"]
            adminp["<b>app/admin</b><br/>Admin pages"]
            comps["<b>components</b><br/>Shared UI parts"]
        end
        api["<b>app/api</b><br/>REST route handlers"]
        services["<b>services</b><br/>Business rules"]
        subgraph access["Data and external access"]
            direction LR
            repos["<b>repositories</b><br/>Database queries"]
            adapters["<b>adapters</b><br/>Google Identity adapter"]
        end
        db["<b>db</b><br/>Connection, schema, migrations"]
        models["<b>models</b><br/>Domain types and enums<br/>(shared by every package)"]
    end

    guide --> comps
    adminp --> comps
    guide -.->|"HTTP calls"| api
    adminp -.->|"HTTP calls"| api
    api --> services
    services --> repos
    services --> adapters
    repos --> db

    style src fill:none,stroke:#555
    style ui fill:#eef4ff,stroke:#4a6fa5
    style access fill:#f3f3f3,stroke:#777
    style models fill:#fff3cd,stroke:#8a6d00,color:#222
```

**Layering rule:** each package may only import from the layer directly below it, so pages never import services or the database layer directly, and the shared `models` package imports nothing.

## Key

| Symbol | Meaning |
|---|---|
| Box | A folder (package) |
| Solid arrow | "Imports from" (compile-time dependency) |
| Dotted arrow | Runtime call over HTTP only, with no import |
| Yellow box | Shared package that every layer may use |

- **Audience:** developers.
- **Risk reduced:** tangled code, such as pages that query the database directly.
