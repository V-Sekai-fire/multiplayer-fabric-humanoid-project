# Contributing

A Godot 4 project providing humanoid avatar support via the VRM addon
and MToon shader.  The project is the reference integration for
VRM-spec avatars inside the multiplayer-fabric runtime — it is both a
testbed and a distributable addon bundle.

## Guiding principles

- **Editor-first testing.** There is no headless build step; validate
  changes by opening the project in the Godot editor and running the
  included test scenes.  Automated checks that can run headless
  (`godot --headless --quit`) are preferred for CI.
- **Addon isolation.** Each addon under `addons/` must function
  independently.  Do not introduce cross-addon `preload` calls; use
  signals or autoloads for cross-cutting concerns.
- **VRM spec compliance.** Avatar loading must pass the
  [VRM validation tool](https://hub.vroid.com/en/engine) for any VRM
  0.x or 1.x file.  Do not add fields or behaviours that conflict with
  the published VRM specification.
- **MToon shader parity.** Shader output should match the reference
  Unity MToon implementation for the same material parameters.  When in
  doubt, match Unity's render output, not a simplified approximation.
- **No binary assets in PRs.** Commit only `.gd`, `.tres`, `.tscn`,
  `.gdshader`, and `.import` metadata files.  Large binary assets
  (meshes, textures) are distributed separately and referenced by path.

## Workflow

1. Open the project in Godot 4.x (matching the `project.godot`
   `config/features` version).
2. Make your change in the relevant addon or scene.
3. Run the `test/` scenes headless:
   ```
   godot --headless --path . --quit-after 5 test/run_tests.tscn
   ```
4. Confirm no errors in the Godot output log.
5. Commit with message `addon(<name>): <description>` or
   `scene(<name>): <description>`.

## Design notes

### VRM loading pipeline

VRM files are loaded via the `addons/vrm/` addon which registers a
custom `ResourceFormatLoader`.  The loader parses the GLB binary,
extracts VRM JSON metadata, and constructs a Godot scene tree with
`BoneAttachment3D` nodes per skeleton bone.  MToon materials are
converted to `addons/Godot-MToon-Shader/` `ShaderMaterial` instances at
load time — do not convert them to `StandardMaterial3D`.

### Spring bone simulation

Secondary animation (hair, clothing) uses the VRM spring bone extension.
Spring bone nodes are updated in `_physics_process` at a fixed timestep.
Do not update them in `_process` — doing so makes the simulation
frame-rate dependent.
