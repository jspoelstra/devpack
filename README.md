# devpack

**Your personal curated APM skill pack.**

This repository is the single source of truth for skills, plugins, and AI coding guidelines you like to use across projects. You curate everything here (pulling from the `awesome-copilot` marketplace + your own local plugins), then bootstrap new projects from it quickly.

## How to Bootstrap a New Project

This is the main intended workflow.

### Recommended Approach: Copy the Manifest

1. **In the new project**, copy the following from `devpack`:

   - `apm.yml` (the curated list of dependencies)
   - `plugins/` (only needed if you want your local plugins, such as `karpathy-principles` or `microsoft-foundry-agents`)

2. **(Optional but recommended)** Copy the generic agent guidelines templates:
   - `plugins/karpathy-principles/templates/AGENTS.md` → `AGENTS.md`
   - `plugins/karpathy-principles/templates/copilot-instructions.md` → `.github/copilot-instructions.md`

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
    - github/jacobspoelstra/devpack/plugins/microsoft-foundry-agents
    - github/jacobspoelstra/devpack/plugins/anthropic-react-frontend
    - github/jacobspoelstra/devpack/plugins/azure-infrastructure-architecture
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
| `microsoft-foundry`    | Local (`plugins/microsoft-foundry-agents`) | Microsoft AI Foundry router + hosted agents, evaluation/observability, model deployment |
| `frontend-design`      | Local (`plugins/anthropic-react-frontend`) | Anthropic skill for distinctive, production-grade React + modern frontend (avoids AI slop) |
| `cloud-solution-architect` | Local (`plugins/azure-infrastructure-architecture`) | Well-Architected Framework, architecture styles, design patterns, and technology selection |

Plus supporting files:
- `AGENTS.md` (root-level instructions for all AI tools)
- `.github/copilot-instructions.md` (GitHub Copilot in VSCode)
- `plugins/karpathy-principles/` — a full APM plugin with skill + ready-to-copy templates
- `plugins/microsoft-foundry-agents/` — Curated Microsoft AI Foundry agent skills (official microsoft-foundry router + hosted agents, observability, and models)
- `plugins/anthropic-react-frontend/` — Anthropic skills for high-quality React/frontend development (`frontend-design`, `web-artifacts-builder`)
- `plugins/azure-infrastructure-architecture/` — Azure Bicep, containers (AKS), and architecture skills (`cloud-solution-architect`, `azure-prepare`, `azure-kubernetes`)

## Microsoft AI Foundry Agent Skills

This pack includes a curated selection of official **Microsoft AI Foundry** (Azure AI Foundry) agent skills, packaged as a local APM plugin.

**Source:** Curated from the excellent official collection at [microsoft/skills](https://github.com/microsoft/skills) (specifically the `azure-skills` plugin).

### Included Skills

| Skill                    | Purpose |
|--------------------------|---------|
| `microsoft-foundry`      | Main router skill. Intelligently maps your intent to the right sub-skill, discovery steps, and MCP tools. Excellent starting point for almost all Foundry work. |
| `foundry-hosted-agents`  | Building, deploying, and managing containerized hosted agents (using Microsoft Agent Framework, LangGraph, custom code, etc.). |
| `foundry-observability`  | Evaluation, tracing, batch + continuous evaluation, prompt optimization, and production monitoring. |
| `foundry-models`         | Model discovery, preset & custom deployments, capacity planning, and quota management. |

These skills are especially powerful for end-to-end agent development on Foundry (create → deploy → invoke → observe/optimize loops).

### Usage

After installing the plugin (`apm install ./plugins/microsoft-foundry-agents --target agent-skills`), the skills become available via:

- `/skills microsoft-foundry`
- `/skills foundry-hosted-agents`
- etc.

The `microsoft-foundry` router is designed to be loaded first in most cases — it will guide you to the correct detailed sub-skill and mandatory pre-checks.

### Why Curated?

The full Microsoft Foundry skill set is very comprehensive. This plugin includes the highest-signal skills for typical agent development workflows while keeping context manageable.

You can request additional skills from the Microsoft collection (e.g. `finetuning`, `foundry-governance`, `foundry-memory`, etc.) to be added to this plugin.

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

You can also create your own local plugins under `plugins/` (see `plugins/karpathy-principles/`, `plugins/microsoft-foundry-agents/`, `plugins/anthropic-react-frontend/`, and `plugins/azure-infrastructure-architecture/` as examples).

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
- Expand the `microsoft-foundry-agents` plugin with additional official Foundry skills (fine-tuning, memory, governance, toolboxes, etc.).
- Expand `anthropic-react-frontend` with more React/design system skills from Anthropics.
- Expand `azure-infrastructure-architecture` with more Bicep patterns, Azure AI Search skills, and deployment skills (`azure-deploy`, `azure-validate`).
- Add more local plugins for things you use frequently (e.g. review workflows, project-specific agents, etc.).

This repo exists so you never have to re-explain or re-collect your favorite skills from scratch when starting something new.

