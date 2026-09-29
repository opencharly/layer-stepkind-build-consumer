# stepkind-build-consumer

A reference **consumer** candy for OpenCharly's F-STEP-EMIT build leg — the
STEP-class analogue of `candy/examplestep-consumer` (which consumes the VERB-class
`examplestep` at box build).

The candy composes
[`plugin-example-stepkind`](https://github.com/opencharly/plugin-example-stepkind)
and its `plan:` carries a **build-context** `run:` step `plugin: examplestepkind`
— a `class:step` word whose provider declares `Emits=true`. Composed onto a POD
via `add_candy`, the overlay build lowers the run-step to an external step
(`external:examplestepkind`) whose `deploykit.OCITarget` open external-step arm
invokes the plugin's `OpEmit` and splices the returned Containerfile `RUN`,
baking `/etc/examplestepkind-build-baked` (carrying the payload marker) into the
overlay image.

## What it provides

| Property | Value |
|---|---|
| Layer / candy | `stepkind-build-consumer` |
| Composes | `plugin-example-stepkind` (provides the `examplestepkind` step) |
| Build step | `plugin: examplestepkind` with `marker: STEPKIND-BUILD-BAKED-OK` |
| Baked artifact | `/etc/examplestepkind-build-baked` (token `STEPKIND-BUILD-BAKED-OK`) |
| Packages / service | none |

## How to use it

Compose it **with** `plugin-example-stepkind` on a pod via `add_candy`:

```yaml
my-pod:
  pod:
    add_candy:
      - github.com/opencharly/plugin-example-stepkind/candy/plugin-example-stepkind
      - github.com/opencharly/layer-stepkind-build-consumer
```

The build-context `run:` step emits the Containerfile `RUN`; the runtime `check:`
then proves the bake ran by asserting `/etc/examplestepkind-build-baked` carries
its token inside the deployed container.

## Layout

- `charly.yml` — the `stepkind-build-consumer:` candy entity (the nested `candy:`
  composition, the build-context `examplestepkind` run step, and the runtime
  `file:` `check:` probe). It carries **no `skill:` entity**.
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `README.md` — this user overview.

## Related

- Owning family skill: `/charly-internals:install-plan` (the `InstallPlan` /
  `deploykit.OCITarget` external-step build-emit arm)
- Plugin: [`plugin-example-stepkind`](https://github.com/opencharly/plugin-example-stepkind)
- [`opencharly/charly`](https://github.com/opencharly/charly) — the charly CLI and image builder
- [`opencharly/opencharly`](https://github.com/opencharly/opencharly) — the umbrella
