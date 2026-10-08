# Diagram 11: Entity Relationship Diagram 
**Type:** ERD, crow's-foot notation · **Scope:** one table per stored class, plus join tables for the many-to-many associations. It is derived from `class.md`.

```mermaid
---
title: "ERD : Campus Meal Guide database"
---
erDiagram
    stalls {
        int id PK
        varchar name
        varchar location
    }
    meals {
        int id PK
        int stall_id FK
        int verified_by FK "nullable"
        varchar name
        decimal price
        varchar status "MealStatus"
        timestamp verified_at "nullable"
        timestamp created_at
        timestamp updated_at
    }
    ingredients {
        int id PK
        varchar name
    }
    allergens {
        int id PK
        varchar name
    }
    dietary_restrictions {
        int id PK
        varchar name
    }
    meal_ingredients {
        int meal_id PK, FK
        int ingredient_id PK, FK
    }
    meal_allergens {
        int meal_id PK, FK
        int allergen_id PK, FK
    }
    meal_dietary_restrictions {
        int meal_id PK, FK
        int dietary_restriction_id PK, FK
    }
    admins {
        int id PK
        varchar email UK "PII"
        varchar google_id UK "PII"
        varchar display_name "PII"
    }
    meal_reports {
        int id PK
        int meal_id FK
        int reviewed_by FK "nullable"
        text description "may contain PII"
        varchar status "ReportStatus"
        timestamp created_at
        timestamp resolved_at "nullable"
    }
    meal_choices {
        int id PK
        int meal_id FK
        varchar session_hash "pseudonymous ID, treat as PII"
        timestamp chosen_at
    }

    stalls ||--o{ meals : sells
    admins |o--o{ meals : verifies
    meals ||--|{ meal_ingredients : lists
    ingredients ||--o{ meal_ingredients : "used in"
    meals ||--o{ meal_allergens : declares
    allergens ||--o{ meal_allergens : "declared in"
    meals ||--o{ meal_dietary_restrictions : suits
    dietary_restrictions ||--o{ meal_dietary_restrictions : "suited by"
    meals ||--o{ meal_reports : receives
    admins |o--o{ meal_reports : reviews
    meals ||--o{ meal_choices : records
```

## Key

| Symbol | Meaning |
|---|---|
| `PK` / `FK` / `UK` | Primary key / foreign key / unique key |
| `\|\|` | Exactly one |
| `\|o` | Zero or one |
| `o{` | Zero or many |
| `\|{` | One or many |
| Comment `"PII"` | Personally identifiable information column |
| `"nullable"` | Value may be empty |

## Notes

- **Cardinality matches `class.md`:** a meal lists one or more ingredients (`||--|{`), while allergens and dietary restrictions may be zero or many.
- **Status columns** hold the values of `MealStatus` and `ReportStatus`.
- **PII columns:** `admins.email`, `admins.google_id`, `admins.display_name`, `meal_reports.description`, and `meal_choices.session_hash`. Student accounts do not exist, so no student identity is stored.
- **Audience:** database developers and whoever handles data privacy.
- **Risk reduced:** a schema that drifts from the class model, or unprotected personal data.
