---
allowed-tools: [Bash, Read]
description: "Analyze changes and commit to git without pushing"
---

Please analyze the current git changes and create a commit:

1. First, check the git status and diff to understand what has changed
2. Analyze the changes to determine:
   - The type of change (feat, fix, docs, refactor, test, chore, etc.)
   - The appropriate emoji from our COMMIT_GUIDE.md
   - A clear, concise commit message
3. Stage the appropriate files
4. Create a commit following our standards (Conventional Commits + Gitmoji)
5. DO NOT push the changes

Remember:
- Follow the format: `<type>: <emoji> <description>`
- Do NOT include AI attribution in commit messages
- If working with submodules, handle them separately

$ARGUMENTS