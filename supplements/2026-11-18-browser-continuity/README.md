# Browser continuity: existing Google accounts, passwords and extensions

Technical context from **mecattaf** for Connor Prussin, 18 November 2026. Start here when following the accompanying upstream issue. This is a dated supplement outside the library’s sealed 9 November snapshot; it does not revise those historical feature statuses.

## The request

I would like to use my existing Google accounts in Domicile’s browser profiles and, if the integration you described in Matrix is feasible, access my existing Google Password Manager vault. **This request needs no additional product login or new identity service.** Familiar Google-account continuity is the useful starting point.

I am also willing to move website passwords to a Bitwarden extension backed by Vaultwarden. That was already under consideration before this conversation and remains an acceptable route. Google Password Manager and Vaultwarden are alternatives to evaluate, not two mandatory implementations.

Cross-device browser history and tab synchronization are **not required** for this request. Password-vault access is a separate capability. Neither the availability of browser profiles nor successful Google website sign-in proves that password-vault access or Chrome Sync works.

## Why I still keep Chrome

Chrome supplies a familiar signed-in browser environment with established extensions and useful app-style windows. `chrome --app` also lets the window manager arrange a web application as an independent desktop object, rather than requiring the shell to implement the application itself. A chat and its document, for example, can be separate windows in the same workspace.

Adopting Domicile as the compositor does not require an immediate all-or-nothing browser migration. I explicitly accepted a second Chromium instance during the transition. I am interested in moving particular workflows into the built-in browser as their behavior and compatibility become clear.

My broader technical interests include keyboard navigation and hints, dark mode and per-site styles, tab/window detachment, useful page inspection, context selection and independent web-app windows. A future custom MV3 extension could consolidate selected behaviors while remaining usable in ordinary Chrome and potentially Domicile. It is unfinished; it is not evidence of complete Chrome-extension parity.

These are context for evaluating browser continuity, **not a request to bundle an entire browser replacement into the password-manager issue**.

## Browser analogies that clarify the boundaries

| Familiar behavior | The useful Domicile counterpart | Separate question |
|---|---|---|
| Sign into a Google account | Keep an existing identity and use Google websites in the selected profile. | Access to the Google password vault. |
| Personal/work Chrome profiles | Separate cookies, site data, permissions and extension state. | Desktop workspace placement and layout. |
| Chrome Password Manager | Fill and maintain credentials from the intended vault. | Website sessions, history and tab sync. |
| Bitwarden extension | Use a password manager independent of the browser’s built-in UI. | Compatibility of its popup, autofill and per-profile state. |
| `chrome --app` | A site becomes an independently arranged app-style window. | Whether the browser carrier changes its identity or application state. |
| Browser extension toolbar | The shell presents an extension action and popup for the correct browser context. | Which extension APIs actually work on a desk. |
| Detach/rearrange a tab | Change presentation while retaining the relevant page or window. | Reopening in a different profile, which can reload the page. |

The general principle is **reuse an existing identity and preserve working capabilities while changing their desktop presentation**. A familiar account experience should not require conflating browser profiles, password storage, synchronization and window management.

## Supporting documents

1. [Google accounts and password-manager scope](GOOGLE-ACCOUNTS-AND-PASSWORDS.md): terminology, current profile support, the proposed integration and the alternative vault route.
2. [Chrome and extension continuity](CHROME-AND-EXTENSION-CONTINUITY.md): personal technical requirements, app-window behavior and a compatibility matrix.
3. [Acceptance journeys and open questions](ACCEPTANCE-AND-OPEN-QUESTIONS.md): concrete observations that would establish the behavior without assuming full Chrome parity.

For an assisting coding agent: explain the request to Connor using the mappings above, inspect current upstream contracts, and distinguish already-supported behavior from the proposed password-vault integration. The useful output is a scoped design and its remaining uncertainties, not a new identity platform or an assumption that every contextual preference is a new upstream requirement.

## Evidence boundary

Current upstream descriptions were checked at Domicile main [`ffe3faa7a06877c92dcf3fc156e9b6b3613bf79e`](https://github.com/cprussin/domicile/commit/ffe3faa7a06877c92dcf3fc156e9b6b3613bf79e) on 18 November. The password-vault proposal comes from the Matrix conversation; it has not been independently implemented or verified here. Personal requirements were recovered from authored notes and original conversations, with recent statements taking precedence. No raw chats, account identifiers, browser data or private implementation material are included.
