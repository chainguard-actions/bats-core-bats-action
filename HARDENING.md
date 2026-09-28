<!-- markdownlint-disable -->

# Hardening Report: bats-core--bats-action/3.0.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **bats-core--bats-action/3.0.0** was hardened automatically. 24 finding(s) were identified and resolved across 4 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

action.yaml contains 5 uses of `actions/cache@v4` pinned to a mutable version tag rather than an immutable 40-character commit SHA. A tag can be moved to point to a different (potentially malicious) commit at any time, enabling supply-chain attacks. All occurrences should be replaced with the full SHA, e.g. `actions/cache@1bd1e32a3bdc45362d1e726936510720a7c6158d # v4`.

Locations:

- `action.yaml:106`
- `action.yaml:186`
- `action.yaml:218`
- `action.yaml:251`
- `action.yaml:284`

### script-injection (severity: high)

Multiple `run:` blocks in action.yaml directly interpolate `${{ inputs.* }}` and `${{ steps.*.outputs.* }}` expressions inside shell command strings (rule a). GitHub Actions performs template substitution before the shell ever sees the string, so a caller-supplied value containing shell metacharacters (`;`, `|`, `$(...)`, backticks, etc.) is executed as shell code.

Affected lines and offending expressions:
- Step 'Download and install Bats' (line ~132): `VERSION=${{ inputs.bats-version }}`
- Step 'Calculate DESTDIR for Bats-support' (line ~177): `if [ -z "${{ inputs.support-path }}" ] || [ "${{ inputs.support-path }}" == ... ]`
- Step 'Download and install Bats-support' (line ~197): `VERSION=${{ inputs.support-version }}`
- Step 'Download and install Bats-support' (line ~210): `[[ "${{ inputs.support-clean }}" == "true" ]]`
- Step 'Calculate DESTDIR for Bats-assert' (line ~215): `if [ -z "${{ inputs.assert-path }}" ] || [ "${{ inputs.assert-path }}" == ... ]`
- Step 'Download and install Bats-assert' (line ~230): `VERSION=${{ inputs.assert-version }}`
- Step 'Download and install Bats-assert' (line ~243): `[[ "${{ inputs.assert-clean }}" == "true" ]]`
- Step 'Calculate DESTDIR for Bats-detik' (line ~248): `if [ -z "${{ inputs.detik-path }}" ] || [ "${{ inputs.detik-path }}" == ... ]`
- Step 'Download and install Bats-detik' (line ~263): `VERSION=${{ inputs.detik-version }}`
- Step 'Download and install Bats-detik' (line ~276): `[[ "${{ inputs.detik-clean }}" == "true" ]]`
- Step 'Calculate DESTDIR for Bats-file' (line ~281): `if [ -z "${{ inputs.file-path }}" ] || [ "${{ inputs.file-path }}" == ... ]`
- Step 'Download and install Bats-file' (line ~296): `VERSION=${{ inputs.file-version }}`
- Step 'Download and install Bats-file' (line ~309): `[[ "${{ inputs.file-clean }}" == "true" ]]`
- Step 'Print info' (line ~330+): `${{ steps.bats-install.outputs.bats-installed }}`, `${{ steps.libpath.outputs.libpath }}`, `${{ steps.set-paths.outputs.tmp-path }}`

Fix: move each expression into an `env:` block and reference it as a quoted shell variable (e.g. `"$VERSION"`) inside the `run:` script.

Locations:

- `action.yaml:132`
- `action.yaml:177`
- `action.yaml:197`
- `action.yaml:210`
- `action.yaml:215`
- `action.yaml:230`
- `action.yaml:243`
- `action.yaml:248`
- `action.yaml:263`
- `action.yaml:276`
- `action.yaml:281`
- `action.yaml:296`
- `action.yaml:309`

### github-env-injection (severity: high)

Four `calculate-*-destdir` steps write caller-supplied `${{ inputs.*-path }}` values directly to `$GITHUB_ENV` without sanitization. Because GitHub Actions performs template substitution before the shell runs, a value containing a newline character can inject an arbitrary key=value pair into the runner's environment, allowing an attacker to override environment variables seen by subsequent steps (e.g. overriding `PATH`, `LD_PRELOAD`, or any secret variable name).

