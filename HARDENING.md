<!-- markdownlint-disable -->

# Hardening Report: bats-core--bats-action/4.1.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `374911c4be584df24e6da57af2d990570ef0f859`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **bats-core--bats-action/4.1.0** was hardened automatically. 39 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Rule (a): the 'Download and install Bats' step puts an input expression straight into the script: `VERSION=${{ inputs.bats-version }}`. Rule (b): workflow-influenced values are expanded without quotes, e.g. `[[ $VERSION == v* ]]`, `mkdir -p ${TEMPDIR}`, `./install.sh ${DESTDIR}`.

Locations:

- `action.yaml:138`

### script-injection (severity: high)

Rule (a): the 'Calculate DESTDIR for Bats-support' step puts `${{ inputs.support-path }}` straight into the script: `if [ -z "${{ inputs.support-path }}" ] || ...` and `echo "SUPPORT_DESTDIR=${{ inputs.support-path }}" >> $GITHUB_ENV`.

Locations:

- `action.yaml:202`

### script-injection (severity: high)

Rule (a): the 'Download and install Bats-support' step puts input expressions straight into the script: `VERSION=${{ inputs.support-version }}` and `[[ "${{ inputs.support-clean }}" == "true" ]]`. Rule (b): the inherited env vars SUPPORT_DESTDIR, TEMPDIR and CMD are expanded without quotes, e.g. `DESTDIR=${SUPPORT_DESTDIR}`, `${CMD} mkdir -p ${DESTDIR}/src/`, `rm -rf ${TEMPDIR}`.

Locations:

- `action.yaml:221`

### script-injection (severity: high)

Rule (a): the 'Calculate DESTDIR for Bats-assert' step puts `${{ inputs.assert-path }}` straight into the script, including `echo "ASSERT_DESTDIR=${{ inputs.assert-path }}" >> $GITHUB_ENV`.

Locations:

- `action.yaml:251`

### script-injection (severity: high)

Rule (a): the 'Download and install Bats-assert' step puts input expressions straight into the script: `VERSION=${{ inputs.assert-version }}` and `[[ "${{ inputs.assert-clean }}" == "true" ]]`. Rule (b): ASSERT_DESTDIR, TEMPDIR and CMD are expanded without quotes, e.g. `DESTDIR=${ASSERT_DESTDIR}`, `${CMD} install -m755 load.bash ${DESTDIR}/load.bash`.

Locations:

- `action.yaml:270`

### script-injection (severity: high)

Rule (a): the 'Calculate DESTDIR for Bats-detik' step puts `${{ inputs.detik-path }}` straight into the script, including `echo "DETIK_DESTDIR=${{ inputs.detik-path }}" >> $GITHUB_ENV`.

Locations:

- `action.yaml:300`

### script-injection (severity: high)

Rule (a): the 'Download and install Bats-detik' step puts input expressions straight into the script: `VERSION=${{ inputs.detik-version }}` and `[[ "${{ inputs.detik-clean }}" == "true" ]]`. Rule (b): DESTDIR, TEMPDIR and CMD are expanded without quotes, e.g. `${CMD} mkdir -p ${DESTDIR}/src/`.

Locations:

- `action.yaml:319`

### script-injection (severity: high)

Rule (a): the 'Calculate DESTDIR for Bats-file' step puts `${{ inputs.file-path }}` straight into the script, including `echo "FILE_DESTDIR=${{ inputs.file-path }}" >> $GITHUB_ENV`.

Locations:

- `action.yaml:348`

### script-injection (severity: high)

Rule (a): the 'Download and install Bats-file' step puts input expressions straight into the script: `VERSION=${{ inputs.file-version }}` and `[[ "${{ inputs.file-clean }}" == "true" ]]`. Rule (b): DESTDIR, TEMPDIR and CMD are expanded without quotes, e.g. `${CMD} mkdir -p ${DESTDIR}/src/`.

Locations:

- `action.yaml:367`

### script-injection (severity: high)

Rule (b): the 'build BATS_LIB_PATH' step expands inherited env vars without quotes: `S="$(winfix ${SUPPORT_DESTDIR%/*})"` (and the same for ASSERT_DESTDIR, DETIK_DESTDIR and FILE_DESTDIR).

Locations:

- `action.yaml:406`

### script-injection (severity: high)

Rule (a): the 'Print info' step puts expressions straight into the script, e.g. `echo "Bats lib to use in BATS_LIB_PATH env ${{ steps.libpath.outputs.libpath }}"` and `echo "Tmp path: ${{ steps.set-paths.outputs.tmp-path }}"`.

Locations:

- `action.yaml:421`

### github-env-injection (severity: high)

