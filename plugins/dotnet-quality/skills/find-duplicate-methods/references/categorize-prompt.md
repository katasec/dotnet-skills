# Categorize prompt

Adapted from obra/superpowers-lab `finding-duplicate-functions` (MIT). Run once per batch with a
small, fast model. Replace `<BATCH_PATH>` and `<OUTPUT_PATH>`.

```
Read the method list at <BATCH_PATH>. Each entry has id, file, type, name, signature and preview
(the first lines of the method).

Give each method exactly ONE category for the job it does: WHAT it does, not HOW or where.
Two methods that do the same job in different classes or projects must get the same category.

Categories:
- json-serialization: writing or reading JSON, Utf8JsonWriter, JsonElement, JsonNode
- text-format: formatting, escaping, truncating, case conversion, building display strings
- text-parse: parsing strings, numbers, durations, identifiers, command lines
- comparison-rules: evaluating conditions, comparing values to thresholds, matching operators
- validation: checking input, names, shapes or limits and reporting what is wrong
- file-io: reading, writing, atomic replace, temp files, directories
- path-resolution: resolving, normalizing, guarding or abbreviating paths
- process-exec: starting processes, environment, exit codes, stdout and stderr
- http: building requests, sending, reading status and errors
- hashing-ids: hashes, fingerprints, deterministic ids
- config-env: reading settings, environment variables, manifests
- chat-messages: building, mapping or normalizing model messages and tool calls
- tool-dispatch: running tools or capabilities, arguments and results
- checkpoint-resume: saving, checking or replaying paused work
- orchestration: running steps, branches, loops, retries
- errors-diagnostics: building exceptions, error messages, diagnostics
- cli-commands: command and option definitions, console output
- mapping: converting one type or DTO to another
- other-<word>: none of the above; replace <word> with one specific word

Rules:
1. Constructors: the job of the type they build.
2. Choose the primary job when several fit.
3. Do not skip any method.

Write a JSON array to <OUTPUT_PATH> using the Write tool, one entry per method:
{"id": N, "category": "...", "purpose": "one sentence"}
Copy each id exactly as given.
Write ALL entries; do not summarize.
```
