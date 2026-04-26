# release-workflow

Reusable GitHub Actions release workflow used by every [skills-il](https://github.com/skills-il) category repo.

Single source of truth for:
- Creating a GitHub Release with auto-generated notes when a `v*` tag is pushed
- Emitting a [Sigstore](https://www.sigstore.dev/) attestation via `actions/attest-build-provenance` so `gh attestation verify` can validate the chain of custody

## Usage

In any skills-il repo, create `.github/workflows/release.yml`:

```yaml
on:
  push:
    tags: ['v*']
jobs:
  release:
    uses: skills-il/release-workflow/.github/workflows/release.yml@v1
```

### Pinning to an immutable version

`v1` is a moving alias that always points at the latest backward-compatible commit. For supply-chain hardening, pin to the immutable semver tag instead:

```yaml
jobs:
  release:
    uses: skills-il/release-workflow/.github/workflows/release.yml@v1.0.0
```

### Overriding `subject_path`

The workflow attests `**/SKILL.md`, `**/SKILL_HE.md`, and `**/metadata.json` by default. Override with newline-separated globs (the upstream `actions/attest-build-provenance` action does **not** accept space-separated values):

```yaml
jobs:
  release:
    uses: skills-il/release-workflow/.github/workflows/release.yml@v1
    with:
      subject_path: |
        src/**/*.ts
        package.json
```

## Why

Consumers of `gh skill install` (and the skills-il catalog's Security Scorecard) check for a signed release attestation as a Critical-tier signal. This reusable workflow means every owned repo earns it with one line of YAML instead of 30.
