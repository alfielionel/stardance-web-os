# wobblyOS

A tiny browser-based desktop OS with physically simulated windows.
No frameworks. No canvas. Just springs.

## structure

```
index.html          — shell, loads HTMX, background, boots the app
content/welcome     — login screen, served on first load
content/home        — the desktop: windows, taskbar, apps
```

## how it works

Every window is a DOM element driven by a spring simulation running in `requestAnimationFrame`. There are no CSS transitions or keyframe animations anywhere.

Each window tracks:
- a **target position** (where the mouse is dragging it to)
- a **current position** (which chases the target at 18% per frame)
- a **velocity** derived from the remaining gap

That one velocity value feeds rotation, squash & stretch, and a sine-based bounce simultaneously.

A second spring handles open/close scale — windows pop in from nothing with an overshoot, and squash down toward the taskbar when closed. The taskbar button itself animates in on its own spring, 500ms after the window disappears.

## apps

| icon | name | what it does |
|------|------|-------------|
| 🖥️ | Console | in-browser JS console, intercepts `console.log/warn/error/info` |
| 📺 | Subscribe | embedded @plutato YouTube playlist |

## stack

- [HTMX](https://htmx.org) — page transitions between welcome and home
- [DaisyUI](https://daisyui.com) — component styles
- [Tailwind](https://tailwindcss.com) — utility classes (browser build)
- [Crimson Pro](https://fonts.google.com/specimen/Crimson+Pro) — font
- zero build step, zero bundler

## running locally

Just serve the project root over HTTP — the HTMX requests need a server.

```bash
npx serve .
# or
python -m http.server
```

Then open `http://localhost:3000` (or whatever port).

## adding a new app

1. Add a desktop icon to the `#desktop` div in `content/home`
2. Create a window element (or build it in JS)
3. Call `makeWobblyWindow(el, "🎯 label")` — drag, animation, minimize, taskbar all come for free
4. Return `{ minimize, restore }` if you need to control it externally
