# Keep It Simple Evaluations

These are proposed behavioral evaluations, not recorded test results. No baseline or cross-model comparison has been measured.

Use these scenarios when validating or refining the skill, not during ordinary planning or implementation. Run comparable requests with and without the skill in fresh sessions. Supply the request and input artifacts to the executing agent; keep the expected behavior and failure criteria with the evaluator. Record the model, environment, actual output, and observed differences when tests are run.

## 1. Plan a bounded feature

**Request:** Plan a CSV download of the signed-in user's invoices. The product intent requires invoice number, date, and total, using the existing authorization rules. Assess the suggested additions before creating tasks.

**Inputs:** A small repository with an invoice query already scoped to the current user and a CSV library already available. Proposed additions include the download endpoint, CSV rendering, access-control coverage, a configurable column selector, scheduled exports, and an export-provider registry for possible future formats. There are no current requirements for the last three additions.

**Expected behavior:** The plan maps meaningful changes to requirements. It classifies the endpoint, rendering, and necessary verification as core, and the optional behavior as non-core. It separately challenges the registry as implementation complexity. It reuses the existing query and library, leaves speculative additions out by default, and briefly explains the consequential omissions. The change inventory and tasks agree.

**Failure criteria:** Optional features become implementation tasks without authorization; the registry is justified only by future formats; authorization is omitted to save code; every line receives a classification; or the agent stops repeatedly for permission to omit speculative work.

## 2. Implement without weakening the contract

**Request:** Implement the selected task in an approved plan: allow an owner to rename a project through the existing update flow. Keep the accepted architecture and current review boundary.

**Inputs:** A small repository with an existing update method, owner authorization, a shared name validator used by creation and updates, and relevant tests. The approved plan requires retaining those rules and proving owner success, non-owner rejection, and invalid-name rejection. A background-job library is installed, but the task requires no asynchronous work.

**Expected behavior:** The agent uses the existing update flow and keeps the shared validation and authorization. It adds only missing behavior and meaningful coverage. It creates no rename service hierarchy, event bus, job, or configurable naming policy merely because those could become useful. It verifies the selected task and respects the existing stopping rule. Routine implementation does not produce a separate classification report for each edit.

**Failure criteria:** The agent adds future-facing infrastructure; deletes shared validation to minimize lines; bypasses the approved design without revisiting it; or expands into unrelated cleanup. Treating every abstraction as inherently wrong also fails.

## 3. Assess a branch with mixed evidence

**Request:** Assess the current branch with keep-it-simple. Recommend simplifications without editing files.

**Inputs:** A temporary Git repository with an explicit base branch and agreed intent to add an authenticated CSV invoice download. Committed changes add the endpoint and required authorization plus a generic provider registry with a single CSV implementation. Staged changes remove a required authorization check. Unstaged changes add a configurable column selector absent from the intent. An untracked file contains an unrelated draft. Existing code outside the diff contains a shared validator with demonstrated callers.

**Expected behavior:** The agent states the comparison baseline and distinguishes committed, staged, unstaged, and relevant untracked changes. It identifies the selector as non-core, the unnecessary registry as excess implementation complexity, and the authorization deletion as a core regression. Findings point to inspected code, explain a concrete cost, and propose proportionate simplifications. The unrelated draft is excluded and disclosed; existing shared code is not targeted merely to reduce file count. No files are modified.

**Variation:** Remove the intent and plan from the inputs. The agent reports uncertainty about core/non-core status, asks for the missing intent when necessary, and limits confident conclusions to what the available evidence establishes.

**Failure criteria:** The assessment inspects only the worktree diff and misses committed additions; invents an intent or base branch; recommends removing required checks; labels existing unrelated debt as a new regression; or applies its recommendations automatically.

## Discovery boundary

A request such as "Explain what this existing function does" should receive an explanation. The skill should not turn it into an unsolicited branch audit or refactoring assignment.
