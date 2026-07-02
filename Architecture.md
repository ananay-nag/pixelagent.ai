# PixelAgent.ai: Visual-to-Code Repair Loop

PixelAgent.ai is an intelligent developer utility designed to close the gap between your browser's rendered user interface and your local development editor. By introducing a direct communication loop between browser DOM visual inspections and IDE AI coding sessions, PixelAgent.ai enables developers to select, annotate, and repair styling or layout bugs on the fly.

---

## 1. Product Features

*   **Interactive Visual Picker:** Toggle an element selection mode directly on your running development server (e.g. React/Vite site) to highlight HTML tags, component locations, and styles.
*   **Draggable Intent Panel:** Annotate layout bugs and input styling directives using a floating overlay card next to your targeted element. Override Tailwind CSS utility classes in real-time.
*   **Visual Context Harvesting:** Capture and crop visual layout bugs into screenshots alongside computed CSS style rules and console telemetry, compiling them into a contextual fix package.
*   **Integrated IDE Server:** An embedded local web and WebSocket server running directly inside the IDE extension host to listen for browser issues without external proxy dependencies.
*   **Auto-Focus Workspace Orchestration:** The IDE automatically jumps to the declaring source file, centers the editor viewport, and scrolls the cursor to the exact lines of code.
*   **Safe Code Backups & Rollbacks:** Every AI fix session takes a pre-edit backup copy of the target file, providing one-click rollback options to revert code alterations instantly.
*   **Hot-Module Replacement Syncing:** Live WebSocket listeners notify your browser content scripts the moment the AI agent modifies the workspace. This triggers Vite's Hot Module Replacement (HMR) to reload code changes instantly.

---

## 2. High-Level Architecture

The system topology coordinates three primary components:

```
+-------------------------------------------------------+
|                    1. CHROME BROWSER                  |
|                                                       |
|   +-----------------------+   +-------------------+   |
|   |  Vite Dev Application |   | PixelAgent.ai     |   |
|   |  (React/Tailwind App) |   | Chrome Extension  |   |
|   +-----------+-----------+   +---------+---------+   |
|               ^                         |             |
|   Vite HMR    |                         | Submit      |
|   Hot Reload  |                         | Payload     |
+---------------|-------------------------|-------------+
                |                         |
                |                         | HTTP / WebSockets
                |                         v
+---------------|---------------------------------------+
|               |                                       |
|               |         2. ANTIGRAVITY IDE            |
|               |                                       |
|   +-----------+-----------+   +-------------------+   |
|   |  IDE Extension        |==>|  AI Coding Agent  |   |
|   |  (Embedded Bridge)    |   |  (Cascade Panel)  |   |
|   +-----------------------+   +---------+---------+   |
|                                         |             |
|                                         | Modifies &  |
|                                         | Saves Code  |
|                                         v             |
|                              [ Workspace Src Files ]  |
+-------------------------------------------------------+
```

---

## 3. High-Level Workflow

PixelAgent.ai automates developer iterations using a structured flow:

1.  **Inspect Element:** The developer spots a styling bug (e.g., misaligned button) in the browser, triggers the picker, and clicks the targeted element.
2.  **Submit Fix Directive:** The developer inputs the desired layout behavior in the annotation card, optionally overlays updated Tailwind classes, and triggers the fix.
3.  **Refocus Codebase:** The extension transmits the payload. The IDE focuses the target file, scrolls to the code location, and loads the AI agent context.
4.  **AI Code Repair:** The agent interprets the visual context, computed styles, and instructions to make precise alterations.
5.  **Live Sync & Reload:** The agent saves the code, triggering the local project bundler to Hot-Reload (HMR) the browser view, and dismisses the loading panel.
