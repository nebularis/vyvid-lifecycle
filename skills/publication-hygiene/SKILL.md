---
name: publication-hygiene
description: What may and may not be published from a repository. Use before committing, before opening or describing a pull request, and whenever writing anything that will be public, such as documentation, code comments, commit messages, examples, release notes, environment notes or agent memory kept in the repository.
---

# Publication hygiene

This skill's tasks span the activities documentation, licensing-and-ip, secrets-management and privacy-engineering, with the qualities confidentiality and privacy, and a task often needs several of them, named together and separated by commas. Read each row's flags and the line under it, where an entry's boundary, its successor and the debt against it are named.

This repository is {{visibility}}, and so is everything it publishes. Anything committed stays in its history, so the time to catch a problem is before the commit.

## Never publish

- names or details that identify a person or a client, beyond what the project itself names
- credentials, tokens, keys, private hostnames, internal URLs, mirror or index addresses, private repository paths
- notes true of one machine or one person's setup, such as local paths, installed versions, network behaviour or remotes. Keep them in that person's own configuration or agent memory
- text from a confidential source, quoted or close to it

## Sources and examples

- **Anonymise a real source** before it enters a sketch or an example. Replace organisations, people, places and reference numbers with neutral placeholders, and keep what the example needs to show.
- **Documents of a general-purpose layer stay domain-neutral.** Their examples come from several domains, and never cite a downstream project. Sketches and plans may name downstream projects.
- **Generated content drifts toward the domains a model has seen most.** Check AI-drafted examples for it, as the project's AI contribution policy asks.
- **Placeholder IRIs use `example.org`**, never a real organisation's domain.

## Delivery identifiers stay in working documents

How work was planned and delivered is not part of what a published document means. Production text never names it. Production text is {{production_text}}.

- **Never in production text:** slice and phase identifiers (`ABC-1`, `XYZ-4.2`), question identifiers (`ABC-Q3`, `XYZ-Q2`), unit, epic and plan names used as labels (`ABC`, `XYZ`), technical debt and test row identifiers (`DEBT-49`, `ABC1-02`), and delivery phrasing such as "this slice", "the plan's brief", "flagged at", "at verification", "not yet written in ABC-2" or "open for the maintainer".
- **Say what instead.** State the fact the identifier stood for. A term not yet specified is "not yet specified". A rule decided in a published decision cites that decision. A modelling choice is stated as the choice, with its reason. A release note says what changed, never which slice changed it.
- **Production text cites only published decisions.** A citation of a decision that is not yet published waits until it is, or the text cites the published text that states the rule instead.
- **Working documents keep them.** {{working_documents}} use delivery identifiers freely, since that is what they are for.
- **Existing occurrences are debt, not precedent.** Some older documents and release notes still name slices. Do not copy them. Remove them when you next edit that text.
- **Check before handing off.** Search what you wrote in the production text for slice, question and debt identifiers and for the word "slice". Where a section is extracted into a generated file, the identifier is carried into it as well, so fix the source and regenerate.

## AI disclosure

If an AI agent is making commits, then the project's AI contribution policy, where it has one, governs the disclosure rules. In short:

- The trailer must survive a squash merge.
- AI-drafted text published under the project's name is reviewed by a person first, or labelled as AI-generated.

If a human is making the commit then disclosure is their responsibility.

## The deny-list check

Where a project has a deny-list check, {{deny_check}} checks text about to be committed against a personal list of terms, which is never published. It reads the first of these that exists, and passes when none does:

1. the file named by the variable the project sets
2. `${XDG_CONFIG_HOME:-~/.config}/<project>/deny-terms` on macOS and Linux
3. `%APPDATA%\<project>\deny-terms` on Windows

The file holds one case-insensitive regular expression per line, with `#` for comments. A line starting `!` names a path glob to skip, for a file where a term is legitimate. By default the check reads only the lines being committed (`git diff --cached`), and `--all` scans the tracked tree. A finding names the file, the line and the pattern's line number in the list, never the pattern.

## When something slipped through

Remove it in a new commit, and tell {{maintainer}}. Rewriting published history is {{maintainer}}'s decision, never the agent's.
