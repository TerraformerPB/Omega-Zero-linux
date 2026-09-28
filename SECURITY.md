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

## Coordinated vulnerability disclosure

The process follows ISO/IEC 29147 / 30111 and the reporting obligations of the EU Cyber Resilience Act (Regulation (EU) 2024/2847). The flow is documented in `docs/FLOWCHARTS.md` (section 7).

| Step | Target time |
|---|---|
| Acknowledge receipt to the reporter | 3 working days |
| Initial triage and severity assessment | 7 days |
| Actively exploited vulnerability / severe incident: CRA early warning to CSIRT (BSI) via the ENISA single reporting platform | ≤ 24 hours after becoming aware |
| CRA vulnerability / incident notification | ≤ 72 hours |
| CRA final report | ≤ 14 days after a fix is available (vulnerabilities), ≤ 1 month (severe incidents) |
| Personal data breach: notification to the supervisory authority by the controller (GDPR Art. 33) | ≤ 72 hours |
| Default embargo before public disclosure | 90 days, shorter if actively exploited |

The CRA obligations apply once IPOS or Omega-Zero components are made available in the course of a commercial activity. The project follows the process voluntarily before that point.

Reporters acting in good faith within this policy will not be pursued by the project. This does not affect rights of third parties.

## Supported versions

There is no supported production release yet. A support period (CRA: generally at least five years) will be defined before the first production release.

## Data protection in reports

Reports are processed only for handling the vulnerability (GDPR Art. 6(1)(f)). Reporter contact data is deleted after closure of the case unless the reporter asks to be credited.
