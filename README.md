# Tablewright

A client-side SQLite database viewer and editor in a single HTML file. Open a `.db` or `.sqlite` file, browse and edit rows, run SQL, and download the result. Everything runs in your browser, with no backend and no uploads.

## Features

**Open and save**
- Open a file with the file picker, `Ctrl/Cmd+O`, or by dropping it anywhere in the window
- Create a new empty database
- Download the edited database with `Ctrl/Cmd+S`
- Unsaved-changes indicator and a leave-page guard

**Browse**
- Sidebar with tables and views, row counts and a live filter (`Ctrl/Cmd+K`)
- Multiple table tabs, each remembering its own page, sort, filters, hidden columns and column widths
- Click a header to sort ascending, descending or off, applied in SQL
- Per-column filters: contains, equals, is null
- Page sizes of 50, 100, 250 and 500, with a range readout
- Drag column borders to resize, double-click to reset, or use the arrow keys on a focused handle
- Show or hide columns, and a compact row density toggle
- Distinct rendering for NULL, empty strings, numbers, long text and BLOBs

**Edit**
- Inline cell editing with Enter, Esc and Tab, plus a Set NULL control
- Add, duplicate and delete rows, with confirmation for deletes
- Row details panel with Copy as JSON and Copy as INSERT
- Views and WITHOUT ROWID tables open read-only, with a visible badge

**Query and structure**
- SQL tab with `Ctrl/Cmd+Enter`, multiple statements, timing, verbatim errors and saved history
- Query table shortcut that fills in a `SELECT` for the open table
- Structure view with the `CREATE` statement, columns, indexes and foreign keys

**Import and export**
- Import a CSV as a new table
- Export a table as CSV or JSON, including active filters and sort
- Export any SQL result set as CSV

**Interface**
- Light and dark themes, following the system by default
- Inter typeface in four weights
- Warning banner on screens narrower than 768 px

## Quick start

Open the hosted page, or run it locally:

```bash
git clone https://github.com/drnx64/tablewright.git
cd tablewright
python3 -m http.server 8000
```

Then visit `http://localhost:8000` and drop in a database file.

Tablewright uses the asm.js build of sql.js, so it does not need WebAssembly. It still has to be served over HTTP so the CDN script and fonts load reliably.

## Deploy with GitHub Pages

1. Push `index.html` to the `main` branch.
2. Go to Settings, then Pages.
3. Under Build and deployment, choose Deploy from a branch, select `main` and the `/ (root)` folder, then save.
4. Your site appears at `https://<your-username>.github.io/tablewright/`.

## Keyboard shortcuts

| Action | Shortcut |
| --- | --- |
| Open file | `Ctrl/Cmd+O` |
| Download database | `Ctrl/Cmd+S` |
| Run query | `Ctrl/Cmd+Enter` |
| Filter tables | `Ctrl/Cmd+K` |
| Show shortcuts | `Ctrl/Cmd+/` |
| Edit a cell | `Enter` or double-click |
| Save an edit | `Enter` |
| Cancel an edit | `Esc` |
| Save and edit the next cell | `Tab` |
| Move between cells | Arrow keys |

## How it works

- One `index.html` file with inline CSS and JavaScript, no framework and no build step.
- The only dependency is [sql.js](https://github.com/sql-js/sql.js), pinned to version 1.10.3 on jsDelivr. The only other external request is the Inter font from Google Fonts, which falls back to system fonts.
- Rows are read page by page, so large tables never fill the DOM.
- Row edits use `UPDATE … WHERE rowid = ?` and then re-read the row, so type coercion and triggers show up immediately.
- User-supplied values are always bound parameters, and identifiers are double-quoted with embedded quotes escaped.

## Limitations

- Edits live in memory until you download the database. Reloading the page discards them.
- Add row cannot insert an empty string. Leave a field empty to use the column default, then edit the cell.
- SQL results render the first 500 rows.
- Very large database files are limited by browser memory, and the asm.js engine is slower than WebAssembly.
- Narrow screens are supported but not recommended.

## Browser support

Current versions of Chrome, Edge, Firefox and Safari.

## Contributing

Issues and pull requests are welcome. Keep the project to a single dependency-light file, and check changes in both themes and at 900 px and 1280 px widths.

## License

MIT. See [LICENSE](LICENSE).

## Contact

Best, [@drnx64](https://discord.com/users/929998586375180288) on Discord.
