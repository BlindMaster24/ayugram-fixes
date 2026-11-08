# Screen reader check (NVDA)

This is not testable by unit tests: `Telegram/SourceFiles/test/` has no accessibility tests, and
imitating a screen reader is meaningless. Every screen needs an NVDA "before/after" log, and that
log goes into the pull request description.

## Requirements

- NVDA with `NVDA menu -> Tools -> Speech Viewer`
- Accessibility Insights for Windows (shows the UIA tree: ControlType, Name, IsEnabled)

## Procedure per screen

1. Run `out\Debug\AyuGram.exe` and open the screen.
2. Record in Speech Viewer what is announced and with which role ("button", "menu item", "list item").
3. Check in Accessibility Insights that the element has a `ControlType`, a `Name`, and an action
   (`Invoke`/`Press`) instead of being plain text.
4. Repeat on the official AyuGram build for the "before" baseline.

## Screen checklist

| Screen | Expected | Status |
|---|---|---|
| Chat context menu | announced as a menu, items as menu items | fix landed (`d5ed937ea2`), unverified |
| Attach menu | items announced as menu items | same fix |
| Reaction picker | reaction with count, selected state | not implemented |
| Emoji/sticker panel | the character, or "sticker from pack X" | not implemented |
| Attachment previews | "photo 1 of 3, N MB" | not implemented |
| AyuGram settings (`SourceFiles/ayu/`) | headings, toggles, input fields | not implemented |

## Evidence format for a pull request

```
NVDA (Speech Viewer), AyuGram 7.2.9:
  before: "Open in new window, Archive, Mute notifications"
  after:  "menu, Open in new window, menu item, 1 of 7"
```
