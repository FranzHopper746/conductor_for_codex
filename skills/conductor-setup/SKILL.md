---
name: conductor-setup
description: Scaffolds the project and sets up the Conductor environment
---
<!-- markdownlint-disable MD013 -->

# User Input

```text
$ARGUMENTS
```

CRITICAL: Conductor artifacts are **local-only**.

- You MUST NOT modify anything outside the repository root.
- Ensure that the `.gitignore` file includes the `conductor/` directory. Do not remove `conductor/` from the ignore rules. If the `conductor/` directory is not already listed in `.gitignore`, add it to the file.
- You MUST NOT `git add`/`git commit` Conductor artifacts unless the user explicitly requests it.

## 1.0 SYSTEM DIRECTIVE

You are an AI agent. Your primary function is to set up and manage a software project using the Conductor methodology. This document is your operational protocol. Adhere to these instructions precisely and sequentially. Do not make assumptions.

CRITICAL: You must validate the success of every tool call. If any tool call fails, you MUST halt the current operation immediately, announce the failure to the user, and await further instructions.

CRITICAL: When determining model complexity, Favor  deep reasoning. Context driven development is everything.

## 1.2 PRE-INITIALIZATION OVERVIEW

1. **Provide High-Level Overview:**
   - Present the following overview of the initialization process to the user:
     > "Welcome to Conductor. I will guide you through the following steps to set up your project:
     >
     > 1. **Project Discovery:** Analyze the current directory to determine if this is a new or existing project.
     > 2. **Product Definition:** Collaboratively define the product's vision, design guidelines, and technology stack.
     > 3. **Configuration:** Select appropriate code style guides and customize your development workflow.
     > 4. **Track Generation:** Define the initial **track** (a high-level unit of work like a feature or bug fix) and automatically generate a detailed plan to start development.
     >
     > Let's get started!"

## 3.0 INITIAL PLAN AND TRACK GENERATION

**PROTOCOL: Interactively define project requirements, propose a single track, and then automatically create the corresponding track and its phased plan.**

### 3.1 Generate Product Requirements (Interactive)(For greenfield projects only)

1. **Transition to Requirements:** Announce that the initial project setup is complete. State that you will now begin defining the high-level product requirements by asking about topics like user stories and functional/non-functional requirements.
2. **Analyze Context:** Read and analyze the content of `conductor/product.md` to understand the project's core concept.
3. **Ask Questions Sequentially:** Ask one question at a time. Wait for and process the user's response before asking the next question. Continue this interactive process until you have gathered enough information.

   - **CONSTRAINT** Limit your inquiries to a maximum of 5 questions.
   - **SUGGESTIONS:** For each question, generate 3 high-quality suggested answers based on common patterns or context you already have.
   - **General Guidelines:**

     - **1. Classify Question Type:** Before formulating any question, you MUST first classify its purpose as either "Additive" or "Exclusive Choice".

       - Use **Additive** for brainstorming and defining scope (e.g., users, goals, features, project guidelines). These questions allow for multiple answers.
       - Use **Exclusive Choice** for foundational, singular commitments (e.g., selecting a primary technology, a specific workflow rule). These questions require a single answer.

     - **2. Formulate the Question:** Based on the classification, you MUST adhere to the following:

       - **If Additive:** Formulate an open-ended question that encourages multiple points. You MUST then present a list of options and add the exact phrase "(Select all that apply)" directly after the question.
       - **If Exclusive Choice:** Formulate a direct question that guides the user to a single, clear decision. You MUST NOT add "(Select all that apply)".

     - **3. Interaction Flow:** \* **CRITICAL:** You MUST ask questions sequentially (one by one). Do not ask multiple questions in a single turn. Wait for the user's response after each question.
       - The last two options for every multiple-choice question MUST be "Type your own answer" and "Auto-generate the rest of requirements and move to the next step".
       - Confirm your understanding by summarizing before moving on.
     - **Format:** You MUST present these as a vertical list, with each option on its own line.
     - **Structure:**
       A) [Option A]
       B) [Option B]
       C) [Option C]
       D) [Type your own answer]
       E) [Auto-generate the rest of requirements and move to the next step]

   - **AUTO-GENERATE LOGIC:** If the user selects option E, immediately stop asking questions for this section. Use your best judgment to infer the remaining details based on previous answers and project context.

- **CRITICAL:** When processing user responses or auto-generating content, the source of truth for generation is **only the user's selected answer(s)**. You MUST completely ignore the questions you asked and any of the unselected `A/B/C` options you presented. This gathered information will be used in subsequent steps to generate relevant documents. DO NOT include the conversational options (A, B, C, D, E) in the gathered information.

1. **Continue:** After gathering enough information, immediately proceed to the next section.

### 3.2 Propose a Single Initial Track (Automated + Approval)

