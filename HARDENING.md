<!-- markdownlint-disable -->

# Hardening Report: bats-core--bats-action/4.0.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `374911c4be584df24e6da57af2d990570ef0f859`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **bats-core--bats-action/4.0.0** was hardened automatically. 38 finding(s) were identified and resolved across 3 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Step 'Download and install Bats': (a) puts an expression straight into the run script: `VERSION=${{ inputs.bats-version }}`. (b) It also expands ${TEMPDIR}/${DESTDIR} without quotes (`mkdir -p ${TEMPDIR}`, `./install.sh ${DESTDIR}`, `rm -rf ${TEMPDIR}`).

Locations:

- `action.yaml:138`

### script-injection (severity: high)

Step 'Calculate DESTDIR for Bats-support': (a) puts expressions straight into the run script: `if [ -z "${{ inputs.support-path }}" ] || [ "${{ inputs.support-path }}" == ...` and `echo "SUPPORT_DESTDIR=${{ inputs.support-path }}" >> $GITHUB_ENV`.

Locations:

- `action.yaml:197`

### script-injection (severity: high)

Step 'Download and install Bats-support': (a) puts expressions straight into the run script: `VERSION=${{ inputs.support-version }}` and `[[ "${{ inputs.support-clean }}" == "true" ]]`. (b) It also expands the inherited env vars ${CMD}, ${DESTDIR} (from SUPPORT_DESTDIR) and ${TEMPDIR} without quotes, e.g. `${CMD} mkdir -p ${DESTDIR}/src/` and `${CMD} install -m755 load.bash ${DESTDIR}/load.bash`.

Locations:

- `action.yaml:216`

### script-injection (severity: high)

Step 'Calculate DESTDIR for Bats-assert': (a) puts expressions straight into the run script: `if [ -z "${{ inputs.assert-path }}" ] ...` and `echo "ASSERT_DESTDIR=${{ inputs.assert-path }}" >> $GITHUB_ENV`.

Locations:

- `action.yaml:246`

### script-injection (severity: high)

Step 'Download and install Bats-assert': (a) puts expressions straight into the run script: `VERSION=${{ inputs.assert-version }}` and `[[ "${{ inputs.assert-clean }}" == "true" ]]`. (b) It also expands ${CMD}, ${DESTDIR} (from ASSERT_DESTDIR) and ${TEMPDIR} without quotes.

Locations:

- `action.yaml:265`

### script-injection (severity: high)

Step 'Calculate DESTDIR for Bats-detik': (a) puts expressions straight into the run script: `if [ -z "${{ inputs.detik-path }}" ] ...` and `echo "DETIK_DESTDIR=${{ inputs.detik-path }}" >> $GITHUB_ENV`.

Locations:

- `action.yaml:295`

### script-injection (severity: high)

Step 'Download and install Bats-detik': (a) puts expressions straight into the run script: `VERSION=${{ inputs.detik-version }}` and `[[ "${{ inputs.detik-clean }}" == "true" ]]`. (b) It also expands ${CMD}, ${DESTDIR} and ${TEMPDIR} without quotes, e.g. `${CMD} mkdir -p ${DESTDIR}/src/`.

Locations:

- `action.yaml:314`

### script-injection (severity: high)

Step 'Calculate DESTDIR for Bats-file': (a) puts expressions straight into the run script: `if [ -z "${{ inputs.file-path }}" ] ...` and `echo "FILE_DESTDIR=${{ inputs.file-path }}" >> $GITHUB_ENV`.

Locations:

- `action.yaml:343`

### script-injection (severity: high)

Step 'Download and install Bats-file': (a) puts expressions straight into the run script: `VERSION=${{ inputs.file-version }}` and `[[ "${{ inputs.file-clean }}" == "true" ]]`. (b) It also expands ${CMD}, ${DESTDIR} and ${TEMPDIR} without quotes.

Locations:

- `action.yaml:362`

### script-injection (severity: high)

Step 'build BATS_LIB_PATH': (b) expands inherited env vars without quotes: `S="$(winfix ${SUPPORT_DESTDIR%/*})"` (and the same for ASSERT_DESTDIR, DETIK_DESTDIR, FILE_DESTDIR).

Locations:

- `action.yaml:401`

### script-injection (severity: high)

Step 'Print info': (a) puts expressions straight into the run script, e.g. `echo "Bats lib to use in BATS_LIB_PATH env ${{ steps.libpath.outputs.libpath }}"` and `echo "Tmp path: ${{ steps.set-paths.outputs.tmp-path }}"`, plus the ${{ steps.*.outputs.* }} installed flags.

Locations:

- `action.yaml:416`

### github-env-injection (severity: high)

