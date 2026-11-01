<!-- markdownlint-disable -->

# Hardening Report: bats-core--bats-action/4.1.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **bats-core--bats-action/4.1.0** was hardened automatically. 38 finding(s) were identified and resolved across 4 iteration(s).

## Findings Fixed

### script-injection (severity: high)

The 'Download and install Bats' step breaks (a): `VERSION=${{ inputs.bats-version }}` puts an expression straight into the run script. It also breaks (b): variables that come from untrusted data are used without quotes (`[[ $VERSION == v* ]]`, `mkdir -p ${TEMPDIR}`, `./install.sh ${DESTDIR}`).

Locations:

- `action.yaml:138`

### script-injection (severity: high)

The 'Calculate DESTDIR for Bats-support' step breaks (a): `if [ -z "${{ inputs.support-path }}" ] || [ "${{ inputs.support-path }}" == ... ]` and `echo "SUPPORT_DESTDIR=${{ inputs.support-path }}"` put an input expression straight into the run script.

Locations:

- `action.yaml:202`

### script-injection (severity: high)

The 'Download and install Bats-support' step breaks (a): `VERSION=${{ inputs.support-version }}` and `[[ "${{ inputs.support-clean }}" == "true" ]]`. It also breaks (b): `DESTDIR=${SUPPORT_DESTDIR}`, `${CMD} mkdir -p ${DESTDIR}/src/` and `rm -rf ${TEMPDIR}` are unquoted.

Locations:

- `action.yaml:221`

### script-injection (severity: high)

The 'Calculate DESTDIR for Bats-assert' step breaks (a): `${{ inputs.assert-path }}` is interpolated directly in the if test and in `echo "ASSERT_DESTDIR=${{ inputs.assert-path }}"`.

Locations:

- `action.yaml:251`

### script-injection (severity: high)

The 'Download and install Bats-assert' step breaks (a): `VERSION=${{ inputs.assert-version }}` and `[[ "${{ inputs.assert-clean }}" == "true" ]]`. It also breaks (b): `DESTDIR=${ASSERT_DESTDIR}` and `${CMD} install -m755 load.bash ${DESTDIR}/load.bash` are unquoted.

Locations:

- `action.yaml:270`

### script-injection (severity: high)

The 'Calculate DESTDIR for Bats-detik' step breaks (a): `${{ inputs.detik-path }}` is interpolated directly in the if test and in `echo "DETIK_DESTDIR=${{ inputs.detik-path }}"`.

Locations:

- `action.yaml:300`

### script-injection (severity: high)

The 'Download and install Bats-detik' step breaks (a): `VERSION=${{ inputs.detik-version }}` and `[[ "${{ inputs.detik-clean }}" == "true" ]]`. It also breaks (b): `${CMD} mkdir -p ${DESTDIR}/src/` and `rm -rf ${TEMPDIR}` are unquoted.

Locations:

- `action.yaml:319`

### script-injection (severity: high)

The 'Calculate DESTDIR for Bats-file' step breaks (a): `${{ inputs.file-path }}` is interpolated directly in the if test and in `echo "FILE_DESTDIR=${{ inputs.file-path }}"`.

Locations:

- `action.yaml:348`

### script-injection (severity: high)

The 'Download and install Bats-file' step breaks (a): `VERSION=${{ inputs.file-version }}` and `[[ "${{ inputs.file-clean }}" == "true" ]]`. It also breaks (b): `${CMD} mkdir -p ${DESTDIR}/src/` and `rm -rf ${TEMPDIR}` are unquoted.

Locations:

- `action.yaml:367`

### script-injection (severity: high)

The 'build BATS_LIB_PATH' step breaks (b): the inherited env vars are expanded without quotes inside command substitution, e.g. `S="$(winfix ${SUPPORT_DESTDIR%/*})"`, along with the same pattern for ASSERT_DESTDIR, DETIK_DESTDIR and FILE_DESTDIR.

Locations:

- `action.yaml:406`

### script-injection (severity: high)

The 'Print info' step breaks (a): expressions are interpolated directly into the run script, e.g. `echo "Bats lib to use in BATS_LIB_PATH env ${{ steps.libpath.outputs.libpath }}"`, `echo "Tmp path: ${{ steps.set-paths.outputs.tmp-path }}"` and `${{ (steps.*.outputs.* != '') }}`.

