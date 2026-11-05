<!-- SPDX-License-Identifier: MIT -->

# Vyvid Lifecycle Domain

Вивід: домен життєвого циклу розробки

This domain holds the skills for how development work is run, designed, branched and published, kept so that any project can use them. It holds no register.

- `skills/lifecycle` is the procedure for a unit of work. Only a plan and a status record are mandatory. A status record's slice board is the one place a slice's state is written, and the work board reads it.
- `skills/design` is how to write a sketch, a plan or a decision record, and how to put a question to the maintainer.
- `skills/branching` is the git mechanics around a unit of work: the hooks, merging, measuring a branch, worktrees and a task that spans repositories.
- `skills/publication-hygiene` is what may and may not be published from a repository.

Every location and command in a skill is a value, such as `{{status}}` or `{{board}}`, and `domain.toml` gives each its default. A project overrides any of them in its `skill_values`, and adds its own text to a skill with `.vyvid/skills/lifecycle/<skill>.md`.

[MIT](LICENSE)
