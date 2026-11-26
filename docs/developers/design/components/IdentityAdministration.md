---
title: IdentityAdministration
parent: Component reference
grand_parent: Design
nav_order: 11
nav_exclude: true
---

# Hosted Identity administration

[Component reference]({{ site.baseurl }}{% link docs/developers/design/components/index.md %}) · [Source specification](https://github.com/delzubu/atlas-core/blob/4a656981ba21c6139a3d3750e0cb76f8d706284a/docs/design/components/IdentityAdministration.md)

One app-suite frame hosts My account and capability-scoped Administration. Use the
existing Users/Templates/Clients views and editors; no second local UI or component kit.

My account shows verified identity, account state and offered tools. A draft toggle
has no side effect; one primary Submit changes action saves the batch. Tool and linked
Workspace approvals show separate states. Pending/disabled/zero-subscription accounts
remain understandable and never imply access merely because a request was accepted.

Administration uses a scoped request inbox and tenant-only Users/Templates/Clients/Rules
pages. User detail separates four app roles from Tenant administrator (this deployment).
Role hints come from canonical vocabulary; visible actions/choices come from the server.
Sensitive actions return from step-up with their draft intact and explicit confirmation.

Existing-value editors keep Save/Cancel, dirty states and revision conflicts. Credentials
stay masked unless explicitly replaced; clearing is distinct from unchanged. Bulk template
actions preview scope and show independent outcomes. Client callback emptiness is labelled
Any callback URL, including the broader delivery consequence; populated lists are Registered
callbacks. Never reset a client secret as a side effect of editing a name or callback.

Use shared forms, SuiteModal, FieldConflictDialog, ToastProvider and Help. One primary action,
sentence-case verb labels, keyboard/focus restoration, 44px touch targets and narrow layouts.
Errors persist until resolved; status is not color-only. Validate real rendered portal,
request/detail, user/system-role, client editing, credential masking and step-up/error states.

## Final interaction details

Template application uses one shared one-user/bulk review dialog: exact affected users, settings/credential field names, typed reset confirmation, up to 100 accounts per batch and persistent per-field outcomes. Copying credentials is separate from account creation/access grants. Conflicts require a fresh preview.

Independent editors register dirty state with one shared page guard; tab/hash/back navigation and sign-out confirm discarding it. Versioned settings/credentials/role drafts retain the revision originally edited through parent refreshes. Step-up opens a same-account sign-in window; its verified callback refreshes the in-memory session/CSRF while the original draft remains and requires an explicit save retry.

## Static hosting and entry points

The one application lives at `<website>/apps/identity/`, built and deployed through the normal website pipeline. My account is available to everyone; the suite account menu shows Administration only after a live server check confirms tenant administration or a scoped approval role. Recheck on opening the menu and hide the action on failure, sign-out or account change. Tenant administrators enter Users; scoped administrators enter Access requests. Direct URLs never confer authority.

Sign-in opens a separate window. A fixed callback, browser binding, random state, one-use 60-second code and S256 PKCE protect the handoff; the original tab checks exact message origin and source window. Only the pending verifier lives in that tab's memory. Session credentials and drafts are never persisted. Reload asks for sign-in again. Administrative writes still require the exact website Origin, CSRF proof, current authority and recent verified upstream authentication. No general suite bearer token or ambient cookie can enter the account API. Block framed rendering; static CSP connects only to Identity and loads no external assets. SWA also applies response-header framing/referrer/cache protections; Blob has no configurable response-header framing policy, so the entry point refuses frames before rendering.
