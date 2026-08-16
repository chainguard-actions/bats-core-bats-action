<!-- markdownlint-disable -->

# Hardening Report: bats-core--bats-action/4.0.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **bats-core--bats-action/4.0.0** was hardened automatically. 26 finding(s) were identified and resolved across 2 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Sub-rule (a): Multiple run: blocks in action.yaml directly interpolate ${{ inputs.* }} expressions inside shell commands without routing through env: variables. Examples include: `VERSION=${{ inputs.bats-version }}` (bats-install step), `VERSION=${{ inputs.support-version }}` (support-install step), `VERSION=${{ inputs.assert-version }}` (assert-install step), `VERSION=${{ inputs.detik-version }}` (detik-install step), `VERSION=${{ inputs.file-version }}` (file-install step). Additionally, the calculate-*-destdir steps use `${{ inputs.*-path }}` directly in if-conditions and echo commands inside run: blocks, and the install steps use `${{ inputs.*-clean }}` in shell conditionals. Any of these inputs can contain shell metacharacters that will be interpreted by the shell before quoting can protect them.

Locations:

- `action.yaml:113`
- `action.yaml:152`
- `action.yaml:153`
- `action.yaml:157`
- `action.yaml:175`
- `action.yaml:196`
- `action.yaml:197`
- `action.yaml:201`
- `action.yaml:218`
- `action.yaml:237`
- `action.yaml:238`
- `action.yaml:242`
- `action.yaml:259`
- `action.yaml:278`
- `action.yaml:279`
- `action.yaml:283`
- `action.yaml:300`

### github-env-injection (severity: high)

Four calculate-DESTDIR steps in action.yaml write ${{ inputs.*-path }} values directly to $GITHUB_ENV without sanitization (no `printf '%s' ... | tr -d '\n\r'` step). An attacker-controlled input containing newlines can inject arbitrary environment variables. Affected steps: 'Calculate DESTDIR for Bats-support' writes `echo "SUPPORT_DESTDIR=${{ inputs.support-path }}" >> $GITHUB_ENV`; 'Calculate DESTDIR for Bats-assert' writes `echo "ASSERT_DESTDIR=${{ inputs.assert-path }}" >> $GITHUB_ENV`; 'Calculate DESTDIR for Bats-detik' writes `echo "DETIK_DESTDIR=${{ inputs.detik-path }}" >> $GITHUB_ENV`; 'Calculate DESTDIR for Bats-file' writes `echo "FILE_DESTDIR=${{ inputs.file-path }}" >> $GITHUB_ENV`.

Locations:

- `action.yaml:157`
- `action.yaml:201`
- `action.yaml:242`
- `action.yaml:283`

### unpinned-uses (severity: high)

Two workflow files reference actions by mutable branch names instead of full 40-character commit SHAs. In test-public-action.yaml: `uses: bats-core/bats-action@main` appears in both the public_test and public_test_trigger_cache jobs (branch ref `main`). In update-pre-commit-hooks.yaml: `uses: brokenpip3/action-pre-commit-update@main` (branch ref `main`). These can be silently updated by the action owner to run arbitrary code.

Locations:

- `.github/workflows/test-public-action.yaml:26`
- `.github/workflows/test-public-action.yaml:68`
- `.github/workflows/update-pre-commit-hooks.yaml:13`

### broad-permissions (severity: medium)

scorecard.yaml sets `permissions: read-all` at the top level, which grants overly broad read access to all repository scopes. This should be replaced with specific minimal permissions scopes.

Locations:

- `.github/workflows/scorecard.yaml:17`

### missing-permissions (severity: medium)

Four CI workflow files have no top-level `permissions:` key and no job-level `permissions:` keys on any of their jobs. Without explicit permissions, GitHub Actions defaults to the repository's default token permissions, which may be overly permissive. Affected files: test-local-action.yaml, test-local-action-inside-home.yaml, test-local-action-with-conditionals.yaml, test-public-action.yaml, and update-pre-commit-hooks.yaml.

Locations:

- `.github/workflows/test-local-action.yaml:1`
- `.github/workflows/test-local-action-inside-home.yaml:1`
- `.github/workflows/test-local-action-with-conditionals.yaml:1`
- `.github/workflows/test-public-action.yaml:1`
- `.github/workflows/update-pre-commit-hooks.yaml:1`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.bats-version }}" appears directly in run: block of step "Download and install Bats"; move to env: map

Locations:

- `action.yml:138`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.support-path }}" appears directly in run: block of step "Calculate DESTDIR for Bats-support"; move to env: map

Locations:

- `action.yml:197`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.support-path }}" appears directly in run: block of step "Calculate DESTDIR for Bats-support"; move to env: map

Locations:

- `action.yml:197`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.support-path }}" appears directly in run: block of step "Calculate DESTDIR for Bats-support"; move to env: map

Locations:

