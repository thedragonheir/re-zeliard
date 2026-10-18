# Repository Guidelines

## Project Overview

This repository contains an unofficial C++23 and SDL3 port and reimplementation of **Zeliard**, originally released for DOS in 1989.

The project focuses on faithfully recreating the original game behavior, rendering, timing, data formats, music and gameplay systems.

Preservation, reverse engineering and technical understanding take priority over modernization, convenience or visual polish.

The following sources are primary reference material:

- Original assembly sources
- Original game binaries and data files
- Extracted graphics and other assets
- Original music captures
- Existing reverse-engineering documentation
- Verified behavior from the DOS version

Do not replace confirmed original behavior with assumptions or modern conventions.

Accuracy comes first, polish later.

## Project Structure and Module Organization

Keep game code under `src/`, divided by domain and original game responsibility.

Current source areas include:

- `src/town/` for town behavior, actors, rendering, movement, transitions, NPCs and dialogs
- `src/mdt/` for MDT parsing and town data structures
- `src/grp/` for GRP decompression, decoding and graphics resources
- `src/mcga/` for palette handling and MCGA drawing behavior
- `src/hud/` for HUD elements, equipment panels and status rendering
- `src/zeliard.cpp` for the current application entry point, SDL startup and main loop
- `src/zeliard.hpp` for shared application state and declarations

Repository layout:

```text
AGENTS.md   Repository workflow and contribution guidelines
README.md   Public project overview, status, build information and roadmap
asm/        Original assembly sources and reverse-engineering references
assets/     Project assets, runtime resources, music sources and reference data
docs/       Technical documentation, format research and fidelity notes
game/       Original game data and binaries used by the runtime and tools
src/        C++23 source code
tools/      Python tools for inspecting and extracting original game data
web/        Browser shell and support files for the WebAssembly build
out/        Generated CMake build output
```

Do not move folders unless explicitly requested.

Do not delete files unless explicitly requested.

Do not rename or reorganize original data files unless explicitly requested.

Treat `asm/`, `tools/`, `game/` and `assets/` as active reference and data directories, not as unused clutter.

Do not edit generated files under `out/` manually.

Do not accidentally compile unrelated reference code, including BR0X or other external reference implementations.

## Build, Test and Development Commands

Run build commands from the repository root.

### Native Debug Build

```text
cmake --preset x64-debug
cmake --build --preset x64-debug
out\build\x64-debug\Zeliard.exe
```

### Native Release Build

```text
cmake --preset x64-release
cmake --build --preset x64-release
```

### WebAssembly Build

```text
cmake --preset Website
cmake --build --preset Website
```

There is currently no automated test suite.

Validate C++ changes by rebuilding the affected target and launching the application.

Validate Python tools by running the affected script against representative files from `game/` or `assets/`.

Validate web changes by building the Website preset and loading the generated browser version.

Keep generated files, build artifacts and temporary extraction output out of commits.

## Coding Style and Naming Conventions

Use C++23 and SDL3.

Use 4-space indentation.

Put braces on their own lines.

Prefer compact and readable control flow.

Use PascalCase for project-owned constants, variables, functions, classes and other identifiers where practical.

Examples:

```text
WindowWidth
InternalHeight
WindowTitle
IsRunning
GrpLoader
BinaryReader
```

Keep source filenames lowercase.

Examples:

```text
binary_reader.cpp
grp_unpack.cpp
town_render.cpp
mcga_palette.cpp
```

Do not rename SDL, CMake, standard library or other external API symbols.

Do not rename existing files or identifiers solely to enforce a newer naming preference unless the task explicitly requires it.

Prefer clear names over convention-only abbreviations.

Keep comments short and useful.

Comments should explain why code exists, especially when documenting:

- Reverse-engineered behavior
- Original timing constraints
- Binary format quirks
- Non-obvious compatibility behavior
- Assembly-backed constants
- Intentional differences from modern conventions

Do not add obvious, repetitive or tutorial-style comments.

Follow the language of the existing comments in the file.

Never write comments fully in uppercase.

Avoid compiler warnings.

Pay particular attention to Windows entry-point declarations and inconsistent annotation warnings.

## Scope and Change Discipline

Keep changes narrowly scoped to the requested task.

Avoid broad rewrites unless explicitly requested.

Avoid unrelated:

- Refactoring
- Renaming
- Formatting changes
- Folder changes
- Architecture changes
- Dependency changes
- Documentation rewrites