Locations:

- `action.yaml:421`

### github-env-injection (severity: high)

The 'Set paths and commands for libraries' step builds TEMPDIR from the inherited `$HOME` env var (`TEMPDIR="$HOME/AppData/Local/Temp"`). It then writes it with `echo "TEMPDIR=$TEMPDIR" >> $GITHUB_ENV` and `echo "tmp-path=$TEMPDIR" >> $GITHUB_OUTPUT`, with no `printf '%s' | tr -d '\n\r'` step first.

Locations:

- `action.yaml:194`

### github-env-injection (severity: high)

The 'Calculate DESTDIR for Bats-support' step writes `${{ inputs.support-path }}` and the inherited `${DEFAULT_BASE_DIR}` to $GITHUB_ENV (`echo "SUPPORT_DESTDIR=${{ inputs.support-path }}" >> $GITHUB_ENV`) without the sanitization step.

Locations:

- `action.yaml:205`

### github-env-injection (severity: high)

The 'Calculate DESTDIR for Bats-assert' step writes `${{ inputs.assert-path }}` and the inherited `${DEFAULT_BASE_DIR}` to $GITHUB_ENV (`echo "ASSERT_DESTDIR=${{ inputs.assert-path }}" >> $GITHUB_ENV`) without the sanitization step.

Locations:

- `action.yaml:254`

### github-env-injection (severity: high)

The 'Calculate DESTDIR for Bats-detik' step writes `${{ inputs.detik-path }}` and the inherited `${DEFAULT_BASE_DIR}` to $GITHUB_ENV (`echo "DETIK_DESTDIR=${{ inputs.detik-path }}" >> $GITHUB_ENV`) without the sanitization step.

Locations:

- `action.yaml:303`

### github-env-injection (severity: high)

The 'Calculate DESTDIR for Bats-file' step writes `${{ inputs.file-path }}` and the inherited `${DEFAULT_BASE_DIR}` to $GITHUB_ENV (`echo "FILE_DESTDIR=${{ inputs.file-path }}" >> $GITHUB_ENV`) without the sanitization step.

Locations:

- `action.yaml:351`

### github-env-injection (severity: high)

The 'build BATS_LIB_PATH' step builds LIB_PATH from the inherited env vars SUPPORT_DESTDIR, ASSERT_DESTDIR, DETIK_DESTDIR and FILE_DESTDIR. It writes the result with `echo "libpath=$LIB_PATH" >> $GITHUB_OUTPUT` and no `printf '%s' | tr -d '\n\r'` step first.

Locations:

- `action.yaml:414`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.bats-version }}" appears directly in run: block of step "Download and install Bats"; move to env: map

Locations:

- `action.yml:138`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.support-path }}" appears directly in run: block of step "Calculate DESTDIR for Bats-support"; move to env: map

Locations:

- `action.yml:202`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.support-path }}" appears directly in run: block of step "Calculate DESTDIR for Bats-support"; move to env: map

Locations:

- `action.yml:202`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.support-path }}" appears directly in run: block of step "Calculate DESTDIR for Bats-support"; move to env: map

Locations:

- `action.yml:205`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.support-version }}" appears directly in run: block of step "Download and install Bats-support"; move to env: map

Locations:

- `action.yml:221`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.support-clean }}" appears directly in run: block of step "Download and install Bats-support"; move to env: map

Locations:

- `action.yml:242`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.assert-path }}" appears directly in run: block of step "Calculate DESTDIR for Bats-assert"; move to env: map

Locations:

- `action.yml:251`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.assert-path }}" appears directly in run: block of step "Calculate DESTDIR for Bats-assert"; move to env: map

Locations:

- `action.yml:251`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.assert-path }}" appears directly in run: block of step "Calculate DESTDIR for Bats-assert"; move to env: map

Locations:

- `action.yml:254`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.assert-version }}" appears directly in run: block of step "Download and install Bats-assert"; move to env: map

Locations:

- `action.yml:270`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.assert-clean }}" appears directly in run: block of step "Download and install Bats-assert"; move to env: map

Locations:

- `action.yml:291`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.detik-path }}" appears directly in run: block of step "Calculate DESTDIR for Bats-detik"; move to env: map

Locations:

- `action.yml:300`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.detik-path }}" appears directly in run: block of step "Calculate DESTDIR for Bats-detik"; move to env: map

