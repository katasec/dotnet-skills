# dotnet-skills

.NET agent skills in the [Agent Skills](https://agentskills.io/specification) format, for Claude
Code, GitHub Copilot and Codex. Layout follows [dotnet/skills](https://github.com/dotnet/skills):
`plugins/<plugin>/skills/<skill>/SKILL.md`.

## Install

```bash
npx skills add katasec/dotnet-skills --global --agent claude-code codex github-copilot --skill '*' -y
```

## Skills

| Plugin | Skill | What it does |
|---|---|---|
| `dotnet-quality` | [`find-duplicate-methods`](plugins/dotnet-quality/skills/find-duplicate-methods/SKILL.md) | Finds C# methods that do the same job under different names: a Roslyn pass lists every method, an agent groups them by job and compares each group |

## Requirements

Skills with C# scripts run them as file-based apps (`dotnet <script>.cs`), so they need a .NET 10
or later SDK on the machine. The scripts only parse source text; the target repo's own .NET version
does not matter.

## License

MIT. Adapted third-party work is listed in [THIRD-PARTY-NOTICES.md](THIRD-PARTY-NOTICES.md).
