# Git Workflow Guide

## Submodule Workflow

### Initial Setup
```bash
git clone --recursive <repo-url>
# OR if already cloned:
git submodule update --init --recursive
```

### Daily Workflow

1. **Check Current Directory**
   ```bash
   pwd  # CRITICAL: Always verify location first
   ```

2. **Update Submodules**
   ```bash
   git submodule update --remote --merge
   ```

3. **Make Changes in Submodule (Use ABSOLUTE paths)**
   ```bash
   cd /full/path/to/cloudz.pro/api  # or web-back-office
   git checkout main
   git pull origin main
   # Make your changes
   git add .
   git commit -m "type: 📝 description"  # NO AI attribution!
   git push origin main
   ```

4. **Update Parent Repository**
   ```bash
   cd /full/path/to/cloudz.pro  # Absolute path to root
   git add api  # or web-back-office
   git commit -m "chore: 🔄 update submodule reference"
   git push origin main
   ```

### Common Issues

#### "Cannot add files from submodule"
- You must commit within the submodule directory first
- Cannot use `git add api/file.ts` from parent directory

#### "Submodule has uncommitted changes"
- Enter the submodule and commit/stash changes
- Or discard with `git checkout .` inside submodule

#### "Detached HEAD in submodule"
```bash
cd submodule-dir
git checkout main
git pull origin main
```

## Commit Standards

### Format
```
<type>: <emoji> <description>

[optional body]

[optional footer]
```

### Types & Emojis
- `feat: ✨` - New feature
- `fix: 🐛` - Bug fix
- `docs: 📝` - Documentation only
- `style: 💄` - Formatting, missing semicolons, etc
- `refactor: ♻️` - Code change that neither fixes a bug nor adds a feature
- `perf: ⚡️` - Performance improvement
- `test: ✅` - Adding missing tests
- `chore: 🔧` - Changes to build process or auxiliary tools

See @COMMIT_GUIDE.md for complete emoji reference.

### Examples
```bash
git commit -m "feat: ✨ add user authentication module"
git commit -m "fix: 🐛 resolve login validation error"
git commit -m "docs: 📝 update API documentation"
git commit -m "chore: 🔄 update submodule references"
```

### Critical Commit Rules (Session Learning: 2025-07-05)
- **NEVER** include "🤖 Generated with Claude Code" or "Co-Authored-By: Claude"
- **NEVER** include ANY AI attribution in commit messages
- Always use absolute paths for submodule navigation
- Keep subject line under 50 characters
- Use imperative mood ("add" not "added")
- Reference issues when applicable

## Branch Strategy

### Main Branch
- Always stable and deployable
- Protected branch - requires PR
- All submodules track main

### Feature Branches
```bash
git checkout -b feature/user-authentication
git checkout -b fix/login-validation
git checkout -b docs/api-update
```

### Pull Requests
1. Create from feature branch to main
2. Include clear description
3. Link related issues
4. Ensure CI passes
5. Request reviews

## Tips for Monorepo

### Check All Status
```bash
# From root directory
git status
git submodule foreach git status
```

### Update Everything
```bash
git pull origin main
git submodule update --remote --merge
```

### Clean Working Directory
```bash
git submodule foreach git clean -fd
git submodule foreach git checkout .
```