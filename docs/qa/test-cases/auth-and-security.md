# Test Cases: Authentication and Security (High Level)

The admin area is protected at **two independent layers**. Every request to the admin area is checked before anything is shown, **and** every admin action checks authorization again on the server. This second check matters because a server-side action can be called directly, not only through the page that normally shows it.

Two questions are kept separate:
- **Authentication:** *Is this a real, signed-in user?*
- **Authorization:** *Is this signed-in user on the list of authorized admins?*

These cases describe behaviour only. Configuration names, routes and implementation details are deliberately left out.

---

### AUTH-001: An unauthenticated visitor can't reach the admin area

| Field | Value |
|---|---|
| Requirement | REQ-AUTH-001: any unauthenticated request to the admin area is denied |
| Risk | RISK-004 Unauthorized admin access |
| Priority | Critical |
| Technique | Negative |
| Level | Unit (routing decision) + auth integration (request/session flow) |
| Preconditions | No signed-in session |
| Steps | 1. Request any admin page directly |
| Expected result | **Redirected to sign-in** before any admin content is produced |
| Automation | Automated |
| Evidence | Automated unit test (routing decision) and integration test (request gate) |

### AUTH-002: A signed-in user who isn't an authorized admin is denied

| Field | Value |
|---|---|
| Requirement | REQ-AUTH-002: only accounts on the admin allow-list are authorized, however they authenticated |
| Risk | RISK-004: any valid account can act as admin |
| Priority | Critical |
| Technique | Negative (equivalence class: *authenticated but not authorized*) |
| Level | Unit |
| Preconditions | A valid signed-in account whose email is **not** on the allow-list |
| Test data | Placeholder email addresses only; no real credentials |
| Steps | 1. Evaluate admin authorization for that account |
| Expected result | **Denied**, exactly as if not signed in. A failed sign-in shows the same generic message whether the password was wrong or the account simply isn't authorized, so the response reveals nothing. |
| Automation | Automated |
| Evidence | Automated unit test (authorization) |
| Notes | The allow-list is matched case-insensitively and whitespace-trimmed. Several admin accounts are supported. |

### AUTH-004: Missing authorization configuration means nobody gets in (fail closed)

| Field | Value |
|---|---|
| Requirement | REQ-AUTH-003: if the admin allow-list is missing or empty, nobody is authorized |
| Risk | RISK-004: a deployment mistake silently opens the admin area to everyone |
| Priority | Critical |
| Technique | Configuration / fail-closed |
| Level | Unit |
| Preconditions | Allow-list configuration missing, or present but empty |
| Steps | 1. Evaluate admin authorization for any account |
| Expected result | **Nobody is authorized.** The system fails closed, never open. |
| Automation | Automated |
| Evidence | Automated unit test (authorization) |
| Notes | A configuration mistake must never make the admin area *more* open. Failing closed turns such a mistake into a visible lockout instead of a silent security hole. |

### AUTH-006: Every admin action checks authorization itself

| Field | Value |
|---|---|
| Requirement | REQ-AUTH-005: every server-side admin action independently enforces sign-in and authorization |
| Risk | RISK-004: an admin action is reachable by a direct request, bypassing the page-level gate |
| Priority | Critical |
| Technique | Structural (static regression guard) |
| Level | Static guard |
| Preconditions | — |
| Steps | 1. Scan the source code of every server-side admin action<br>2. Check that each one (except sign-in/sign-out) performs the admin authorization check |
| Expected result | **Every admin action is protected.** A new action added later without the check makes the test fail. |
| Automation | Automated |
| Evidence | Automated static source-scan test |
| Notes | This guards against a **future** mistake that no ordinary "does this feature work?" test would catch |

---

**Related security checks (automated, not listed in full):**
- **No server secrets in browser code.** A project-wide scan fails if secret-handling code appears in any browser-side file. A second scan checks that secret-holding modules are marked server-only.
- **No private data on the public confirmation page.** Booking identifiers, private link tokens, email and phone number are never shown.
- **SMS reminders never include the customer's name or a manage link.**

**Scope note:** these checks verify specific, known security properties. They are **not a formal security review**, and no penetration test has been performed.
