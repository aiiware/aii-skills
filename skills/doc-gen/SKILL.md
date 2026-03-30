---
name: doc-gen
description: Generate documentation for code and APIs
allowed-tools: Read Grep Glob
argument-hint: "[file, module, or scope]"
---

# Documentation Generation

Generate clear, accurate documentation for the specified code.

## Input

`$ARGUMENTS` specifies the target and optional format:

- **File** — `src/api/client.ts` — Document all exports
- **Module** — `src/auth/` — Document the module's API
- **Function** — `createSession` — Document a specific function
- **Scope keyword** — `api` — Document all API endpoints
- **Format hint** — `src/utils.ts jsdoc` — Use specific doc format

## Process

### 1. Analyze
- Read the source code thoroughly
- Identify the public API surface
- Trace usage patterns (how is this code called?)
- Check for existing documentation style

### 2. Detect Format
Match the project's documentation conventions:

| Language | Default Format |
|----------|---------------|
| TypeScript/JavaScript | JSDoc (`/** */`) |
| Python | Docstrings (Google or NumPy style) |
| Go | Godoc comments (`// FuncName ...`) |
| Rust | Rustdoc (`/// ...`) |
| Java/Kotlin | Javadoc (`/** */`) |
| C/C++ | Doxygen (`/** */` or `///`) |
| C# | XML doc comments (`/// <summary>`) |
| Swift | DocC (`/// ...` with Markdown) |
| Ruby | YARD (`# @param`, `# @return`) |
| PHP | PHPDoc (`/** */`) |
| Elixir | ExDoc (`@doc """..."""`, `@moduledoc`) |
| Lua | LDoc (`--- ...`, `-- @param`) |
| Dart | DartDoc (`/// ...`) |
| Zig | Doc comments (`/// ...`) |
| Shell/Bash | Header comments (`# ...`) |

If existing docs exist, match their style exactly.

### 3. Generate Documentation

For **functions/methods**:
- Brief description (one line)
- Parameter descriptions with types
- Return value description with type
- Throws/errors documentation
- Usage example (when non-obvious)

For **classes/structs**:
- Class purpose and responsibility
- Constructor parameters
- Public method documentation
- Usage example

For **modules/packages**:
- Module overview (what it does, when to use it)
- Key exports and their relationships
- Common usage patterns
- Configuration options (if applicable)

For **API endpoints**:
- HTTP method and path
- Request parameters (path, query, body)
- Response format with examples
- Error responses
- Authentication requirements

### 4. Output

- Write documentation inline (as code comments) by default
- If asked for standalone docs, use Markdown format
- Include code examples for non-trivial APIs

## Documentation Rules

- Be accurate — every statement must be verifiable from the source code
- Be concise — say what it does, not how it works internally
- Document the "why" for non-obvious design decisions
- Use the project's existing terminology consistently
- Include types even if the language doesn't require them
- Do not document obvious getters/setters or trivial wrappers
- Keep examples minimal but complete (runnable when possible)
