---
name: freecad-forge-mcp
description: Controls FreeCAD 3D parametric CAD modeling, scripting, FEM analysis, and geometry exports via the freecad-mcp server and XML-RPC addon. Use when the user asks to create 3D CAD models, automate FreeCAD, run CAD scripts, inspect parts, or execute FEM analysis.
---

# FreeCAD MCP Integration & Automation Hub

Provides comprehensive control of FreeCAD parametric 3D CAD modeling, headless batch execution, and interactive GUI manipulation via `freecad-mcp` and the FreeCAD RPC addon.

## Scope & Trigger Context

- **Trigger Keywords**: FreeCAD, CAD, 3D parametric modeling, STEP/STL export, Part workbench, PartDesign, Sketcher, FreeCAD script, FreeCAD FEM analysis, `freecad-mcp`.
- **MCP Server Name**: `freecad` (configured in `Antigravity global mcp_config.json`).

## Verified Paths, Commands & Schemas

- **FreeCAD 1.1 Executables**:
  - GUI: `C:\Program Files\FreeCAD 1.1\bin\FreeCAD.exe`
  - Headless CLI: `C:\Program Files\FreeCAD 1.1\bin\FreeCADCmd.exe`
- **FreeCAD Addon Mod Directories**:
  - FreeCAD 1.1: `%APPDATA%\FreeCAD\v1-1\Mod\FreeCADMCP\`
  - FreeCAD unversioned / fallback: `%APPDATA%\FreeCAD\Mod\FreeCADMCP\`
  - **Required Structure**: Directory must contain `InitGui.py` directly (e.g. `%APPDATA%\FreeCAD\v1-1\Mod\FreeCADMCP\InitGui.py`).
- **Antigravity Global MCP Entry** (`Antigravity global mcp_config.json`):
  ```json
  "freecad": {
    "command": "%USERPROFILE%\\.local\\bin\\uvx.exe",
    "args": [
      "freecad-mcp",
      "--freecadcmd",
      "C:\\Program Files\\FreeCAD 1.1\\bin\\FreeCADCmd.exe"
    ]
  }
  ```
- **RPC Listener Port**: `127.0.0.1:9875` (XML-RPC).

## Proven Fast-Path Workflows

### 1. Activating the Addon in FreeCAD GUI
1. Launch FreeCAD GUI (`FreeCAD.exe`).
2. Switch workbench dropdown to **MCP Addon**.
3. In the toolbar, click **Start RPC Server**.
4. FreeCAD Report View displays: `RPC server listening on 127.0.0.1:9875`.
5. *(Optional Auto-Start)*: In FreeCAD, go to **Edit → Preferences → MCP Addon** and enable **Auto-start RPC server on startup**.

### 2. Verification Commands
- Check TCP port listener:
  ```powershell
  Test-NetConnection -ComputerName 127.0.0.1 -Port 9875
  ```
- Check XML-RPC ping from Python:
  ```powershell
  py -3.12 -c "import xmlrpc.client; s = xmlrpc.client.ServerProxy('http://127.0.0.1:9875'); print(s.ping()); print(s.get_rpc_status())"
  ```

### 3. Core MCP Tool Operations
- `list_documents()`: Lists all open documents in FreeCAD.
- `create_document(name)`: Creates a new document.
- `create_object(type, name, doc_name, ...)`: Creates parametric shapes (`Part::Box`, `Part::Cylinder`, etc.).
- `edit_object(name, doc_name, ...)`: Updates dimensions, placements, properties.
- `get_objects(doc_name)` / `get_object(name, doc_name)`: Inspects geometric hierarchy and attributes.
- `get_view(...)`: Captures high-resolution viewport screenshots from active FreeCAD window.
- `execute_code(code)`: Runs Python code synchronously in the active FreeCAD session.
- `execute_code_headless(code, timeout)`: Runs batch scripts via `FreeCADCmd.exe` without GUI.
- `run_fem_analysis(...)`: Executes FEA simulation pipelines on solids.

## Dead-Ends, Pitfalls & Verified Fixes

- `[WinError 10061 / Connection Refused]` -> FreeCAD GUI is closed or the "Start RPC Server" button was not clicked. -> Verify FreeCAD is running and port 9875 is listening.
- `[Subdirectory nesting in Mod]` -> Extracting `freecad-mcp` root into `Mod` makes FreeCAD miss `InitGui.py`. -> The directory inside `Mod` must be named `FreeCADMCP` and contain `InitGui.py` directly at top level.
- `[Windows PowerShell execution policy blocks .ps1 shims]` -> Calling `uvx.ps1` fails with execution policy error. -> Call `C:\Users\hando\.local\bin\uvx.exe` directly.
- `[FreeCAD not in PATH]` -> Headless execution fails when `FreeCADCmd` cannot be located. -> Explicitly supply `--freecadcmd "C:\Program Files\FreeCAD 1.1\bin\FreeCADCmd.exe"` in MCP args.

## Token-Saving Shortcuts

- Always check `get_rpc_status()` before executing complex GUI operations.
- For non-interactive batch geometry generation or CAD export, prefer `execute_code_headless` to avoid GUI overhead.
- When generating complex geometry, write modular Python scripts using `Part`, `PartDesign`, or `Draft` modules and run via `execute_code`.
