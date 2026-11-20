<!-- markdownlint-disable -->

# Hardening Report: bats-core--bats-action/3.0.1

> This file was generated automatically by the hardening agent.

**Policy SHA:** `374911c4be584df24e6da57af2d990570ef0f859`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **bats-core--bats-action/3.0.1** was hardened automatically. 38 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Rule (a) and (b): step 'Download and install Bats' puts expressions straight into the script: `VERSION=${{ inputs.bats-version }}` and `-H "Authorization: token ${{ github.token }}"`. It also expands variables without quotes (${TEMPDIR}, ${DESTDIR}, and ${URL}/archive/refs/tags/v${VERSION}.tar.gz, where VERSION comes from the input).

Locations:

- `action.yaml:134`
- `action.yaml:141`
- `action.yaml:148`

### script-injection (severity: high)

Rule (a): step 'Calculate DESTDIR for Bats-support' puts ${{ inputs.support-path }} straight into the script: `if [ -z "${{ inputs.support-path }}" ] ...` and `echo "SUPPORT_DESTDIR=${{ inputs.support-path }}"`.

Locations:

- `action.yaml:186`

### script-injection (severity: high)

Rule (a) and (b): step 'Download and install Bats-support' puts `VERSION=${{ inputs.support-version }}` and `[[ "${{ inputs.support-clean }}" == "true" ]]` straight into the script. It also expands workflow-controlled values without quotes: ${url}, ${DESTDIR} (from env SUPPORT_DESTDIR), ${CMD} and ${TEMPDIR}.

Locations:

- `action.yaml:205`
- `action.yaml:221`

### script-injection (severity: high)

Rule (a): step 'Calculate DESTDIR for Bats-assert' puts ${{ inputs.assert-path }} straight into the script, in the if test and in the echo to $GITHUB_ENV.

Locations:

- `action.yaml:228`

### script-injection (severity: high)

Rule (a) and (b): step 'Download and install Bats-assert' puts `VERSION=${{ inputs.assert-version }}` and `[[ "${{ inputs.assert-clean }}" == "true" ]]` straight into the script. It also expands ${url}, ${DESTDIR} (from ASSERT_DESTDIR), ${CMD} and ${TEMPDIR} without quotes.

Locations:

- `action.yaml:247`
- `action.yaml:263`

### script-injection (severity: high)

Rule (a): step 'Calculate DESTDIR for Bats-detik' puts ${{ inputs.detik-path }} straight into the script, in the if test and in the echo to $GITHUB_ENV.

Locations:

- `action.yaml:270`

### script-injection (severity: high)

Rule (a) and (b): step 'Download and install Bats-detik' puts `VERSION=${{ inputs.detik-version }}` and `[[ "${{ inputs.detik-clean }}" == "true" ]]` straight into the script. It also expands ${url}, ${DESTDIR}, ${CMD} and ${TEMPDIR} without quotes.

Locations:

- `action.yaml:289`
- `action.yaml:304`

### script-injection (severity: high)

Rule (a): step 'Calculate DESTDIR for Bats-file' puts ${{ inputs.file-path }} straight into the script, in the if test and in the echo to $GITHUB_ENV.

Locations:

- `action.yaml:311`

### script-injection (severity: high)

Rule (a) and (b): step 'Download and install Bats-file' puts `VERSION=${{ inputs.file-version }}` and `[[ "${{ inputs.file-clean }}" == "true" ]]` straight into the script. It also expands ${url}, ${DESTDIR}, ${CMD} and ${TEMPDIR} without quotes.

Locations:

- `action.yaml:330`
- `action.yaml:346`

### script-injection (severity: high)

Rule (b): step 'build BATS_LIB_PATH' expands inherited env vars without quotes inside command substitutions: `S="$(winfix ${SUPPORT_DESTDIR%/*})"`, and the same for ASSERT_DESTDIR, DETIK_DESTDIR and FILE_DESTDIR. These values come from the *-path inputs.

Locations:

- `action.yaml:362`

### script-injection (severity: high)

Rule (a): step 'Print info' puts expressions straight into the script, e.g. `echo "Bats lib to use in BATS_LIB_PATH env ${{ steps.libpath.outputs.libpath }}"` and `echo "Tmp path: ${{ steps.set-paths.outputs.tmp-path }}"`. The libpath output comes from the *-path inputs.

