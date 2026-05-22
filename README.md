# EliteSheets

🇬🇧 English | [🇪🇪 Eesti](README.et.md)

---

**Stop exporting sheets one by one.** EliteSheets is a Revit add-in that lets you select any number of sheets and export them all to PDF, DWG, and DXF in a single click — without ever leaving Revit.

---

## What it does

EliteSheets adds a **Jooniste Printimine** button to the RK Tools tab in your Revit ribbon. Click it to open a clean, floating panel where you can:

- **Pick your sheets** — browse, search, and filter your entire sheet list. Check the ones you want.
- **Choose your formats** — export to PDF, DWG, DXF, or all three at once.
- **Hit Export** — EliteSheets handles the rest in the background while you stay in Revit.

---

## Key features

**Batch export to PDF, DWG and DXF**  
Select as many sheets as you need and export them all at once. No more repeating the same export dialog for every sheet.

**Smart sheet merging**  
Working with drawing sets? EliteSheets automatically detects grouped sheets and merges them side-by-side into a single combined DXF file — ready to send.

**Search and filter**  
Quickly find the sheets you need by typing in the search bar. Filter by sheet number, name, or any custom parameter.

**Stays out of your way**  
The panel is modeless — it stays open while you work. Switch views, open families, navigate the model — EliteSheets keeps running in the background.

**Dark and light theme**  
Toggle between dark and light mode to match your workspace preference.

**Clear error reporting**  
If any sheet fails to export, you'll see exactly which ones and why — no silent failures.

---

## Compatibility

- Revit 2024
- Revit 2026

---

## License

See [LICENSE.txt](LICENSE.txt).

---

## Features

- **Multi-format export** — PDF, DWG, and DXF in a single operation
- **Smart sheet grouping** — sheets following the `-7-XX_` naming convention are automatically grouped and merged into a single DXF using a template file
- **DXF template merging** — merged sheets are inserted side-by-side into a configurable DXF template via `netDxf`
- **Sheet filtering & search** — filter the sheet list by number, name, or any parameter directly in the UI
- **Dark / Light theme** — toggle between Material Design themes at runtime
- **Single-instance modeless window** — the panel stays open while you work in Revit; re-clicking the button restores the existing window
- **Per-session logging** — errors are written to `%AppData%\RKTools\Logs\EliteSheets_YYYYMMDD.log`
- **User-facing error reporting** — failed DWG exports are collected and shown in a summary dialog on completion

---

## Supported Revit Versions

| Revit Version | Target Framework |
|---|---|
| Revit 2024 | `net48` |
| Revit 2026 | `net8.0-windows` |

---

## Prerequisites

- [.NET SDK 8+](https://dotnet.microsoft.com/download) (for building)
- Revit 2024 or Revit 2026 installed (for running)
- Revit API NuGet packages are resolved automatically via [Nice3point.Revit.Api](https://www.nuget.org/packages/Nice3point.Revit.Api.RevitAPI) — **no local Revit DLL paths required to build**

---

## Building

```bash
# Debug
dotnet build NameDrawings/EliteSheets.csproj -c Debug

# Release
dotnet build NameDrawings/EliteSheets.csproj -c Release
```

The project targets both `net48` and `net8.0-windows`. The `BaseOutputPath` in the project file points to the developer's machine; override it locally if needed:

```bash
dotnet build NameDrawings/EliteSheets.csproj -c Release /p:BaseOutputPath=./bin
```

Dependencies (MaterialDesignThemes, netDxf, Newtonsoft.Json, etc.) are embedded into the output DLL via **Costura.Fody** — no additional files need to be deployed alongside the add-in.

---

## Installation

1. Build in Release mode for the target framework (`net48` for Revit 2024, `net8.0-windows` for Revit 2026).
2. Copy the output `EliteSheets.dll` to your Revit add-ins folder:
   - `%APPDATA%\Autodesk\Revit\Addins\2024\` (Revit 2024)
   - `%APPDATA%\Autodesk\Revit\Addins\2026\` (Revit 2026)
3. Create a `.addin` manifest file pointing to the DLL (if not already present).
4. Launch Revit — the **RK Tools → Tools → Jooniste Printimine** button will appear on the ribbon.

---

## Configuration

Optional settings can be placed at:

```
C:\ProgramData\RK Tools\EliteSheets\config.json
```

| Key | Type | Description |
|---|---|---|
| `TemplateDxfPath` | `string` | Absolute path to the DXF template used for merged group exports |

Example:

```json
{
  "TemplateDxfPath": "C:\\Templates\\drawing_template.dxf"
}
```

---

## Sheet Grouping Convention

Sheets are automatically split into **singles** and **groups** based on their sheet number:

- **Group pattern**: `{prefix}-7-{groupId}_{title}--{order}`  
  e.g. `A-7-05_Floor Plan--1`, `A-7-05_Floor Plan--2`
- All sheets sharing the same `groupId` are merged side-by-side into a single DXF output.
- Sheets that don't match the pattern are exported individually.

---

## Logging

Runtime errors are logged to:

```
%AppData%\RKTools\Logs\EliteSheets_YYYYMMDD.log
```

Each entry includes a timestamp, log level (`INFO` / `ERROR`), message, and full exception stack trace where applicable.

---

## Dependencies

| Package | Purpose |
|---|---|
| `Nice3point.Revit.Api.*` | Revit API references (compile-time only) |
| `MaterialDesignThemes` | WPF UI theming |
| `netDxf` | DXF reading, writing, and merging |
| `Newtonsoft.Json` | JSON config parsing |
| `Costura.Fody` | Embed all dependencies into the output DLL |
| `ricaun.Revit.UI` | Ribbon button registration helpers |

---

## License

See [LICENSE.txt](LICENSE.txt).