The 'Calculate DESTDIR for Bats-support' step writes `inputs.support-path` (and the inherited DEFAULT_BASE_DIR) to $GITHUB_ENV with no `printf '%s' ... | tr -d '\n\r'` step: `echo "SUPPORT_DESTDIR=${{ inputs.support-path }}" >> $GITHUB_ENV`.

Locations:

- `action.yaml:205`

### github-env-injection (severity: high)

The 'Calculate DESTDIR for Bats-assert' step writes `inputs.assert-path` to $GITHUB_ENV with no sanitization: `echo "ASSERT_DESTDIR=${{ inputs.assert-path }}" >> $GITHUB_ENV`.

Locations:

- `action.yaml:254`

### github-env-injection (severity: high)

The 'Calculate DESTDIR for Bats-detik' step writes `inputs.detik-path` to $GITHUB_ENV with no sanitization: `echo "DETIK_DESTDIR=${{ inputs.detik-path }}" >> $GITHUB_ENV`.

Locations:

- `action.yaml:303`

### github-env-injection (severity: high)

The 'Calculate DESTDIR for Bats-file' step writes `inputs.file-path` to $GITHUB_ENV with no sanitization: `echo "FILE_DESTDIR=${{ inputs.file-path }}" >> $GITHUB_ENV`.

Locations:

- `action.yaml:351`

### github-env-injection (severity: high)

The 'build BATS_LIB_PATH' step builds LIB_PATH from the inherited env vars SUPPORT_DESTDIR, ASSERT_DESTDIR, DETIK_DESTDIR and FILE_DESTDIR, which came from inputs. It then writes the value to $GITHUB_OUTPUT with no sanitization: `echo "libpath=$LIB_PATH" >> $GITHUB_OUTPUT`.

Locations:

- `action.yaml:414`

### github-env-injection (severity: high)

On Windows, the 'Set paths and commands for libraries' step builds TEMPDIR from the inherited $HOME (`TEMPDIR="$HOME/AppData/Local/Temp"`). It then writes it to $GITHUB_ENV and $GITHUB_OUTPUT with no sanitization: `echo "TEMPDIR=$TEMPDIR" >> $GITHUB_ENV`.

Locations:

- `action.yaml:194`

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

### prior-fix (severity: high)

