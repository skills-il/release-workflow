# release-workflow

Reusable GitHub Actions release workflow used by every [skills-il](https://github.com/skills-il) category repo and the [mcps](https://github.com/skills-il/mcps) monorepo.

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

Optional: override `subject_path` to target different files:

```yaml
jobs:
  release:
    uses: skills-il/release-workflow/.github/workflows/release.yml@v1
    with:
      subject_path: 'src/**/*.ts package.json'
```

## Why

Consumers of `gh skill install` (and the skills-il catalog's Security Scorecard) check for a signed release attestation as a Critical-tier signal. This reusable workflow means every owned repo earns it with one line of YAML instead of 30.
