# Diagram 10: UML Deployment (Provisional)

**Type:** UML deployment · **Scope:** nodes, execution environments, and artifacts for the MVP. **Provisional:** the hosting provider is not chosen yet, so generic node names are used.

```mermaid
---
title: "UML Deployment diagram (PROVISIONAL): Campus Meal Guide"
config:
  flowchart:
    wrappingWidth: 340
    nodeSpacing: 55
    rankSpacing: 80
    padding: 18
---
flowchart LR
    subgraph sdev["Student's phone or computer"]
        direction TB
        subgraph senv["Web browser"]
            sart["Rendered guide pages"]
        end
    end

    subgraph adev["Admin's computer"]
        direction TB
        subgraph aenv["Web browser"]
            aart["Rendered admin pages"]
        end
    end

    subgraph apph["Application host<br/>(provider to be chosen)"]
        direction TB
        subgraph appenv["Render"]
            appart["Campus Meal Guide web app<br/>(Render)"]
        end
    end

    subgraph dbh["Database host <br/>(provider to be chosen)"]
        direction TB
        subgraph dbenv["PostgreSQL server"]
            dbart["Meal Guide database<br/>(schema and data)"]
        end
    end

    google["«external system»<br/>Google Identity"]

    sart -->|"HTTPS"| appart
    aart -->|"HTTPS"| appart
    aart -->|"HTTPS (sign-in redirect)"| google
    appart -->|"SQL over TCP"| dbart
    appart -->|"OpenID Connect over HTTPS"| google

    style senv fill:#fffbe6,stroke:#b59b00
    style aenv fill:#fffbe6,stroke:#b59b00
    style appenv fill:#fffbe6,stroke:#b59b00
    style dbenv fill:#fffbe6,stroke:#b59b00
    style sdev fill:#effaf0,stroke:#4f8a55
    style adev fill:#eef4ff,stroke:#4a6fa5
    style apph fill:#f3f3f3,stroke:#777
    style dbh fill:#f3f3f3,stroke:#777
    style google fill:#e2e2e2,stroke:#666,color:#111
```

## Key

| Symbol | Meaning |
|---|---|
| Large box `«device»` or `«node»` | Physical or virtual machine |
| Box inside a node `«execution environment»` | Software that runs the artifact |
| Innermost box `«artifact»` | The file or build that is deployed |
| Labelled arrow | Communication path and its protocol |

## Notes

- This diagram contains no secrets, credentials, or real addresses. Hosting details are filled in once the provider is chosen.
- **Audience:** whoever deploys the system, and the instructor.
- **Risk reduced:** late hosting surprises, such as an exposed database or an unplanned dependency.
