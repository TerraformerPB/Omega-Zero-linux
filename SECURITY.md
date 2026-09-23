# Security Policy

## Scope

Security issues affecting Omega-Zero-linux, IPOS, the security core, build system, update system, gateway, policy engine or published artifacts are in scope.

## Reporting

Do not publish an unpatched critical vulnerability as a public issue.

Until a dedicated security-reporting channel is established, use the repository's private security-reporting mechanism where available.

A useful report should contain:

- affected component
- affected version/commit
- attack preconditions
- reproducible steps or proof of concept
- security impact
- suggested mitigation, if known

Do not include real credentials, private keys, personal data or production secrets.

## Security principles

Security fixes take priority over feature development when a vulnerability can affect the trusted computing base, device identity, boot chain, update mechanism, policy enforcement or credential isolation.

Every security-sensitive change should receive independent review.

## Disclosure

The project should establish a coordinated disclosure process before production release.
