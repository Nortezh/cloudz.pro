---
description: "Implement feature following KISS principle with mandatory test gates"
allowed-tools: ["TodoWrite", "TodoRead", "Read", "Edit", "MultiEdit", "Write", "Bash", "Glob", "Grep"]
extended-thinking: true
---

# /implement - Enhanced TDD Implementation with KISS + Test Gates

@/Users/l_pasawee/Documents/ghq.nosync/github-k-toshio/Nortezh/cloudz.pro/doc/enhanced-development-workflow.md@

## Implementation Task

**Feature to implement:** $ARGUMENTS

## 🎯 Mandatory Success Criteria

✅ **KISS Principle**: Code must be "stupid simple" - junior dev understands in 5 minutes  
✅ **Test Gate**: `pnpm test && pnpm test:e2e` MUST pass 100% before completion  
✅ **Functions**: <20 lines, avoid nesting >2 levels, self-documenting names  
✅ **Abstractions**: Only create when pattern repeats 3+ times  

## 🚀 Implementation Workflow

### Phase 1: Discovery & Planning (REQUIRED)
1. **TodoWrite**: Break feature into SIMPLE tasks
2. **Exploration**: Use Read/LS/Glob to understand existing patterns  
3. **Architecture**: Choose SIMPLEST viable solution
4. **KISS Check**: Validate approach is "stupid simple"
5. **Pre-flight**: Ensure current tests pass

### Phase 2: TDD Implementation (MANDATORY TEST GATES)
1. **RED**: Write minimal failing tests
2. **GREEN**: Implement SIMPLEST solution that works
3. **REFACTOR**: Clean up while keeping it STUPID SIMPLE
4. **TEST GATE**: Run `pnpm test && pnpm test:e2e` - MUST PASS 100%
5. **TodoWrite**: Mark progress ONLY if all tests pass

### Phase 3: Validation & Completion (FINAL GATES)
1. **FINAL TEST GATE**: Complete test suite - ZERO failures allowed
2. **KISS Validation**: Review for unnecessary complexity
3. **Documentation**: Update CLAUDE.md with simple solutions
4. **TodoWrite**: Mark complete ONLY after 100% test pass

## 🚨 FAILURE RESPONSE PROTOCOL

**If Tests Fail:**
- STOP immediately - do not proceed
- Fix failing tests before continuing
- Re-run full test suite to validate
- Simplify approach if fix is complex

**If Code Too Complex:**
- Pause implementation
- Find simpler solution
- Restart with simpler approach
- Validate solution is "stupid simple"

## ✅ Definition of Done (ALL REQUIRED)

- [ ] Feature works as intended
- [ ] Code follows KISS principle (stupid simple)
- [ ] 100% unit tests pass (`pnpm test`)
- [ ] 100% E2E tests pass (`pnpm test:e2e`)
- [ ] Zero TypeScript errors (`pnpm typecheck`)
- [ ] CLAUDE.md updated with learnings
- [ ] TodoWrite marked complete

## 🚨 Red Flags - DO NOT PROCEED

🚨 Any test failures (unit or E2E)  
🚨 Code requires extensive comments to understand  
🚨 Implementation has >3 levels of abstraction  
🚨 Junior developer couldn't modify in 30 minutes  
🚨 Function longer than 20 lines  
🚨 Nested conditions >2 levels deep  

---

**Begin implementation following this enhanced workflow. Remember: SIMPLE solutions that pass ALL tests are better than clever solutions that break things.**