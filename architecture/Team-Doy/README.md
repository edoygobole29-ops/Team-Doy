<<<<<<< HEAD
# Architecture Design Document: Campus Meal Guide (Team DOY)

Architectural views for the MVP of the *Affordable and Nutritious Meals for Students with Dietary Restrictions* project. Each diagram is written in Mermaid and renders on GitHub.

## Diagrams

| # | Diagram | File | View | Owner | Reviewer |
|---|---|---|---|---|---|
| 1 | C4 system context | [context.md](context.md) | Structure | _Damiar_ | _Gelilio_ |
| 2 | C4 container | [containers.md](containers.md) | Structure | _Gelilio_ | _Damiar_ |
| 3 | Use case | [use-cases.md](use-cases.md) | Scenarios | _Benavides_ | _Gobole_ |
| 4 | Activity | [activity.md](activity.md) | Process | _Gobole_ | _Damiar_ |
| 5 | Sequence | [sequence.md](sequence.md) | Process | _Gobole_ | _Benavides_ |
| 6 | Class | [class.md](class.md) | Logical | _Benavides_ | _Damiar_ |
| 7 | State machine | [state-machine.md](state-machine.md) | Logical | _Gelilio_ | _Gobole_|
| 8 | Package | [packages.md](packages.md) | Development | _Damiar_ | _Gelilio_ |
| 9 | UML component | [components.md](components.md) | Development | _Gelilio_ | _Benavides_ |
| 10 | Deployment (Provisional) | [deployment.md](deployment.md) | Physical | _Gobole_ | _Gelilio_ |
| 11 | ERD (draft) | [erd.md](erd.md) | Data | _Damiar_ | _Gobole_ |

Fill in the owner and reviewer columns to match Form 1. The suggested 4-member split is A: 1, 2, 10; B: 3, 4; C: 5, 8, 9; D: 6, 7, 11.

## Key decisions behind the diagrams

- **Scope:** the directory MVP (M1 to M9). Payments, group buying and the meal planner are out of scope.
- **Containers:** one Next.js web application (pages and REST API together) and one PostgreSQL database. Google Identity is the only external system.
- **Actors:** Student (guest), Admin (Verifier) and Google Identity. Vendors are a data source handled offline by the Admin.
- **Main entity:** `Meal`, with states DRAFT, VERIFIED, FLAGGED, ARCHIVED.

## Cross-view consistency checks

| Check | Views compared | Result |
|---|---|---|
| Same actors everywhere | Context, container, use case | Student, Admin, Google Identity |
| Same containers everywhere | Container, component, deployment | Web Application, PostgreSQL |
| Status values match exactly | Class `MealStatus`, state machine, ERD `meals.status` | DRAFT, VERIFIED, FLAGGED, ARCHIVED |
| Multiplicities match | Class associations, ERD cardinalities | Meal lists 1 or more ingredients; all other many-to-many links are 0 or many |
| Every Must-have has a use case | MVP list M1 to M9, use case diagram | See the traceability table in `use-cases.md` |
| External service behind an interface | Component, package | `IIdentityProvider`, implemented by the `adapters` package |
| Flow uses real states | Activity, sequence, state machine | Verify is DRAFT to VERIFIED; a report moves a meal to FLAGGED |

## Assumptions to confirm with the team

1. Folder names in `packages.md` and endpoint paths in `sequence.md` are proposals.
2. The `allergen-free` filter is modelled as "exclude meals that declare allergen X".
3. Editing a VERIFIED meal sends it back to DRAFT so it must be verified again.
4. Hosting provider is not chosen, so `deployment.md` stays Provisional.

## Contribution log and AI-use disclosure

Complete this in Form 4. Each member writes their own row, including any AI tool used to draft these diagrams and how the output was checked against the MVP document.
=======
# Team-Doy
Affordable and Nutritious Meals for Students with Dietary Restrictions
>>>>>>> 90feeef891883ee8cbf9c0f352968f55d20a318b
