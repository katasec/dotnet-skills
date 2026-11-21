# Find-duplicates prompt

Adapted from obra/superpowers-lab `finding-duplicate-functions` (MIT). Run once per category that
has two or more methods, with the strongest available model. Replace `{CATEGORY}` and
`<CATEGORY_PATH>`.

```
Read the methods at <CATEGORY_PATH>. They all fall in the "{CATEGORY}" category and include their
full bodies.

Find groups of methods that do the SAME job: they take the same kind of input and apply the same
rule, even if they have different names, signatures, types, projects or implementations, or one is
more general than the other.

Differing edge cases (one accepts int and the other throws, one trims and the other does not) do
NOT make them separate. That difference is drift between copies of one rule; report it.

Not duplicates:
- the same name doing different jobs
- an interface and its implementation, or an override and its base
- a thin public wrapper and the one method it calls
- tiny generic code (a null check, a one-line property) that is clearer inline

Output a JSON array, one entry per group, and nothing else:
[
  {
    "job": "<what every method in the group does, one line>",
    "confidence": "HIGH|MEDIUM|LOW",
    "methods": [
      {"file": "...", "type": "...", "name": "...", "line": <startLine>}
    ],
    "difference": "<the concrete behavioural difference, or 'none'>",
    "keep": "<which one should own the job, and why (e.g. the other's project already references it)>"
  }
]

HIGH: same input kind and same rule; you can state the difference exactly.
MEDIUM: same job, but you need the source to confirm the rule matches.
LOW: related; worth a look.
Return [] when there are no groups.
```