1. **State Your Goal:** Announce that you will now propose an initial track to get the project started. Briefly explain that a "track" is a high-level unit of work (like a feature or bug fix) used to organize the project.
2. **Generate Track Title:** Analyze the project context (`product.md`, `tech-stack.md`) and (for greenfield projects) the requirements gathered in the previous step. Generate a single track title that summarizes the entire initial track. For existing projects (brownfield): Recommend a plan focused on maintenance and targeted enhancements that reflect the project's current state.

   - Greenfield project example (usually MVP):

     ```markdown
     To create the MVP of this project, I suggest the following track:

     - Build the core functionality for the tip calculator with a basic calculator and built-in tip percentages.
     ```

   - Brownfield project example:

     ```markdown
     To create the first track of this project, I suggest the following track:

     - Create user authentication flow for user sign in.
     ```

3. **User Confirmation:** Present the generated track title to the user for review and approval. If the user declines, ask the user for clarification on what track to start with.

### 3.3 Convert the Initial Track into Artifacts (Automated)

1. **State Your Goal:** Once the track is approved, announce that you will now create the artifacts for this initial track.
2. **Initialize Tracks File:** Create the `conductor/tracks.md` file with the initial header and the first track:

    ```markdown
    # Project Tracks

    This file tracks all major tracks for the project. Each track has its own detailed plan in its respective folder.

    ---

    - [ ] **Track: <Track Description>**
          _Link: [./<Tracks Directory Name>/<track_id>/](./<Tracks Directory Name>/<track_id>/)_
    ```

    (Replace `<Tracks Directory Name>` with the actual name of the tracks folder resolved via the protocol.)

3. **Generate Track Artifacts:**
    a. **Define Track:** The approved title is the track description.
    b. **Generate Track-Specific Spec & Plan:**
    i. Automatically generate a detailed `spec.md` for this track.
    ii. Automatically generate a `plan.md` for this track. - **CRITICAL:** The structure of the tasks must adhere to the principles outlined in the workflow file at `conductor/workflow.md`. For example, if the workflow specificies Test-Driven Development, each feature task must be broken down into a "Write Tests" sub-task followed by an "Implement Feature" sub-task. - **CRITICAL:** Include status markers `[ ]` for **EVERY** task and sub-task. The format must be: - Parent Task: `- [ ] Task: ...` - Sub-task: `- [ ] ...` - **CRITICAL: Inject Phase Completion Tasks.** You MUST read the `conductor/workflow.md` file to determine if a "Phase Completion Verification and Checkpointing Protocol" is defined. If this protocol exists, then for each **Phase** that you generate in `plan.md`, you MUST append a final meta-task to that phase. The format for this meta-task is: `- [ ] Task: Conductor - User Manual Verification '<Phase Name>' (Protocol in workflow.md)`. You MUST replace `<Phase Name>` with the actual name of the phase.
    c. **Create Track Artifacts:**
    i. **Generate and Store Track ID:** Create a unique Track ID from the track description using format `shortname_YYYYMMDD` and store it. You MUST use this exact same ID for all subsequent steps for this track.
    ii. **Create Single Directory:** Resolve the **Tracks Directory** via the **Universal File Resolution Protocol** and create a single new directory: `<Tracks Directory>/<track_id>/`.
    iii. **Create `metadata.json`:** In the new directory, create a `metadata.json` file with the correct structure and content, using the stored Track ID. An example is: - `json
        {
        "track_id": "<track_id>",
        "type": "feature", // or "bug"
        "status": "new", // or in_progress, completed, cancelled
        "created_at": "YYYY-MM-DDTHH:MM:SSZ",
        "updated_at": "YYYY-MM-DDTHH:MM:SSZ",
        "description": "<Initial user description>"
        }
        `
    Populate fields with actual values. Use the current timestamp.
    iv. **Write Spec and Plan Files:** In the exact same directory, write the generated `spec.md` and `plan.md` files.
    v. **Write Index File:** In the exact same directory, write `index.md` with content:

    ```markdown # Track <track_id> Context

            - [Specification](./spec.md)
            - [Implementation Plan](./plan.md)
            - [Metadata](./metadata.json)
            ```

    d. **Commit State:** After all track artifacts have been successfully written, you MUST immediately write to `conductor/setup_state.json` with the exact content:
    `{"last_successful_step": "3.3_initial_track_generated"}`

    e. **Announce Progress:** Announce that the track for "<Track Description>" has been created.

### 3.4 Final Announcement

1. **Announce Completion:** After the track has been created, announce that the project setup and initial track generation are complete.
2. **Save Conductor Files:** Add and commit all files with the commit message `conductor(setup): Add conductor setup files`.
3. **Next Steps:** Inform the user that they can now begin work by running `/conductor:implement`.

