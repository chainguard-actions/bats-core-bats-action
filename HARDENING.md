<!-- markdownlint-disable -->

# Hardening Report: bats-core--bats-action/3.0.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **bats-core--bats-action/3.0.0** was hardened automatically. 24 finding(s) were identified and resolved across 3 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Multiple run: blocks in action.yaml directly interpolate ${{ inputs.* }} expressions into shell commands (sub-rule a). This allows an attacker who controls the calling workflow's inputs to inject arbitrary shell commands. Affected steps and patterns: 'Download and install Bats' step uses `VERSION=${{ inputs.bats-version }}`; 'Calculate DESTDIR for Bats-support' step uses `${{ inputs.support-path }}` in if-conditions and echo; 'Download and install Bats-support' step uses `VERSION=${{ inputs.support-version }}` and `[[ "${{ inputs.support-clean }}" == "true" ]]`; same patterns repeat for assert, detik, and file variants. All inputs should be passed via env: variables and then double-quoted in the shell script.

Locations:

- `action.yaml:113`
- `action.yaml:148`
- `action.yaml:150`
- `action.yaml:152`
- `action.yaml:159`
- `action.yaml:168`
- `action.yaml:183`
- `action.yaml:185`
- `action.yaml:187`
- `action.yaml:194`
- `action.yaml:203`
- `action.yaml:218`
- `action.yaml:220`
- `action.yaml:222`
- `action.yaml:229`
- `action.yaml:238`
- `action.yaml:253`
- `action.yaml:255`
- `action.yaml:257`
- `action.yaml:264`
- `action.yaml:273`

### github-env-injection (severity: high)

Four run: blocks write ${{ inputs.*-path }} values directly to $GITHUB_ENV without sanitization (no `printf '%s' ... | tr -d '\n\r'` step). An attacker-controlled input containing embedded newlines could inject arbitrary key=value pairs into the runner environment. Failing lines: `echo "SUPPORT_DESTDIR=${{ inputs.support-path }}" >> $GITHUB_ENV`, `echo "ASSERT_DESTDIR=${{ inputs.assert-path }}" >> $GITHUB_ENV`, `echo "DETIK_DESTDIR=${{ inputs.detik-path }}" >> $GITHUB_ENV`, `echo "FILE_DESTDIR=${{ inputs.file-path }}" >> $GITHUB_ENV`.

Locations:

- `action.yaml:152`
- `action.yaml:187`
- `action.yaml:222`
- `action.yaml:257`

### unpinned-uses (severity: high)

All 5 `uses:` references in action.yaml pin to the mutable tag `@v4` instead of an immutable 40-character commit SHA. If the actions/cache repository is compromised or the tag is moved, the action will silently execute attacker-controlled code. All five occurrences of `uses: actions/cache@v4` (for Bats, Bats-support, Bats-assert, Bats-detik, and Bats-file cache steps) must be replaced with `uses: actions/cache@<full-40-char-SHA> # v4`.

Locations:

- `action.yaml:100`
- `action.yaml:163`
- `action.yaml:198`
- `action.yaml:233`
- `action.yaml:268`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.bats-version }}" appears directly in run: block of step "Download and install Bats"; move to env: map

Locations:

- `action.yml:134`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.support-path }}" appears directly in run: block of step "Calculate DESTDIR for Bats-support"; move to env: map

Locations:

- `action.yml:186`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.support-path }}" appears directly in run: block of step "Calculate DESTDIR for Bats-support"; move to env: map

Locations:

- `action.yml:186`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.support-path }}" appears directly in run: block of step "Calculate DESTDIR for Bats-support"; move to env: map

Locations:

- `action.yml:189`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.support-version }}" appears directly in run: block of step "Download and install Bats-support"; move to env: map

Locations:

- `action.yml:205`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.support-clean }}" appears directly in run: block of step "Download and install Bats-support"; move to env: map

Locations:

- `action.yml:221`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.assert-path }}" appears directly in run: block of step "Calculate DESTDIR for Bats-assert"; move to env: map

Locations:

- `action.yml:228`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.assert-path }}" appears directly in run: block of step "Calculate DESTDIR for Bats-assert"; move to env: map

Locations:

- `action.yml:228`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.assert-path }}" appears directly in run: block of step "Calculate DESTDIR for Bats-assert"; move to env: map

Locations:

- `action.yml:231`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.assert-version }}" appears directly in run: block of step "Download and install Bats-assert"; move to env: map