Affected lines:
- Step 'Calculate DESTDIR for Bats-support' (~line 180): `echo "SUPPORT_DESTDIR=${{ inputs.support-path }}" >> $GITHUB_ENV`
- Step 'Calculate DESTDIR for Bats-assert' (~line 222): `echo "ASSERT_DESTDIR=${{ inputs.assert-path }}" >> $GITHUB_ENV`
- Step 'Calculate DESTDIR for Bats-detik' (~line 255): `echo "DETIK_DESTDIR=${{ inputs.detik-path }}" >> $GITHUB_ENV`
- Step 'Calculate DESTDIR for Bats-file' (~line 288): `echo "FILE_DESTDIR=${{ inputs.file-path }}" >> $GITHUB_ENV`

Fix: route each input through an `env:` block and sanitize before writing:
```yaml
env:
  SUPPORT_PATH: ${{ inputs.support-path }}
run: |
  safe=$(printf '%s' "$SUPPORT_PATH" | tr -d '\n\r')
  echo "SUPPORT_DESTDIR=$safe" >> "$GITHUB_ENV"
```

Locations:

- `action.yaml:180`
- `action.yaml:222`
- `action.yaml:255`
- `action.yaml:288`

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

**Fixes applied:** unpinned-uses, script-injection, github-env-injection, static-inline-injection

**Notes:**

Fixed all security findings in hardened/action/action.yaml:
1. Pinned all 5 `actions/cache@v4` references to full SHA `0057852bfaa89a56745cba8c7296529d2fc39830`.
2. Moved all `${{ inputs.* }}` expressions from run: blocks into env: blocks for: bats-version, support-version, support-clean, support-path, assert-version, assert-clean, assert-path, detik-version, detik-clean, detik-path, file-version, file-clean, file-path.
3. Fixed github-env-injection in all four calculate-*-destdir steps by sanitizing path values with `printf '%s' "$VAR" | tr -d '\n\r'` before writing to $GITHUB_ENV.
4. Fixed the Print info step by moving `${{ steps.*.outputs.* }}` expressions to env: variables and replacing the boolean expressions with shell equivalents using `[ -n "$VAR" ] && echo true || echo false`.

### Iteration 2

**Fixes applied:** script-injection

**Notes:**

Fixed all 5 script-injection findings in action.yaml by adding double quotes around unquoted shell variables in the 'run:' blocks:

1. 'Download and install Bats' step: Quoted ${TEMPDIR}, ${DESTDIR}, ${URL}, ${VERSION} in mkdir, curl, tar, install.sh, and rm commands.

2. 'Download and install Bats-support' step: Quoted ${TEMPDIR}, ${DESTDIR}, ${url}, ${CMD} in mkdir, curl, tar, install, and rm commands. Also quoted $fn and $(basename $fn) in the for loop.

3. 'Download and install Bats-assert' step: Same pattern as support — quoted all variables including loop variables.

4. 'Download and install Bats-detik' step: Same pattern — quoted ${TEMPDIR}, ${DESTDIR}, ${url}, ${CMD}, $fn, and $(basename $fn).

5. 'Download and install Bats-file' step: Same pattern — quoted all variables.

All user-controlled input values (bats-version, support-version, assert-version, detik-version, file-version) flow through VERSION/url variables which are now properly double-quoted to prevent shell word-splitting and glob expansion.

### Iteration 3

**Fixes applied:** script-injection

**Notes:**

Fixed unquoted shell variable expansions in the 'build BATS_LIB_PATH' step of action.yaml. The four winfix() calls now properly quote the parameter expansions: `winfix "${SUPPORT_DESTDIR%/*}"`, `winfix "${ASSERT_DESTDIR%/*}"`, `winfix "${DETIK_DESTDIR%/*}"`, and `winfix "${FILE_DESTDIR%/*}"`. This prevents word splitting and glob expansion when these variables (derived from user-controlled inputs) contain shell metacharacters such as spaces, glob characters, or semicolons.

### Iteration 4

**Fixes applied:** github-env-injection

**Notes:**

Fixed all github-env-injection findings in action.yaml:
1. set-paths step (lines 127-130): Added sanitization for DEFAULT_BASE_DIR, CMD, and TEMPDIR using `printf '%s' ... | tr -d '\n\r'` before writing to $GITHUB_ENV and $GITHUB_OUTPUT.
2. Calculate DESTDIR steps for bats-support, bats-assert, bats-detik, and bats-file (lines 141, 172, 214, 255): Fixed the `if` branches to sanitize `${DEFAULT_BASE_DIR}/bats-*` values before writing to $GITHUB_ENV, matching the sanitization already present in the `else` branches.
3. libpath step (line 316): Added sanitization for LIB_PATH using `printf '%s' ... | tr -d '\n\r'` before writing to $GITHUB_OUTPUT.

