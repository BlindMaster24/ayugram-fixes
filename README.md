# AyuGram Desktop accessibility

Additive patches that make parts of AyuGram Desktop announce themselves to a screen reader
(NVDA on Windows) instead of being read as flat text. No visual change, no behaviour change
when no screen reader is running.

## Why it is needed

AyuGram Desktop is a tdesktop fork, and tdesktop paints its widgets with `QPainter`. Qt only
provides accessibility for the widgets it knows, so anything custom has to answer
`RpWidget::accessibilityRole()` and friends itself. The infrastructure already exists upstream
(`lib_ui/ui/accessible/*`, `RpWidget::accessibility*`); these patches fill individual gaps that
were never covered.

## Contents

```
patches/desktop-ayu-accessibility.patch   menus: popup container role -> items are announced as
                                          menu items instead of a string of text (4 files, 47 lines)
patches/desktop-media-list.patch          shared media list: role, item count, item names and
                                          geometry, so photos, videos and files can be reached
                                          (2 files, 72 lines)
patches/lib_ui-menu-role.patch            the menu fix where it actually belongs, in lib_ui
                                          (1 file, 3 lines)
docs/screen-reader-checklist.md           how to verify with NVDA, and the evidence format
```

Every patch is additive: only new overrides and helpers, no existing line is modified. That is
what keeps them cheap to review and cheap to rebase when AyuGram merges a new tdesktop tag.

## What is deliberately not here

- Build environment workarounds (`/MP2`, msys package names, `dnsapi.lib`, mirrors). They are
  specific to one machine and must not leak into anyone else's build.
- Our private tooling (the review checker, git hooks). It lives in a private repository.

## Apply

```bash
git am patches/desktop-ayu-accessibility.patch
git am patches/desktop-media-list.patch
```

For the upstream-shaped variant, apply `patches/lib_ui-menu-role.patch` inside `Telegram/lib_ui`
instead of the first patch.

## Verify

Follow `docs/screen-reader-checklist.md`: NVDA with Speech Viewer, plus Accessibility Insights for
the UIA tree. The expected wording for a chat context menu is:

```
before: "Open in new window, Archive, Mute notifications"
after:  "menu, Open in new window, menu item, 1 of 7"
```

## Status

Both patches compile and link into the client. They have not been verified with a screen reader
yet, so nothing has been proposed upstream. Verification logs come first.
