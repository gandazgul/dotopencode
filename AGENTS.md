Before starting a session make sure the repo is clean with git status. If there
are uncommitted changes, ask the user if they want to commit or stash them
before proceeding.

Use @docs-writer subagent for all documentation related tasks. This includes
writing new documentation, updating existing documentation, and maintaining the
overall structure of the documentation. Invoke it when significant changes have
been made to the project that require documentation updates, or when the user
explicitly requests documentation work.

Use @playwright-tester subagent for all end-to-end testing tasks. This includes
writing new tests, updating existing test coverage, and verifying UI
functionality. Invoke it when new UI has been created, when existing UI is
modified, or when the user explicitly requests Playwright tests to be created or
run.

Remember to use tavily and context7 to search the web and get documentation for
libraries, frameworks, and tools. Use these mcp servers to gather information
and insights that can help you make informed decisions and up to date
implementations.

## Memory (mnemosyne)

- At the start of a session, use memory_recall and memory_recall_global to  
  search for context relevant to the user's first message.
- After decisions, when the user asks you to remember something, or when you  
  complete a task, use memory_store to save a concise summary.
- Delete contradicted memories with memory_delete before storing updated ones.
- Use memory_recall_global / memory_store_global for cross-project preferences.
- When you are done with a task store any memories that you think are relevant
  to the user and the project.

## Code Search (cymbal)

Use `code_search` / `code_show` (cymbal) before opening files or running broad
text grep to find where logic lives or how a feature works.

- `code_search "symbol"` to find a function, class, or type across the codebase
- `code_search --textSearch` for full-text regex grep (fallback for textual
  patterns)
- `code_show <symbol>` to read the exact source of a specific symbol
- `code_outline <file>` to see all symbols defined in a file at a glance
- `code_refs <symbol>` to find all callers and references to a symbol
- `code_impact <symbol>` to measure transitive blast radius before modifying
- `code_trace <symbol>` to explore caller/callee graphs visually
- `code_investigate <symbol>` for an in-depth breakdown of complex symbols
- `code_structure` for a project overview (entry points, hotspots, packages)
- `code_impls <symbol>` to find types that implement a given interface
- `code_importers <file>` to find everything that imports a file or package