Step 'Calculate DESTDIR for Bats-support' writes `echo "SUPPORT_DESTDIR=${{ inputs.support-path }}" >> $GITHUB_ENV` and `echo "SUPPORT_DESTDIR=${DEFAULT_BASE_DIR}/bats-support" >> $GITHUB_ENV` (DEFAULT_BASE_DIR is inherited env). Neither value goes through printf '%s' | tr -d '\n\r' first.

Locations:

- `action.yaml:200`

### github-env-injection (severity: high)

Step 'Calculate DESTDIR for Bats-assert' writes `echo "ASSERT_DESTDIR=${{ inputs.assert-path }}" >> $GITHUB_ENV` (and a value built from DEFAULT_BASE_DIR) without stripping newlines first.

Locations:

- `action.yaml:249`

### github-env-injection (severity: high)

Step 'Calculate DESTDIR for Bats-detik' writes `echo "DETIK_DESTDIR=${{ inputs.detik-path }}" >> $GITHUB_ENV` (and a value built from DEFAULT_BASE_DIR) without stripping newlines first.

Locations:

- `action.yaml:298`

### github-env-injection (severity: high)

Step 'Calculate DESTDIR for Bats-file' writes `echo "FILE_DESTDIR=${{ inputs.file-path }}" >> $GITHUB_ENV` (and a value built from DEFAULT_BASE_DIR) without stripping newlines first.

Locations:

- `action.yaml:346`

### github-env-injection (severity: high)

Step 'build BATS_LIB_PATH' writes `echo "libpath=$LIB_PATH" >> $GITHUB_OUTPUT`. LIB_PATH is built from the inherited env vars SUPPORT_DESTDIR, ASSERT_DESTDIR, DETIK_DESTDIR and FILE_DESTDIR, and it is not passed through printf '%s' | tr -d '\n\r' before the write.

Locations:

- `action.yaml:409`

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

### prior-fix (severity: high)

