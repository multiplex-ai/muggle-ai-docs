# Preferences

Preferences control how Muggle Test behaves during testing — whether it reuses your last project, shows the browser window, publishes results automatically, and so on. They are read by the `@muggleai/works` skills and tools and stored locally at `~/.muggle-ai/preferences.json`.

## Setting Values

Ask your AI assistant, or use the `/muggle:preferences` skill:

> "Show my muggle preferences"
>
> "Set autoLogin to always"
>
> "Reset muggle preferences"

Most preferences accept one of three values:

| Value | Behavior |
| :----- | :------- |
| `ask` | Prompt you each time (the default) |
| `always` | Do it automatically, no prompt |
| `never` | Skip it automatically, no prompt |

A few preferences use a different value set, noted in the table.

## Available Preferences

| Preference | Values | Default | Controls |
| :--------- | :----- | :------ | :------- |
| `autoLogin` | always / ask / never | ask | Reuse saved credentials without prompting when a tool needs auth |
| `autoSelectProject` | always / ask / never | ask | Reuse the last-used project for this repo without prompting |
| `autoSelectLocalHost` | always / ask / never | ask | Reuse the dev-server URL from the previous local run |
| `showElectronBrowser` | always / ask / never | ask | Show the Electron browser window during local tests |
| `openTestResultsAfterRun` | always / ask / never | ask | Open the run's results page on the dashboard after a local run |
| `defaultExecutionMode` | local / remote / ask | ask | Which execution mode to default to when a skill supports both |
| `autoPublishLocalResults` | always / ask / never | ask | Upload local results to the cloud for team visibility |
| `suggestRelatedUseCases` | always / ask / never | ask | Suggest related use cases after creating or running one |
| `suggestRelatedTestCases` | always / ask / never | ask | Suggest related test cases after creating or running one |
| `autoDetectChanges` | always / ask / never | ask | Scan local git changes and map them to affected test cases |
| `postPRVisualWalkthrough` | always / ask / never | ask | Post a screenshot walkthrough comment to the PR after testing |
| `autoCreatePR` | always / ask / never | ask | Push the branch and open a PR without prompting |
| `checkForUpdates` | always / ask / never | ask | Check for newer Muggle Test versions at session start |
| `verboseOutput` | always / ask / never | ask | Show detailed progress logs during skills and tools |
| `autoUseWorktree` | always / ask / never | ask | Create a git worktree to isolate non-trivial work |
| `autoRebase` | always / ask / never | ask | Rebase onto the default branch before running dev servers or tests |
| `autoCleanup` | always / ask / never | ask | After a PR merges, remove the worktree, delete branches, clear artifacts |
| `autoE2ETest` | always / ask | always | Run E2E acceptance at the end of every `/muggle:do` cycle |

## Resetting

> "Reset my muggle preferences"

This restores every preference to its default — `ask` for most, `always` for `autoE2ETest`.

## Next Steps

| Goal | Resource |
| :--- | :------- |
| See the skills that read these | [Plugin Skills](local-testing/skills.md) |
| Set up local testing | [Local Testing Setup](local-testing/setup.md) |
