# MD3 Layout and Responsive Design

Reference for Material Design 3's layout system: breakpoints, canonical layouts, and responsive implementation.

## Window Size Classes

MD3 defines 5 breakpoint classes:

| Class | Width Range | Typical Devices | Columns |
|-------|-----------|----------------|---------|
| Compact | < 600dp | Phone portrait | 4 |
| Medium | 600–839dp | Tablet portrait, foldable | 8 |
| Expanded | 840–1199dp | Tablet landscape, small desktop | 12 |
| Large | 1200–1599dp | Desktop | 12 |
| Extra-large | 1600dp+ | Ultra-wide, large desktop | 12 |

### CSS Media Queries

```css
/* Compact (default — mobile-first) */
/* No media query needed, this is the base */

/* Medium */
@media (min-width: 600px) { }

/* Expanded */
@media (min-width: 840px) { }

/* Large */
@media (min-width: 1200px) { }

/* Extra-large */
@media (min-width: 1600px) { }
```

### dp to px Conversion
On the web, 1dp ≈ 1px at standard density. The breakpoint values translate directly to CSS pixels.

## Layout Anatomy

### Key Terms

- **Window**: The visible area of the app
- **Pane**: A layout container within the window. A pane is fixed, flexible, floating, or semi-permanent
- **Column**: A vertical content block within a pane
- **Margin**: Space between screen edge and content
- **Gutter**: Space between columns
- **Spacer**: Space between panes (in multi-pane layouts)

### Margin and Gutter Values

| Window Size | Margins | Gutters |
|-------------|---------|---------|
| Compact | 16dp | 8dp |
| Medium | 24dp | 16dp |
| Expanded | 24dp | 16dp |
| Large | 24dp | 24dp |
| Extra-large | 24dp | 24dp |

## Canonical Layouts

MD3 defines 3 canonical layouts as starting points. Always begin from one of these rather than from a raw grid.

### Feed Layout

**Use when**: Displaying a large collection of browsable items (social feed, news, product grid).

```
Compact:    Single column of cards
Medium:     2-column grid
Expanded:   3-column grid
Large:      4-column grid + optional side panel
```

```html
<div class="md3-feed">
  <div class="md3-feed__item">
    <!-- Card content -->
  </div>
  <div class="md3-feed__item">
    <!-- Card content -->
  </div>
  <!-- More items -->
</div>
```

```css
.md3-feed {
  display: grid;
  gap: 8px;
  padding: 16px;
  grid-template-columns: 1fr; /* Compact: 1 column */
}

@media (min-width: 600px) {
  .md3-feed {
    grid-template-columns: repeat(2, 1fr);
    gap: 16px;
    padding: 24px;
  }
}

@media (min-width: 840px) {
  .md3-feed {
    grid-template-columns: repeat(3, 1fr);
  }
}

@media (min-width: 1200px) {
  .md3-feed {
    grid-template-columns: repeat(4, 1fr);
    gap: 24px;
  }
}
```

### List-Detail Layout

**Use when**: Browsing a list of items where each has detailed content (email, file browser, contacts).

```
Compact:    List view OR detail view (navigate between them)
Medium:     Side-by-side list (1/3) + detail (2/3)
Expanded:   Side-by-side with wider detail pane
```

```html
<div class="md3-list-detail">
  <aside class="md3-list-detail__list">
    <md-list>
      <md-list-item type="button" class="active">
        <div slot="headline">Item 1</div>
        <div slot="supporting-text">Description</div>
      </md-list-item>
      <md-list-item type="button">
        <div slot="headline">Item 2</div>
        <div slot="supporting-text">Description</div>
      </md-list-item>
    </md-list>
  </aside>
  <main class="md3-list-detail__detail">
    <h2>Item 1 Detail</h2>
    <p>Full content here...</p>
  </main>
</div>
```

```css
.md3-list-detail {
  display: flex;
  flex-direction: column;
  min-height: 100%;
}

.md3-list-detail__list {
  background: var(--md-sys-color-surface-container);
  border-radius: var(--md-sys-shape-corner-large);
  overflow: auto;
}

.md3-list-detail__detail {
  flex: 1;
  padding: 24px;
}

/* Compact: show one at a time */
@media (max-width: 599px) {
  .md3-list-detail__detail { display: none; }
  .md3-list-detail--detail-active .md3-list-detail__list { display: none; }
  .md3-list-detail--detail-active .md3-list-detail__detail { display: block; }
}

/* Medium+: side by side */
@media (min-width: 600px) {
  .md3-list-detail {
    flex-direction: row;
    gap: 24px;
    padding: 24px;
  }
  .md3-list-detail__list {
    width: 360px;
    flex-shrink: 0;
  }
}

/* Expanded: wider detail */
@media (min-width: 840px) {
  .md3-list-detail__list {
    width: 400px;
  }
}
```

