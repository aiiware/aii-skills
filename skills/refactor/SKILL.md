---
name: refactor
description: Refactor code with clear goals and safe transformations
allowed-tools: Read Write Edit Grep Glob Bash
argument-hint: "[file or directory] [goal]"
---

# Refactor

Refactor the specified code to improve its structure, readability, or performance while preserving existing behavior.

## Input

`$ARGUMENTS` should specify the target (file path, function name, or directory) and optionally the refactoring goal. Examples:

- `src/utils.ts` — General cleanup
- `src/auth.ts extract-function` — Extract reusable functions
- `src/api/ simplify` — Simplify API layer
- `src/parser.ts rename` — Improve naming throughout

## Process

### 1. Analyze
- Read the target code and understand its purpose
- Identify callers and dependents (grep for imports/references)
- Check for existing tests
- Review AGENTS.md for project conventions

### 2. Plan
Before making changes, clearly state:
- **Goal**: What the refactoring achieves
- **Scope**: Which files will change
- **Risk**: What could break and how to verify
- **Approach**: Step-by-step transformation plan

### 3. Execute
Apply transformations incrementally:
- Make one logical change at a time
- Preserve public API contracts unless explicitly changing them
- Keep commit-worthy chunks (each step should leave code working)
- Update imports and references across the codebase

### 4. Verify

- Ensure all references are updated (grep for old names)
- Run existing tests if available
- Confirm no dead code or orphaned imports remain

Detect and run the project's test command:

| Language | Test Commands |
| -------- | ------------- |
| TypeScript/JavaScript | `npm test`, `npx vitest`, `npx jest`, `bun test` |
| Python | `pytest`, `python -m unittest` |
| Go | `go test ./...` |
| Rust | `cargo test` |
| Java | `mvn test`, `gradle test` |
| C# | `dotnet test` |
| C/C++ | `ctest`, `make test` |
| Ruby | `bundle exec rspec`, `rake test` |
| PHP | `./vendor/bin/phpunit`, `./vendor/bin/pest` |
| Swift | `swift test` |
| Elixir | `mix test` |
| Dart | `dart test`, `flutter test` |

## Refactoring Techniques

### Structural Refactoring

- **Extract Function/Method** — Break long functions into focused units
- **Inline** — Remove unnecessary indirection (wrappers that add no value)
- **Move** — Relocate code to a more logical module
- **Extract Class/Module** — Split a class doing too much into focused units
- **Collapse Hierarchy** — Merge superclass and subclass when subclass adds nothing
- **Replace Inheritance with Composition** — Use delegation instead of deep class hierarchies

### Naming & Clarity

- **Rename** — Improve clarity of names (variables, functions, types, files)
- **Replace Magic Numbers/Strings** — Extract to named constants
- **Introduce Explaining Variable** — Break complex expressions into named intermediates

### Simplification

- **Simplify Conditionals** — Flatten nested if/else, use early returns / guard clauses
- **Replace Conditional with Polymorphism** — Use method dispatch instead of type-checking switches
- **Remove Dead Code** — Delete unreachable code, unused imports, commented-out blocks
- **Remove Feature Envy** — Move logic to the class/module that owns the data it operates on

### Data & Parameters

- **Reduce Parameters** — Group related params into objects/structs
- **Encapsulate Field** — Replace direct field access with getter/setter when invariants matter
- **Replace Temp with Query** — Replace a temporary variable with a method call

### Duplication

- **Extract Shared Logic** — Only when 3+ occurrences (premature abstraction is worse than duplication)
- **Pull Up / Push Down** — Move shared methods to a common parent, or specialize to subclass
- **Introduce Template Method** — Define algorithm skeleton, let subclasses override steps

### Language-Specific Idioms

| Language | Common Refactorings |
| -------- | ------------------- |
| TypeScript/JS | Replace callbacks with async/await; use optional chaining (`?.`); replace `any` with proper types |
| Python | Replace loops with comprehensions; use dataclasses/NamedTuple; replace `**kwargs` with typed params |
| Go | Replace error string checks with sentinel errors; use interfaces for testability; replace mutex with channels where appropriate |
| Rust | Replace `unwrap()` with proper error handling (`?`); use `impl Trait` over generics when possible; replace `clone()` with borrows |
| Java/Kotlin | Replace anonymous classes with lambdas; use sealed classes; use records/data classes for DTOs |
| C/C++ | Replace raw pointers with smart pointers; use RAII patterns; replace macros with constexpr/templates |
| C# | Replace event handlers with delegates; use pattern matching; use `using` for disposables |
| Swift | Replace force unwraps (`!`) with optional binding; use value types (struct) over reference types where appropriate |
| Ruby | Replace conditionals with guard clauses; use `&.` (safe navigation); extract service objects from fat models |
| PHP | Replace arrays with typed DTOs; use constructor promotion; replace static calls with dependency injection |

### Before / After Visualization

When presenting refactoring changes, show the transformation clearly:

```text
BEFORE                              AFTER
─────────────────────               ─────────────────────
function process(data) {            function process(data) {
  if (data) {                         if (!data) return null;
    if (data.valid) {                 if (!data.valid) return error;
      // ... 50 lines                 return doWork(data);
    } else {                        }
      return error;
    }                               function doWork(data) {
  } else {                            // ... focused logic
    return null;                    }
  }
}
```

Use this format to make the improvement visible. Show the structural change, not just the diff.

## Rules

- Never change behavior unless explicitly asked
- Preserve existing test coverage
- Keep changes minimal and focused on the stated goal
- If the refactoring scope grows beyond the target, stop and report what else should change
- Prefer the simplest transformation that achieves the goal
- Apply language-specific idioms — don't write Java-style code in Python or vice versa