Do not invent engine systems before the current task requires them.

Do not create large abstractions for behavior that can currently be implemented clearly with simple code.

Prefer simple, working and verifiable C++ over premature architecture.

Use RAII where it provides a clear ownership or cleanup benefit.

Do not over-engineer.

Do not modernize original behavior randomly.

First understand how Zeliard works, then rebuild that behavior cleanly in C++23 and SDL3.

## Reverse-Engineering Rules

Treat the original assembly as the primary behavioral reference.

Before changing gameplay, rendering, timing, decoding or data handling:

1. Inspect the relevant assembly.
2. Inspect the current C++ implementation.
3. Inspect related documentation.
4. Inspect representative original data.
5. Compare against verified DOS behavior where possible.
6. Preserve established behavior unless the task explicitly requires a change.

Do not guess binary offsets, record layouts or decoded structures when assembly evidence is available.

For binary decoding work:

- Distinguish `ORG` values and segment labels from real file offsets
- Confirm source offsets
- Confirm decoded sizes
- Confirm end offsets
- Confirm record counts
- Confirm unsigned versus signed byte semantics
- Account for implicit instruction behavior such as `lodsb`, `lodsw`, `stosb`, `rep` and `loop`
- Confirm pointer bases and runtime address conversions
- Log or document important offsets and decoded byte counts when useful

Before translating assembly behavior into C++:

- Express the behavior as clear pseudo-code
- Confirm the relationship between labels, runtime addresses and file offsets
- Confirm update order where it affects visible behavior
- Confirm integer overflow or wrapping behavior where relevant
- Avoid assuming that assembly labels represent direct file positions

Clearly distinguish between:

- Confirmed assembly-backed behavior
- Behavior observed in the original executable
- Strong inference
- Unverified assumptions

Accuracy has priority over implementation convenience.

## Rendering and Timing

Keep the internal logical resolution at:

```text
320x200
```

Use SDL3 as the platform, rendering, input and future audio layer.

Do not introduce a large game engine.

Keep gameplay state integer-based where the original behavior depends on integer or fixed-step updates.

Render interpolation must remain visual only.

Never feed interpolated positions back into:

- Gameplay state
- Collision detection
- Map transitions
- Actor logic
- Input handling
- Camera thresholds

Prefer original DOS timing and behavior over modernized movement or animation.

Use nearest-neighbor presentation for pixel art unless the task explicitly requires another method.

Do not add filtering, smoothing, anti-aliasing or fractional source rendering that damages the original pixel structure.

The desktop presentation should preserve the intended corrected proportions.

Do not stretch the game to widescreen by default.

Fullscreen and resized-window behavior must preserve the complete logical frame.

## Original Data and Asset Policy

Preserve and use the original `.SAR`, `.GRP`, `.MDT` and related data formats.

Do not replace original assets with approximations when the original data can be decoded, extracted or reconstructed.

Original files remain reference material.

Generated or reconstructed assets should preserve the original behavior and appearance as closely as possible.

Do not convert original data into modern formats merely for convenience when direct runtime decoding remains practical.

Do not move, rename, modify or reorganize original data files unless explicitly requested.

Do not commit copyrighted original game data to public distributions unless its redistribution is legally permitted.

Code should fail clearly when required original data is missing or invalid.

## Music Assets

Original music research and editable reconstructions are stored under:

```text
assets/music/src/
```

Use lowercase filenames with underscores.

Preserve the track number and full official track title.

Use the same basename for all source files belonging to the same track.

Example:

```text
01_radiance_of_esmeranti.vgz
01_radiance_of_esmeranti.txt
01_radiance_of_esmeranti_p.txt
01_radiance_of_esmeranti.fur
```

File meanings:

- `.vgz` contains an original compressed VGM capture
- `.txt` contains a VGM text export or technical analysis
- `_p.txt` contains AdLib or OPL patch analysis
- `.fur` contains an editable Furnace Tracker project

The `_p` suffix means patches.

Do not modify original `.vgz` captures.

Store editable music reconstructions as Furnace `.fur` project files.

Runtime-ready music resources belong directly under:

```text
assets/music/
```

Music reconstruction should preserve:

- YM3812 register behavior
- Instrument parameters
- Note timing
- Rhythm behavior
- Vibrato
- Loops
- Tempo
- Channel allocation

Do not replace original compositions with approximate arrangements when accurate reconstruction remains possible.

## Documentation

Use lowercase `docs/`.

Do not create `Docs/`.

