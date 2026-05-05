Use the `dobox_get_entries` MCP tool to fetch the user's unresolved DoBox entries. Present the results as a clear, organized list.

For each entry, show:
- **Subject** (bold)
- Description (if available, truncated to ~100 chars)
- Due date (if set, formatted as a human-readable date)
- Number of communications attached
- Whether it's an AI suggestion

Group entries by due date:
1. Overdue (past due date)
2. Due today
3. Upcoming (future due date)
4. No due date

If there are no entries, let the user know their DoBox is empty.

If the tool is not available or returns an error, let the user know the DoBox connector may not be connected yet and suggest they check their plugin settings to ensure the DoBox MCP server is enabled.
