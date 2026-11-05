---
name: design
description: How to design in a project. Use when writing or revising a sketch, a plan or slice brief, a decision record, or design questions for the maintainer, when presenting options for an architecture or modelling decision, and when a design adds a component, a dependency or a derived artefact.
---

# Designing

## Design first

Ask the index first, with every design activity that fits the work, such as solution-design, ontology-design, physical-data-modelling or integration-design, and the phase the design runs in. A design often needs several activities, and `--kind` filters exactly, so leave it off unless only one kind of entry will do. Read each row's flags and the line under it, where an entry's boundary, its successor and the debt against it are named, and add `question` among the kinds when the roadmap matters. Where it answers weakly, read the documents as before, and report any answer they hold that no register entry does.

Before planning code, find the decisions and architecture sections the change touches, with `find <the change> --kind decision` and `read`, and propose a decision record with either a design sketch or a detailed entry in the design specification. Keep these current with every change they cover:

- the root `README.md` (new structure, folders or projects) and each project's `README.md`
- the decision catalogue, `{{decisions}}/README.md`

A plan for code is detailed enough to check its alignment with the architecture and the design specification.

## Standards for a design

- **A class, column, status or classification is added only when some query or check answers differently because of it.** A distinction that changes no answer is one more thing to keep consistent, for nothing.
- **A fact a query or check compares is a value from a closed set, never free text.** Free text cannot be compared and drifts into variants.
- **A section, row or definition is referred to by a name that survives an edit, never by its position.** A line number or a column index is right until the next edit, then silently wrong.
- **A thing is recorded once, however many documents mention it, and every other mention points at that record, but a node is never shared across an aggregate boundary or where concurrent writers would contend for it.** Two records of one thing diverge. A derived restatement, such as a new version, a projection to a lower layer or a promoted value, is not a second record, since it is derived and never edited.

## Standards for structure

- **An architectural quantum is the smallest set that must change and be released together because of its coupling, so its size is the lower bound on incremental change.** It grows with every dependency, and is kept small by isolating change, refusing an unnecessary dependency, and insulating a boundary behind a pinned contract, an abstraction or a gate. A quantum is derived from coupling, never declared, and in each view separately. In the software view a quantum is per deployment: components sharing a store, or calling one another synchronously, are one quantum, and a broker separates them only where nothing else couples them, so one deployment of the same code can form different quanta from another. The process view is the third.
- **Every architectural and design standard has a check that fails when it is broken, or a recorded reason it cannot.** A standard nothing checks holds only while an agent remembers it, and the index lists the ones nothing enforces.
- **Whatever a tool can derive is derived and never edited by hand, and committed only where a consumer fetches it from the repository, with a check that it regenerates unchanged.** A fact keeps one source, and an edited copy of a derived one becomes a second source that drifts. Nothing a build writes into its output folder is tracked.
- **A component added to remove a cost, such as a resident server or a cache, is added only once that cost is measured and recorded.** How data is spread across stores and graphs is a design of its own, and this rule does not decide it.

## Writing a sketch or a plan

- Re-read the latest version of everything the design touches. `reaches <ID>` names what an entry touches, and `read <ID>` returns its current text. Never work from memory, and above all never describe a component's behaviour without re-reading it.
- Check each modelling consequence against the model itself. To find the terms a design would reuse or collide with, ask `find <what the term means> --kind term`, separately from the questions about rules and decisions, which never take `--kind term`.
- Check the design against the roadmap: the epic plans, the held design questions, which `find <the design> --kind pending-decision` returns, and the slices waiting on this one. A design that works now and makes a roadmap item harder is wrong.
- Say a design decision once, and cross-reference it.
- A design that depends on how a triple store evaluates SPARQL cites the finding in the triple-stores domain it rests on, or probes the store and adds one first (skill `probing`). The specification and a store often differ.
- A design that generates a query, or adds a SHACL-SPARQL constraint, states its cost in the terms of the triple-stores domain's query rules (skill `query-writing`): how many focus nodes or solutions it runs over, and which of its rules it relies on. Say which engine each speed claim is for.
- A plan's header declares the unit's slice prefix, `**Prefix:** ABC`, and never names a branch, since the board derives the branch from where it finds a slice's row. Every slice the plan names has a row on the unit's board from the day the plan is written, `planned` until it moves.

## Presenting a question to {{maintainer}}

1. **Set the scene.** What exists today, what is missing, and why it matters. State the facts about the current model that constrain the options, each checked against the source.
2. **Draw pictures, by default.** Several, one per view the option changes: the data flow (where each value comes from, for example compile, the caller or the query, and which graph or file it lands in), the state before and after, a timeline of the failure cases (a race, a partial write, a retry), and the options side by side. Show them inline when presenting, and put the ones that last into the plan or sketch as mermaid, which renders on GitHub. Check every diagram renders before handing off, for example by loading it with mermaid in a browser. Three rules, each found the hard way. Labels are ASCII only, because non-ASCII punctuation such as an em dash, and HTML entities, fail in some renderers. An edge targets a node, never a subgraph's own container ID, which breaks rendering. In a `classDiagram` a relation is `A --> B : label`, never `A : label --> B`, which silently declares a member instead.
3. **Each option, then its consequences**, compared on:
   - *design overheads*: is it hard to reason about, easy to model incorrectly, hard to assure or govern, or brittle under likely change
   - *runtime overheads*: does it explode the data, invite inconsistency, block common traversals or processing, or make common operations, such as aggregating or summing a measure, behave in surprising ways. Where the option writes terms at runtime, compare it on all four drivers: defensible history, storage, retrieval and throughput
   - *space and time, quantified*: the order of each cost (triples stored per write, template fills or solutions per update, round trips, reads to reconstruct), then a worked example with stated numbers in a table. Name where an option's cost crosses another's
4. **KISS,** applied to a model that already reflects reality, never by waving reality away. Is it needed now? If not, is it needed at all? If later, can it wait, or is there value in doing it now? Is it as simple as it can be? KISS never overrides architectural conformance, non-functional requirements or standards in force.
5. **A leaning**, with its reason. Then wait for the answer.

Name each question with the unit's prefix (for example ABC-Q1) so answers can be recorded against it. Record the answers in the plan or sketch, and in the status record's history.

A question raised by a bold lead, `**ABC-Q7. <question>**`, or by a table row whose first cell is its identifier, is an entry of the index, open until its text says Answered with the date, as in `Answered 2026-10-19, (a).` An answer that states a rule a later slice must obey is promoted in the same change to a standard, a mechanism or a decision addendum, and the answer then names it.

## Decisions

- Decision records live only in `{{decisions}}/`, and are listed in its `README.md`. Take the next free number from that catalogue, and re-check it before merging, since parallel work can claim the same number.
- A decision record states context, decision and consequences. A decision found while building is recorded as an addendum, dated, rather than by rewriting the accepted text.
- An addendum that reverses or narrows a numbered decision of its file opens with a bold lead naming each, `**Reverses decision 8.**` or `**Narrows decisions 3 and 6.**`, so the index marks the text it changed and sends a reader of it to the addendum. A whole decision replaced by another says so in its Status line.
- The agent drafts as Proposed. Only {{maintainer}} accepts.

## Links and paths

- New links use canonical paths.
- Move files with `git mv`, and update every source, configuration, CI, documentation and website reference in the same change. A historical record may keep an old path only with a relocation note.
