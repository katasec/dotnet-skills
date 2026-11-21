---
name: find-duplicate-methods
description: Finds C# methods that do the same job under different names, signatures or edge cases (semantic duplicates) across a .NET codebase, by listing every method with Roslyn, grouping methods by job, and comparing within each group. USE FOR "find duplicate code paths", "same logic implemented twice", "reimplemented helpers", "redundant code across projects", deslop or cleanup audits of an existing codebase, and before consolidating utilities. DO NOT USE FOR merging the duplicates once found (use csharp-refactoring), copy-paste or text clone detection only, reviewing a single diff for reuse, or non-C# code.
---

# Find Duplicate Methods

Lists every C# method in a source tree with a parse-only Roslyn pass, groups the methods by the
job they do, and compares each group to report methods that do the same job under different names.
The output is a findings table of verified duplicate pairs with the exact difference between them.

## When to Use

- Auditing an existing codebase for redundant code paths (a deslop or cleanup pass)
- Suspecting a helper was written again instead of reused, especially in LLM-written code
- Before consolidating utilities, to know every copy

## When Not to Use

- Merging duplicates that are already known: use `csharp-refactoring`
- Reviewing one diff for reuse: a review of that diff is cheaper
- Text or copy-paste clones only: a clone detector is cheaper

## Inputs

| Input | Required | Description |
|-------|----------|-------------|
| Source root | Yes | Folder holding the C# sources, usually the repo's `src`. Exclude test projects unless asked |
| Scratch folder | No | Where the catalog and working files go. Default: a new temp folder outside the repo |

## Workflow

### Step 1: List every method

Requires a .NET SDK that runs file-based apps (`dotnet --version` 10.0 or later). If it is older,
stop and report that prerequisite; do not install anything.

```bash
dotnet scripts/Find-Methods.cs -- <source-root> <scratch>/catalog.json
```

The script prints `<n> methods from <m> files`. Exit code 3 means some files were skipped; list
them in the report. Never write the catalog inside the scanned repo.

Every method is kept, including private and static ones. Most semantic duplicates are private
helpers in different types, so filtering to public methods hides them.

### Step 2: Group methods by job

Split the catalog into batches of at most about 150 methods (one top-level project folder per
batch fits most repos). For each batch, run a subagent with
[references/categorize-prompt.md](references/categorize-prompt.md), giving it only each method's
`id`, `file`, `type`, `name`, `signature` and `preview`. Use a small, fast model; this step only labels.
Merge the batch outputs into `<scratch>/categorized.json`.

Join labels to the catalog on `id`, never on names or order. Check that every catalog `id` has
exactly one category before going on.

### Step 3: Compare within each group

For each category with **two or more** methods, run a subagent with
[references/find-duplicates-prompt.md](references/find-duplicates-prompt.md), giving it the full
`body` of every method in that category. Use the strongest available model; this step decides.

Two methods are enough. A pair is the common case, so do not skip small categories.

### Step 4: Verify before reporting

For each HIGH or MEDIUM group, read both methods in the source (not the catalog) and confirm they
take the same kind of input and apply the same rule. Drop any group that does not survive the
read.

Differing edge cases between copies are drift. Drift is the reason to merge, not a reason to keep
them separate: name the difference.

### Step 5: Report

Report only the verified groups, in this table, most valuable first:

| # | Job | Locations | Difference | Keep | Confidence |
|---|-----|-----------|------------|------|------------|
| 1 | Write a tool-argument value as JSON | `Core/PipelineRunner.cs:853`, `ChatClients/AnthropicMessageMapping.cs:191` | Core throws on `int`, `float`, `decimal`; ChatClients accepts them | the one whose project the other already references | HIGH |

End with the counts (methods listed, categories compared, groups found, groups verified) and any
skipped files. To merge a group, hand it to `csharp-refactoring`.

## Validation

- [ ] `catalog.json` count matches the script's printed count, and no file in the report was skipped silently
- [ ] Every category with two or more methods was compared
- [ ] Every reported group was read in source and states its difference
- [ ] Nothing in the scanned repo was changed

## Common Pitfalls

| Pitfall | Solution |
|---------|----------|
| Searching by name or `grep` | Names differ between duplicates; compare by job, which is the point of the catalog |
| Keeping only public methods | Keep all; duplicates are usually private helpers |
| Skipping categories with fewer than three methods | Compare every category with two or more |
| Sending full bodies to the categorize step | Send the `preview`; full bodies only go to the compare step |
| Calling different edge cases "separate purposes" | Same input kind and rule means a duplicate; the edge case is the drift to report |
| Reporting unverified groups | Read both methods in source first |
| Fixing while finding | This skill reports; `csharp-refactoring` merges |

## Credits

The extract, categorize and compare method is adapted from obra/superpowers-lab
`finding-duplicate-functions` (MIT). `scripts/Find-Methods.cs` is adapted from dotnet/skills
`find-untested-sources` (MIT). See the repository's `THIRD-PARTY-NOTICES.md`.
