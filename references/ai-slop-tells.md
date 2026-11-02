# AI Slop Tells (repo edition)

Patterns that make a repo look machine-dumped. Remove or rewrite them during tidy passes.

## Documentation slop

| Tell | Do instead |
| --- | --- |
| `CHANGELOG_AGENT.md`, `FIXES.md`, `SESSION_NOTES.md`, `IMPLEMENTATION_PLAN_v3.md` | Merge lasting truth into README/`docs/`; delete session litter |
| "Comprehensive guide to..." titles | Name the real topic in plain words |
| Emoji headers and trophy badges everywhere | Calm headings; badges only if the project already uses them |
| Fake architecture diagrams that do not match the tree | Document the actual folders |
| Duplicate READMEs in every subfolder saying the same thing | One source of truth; short local notes only when needed |
| "This module provides a robust, scalable, enterprise-ready..." | Say what it does in one sentence |

## Code slop

| Tell | Do instead |
| --- | --- |
| Huge generated-looking files with no seams | Split by responsibility; name parts clearly |
| `utils.ts` / `helpers.js` grab bags | Feature-local helpers or named modules (`dates.ts`, `money.ts`) |
| Comment essays restating the next line | Delete; keep comments for non-obvious why |
| `// AI generated` / `// TODO: implement later` carpets | Finish, ticket, or delete |
| Empty abstractions "for flexibility" | Concrete code until a second use appears |
| Copy-pasted components with `2` / `New` / `Final` suffixes | One component; parameters for variation |
| Defensive try/catch that swallows everything | Catch what you can handle; let the rest surface |
| Over-mocked types / `as any` / `as unknown as` chains | Fix the types |
| Decorative folders (`core/`, `shared/`, `common/`) with one file each | Flatten until the category earns its weight |

## Structure slop

| Tell | Do instead |
| --- | --- |
| Everything in project root | Group by feature or layer the project already uses |
| Nested `components/components/ui/ui` | Flatten to the shallowest clear path |
| Mixed `src/` and loose root modules for the same app | Pick one home |
| Checked-in `node_modules`, build artifacts, `.DS_Store` | Ignore and remove from tracking |
| Multiple competing state folders left mid-migrate | Finish the migrate or revert; do not leave both |

## Refactor slop

| Tell | Do instead |
| --- | --- |
| Big-bang rewrite in one shot | Small structural steps; tests green between them |
| Behavior change hidden inside a "cleanup" commit | Tidy-first: structure commits separate from behavior |
| Renames without updating imports/CI/docs | Trace every call site and path reference |
| "Cleanup" that deletes unknown code | Ask; quarantine behind a flag; or leave alone |
