# Changelog

## Unreleased

### Changed

- Move the package from `sendmux/core` to `qubrics/core`, with helpers in `Qubrics\Core`. Update your imports and replace `SendmuxApiError` with `QubricsApiError`.
- Update SDK documentation links to qubrics.com.
- Keep `smx_root_`, `smx_mbx_` and `smx_agent_` credential prefixes, HTTP headers, response fields and retry behaviour unchanged.

## 2.1.1 (2026-09-16)

### Security

- Require Guzzle `^7.15.5` and its maintained PSR7 dependency for HTTP security fixes. If you use a custom persistent cookie jar, follow the [upgrade warning](README.md#installation) before updating.

## 2.1.0 (2026-09-11)

- Add REST OAuth access tokens and callable token providers with validation before authenticated requests.
- Respect explicit non-retryable errors and elapsed retry budgets while preserving the final response and retry metadata.

## 2.0.0 (2026-09-08)

- Add a Sending credential surface and support scoped agent tokens for Mailbox and owner-approved Sending access.
