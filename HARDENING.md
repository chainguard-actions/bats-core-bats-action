<!-- markdownlint-disable -->

# Hardening Report: bats-core--bats-action/2.1.1

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **bats-core--bats-action/2.1.1** was hardened automatically. 15 finding(s) were identified and resolved across 2 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Multiple `run:` blocks in action.yaml directly interpolate `${{ inputs.* }}` expressions inside shell command strings (rule a — direct expression interpolation). An attacker controlling these inputs can inject arbitrary shell commands.

"Download and install Bats" step: `VERSION=${{ inputs.bats-version }}` (line 112) — inputs.bats-version is interpolated directly into the shell.

"Download and install Bats-support" step: `VERSION=${{ inputs.support-version }}` (line 140), `DESTDIR=${{ inputs.support-path }}` (line 141), `[[ "${{ inputs.support-clean }}" = "true" ]]` (line 153).

"Download and install Bats-assert" step: `VERSION=${{ inputs.assert-version }}` (line 164), `DESTDIR=${{ inputs.assert-path }}` (line 165), `[[ "${{ inputs.assert-clean }}" = "true" ]]` (line 177).

"Download and install Bats-detik" step: `VERSION=${{ inputs.detik-version }}` (line 188), `DESTDIR=${{ inputs.detik-path }}` (line 189), `[[ "${{ inputs.detik-clean }}" = "true" ]]` (line 201).

"Download and install Bats-file" step: `VERSION=${{ inputs.file-version }}` (line 212), `DESTDIR=${{ inputs.file-path }}` (line 213), `[[ "${{ inputs.file-clean }}" = "true" ]]` (line 225).

"Debug print if installed" step: `echo "Bats installed: ${{ (steps.bats-install.outputs.bats-installed != '') }}"` and similar lines (lines 230–234) interpolate steps.*.outputs.* expressions directly into echo commands.

Fix: move all `${{ inputs.* }}` values into `env:` variables and reference them as quoted shell variables (e.g., `"$VERSION"`) inside the `run:` block.

Locations:

- `action.yaml:112`
- `action.yaml:140`
- `action.yaml:141`
- `action.yaml:153`
- `action.yaml:164`
- `action.yaml:165`
- `action.yaml:177`
- `action.yaml:188`
- `action.yaml:189`
- `action.yaml:201`
- `action.yaml:212`
- `action.yaml:213`
- `action.yaml:225`
- `action.yaml:230`

### unpinned-uses (severity: high)

All 5 `uses: actions/cache@v4` references in action.yaml use the mutable tag `@v4` instead of a pinned full 40-character commit SHA. A tag can be moved by the upstream repository owner (or a compromised account) to point to malicious code, enabling a supply-chain attack. Fix: pin each reference to a specific commit SHA, e.g. `uses: actions/cache@1bd1e32a3bdc45362d1e726936510720a7c6158d # v4`.

Locations:

- `action.yaml:96`
- `action.yaml:132`
- `action.yaml:156`
- `action.yaml:180`
- `action.yaml:204`

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

**Fixes applied:** unpinned-uses, script-injection, static-inline-injection

**Notes:**

Fixed all findings in hardened/action/action.yaml:
1. Pinned all 5 `actions/cache@v4` references to full SHA `actions/cache@0057852bfaa89a56745cba8c7296529d2fc39830 # v4`.
2. Moved all `${{ inputs.* }}` expressions out of run: blocks into env: maps for each affected step (bats-install, support-install, assert-install, detik-install, file-install), referencing them as quoted shell variables.
3. Moved `${{ steps.*.outputs.* }}` expressions in the 'Debug print if installed' step into env: variables, referencing them as plain shell variables in the run block.

### Iteration 2

**Fixes applied:** script-injection

**Notes:**

Fixed all 5 script-injection findings in hardened/action/action.yaml by adding double quotes around all unquoted variable expansions that use user-controlled input values (VERSION, DESTDIR, TEMPDIR, URL/url). Changes applied to: (1) 'Download and install Bats' step - quoted VERSION in [[ ]] test, TEMPDIR, DESTDIR, URL in curl, DESTDIR in install.sh call, TEMPDIR in rm; (2) 'Download and install Bats-support' step - quoted TEMPDIR, DESTDIR/src/, url, TEMPDIR, DESTDIR/load.bash, $fn, DESTDIR/src/$(basename "$fn"), TEMPDIR in cleanup; (3) 'Download and install Bats-assert' step - same pattern as support; (4) 'Download and install Bats-detik' step - same pattern with DESTDIR/$(basename "$fn") for lib/*.bash; (5) 'Download and install Bats-file' step - same pattern as support. Also fixed unquoted $GITHUB_OUTPUT references to use "$GITHUB_OUTPUT".

