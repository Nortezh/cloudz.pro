---
description: Navigate to specific project directory in the monorepo
allowed-tools: Bash(cd:*, pwd:*)
---

# Change Working Directory

Quick navigation between monorepo project directories.

## Usage

```
/workdir <directory>
```

Where `<directory>` can be:
!`PROJECT_ROOT=$(git rev-parse --show-toplevel 2>/dev/null || pwd); find "$PROJECT_ROOT" -maxdepth 1 -type d ! -path "$PROJECT_ROOT" ! -name ".*" ! -name "node_modules" ! -name "build" ! -name "dist" | sort | while read dir; do name=$(basename "$dir"); echo "- \`$name\` - Navigate to /$name directory"; done; echo "- \`root\` - Navigate to project root directory"`

## Examples

```
!`PROJECT_ROOT=$(git rev-parse --show-toplevel 2>/dev/null || pwd); find "$PROJECT_ROOT" -maxdepth 1 -type d ! -path "$PROJECT_ROOT" ! -name ".*" ! -name "node_modules" ! -name "build" ! -name "dist" | head -2 | while read dir; do name=$(basename "$dir"); echo "/workdir $name"; done; echo "/workdir root"`
```

## Command Implementation

```bash
# Check current directory first
pwd

# Find project root (git repo or current directory)
PROJECT_ROOT=$(git rev-parse --show-toplevel 2>/dev/null || pwd)

# Navigate based on argument
if [ "$ARGUMENTS" = "root" ]; then
  cd "$PROJECT_ROOT"
elif [ -d "$PROJECT_ROOT/$ARGUMENTS" ]; then
  cd "$PROJECT_ROOT/$ARGUMENTS"
else
  echo "Available options:"
  find "$PROJECT_ROOT" -maxdepth 1 -type d ! -path "$PROJECT_ROOT" ! -name ".*" ! -name "node_modules" ! -name "build" ! -name "dist" | sort | while read dir; do
    echo "  $(basename "$dir")"
  done
  echo "  root"
  exit 1
fi

# Confirm new location
pwd
echo "✅ Switched to $(basename $(pwd)) project directory"

# Show quick info about current project
if [ -f "package.json" ]; then
  echo "📦 Package manager: $([ -f "bun.lockb" ] && echo "bun" || echo "pnpm")"
elif [ -f "go.mod" ]; then
  echo "🐹 Go project detected"
fi
```

## Notes

- Uses dynamic project root detection (git or current directory)
- Portable across different environments and projects
- Confirms successful directory change
- Shows relevant project info (package manager)
- Follows monorepo navigation patterns from CLAUDE.md