# Org Chart Viewer

A lightweight, interactive org chart viewer that renders a CSV file as a visual tree. Supports drag-and-drop reassignments, inline editing, change tracking, and dual CSV formats. Zero dependencies, no build step — just open in a browser.

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

1. Click **Upload CSV** and select a CSV file
2. The org chart renders as an interactive tree
3. Drag cards to reassign managers, or click cards to edit details
4. Export changes back to CSV or reset to original

### CSV Formats

**Standard Format:**
```csv
Name,Title,Manager,Location
Alice,CEO,,San Francisco
Bob,VP Engineering,Alice,Seattle
Carol,Senior Engineer,Bob,Seattle
```

**Rover Format (auto-detected):**
```csv
"Name","User ID","Job Title","Location","Manager UID"
"Alice","001","CEO","San Francisco",""
"Bob","002","VP Engineering","Seattle","001"
"Carol","003","Senior Engineer","Seattle","002"
```

- **Name**: person's full name (required)
- **Title** / **Job Title**: job title (required)
- **Manager** / **Manager UID**: manager's name or User ID (empty for top-level)
- **Location**: optional, shown below title on cards
- **User ID**: optional, preserved for round-trip export in Rover format

Format is auto-detected. Export preserves the original format.

### Controls

- **Upload CSV** — load a new org chart
- **Expand All** — expand the entire tree
- **Managers Only** — show only the management chain (hide ICs)
- **Collapse All** — collapse everything
- **Save CSV** — export current state (only visible after changes)
- **Reset** — discard all changes (only visible after changes)

### Auto-Load from URL

Append `?file=<path>` to load a CSV automatically:

```
http://localhost:8080?file=data/my-org.csv
```

Personal CSV files go in the `data/` directory, which is gitignored.

### Claude Code Skill: `/load-sheet`

If you use [Claude Code](https://docs.anthropic.com/en/docs/claude-code), the included `/load-sheet` skill downloads a Google Spreadsheet as CSV and provides the auto-load URL.

```
/load-sheet 'My Spreadsheet Name' use Sheet1
```

Requires the [`gws` CLI](https://github.com/nicholasgasior/gws) to be installed and authenticated (`gws auth login`).

## Features

### Drag-and-Drop Reassignment

Drag any person card onto a manager card to reassign their reporting relationship. Visual feedback shows valid (green) and invalid (red) drop targets. Prevents circular dependencies and self-assignment.

### Edit Panel

Click any person card to open an edit panel where you can:
- Change their title
- Change their manager (dropdown)
- Change their location
- View their direct reports

Changes are tracked and can be exported or reset.

### Stats Bar

Shows real-time org statistics:
- Total people count
- Associates (ICs) vs Managers
- Managers of Managers
- Open roles (unfilled positions)
- Associate/Manager ratio

### Change Tracking

Visual diff highlighting after modifications:
- **Orange border** — person moved to new manager
- **Purple border** — title edited
- **Orange + purple** — both moved and edited
- Shows original manager below moved cards

### Open Roles

Leave the "Name" field empty to create placeholders for open positions. Open roles appear with placeholder names and are tracked in stats.

### Per-Node Expand

Each manager card has two click targets:
- **Click the "N direct reports" text** — expand/collapse showing all directs (ICs + managers)
- **Click the +/− icon** — expand/collapse showing only managers in the subtree

### Hover Tooltips

Hover over manager cards to see subtree statistics (total, associates, managers, open roles, ratio).
