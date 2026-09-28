<!-- markdownlint-disable -->

# Hardening Report: bats-core--bats-action/2.1.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **bats-core--bats-action/2.1.0** was hardened automatically. 15 finding(s) were identified and resolved across 2 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Multiple `run:` blocks in action.yaml directly interpolate `${{ inputs.* }}` expressions inside shell commands (rule a). GitHub Actions performs YAML template substitution before the shell parses the string, so an attacker-controlled input value is spliced raw into the shell, enabling arbitrary command injection.

Affected lines and offending expressions:
- Step "Download and install Bats": `VERSION=${{ inputs.bats-version }}`
- Step "Download and install Bats-support": `VERSION=${{ inputs.support-version }}`, `DESTDIR=${{ inputs.support-path }}`, `[[ "${{ inputs.support-clean }}" = "true" ]]`
- Step "Download and install Bats-assert": `VERSION=${{ inputs.assert-version }}`, `DESTDIR=${{ inputs.assert-path }}`, `[[ "${{ inputs.assert-clean }}" = "true" ]]`
- Step "Download and install Bats-detik": `VERSION=${{ inputs.detik-version }}`, `DESTDIR=${{ inputs.detik-path }}`, `[[ "${{ inputs.detik-clean }}" = "true" ]]`
- Step "Download and install Bats-file": `VERSION=${{ inputs.file-version }}`, `DESTDIR=${{ inputs.file-path }}`, `[[ "${{ inputs.file-clean }}" = "true" ]]`

Fix: move each input into an `env:` block and reference it as a quoted shell variable (e.g., `"$VERSION"`) inside the `run:` script.

Locations:

- `action.yaml:100`
- `action.yaml:130`
- `action.yaml:131`
- `action.yaml:148`
- `action.yaml:163`
- `action.yaml:164`
- `action.yaml:181`
- `action.yaml:196`
- `action.yaml:197`
- `action.yaml:213`
- `action.yaml:228`
- `action.yaml:229`
- `action.yaml:245`

### unpinned-uses (severity: high)

All five `uses: actions/cache@v4` references in action.yaml use a mutable version tag (`@v4`) instead of a pinned 40-character commit SHA. A tag can be moved by the upstream repository owner (or a compromised account) to point to malicious code, creating a supply-chain attack vector. Each cache step for bats, bats-support, bats-assert, bats-detik, and bats-file is affected.

Fix: pin each reference to a full SHA, e.g.:
  `uses: actions/cache@1bd1e32a3bdc45362d1e726936510720a7c6158d # v4`

Locations:

- `action.yaml:84`
- `action.yaml:120`
- `action.yaml:155`
- `action.yaml:187`
- `action.yaml:219`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.bats-version }}" appears directly in run: block of step "Download and install Bats"; move to env: map

Locations:

- `action.yml:128`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.support-version }}" appears directly in run: block of step "Download and install Bats-support"; move to env: map

Locations:

- `action.yml:165`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.support-path }}" appears directly in run: block of step "Download and install Bats-support"; move to env: map

Locations:

- `action.yml:166`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.support-clean }}" appears directly in run: block of step "Download and install Bats-support"; move to env: map

Locations:

- `action.yml:183`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.assert-version }}" appears directly in run: block of step "Download and install Bats-assert"; move to env: map

Locations:

- `action.yml:198`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.assert-path }}" appears directly in run: block of step "Download and install Bats-assert"; move to env: map

Locations:

- `action.yml:199`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.assert-clean }}" appears directly in run: block of step "Download and install Bats-assert"; move to env: map

Locations:

- `action.yml:216`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.detik-version }}" appears directly in run: block of step "Download and install Bats-detik"; move to env: map

Locations:

- `action.yml:231`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.detik-path }}" appears directly in run: block of step "Download and install Bats-detik"; move to env: map

Locations:

- `action.yml:232`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.detik-clean }}" appears directly in run: block of step "Download and install Bats-detik"; move to env: map

Locations:

- `action.yml:248`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.file-version }}" appears directly in run: block of step "Download and install Bats-file"; move to env: map

Locations:

- `action.yml:263`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.file-path }}" appears directly in run: block of step "Download and install Bats-file"; move to env: map

Locations:

- `action.yml:264`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.file-clean }}" appears directly in run: block of step "Download and install Bats-file"; move to env: map

Locations:

- `action.yml:281`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection, static-inline-injection, unpinned-uses

**Notes:**

Fixed action.yaml (the only action file present — no action.yml exists):

1. Pinned all 5 `actions/cache@v4` references to full SHA `0057852bfaa89a56745cba8c7296529d2fc39830` with `# v4` comment.

2. Moved all `${{ inputs.* }}` expressions from run: blocks into env: blocks for the five install steps:
   - Bats: VERSION
   - Bats-support: VERSION, DESTDIR, INPUT_SUPPORT_CLEAN
   - Bats-assert: VERSION, DESTDIR, INPUT_ASSERT_CLEAN
   - Bats-detik: VERSION, DESTDIR, INPUT_DETIK_CLEAN
   - Bats-file: VERSION, DESTDIR, INPUT_FILE_CLEAN

   Shell scripts now reference these as plain env vars ($VERSION, $DESTDIR, $INPUT_*_CLEAN) instead of inline ${{ }} expressions. The Debug print step uses only steps.*.outputs expressions (not user-controlled inputs) so those were left as-is.

### Iteration 2

**Fixes applied:** script-injection

**Notes:**

Fixed two script-injection findings in hardened/action/action.yaml:

1. Finding (a): Moved all ${{ steps.*.outputs.* }} expressions from the 'Debug print if installed' run: block into an env: block with named variables (BATS_INSTALLED, SUPPORT_INSTALLED, ASSERT_INSTALLED, DETIK_INSTALLED, FILE_INSTALLED). The run: script now references plain shell variables.

2. Finding (b): Added double-quotes around all unquoted variable expansions (${TEMPDIR}, ${DESTDIR}, ${URL}/${url}, ${VERSION}, $fn) in all five install steps (Bats, Bats-support, Bats-assert, Bats-detik, Bats-file). Also fixed the unquoted $GITHUB_OUTPUT redirect in the Bats install step. The ${CMD} variable is intentionally left unquoted as it holds either an empty string or 'sudo' and requires word-splitting to function correctly.

