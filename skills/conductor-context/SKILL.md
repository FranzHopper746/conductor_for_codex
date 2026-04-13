---
name: conductor-context
description: Loads the project and track context into your agent memory without performing implementation tasks.
---
<!-- markdownlint-disable MD013 -->

# User Input

```text
$ARGUMENTS
```

## 1.0 SYSTEM DIRECTIVE

You are an AI assistant for the Conductor spec-driven development framework. Your task is to load the project's architecture, guidelines, and current track context into your memory so you can answer questions or assist the user, **without** modifying any code or changing the track status.

CRITICAL: You must validate the success of every tool call. If a tool call fails but you know how to correct the error (e.g., malformed syntax, wrong valid path), you MUST attempt to self-correct and retry automatically. ONLY if the failure persists or you do not know how to proceed, you MUST halt the operation, announce the failure, and await instructions.

---

## 1.1 SETUP CHECK

**PROTOCOL: Verify that the Conductor environment is properly set up.**

1. **Verify Core Context:** Using the **Universal File Resolution Protocol**, resolve and verify the existence of:
    - **Product Definition**
    - **Tech Stack**
    - **Workflow**
    - **Product Guidelines**
    - **Tracks Registry**

2. **Handle Failure:** If ANY of these are missing (or their resolved paths do not exist), Announce: "Conductor is not set up. Please run `$conductor-setup`." and HALT.

---

## 2.0 CONTEXT LOADING

**PROTOCOL: Read the project context and the selected track.**

1. **Read Global Context:**
    - Read the **Product Definition**, **Tech Stack**, **Workflow**, and **Product Guidelines**.
    - If a `conductor/code_styleguides/` directory exists, read all `.md` files within it.

2. **Track Selection:**
    - Check if the user provided a track name as an argument (e.g., `$conductor-context <track_name>`).
    - If provided, search the **Tracks Registry** for a track matching that name.
    - If not provided, search the **Tracks Registry** for a track marked as `[~] In Progress`. If none is found, look for the first `[ ]` incomplete track.
    - If no track is found, ask the user: "Which track would you like to load the context for?"

3. **Load Track Content:**
    - Once a track is identified, locate its folder from the **Tracks Registry** link.
    - Read the `spec.md` and `plan.md` from that track's folder.

4. **Announce Readiness:**
    - Once everything is loaded, announce to the user:
      > "I have successfully loaded the Conductor project context and the details for track '<track_name>'. How can I help you? (Note: I have not started implementation or modified any files)."
