---
name: tidy-repo
description: Keep a repository organized, findable, and professionally clean. Split oversized files, refactor structure safely, remove dead clutter and AI-slop docs/code, and raise readability without changing behavior by accident. Use when the user asks to tidy, organize, clean up, refactor structure, split files, or make the codebase less messy or less AI-generated.
---

# Tidy Repo

Make the tree look like a careful human owns it: clear folders, small enough files, readable modules, easy to find things, no junk, no AI slop.

This skill is about **structure, clarity, and hygiene**. It is not a feature dump and not a drive-by rewrite of product behavior.

Influences: repo-janitor style cleanup, Kent Beck tidy-first (structure before behavior), pragmatic incremental refactoring.

## When to use

- "Tidy the repo" / "organize this" / "clean up the project"
- Files or folders feel sprawling, duplicated, or hard to navigate
- Need to split a god file / extract a module
- Docs and scripts look AI-generated, hyperbolic, or session-logged
- Pre-ship polish: professional layout before handoff or open source

## When to go light or skip

- Urgent bugfix with a clear one-line change (do not "tidy" the neighborhood unasked)
- User only wants a behavior change: still prefer a tiny structural prep if it unblocks the fix, but do not expand scope
- Unknown legacy: ask before deleting

## Operating principles

1. **Findable over clever.** A new contributor should guess where code lives.
2. **Small enough to read.** Split when a file mixes jobs or exceeds comfortable review size.
3. **Tidy-first.** Structural changes (move, rename, extract) stay behavior-neutral and preferably separate from feature/bug commits.
4. **Ask before destructive deletes** when unsure (legacy paths, scripts, "unused" packages).
5. **No AI slop.** Strip fake polish in docs and empty abstraction in code. See [references/ai-slop-tells.md](references/ai-slop-tells.md).
6. **Verify moves.** Update imports, configs, CI, and docs that pointed at old paths. Run tests or a smoke check after structural work.

## Workflow

Run steps in order. Do not parallelize moves that collide. Report what you did after each major step.

### 1. Survey

Map the repo before touching it:

- Top-level layout and entrypoints
- Where app code, packages, scripts, docs, and assets live
- Obvious clutter: duplicate folders, root junk, generated artifacts checked in, session markdown
- Hotspots: oversized files, `utils` dumps, copy-paste `Final2` modules
- Existing conventions (feature folders vs layer folders). **Match the house style** unless the user asked for a redesign.

State a short plan: what you will organize, split, delete (pending confirm), and leave alone.

### 2. Separate concerns at the top level

Non-deployable material (docs, notes, reference) should not be tangled inside deployable app roots without reason. Prefer clear homes, for example:

```text
src/ or app/     product code
packages/        shared libraries (monorepo)
scripts/         maintained tooling
docs/            real documentation
public/ or static/  shipped assets
```

Do not invent a heavyweight taxonomy the project does not need. Flatten until a folder earns its name.

### 3. Organize for findability

- Group by **feature/domain** when the product is feature-shaped; keep **layer** layout if the repo already consistently uses it
- One idea per folder; avoid `misc/`, `old/`, `temp/`, `new-new/`
- Keep paths shallow enough to scan
- Align names with product language (`billing/`, `auth/`), not vague `core/`/`shared/` unless they already mean something here

Before moving: search for path string references (imports, CI, Docker, README). After moving: same search for old paths.

### 4. Split when needed

Split a file when **any** of these hold:

- Multiple unrelated responsibilities
- Hard to review in one sitting (rule of thumb: hundreds of noisy lines with mixed concerns)
- UI + data fetching + pure utils tangled with no seams
- Tests cannot target one behavior without dragging the world

How to split:

1. Identify seams (types, pure helpers, components, data adapters)
2. Extract with behavior unchanged (tidy-first)
3. Name the new module for its job
4. Re-export temporarily only if callers need a soft landing; remove shims once call sites move
5. Run tests / typecheck / smoke

Do **not** split into microscopic files that force hopscotch through twenty modules for one idea.

### 5. Refactor for readability (still structural)

Prefer:

- Clear names over comments that apologize
- Straight-line control flow over clever nesting
- Real error handling over swallowed catches
- Shared constants/types in one obvious place
- Deleting dead code and unused exports once proven unused

Avoid:

- Rewriting working code in a new "clean architecture" for sport
- Introducing patterns the codebase does not use (DI containers, factories) without need
- Mixing bugfixes into the tidy pass unless the user asked

### 6. Remove clutter (with confirmation when unsure)

Candidates:

- Abandoned feature flags and unreachable branches
- One-off diagnostic scripts that are not part of the product toolchain
- Checked-in build output and dependency directories
- Duplicate docs and agent session logs
- Empty or placeholder packages

**Ask the user** before deleting anything that might be load-bearing, undocumented ops tooling, or temporarily unused.

### 7. Clean documentation

On remaining product docs (README, `docs/`):

- Accurate, succinct, matching the real tree
- Setup steps that work
- No hyperbolic AI tone; no emoji storms unless brand-required
- Prefer one good README section over five status markdown files

Skip rewriting agent skill/prompt instruction files unless the user asked.

### 8. Final pass

Walk [references/quality-checklist.md](references/quality-checklist.md). Summarize:

- Structure changes (moves/renames)
- Splits/extractions
- Deletes (and anything skipped pending user input)
- Doc cleanups
- Verification run (tests, typecheck, smoke)

## Code quality bar

Professional readable code in this skill means:

| Prefer | Avoid |
| --- | --- |
| Intentional names | `data2`, `temp`, `handleStuff` |
| One job per module | God components / god services |
| Small pure helpers next to use | Endless generic `utils` |
| Explicit types at boundaries | `any` everywhere |
| Comments for non-obvious why | Narrating `i++` |
| Consistent style with the repo | Personal formatter war |

Match existing lint/format tooling. Do not add new tooling in a tidy pass unless asked.

## Safety rules

- Never mix large structural renames and behavior changes in one opaque blob of work when you can sequence them
- Never update git config; never destructive git (`push --force`, hard reset) unless the user explicitly asks
- Never commit unless the user asks
- Never delete secrets-bearing files without care; never invent replacements for unknown credentials
- Extra caution on deploy paths, CI workflows, package `exports`, and public API surfaces

## Anti-patterns

- "Cleaning" by generating more markdown about the cleanup
- Moving everything into `clean-architecture/` folders in one heroic PR
- Renaming half the codebase for aesthetics mid-feature
- Deleting code you do not understand to raise "tidiness"
- Leaving broken imports after a move

## File map

| Path | Role |
| --- | --- |
| [references/quality-checklist.md](references/quality-checklist.md) | End-of-pass verification checklist |
| [references/ai-slop-tells.md](references/ai-slop-tells.md) | Doc/code/structure tells to remove |
