# AGENTS.md — multiplayer-fabric-humanoid-project

Guidance for AI coding agents working in this submodule.

## What this is

Godot 4 project containing humanoid character rigs, IK skeletons, and
related tooling for the V-Sekai avatar pipeline. Used as a reference and
test bed for humanoid bone structure, constraints, and animation.

## Running

Import in the V-Sekai custom Godot editor fork (`multiplayer-fabric-godot`).

```sh
godot --headless --quit     # smoke test
```

## Key files

| Path | Purpose |
|------|---------|
| `project.godot` | Godot 4 project config |
| `humanoid/` | Humanoid rig scenes and scripts |
| `addons/` | Any addons bundled with the project |

## Conventions

- Requires the custom Godot fork.
- GDScript files need SPDX headers:
  ```gdscript
  # SPDX-License-Identifier: MIT
  # Copyright (c) 2026 K. S. Ernest (iFire) Lee
  ```
- Commit message style: sentence case, no `type(scope):` prefix.
  Example: `Adjust spine twist weight for shoulder abduction`