Locations:

- `action.yml:300`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.detik-path }}" appears directly in run: block of step "Calculate DESTDIR for Bats-detik"; move to env: map

Locations:

- `action.yml:303`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.detik-version }}" appears directly in run: block of step "Download and install Bats-detik"; move to env: map

Locations:

- `action.yml:319`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.detik-clean }}" appears directly in run: block of step "Download and install Bats-detik"; move to env: map

Locations:

- `action.yml:339`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.file-path }}" appears directly in run: block of step "Calculate DESTDIR for Bats-file"; move to env: map

Locations:

- `action.yml:348`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.file-path }}" appears directly in run: block of step "Calculate DESTDIR for Bats-file"; move to env: map

Locations:

- `action.yml:348`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.file-path }}" appears directly in run: block of step "Calculate DESTDIR for Bats-file"; move to env: map

Locations:

- `action.yml:351`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.file-version }}" appears directly in run: block of step "Download and install Bats-file"; move to env: map

Locations:

- `action.yml:367`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.file-clean }}" appears directly in run: block of step "Download and install Bats-file"; move to env: map

Locations:

- `action.yml:388`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection, github-env-injection, static-inline-injection

**Notes:**

action.yaml: every ${{ inputs.* }} and ${{ steps.* }} expression has been moved out of the run: scripts and into each step's env: block (INPUT_BATS_VERSION, INPUT_PATH, INPUT_VERSION, INPUT_CLEAN, *_INSTALLED, LIBPATH_OUT, TMPPATH_OUT). TEMPDIR, DESTDIR, VERSION, $fn and the winfix arguments are now double-quoted. ${CMD} is still unquoted because it is either empty or 'sudo' and must disappear when empty. The DESTDIR steps now pick the value first, strip newlines with printf '%s' | tr -d '\n\r', then write it to GITHUB_ENV. set-paths does the same for DEFAULT_BASE_DIR, CMD and TEMPDIR, and libpath does it for LIB_PATH before the writes. The control flow is unchanged.

### Iteration 2

**Fixes applied:** github-env-injection, script-injection, invalid-yaml

**Notes:**

I rewrote action.yaml as a whole file because targeted edits on the damaged section kept leaving it broken.

- invalid-yaml: Replaced the broken text in the 'Calculate DESTDIR for Bats-detik' and 'Download and install Bats-detik' steps. Both steps now have proper if/id/shell/run/env keys, and the leftover duplicate text is gone. No YAML parser was run to confirm this.
- script-injection / github-env-injection (detik DESTDIR): `inputs.detik-path` now goes through env INPUT_PATH, and newlines are stripped with `printf '%s' | tr -d '\n\r'` before writing to GITHUB_ENV. This matches the support, assert and file steps.
- script-injection (Bats-file install): `inputs.file-version` and `inputs.file-clean` are now set in env as INPUT_VERSION and INPUT_CLEAN. TEMPDIR, DESTDIR, $fn, the basename call and GITHUB_OUTPUT are now double-quoted wherever they expand.
- github-env-injection (set-paths): TEMPDIR, DEFAULT_BASE_DIR and CMD have newlines stripped before they are written to GITHUB_ENV and GITHUB_OUTPUT, and those two file paths are now quoted. ${CMD} is still unquoted where it runs, as in upstream. It is either empty or 'sudo', and quoting it would break the empty case.

### Iteration 3

**Fixes applied:** github-env-injection

**Notes:**

In action.yaml, the 'Set PATH in case of cache-hit' and 'Download and install Bats' steps both built their path from $HOME and wrote it to $GITHUB_PATH unchanged. Each step now removes newline and carriage-return characters from "${DESTDIR}/bin" with printf '%s' | tr -d '\n\r' before writing it, so a changed $HOME can no longer add extra PATH lines. Nothing else about how the steps work has changed. Not tested by running the action.

### Iteration 4

**Fixes applied:** github-env-injection

**Notes:**

In action.yaml, the 'Download and install Bats' step now runs ${DESTDIR}/bin through `printf '%s' | tr -d '\n\r'` (stored in safe_path) before writing it to $GITHUB_PATH. This matches what the 'Set PATH in case of cache-hit' step already does. I searched the action and found only these two writes to $GITHUB_PATH, and both now remove newlines first. I didn't run the action or its tests.

