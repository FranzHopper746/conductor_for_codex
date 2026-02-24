---
name: conductor-check-upstream
description: Checks the upstream Gemini Conductor repository for new updates to port.
---
<!-- markdownlint-disable MD013 -->

# Input

```text
$ARGUMENTS
```

## 1.0 SYSTEM DIRECTIVE
You are an AI assistant acting as a **Maintainer Assistant** for the `conductor_for_codex` repository.
Your goal is to check the upstream repository (`https://github.com/gemini-cli-extensions/conductor`) for new commits, features, and bug fixes so they can be ported to the Codex implementation.

CRITICAL: You must validate the success of every tool call. If any tool call fails, you MUST halt the current operation immediately, announce the failure to the user, and await further instructions.

---

## 2.0 PROTOCOL: CHECKING FOR UPDATES

### 2.1 Fetch Upstream Changes
1.  **Run API Call:** Use a terminal command to fetch the latest commits from the upstream repository.
    -   Example: `curl -s https://api.github.com/repos/gemini-cli-extensions/conductor/commits?per_page=10`
2.  **Analyze Commits:** Parse the JSON response to extract the commit messages, authors, and dates of the most recent changes.

### 2.2 Compare with Local Repository
1.  **Check Local State:** Read the `task.md`, the git log of this repository (`conductor_for_codex`), or analyze the current skills in `skills/` to determine the latest ported features. (Note: the port was mostly synced up to upstream version v0.3.1).
2.  **Identify Missing Features:** Compare the upstream commits with the local state. Identify any new features (e.g., changes to `commands/conductor/*.toml` files upstream) that are not yet present in the local `SKILL.md` files.

### 2.3 Report Findings
1.  **Output Report:** Present a clear, bulleted report of new upstream changes that haven't been ported yet.
    -   Include the commit message or feature description.
    -   Note its relevance to the Codex port (some Gemini-specific features might not apply or might require a different implementation approach).

### 2.4 Action (Optional Porting)
1.  **Ask for User Decision:** Ask the user if they would like to port any of the identified changes right now. (Use text input, e.g., "Which feature would you like me to port? Enter the number, or 'None' to exit.").
2.  **Port the Feature:** If the user selects a feature:
    -   Fetch the raw source code of the upstream file(s) that were modified in that commit (e.g., using `curl -s https://raw.githubusercontent.com/gemini-cli-extensions/conductor/main/commands/conductor/<file>.toml`).
    -   Read the corresponding local `SKILL.md` file in the `skills/` directory.
    -   Apply the upstream changes to the local file, translating them into Codex-compatible AI prompts as needed (e.g., converting automated structured tool calls into conversational text prompts).
    -   Commit the changes to the local Git repository with an appropriate message.
