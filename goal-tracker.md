# Goal Tracker (Work Todo Board)

## Overview
A single-file, offline-first task tracker that runs entirely in the browser. Tasks are organized into four columns, can be edited inline, dragged between columns, tagged with Programs, and persisted in `localStorage`. Data can be exported/imported as JSON.

## Core Features
- Four fixed columns: Waiting on Me, Waiting on Someone Else, Blocked, Completed.
- Inline edit for title/description with automatic save.
- Drag-and-drop between columns (hold to drag).
- Right-click context menu for move/delete.
- Program tags with color-coding and filtering.
- Local persistence with export/import JSON.

## Data Model
### Card
- `title`: string
- `description`: string
- `createdAt`: ISO timestamp string
- `updatedAt`: ISO timestamp string
- `programId`: string or null

### Program
- `id`: string (prefix `prog_` + timestamp)
- `name`: string
- `color`: hex string (e.g. `#3b82f6`)

### Storage Schema (localStorage)
Key: `task-tracker-data`

```json
{
  "version": 2,
  "savedAt": "2025-01-09T12:00:00.000Z",
  "programs": [
    { "id": "prog_1704800000000", "name": "Infra", "color": "#3b82f6" }
  ],
  "columns": {
    "waiting-on-me": [
      {
        "title": "Draft proposal",
        "description": "Outline milestones",
        "createdAt": "2025-01-09T12:00:00.000Z",
        "updatedAt": "2025-01-09T12:10:00.000Z",
        "programId": "prog_1704800000000"
      }
    ],
    "waiting-on-others": [],
    "blocked": [],
    "completed": []
  }
}
```

## Interaction Spec
### Add Task
- Click `+ Add Task` in a column.
- A new card is created and immediately enters edit mode.
- If a Program filter is active, the new card is assigned that Program.

### Edit Task
- Click a card to edit.
- `Enter` in title moves focus to description.
- `Escape` saves and exits edit mode.
- If both title and description are empty, the card is removed.

### Drag-and-Drop
- Press and hold on a card to start dragging.
- A placeholder indicates the original location.
- Drop into another column to move.

### Context Menu
- Right-click a card to open options.
- Move to another column or delete.

### Program Tags
- Programs are created, edited, and deleted via the Programs modal.
- Programs can be used as filters via the Program bar.
- Deleting a program removes the tag from all cards.

### Export/Import
- Export downloads a JSON snapshot of the current `localStorage` data.
- Import replaces all current data with the provided JSON.

## Visual Rules
- Column header and card colors are column-specific.
- Program tags display a contrasting text color for readability.

## Error Handling
- Invalid JSON on import shows an alert.
- Clearing completed tasks prompts for confirmation.

## Extensibility Notes
- Add new columns by updating the `columns` map and HTML structure.
- Add new program colors by extending `presetColors`.
- If you add server-side sync, replace `saveToStorage` and `loadFromStorage`.
