# Thoughts

**Turn long AI chats into a clear, organized list of tasks.**

Thoughts is a browser extension plus a website. The extension captures a student's AI conversations in the background. The website turns them into an organized task list. Each task has a confidence score, a concept diagram, and a recommended path to completion.

> **Status:** Early development. Nothing below is built unless marked otherwise. See the [Roadmap](#roadmap).

---

## The Main Thing

**The extension's capture-to-task pipeline, working end to end for one AI site.**

```
Extension reads a chat → sends it to our API → backend creates a task → a page shows it
```

Everything else (login, dashboard polish, diagram, settings) is an upgrade to this chain. Build the thin version first, even if it is ugly. If this chain works, we have a real product. Anything not on this path goes in [Parked Ideas](#parked-ideas) until it works.

---

## The Problem

Long AI conversations are hard to use after the fact:

- Important ideas get buried in walls of text.
- It's rarely clear what the actual next steps are.
- Finding an earlier chat means scrolling through endless messages.

## The Solution

Two parts that share one backend:

1. **Browser extension (the core of the product):** captures conversations in the background, with a visible on/off switch.
2. **Website:** a dashboard that turns captured conversations into tasks, and a detail view for each task.

---

## Brand

| Color | Name | Hex | Use |
|---|---|---|---|
| ⬛ | **True Black** | `#000000` | Text, nav, primary buttons, dark surfaces |
| 🟨 | **Rich Gold** | `#D4AF37` *(proposed, confirm)* | Accents, highlights, confidence bar, key actions |
| ⬜ | White / light gray | `#FFFFFF` / `#F5F5F5` | Backgrounds and cards |

- Gold on black has strong contrast. **Gold text on white does not**, so on light backgrounds use black text and use gold only for fills, borders, and bars.
- Define colors once as design tokens (CSS variables or the Tailwind theme), never hard-coded per component.
- The wireframes are low-fidelity grayscale. Brand colors are applied on top of them.

---

## User Flow

```mermaid
flowchart TD
    A[01 Landing / Login] -->