Locations:

- `action.yaml:377`

### github-env-injection (severity: high)

Step 'Calculate DESTDIR for Bats-support' writes `echo "SUPPORT_DESTDIR=${{ inputs.support-path }}" >> $GITHUB_ENV` with no printf '%s' | tr -d '\n\r' cleanup first.

Locations:

- `action.yaml:189`

### github-env-injection (severity: high)

Step 'Calculate DESTDIR for Bats-assert' writes `echo "ASSERT_DESTDIR=${{ inputs.assert-path }}" >> $GITHUB_ENV` with no newline cleanup first.

Locations:

- `action.yaml:231`

### github-env-injection (severity: high)

Step 'Calculate DESTDIR for Bats-detik' writes `echo "DETIK_DESTDIR=${{ inputs.detik-path }}" >> $GITHUB_ENV` with no newline cleanup first.

Locations:

- `action.yaml:273`

### github-env-injection (severity: high)

Step 'Calculate DESTDIR for Bats-file' writes `echo "FILE_DESTDIR=${{ inputs.file-path }}" >> $GITHUB_ENV` with no newline cleanup first.

Locations:

- `action.yaml:314`

### github-env-injection (severity: high)

Step 'build BATS_LIB_PATH' writes `echo "libpath=$LIB_PATH" >> $GITHUB_OUTPUT` with no newline cleanup first. LIB_PATH is built from the inherited env vars SUPPORT_DESTDIR, ASSERT_DESTDIR, DETIK_DESTDIR and FILE_DESTDIR, which come from the inputs.

Locations:

- `action.yaml:370`

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

### prior-fix (severity: high)

