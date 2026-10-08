# Diagram 3: UML Use Case, Campus Meal Guide

**Type:** UML use case · **Scope:** every actor from the context diagram and 9 goal-level use cases from the Must-have features.

```mermaid
---
title: "UML Use Case diagram: Campus Meal Guide (MVP)"
config:
  flowchart:
    wrappingWidth: 340
    nodeSpacing: 35
    rankSpacing: 120
    padding: 18
---
flowchart LR

    student["Student / Guest"]

    subgraph sys["Campus Meal Guide"]
        direction LR

        uc1(["Browse Meals"])
        uc2(["Filter Meals"])
        uc3(["View Meal Details"])
        uc4(["Report Meal Issue"])
        uc5(["Record Meal Choice"])
        uc6(["Sign In"])
        uc7(["Manage Meal Listing"])
        uc8(["Verify Meal Listing"])
        uc9(["Review Reports"])
    end

    admin["Admin / Verifier"]
    google["Google Identity"]

    student ---> uc1
    student ---> uc2
    student ---> uc3
    student ---> uc4
    student ---> uc5

    admin --> uc7
    admin --> uc8
    admin --> uc9

    uc7 -.->|"include"| uc6
    uc8 -.->|"include"| uc6
    uc9 -.->|"include"| uc6

    uc6 ---> google

    classDef actor fill:#fff3cd,stroke:#8a6d00,color:#222
    classDef ext fill:#e2e2e2,stroke:#666,color:#222

    class student,admin actor
    class google ext

    style sys fill:none,stroke:#555
```

## Key

| Symbol | Meaning |
|---|---|
| Yellow box | Primary actor (a person) |
| Grey box | Secondary actor (an external system) |
| Rounded shape inside the frame | A use case (verb + object) |
| Solid line | Actor takes part in the use case |
| Dashed arrow `«include»` | The use case always includes the target use case |
| Large frame | System boundary |

## Traceability to the MVP feature list

| Use case | Must-have feature |
|---|---|
| Browse Meals | M1 |
| Filter Meals | M2 and M3 (one use case; diet and price are used together) |
| View Meal Details | M4 |
| Report Meal Issue | M5 |
| Record Meal Choice | M9 |
| Sign In | M6 |
| Manage Meal Listing | M7 (add, edit, archive) |
| Verify Meal Listing | M8 |
| Review Reports | M8 |

- The Admin reaches **Sign In** through the `«include»` of each admin use case, so every admin goal requires a Google sign-in first.
- **Audience:** the team and the instructor.
- **Risk reduced:** building features no actor needs, or missing a Must-have.
