# PNPM Configuration Defaults 7/8

## Method

1. Go to each version's `npmrc` documenation page
   - v7: https://pnpm.io/7.x/npmrc
   - v8: https://pnpm.io/npmrc (while latest, after: https://pnpm.io/8.x/npmrc)
2. Run this script in the browser developer tools to get the list of options
   ```javascript
   [...document.querySelectorAll('ul.table-of-contents > li > ul > li > a.table-of-contents__link')]
     .map(a => a.textContent)
     .join('\n')
   ```
   This produces 76 entries for v7 and 78 entries for v8.
3. Save each to a respective `.npmrc` file and fill in the defaults per the docs
   
   This gives us `7.npmrc` and `8.npmrc`
4. Diff the two configuration files to find removals/additions/changes:
   
   `git diff --no-index 7.npmrc 8.npmrc > diff.diff`
   ```diff
   diff --git a/7.npmrc b/8.npmrc
   index 61a345c..8308caa 100644
   --- a/7.npmrc
   +++ b/8.npmrc
   @@ -13,7 +13,6 @@ modules-cache-max-age=10080
   lockfile=true
   prefer-frozen-lockfile=true
   lockfile-include-tarball-url=false
   -use-lockfile-v6=false
   registry=https://registry.npmjs.org/
   <URL>:_authToken=
   <URL>:tokenHelper=
   @@ -35,10 +34,10 @@ fetch-retry-factor=10
   fetch-retry-mintimeout=10000
   fetch-retry-maxtimeout=60000
   fetch-timeout=60000
   -auto-install-peers=false
   -dedupe-peer-dependents=false
   +auto-install-peers=true
   +dedupe-peer-dependents=true
   strict-peer-dependencies=false
   -resolve-peers-from-workspace-root=false
   +resolve-peers-from-workspace-root=true
   no-color=auto
   loglevel=info
   use-beta-cli=false
   @@ -57,8 +56,9 @@ node-mirror:<releaseDir>=https://nodejs.org/download/<releaseDir>/
   link-workspace-packages=true
   prefer-workspace-packages=false
   shared-workspace-lockfile=true
   -save-workspace-protocol=true
   +save-workspace-protocol=rolling
   include-workspace-root=false
   +ignore-workspace-cycles=false
   use-running-store-server=false
   save-prefix='^'
   tag=latest
   @@ -71,6 +71,8 @@ update-notifier=true
   prefer-symlinked-executables=true, when node-linker is set to hoisted and the system is POSIX
   verify-store-integrity=true
   ignore-compatibility-db=false
   -resolution-mode=highest
   +resolution-mode=lowest-direct
   registry-supports-time-field=false
   extend-node-path=true
   +deploy-all-files=false
   +dedupe-direct-deps=false
   ```
5. Determine the changes:
   - `use-v6-lockfile` was removed (was `false`) and became `true` by default
     and is no longer configurable
     - https://github.com/pnpm/pnpm/releases/tag/v8.0.0 "Lockfile Modifications"
   - `auto-install-peers`, `dedupe-peer-dependents` and
     `resolve-peers-from-workspace-root` changed to `true` (were `false`)
   - `save-workspace-protocol` changed to `rolling` (was `true`)
   - `ignore-workspace-cycles` was introduced and is `false`
   - `resolution-mode` changed to `lowest-direct` (was `highest`)
   - `deploy-all-files` and `dedupe-direct-deps` were introduced and are `false`

   I cross-checked these with the release notes for PNPM 8 releases and they
   seem to line up!
