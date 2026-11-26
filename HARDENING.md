<!-- markdownlint-disable -->

# Hardening Report: bats-core--bats-action/4.0.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `374911c4be584df24e6da57af2d990570ef0f859`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **bats-core--bats-action/4.0.0** was hardened automatically. 38 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Rule (a) and (b), step 'Download and install Bats': `VERSION=${{ inputs.bats-version }}` puts an expression straight into the script. Later lines also expand variables unquoted: `mkdir -p ${TEMPDIR}`, `mkdir -p ${DESTDIR}` and `[[ $VERSION == v* ]]`.

Locations:

- `action.yaml:138`

### script-injection (severity: high)

Rule (a), step 'Calculate DESTDIR for Bats-support': the script contains `if [ -z "${{ inputs.support-path }}" ] || [ "${{ inputs.support-path }}" == ... ]` and `echo "SUPPORT_DESTDIR=${{ inputs.support-path }}"`.

Locations:

- `action.yaml:197`
- `action.yaml:200`

### script-injection (severity: high)

Rule (a) and (b), step 'Download and install Bats-support': `VERSION=${{ inputs.support-version }}` and `[[ "${{ inputs.support-clean }}" == "true" ]]` put expressions straight into the script. Workflow-controlled variables are also expanded unquoted: `DESTDIR=${SUPPORT_DESTDIR}`, `${CMD} mkdir -p ${DESTDIR}/src/`, `mkdir -p ${TEMPDIR}` and `install -m755 load.bash ${DESTDIR}/load.bash`.

Locations:

- `action.yaml:216`
- `action.yaml:237`

### script-injection (severity: high)

Rule (a), step 'Calculate DESTDIR for Bats-assert': the script contains `${{ inputs.assert-path }}` in the `if [ -z ... ]` test and in `echo "ASSERT_DESTDIR=${{ inputs.assert-path }}"`.

Locations:

- `action.yaml:246`
- `action.yaml:249`

### script-injection (severity: high)

Rule (a) and (b), step 'Download and install Bats-assert': `VERSION=${{ inputs.assert-version }}` and `[[ "${{ inputs.assert-clean }}" == "true" ]]` put expressions straight into the script. ${ASSERT_DESTDIR}, ${DESTDIR}, ${TEMPDIR} and ${CMD} are also expanded unquoted.

Locations:

- `action.yaml:265`
- `action.yaml:286`

### script-injection (severity: high)

Rule (a), step 'Calculate DESTDIR for Bats-detik': the script contains `${{ inputs.detik-path }}` in the `if` test and in `echo "DETIK_DESTDIR=${{ inputs.detik-path }}"`.

Locations:

- `action.yaml:295`
- `action.yaml:298`

### script-injection (severity: high)

Rule (a) and (b), step 'Download and install Bats-detik': `VERSION=${{ inputs.detik-version }}` and `[[ "${{ inputs.detik-clean }}" == "true" ]]` put expressions straight into the script. ${DESTDIR}, ${TEMPDIR} and ${CMD} are also expanded unquoted.

Locations:

- `action.yaml:314`
- `action.yaml:334`

### script-injection (severity: high)

Rule (a), step 'Calculate DESTDIR for Bats-file': the script contains `${{ inputs.file-path }}` in the `if` test and in `echo "FILE_DESTDIR=${{ inputs.file-path }}"`.

Locations:

- `action.yaml:343`
- `action.yaml:346`

### script-injection (severity: high)

Rule (a) and (b), step 'Download and install Bats-file': `VERSION=${{ inputs.file-version }}` and `[[ "${{ inputs.file-clean }}" == "true" ]]` put expressions straight into the script. ${DESTDIR}, ${TEMPDIR} and ${CMD} are also expanded unquoted.

Locations:

- `action.yaml:362`
- `action.yaml:383`

### script-injection (severity: high)

Rule (b), step 'build BATS_LIB_PATH': variables inherited from the workflow environment are expanded unquoted inside command substitution, e.g. `S="$(winfix ${SUPPORT_DESTDIR%/*})"`. The same applies to ASSERT_DESTDIR, DETIK_DESTDIR and FILE_DESTDIR.

Locations:

- `action.yaml:401`

### script-injection (severity: high)

Rule (a), step 'Print info': the echo lines put `${{ steps.*.outputs.* }}` expressions straight into the script, e.g. `echo "Bats lib to use in BATS_LIB_PATH env ${{ steps.libpath.outputs.libpath }}"` and `echo "Tmp path: ${{ steps.set-paths.outputs.tmp-path }}"`.

Locations:

- `action.yaml:416`

### github-env-injection (severity: high)

Step 'Calculate DESTDIR for Bats-support' writes `echo "SUPPORT_DESTDIR=${{ inputs.support-path }}" >> $GITHUB_ENV` and `echo "SUPPORT_DESTDIR=${DEFAULT_BASE_DIR}/bats-support" >> $GITHUB_ENV` (DEFAULT_BASE_DIR is inherited from the environment). Neither value goes through `printf '%s' | tr -d '\n\r'` first.

Locations:

- `action.yaml:200`

### github-env-injection (severity: high)

Step 'Calculate DESTDIR for Bats-assert' writes `echo "ASSERT_DESTDIR=${{ inputs.assert-path }}" >> $GITHUB_ENV` (and the DEFAULT_BASE_DIR variant) without stripping newlines first.

Locations:

- `action.yaml:249`

### github-env-injection (severity: high)

Step 'Calculate DESTDIR for Bats-detik' writes `echo "DETIK_DESTDIR=${{ inputs.detik-path }}" >> $GITHUB_ENV` (and the DEFAULT_BASE_DIR variant) without stripping newlines first.

Locations:

- `action.yaml:298`

### github-env-injection (severity: high)

Step 'Calculate DESTDIR for Bats-file' writes `echo "FILE_DESTDIR=${{ inputs.file-path }}" >> $GITHUB_ENV` (and the DEFAULT_BASE_DIR variant) without stripping newlines first.

Locations:

- `action.yaml:346`

### github-env-injection (severity: high)

Step 'build BATS_LIB_PATH' writes `echo "libpath=$LIB_PATH" >> $GITHUB_OUTPUT`. LIB_PATH is built from SUPPORT_DESTDIR, ASSERT_DESTDIR, DETIK_DESTDIR and FILE_DESTDIR, which are inherited from the environment and come from inputs, and it is not sanitized with `printf '%s' | tr -d '\n\r'`.

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

I fixed every finding in action.yaml. All the findings point to that file, even the ones that say action.yml.

- **Inputs out of run: scripts:** every `${{ inputs.* }}` expression in a `run:` block now goes through the step's `env:` block. That covers bats/support/assert/detik/file version, path and clean. The same goes for the `steps.*.outputs` expressions in the 'Print info' step.
- **Quoting:** I quoted the variables that were expanded bare: VERSION, TEMPDIR, DESTDIR, `$fn`, `basename`, the arguments to `winfix`, and `$GITHUB_ENV`/`$GITHUB_OUTPUT`. CMD is either 'sudo' or empty, so it is now `${CMD:+"$CMD"}`. That drops out when empty, as the unquoted form did before.
- **Newline stripping:** the four 'Calculate DESTDIR' steps now build the value first, strip newlines with `printf '%s' | tr -d '\n\r'`, then write it to GITHUB_ENV. The libpath output gets the same treatment before it goes to GITHUB_OUTPUT.
- **Prior fix:** this re-applies the previous harden's changes (moving the version and path inputs into `env:`), and goes further on quoting.

The action should behave the same as upstream. I haven't run it or re-run the analysis.

