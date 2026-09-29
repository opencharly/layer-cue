# cue

The CUE data-validation and configuration CLI for OpenCharly images.

The `cue` candy installs the pinned [cue](https://github.com/cue-lang/cue) CLI
(v0.16.1) as the single static binary `/usr/local/bin/cue`. The pin matches
charly's embedded `cuelang.org/go` library, so the CLI that vendors a schema is
the same CUE version the library compiles it with.

`cue` is a **dev-time tool**: charly's runtime never shells out to it — every
build/deploy/check path validates through the embedded `cuelang.org/go` library.
The CLI is here so developers and agents can run the offline schema-vendoring
pipeline (`cue import jsonschema:`, `cue mod get`) and author/validate schemas
in-box.

## What it provides

| Property | Value |
|---|---|
| Layer / candy | `cue` |
| Binary | `/usr/local/bin/cue` |
| Version | pinned `v0.16.1` (`CUE_VERSION` var) |
| Install | `download:` the cue-lang release tarball, extract `cue` (distro-agnostic, no `package:`) |
| Service / port | none |

## How to use it

Compose the layer by pinning this repo in a box's `candy:` list:

```yaml
my-dev-box:
  candy:
    base: fedora
    candy:
      - '@github.com/opencharly/layer-cue:v2026.239.1620'
```

Then, inside the built image (or on a dev host):

```bash
cue version                              # cue version v0.16.1
cue export -e y /tmp/check.cue           # evaluates CUE, not just parses it
```

The candy's `plan:` asserts the binary at the fixed path, `cue version` reporting
the pinned `v0.16.1`, and a real `cue export` of a computed field (`y: x*2` →
`42`) — a non-functional binary fails the check.

## Layout

- `charly.yml` — the `cue:` candy entity (the `CUE_VERSION` var, the
  `download:`/extract `plan:`, and the `check:` assertions) and the embedded
  `cue-skill:` skill entity.
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `README.md` — this user overview.

## Related

- Owning skill: `/charly-tools:cue`
- Feeds: `/charly-internals:egress` — the egress-validation schema-vendoring pipeline
- [`opencharly/charly`](https://github.com/opencharly/charly) — the charly CLI and image builder
- [`opencharly/opencharly`](https://github.com/opencharly/opencharly) — the umbrella