Carried forward from this version's previous harden, which fixed it; fix it again unless the code no longer has the problem. The previous harden changed action.yaml without recording a finding for it, and this analysis reported nothing there. Re-apply its security changes unless the code no longer needs them. The previous patch:
```diff
diff --git a/action.yaml b/action.yaml
index 349275f..e716d00 100644
--- a/action.yaml
+++ b/action.yaml
@@ -103,10 +103,26 @@ outputs:
   tmp-path:
     description: "Temporary path with each library tests"
     value: ${{ steps.set-paths.outputs.tmp-path }}
-
 runs:
   using: composite
   steps:
     # This action would be much easier if only matrix steps will be supported in a composite action
     - name: "Set cache for Bats"
       uses: actions/cache@d4323d4df104b026a6aa633fdb11d772146be0bf # v4
@@ -116,7 +132,6 @@ runs:
         path: |
           ~/.local/share/bats
         key: ${{ runner.os }}-${{ runner.arch }}-bats-${{ inputs.bats-version }}
-
     - name: "Set PATH in case of cache-hit"
       if: inputs.bats-install == 'true' && steps.bats-cache.outputs.cache-hit == 'true'
       id: bats-cache-path
@@ -124,36 +139,37 @@ runs:
       run: |
         DESTDIR="$HOME/.local/share/bats"
         echo "${DESTDIR}/bin" >> "$GITHUB_PATH"
-
     - name: "Download and install Bats"
       if: inputs.bats-install == 'true' && steps.bats-cache.outputs.cache-hit != 'true'
       id: bats-install
       shell: bash
+      env:
+        BATS_VERSION: ${{ inputs.bats-version }}
+        GITHUB_TOKEN: ${{ github.token }}
       run: |
         # In $HOME to avoid sudo requirements
-        VERSION=${{ inputs.bats-version }}
+        VERSION="$BATS_VERSION"
         DESTDIR="$HOME/.local/share/bats"
         TEMPDIR="/tmp/bats"
         URL="https://github.com/bats-core/bats-core/"
 
         # From https://github.com/fluxcd/flux2/blob/44d69d6fc0c353e79c1bad021a4aca135033bce8/action/action.yml#L35
         if [[ -z "$VERSION" ]] || [[ "$VERSION" = "latest" ]]; then
-          VERSION=$(curl -fsSL --retry 4 --retry-connrefused -H "Authorization: token ${{ github.token }}" https://api.github.com/repos/bats-core/bats-core/releases/latest | grep tag_name | cut -d '"' -f 4)
+          VERSION=$(curl -fsSL --retry 4 --retry-connrefused -H "Authorization: token $GITHUB_TOKEN" https://api.github.com/repos/bats-core/bats-core/releases/latest | grep tag_name | cut -d '"' -f 4)
         fi
-        [[ $VERSION == v* ]] && VERSION="${VERSION:1}"
+        [[ "$VERSION" == v* ]] && VERSION="${VERSION:1}"
 
-        mkdir -p ${TEMPDIR}
-        mkdir -p ${DESTDIR}
+        mkdir -p "${TEMPDIR}"
+        mkdir -p "${DESTDIR}"
 
-        curl -sL --retry 4 --retry-connrefused -H "Authorization: token ${{ github.token }}" ${URL}/archive/refs/tags/v${VERSION}.tar.gz | tar xz -C ${TEMPDIR} --strip-components 1 && cd ${TEMPDIR}
+        curl -sL --retry 4 --retry-connrefused -H "Authorization: token $GITHUB_TOKEN" "${URL}/archive/refs/tags/v${VERSION}.tar.gz" | tar xz -C "${TEMPDIR}" --strip-components 1 && cd "${TEMPDIR}"
 
-        ./install.sh ${DESTDIR}
+        ./install.sh "${DESTDIR}"
         echo "Bats v${VERSION} installed in ${DESTDIR}"
         echo "${DESTDIR}/bin" >> "$GITHUB_PATH"
-        echo "bats-installed=true" >> $GITHUB_OUTPUT
-
-        rm -rf ${TEMPDIR} || exit 0
+        echo "bats-installed=true" >> "$GITHUB_OUTPUT"
 
+        rm -rf "${TEMPDIR}" || exit 0
     - name: Set paths and commands for libraries
       shell: bash
       id: set-paths
@@ -177,18 +193,19 @@ runs:
         echo "CMD=$CMD" >> $GITHUB_ENV
         echo "TEMPDIR=$TEMPDIR" >> $GITHUB_ENV
         echo "tmp-path=$TEMPDIR" >> $GITHUB_OUTPUT
-
     - name: "Calculate DESTDIR for Bats-support"
       if: inputs.support-install == 'true'
       id: calculate-support-destdir
       shell: bash
+      env:
+        SUPPORT_PATH: ${{ inputs.support-path }}
       run: |
-        if [ -z "${{ inputs.support-path }}" ] || [ "${{ inputs.support-path }}" == "/usr/lib/bats-support" ]; then
+        if [ -z "$SUPPORT_PATH" ] || [ "$SUPPORT_PATH" == "/usr/lib/bats-support" ]; then
           echo "SUPPORT_DESTDIR=${DEFAULT_BASE_DIR}/bats-support" >> $GITHUB_ENV
         else
-          echo "SUPPORT_DESTDIR=${{ inputs.support-path }}" >> $GITHUB_ENV
+          safe=$(printf '%s' "$SUPPORT_PATH" | tr -d '\n\r')
+          echo "SUPPORT_DESTDIR=$safe" >> $GITHUB_E
... (truncated)
```

Locations:

- `action.yaml`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection, github-env-injection, static-inline-injection, prior-fix

**Notes:**

I fixed every finding in action.yaml. Nothing was tested or run after the edit.

- **Script injection:** every `${{ }}` expression that sat inside a `run:` block now goes through the step's `env:` block instead. This covers the bats/support/assert/detik/file version, path and clean inputs, `github.token`, and the step outputs used in 'Print info'.
- **Quoting:** the paths, URLs and loop variables (TEMPDIR, DESTDIR, url, fn) are now in double quotes, including the winfix arguments in 'build BATS_LIB_PATH'. `${CMD}` is still unquoted on purpose. It is either `sudo` or empty, set by the action itself, and quoting it would break the empty case.
- **Env/output writes:** the four `*_DESTDIR` values written to `$GITHUB_ENV` and the `libpath` value written to `$GITHUB_OUTPUT` now have newlines stripped first (`printf '%s' | tr -d '\n\r'`).
- **Previous harden:** its changes are applied again. `$GITHUB_ENV` and `$GITHUB_OUTPUT` are now quoted wherever they are written.

The action's behaviour is otherwise the same as upstream. Every shell is still bash, and the `actions/cache` pin was already a full commit SHA, so it is unchanged.

