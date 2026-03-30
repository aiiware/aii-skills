---
name: test-gen
description: Generate unit tests for functions and modules
allowed-tools: Read Write Edit Grep Glob Bash
argument-hint: "[file or function]"
---

# Test Generation

Generate comprehensive unit tests for the specified code.

## Input

`$ARGUMENTS` specifies the target:

- **File** — `src/utils.ts` — Generate tests for all exported functions
- **Function** — `parseConfig` — Generate tests for a specific function
- **Directory** — `src/validators/` — Generate tests for all modules

## Process

### 1. Analyze the Target
- Read the source code to understand inputs, outputs, and side effects
- Identify public API surface (exported functions, methods, classes)
- Note dependencies that need mocking
- Check for existing tests to match style and framework

### 2. Detect Test Framework

Detect the project's test framework by checking config files and existing tests:

| Language | Config File | Frameworks |
| -------- | ----------- | ---------- |
| TypeScript/JavaScript | `package.json` | Jest, Vitest, Mocha, Ava, Bun test |
| Python | `pyproject.toml`, `setup.py`, `setup.cfg` | pytest, unittest, nose2 |
| Go | `go.mod` | Standard `testing` package, testify |
| Rust | `Cargo.toml` | Built-in `#[cfg(test)]`, `#[test]` |
| Java | `pom.xml`, `build.gradle` | JUnit 5, TestNG, Mockito |
| Kotlin | `build.gradle.kts` | JUnit 5, Kotest, MockK |
| C# | `*.csproj` | xUnit, NUnit, MSTest |
| C/C++ | `CMakeLists.txt`, `Makefile` | Google Test, Catch2, CTest, Unity |
| Swift | `Package.swift`, `*.xcodeproj` | XCTest, Swift Testing |
| Ruby | `Gemfile` | RSpec, Minitest |
| PHP | `composer.json` | PHPUnit, Pest |
| Elixir | `mix.exs` | ExUnit |
| Dart | `pubspec.yaml` | `package:test`, Flutter test |
| Zig | `build.zig` | Built-in `std.testing` |
| Shell/Bash | — | Bats, shUnit2 |

If no config file is found, check existing test files and match their patterns.

### 3. Generate Tests

For each function/method, generate tests covering:

**Happy Path**
- Normal inputs with expected outputs
- Common use cases

**Edge Cases**
- Empty inputs (null, undefined, empty string, empty array)
- Boundary values (0, -1, MAX_INT, empty collections)
- Type edge cases (NaN, Infinity for numbers)

**Error Cases**
- Invalid inputs that should throw/return errors
- Missing required parameters
- Malformed data

**Integration Points** (if applicable)
- Mock external dependencies
- Verify correct calls to dependencies
- Test error propagation from dependencies

### 4. Output

Write test files following project conventions:
- **Location**: Adjacent to source (e.g., `utils.test.ts`) or in `tests/` directory — match existing pattern
- **Naming**: Match project convention (`*.test.*`, `*_test.*`, `test_*.*`)
- **Style**: Match existing test style (describe/it, test(), t.Run, etc.)

## Test Quality Rules

- Each test should test ONE behavior
- Test names should describe the expected behavior, not the implementation
- Use descriptive assertion messages
- Avoid testing implementation details — test behavior and contracts
- Keep tests independent — no shared mutable state
- Prefer real values over mocks when practical
- Include setup/teardown only when necessary
