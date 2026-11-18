# Google accounts and password-manager scope

18 November 2026. Companion to the [browser-continuity handoff](README.md). This document describes the requested user experience and the current documented boundaries; it does not claim that Google Password Manager integration already works.

## What would make the transition useful

A user with an existing Google account should be able to keep using that identity in the intended Domicile browser profile. If Domicile can access the account’s Google Password Manager vault through a supported integration, that could remove an important reason to keep a separate Chrome browser for those websites.

The requested continuity does not require another product login. It also does not require replacing the user’s Google identity with a Domicile-specific account. The implementation may need Google authorization and the appropriate browser integration; those are technical questions to establish rather than assumptions supplied by a successful website login.

I am equally willing to evaluate a separate password-manager extension backed by Vaultwarden. The goal is a dependable personal browser workflow, with an honest statement of which path works.

## Distinguish five mechanisms

| Mechanism | What it provides | What it does not establish by itself |
|---|---|---|
| Google website sign-in | Authenticated use of a Google website in a particular cookie/storage context. | Browser-level identity integration, Google Password Manager or Chrome Sync. |
| Domicile browser profile | Separation of cookies, storage, history, permissions, extensions and extension state. | A connection to a Google-hosted password vault. |
| Google Password Manager | Credential storage/fill and any supported account-backed password continuity. | Synchronization of history, tabs, website sessions or extension preferences. |
| A separate password manager | Its own vault, browser extension and supported credential behavior. | That its extension functions correctly in every browser or profile. |
| Browser synchronization | Replication of supported categories such as bookmarks, history or open tabs. | That an implementation can use another browser vendor’s service. |

Using the same email/provider identity is a continuity opportunity, not a migration mechanism for every local browser artifact. Existing sessions, extension settings, local encrypted credentials and passkeys may need separate handling.

## Current Domicile profiles already solve an important part

The [profile architecture](https://github.com/cprussin/domicile/blob/ffe3faa7a06877c92dcf3fc156e9b6b3613bf79e/docs/architecture/BROWSER-PROFILES.md) documents a real Chromium profile per browser profile, with separate cookies, storage, cache, history, permissions, extensions and extension state. That last distinction matters: a storage partition alone would not provide independent password-manager extension logins.

Relevant existing behavior:

- A user can have distinct personal/work browser profiles on the same desktop; pages from those profiles can participate in the same desktop layout.
- Configured site routing and explicit profile selection determine the browser context. The shell can show which profile a window uses.
- Redirects and popups with an opener remain in the starting profile so a sign-in flow can complete there.
- Moving a page to a different profile retains its desktop window identity/group but reloads the page in the new browser context. Form input and POST bodies do not carry across.
- Extensions are installed across profiles with per-profile enabled state and separate extension data. A shared installation does not mean a shared vault login.

These are documented source behaviors at the pinned revision, not proof of complete Chrome parity or a statement about which version every user has installed. [Shell browser API](https://github.com/cprussin/domicile/blob/ffe3faa7a06877c92dcf3fc156e9b6b3613bf79e/docs/SHELL-BROWSER-WINDOWS.md#browser-profiles).

## Google Password Manager: the new proposal

In Matrix, Connor first explained that Chromium’s internal password-manager UI was stripped out and suggested a separate password manager. He subsequently said he believes Domicile could support Google Password Manager access that synchronizes with the user’s Google password vault, and asked for an issue to track returning to it after the next release.

That later proposal is the reason for the issue. It is more specific than restoring a local password-saving UI. The existing [shell browser documentation](https://github.com/cprussin/domicile/blob/ffe3faa7a06877c92dcf3fc156e9b6b3613bf79e/docs/SHELL-BROWSER-WINDOWS.md#the-keyboard) still lists built-in password saving/autofill/passkeys as absent and points to password-manager extensions. The proposed account-backed route therefore needs its own design and validation.

Useful capability distinctions for that design:

| Capability | Question to establish |
|---|---|
| Account linkage | What explicit Google sign-in/authorization is required, and how does the user see which identity is linked? |
| Read and fill | Can a credential saved in the existing account-backed vault be offered and filled for the correct site/profile? |
| Save/update | Can a new password or changed password be saved to the intended vault, with understandable user confirmation? |
| Persistence | Does the linkage remain useful after an ordinary browser/desktop restart? What happens on sign-out or account change? |
| Several profiles | Can personal/work contexts remain clear, including when different Google identities or password-manager logins are used? |
| Passkeys | Are passkeys included, separate or unsupported? Password support should not silently imply them. |
| Device continuity | If account-backed password changes propagate elsewhere, which password-manager mechanism supplies that behavior? |

The Google integration is proposed, not selected as the only acceptable route. No particular private API access, browser impersonation technique or release date is assumed.

## Vaultwarden remains an alternative

My interest in a separate vault predates this Matrix exchange. A Bitwarden browser extension backed by Vaultwarden could keep passwords independent of Domicile’s built-in browser UI. Connor also described using Vaultwarden himself.

For this route, the relevant questions are whether the extension loads in the intended profile, opens and focuses its UI correctly, fills the correct website, saves/updates credentials and retains the intended per-profile state. Document account selection and passkey behavior explicitly. Do not treat nominal MV3 compatibility as an end-to-end password-manager test.

This is an acceptable alternative to investigate, not a claim that a new vault has been deployed or that every client capability is already qualified. [Vaultwarden project](https://github.com/dani-garcia/vaultwarden), [Bitwarden browser-extension documentation](https://bitwarden.com/help/getting-started-browserext/).

## History/mobile sync is separate and optional

Phone history/open-tab access came up as an existing browser convenience. I explicitly said it is not required for this request. Supporting Google-account passwords would not automatically supply that feature.

Brave is a useful reference for separation of mechanisms: its Sync v2 uses a device chain, client-side encryption and Brave-operated infrastructure while reusing Chromium’s sync protocol. It is not evidence that Domicile can join a Brave mobile sync chain. [Brave Sync design](https://github.com/brave/brave-browser/wiki/Brave-Sync-v2).

Google also documents restrictions on private Chrome APIs for third-party browsers. A working Google website login should therefore not be used as evidence of Chrome Sync or password-service entitlement. The appropriate integration path remains a question for the implementation. [Chromium API-key guidance](https://www.chromium.org/developers/how-tos/api-keys/), [private API availability announcement](https://blog.chromium.org/2021/01/limiting-private-api-availability-in.html).

The useful scope is **existing identity → clear profile → working password workflow**, with history/tab sync left separate.
