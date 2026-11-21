# Third-party notices

This repository adapts work from the projects below, each under the MIT License.

## dotnet/skills

- Source: https://github.com/dotnet/skills, `plugins/dotnet-test/skills/find-untested-sources/scripts/Find-UntestedSources.cs` (commit `9864ca7`)
- Used in: `plugins/dotnet-quality/skills/find-duplicate-methods/scripts/Find-Methods.cs` (file walk, skip rules, snapshot and syntax read, namespace lookup, diagnostics)
- Copyright (c) .NET Foundation and Contributors

## obra/superpowers-lab

- Source: https://github.com/obra/superpowers-lab, `skills/finding-duplicate-functions` (commit `344b079`)
- Used in: `plugins/dotnet-quality/skills/find-duplicate-methods` (the extract, categorize and compare method; both prompts in `references/`)
- Copyright (c) 2025 Jesse Vincent

## MIT License (applies to each project above)

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and
associated documentation files (the "Software"), to deal in the Software without restriction,
including without limitation the rights to use, copy, modify, merge, publish, distribute,
sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or
substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT
NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND
NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES
OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN
CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
