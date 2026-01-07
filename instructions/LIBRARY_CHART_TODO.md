# Library Chart Refactoring Plan

We planned to create a Helm library chart to share common templates across the `servarr` stack.

## Goal
Create `charts/pindaroli-common` (Library Chart) and refactor `charts/servarr` to use it.

## Plan
1.  **Create Library Chart**:
    *   New chart at `charts/pindaroli-common` with `type: library`.
    *   Add shared `_common.tpl` for labels and deployment structures.
2.  **Update Servarr**:
    *   Add dependency in `charts/servarr/Chart.yaml` pointing to `file://../pindaroli-common`.
    *   Refactor `sonarr` deployment to use the shared template as a proof of concept.

## Resume Prompt
To resume this work, simply ask Gemini:

> "I want to resume the library chart refactoring. Read LIBRARY_CHART_TODO.md and start implementing the pindaroli-common chart."
