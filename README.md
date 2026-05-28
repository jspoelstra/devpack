# devpack

**Your personal curated APM skill pack.**

This repository is the single source of truth for skills, plugins, and AI coding guidelines you like to use across projects. You curate everything here (pulling from the `awesome-copilot` marketplace + your own local plugins), then bootstrap new projects from it quickly.

## How to Bootstrap a New Project

This is the main intended workflow.

### Recommended Approach: Copy the Manifest

1. **In the new project**, copy the following from `devpack`:

   - `apm.yml` (the curated list of dependencies)
   - `plugins/` (only needed if you want your local plugins, such as `karpathy-principles`)

2. **(Optional but recommended)** Also copy useful root files:
   - `AGENTS.md`
   - `.github/copilot-instructions.md`

3. Run the install:

   ```bash
   cd your-new-project
   apm install --target agent-skills
   ```

   APM will:
   - Resolve all dependencies declared in the copied `apm.yml`
   - Download marketplace plugins into `apm_modules/`
   - Deploy skills into `.agents/skills/`
   - Create/update `apm.lock.yaml`

4. Commit in the new project (typical files to commit):
   - `apm.yml`
   - `apm.lock.yaml`
   - `.agents/skills/` (the deployed skills)
   - `AGENTS.md` and `.github/copilot-instructions.md` (if copied)

   Do **not** commit `apm_modules/`.

### Alternative: Cherry-pick Specific Plugins

Instead of copying the entire `apm.yml`, you can reference individual plugins from this repo directly (including local ones via git path):

```yaml
dependencies:
  apm:
    - github/jacobspoelstra/devpack/plugins/karpathy-principles
    - github/awesome-copilot/plugins/context-engineering
    - github/awesome-copilot/plugins/doublecheck
    # ... pick only what you want
```

This is great when you only need a subset.

### Minimal Example for a Brand New Project

```bash
mkdir my-new-project && cd my-new-project
# Copy apm.yml (and plugins/ if desired) from ~/git/devpack
apm install --target agent-skills
```

## Currently Curated in This Pack

Skills deployed to `.agents/skills/` (available via `/skills` in Grok and auto-discovery):

| Skill                  | Source                              | Description |
|------------------------|-------------------------------------|-----------|
| `karpathy-principles`  | Local (`plugins/karpathy-principles`) | Karpathy-inspired coding discipline (Simplicity First, Surgical Changes, Think Before Coding, Goal-Driven Execution) |
| `context-map`          | context-engineering                 | Generate a map of relevant files before making changes |
| `refactor-plan`        | context-engineering                 | Create a concrete plan before multi-file refactors |
| `what-context-needed`  | context-engineering                 | Ask what files/context are needed before answering |
| `doublecheck`          | doublecheck                         | Three-layer verification to catch hallucinations |

Plus supporting files:
- `AGENTS.md` (root-level instructions for all AI tools)
- `.github/copilot-instructions.md` (GitHub Copilot in VSCode)
- `plugins/karpathy-principles/` — a full APM plugin with skill + ready-to-copy templates

## Adding More Skills

While working in `devpack` (or any project):

```bash
# Discover
apm search "keyword@awesome-copilot"
apm marketplace browse awesome-copilot

# Inspect
apm view some-plugin@awesome-copilot

# Install into this pack (it will be added to apm.yml)
apm install some-plugin@awesome-copilot --target agent-skills
```

After adding, commit the updated `apm.yml` and `apm.lock.yaml`.

You can also create your own local plugins under `plugins/` (see `plugins/karpathy-principles/` as an example).

## Keeping Projects in Sync

- When you improve `devpack`, just copy the latest `apm.yml` (and `plugins/` if changed) into existing projects and re-run `apm install`.
- Or update individual lines in a project's `apm.yml` to point at newer commits of plugins from this repo.
- Use `apm update` in projects when you want to pull the latest resolved versions.

## Useful APM Commands

```bash
apm install --target agent-skills     # Main command to populate skills
apm update                            # Update dependencies to latest matching refs
apm list                              # See what is installed
apm compile                           # Generate AGENTS.md / copilot-instructions etc. for multiple tools
apm pack                              # Create distributable bundles from plugins
```

## Repository Layout

| Path                        | Purpose                              | Commit?          |
|----------------------------|--------------------------------------|------------------|
| `apm.yml`                  | The curated manifest (source of truth) | Yes             |
| `plugins/`                 | Your own local APM plugins           | Yes             |
| `AGENTS.md`, `.github/...` | Instruction files for AI tools       | Yes             |
| `apm.lock.yaml`            | Resolved dependency lockfile         | Yes             |
| `.agents/skills/`          | Deployed skills (managed by APM)     | Yes             |
| `apm_modules/`             | Downloaded plugin source             | **No**          |

## Future Improvements

- Publish `devpack` (or individual plugins) to a marketplace so new projects can depend on `github/jacobspoelstra/devpack` directly without copying files.
- Add more local plugins for things you use frequently (e.g. review workflows, project-specific agents, etc.).

This repo exists so you never have to re-explain or re-collect your favorite skills from scratch when starting something new.

