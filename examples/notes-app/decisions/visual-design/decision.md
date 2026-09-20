## VisualDesign

### Context

`RenderNoteList` fixes what the notes screen shows and in what order, but not how it looks. Layout, spacing, type, and color are choices the implementation would otherwise make on its own.

### Requirement

Every screen must be implemented against a design the author has approved, so that appearance is not decided during implementation.

### Decision

The implementation follows the wireframes and design tokens kept in its own repository under a `design/` directory: one wireframe per screen, and one token file for color, type, and spacing. Behavior is specified here; appearance is specified there; a screen that departs from either is a defect.
