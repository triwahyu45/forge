# FORGE — FreeCAD 3D Parametric CAD & FEM Automation

AI Agent Skill for **Antigravity**, **Claude Code**, and **OpenAI Codex**. Controls FreeCAD 1.1 parametric 3D CAD modeling, headless Python scripting, FEM analysis, and STEP/STL exports via `freecad-mcp` and XML-RPC.

---

## Quick Install (Claude Code / Codex / Antigravity)

Clone this skill into your agent's global or project skills directory:

| Agent | Global Install Path | Project-Local Path |
| :--- | :--- | :--- |
| **Antigravity** | `~/.gemini/config/skills/FORGE` | `.agents/skills/FORGE` |
| **Claude Code** | `~/.claude/skills/FORGE` | `.claude/skills/FORGE` |
| **OpenAI Codex** | `~/.codex/skills/FORGE` | `.agents/skills/FORGE` |

```bash
# Example (Antigravity / Claude Code / Codex — pick your path):
git clone https://github.com/triwahyu45/FORGE.git ~/.claude/skills/FORGE
```

---

## Requirements & Dependencies (For Agents)

- **Runtime / Tools**: **FreeCAD 1.1** (`FreeCAD.exe` / `FreeCADCmd.exe`), **`uv` / `uvx`**, and the **FreeCADMCP** addon installed in `%APPDATA%\FreeCAD\v1-1\Mod\FreeCADMCP\`
- **MCP Servers**: Required: `freecad` (`freecad-mcp` via `uvx`, XML-RPC on `127.0.0.1:9875`)
- **Related Skills**: Pairs with [`titan`](https://github.com/triwahyu45/titan) for CAD-to-MuJoCo robotics simulation
- **API Keys / Secrets**: None

```json
{
  "mcpServers": {
    "freecad": {
      "command": "uvx",
      "args": ["freecad-mcp", "--freecadcmd", "C:\\Program Files\\FreeCAD 1.1\\bin\\FreeCADCmd.exe"]
    }
  }
}
```

---

## Included Files

- `SKILL.md` — Verified FreeCAD paths, MCP setup, headless/GUI workflows, & `get_view` verification gate

## When to Trigger / Search Keywords

`forge`, `freecad`, `cad`, `3d parametric`, `step export`, `stl export`, `fem analysis`, `partdesign`, `sketcher`
