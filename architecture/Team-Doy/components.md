# Diagram 9: UML Component, Web Application API

**Type:** UML component · **Scope:** the API inside the Next.js Web Application container, with the interfaces each component provides and requires. Google Identity sits behind an adapter interface.

```mermaid
---
title: "UML Component diagram: Web Application API (Next.js container)"
config:
  flowchart:
    wrappingWidth: 340
    nodeSpacing: 55
    rankSpacing: 80
    padding: 18
---
flowchart TB
    browser["<b>Pages in the browser</b><br/>[Student and Admin pages]"]:::outside

    subgraph api["Web Application container: REST API"]
        direction TB

        iGuest(["IGuestApi"]):::iface
        iAdmin(["IAdminApi"]):::iface
        gr["<b>Guest Routes</b>"]:::comp
        ar["<b>Admin Routes</b>"]:::comp

        iCat(["ICatalog"]):::iface
        iFeed(["IFeedback"]):::iface
        iList(["IListingManagement"]):::iface
        iAuth(["IAuth"]):::iface

        cat["<b>Catalog Service</b><br/>browse, filter, details"]:::comp
        feed["<b>Feedback Service</b><br/>reports, review, choices"]:::comp
        lst["<b>Listing Service</b><br/>add, edit, archive, verify"]:::comp
        auth["<b>Auth Service</b><br/>sign-in, session, admin check"]:::comp

        iMeal(["IMealRepository"]):::iface
        iFr(["IFeedbackRepository"]):::iface
        iAr(["IAdminRepository"]):::iface
        iIdp(["IIdentityProvider"]):::iface

        data["<b>Data Access</b><br/>repositories"]:::comp
        adapter["<b>Google Identity Adapter</b>"]:::comp
    end

    pg[("<b>PostgreSQL</b><br/>[Container]")]:::outside
    google["<b>Google Identity</b><br/>[External System]"]:::outside

    browser -.-> iGuest
    browser -.-> iAdmin
    iGuest --- gr
    iAdmin --- ar

    gr -.-> iCat
    gr -.-> iFeed
    ar -.-> iAuth
    ar -.-> iList
    ar -.-> iFeed

    iCat --- cat
    iFeed --- feed
    iList --- lst
    iAuth --- auth

    cat -.-> iMeal
    lst -.-> iMeal
    feed -.-> iMeal
    feed -.-> iFr
    auth -.-> iAr
    auth -.-> iIdp

    iMeal --- data
    iFr --- data
    iAr --- data
    iIdp --- adapter

    data -->|"SQL over TCP"| pg
    adapter -->|"OpenID Connect over HTTPS"| google

    classDef comp fill:#dbe9fb,stroke:#2e6295,color:#111
    classDef iface fill:#fff3cd,stroke:#8a6d00,color:#111
    classDef outside fill:#e2e2e2,stroke:#666,color:#111
    style api fill:none,stroke:#555,stroke-dasharray:5 5
```


## Key

| Symbol | Meaning |
|---|---|
| Blue box `«component»` | A component inside the API |
| Yellow rounded shape | An interface (a named contract) |
| Solid line, component to interface | The component **provides** the interface |
| Dashed arrow, component to interface | The component **requires** the interface |
| Grey box | Outside this container |
| Labelled solid arrow | Runtime connection and its protocol |

## Notes

- Google Identity is reached only through `IIdentityProvider`, so Auth Service never depends on Google directly and can be tested with a fake.
- The database is reached only through the three repository interfaces provided by Data Access.
- **Audience:** back-end developers.
- **Risk reduced:** tight coupling to Google or to the database, which makes the code hard to test or change.
