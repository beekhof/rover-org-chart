# Org Chart Viewer

A lightweight, interactive org chart viewer that renders a CSV file as a visual tree. Zero dependencies, no build step — just open in a browser.

## Running Locally

Open `index.html` directly in your browser, or serve it locally:

```bash
# Python
python3 -m http.server 8080

# Node
npx serve .
```

Then open http://localhost:8080.

## Usage

1. Click **Upload CSV** and select a CSV file with columns: `Name`, `Title`, `Manager`
2. The org chart renders as an interactive tree

### CSV Format

```csv
Name,Title,Manager
Alice,CEO,
Bob,VP Engineering,Alice
Carol,Senior Engineer,Bob
```

### Controls

- **Expand All** — expand the entire tree
- **Managers Only** — show only the management chain (hide ICs)
- **Collapse All** — collapse everything

### Per-Node Expand

Each manager card has two click targets:

- **Click the "N direct reports" text** — expand/collapse showing all directs (ICs + managers)
- **Click the +/− icon** — expand/collapse showing only managers in the subtree
