# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Overview

This repository contains the **BMAD (BMad Method)** framework - an AI-driven agile development orchestration system. The framework uses specialized AI agents, workflows, and tasks to manage the complete software development lifecycle through structured, repeatable processes.

**Key Modules:**
- `bmad/core/` - Core BMAD platform with universal agents, tasks, and resources
- `bmad/bmm/` - BMad Method Module with 12 specialized agents and 34+ workflows across 4 development phases
- `bmad/_cfg/` - Configuration, manifests, and agent customization files
- `docs/` - Project output folder for generated artifacts

## Architecture

### Multi-IDE Agent System

BMAD agents are deployed across multiple IDE environments:
- **Claude Code**: Slash commands in `.claude/commands/bmad/`
- **Cursor**: MDC rules in `.cursor/rules/bmad/`
- **Codex**: Custom prompts in `$CODEX_HOME/prompts/bmad-*.md`
- **Windsurf**: Agent configurations in `.windsurf/rules/bmad/`

All agents reference the same source files in `bmad/` directory but use different activation mechanisms per IDE.

### Agent Architecture

**Agent Structure:**
- Each agent is defined in a markdown file with YAML frontmatter and XML persona
- Agents have activation sequences, menus, personas, and workflow handlers
- Agents load configuration from `bmad/core/config.yaml` (or module-specific config)
- Configuration includes: `user_name`, `communication_language`, `output_folder`, `bmad_folder`

**Core Agents:**
- `bmad-master` - Master executor and workflow orchestrator (Core module)

**BMM Agents (12 specialized):**
- `pm` (Product Manager), `analyst`, `architect` (Architect)
- `sm` (Scrum Master), `dev` (Developer), `tea` (Test Engineer)
- `ux-designer`, `tech-writer`
- Game development agents: `game-designer`, `game-developer`, `game-architect`

### Workflow Execution System

**Core Workflow Engine:**
- `bmad/core/tasks/workflow.xml` - The core OS for executing all BMAD workflows
- Workflows are YAML configurations that define steps, resources, templates, and validation
- Agents load workflow.xml and pass the YAML path as `workflow-config` parameter

**Workflow Phases (BMM):**
1. **Phase 0**: Documentation (brownfield projects) - `document-project`
2. **Phase 1**: Analysis (optional) - `brainstorm-project`, `product-brief`, `research`, `domain-research`
3. **Phase 2**: Planning (required) - `prd`, `tech-spec`, `create-ux-design`
4. **Phase 3**: Solutioning (Levels 3-4) - `architecture`, `create-epics-and-stories`, `implementation-readiness`
5. **Phase 4**: Implementation (iterative) - `sprint-planning`, `create-story`, `dev-story`, `code-review`, `story-ready`, `story-done`, `retrospective`
6. **Testing**: Parallel quality workflows - `testarch-*` workflows (ATDD, automation, CI, framework, NFR assessment, etc.)
7. **Diagrams**: Excalidraw diagram creation - `create-excalidraw-diagram`, `create-excalidraw-flowchart`, `create-excalidraw-wireframe`, `create-excalidraw-dataflow`

### Scale-Adaptive System

BMAD automatically adapts to project complexity through three tracks:
- **Quick Flow** (Levels 0-1): Bug fixes and small features using `tech-spec` workflow
- **BMad Method** (Level 2): `prd` with optional architecture
- **Enterprise Method** (Levels 3-4): Full `prd` + comprehensive `architecture`

### Task System

**Core Tasks:**
- `advanced-elicitation.xml` - Structured elicitation using 30+ techniques from CSV knowledge base
- `workflow.xml` - Universal workflow executor (loads YAML, executes steps, saves outputs)
- `validate-workflow.xml` - Run validation checklists against workflow outputs
- `index-docs.xml` - Generate index.md for document directories

### Resources

**Excalidraw Helpers (Core):**
- `bmad/core/resources/excalidraw/` - Universal Excalidraw diagram creation knowledge
- `excalidraw-helpers.md` - Element creation patterns (text width calculation, grouping, arrows, alignment)
- `validate-json-instructions.md` - JSON validation workflow
- Used by all diagram-creating workflows/agents

## Working with BMAD Agents

### Activating Agents in Claude Code

Use slash commands to activate agents:
```
/bmad:core:agents:bmad-master - Activate master orchestrator
/bmad:bmm:agents:dev - Activate development agent
/bmad:bmm:agents:architect - Activate architect agent
/bmad:bmm:workflows:dev-story - Execute dev-story workflow
```

### Agent Activation Sequence

