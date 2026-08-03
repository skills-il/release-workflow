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

The workflow attests `**/SKILL.md`, `**/SKILL_HE.md`, `**/metadata.json` and `**/scripts/**` by default.

`**/scripts/**` covers the helper scripts a skill ships, which are the files a user actually EXECUTES. Leaving them unattested was the bigger gap: a tampered `comparables.py` would have passed every provenance check while the markdown around it verified cleanly. Directories matched by the glob are skipped automatically (the upstream action stats each match and keeps only regular files), and the subject count stays far below the action's 1024 limit (the largest category repo produces ~144). Override with newline-separated globs (the upstream `actions/attest-build-provenance` action does **not** accept space-separated values):

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
