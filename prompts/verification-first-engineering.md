# Verification-First Engineering Protocol

## Overview

A rigorous engineering protocol that avoids both dumbing down (over-simplification) and gold-plating (over-engineering). This protocol demands evidence-based decision making and maintains strict control over change blast radius while systematically untangling legacy complexity.

## Core Principles

### 1. Evidence-Based Decisions
- **Demand grep-level evidence**: Every claim about code behavior must be backed by concrete evidence (actual code snippets, grep results, traces)
- **Real call paths**: Trace actual execution paths rather than assumed behavior
- **No folklore**: Question and verify tribal knowledge against actual code

### 2. Blast Radius Control
- Make minimal, surgical changes
- Understand dependency chains before modification
- Isolate changes to limit impact surface
- Verify changes don't break existing behavior

### 3. Complexity Management
- **Separate essential vs accidental complexity**
  - Essential: inherent to the problem domain
  - Accidental: artifacts of implementation choices
- **Preserve invariants**: Identify and maintain critical system guarantees
- **Delete redundant folklore**: Remove documentation and code that no longer reflects reality

### 4. Ghost Wiring Detection
- **Untangle legacy "ghost wiring"**: Dead code paths, obsolete configurations, phantom dependencies
- Identify: Use grep/search to find all references
- Verify: Confirm which paths are actually used
- Remove: Safely eliminate unused code after verification

## The Protocol (Recursive Process)

### Phase 1: Frame Unknowns
- What do we know?
- What do we need to know?
- What assumptions are we making?
- Document knowledge gaps explicitly

### Phase 2: Verify Topology
- Map actual system structure with evidence
- Identify all entry points and call paths
- Document dependencies with concrete references (file:line)
- Distinguish between design docs and reality

### Phase 3: Pressure Test
- Challenge assumptions with evidence
- Look for counterexamples in codebase
- Test edge cases conceptually
- Identify potential failure modes

### Phase 4: Build
- Make minimal, targeted changes
- Preserve existing invariants
- Document why changes are necessary (not just what changed)
- Avoid scope creep

### Phase 5: Validate Flow
- Test actual execution paths (not just happy paths)
- Verify no unintended side effects
- Check that invariants still hold
- Confirm blast radius is contained

### Phase 6: Report with Proof
- Document changes with evidence
- Show before/after comparisons
- Include test results, traces, or other proof
- Explain what was preserved and why

## Recursion
When encountering new unknowns or complexity:
- Return to Phase 1
- Maintain context from previous iterations
- Build understanding incrementally

## Anti-Patterns to Avoid

### Dumbing Down
- ❌ Ignoring edge cases "to keep it simple"
- ❌ Removing complexity without understanding it
- ❌ Assuming "it probably works like..."
- ❌ Skipping verification "because it's obvious"

### Gold Plating
- ❌ Adding abstraction layers "for future flexibility"
- ❌ Implementing features before they're needed
- ❌ Over-engineering solutions to simple problems
- ❌ Creating frameworks when scripts suffice

### Evidence Failures
- ❌ "I think the code does X" without grep proof
- ❌ Trusting comments over actual code
- ❌ Following outdated documentation
- ❌ Accepting hearsay about system behavior

## Key Questions to Ask

1. **Where is the evidence?**
   - Can I grep for this?
   - What does the actual code say?

2. **What's the blast radius?**
   - What depends on this?
   - What will break if I change it?

3. **Is this essential or accidental?**
   - Does this complexity solve the core problem?
   - Or is it a workaround/artifact?

4. **What invariants must hold?**
   - What guarantees does the system provide?
   - What assumptions do callers make?

5. **Is this ghost wiring?**
   - Is this code actually used?
   - When was it last modified/called?

## Usage Examples

### Example 1: Investigating a Function
```
Frame: "What does processData() actually do?"
Verify: grep -r "processData" → find all call sites
Topology: Map callers → identify patterns
Pressure Test: Are there untested branches?
Build: Add logging/tests for unclear paths
Validate: Run with real data, observe behavior
Report: "processData handles 3 cases (with evidence)..."
```

### Example 2: Removing Dead Code
```
Frame: "Can we remove the legacy Parser?"
Verify: grep -r "LegacyParser" → find references
Topology: Check imports, configs, reflection usage
Pressure Test: Any runtime/dynamic loading?
Build: Comment out → run full test suite
Validate: All tests pass, no production errors
Report: "Removed LegacyParser (unused since commit abc123)"
```

### Example 3: Refactoring Complex Logic
```
Frame: "Why is calculateTotal() 300 lines?"
Verify: Analyze actual branches taken (add logging)
Topology: Map inputs → outputs → side effects
  - Essential: Tax calculations, discounts, currency
  - Accidental: Caching bug workarounds, dead branches
Build: Extract essential, remove accidental
Validate: Compare outputs for 1000 test cases
Report: "Reduced from 300 → 80 lines, same behavior (proven)"
```

## Checklist for Each Change

- [ ] Have I gathered grep-level evidence?
- [ ] Do I understand the real call paths?
- [ ] Have I identified the blast radius?
- [ ] Have I separated essential from accidental complexity?
- [ ] Have I checked for ghost wiring?
- [ ] Are system invariants preserved?
- [ ] Can I prove the change works?
- [ ] Is my report backed by evidence?

## Benefits

1. **Avoids regressions**: Evidence-based changes reduce surprises
2. **Reduces technical debt**: Systematically removes cruft
3. **Maintains quality**: Preserves what works, fixes what doesn't
4. **Builds understanding**: Forces deep comprehension before changes
5. **Enables confidence**: Proof-based validation gives certainty

## When to Use

- Legacy codebases with unclear behavior
- Critical systems requiring high reliability
- Complex refactoring efforts
- Technical debt cleanup
- When "tribal knowledge" conflicts with reality
- Before making changes to unfamiliar code

---

*Remember: The goal is not perfection, but verified correctness within a controlled blast radius.*
