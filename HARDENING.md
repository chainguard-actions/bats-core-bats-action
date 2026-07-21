<!-- markdownlint-disable -->

# Hardening Report: bats-core--bats-action/2.1.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **bats-core--bats-action/2.1.0** was hardened automatically. 15 finding(s) were identified and resolved across 3 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Multiple run: blocks in action.yaml directly interpolate ${{ inputs.* }} expressions inside shell command strings (rule a). Before the shell executes the script, GitHub Actions performs YAML template substitution, so a caller-supplied input value containing shell metacharacters (e.g. `; malicious_cmd`) would be executed as shell code.

Affected expressions:
- Line 128 ("Download and install Bats" step): VERSION=${{ inputs.bats-version }}
- Line 165 ("Download and install Bats-support" step): VERSION=${{ inputs.support-version }}
- Line 166 ("Download and install Bats-support" step): DESTDIR=${{ inputs.support-path }}
- Line 183 ("Download and install Bats-support" step): ${{ inputs.support-clean }} in conditional
- Line 199 ("Download and install Bats-assert" step): VERSION=${{ inputs.assert-version }}
- Line 200 ("Download and install Bats-assert" step): DESTDIR=${{ inputs.assert-path }}
- Line 215 ("Download and install Bats-assert" step): ${{ inputs.assert-clean }} in conditional
- Line 232 ("Download and install Bats-detik" step): VERSION=${{ inputs.detik-version }}
- Line 233 ("Download and install Bats-detik" step): DESTDIR=${{ inputs.detik-path }}
- Line 246 ("Download and install Bats-detik" step): ${{ inputs.detik-clean }} in conditional
- Line 264 ("Download and install Bats-file" step): VERSION=${{ inputs.file-version }}
- Line 265 ("Download and install Bats-file" step): DESTDIR=${{ inputs.file-path }}
- Line 278 ("Download and install Bats-file" step): ${{ inputs.file-clean }} in conditional
- Lines 287-291 ("Debug print if installed" step): ${{ steps.*.outputs.* }} expressions

Fix: Move each input into an env: block and reference it as a quoted shell variable (e.g. "$VERSION"), never interpolate ${{ }} directly inside a run: script.

Locations:

- `action.yaml:128`
- `action.yaml:165`
- `action.yaml:166`
- `action.yaml:183`
- `action.yaml:199`
- `action.yaml:200`
- `action.yaml:215`
- `action.yaml:232`
- `action.yaml:233`
- `action.yaml:246`
- `action.yaml:264`
- `action.yaml:265`
- `action.yaml:278`
- `action.yaml:287`

### unpinned-uses (severity: high)

All five uses: references in action.yaml pin to the mutable tag @v4 rather than an immutable 40-character SHA commit hash. If the actions/cache repository is compromised or the tag is moved, the action will silently execute attacker-controlled code.

Failing references:
- uses: actions/cache@v4 ("Set cache for Bats" step, line 106)
- uses: actions/cache@v4 ("Set cache for Bats-support" step, line 153)
- uses: actions/cache@v4 ("Set cache for Bats-assert" step, line 187)
- uses: actions/cache@v4 ("Set cache for Bats-detik" step, line 220)
- uses: actions/cache@v4 ("Set cache for Bats-file" step, line 253)

Fix: Pin each reference to a full SHA, e.g. uses: actions/cache@1bd1e32a3bdc45362d1e726936510720a7c6158d # v4

Locations:

- `action.yaml:106`
- `action.yaml:153`
- `action.yaml:187`
- `action.yaml:220`
- `action.yaml:253`

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

1. unpinned-uses: Pinned all 5 `actions/cache@v4` references to the full SHA `0057852bfaa89a56745cba8c7296529d2fc39830` with `# v4` comment.

2. script-injection / static-inline-injection: Moved all ${{ inputs.* }} and ${{ steps.*.outputs.* }} expressions out of run: blocks and into env: blocks for each affected step:
   - 'Download and install Bats': VERSION env var
   - 'Download and install Bats-support': VERSION, DESTDIR, SUPPORT_CLEAN env vars
   - 'Download and install Bats-assert': VERSION, DESTDIR, ASSERT_CLEAN env vars
   - 'Download and install Bats-detik': VERSION, DESTDIR, DETIK_CLEAN env vars
   - 'Download and install Bats-file': VERSION, DESTDIR, FILE_CLEAN env vars
   - 'Debug print if installed': BATS_INSTALLED, SUPPORT_INSTALLED, ASSERT_INSTALLED, DETIK_INSTALLED, FILE_INSTALLED env vars

All shell scripts now reference only plain environment variables, preventing shell injection attacks.

### Iteration 2

**Fixes applied:** script-injection

**Notes:**

Fixed unquoted shell variable expansions in all five install steps of action.yaml:

1. **bats-install step**: Quoted `$VERSION` in the `[[ "$VERSION" = v* ]]` conditional test; quoted `${URL}`, `${VERSION}`, `${TEMPDIR}`, `${DESTDIR}` in curl URL, mkdir, tar, install.sh, and rm commands; also fixed unquoted `$GITHUB_OUTPUT`.

2. **support-install step**: Quoted `${VERSION}` in URL construction, `${TEMPDIR}`, `${DESTDIR}`, `${url}` in mkdir, curl, install, and rm commands; quoted `$fn` and `$(basename "$fn")` in the for loop.

3. **assert-install step**: Same pattern as support-install — quoted all `${TEMPDIR}`, `${DESTDIR}`, `${url}`, `$fn`, and `$(basename "$fn")` expansions.

4. **detik-install step**: Same pattern — quoted all `${TEMPDIR}`, `${DESTDIR}`, `${url}`, `$fn`, and `$(basename "$fn")` expansions.

5. **file-install step**: Same pattern — quoted all `${TEMPDIR}`, `${DESTDIR}`, `${url}`, `$fn`, and `$(basename "$fn")` expansions.

All input-derived env vars (VERSION, DESTDIR) are already passed via the step's `env:` block (not directly via `${{ }}` in the run script), so the primary fix was ensuring proper double-quoting to prevent shell metacharacter injection.

### Iteration 3

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all 5 workflow files in hardened/action/.github/workflows/:

1. unpinned-uses: Pinned all mutable action references to full 40-char SHAs:
   - actions/checkout@v4 → @11d5960a326750d5838078e36cf38b85af677262 # v4 (7 occurrences across 5 files)
   - bats-core/bats-action@main → @562ebf679115bde788f656cd532408c2d125297d # main (2 occurrences in test-public-action.yaml)
   - brokenpip3/action-pre-commit-update@main → @24b7a510db4356b7cb0a2d0b8f72d1226fa7195b # main (1 occurrence in update-pre-commit-hooks.yaml)

2. missing-permissions: Added `permissions: {}` top-level block to all 5 workflow files to enforce least-privilege token access.

