# Mona's Project Pulse implementation plan

## Summary

Build Mona's Project Pulse as a dependency-free static dashboard using plain
HTML, CSS, and JSON. It should present multiple projects as accessible,
responsive cards showing each project's name, owner, status, recent activity,
and priority.

The Orchestrator coordinates the work. The Planner owns this plan, the
Designer owns the visual and accessibility direction, and the Coder owns the
runnable implementation and integration.

## File assignments

| File | Owner | Responsibility |
| --- | --- | --- |
| `app/index.html` | Coder | Semantic page shell, dashboard structure, data loading and rendering, project-card markup, and loading/error states. |
| `app/styles.css` | Designer | Visual system, responsive layout, hierarchy, cards, status and priority treatments, focus states, and accessible contrast/spacing. |
| `app/project-data.json` | Coder, with Designer input | Deterministic sample records under a top-level `projects` array. |
| `.vscode/launch.json` | Coder | Strict JSON launch configuration that serves `app/` and opens `index.html`. |
| `docs/project-pulse-plan.md` | Planner / Orchestrator | This implementation plan and its ownership, dependencies, and validation criteria. |

Agents must not modify another agent's primary files without an explicit
handoff from the Orchestrator.

## Required behavior

- Use the exact title **Project Pulse** in `app/index.html`.
- Reference `styles.css` and `project-data.json`.
- Load data from `app/project-data.json`.
- Render visible project cards using the `project-card` class.
- Display `name`, `owner`, `status`, `recentActivity`, and `priority` for each project.
- Use a top-level `projects` array in `app/project-data.json`.
- Provide a polished, responsive dashboard rather than an unstyled page.
- Open the dashboard page instead of a directory listing from VS Code.

Each project record must contain at least:

```json
{
  "name": "Example project",
  "owner": "Contributor name",
  "status": "Active",
  "recentActivity": "Recent contributor-facing update",
  "priority": "High"
}
```

## Ordered implementation steps

### 1. Confirm the implementation contract

**Owner:** Orchestrator

- Review the Project Pulse brief and this plan.
- Confirm the framework-free approach and required paths.
- Confirm file ownership boundaries.
- Communicate the fixed selectors, data key, fields, and launch requirements
  before delegating.

This step happens first.

### 2. Define the visual and accessibility direction

**Owner:** Designer  
**File:** `app/styles.css`

The Designer will create:

- A clear heading, supporting context, and project-card grid hierarchy.
- A responsive layout from one column on narrow screens to multiple columns on
  wider screens.
- Readable typography, spacing, and content density.
- Distinct, accessible status and priority treatments that do not rely on
  color alone.
- Rounded cards, visible shadows, and clear visual grouping.
- The required `.dashboard` and `.project-card` selectors.
- Hover and keyboard focus states.
- Wrapping behavior for long names, owners, and activity text.
- Reduced-motion-safe transitions if transitions are used.

The Designer will not modify `app/index.html` or add a CSS framework.

### 3. Create deterministic project data

**Owner:** Coder  
**File:** `app/project-data.json`

The Coder will:

- Create valid JSON with a top-level `projects` array.
- Include multiple representative projects.
- Include all required fields on every record.
- Include realistic variation in statuses, priorities, owners, and activity
  lengths.
- Avoid comments, trailing commas, external APIs, or runtime dependencies.

### 4. Implement the semantic dashboard and rendering

**Owner:** Coder  
**File:** `app/index.html`

The Coder will:

- Add standard HTML structure and the exact `Project Pulse` title.
- Reference `styles.css` and load `project-data.json`.
- Create a semantic `.dashboard` container and contributor-oriented
  introductory content.
- Render one visible `.project-card` per project.
- Display each project's name, owner, status, recent activity, and priority
  with clear labels.
- Use accessible markup and ensure meaning is not communicated through color
  alone.
- Provide explicit loading, empty-list, malformed-data, and fetch-failure
  states.
- Avoid silently replacing invalid data with misleading success content.

The implementation may keep its behavior in the HTML file unless the
Orchestrator explicitly assigns another file.

### 5. Create the VS Code launch configuration

**Owner:** Coder  
**File:** `.vscode/launch.json`

Create strict JSON with no comments and a configuration named exactly
**Run Project Pulse Dashboard**. It must:

- Run `python3 -m http.server 5500`.
- Set `cwd` to `${workspaceFolder}/app`.
- Open `http://localhost:%s/index.html`.
- Open `index.html`, not the `app/` directory listing.
- Remain deterministic and compatible with the repository's VS Code setup.

### 6. Integrate and review