- `action.yml:200`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.support-version }}" appears directly in run: block of step "Download and install Bats-support"; move to env: map

Locations:

- `action.yml:216`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.support-clean }}" appears directly in run: block of step "Download and install Bats-support"; move to env: map

Locations:

- `action.yml:237`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.assert-path }}" appears directly in run: block of step "Calculate DESTDIR for Bats-assert"; move to env: map

Locations:

- `action.yml:246`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.assert-path }}" appears directly in run: block of step "Calculate DESTDIR for Bats-assert"; move to env: map

Locations:

- `action.yml:246`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.assert-path }}" appears directly in run: block of step "Calculate DESTDIR for Bats-assert"; move to env: map

Locations:

- `action.yml:249`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.assert-version }}" appears directly in run: block of step "Download and install Bats-assert"; move to env: map

Locations:

- `action.yml:265`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.assert-clean }}" appears directly in run: block of step "Download and install Bats-assert"; move to env: map

Locations:

- `action.yml:286`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.detik-path }}" appears directly in run: block of step "Calculate DESTDIR for Bats-detik"; move to env: map

Locations:

- `action.yml:295`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.detik-path }}" appears directly in run: block of step "Calculate DESTDIR for Bats-detik"; move to env: map

Locations:

- `action.yml:295`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.detik-path }}" appears directly in run: block of step "Calculate DESTDIR for Bats-detik"; move to env: map

Locations:

- `action.yml:298`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.detik-version }}" appears directly in run: block of step "Download and install Bats-detik"; move to env: map

Locations:

- `action.yml:314`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.detik-clean }}" appears directly in run: block of step "Download and install Bats-detik"; move to env: map

Locations:

- `action.yml:334`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.file-path }}" appears directly in run: block of step "Calculate DESTDIR for Bats-file"; move to env: map

Locations:

- `action.yml:343`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.file-path }}" appears directly in run: block of step "Calculate DESTDIR for Bats-file"; move to env: map

Locations:

- `action.yml:343`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.file-path }}" appears directly in run: block of step "Calculate DESTDIR for Bats-file"; move to env: map

Locations:

- `action.yml:346`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.file-version }}" appears directly in run: block of step "Download and install Bats-file"; move to env: map

Locations:

- `action.yml:362`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.file-clean }}" appears directly in run: block of step "Download and install Bats-file"; move to env: map

Locations:

- `action.yml:383`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection, static-inline-injection, github-env-injection, unpinned-uses, broad-permissions, missing-permissions

**Notes:**

Fixed all findings in action.yaml and workflow files:

1. script-injection/static-inline-injection: Moved all ${{ inputs.* }} expressions from run: blocks to env: blocks in action.yaml. Affected steps: bats-install (bats-version), calculate-support-destdir (support-path), support-install (support-version, support-clean), calculate-assert-destdir (assert-path), assert-install (assert-version, assert-clean), calculate-detik-destdir (detik-path), detik-install (detik-version, detik-clean), calculate-file-destdir (file-path), file-install (file-version, file-clean).

2. github-env-injection: The four calculate-DESTDIR steps now sanitize path values with `printf '%s' "$VAR" | tr -d '\n\r'` before writing to $GITHUB_ENV.

3. unpinned-uses: Pinned bats-core/bats-action@main to full SHA 562ebf679115bde788f656cd532408c2d125297d in test-public-action.yaml (both occurrences), and brokenpip3/action-pre-commit-update@main to 24b7a510db4356b7cb0a2d0b8f72d1226fa7195b in update-pre-commit-hooks.yaml.

4. broad-permissions: Replaced `permissions: read-all` in scorecard.yaml with specific `contents: read` and `actions: read`.

5. missing-permissions: Added `permissions: contents: read` to test-local-action.yaml, test-local-action-inside-home.yaml, test-local-action-with-conditionals.yaml, and test-public-action.yaml. Added `permissions: contents: write` to update-pre-commit-hooks.yaml (required for committing pre-commit hook updates).

### Iteration 2

**Fixes applied:** script-injection, github-env-injection

**Notes:**

Fixed two security issues in hardened/action/action.yaml:

1. github-env-injection (line 340): Added sanitization in the 'build BATS_LIB_PATH' step before writing to GITHUB_OUTPUT. LIB_PATH is now sanitized with `safe_libpath=$(printf '%s' "$LIB_PATH" | tr -d '\n\r')` before the echo to $GITHUB_OUTPUT, preventing newline injection.

2. script-injection (line 348): In the 'Print info' step, moved all ${{ steps.*.outputs.* }} and ${{ steps.*.outputs.* }} expressions out of the run: block into the step's env: block (as BATS_INSTALLED, SUPPORT_INSTALLED, ASSERT_INSTALLED, DETIK_INSTALLED, FILE_INSTALLED, LIB_PATH_OUT, TMP_PATH_OUT). The shell script now references these as plain environment variables, preventing shell metacharacter injection from attacker-controlled step output values.

