# AGENTS.md — layer-cue

Standalone candy repo for the `cue` layer — the pinned CUE CLI. The candy lives
in `charly.yml` at the repo root: the `CUE_VERSION` var, the
`download:`/extract `plan:` steps, the `check:` assertions, and the embedded
`skill:` entity projected into the marketplace corpus as `/charly-tools:cue`.

Canonical files:

- `charly.yml` — the `cue:` candy entity and the `cue-skill:` skill entity.
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `README.md` — user overview only; never agent guidance.

## Load these skills first (R0)

- `/charly-tools:cue` — the owning skill. The pinned release, why the CLI version
  matches the embedded library, and the schema-vendoring pipeline it feeds. Load
  before editing or troubleshooting the candy.
- `/charly-image:layer` — the candy authoring reference (`charly.yml` schema,
  `plan:` step verbs incl. `download:`/`check:`, package sections, service
  declarations). Load before editing any entity field or plan step.

## Build / validate / test

- The merge gate is the **org-wide** `charly/pr-validator` (required check
  `validate / validate`, defined in `opencharly/.github`); this repo has **no
  per-repo candy gate** and ships only `.github/workflows/tag-on-merge.yml`.
- The candy's `plan:` `check:` steps are the functional evidence: the binary at
  `/usr/local/bin/cue`, `cue version` reporting the pinned `v0.16.1`, and a real
  `cue export` of a computed field. They must stay valid on every distro arm they
  run on.
- Keep `CUE_VERSION` aligned with charly's embedded `cuelang.org/go` version — a
  newer CLI could emit constructs the pinned library rejects.

## Modify this repo

- Edit the `cue:` candy entity AND the `cue-skill:` skill entity in `charly.yml`
  together. The skill is the projected usage source, so a version or behaviour
  change not mirrored in the skill leaves the corpus stale.
- New behaviour claims belong in the `plan:` as an observable `check:` step, and
  in the skill body.

## Landing

- PR-only. Every change lands through a pull request; the org-required
  `charly/pr-validator` validates the diff and body and arms native auto-merge
  on PASS. Direct pushes to `main` are blocked.
- History lives in `CHANGELOG/` (written by `tag-on-merge` at merge time); the
  PR body IS the changelog.
- The authoritative rulebook is the umbrella `AGENTS.md` in
  `opencharly/opencharly` and `charly/AGENTS.md` in the charly repo. Do not
  restate its rules here.