When an agent activates, it follows a critical sequence:
1. Load persona from agent markdown file
2. **IMMEDIATELY** load and read config.yaml (BEFORE any output)
3. Store all config fields as session variables
4. Show greeting using `{user_name}` and communicate in `{communication_language}`
5. Display numbered menu of all available commands
6. Wait for user input (number, command trigger like `*help`, or fuzzy match)
7. Execute selected menu item using appropriate handler

### Menu Handlers

Agents use specialized handlers for different menu item types:

**`workflow` handler:**
- ALWAYS load `bmad/core/tasks/workflow.xml`
- Pass YAML path as `workflow-config` parameter
- Execute workflow.xml instructions precisely
- Save outputs after EACH workflow step (never batch)

**`validate-workflow` handler:**
- Load `bmad/core/tasks/validate-workflow.xml`
- Pass workflow and validation schema (checklist)
- Identify file to validate or ask user

**`action` handler:**
- If `action="#id"`: Find prompt with matching id in agent XML
- If `action="text"`: Execute text as inline instruction

**`exec` handler:**
- Load and execute the file at specified path
- Do NOT improvise

**`data` handler:**
- Load file (JSON/YAML/CSV/XML) first
- Make available as `{data}` variable

### Critical Agent Rules

1. **Config Loading**: Config MUST be loaded at step 2 of activation (before any output)
2. **Communication**: ALWAYS use `{communication_language}` from config
3. **File Loading**: Load files ONLY when executing menu items or workflows require it (exception: config at startup)
4. **Menu Display**: Triggers use asterisk (*) NOT markdown (e.g., `*help`, `*exit`)
5. **Workflow Output**: Written file output is +2sd communication style and uses professional language
6. **Character Consistency**: Stay in character until exit command
7. **File References**: Use `{project-root}` placeholder for absolute paths

## Configuration Files

### Core Configuration
- `bmad/core/config.yaml` - Core module settings
- `bmad/bmm/config.yaml` - BMM module settings (if separate from core)

### Manifests
- `bmad/_cfg/workflow-manifest.csv` - All available workflows (name, description, module, path, standalone)
- `bmad/_cfg/task-manifest.csv` - All available tasks
- `bmad/_cfg/manifest.yaml` - Installation metadata (version, install date, modules, IDEs)

### Agent Customization
- `bmad/_cfg/agents/*.customize.yaml` - Per-agent customization files
- Override default persona, communication style, or menu items

## Key Principles

1. **Runtime Resource Loading**: Load resources at runtime, never pre-load (except config)
2. **Numbered Lists**: Always present numbered lists for user choices
3. **Path Placeholders**: Use `{project-root}` and `{bmad_folder}` for portability
4. **One Step at a Time**: Save outputs after completing EACH workflow step
5. **Direct Execution**: Follow agent instructions exactly - do not improvise
6. **Story-Centric Development**: One story at a time, stories move through defined lifecycle
7. **Just-in-Time Context**: Epic and story context provided exactly when needed
8. **DRY Principle**: Core holds universal knowledge, agents specialize with domain knowledge

## Common Workflows

### Starting a New Project
```
/bmad:bmm:agents:analyst
*workflow-init
```

### Brownfield (Existing Codebase)
```
/bmad:bmm:agents:analyst
*document-project
*workflow-init
```

### Development Sprint
```
/bmad:bmm:agents:sm
*sprint-planning
*create-story
```

```
/bmad:bmm:agents:dev
*dev-story
```

### Multi-Agent Collaboration
```
/bmad:core:agents:bmad-master
*party-mode
```
This orchestrates group discussions between all 19+ installed agents for strategic decisions, creative brainstorming, and complex problem-solving.

### Creating Diagrams
```
/bmad:bmm:workflows:create-diagram
/bmad:bmm:workflows:create-flowchart
/bmad:bmm:workflows:create-wireframe
```

## Important Notes

- **No Pre-Loading**: Never load files speculatively - only when workflows/commands require them
- **Config is Sacred**: The config.yaml file MUST be loaded during agent activation step 2
- **Workflow Engine**: workflow.xml is the core execution engine - always load it for workflow menu items
- **Output Folder**: All generated artifacts go to `{output_folder}` from config (typically `docs/`)
- **Third-Person Communication**: Master agent refers to himself in third person
- **Module Versioning**: Current installation is v6.0.0-alpha.12
- **Multi-IDE Support**: Same agent logic works across Claude Code, Cursor, Codex, and Windsurf

## Documentation

Full documentation available in `bmad/bmm/docs/`:
- Quick Start Guide
- Agents Guide (45 min read)
- Scale Adaptive System (42 min read)
- Workflow guides for each phase
- Brownfield Development Guide
- Testing & QA documentation

External resources:
- Discord: https://discord.gg/gk8jAdXWmj
- GitHub: https://github.com/bmad-code-org/BMAD-METHOD
- YouTube: https://www.youtube.com/@BMadCode
