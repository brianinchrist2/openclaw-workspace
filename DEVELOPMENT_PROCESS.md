# BigQ Development Team - Standard Development Process

**Version:** 1.1
**Date:** 2026-02-22
**Author:** BigQ (BQ) - Super Architect & Lead Developer / Orchestrated by TB

## 1. Overview

This document defines the standard development workflow for BigQ's Development Team. It ensures that every system designed and implemented meets the highest standards of architectural integrity, maintainability, and quality. This process is designed for human-AI collaboration, leveraging AI speed for generation and human wisdom for validation.

## 2. Team Roles & AI Model Allocation

To ensure maximum efficiency and reasoning depth, tasks are delegated to specific roles powered by designated AI models:

*   **The Head Steward (TB):** Orchestrator, task delegator, and workspace manager.
    *   **Model:** `Gemini 3 Flash` (High speed, fast coordination)
*   **The Architect & Lead Developer (BQ):** System design, complex logic, and code review.
    *   **Model:** `Gemini 3 Pro Deep Think` (High reasoning, architectural depth)
*   **The Developer (Coding Sub-agents):** Translating specifications into functional code.
    *   **Model:** `Gemini 3 Flash` (for standard tasks) or `Gemini 3 Pro` (for complex components)
*   **The Researcher (R2D2):** Deep searching, documentation reading, and context gathering.
    *   **Model:** `Gemini 3 Flash` (Fast web search and summarization)
*   **The QA & DevOps:** Automated testing, environment setup, and deployment validation.
    *   **Model:** `Gemini 3 Flash` (Script execution and log analysis)

## 3. Core Principles

*   **Architecture First:** No code is written without a corresponding architectural design.
*   **Rigorous Testing:** "It works" is not enough. It must be proven to work.
*   **Version Control:** Every change must be tracked, reversible, and atomic.
*   **Automation:** Repetitive tasks must be automated.

## 4. Git Version Control Specification

All projects must strictly adhere to the following Git workflow to ensure code stability and team collaboration.

### 4.1 Branching Strategy
*   `main` (or `master`): Production-ready state. Never commit directly to main.
*   `dev` (or `develop`): Integration branch for the next release.
*   `feature/<name>`: New features. Branched from `dev`, merged back to `dev`. (e.g., `feature/user-auth`)
*   `hotfix/<name>`: Urgent fixes for production. Branched from `main`, merged to `main` and `dev`. (e.g., `hotfix/login-crash`)

### 4.2 Commit Message Convention
Commits must be atomic and descriptive. Format: `<type>(<scope>): <subject>`
*   `feat:` A new feature
*   `fix:` A bug fix
*   `docs:` Documentation only changes
*   `style:` Changes that do not affect the meaning of the code (formatting, etc.)
*   `refactor:` A code change that neither fixes a bug nor adds a feature
*   `test:` Adding missing tests or correcting existing tests
*   `chore:` Changes to the build process or auxiliary tools

*Example: `feat(auth): implement JWT token generation`*

### 4.3 Merging & Release
*   Code must be reviewed and tested before merging via Pull Request (PR) or Merge Request (MR).
*   Avoid messy commit histories; rebase or squash feature branches before merging if they contain too many minor iterations.

## 5. The Workflow

### Phase 1: Requirement Analysis & Specification (The Architect - BQ)
**Goal:** Convert business needs into a technical Specification (`SPEC.md`).
**Steps:**
1.  **Clarification:** Confirm understanding of the user's request. Ask "Why?" and "What if?".
2.  **Research:** Use R2D2 to check existing systems, documentation, and APIs.
3.  **Drafting Spec:** Define Problem, Functional/Non-Functional Requirements, Tech Stack, and High-Level Architecture.

### Phase 2: Implementation (The Developer)
**Goal:** Translate the specification into working code.
**Steps:**
1.  **Branching:** Create a new `feature/*` branch.
2.  **Scaffolding:** Set up project structure.
3.  **Core Logic:** Implement business logic following strict coding standards.
4.  **Committing:** Make frequent, atomic commits following the Git specification.

### Phase 3: Verification & Testing (The QA Engineer)
**Goal:** Prove the system works as specified.
**Steps:**
1.  **Unit Testing:** Write and execute tests.
2.  **Integration Testing:** Run the application and verify endpoints.
3.  **Code Review:** BQ reviews the code against `SPEC.md`.

### Phase 4: Deployment & Handover (The DevOps)
**Goal:** Deliver the system to the user/environment.
**Steps:**
1.  **Merging:** Merge the validated `feature` branch into `dev`/`main`.
2.  **Deployment:** Deploy code to the target environment.
3.  **Handover:** Inform TB and the user, providing execution logs and operational status.

## 6. Artifact Storage
All project artifacts must reside in the designated workspace: `C:\workspace\[ProjectName]`.