**Owner:** Orchestrator, with Coder and Designer

Review all four implementation files together to verify:

- HTML selectors match CSS selectors.
- JSON field names match the rendering logic.
- Every required field is visible.
- The grid works with the sample data.
- Loading, empty, malformed-data, and fetch-failure states are explicit.
- The launch configuration serves the correct directory and page.
- No accidental framework assumptions or unrelated changes were introduced.

This step is sequential because it requires all assigned files.

### 7. Validate and hand off

**Owner:** Orchestrator

- Run static and JSON checks.
- Start the configured local server and inspect the rendered dashboard.
- Check responsive behavior and keyboard focus.
- Record validation results, limitations, and launch instructions in the final
  handoff documentation if requested.

## Designer responsibilities

The Designer is accountable for information hierarchy, scanability, responsive
layout, project-card composition, status and priority presentation, typography,
spacing, contrast, visual grouping, rounded styling, shadows, focus visibility,
non-color affordances, and robust handling of long content. The Designer owns
`app/styles.css` and reports design decisions and accessibility tradeoffs to
the Orchestrator.

## Coder responsibilities

The Coder is accountable for semantic structure, JSON loading and rendering,
clear loading/empty/error states, visible required fields, deterministic
sample data, strict `.vscode/launch.json`, the `app/` working directory, and
runtime validation. The Coder owns `app/index.html`, `app/project-data.json`,
and `.vscode/launch.json`, and does not change `app/styles.css` without an
explicit reassignment.

## Dependencies

- The implementation contract must be confirmed before delegation.
- Designer and Coder must preserve the fixed `.dashboard`,
  `.project-card`, `projects`, and required field contracts.
- Page rendering depends on the JSON schema and CSS hooks, but both can be
  developed against the fixed contract.
- Runtime validation depends on all four implementation files being present.
- Browser validation normally depends on HTTP serving; opening the HTML file
  directly may block JSON fetches because of browser file-origin rules.
- Python 3 must be available for the required launch command.
- No package installation is expected for this static implementation.

## Parallel work decisions

After the Orchestrator confirms the contract, these tasks can run in parallel
because their primary file scopes do not overlap:

1. Designer creates `app/styles.css`.
2. Coder creates `app/project-data.json`.
3. Coder creates `app/index.html` against the fixed schema and selectors.
4. Coder creates `.vscode/launch.json`.

The Orchestrator must communicate the fixed contracts before parallel work:
`.dashboard` and `.project-card` hooks, the `projects` top-level key, the five
required fields, the server command, the working directory, and the launch URL.

The following work must be sequential:

1. Brief and repository review before delegation.
2. Contract confirmation before independent implementation.
3. Integration review after all four files exist.
4. Runtime validation after integration.
5. Any final handoff documentation after validation results are known.

## Edge cases

- Empty `projects` array: show an explicit empty state.
- Missing required field: show an unavailable indicator or data error rather
  than an unlabeled blank value.
- Malformed JSON or failed fetch: show a useful user-facing error state.
- Long content: wrap without horizontal overflow.
- Unknown status or priority: preserve readable text with a neutral fallback.
- Small screens: prevent clipping and horizontal scrolling.
- Interactive elements: provide visible keyboard focus.
- Reduced motion: do not make essential information depend on animation.
- Port 5500 conflict: report it explicitly rather than silently changing the
  deterministic launch port.

## Validation expectations

### Static validation

Run:

```bash
python3 -m json.tool app/project-data.json >/dev/null
python3 -m json.tool .vscode/launch.json >/dev/null
```

Also verify that:

- `app/index.html` contains `Project Pulse`, references both assets, and uses
  `project-card` while rendering all required fields.
- `app/styles.css` contains `.dashboard`, `.project-card`, `border-radius`, and
  `box-shadow`.
- `app/project-data.json` contains a top-level `projects` array and every
  record has all five required fields.
- `.vscode/launch.json` contains **Run Project Pulse Dashboard**, the required
  Python command, `${workspaceFolder}/app`, and the `/index.html` URL.

### Runtime validation

Use **Run and Debug** in VS Code:

1. Select **Run Project Pulse Dashboard**.
2. Start the configuration and confirm the server starts.
3. Confirm the browser opens `http://localhost:5500/index.html`.
4. Confirm the heading and multiple project cards are visible.
5. Confirm every card shows owner, status, recent activity, and priority.
6. Resize the viewport to test narrow and wide layouts.
7. Use keyboard navigation to inspect focus visibility.
8. Stop the server after validation.

If a browser is unavailable, use an equivalent local-server request with
`curl` and manually inspect the source and static checks.