Locations:

- `action.yml:247`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.assert-clean }}" appears directly in run: block of step "Download and install Bats-assert"; move to env: map

Locations:

- `action.yml:263`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.detik-path }}" appears directly in run: block of step "Calculate DESTDIR for Bats-detik"; move to env: map

Locations:

- `action.yml:270`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.detik-path }}" appears directly in run: block of step "Calculate DESTDIR for Bats-detik"; move to env: map

Locations:

- `action.yml:270`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.detik-path }}" appears directly in run: block of step "Calculate DESTDIR for Bats-detik"; move to env: map

Locations:

- `action.yml:273`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.detik-version }}" appears directly in run: block of step "Download and install Bats-detik"; move to env: map

Locations:

- `action.yml:289`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.detik-clean }}" appears directly in run: block of step "Download and install Bats-detik"; move to env: map

Locations:

- `action.yml:304`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.file-path }}" appears directly in run: block of step "Calculate DESTDIR for Bats-file"; move to env: map

Locations:

- `action.yml:311`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.file-path }}" appears directly in run: block of step "Calculate DESTDIR for Bats-file"; move to env: map

Locations:

- `action.yml:311`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.file-path }}" appears directly in run: block of step "Calculate DESTDIR for Bats-file"; move to env: map

Locations:

- `action.yml:314`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.file-version }}" appears directly in run: block of step "Download and install Bats-file"; move to env: map

Locations:

- `action.yml:330`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.file-clean }}" appears directly in run: block of step "Download and install Bats-file"; move to env: map

Locations:

- `action.yml:346`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, script-injection, static-inline-injection, github-env-injection

**Notes:**

Fixed all findings in hardened/action/action.yaml:
1. Pinned all 5 `actions/cache@v4` references to full SHA `0057852bfaa89a56745cba8c7296529d2fc39830` with `# v4` comment.
2. Moved all ${{ inputs.* }} expressions from run: blocks into env: blocks (BATS_VERSION, INPUT_SUPPORT_PATH/VERSION/CLEAN, INPUT_ASSERT_PATH/VERSION/CLEAN, INPUT_DETIK_PATH/VERSION/CLEAN, INPUT_FILE_PATH/VERSION/CLEAN). Shell scripts now reference plain env vars double-quoted.
3. For the four 'Calculate DESTDIR' steps that write path inputs to $GITHUB_ENV: sanitized the non-default-path branch using `printf '%s' "$VAR" | tr -d '\n\r'` before writing to $GITHUB_ENV to prevent newline injection.

### Iteration 2

**Fixes applied:** script-injection

**Notes:**

Fixed all three script-injection findings in hardened/action/action.yaml:

1. 'Print info' step (Finding 1): Moved all ${{ steps.*.outputs.* }} expressions to an env: block (BATS_INSTALLED, SUPPORT_INSTALLED, ASSERT_INSTALLED, DETIK_INSTALLED, FILE_INSTALLED, LIB_PATH, TMP_PATH) and referenced them as plain shell variables in the run: block.

2. 'Download and install Bats' step (Finding 2): Added double-quotes around $VERSION in the [[ ]] comparison, and around ${URL}, ${VERSION}, ${TEMPDIR}, ${DESTDIR} in curl, mkdir, tar, and install commands.

3. All four library install steps (Finding 3 - support/assert/detik/file): Quoted ${url} in curl commands; replaced unquoted ${CMD} with ${CMD:+sudo} (safe parameter expansion that yields 'sudo' when CMD='sudo' and nothing when CMD=''); quoted ${TEMPDIR} and ${DESTDIR} throughout; quoted $fn and $(basename $fn) in for loops.

### Iteration 3

**Fixes applied:** script-injection, unpinned-uses, missing-permissions

**Notes:**

Fixed all four findings: (1) Quoted the unquoted variable expansions in winfix() calls in action.yaml (${SUPPORT_DESTDIR%/*} etc. now properly quoted); (2) Quoted all unquoted ${TMP_PATH} and $BATS_LIB_PATH usages in run blocks across all affected workflow files; (3) Pinned all unpinned action references to full 40-char SHAs: actions/checkout@v4→11d5960a..., bats-core/bats-action@main→562ebf67..., brokenpip3/action-pre-commit-update@main→24b7a510...; (4) Added permissions: {} top-level blocks to all 5 workflow files, with job-level contents:write and pull-requests:write for update-pre-commit-hooks.yaml since that job needs to create commits/PRs.

