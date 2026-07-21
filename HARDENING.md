<!-- markdownlint-disable -->

# Hardening Report: bats-core--bats-action/2.1.1

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **bats-core--bats-action/2.1.1** was hardened automatically. 17 finding(s) were identified and resolved across 3 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Multiple `run:` blocks in action.yaml directly interpolate `${{ inputs.* }}` and `${{ steps.*.outputs.* }}` expressions inside shell command strings (sub-rule a). Before the shell executes, GitHub Actions substitutes these values verbatim into the script, enabling an attacker to inject arbitrary shell commands via crafted input values. Affected lines include:
- `VERSION=${{ inputs.bats-version }}` (bats-install step)
- `VERSION=${{ inputs.support-version }}`, `DESTDIR=${{ inputs.support-path }}`, `[[ "${{ inputs.support-clean }}" = "true" ]]` (support-install step)
- `VERSION=${{ inputs.assert-version }}`, `DESTDIR=${{ inputs.assert-path }}`, `[[ "${{ inputs.assert-clean }}" = "true" ]]` (assert-install step)
- `VERSION=${{ inputs.detik-version }}`, `DESTDIR=${{ inputs.detik-path }}`, `[[ "${{ inputs.detik-clean }}" = "true" ]]` (detik-install step)
- `VERSION=${{ inputs.file-version }}`, `DESTDIR=${{ inputs.file-path }}`, `[[ "${{ inputs.file-clean }}" = "true" ]]` (file-install step)
- `${{ (steps.bats-install.outputs.bats-installed != '') }}` etc. (debug-print step)
All these must be moved to `env:` variables and then referenced as quoted `"$VAR"` in the shell.

Locations:

- `action.yaml:97`
- `action.yaml:138`
- `action.yaml:139`
- `action.yaml:151`
- `action.yaml:172`
- `action.yaml:173`
- `action.yaml:185`
- `action.yaml:206`
- `action.yaml:207`
- `action.yaml:219`
- `action.yaml:240`
- `action.yaml:241`
- `action.yaml:253`
- `action.yaml:272`
- `action.yaml:273`
- `action.yaml:285`

### github-env-injection (severity: high)

In the support-install, assert-install, detik-install, and file-install steps, `DESTDIR` is set directly from `${{ inputs.*-path }}` (an untrusted caller-controlled input) and then written unsanitized to `$GITHUB_PATH` via `echo "${DESTDIR}/bin" >> "$GITHUB_PATH"`. An attacker supplying a value containing newlines in the path input can inject arbitrary entries into `$GITHUB_PATH`. The required sanitization (`printf '%s' "$DESTDIR" | tr -d '\n\r'`) is absent before the write.

Locations:

- `action.yaml:155`
- `action.yaml:189`
- `action.yaml:223`
- `action.yaml:257`

### unpinned-uses (severity: high)

Multiple `uses:` references are pinned to mutable tags or branch names rather than immutable 40-character commit SHAs, making the action vulnerable to supply-chain attacks if the referenced tag or branch is moved or overwritten.

In action.yaml:
- `uses: actions/cache@v4` (appears 5 times — bats, support, assert, detik, and file cache steps)

In .github/workflows/test-local-action.yaml and test-local-action-inside-home.yaml and test-local-action-with-conditionals.yaml:
- `uses: actions/checkout@v4`

In .github/workflows/test-public-action.yaml:
- `uses: actions/checkout@v4`
- `uses: bats-core/bats-action@main` (branch ref — especially dangerous)

In .github/workflows/update-pre-commit-hooks.yaml:
- `uses: actions/checkout@v4`
- `uses: brokenpip3/action-pre-commit-update@main` (branch ref — especially dangerous)

Locations:

- `action.yaml:83`
- `action.yaml:128`
- `action.yaml:162`
- `action.yaml:196`
- `action.yaml:230`
- `.github/workflows/test-local-action.yaml:21`
- `.github/workflows/test-local-action-inside-home.yaml:21`
- `.github/workflows/test-local-action-with-conditionals.yaml:21`
- `.github/workflows/test-public-action.yaml:21`
- `.github/workflows/test-public-action.yaml:24`
- `.github/workflows/update-pre-commit-hooks.yaml:12`
- `.github/workflows/update-pre-commit-hooks.yaml:13`

