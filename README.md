# Notes-app/README.md

The documentation entry point for the Notes-app project. It currently acts as a basic placeholder for the repository's high-level description and setup instructions.

## Overview

The Notes-app project is a lightweight, browser-based markdown editor designed for persistent note-taking. The application leverages vanilla JavaScript and the DOM API to provide a dynamic user interface without the overhead of heavy frameworks.

The application logic, primarily housed in `app.js`, handles note creation, real-time markdown rendering via the `marked.js` library, and state synchronization with the browser's `localStorage`. This ensures that user data persists across session refreshes. The UI is structured in `index.html` and styled via `style.css`, utilizing a card-based layout for individual notes.

## Functions / Classes

While the `README.md` file itself does not contain executable logic, it documents a project driven by the following core functions found in the application's source code:

### `addNewNote(text = "")`

Creates and injects a new note component into the DOM. Each note contains a toolset for editing and deletion, a preview pane, and a raw text area.

| Parameter | Type | Description |
| --- | --- | --- |
| `text` | `string` | The initial content of the note. Defaults to an empty string for new notes. |

**Example Usage:**

```javascript
// Adding a blank note via the UI button trigger
addNewNote();

// Initializing a note from stored data
addNewNote("## Task List\n- [x] Refactor CSS\n- [ ] Update README");
```

**Internal Logic:**

- **Markdown Rendering**: Uses the `marked()` function to convert text area input into HTML for the `.main` preview div.
- **State Toggling**: The "edit" button toggles the visibility of the `textarea` and the rendered `.main` preview pane using the `.hidden` CSS class.
- **Event Delegation**: Attaches listeners for input (triggering `updateLS`), deletion (removing the DOM element), and editing.

### `updateLS()`

Synchronizes the current state of all notes in the DOM to the browser's `localStorage`.

| Parameter | Type | Return Value |
| --- | --- | --- |
| None | N/A | `void` |

**Example Usage:**

```javascript
// Automatically called after any input event or note deletion
textArea.addEventListener("input", (e) => {
    updateLS();
});
```

**Internal Logic:**

- Queries all `textarea` elements currently in the document.
- Maps the values into a string array.
- Serializes the array to JSON and stores it under the key `"notes"`.

## Dependencies

The project relies on external CDN-hosted assets for styling and markdown processing:

| Dependency | Type | Purpose |
| --- | --- | --- |
| [FontAwesome](https://cdnjs.cloudflare.com/ajax/libs/font-awesome/7.0.1/css/all.min.css) | Stylesheet | UI icons for the "Add", "Edit", and "Delete" actions. |
| [marked.js](https://cdn.jsdelivr.net/npm/marked@3.0.7/marked.min.js) | Library | Client-side markdown-to-HTML parsing. |

## Notes

### Persistence & Storage

The application utilizes `localStorage`, which is synchronous and limited to approximately 5MB per origin. If a user exceeds this quota (e.g., by storing massive amounts of text or many notes), the `updateLS` function will fail silently or throw a `QuotaExceededError`.

### Edit/Preview Mode

Notes initialize in either "edit" or "preview" mode based on the presence of content.

- **Empty Notes**: Display the `textarea` immediately.
- **Populated Notes**: Display the rendered HTML preview.
- **Toggle Mechanism**: The edit button uses `classList.toggle("hidden")` on both the preview div and the textarea.

### Maintenance

When updating the markdown parser, ensure the `marked()` function call in `app.js` is compatible with newer versions of the library, as versions 4.x+ moved to a different initialization pattern compared to the 3.0.7 version currently linked in `index.html`.