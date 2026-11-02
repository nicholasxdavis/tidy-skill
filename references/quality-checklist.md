# Tidy Repo Quality Checklist

Use after a tidy pass or before calling the repo "clean."

## Structure

- [ ] Deployable code and non-deployable docs live in clear top-level homes
- [ ] Folder names match domain / feature language, not random leftovers
- [ ] No orphan folders with one random file and no purpose
- [ ] Entrypoints are obvious (`src/`, `app/`, `packages/`, `cmd/`, etc.)
- [ ] Generated output is gitignored or clearly labeled (`dist/`, `.next/`, `coverage/`)
- [ ] Secrets stay out of the tree (`.env` gitignored; no keys in code)

## Findability

- [ ] Someone new can find "where does X live?" in under a minute
- [ ] Related modules sit near each other
- [ ] Public APIs have one clear import path (no duplicate barrels that confuse)
- [ ] README points to the real layout (not a fantasy diagram)

## File size and splits

- [ ] No kitchen-sink god files without a reason
- [ ] Components/modules do one job
- [ ] Shared utils are real shares, not a dumping ground named `helpers.ts`
- [ ] Tests sit next to code or in a mirrored `__tests__` / `tests` layout consistently

## Code quality

- [ ] Names describe intent
- [ ] Functions/classes have a single main responsibility
- [ ] Dead code, dead exports, and commented-out blocks removed (or ticketed)
- [ ] Error paths are real, not empty `catch` or `// TODO handle`
- [ ] Types/interfaces match reality (no `any` carpet bombing)
- [ ] Imports are tidy; unused imports gone
- [ ] Formatting matches the project (prettier/eslint/ruff/etc. if present)

## Docs quality (anti-slop)

- [ ] No emoji spam in docs unless the project already uses them as brand
- [ ] No "comprehensive / robust / seamless / cutting-edge" filler
- [ ] No agent session logs or "what I changed today" markdown in the repo
- [ ] Useful notes folded into README or real docs, not twelve status files
- [ ] Setup steps actually work when followed

## Safety

- [ ] Structural changes did not change behavior (tests / smoke still pass)
- [ ] Moves updated all imports, configs, CI paths, and docs that pointed at old paths
- [ ] Uncertain deletes were confirmed with the user
- [ ] No force-push / history rewrite unless the user asked

## Professional finish

- [ ] Root is calm: README, license, config, source, docs - not a junk drawer
- [ ] Scripts in `package.json` / `Makefile` have clear names
- [ ] CI paths still match the new layout
- [ ] The tree looks intentional, not generated