### Drag Handle for Resizable Panes

In list-detail and supporting pane layouts, users can resize panes with a drag handle:

```html
<div class="md3-list-detail">
  <aside class="md3-list-detail__list">...</aside>
  <div class="md3-drag-handle" role="separator" aria-orientation="vertical" tabindex="0"></div>
  <main class="md3-list-detail__detail">...</main>
</div>
```

```css
.md3-drag-handle {
  width: 4px;
  cursor: col-resize;
  background: transparent;
  position: relative;
}
.md3-drag-handle::after {
  content: '';
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  width: 4px;
  height: 24px;
  background: var(--md-sys-color-outline);
  border-radius: 2px;
}
```

### Supporting Pane Layout

**Use when**: Primary content needs supplementary information (document + properties panel, video + comments).

```
Compact:    Stacked — primary on top, supporting below (or bottom sheet)
Medium:     Side-by-side (2/3 primary + 1/3 supporting)
Expanded:   Same but with more space
```

```html
<div class="md3-supporting-pane">
  <main class="md3-supporting-pane__primary">
    <!-- Primary content (2/3) -->
  </main>
  <aside class="md3-supporting-pane__secondary">
    <!-- Supporting content (1/3) -->
  </aside>
</div>
```

```css
.md3-supporting-pane {
  display: flex;
  flex-direction: column;
  gap: 16px;
  padding: 16px;
}

/* Medium+: side by side */
@media (min-width: 600px) {
  .md3-supporting-pane {
    flex-direction: row;
    gap: 24px;
    padding: 24px;
  }
  .md3-supporting-pane__primary { flex: 2; }
  .md3-supporting-pane__secondary { flex: 1; }
}
```

## CSS Container Queries

For component-level responsive behavior (independent of viewport), use container queries:

```css
/* Define a container */
.md3-card-container {
  container-type: inline-size;
  container-name: card;
}

/* Respond to container width */
@container card (min-width: 400px) {
  .md3-card {
    flex-direction: row; /* Horizontal layout when container is wide */
  }
}

@container card (max-width: 399px) {
  .md3-card {
    flex-direction: column; /* Vertical layout when narrow */
  }
}
```

## Adaptive Component Behavior

Components transform across breakpoints:

| Component | Compact | Medium | Expanded+ |
|-----------|---------|--------|-----------|
| Navigation | Bottom bar | Side rail | Side drawer |
| App bar | Small (64dp) | Small (64dp) | Small or Medium (112dp) |
| Dialog | Full-screen | Centered dialog | Centered dialog |
| Bottom sheet | Full height | Partial height | Side sheet |
| Search | Full-screen search view | Persistent search bar | Persistent search bar |
| Cards | Full-width single column | Multi-column grid | Multi-column grid |

## Complete App Layout Example

```html
<div class="md3-app-layout">
  <!-- Navigation (responsive — see navigation-patterns.md) -->
  <nav class="md3-nav" aria-label="Main navigation">
    <!-- Nav content varies by breakpoint -->
  </nav>

  <!-- Main area -->
  <div class="md3-main-area">
    <!-- Top app bar -->
    <header class="md3-top-app-bar">
      <h1 class="md3-top-app-bar__title">Dashboard</h1>
    </header>

    <!-- Content area with canonical layout -->
    <main class="md3-content-area">
      <!-- Use feed, list-detail, or supporting pane here -->
    </main>
  </div>
</div>
```

```css
.md3-app-layout {
  display: flex;
  min-height: 100vh;
  background: var(--md-sys-color-surface);
  color: var(--md-sys-color-on-surface);
}

.md3-main-area {
  flex: 1;
  display: flex;
  flex-direction: column;
  min-width: 0; /* Prevent flex overflow */
}

.md3-content-area {
  flex: 1;
  overflow-y: auto;
}

/* Compact: stack vertically */
@media (max-width: 599px) {
  .md3-app-layout { flex-direction: column; }
  .md3-content-area { padding: 16px; }
}

/* Medium */
@media (min-width: 600px) and (max-width: 839px) {
  .md3-content-area { padding: 24px; }
}

/* Expanded+ */
@media (min-width: 840px) {
  .md3-content-area { padding: 24px; }
}

/* Large+: constrain max content width */
@media (min-width: 1200px) {
  .md3-content-area {
    max-width: 1040px;
    margin: 0 auto;
    padding: 24px;
  }
}
```

## Spacing System

MD3 uses a 4dp base grid for spacing:

| Use | Values |
|-----|--------|
| Component internal padding | 4, 8, 12, 16, 24dp |
| Between components | 8, 12, 16, 24dp |
| Section spacing | 24, 32, 48dp |
| Layout margins | 16dp (compact), 24dp (medium+) |
| Grid gutters | 8dp (compact), 16dp (medium), 24dp (large+) |

Always use multiples of 4dp for consistent spatial rhythm.
