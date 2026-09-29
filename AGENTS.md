# AGENTS.md — layer-stepkind-build-consumer

Standalone candy repo for the `stepkind-build-consumer` layer — a reference
consumer for the F-STEP-EMIT build leg (the STEP-class analogue of
`candy/examplestep-consumer`). The candy lives in `charly.yml` at the repo root:
the nested `candy:` composition of `plugin-example-stepkind`, the build-context
`examplestepkind` run step, and the runtime `file:` `check:` probe. It carries
**no `skill:` entity**.

Canonical files:

- `charly.yml` — the `stepkind-build-consumer:` candy entity (the nested `candy:`
  composition, the build-context `examplestepkind` run step, and the runtime
  `file:` `check:` probe; no `skill:` entity).
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `README.md` — user overview only; never agent guidance.

## Load these skills first (R0)

- `/charly-internals:install-plan` — the family skill: the `InstallPlan` /
  `deploykit.OCITarget` external-step build-emit arm this fixture exercises.
  Load before editing or troubleshooting the candy.
- `/charly-image:layer` — the candy authoring reference (`charly.yml` schema,
  `plan:` step verbs incl. plugin-verb sugar and `check:`, the nested `candy:`
  composition list).
- **Missing owning skill:** this fixture carries no `skill:` entity, so no
  repo-owned skill is projected for the candy; the closest family skill is
  `/charly-internals:install-plan` (the external-step build-emit arm). The gap is
  recorded against the named batch
  [opencharly/opencharly#291](https://github.com/opencharly/opencharly/issues/291).

## Build / validate / test

- The merge gate is the **org-wide** `charly/pr-validator` (required check
  `validate / validate`, defined in `opencharly/.github`); this repo has **no
  per-repo candy gate** and ships only `.github/workflows/tag-on-merge.yml`.
- `charly box validate` at the repo root checks the manifest parses and
  validates.
- The candy's own proof is its `plan:` — the build-context `examplestepkind` run
  step (which the overlay build lowers to `external:examplestepkind` and emits a
  Containerfile `RUN`) plus the runtime `file:` `check:` that asserts the baked
  marker `/etc/examplestepkind-build-baked` carries its token.

## Modify this repo

- Compose it WITH `candy/plugin-example-stepkind` — the plugin provides the
  `examplestepkind` step the plan dispatches.
- Keep the baked path and the `STEPKIND-BUILD-BAKED-OK` marker stable: the
  runtime `check:` asserts both.
- If an owning skill is authored, add the `skill:` entity here and update this
  signpost and the README in the same change.

## Landing

- PR-only. Every change lands through a pull request; the org-required
  `charly/pr-validator` validates the diff and body and arms native auto-merge
  on PASS. Direct pushes to `main` are blocked.
- History lives in `CHANGELOG/` (written by `tag-on-merge` at merge time); the
  PR body IS the changelog.
- The authoritative rulebook is the umbrella `AGENTS.md` in
  `opencharly/opencharly` and `charly/AGENTS.md` in the charly repo. Do not
  restate its rules here.
