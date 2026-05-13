---
description: Work on the specified plan spec file, or the next item in the roadmap if no file is specified.
agent: build
---

Work on the specified spec file, or on the next item in the roadmap if no file
is specified. Follow each task in order and at the end of each one verify the
acceptance criteria is met, lint, test and build and review the code for
adherence to best practices and the project tech stack and conventions.

If working with a website, run it locally and interact with it using the
playwright-cli skill to verify the changes are working as expected. If you
encounter any issues, debug them and fix them before moving on to the next task.

Once you are done, go back and walk through each task again, making sure you
didn't miss anything and that the implementation is solid. If you find any
issues, fix them before moving on. Ensure the what's implemented adheres to the
acceptance criteria.

Clean up after yourself, delete any temporary files you created, and store any
relevant information in memory that might be useful for future sessions.

Condense the tasks in the plan into a concise summary of bullet points and
update it in the reference doc, add a new completed section if needed. Don't
forget to update the readme, roadmap and any other documentation as necessary to
reflect the changes you made. Use the @docs-writer subagent for this if needed.