Document reverse-engineering findings while rebuilding the game.

Update relevant documentation whenever conclusions change concerning:

- Behavior
- Formats
- Binary offsets
- Record layouts
- Timing
- Rendering
- Palette handling
- Actor behavior
- Fidelity
- Build behavior
- Platform support

Keep documentation factual and focused.

Clearly distinguish confirmed assembly-backed behavior from inference or uncertainty.

Do not present experimental assumptions as established facts.

Use exact paths, offsets and identifiers where they materially help future verification.

## README Maintenance

`README.md` is the public overview of the repository and must not fall behind the implementation.

After every substantial project update, review `README.md` and update the affected sections in the same task.

A README review is required when changing any of the following:

- Implemented or incomplete gameplay systems
- Current project status
- Supported controls
- Build commands or CMake presets
- Platform support
- WebAssembly behavior
- Logical or presentation resolution
- Fullscreen behavior
- Required original data
- Project structure
- Major dependencies
- Music support
- Runtime capabilities
- Public preview links
- Roadmap progress
- Important legal or distribution requirements

Do not leave completed features listed as unimplemented.

Do not describe removed, renamed or replaced systems as current.

Do not copy internal implementation details into the README unless they are useful to repository users.

Small internal fixes that do not affect the public project description do not require a README edit.

When completing a substantial task:

1. Review the current README.
2. Identify sections made inaccurate by the change.
3. Update only the affected sections.
4. Keep the README concise, professional and generally readable.
5. Report whether the README was updated.

If no README update is necessary after a substantial internal change, state briefly why the public documentation remains accurate.

## Testing Guidelines

Testing is currently primarily manual.

### Gameplay Changes

- Rebuild the native target
- Launch the game
- Reach the affected scene or flow
- Verify the intended behavior
- Verify nearby and unrelated behavior for regressions
- Compare against the original DOS behavior where possible

### Rendering Changes

- Verify the result on screen
- Compare against original reference material
- Check logical pixel alignment
- Check clipping
- Check palette indices
- Check draw order
- Check fullscreen and resized-window presentation where relevant
- Record remaining visual differences honestly

### Timing Changes

- Confirm fixed-step behavior
- Confirm update order
- Confirm that frame rate does not alter gameplay speed
- Confirm that catch-up limits do not create persistent drift
- Confirm that render interpolation remains presentation-only

### Tool Changes

- Run the affected Python script
- Use representative original data
- Confirm input handling
- Confirm decoded sizes and offsets
- Confirm generated filenames
- Confirm visible output where applicable
- Confirm malformed data produces a useful failure

### Web Changes

- Build the Website preset
- Load the generated browser build
- Verify startup
- Verify input
- Verify asset loading
- Verify the main gameplay flow currently supported
- Check more than one browser when the change could be browser-specific

Report validation honestly.

Do not claim success without evidence.

Do not claim a build was tested when it was not run.

## Commit and Pull Request Guidelines

Use short, imperative and specific commit messages.

Examples:

```text
Fix startup hero idle frame
Add selectable display aspect mode
Fix town dialog outer corner pixels
```

Keep each commit focused on one logical change.

Do not mix unrelated cleanup into feature or fidelity commits.

Pull requests should include:

- A concise behavior summary
- The files or systems changed
- The original behavior or evidence used
- Validation performed
- Screenshots or visual notes for rendering changes
- Known limitations
- Remaining differences or uncertainties
- Relevant documentation updates
- README updates where required

Do not include generated build output.

Do not include temporary extraction files.

Do not include external tools or binaries unless explicitly approved.

## Agent Reporting

After making changes, report:

- Changed files or assets
- Important behavior changes
- Important renamed identifiers
- Documentation updated
- Whether `README.md` was reviewed or updated
- Validation performed
- Remaining issues or uncertainties

Keep reports concise and specific.

Do not pad reports with unrelated implementation details.

Do not claim validation that was not performed.

## Agent Notes

For all Zeliard work:

- Follow this `AGENTS.md`
- Inspect assembly, source, documentation and original data before changing behavior
- Avoid unrelated changes
- Preserve original formats
- Preserve original source material
- Preserve and analyze the original AdLib and OPL music
- Keep editable music reconstructions as Furnace `.fur` projects
- Keep gameplay behavior independent from render interpolation
- Keep generated files out of commits
- Review `README.md` after every substantial update
- Update `README.md` whenever the public project description would otherwise become outdated
- Prefer accuracy first, polish later
