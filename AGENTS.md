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

## Memory (mnemosyne)

- At the start of a session, use memory_recall and memory_recall_global to  
  search for context relevant to the user's first message.
- After decisions, when the user asks you to remember something, or when you  
  complete a task, use memory_store to save a concise summary.
- Delete contradicted memories with memory_delete before storing updated ones.
- Use memory_recall_global / memory_store_global for cross-project preferences.
- When you are done with a task store any memories that you think are relevant
  to the user and the project.