Carried forward from this version's previous harden, which fixed it; fix it again unless the code no longer has the problem. The previous harden changed action.yaml without recording a finding for it, and this analysis reported nothing there. Re-apply its security changes unless the code no longer needs them. The previous patch:
```diff
diff --git a/action.yaml b/action.yaml
index 05b33de..6378b9d 100644
--- a/action.yaml
+++ b/action.yaml
@@ -107,10 +107,26 @@ outputs:
   tmp-path:
     description: "Temporary path with each library tests"
     value: ${{ steps.set-paths.outputs.tmp-path }}
-
 runs:
   using: composite
   steps:
     # This action would be much easier if only matrix steps will be supported in a composite action
     - name: "Set cache for Bats"
       uses: actions/cache@9255dc7a253b0ccc959486e2bca901246202afeb # v4
@@ -120,7 +136,6 @@ runs:
         path: |
           ~/.local/share/bats
         key: ${{ runner.os }}-${{ runner.arch }}-bats-${{ inputs.bats-version }}
-
     - name: "Set PATH in case of cache-hit"
       if: inputs.bats-install == 'true' && steps.bats-cache.outputs.cache-hit == 'true'
       id: bats-cache-path
@@ -128,43 +143,14 @@ runs:
       run: |
         DESTDIR="$HOME/.local/share/bats"
         echo "${DESTDIR}/bin" >> "$GITHUB_PATH"
-
     - name: "Download and install Bats"
       if: inputs.bats-install == 'true' && steps.bats-cache.outputs.cache-hit != 'true'
       id: bats-install
       shell: bash
-      run: |
-        # In $HOME to avoid sudo requirements
-        VERSION=${{ inputs.bats-version }}
-        DESTDIR="$HOME/.local/share/bats"
-        TEMPDIR="/tmp/bats"
-        URL="https://api.github.com"
-
-        declare -a AUTH_ARGS=()
-        if [ "$GITHUB_TOKEN" != "" ]; then
-          AUTH_ARGS=( -H "Authorization: token $GITHUB_TOKEN" )
-        fi 
-
-        # From https://github.com/fluxcd/flux2/blob/44d69d6fc0c353e79c1bad021a4aca135033bce8/action/action.yml#L35
-        if [[ -z "$VERSION" ]] || [[ "$VERSION" = "latest" ]]; then
-          VERSION=$(curl -fsSL --retry 4 --retry-connrefused "${AUTH_ARGS[@]}" "${URL}/repos/bats-core/bats-core/releases/latest" | grep tag_name | cut -d '"' -f 4)
-        fi
-        [[ $VERSION == v* ]] && VERSION="${VERSION:1}"
-
-        mkdir -p ${TEMPDIR}
-        mkdir -p ${DESTDIR}
-
-        curl -sL --retry 4 --retry-connrefused "${AUTH_ARGS[@]}" "${URL}/repos/bats-core/bats-core/tarball/v${VERSION}" | tar xz -C ${TEMPDIR} --strip-components 1 && cd ${TEMPDIR}
-
-        ./install.sh ${DESTDIR}
-        echo "Bats v${VERSION} installed in ${DESTDIR}"
-        echo "${DESTDIR}/bin" >> "$GITHUB_PATH"
-        echo "bats-installed=true" >> $GITHUB_OUTPUT
-
-        rm -rf ${TEMPDIR} || exit 0
       env:
+        INPUT_BATS_VERSION: ${{ inputs.bats-version }}
         GITHUB_TOKEN: ${{ inputs.github-token }}
-
+      run: "# In $HOME to avoid sudo requirements\nVERSION=\"$INPUT_BATS_VERSION\"\nDESTDIR=\"$HOME/.local/share/bats\"\nTEMPDIR=\"/tmp/bats\"\nURL=\"https://api.github.com\"\n\ndeclare -a AUTH_ARGS=()\nif [ \"$GITHUB_TOKEN\" != \"\" ]; then\n  AUTH_ARGS=( -H \"Authorization: token $GITHUB_TOKEN\" )\nfi \n\n# From https://github.com/fluxcd/flux2/blob/44d69d6fc0c353e79c1bad021a4aca135033bce8/action/action.yml#L35\nif [[ -z \"$VERSION\" ]] || [[ \"$VERSION\" = \"latest\" ]]; then\n  VERSION=$(curl -fsSL --retry 4 --retry-connrefused \"${AUTH_ARGS[@]}\" \"${URL}/repos/bats-core/bats-core/releases/latest\" | grep tag_name | cut -d '\"' -f 4)\nfi\n[[ $VERSION == v* ]] && VERSION=\"${VERSION:1}\"\n\nmkdir -p ${TEMPDIR}\nmkdir -p ${DESTDIR}\n\ncurl -sL --retry 4 --retry-connrefused \"${AUTH_ARGS[@]}\" \"${URL}/repos/bats-core/bats-core/tarball/v${VERSION}\" | tar xz -C ${TEMPDIR} --strip-components 1 && cd ${TEMPDIR}\n\n./install.sh ${DESTDIR}\necho \"Bats v${VERSION} installed in ${DESTDIR}\"\necho \"${DESTDIR}/bin\" >> \"$GITHUB_PATH\"\necho \"bats-installed=true\" >> $GITHUB_OUTPUT\n\nrm -rf ${TEMPDIR} || exit 0\n"
     - name: Set paths and commands for libraries
       shell: bash
       id: set-paths
@@ -188,18 +174,19 @@ runs:
         echo "CMD=$CMD" >> $GITHUB_ENV
         echo "TEMPDIR=$TEMPDIR" >> $GITHUB_ENV
         echo "tmp-path=$TEMPDIR" >> $GITHUB_OUTPUT
-
     - name: "Calculate DESTDIR for Bats-support"
       if: inputs.support-install == 'true'
       id: calculate-support-destdir
       shell: bash
+      env:
+        INPUT_SUP
... (truncated)
```

Locations:

- `action.yaml`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection, github-env-injection, static-inline-injection, prior-fix

**Notes:**

action.yaml: moved every ${{ inputs.* }} (bats/support/assert/detik/file version, path, clean) and the steps.*.outputs used in 'Print info' out of run: scripts into env: blocks; scripts now read quoted env vars. Quoted TEMPDIR/DESTDIR/fn expansions everywhere; ${CMD} changed to ${CMD:+"$CMD"} so it still drops out when empty (Windows / HOME-based dir); quoted winfix arguments. Calculate DESTDIR steps and the libpath output now go through printf '%s' | tr -d '\n\r' before writing to GITHUB_ENV/GITHUB_OUTPUT. Quoted $GITHUB_ENV/$GITHUB_OUTPUT. Behavior is otherwise unchanged; prior harden's changes re-applied. Not run or tested.

### Iteration 2

**Fixes applied:** script-injection

**Notes:**

Quoted $VERSION in the 'Download and install Bats' step of action.yaml: `[[ "$VERSION" == v* ]]`. Behavior is unchanged because the right-hand pattern v* is still unquoted, so it still works as a glob.

### Iteration 3

**Fixes applied:** github-env-injection

**Notes:**

Fixed all three findings in action.yaml. Values built from $HOME now have newlines and carriage returns stripped with printf '%s' ... | tr -d '\n\r' before they are written. This covers the ${DESTDIR}/bin line written to $GITHUB_PATH in the 'Set PATH in case of cache-hit' step and in the 'Download and install Bats' step. It also covers TEMPDIR in the 'Set paths and commands for libraries' step, which is written both to $GITHUB_ENV and to the tmp-path output. Each value is captured on its own line, so errexit still works the same way. Nothing else in the action changed. Nothing was run to test these edits.

