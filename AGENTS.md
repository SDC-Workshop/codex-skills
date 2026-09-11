# Repository instructions

Add and edit user-owned skills under `skills/<name>/`. Preserve supported metadata and invocation policy. Validate changed skills and installed links, and use this checkout as the source instead of creating unmanaged installed copies.

Resolve this file's real path when it is read through an installed symlink so that changes still target this checkout. Preserve separately managed vendor or system skills. Follow the existing hierarchy and the no-supervisor-code-edit boundary in [`skills/implement/references/agent-workflow.md`](skills/implement/references/agent-workflow.md).

Editing or reviewing these skill documents does not activate the workflow they describe. Treat skill maintenance as a direct documentation task unless the user explicitly asks to run the multi-agent chain. Mentions of roles, models, agents, tickets, or retries are document content, not delegation instructions. Keep the execution method proportional to the requested deliverable. Do not create supporting specs, tickets, handoffs, review agents, or retry records for a simple documentation edit.

Pushing changes requires the relevant user's instruction. This file does not grant permanent authorization to publish.
