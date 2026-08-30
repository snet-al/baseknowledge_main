# Install the SNET Codex Worldview

## 1. Install the global worldview

```bash
mkdir -p ~/.codex
cp AGENTS.md ~/.codex/AGENTS.md
```

Make sure a global override is not replacing it unintentionally:

```bash
ls -la ~/.codex/AGENTS*
```

If `~/.codex/AGENTS.override.md` exists and is non-empty, Codex uses that file instead of `~/.codex/AGENTS.md`.

## 2. Reinforce the worldview on every user prompt

The hook is optional but recommended for the requirement that every new prompt re-enters the worldview gate.

```bash
mkdir -p ~/.codex/hooks
cp .codex/hooks/snet_worldview_turn.py ~/.codex/hooks/snet_worldview_turn.py
cp .codex/hooks.json ~/.codex/hooks.json
chmod +x ~/.codex/hooks/snet_worldview_turn.py
```

Open Codex and review/trust the hook with `/hooks` when prompted.

## 3. Increase the AGENTS.md instruction budget

Merge `config.toml.snippet` into `~/.codex/config.toml`:

```toml
project_doc_max_bytes = 65536

[features]
hooks = true
```

Do not duplicate `[features]` if it already exists; merge `hooks = true` into the existing table.

## 4. Keep repository instructions concise

Use each repository's root `AGENTS.md` only for facts and conventions specific to that repository, for example:

- Commands to build, lint, and run the project.
- Canonical feature and module locations.
- Repository-specific naming vocabulary.
- Framework and dependency conventions already selected.
- Domain invariants and prohibited changes.
- Nested overrides for specialized modules.

The global file defines how Codex sees software. Repository files describe the concrete software it is seeing.

## 5. Verify

Start a new Codex session and run:

```bash
codex --ask-for-approval never "Summarize the active SNET worldview priorities and explain the task-map gate."
```

Then inspect the loaded instruction sources:

```bash
codex --ask-for-approval never "List the instruction files active in this session."
```

A practical calibration prompt is:

```text
Add cancellation support to the existing order feature. Before proposing code,
show a concise map of the canonical names, feature location, critical functions,
state transitions, persistence, side effects, and strongest relations you found.
```

Codex should inspect the existing feature closure before recommending a new structure.
