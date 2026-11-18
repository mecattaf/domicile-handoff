# Acceptance journeys and open questions

18 November 2026. Proposed ways to evaluate the [Google-account/password workflow](GOOGLE-ACCOUNTS-AND-PASSWORDS.md) and its [browser-continuity context](CHROME-AND-EXTENSION-CONTINUITY.md). These checks have not been run for the proposed Google integration. They are scoped observations, not an assumption about implementation or release scheduling.

## A. Existing identity in the intended profile

1. Choose a regular Domicile browser profile and sign into an existing Google account through the intended supported route.
2. Establish what succeeded: website login, browser account linkage, password-vault linkage, or more than one of these. The UI/documentation should make the distinction clear.
3. Open the same service from an ordinary browser window and an app-style window. Confirm which profile each uses and that routing does not silently select another identity.
4. Exercise a sign-in redirect/popup and confirm it completes in its intended browser context.
5. Verify ordinary close/reopen behavior and state retention at the documented level.

**Useful evidence:** the selected profile, visible account-link state and the observed result. Account identifiers, tokens, cookies or exported profile data are not needed in a public report.

## B. Google password-vault route, if supported

| Journey | Observable result |
|---|---|
| Existing password | A test credential already in the account-backed vault is offered for its intended site and can be filled. |
| New password | Saving a new test credential names the intended destination and makes it available through the supported vault route. |
| Password change | Updating the test credential is distinguishable from creating an unrelated duplicate; the intended vault receives the update. |
| Profile separation | Two browser profiles make their selected identity/vault state understandable; filling in one does not imply access from the other. |
| Restart | The user understands which account linkage and vault availability persist, and whether reauthorization or unlocking is required. |
| Sign-out/account change | The behavior is explicit for Google sign-out, profile sign-out/change and any independent vault lock. |
| Passkeys | State separately whether registration, use and synchronization are supported, limited or outside scope. |
| Another device | Where password continuity is part of the supported vault mechanism, distinguish its observed result from browser history/tab synchronization. |

The key result is a working credential journey, not merely a successful account login. Use suitable test sites/credentials for demonstrations and keep their contents out of reports.

## C. Separate password-manager route

If Google vault integration is unavailable or deferred, evaluate a Bitwarden extension with a Vaultwarden backend as the alternative:

- Extension loading/enabled state in the intended profile.
- Action activation, popup size, focus, unlock and return to the page.
- Fill, save and update on a suitable test site.
- Independent extension state across personal/work profiles.
- Persistence and clearly described unlock requirements after restart.
- Any passkey or native-messaging limitations, stated independently.

This route needs the password manager’s own vault authentication, but no Domicile-specific account or unrelated product login. It does not require a new built-in vault. This document does not assert that the route has already been deployed or fully qualified.

## D. Small browser/app-window comparison

Use the same application workflow in external Chrome and current Domicile. Check the capabilities actually used rather than a synthetic all-of-Chrome score:

| Area | Observe |
|---|---|
| Presentation | Can an app-style window be arranged independently without unnecessary browser chrome? |
| Identity | Which profile contains the page, and is that visible when choosing or moving it? |
| Focus/input | Do activation, typing, scrolling, pinch and applicable shortcuts reach the intended page or browser UI? |
| Extension actions | Do the selected action/popup and active-page behavior work in the chosen profile? |
| Useful page state | Does a presentation-only rearrangement retain the expected page/session? |
| Profile move | Is a page reload and any loss of form state clear, consistent with the current profile contract? |
| Shell reload | What survives a shell-only refresh? Distinguish that from browser/compositor restart or process recovery. |
| Coexistence | Can external Chrome remain a usable fallback while a selected workflow moves into the built-in browser? |

These comparison checks are contextual. They should not become unrelated acceptance blockers for a narrowly scoped password-manager implementation.

## Open questions for the issue

1. What supported mechanism would connect Domicile to an existing Google Password Manager vault, and which Google authorization is required?
2. Is the proposed first increment read/fill only, or does it also save/update? Are passkeys separate?
3. How is the vault/account selection represented across several browser profiles?
4. Which part is reusable engine/browser behavior and which part needs shell UI?
5. If the Google route is deferred, what is the documented, qualified password-manager-extension route?

## Outside this request

Mobile browser development, cross-device history/open-tab sync, complete Chrome feature parity and a new identity/account service are not requirements here. Brave’s sync chain and Google’s browser synchronization are reference mechanisms, not assumed services Domicile can join.

Current upstream [browser-profile](https://github.com/cprussin/domicile/blob/ffe3faa7a06877c92dcf3fc156e9b6b3613bf79e/docs/architecture/BROWSER-PROFILES.md), [browser-window](https://github.com/cprussin/domicile/blob/ffe3faa7a06877c92dcf3fc156e9b6b3613bf79e/docs/SHELL-BROWSER-WINDOWS.md) and [extension](https://github.com/cprussin/domicile/blob/ffe3faa7a06877c92dcf3fc156e9b6b3613bf79e/docs/SHELL-EXTENSIONS.md) contracts provide the baseline. The proposed Google password-vault integration needs additional evidence.
