# Diagram 6: UML Class, Campus Meal Guide domain model

**Type:** UML class · **Scope:** 8 domain classes with typed attributes, multiplicity at both ends of every association, and an enumeration for each status field.

```mermaid
---
title: "UML Class diagram: Campus Meal Guide domain model"
---
classDiagram
    direction TB

    class Stall {
        +int id
        +String name
        +String location
    }
    class Meal {
        +int id
        +String name
        +Decimal price
        +MealStatus status
        +DateTime verifiedAt
        +DateTime createdAt
        +DateTime updatedAt
    }
    class Ingredient {
        +int id
        +String name
    }
    class Allergen {
        +int id
        +String name
    }
    class DietaryRestriction {
        +int id
        +String name
    }
    class Admin {
        +int id
        +String email
        +String googleId
        +String displayName
    }
    class MealReport {
        +int id
        +String description
        +ReportStatus status
        +DateTime createdAt
        +DateTime resolvedAt
    }
    class MealChoice {
        +int id
        +String sessionHash
        +DateTime chosenAt
    }
    class MealStatus {
        DRAFT
        VERIFIED
        FLAGGED
        ARCHIVED
    }
    class ReportStatus {
        OPEN
        RESOLVED
        DISMISSED
    }

    Stall "1" -- "0..*" Meal : sells
    Meal "0..*" -- "1..*" Ingredient : lists
    Meal "0..*" -- "0..*" Allergen : declares
    Meal "0..*" -- "0..*" DietaryRestriction : suits
    Admin "0..1" -- "0..*" Meal : verifies
    Meal "1" -- "0..*" MealReport : receives
    Admin "0..1" -- "0..*" MealReport : reviews
    Meal "1" -- "0..*" MealChoice : records

    Meal ..> MealStatus : status
    MealReport ..> ReportStatus : status
```

## Key

| Symbol | Meaning |
|---|---|
| Box with three parts | Class: name, then attributes (`+` = public, then type and name) |
| `<<enumeration>>` | A fixed set of allowed values |
| Plain line with a verb | Association; the multiplicity at each end counts the objects on **that** side |
| Dotted arrow | The attribute uses that enumeration as its type |

## Design decisions

- `Stall` is the class that holds vendor data. "Vendor" is only a data source in the MVP, not an actor.
- The `allergen-free` filter (M2) is handled as "exclude meals that declare allergen X", so allergens are a class of their own.
- `Meal.verifiedAt` and the `verifies` association give the "verified badge and date" in M4.
- `MealStatus` matches the state machine exactly. `ReportStatus` is the second status field.
- **Audience:** developers and database designers.
- **Risk reduced:** a wrong domain model, such as missing allergens or an unrecorded verifier.