Carried forward from this version's previous harden, which fixed it; fix it again unless the code no longer has the problem. The previous harden changed action.yaml without recording a finding for it, and this analysis reported nothing there. Re-apply its security changes unless the code no longer needs them. The previous patch:
```diff
diff --git a/action.yaml b/action.yaml
index 6196a06..f133ad8 100644
--- a/action.yaml
+++ b/action.yaml
@@ -111,9 +111,26 @@ outputs:
 runs:
   using: composite
   steps:
     # This action would be much easier if only matrix steps will be supported in a composite action
     - name: "Set cache for Bats"
       uses: actions/cache@55cc8345863c7cc4c66a329aec7e433d2d1c52a9 # v4
       if: inputs.bats-install == 'true'
       id: bats-cache
       with:
@@ -127,7 +144,8 @@ runs:
       shell: bash
       run: |
         DESTDIR="$HOME/.local/share/bats"
-        echo "${DESTDIR}/bin" >> "$GITHUB_PATH"
+        safe_path="$(printf '%s' "${DESTDIR}/bin" | tr -d '\n\r')"
+        echo "$safe_path" >> "$GITHUB_PATH"
 
     - name: "Download and install Bats"
       if: inputs.bats-install == 'true' && steps.bats-cache.outputs.cache-hit != 'true'
@@ -135,7 +153,7 @@ runs:
       shell: bash
       run: |
         # In $HOME to avoid sudo requirements
-        VERSION=${{ inputs.bats-version }}
+        VERSION="$INPUT_BATS_VERSION"
         DESTDIR="$HOME/.local/share/bats"
         TEMPDIR="/tmp/bats"
         URL="https://api.github.com"
@@ -149,21 +167,23 @@ runs:
         if [[ -z "$VERSION" ]] || [[ "$VERSION" = "latest" ]]; then
           VERSION=$(curl -fsSL --retry 4 --retry-connrefused "${AUTH_ARGS[@]}" "${URL}/repos/bats-core/bats-core/releases/latest" | grep tag_name | cut -d '"' -f 4)
         fi
-        [[ $VERSION == v* ]] && VERSION="${VERSION:1}"
+        [[ "$VERSION" == v* ]] && VERSION="${VERSION:1}"
 
-        mkdir -p ${TEMPDIR}
-        mkdir -p ${DESTDIR}
+        mkdir -p "${TEMPDIR}"
+        mkdir -p "${DESTDIR}"
 
-        curl -sL --retry 4 --retry-connrefused "${AUTH_ARGS[@]}" "${URL}/repos/bats-core/bats-core/tarball/v${VERSION}" | tar xz -C ${TEMPDIR} --strip-components 1 && cd ${TEMPDIR}
+        curl -sL --retry 4 --retry-connrefused "${AUTH_ARGS[@]}" "${URL}/repos/bats-core/bats-core/tarball/v${VERSION}" | tar xz -C "${TEMPDIR}" --strip-components 1 && cd "${TEMPDIR}"
 
-        ./install.sh ${DESTDIR}
+        ./install.sh "${DESTDIR}"
         echo "Bats v${VERSION} installed in ${DESTDIR}"
-        echo "${DESTDIR}/bin" >> "$GITHUB_PATH"
-        echo "bats-installed=true" >> $GITHUB_OUTPUT
+        safe_path="$(printf '%s' "${DESTDIR}/bin" | tr -d '\n\r')"
+        echo "$safe_path" >> "$GITHUB_PATH"
+        echo "bats-installed=true" >> "$GITHUB_OUTPUT"
 
-        rm -rf ${TEMPDIR} || exit 0
+        rm -rf "${TEMPDIR}" || exit 0
       env:
         GITHUB_TOKEN: ${{ inputs.github-token }}
+        INPUT_BATS_VERSION: ${{ inputs.bats-version }}
 
     - name: Set paths and commands for libraries
       shell: bash
@@ -188,26 +208,34 @@ runs:
           CMD=""
         fi
 
+        TEMPDIR="$(printf '%s' "$TEMPDIR" | tr -d '\n\r')"
+        DEFAULT_BASE_DIR="$(printf '%s' "$DEFAULT_BASE_DIR" | tr -d '\n\r')"
+        CMD="$(printf '%s' "$CMD" | tr -d '\n\r')"
+
         echo "_LIB_PATH=$DEFAULT_BASE_DIR" >> /tmp/bats-lib-path
-        echo "DEFAULT_BASE_DIR=$DEFAULT_BASE_DIR" >> $GITHUB_ENV
-        echo "CMD=$CMD" >> $GITHUB_ENV
-        echo "TEMPDIR=$TEMPDIR" >> $GITHUB_ENV
-        echo "tmp-path=$TEMPDIR" >> $GITHUB_OUTPUT
+        echo "DEFAULT_BASE_DIR=$DEFAULT_BASE_DIR" >> "$GITHUB_ENV"
+        echo "CMD=$CMD" >> "$GITHUB_ENV"
+        echo "TEMPDIR=$TEMPDIR" >> "$GITHUB_ENV"
+        echo "tmp-path=$TEMPDIR" >> "$GITHUB_OUTPUT"
 
     - name: "Calculate DESTDIR for Bats-support"
       if: inputs.support-install == 'true'
       id: calculate-support-destdir
       shell: bash
       run: |
-        if [ -z "${{ inputs.support-path }}" ] || [ "${{ inputs.support-path }}" == "/usr/lib/bats-support" ]; then
-          echo "SUPPORT_DESTDIR=${DEFAULT_BASE_DIR}/bats-support" >> $GITHUB_ENV
+        if [ -z "$INPUT_PATH" ] || [ "$INPUT_PATH" == "/usr/lib/bats-support" ]; then
+          value="${DEFAULT_BASE_DIR}/bats-support"
         else
-          echo "SUPPORT_DESTDIR=${{ inputs.support-path }}" >> $GITHUB_ENV
+          value="$INPUT_PATH"
         fi
+        safe="$(printf
... (truncated)
```

Locations:

- `action.yaml`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection, static-inline-injection, github-env-injection, prior-fix

**Notes:**

I fixed all findings in action.yaml, re-applying the earlier harden's changes and covering the cases it missed.

- **Inputs moved out of the scripts:** every `${{ inputs.* }}` that was written straight into a `run:` script now goes through the step's `env:` block. That covers the Bats version, the four library versions, paths and clean flags, and the 'Print info' step's expressions.
- **Quoting:** variables like `$VERSION`, `TEMPDIR`, `DESTDIR`, `$fn` and the `winfix` arguments are now double-quoted.
- **`$CMD`:** this is either `sudo` or empty, so it can't simply be quoted (an empty quoted value would become a bogus empty command). Each install step turns it into a bash array that is empty when there's no sudo, so commands run the same way as before.
- **Newlines stripped before writing:** values are cleaned with `printf '%s' | tr -d '\n\r'` before going to `$GITHUB_ENV`, `$GITHUB_OUTPUT` or `$GITHUB_PATH`. This applies to the four `*_DESTDIR` values, `TEMPDIR`, `DEFAULT_BASE_DIR`, `CMD`, `LIB_PATH` and the Bats bin path. All redirect targets are now quoted.

Nothing was run to test the changes. The files under tests/ were not modified.

