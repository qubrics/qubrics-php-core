# PHP core Qubrics rebrand

Goal: Prepare PHP core source for `qubrics/core` 3.0.0, with `Qubrics\Core` helpers and `QubricsApiError`, without publication.

Status: coded, not merged. No commit, push, PR, tag, release or Packagist mutation performed in this lane.

## Scope and compatibility

- Base: `a9b5d2146d55df07f8243c8a51bbd5dbf9d9c8d5` on `agent/rebrand-qubrics-php4`.
- Spec: [A11, A5 and A17](https://github.com/qubrics/qubrics/blob/main/specs/rebrand-qubrics/spec.md).
- `composer.json:2` names `qubrics/core`; `composer.json:13` maps `Qubrics\Core\` to `src/`.
- `src/ErrorMapper.php:11` returns `QubricsApiError`; `src/QubricsApiError.php:9` defines the renamed exception.
- All eight source files use `Qubrics\Core`; customer-facing validation, pagination and fallback error messages use Qubrics.
- `smx_root_`, `smx_mbx_` and `smx_agent_` prefixes, HTTP headers, response keys, retry logic and dependency constraints stay unchanged.
- README documents the Qubrics package family, namespace, preferred environment name with legacy fallback, and qubrics.com links. The 3.0.0 install command remains conditional on Packagist availability.
- Changelog records prospective 3.0.0 as unreleased. Historical entries and evidence are untouched.

## Red and green

Added `tests/public-contract.php`: real Composer autoload and installed-package metadata, Auth's accepted credential prefixes and invalid-input rejection, Guzzle's handler stack for bearer headers, Headers helpers, ErrorMapper's renamed exception and retained retry/request metadata. Guzzle's MockHandler prevents outbound HTTP.

- Red: `php tests/public-contract.php`, exit 255 before implementation: `RuntimeException: Composer must install qubrics/core`.
- Green: same command after package/namespace/class rename and Composer installation, exit 0: `Qubrics core public contract passed`.
- Escape closed: the previous CI ran metadata validation and syntax checks only, so it could not detect a broken package identity or PSR-4/public-class mismatch.
- Adjacent boundary: invalid credentials still fail, using the Qubrics message while preserving all accepted prefix values.
- No existing test removed, skipped or weakened.

First green attempt used `composer dump-autoload`; Composer retained the old root-package name in `vendor/composer/installed.php`. `composer install` refreshed that metadata. The temporary lock generated before the rename was moved into the task artifacts because this library does not track a lockfile. Parent review corrected the test's Guzzle middleware composition before execution reached that line.

## Validation

Local PHP 8.4.10 and Composer 2.8.9; CI remains PHP 8.2.

| Command | Result |
| --- | --- |
| `composer install --no-interaction --prefer-dist` | Exit 0, real dependencies and autoload generated |
| `composer validate --strict` | Exit 0, `./composer.json is valid` |
| `find src tests -type f -name '*.php' -print0 \| xargs -0 -n 1 php -l` | Exit 0, all nine PHP files pass |
| `php tests/public-contract.php` | Exit 0, public contract passed |
| `git diff --check` | Exit 0 |

CI now installs Composer dependencies and runs the public contract after metadata and syntax validation. No new test dependency added.

Logs: `/Users/rj/Desktop/GIT-REPOS/sendmux-php-core/.claude/artifacts/rebrand-qubrics-php4/`:

- `core-composer-install-red.log`, `core-composer-install-green.log`.
- `core-public-contract-red.log`, `core-public-contract-stale-metadata.log`, `core-public-contract-green.log`.
- `core-composer-autoload-green.log`, `core-composer-validate.log`, `core-php-syntax.log`.
- `core-generated-composer.lock` records the local dependency resolution and is outside the change-set.

## Handback

Correctness: Package metadata, PSR-4 namespace, every exception reference and customer-facing runtime string renamed together; dependency and runtime control flow unchanged.

Tests: Added public Composer contract, seen red then green; removed none.

Journeys: No browser surface. Local installed-package boundary exercised without network requests. Public Packagist installation not performed.

Copy polish: Independent fresh-reader critique passed for README, Composer description and unreleased changelog. One humanize pass completed on changed prose, followed by a passing protected-set check. Migration instructions explicitly replace the Composer package before updating helper imports and exception handling. Logs: `core-copy-polish.md` and `core-copy-protected-set.log` in the task artifacts.

Status: ready for commit, not merged; parent owns cross-repository review, commits and all later shipping gates. One slice fixed-local; zero shipped.

Torn down: No background processes, containers or browser sessions created. Worktree retained for parent review.

Parked: Public package publication and old-package abandonment remain outside this lane.
