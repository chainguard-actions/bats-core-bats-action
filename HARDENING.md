<!-- markdownlint-disable -->

# Hardening Report: bats-core--bats-action/3.0.1

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **bats-core--bats-action/3.0.1** was hardened automatically. 23 finding(s) were identified and resolved across 3 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Rule (a): Multiple run: blocks in action.yaml directly interpolate ${{ ... }} expressions inside shell commands, enabling script injection. Affected steps: (1) 'Download and install Bats': VERSION=${{ inputs.bats-version }} at line 134, and ${{ github.token }} in two curl commands at lines 141 and 148. (2) 'Calculate DESTDIR for Bats-support': ${{ inputs.support-path }} in shell if-test and echo at line 185. (3) 'Download and install Bats-support': VERSION=${{ inputs.support-version }} and ${{ inputs.support-clean }} interpolated directly. (4) 'Calculate DESTDIR for Bats-assert': ${{ inputs.assert-path }} interpolated directly. (5) 'Download and install Bats-assert': VERSION=${{ inputs.assert-version }} and ${{ inputs.assert-clean }} interpolated directly. (6) 'Calculate DESTDIR for Bats-detik': ${{ inputs.detik-path }} interpolated directly. (7) 'Download and install Bats-detik': VERSION=${{ inputs.detik-version }} and ${{ inputs.detik-clean }} interpolated directly. (8) 'Calculate DESTDIR for Bats-file': ${{ inputs.file-path }} interpolated directly. (9) 'Download and install Bats-file': VERSION=${{ inputs.file-version }} and ${{ inputs.file-clean }} interpolated directly. (10) 'Print info': ${{ steps.*.outputs.* }} expressions in echo commands. All ${{ }} expressions in run: blocks must be moved to env: variables and referenced as "$VAR" in the shell.

Locations:

- `action.yaml:134`
- `action.yaml:141`
- `action.yaml:148`
- `action.yaml:185`
- `action.yaml:188`
- `action.yaml:199`
- `action.yaml:213`
- `action.yaml:220`
- `action.yaml:223`
- `action.yaml:233`
- `action.yaml:247`
- `action.yaml:254`
- `action.yaml:257`
- `action.yaml:267`
- `action.yaml:281`
- `action.yaml:288`
- `action.yaml:291`
- `action.yaml:301`
- `action.yaml:315`
- `action.yaml:338`

### github-env-injection (severity: high)

Four run: blocks write ${{ inputs.*-path }} values directly to $GITHUB_ENV without the required sanitization step (printf '%s' ... | tr -d '\n\r'). An attacker-controlled path input containing newlines can inject arbitrary environment variables into subsequent steps. (1) 'Calculate DESTDIR for Bats-support': echo "SUPPORT_DESTDIR=${{ inputs.support-path }}" >> $GITHUB_ENV. (2) 'Calculate DESTDIR for Bats-assert': echo "ASSERT_DESTDIR=${{ inputs.assert-path }}" >> $GITHUB_ENV. (3) 'Calculate DESTDIR for Bats-detik': echo "DETIK_DESTDIR=${{ inputs.detik-path }}" >> $GITHUB_ENV. (4) 'Calculate DESTDIR for Bats-file': echo "FILE_DESTDIR=${{ inputs.file-path }}" >> $GITHUB_ENV. Each should sanitize first: safe=$(printf '%s' "$INPUT_PATH" | tr -d '\n\r') before writing to $GITHUB_ENV.

Locations:

- `action.yaml:188`
- `action.yaml:223`
- `action.yaml:257`
- `action.yaml:291`

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

**Fixes applied:** script-injection, github-env-injection, static-inline-injection

**Notes:**

Fixed all script injection and github-env-injection findings in hardened/action/action.yaml:

1. 'Download and install Bats': Moved ${{ inputs.bats-version }} → BATS_VERSION and ${{ github.token }} → GITHUB_TOKEN into env: block; referenced as $BATS_VERSION and $GITHUB_TOKEN in shell.

2. 'Calculate DESTDIR for Bats-support': Moved ${{ inputs.support-path }} → SUPPORT_PATH into env: block; added sanitization (printf '%s' "$SUPPORT_PATH" | tr -d '\n\r') before writing to $GITHUB_ENV.

3. 'Download and install Bats-support': Moved ${{ inputs.support-version }} → SUPPORT_VERSION and ${{ inputs.support-clean }} → SUPPORT_CLEAN into env: block.

4. 'Calculate DESTDIR for Bats-assert': Moved ${{ inputs.assert-path }} → ASSERT_PATH into env: block; added sanitization before writing to $GITHUB_ENV.

5. 'Download and install Bats-assert': Moved ${{ inputs.assert-version }} → ASSERT_VERSION and ${{ inputs.assert-clean }} → ASSERT_CLEAN into env: block.

6. 'Calculate DESTDIR for Bats-detik': Moved ${{ inputs.detik-path }} → DETIK_PATH into env: block; added sanitization before writing to $GITHUB_ENV.

7. 'Download and install Bats-detik': Moved ${{ inputs.detik-version }} → DETIK_VERSION and ${{ inputs.detik-clean }} → DETIK_CLEAN into env: block.

8. 'Calculate DESTDIR for Bats-file': Moved ${{ inputs.file-path }} → FILE_PATH into env: block; added sanitization before writing to $GITHUB_ENV.

9. 'Download and install Bats-file': Moved ${{ inputs.file-version }} → FILE_VERSION and ${{ inputs.file-clean }} → FILE_CLEAN into env: block.

10. 'Print info': Moved all ${{ steps.*.outputs.* }} expressions into env: block (BATS_INSTALLED, SUPPORT_INSTALLED, ASSERT_INSTALLED, DETIK_INSTALLED, FILE_INSTALLED, LIB_PATH, TMP_PATH) and referenced as plain env vars in shell.

### Iteration 2

**Fixes applied:** script-injection, github-env-injection

**Notes:**

Fixed all unquoted variable expansions in 5 install steps (bats-install, support-install, assert-install, detik-install, file-install) by adding double quotes around ${url}, ${TEMPDIR}, ${DESTDIR}, $fn, $(basename $fn), and other variables sourced from user-controlled inputs. Fixed github-env-injection in the libpath step by sanitizing LIB_PATH with `printf '%s' "$LIB_PATH" | tr -d '\n\r'` before writing to $GITHUB_OUTPUT.

### Iteration 3

**Fixes applied:** script-injection

**Notes:**

Fixed the unquoted shell variable expansion in the 'Download and install Bats' step (id: bats-install) in hardened/action/action.yaml. Changed `[[ $VERSION == v* ]]` to `[[ "$VERSION" == v* ]]` on line 130 to properly double-quote the VERSION variable (which is sourced from the attacker-controllable input `inputs.bats-version`). While bash's [[ ]] suppresses word splitting, double-quoting is still required as a best practice to prevent any potential injection from untrusted data.