### missing-permissions (severity: medium)

None of the workflow files under .github/workflows/ define a top-level `permissions:` block, and none of the individual jobs within those files define job-level `permissions:` blocks either. Without explicit permissions, workflows run with the default token permissions (which may be read-write depending on repository settings), violating the principle of least privilege. All five workflow files are affected: test-local-action.yaml, test-local-action-inside-home.yaml, test-local-action-with-conditionals.yaml, test-public-action.yaml, and update-pre-commit-hooks.yaml.

Locations:

- `.github/workflows/test-local-action.yaml:1`
- `.github/workflows/test-local-action-inside-home.yaml:1`
- `.github/workflows/test-local-action-with-conditionals.yaml:1`
- `.github/workflows/test-public-action.yaml:1`
- `.github/workflows/update-pre-commit-hooks.yaml:1`

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

**Fixes applied:** script-injection, static-inline-injection, github-env-injection, unpinned-uses, missing-permissions

**Notes:**

Fixed all findings in action.yaml and the 5 workflow files:

1. script-injection/static-inline-injection: Moved all ${{ inputs.* }} and ${{ steps.*.outputs.* }} expressions from run: blocks into env: blocks. Shell scripts now reference plain env vars ($BATS_VERSION, $BATS_SUPPORT_PATH, $BATS_SUPPORT_CLEAN, etc.).

2. github-env-injection: Added sanitization (printf '%s' "$DESTDIR_RAW" | tr -d '\n\r') before writing DESTDIR-derived values to $GITHUB_PATH in all 4 library install steps (support, assert, detik, file).

3. unpinned-uses: Pinned actions/cache@v4 → SHA 0057852bfaa89a56745cba8c7296529d2fc39830 (5x in action.yaml); actions/checkout@v4 → SHA 11d5960a326750d5838078e36cf38b85af677262 (in all workflow files); bats-core/bats-action@main → SHA 562ebf679115bde788f656cd532408c2d125297d; brokenpip3/action-pre-commit-update@main → SHA 24b7a510db4356b7cb0a2d0b8f72d1226fa7195b.

4. missing-permissions: Added permissions: {} at top level of all 5 workflow files. update-pre-commit-hooks.yaml also has job-level contents: write and pull-requests: write for the pre-commit update job.

### Iteration 2

**Fixes applied:** script-injection

**Notes:**

Fixed unquoted shell variables in all five installation steps of action.yaml:

1. 'Download and install Bats' step: Quoted ${TEMPDIR}, ${DESTDIR}, ${URL}/${VERSION} in curl/tar command, ${DESTDIR} in ./install.sh call, and fixed unquoted $GITHUB_OUTPUT.

2. 'Download and install Bats-support' step: Quoted ${TEMPDIR}, ${DESTDIR}/src/, ${url} in curl/tar command, ${DESTDIR}/load.bash in install call, $fn and ${DESTDIR}/src/$(basename $fn) in loop, and ${TEMPDIR} in rm call.

3. 'Download and install Bats-assert' step: Same pattern as support step — quoted all unquoted variables.

4. 'Download and install Bats-detik' step: Same pattern — quoted ${TEMPDIR}, ${DESTDIR}/src/, ${url}, $fn, ${DESTDIR}/$(basename $fn), and ${TEMPDIR} in rm.

5. 'Download and install Bats-file' step: Same pattern as support/assert steps — quoted all unquoted variables.

All variables derived from user-controlled inputs (bats-version, support-version, assert-version, detik-version, file-version, and their path inputs) are now properly double-quoted throughout the shell scripts, preventing shell metacharacter injection.

### Iteration 3

**Fixes applied:** script-injection

**Notes:**

Fixed the unquoted shell variable expansion in the 'Download and install Bats' step. Changed `[[ $VERSION = v* ]]` to `[[ "$VERSION" = v* ]]` at line 113 of hardened/action/action.yaml. The variable VERSION is derived from the BATS_VERSION env var which is set from inputs.bats-version (a workflow-controllable input), so it must be double-quoted in all shell expansions.

