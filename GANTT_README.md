# Gantt Chart with Deneb

This repository contains a fully functional Gantt chart implementation using Deneb (Vega-Lite specification).

## Files

### 1. `gantt-chart.html`
A standalone HTML file with an embedded Gantt chart visualization. Open this file in any web browser to see the chart in action.

**Features:**
- Interactive tooltips showing task details
- Color-coded by status (Completed, In Progress, Pending)
- Assignee labels on each task bar
- Export functionality (PNG, SVG, or Vega spec)
- Responsive design

**To use:**
1. Open `gantt-chart.html` in a web browser
2. Hover over task bars to see detailed information
3. Click the menu icon (⋮) in the top-right to export

### 2. `gantt-deneb-spec.json`
A Deneb specification file that can be imported into Power BI's Deneb custom visual.

**Features:**
- Ready to use with Power BI
- "Today" marker showing current date
- Hover effects for better interactivity
- Customizable colors and styles
- Progress tracking support

**To use in Power BI:**
1. Install the Deneb custom visual from AppSource
2. Add your data to Power BI with the required columns (see Data Structure below)
3. Add the Deneb visual to your report
4. In the Deneb visual, click "Create" or "Edit"
5. Copy the contents of `gantt-deneb-spec.json`
6. Paste into the Deneb specification editor
7. Map your data fields to the expected field names

## Data Structure

Your data source should include the following columns:

| Column Name | Data Type | Description | Example |
|------------|-----------|-------------|---------|
| **Task** | Text | Name of the task | "Planning", "Development" |
| **StartDate** | Date | Start date of the task | "2025-01-01" |
| **EndDate** | Date | End date of the task | "2025-01-15" |
| **Status** | Text | Current status of the task | "Completed", "In Progress", "Pending", "Not Started", "On Hold", "Delayed" |
| **Assignee** | Text | Person or team assigned | "Team A", "John Smith" |
| **Progress** | Number | Percentage complete (0-100) | 50, 75, 100 |

### Example Data:

```csv
Task,StartDate,EndDate,Status,Assignee,Progress
Planning,2025-01-01,2025-01-15,Completed,Team A,100
Design,2025-01-10,2025-01-30,Completed,Team B,100
Development,2025-01-25,2025-03-15,In Progress,Team C,65
Testing,2025-03-01,2025-03-31,Pending,Team D,0
Deployment,2025-03-25,2025-04-10,Pending,Team A,0
Documentation,2025-02-15,2025-04-05,In Progress,Team B,40
```

## Customization

### Changing Colors

In the JSON specification, modify the `color` encoding's `scale` property:

```json
"scale": {
  "domain": ["Not Started", "In Progress", "Completed", "On Hold", "Delayed"],
  "range": ["#9E9E9E", "#2196F3", "#4CAF50", "#FF9800", "#F44336"]
}
```

### Adjusting Chart Size

In `gantt-deneb-spec.json`, modify:
```json
"width": 800,
"height": 400
```

For the HTML version, modify the `height` property in the spec object.

### Date Format

Change the date display format in the x-axis configuration:
```json
"axis": {
  "format": "%b %d, %Y"  // Change this format string
}
```

Common formats:
- `%b %d, %Y` → Jan 01, 2025
- `%m/%d/%Y` → 01/01/2025
- `%Y-%m-%d` → 2025-01-01
- `%d %b` → 01 Jan

### Bar Height

Adjust the bar thickness by modifying:
```json
"mark": {
  "type": "bar",
  "height": {
    "band": 0.7  // Value between 0 and 1
  }
}
```

## Features Explained

### 1. Color Coding
Tasks are automatically color-coded based on their status:
- **Completed** (Green): Tasks that are finished
- **In Progress** (Blue): Tasks currently being worked on
- **Pending** (Yellow/Orange): Tasks that haven't started yet
- **Not Started** (Gray): Future tasks
- **On Hold** (Orange): Paused tasks
- **Delayed** (Red): Tasks behind schedule

### 2. Today Marker
A vertical dashed line shows the current date, making it easy to see which tasks are current, upcoming, or overdue.

### 3. Interactive Tooltips
Hover over any task bar to see:
- Task name
- Assigned person/team
- Start and end dates
- Duration
- Current status
- Progress percentage

### 4. Task Labels
Each bar displays the assignee name inside or next to it for quick reference.

## Troubleshooting

### Issue: "Cannot read property 'StartDate'"
**Solution:** Make sure your data columns match exactly: `Task`, `StartDate`, `EndDate`, `Status`, `Assignee`, `Progress`

### Issue: Bars not showing correctly
**Solution:** Ensure your date columns are in a proper date format (YYYY-MM-DD) or Date data type in Power BI

### Issue: Today marker not appearing
**Solution:** The marker uses `now()` function. If it's outside your data's date range, you won't see it.

### Issue: Text overlapping
**Solution:** Reduce the number of tasks displayed or increase the chart height

## Advanced Customization

### Adding Progress Bars Within Tasks

You can modify the specification to show progress as a filled portion of each bar. This requires adding another layer with calculated x2 values based on progress percentage.

### Filtering by Assignee

Add a parameter selection to filter tasks by assignee:

```json
"params": [
  {
    "name": "assignee_selection",
    "select": {"type": "point", "fields": ["Assignee"]},
    "bind": "legend"
  }
]
```

### Grouping Tasks

To group tasks by category or phase, add a faceting layer:

```json
"facet": {
  "row": {
    "field": "Phase",
    "type": "nominal"
  }
}
```

## License

This code is free to use and modify for your projects.

## Support

For issues or questions about Deneb/Vega-Lite syntax, refer to:
- [Vega-Lite Documentation](https://vega.github.io/vega-lite/)
- [Deneb Documentation](https://deneb-viz.github.io/)
- [Vega-Lite Examples](https://vega.github.io/vega-lite/examples/)
