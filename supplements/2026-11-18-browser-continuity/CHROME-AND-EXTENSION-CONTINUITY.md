# Why keep Chrome, and what would allow selected workflows to move?

18 November 2026. Personal technical context for the [account/password request](GOOGLE-ACCOUNTS-AND-PASSWORDS.md). These observations explain current usage; they are not a demand to implement every row as part of that issue.

## Chrome is a working environment, not only a password manager

The reasons to retain Chrome include signed-in identities, extension behavior, everyday input quality and independently arranged web applications. Profiles and password storage cover only part of that environment.

I have deliberately kept ordinary web applications separate from desktop-shell implementation. Collaborative documents, chat and editors can have their own windows, arranged by the window manager. A chat and a document can sit beside one another without being rebuilt as components inside the shell.

`chrome --app` is a familiar carrier for this: a URL-backed application becomes an application-like window with little browser chrome. The application’s own session and backend remain responsible for its state. The shell supplies placement, focus, workspace organization and desktop interaction. That division is useful even when the eventual browser carrier changes.

I also explicitly accepted an additional Chromium instance while trying Domicile. Replacing the compositor and replacing an established browser workflow are different steps. Keeping external Chrome during gradual adoption is an intentional compatibility route.

## Personal requirements matrix

| Area | Behavior to preserve or evaluate | Status of the requirement |
|---|---|---|
| Profiles | Clear personal/work sign-ins, site data, permissions and extension state. | Current need; upstream profiles already address much of it. |
| Existing Google identity | Keep using an existing account for Google websites and any supported account-linked browser functionality. | Current need; not equivalent to requesting Chrome Sync. |
| Passwords | Evaluate the proposed Google vault integration or a separate password-manager extension with Vaultwarden. | Open implementation choice; neither route is claimed qualified here. |
| Keyboard browsing | Familiar navigation/hints, find/marks where useful, website exceptions and compatibility with web-app shortcuts. | Established workflow; exact extension/API compatibility needs checking. |
| Appearance | Dark mode and per-site styles without rebuilding every website. | Established workflow; can remain in a separate extension. |
| Tab/window utilities | Detach a page into a window, preserve useful page state, inspect pages and export selected/open URLs. | Established workflow; desk semantics may differ from Chrome’s tab strip. |
| Browser automation | Understand which existing automation extensions/native-messaging routes work, need an adapter or should remain in external Chrome. | Compatibility question, not a complete-parity claim. |
| App-style windows | Independent site windows with predictable focus/placement and useful desktop identity. Separate chat/document windows remain possible. | Current need; `chrome --app` is the reference behavior. |
| Everyday input | Normal typing, scrolling, pinch and applicable navigation gestures. | Acceptance requirement; historical concerns do not establish a current defect. |
| Continuity | Ordinary shell presentation changes should preserve the intended running applications; profile changes and restarts need clearly stated semantics. | Current need; different transitions offer different guarantees. |

Phone history/open-tab sync is optional and not required by the password issue. PiP and Google Docs Offline occurred in an installed-extension inventory; that alone does not establish either as a current migration blocker.

## Established extension habits

The following were recorded in personal browser notes:

| Habit/reference | What matters about the behavior |
|---|---|
| Vimium, with familiar yellow hints | Keyboard-first navigation and exceptions for sites with their own controls. Nyxt-style navigation was also a useful reference. |
| Detach Tab to New Window | A familiar Ctrl+M action for moving a page out into its own window, ideally without unnecessary reload/state loss. |
| Dark Reader / per-site userstyles | Useful website appearance without requiring a new browser renderer or shell implementation. |
| Click-to-remove | Selectively hide an unwanted page element with a clear undo route. |
| WhatFont / element inspection | Inspect a page’s visual details and make explicit references to selected elements. |
| Save All Tab URLs | Simple, useful access to the current browser collection. |
| Minimal new-tab page | A quiet black-background clock was an existing preference; the exact new-tab presentation can change. |
| Ad blocking and other focused extensions | Keep established specialist functionality separate when consolidation would not help. |

The goal of a future custom MV3 extension is a coherent set of selected behaviors and context tools. It is **not** motivated by a blanket assumption that Domicile refuses other extensions. The custom consolidation is unfinished and does not establish feature parity or Domicile compatibility. Existing focused extensions may remain the better route for particular capabilities.

Page/text/element selection also provides a useful analogy for desktop integration: select an explicit object, preserve enough identity to refer to it later, and hand that reference to the application that owns the interaction. A browser overlay can supply page context; it does not automatically provide access to native application controls.

## What is specific to Domicile

Current [shell-extension documentation](https://github.com/cprussin/domicile/blob/ffe3faa7a06877c92dcf3fc156e9b6b3613bf79e/docs/SHELL-EXTENSIONS.md) supplies a concrete compatibility model:

- Browser guests participate in the extension tab model; the shell presents action icons and popups.
- Activating an extension action grants `activeTab` as a browser toolbar would. Correct activation/focus is part of the integration, not decorative UI.
- Extension popups need the correct popup context. A normal page frame can render a password-manager popup incorrectly.
- Side-panel requests become shell-managed popup windows. Placement, sizing and focus need the shell’s handling.
- Some tab-strip actions, including `tabs.move`, `tabs.group` and `tabs.discard`, are explicitly unsupported on a desk.
- The current profile architecture gives each profile separate extension state and profile-scoped visibility. Check that behavior rather than assuming one global browser context.

Consequently, “both use Chromium” is not enough to establish extension compatibility. Test the particular features and supply a documented fallback/refusal where their browser semantics do not map onto a desktop. [Extension architecture](https://github.com/cprussin/domicile/blob/ffe3faa7a06877c92dcf3fc156e9b6b3613bf79e/docs/architecture/EXTENSIONS.md), [profile architecture](https://github.com/cprussin/domicile/blob/ffe3faa7a06877c92dcf3fc156e9b6b3613bf79e/docs/architecture/BROWSER-PROFILES.md).

## Historical reasons versus current bugs

Earlier personal tests raised questions about embedded-browser presentation latency and momentum after finger lift. Those observations helped motivate keeping web applications in Chrome. They are dated usage context, **not a claim that the latest Domicile still has those problems**, and not an independently controlled benchmark.

The relevant next comparison is behavioral: can a selected everyday workflow move cleanly into the current built-in browser? Establish the answer per capability. Profile support, an acceptable password route and a few well-qualified extension/app-window behaviors could be valuable without reproducing all of Chrome first.
