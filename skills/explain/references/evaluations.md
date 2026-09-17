# Explain Evaluations

These are proposed behavioral evaluations. No baseline, fresh-session behavioral test, or cross-model comparison has been measured.

Use these scenarios when validating or refining the skill. Give the executing agent only the request and input artifacts; keep expected behavior and failure criteria with the evaluator. Compare equivalent requests with and without the skill, recording the model, environment, output, and observed differences.

## 1. Explain specified code

**Request:** Explain `Checkout#total` in detail.

**Inputs:** A small repository where `Checkout#total` sums line items, calls a discount helper, then applies tax. Include the helper, a caller, and tests demonstrating rounding and an empty cart. Include unrelated uncommitted changes elsewhere.

**Expected behavior:** The explanation stays focused on the requested method, follows the helper and caller far enough to explain the contract, traces a concrete example through discount and tax, and describes rounding and empty-cart behavior. It cites inspected source locations and distinguishes test expectations from tests actually run. It does not assume the user knows domain-specific terms.

**Variation:** Supply only a pasted method that calls an unavailable discount helper. The explanation describes the visible flow and identifies the helper's unknown behavior without inventing its implementation.

**Failure criteria:** The agent explains the unrelated working tree instead; merely paraphrases each line; invents business intent or helper behavior; claims tests passed without running them; or edits the code.

## 2. Explain all current uncommitted changes

**Request:** Explain my uncommitted code in detail.

**Inputs:** A temporary Git repository with a committed order endpoint. Stage a change that calls a new validation helper, then make an unstaged adjustment to the same call. Leave the helper and its tests untracked. Also include a deleted tracked file and an unrelated changed configuration file. Supply the committed versions and relevant callers.

**Expected behavior:** The agent accounts for staged, unstaged, and untracked changes, explains the final working-tree behavior against `HEAD`, and describes the index/worktree difference where it changes behavior. It reads the untracked helper, explains the deletion and configuration change, groups related changes, and traces one request through the affected flow. It identifies tests as evidence of intended behavior without claiming execution. Repository files and Git state remain unchanged.

**Variation:** Stage an edit, then undo it only in the working tree. The agent reports the staged change and its unstaged reversal even though the combined tracked diff against `HEAD` is empty.

**Failure criteria:** The agent uses only `git diff`; misses untracked code or a deletion; double-counts staged and unstaged changes as independent final behavior; silently omits an unrelated change; explains branch history as uncommitted work; or modifies Git state.

## 3. Handle an absent baseline or absent input

**Request:** Explain the current uncommitted code.

**Inputs:** A temporary Git repository with no commits, one staged source file, and one untracked source file that it calls.

**Expected behavior:** The agent recognizes that `HEAD` does not exist, explains the available files as new code, and avoids inventing a previous implementation.

**Variations:** In a clean committed repository, it reports that there are no uncommitted changes. Outside a repository with no supplied code, it asks for code or a repository location. If a relevant file cannot be read, it identifies the gap and limits claims while explaining the accessible code.

**Failure criteria:** A missing `HEAD` causes the agent to abandon readable code; it invents a diff; silently switches to the latest commit in a clean repository; or guesses inaccessible behavior.

## Discovery boundary

Requests such as "Fix this function", "Review these changes for bugs", or "Commit my work" should activate the corresponding implementation, review, or commit workflow. This skill must not substitute an explanation for those requested actions. An explicit request to explain code before another action can use this skill for that explanation.
