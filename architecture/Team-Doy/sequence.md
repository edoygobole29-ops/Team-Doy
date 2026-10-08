# Diagram 5: UML Sequence, Admin signs in and verifies a meal

**Type:** UML sequence · **Scope:** the riskiest flow. It combines the one external system (Google sign-in), an authorization check, and a status change (DRAFT to VERIFIED).

```mermaid
---
title: "UML Sequence Diagram: Admin signs in and verifies a meal listing"
---
sequenceDiagram
    autonumber
    actor Admin
    participant Page as Admin Page<br/>(browser)
    participant G as Google Identity<br/>(external)
    participant API as REST API<br/>(Node.js)
    participant DB as Database<br/>(PostgreSQL)
 
    rect rgba(70, 130, 220, 0.12)
        Note over Admin,DB: Phase 1: Sign in
        Admin->>Page: Click "Sign in with Google"
        Page-)G: Redirect to Google sign-in
        G--)Page: Return authorization code
        Page->>API: POST /auth/callback (code)
        API->>G: Exchange code for ID token
        G-->>API: ID token (verified email)
        API->>DB: Find admin by email
        DB-->>API: Admin record or none
        alt Email matches an Admin record
            API-->>Page: Session cookie
            Page-->>Admin: Show admin dashboard
        else No matching Admin record
            API-->>Page: 403 Forbidden
            Page-->>Admin: Show "Not authorized"
        end
    end
 
    rect rgba(80, 170, 90, 0.12)
        Note over Admin,DB: Phase 2: Verify a DRAFT meal
        Admin->>Page: Click "Verify" on a meal
        Page->>API: POST /meals/{id}/verify
        API->>API: Validate session cookie
        alt Session missing or expired
            API-->>Page: 401 Unauthorized
            Page-->>Admin: Ask to sign in again
        else Session valid
            API->>DB: Get meal status
            DB-->>API: Meal with status
            alt Status is DRAFT
                API->>DB: Set VERIFIED, verified_at, verified_by
                DB-->>API: Update confirmed
                API-->>Page: 200 OK (meal is VERIFIED)
                Page-->>Admin: Show verified badge and date
            else Status is not DRAFT
                API-->>Page: 409 Conflict
                Page-->>Admin: Show "Only DRAFT meals can be verified"
            end
        end
    end
```

## Key

| Symbol | Meaning |
|---|---|
| Solid arrow with filled head | Synchronous request |
| Dashed arrow | Reply |
| `alt` / `else` | Alternative branches; only one runs |
| Shaded band | A phase of the flow |

## Notes

- Every call in this flow is synchronous. The MVP has no asynchronous messages, so none are drawn.
- Endpoint paths (`/auth/callback`, `/meals/{id}/verify`) are **proposed** and must match the final API contract.
- **Audience:** front-end and back-end developers.
- **Risk reduced:** unauthorized verification, and invalid status changes found only at integration time.
