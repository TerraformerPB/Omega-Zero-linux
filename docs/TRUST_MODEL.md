# IPOS Trust Model

## 1. Trust hierarchy

Trust must flow upward only from stronger roots to weaker layers.

~~~text
Immutable Hardware Root
        ↓
Boot Verification
        ↓
Verified System
        ↓
Security Enforcement
        ↓
Identity
        ↓
Policy
        ↓
Application
        ↓
Service
~~~

No lower layer may be trusted merely because a higher layer says it is trusted.

## 2. Four identities

### Device Identity
Hardware-bound and used to establish device authenticity.

### User Identity
Represents the authenticated human/user account.

### Application Identity
Represents the isolated application context.

### Service Identity
Represents an individual backend service.

These identities are never interchangeable.

## 3. Access decision

A service request should conceptually evaluate:

~~~text
device_identity_valid
AND device_attestation_acceptable
AND user_session_valid
AND application_identity_valid
AND service_identity_valid
AND requested_action_allowed
AND data_flow_allowed
AND network_policy_allowed
~~~ 

Only then may access be granted.

## 4. Continuous trust

A successful login is not permanent trust.

The system should support:
- session expiry
- re-attestation
- credential rotation
- policy changes
- immediate revocation
- device quarantine

## 5. Fail closed

For security-critical decisions:

~~~text
UNKNOWN = DENY
INVALID = DENY
MISSING = DENY
EXPIRED = DENY
REVOKED = DENY
~~~

Availability exceptions must be explicit and documented.

## 6. Privacy-preserving identity

Device identity must not automatically become a universal tracking identifier.

Where technically possible:
- use scoped identifiers
- minimize attestation data
- avoid unnecessary hardware identifiers
- use privacy-preserving credentials
- separate operational identity from personal identity

## 7. Attestation

The server should validate:
- device key
- boot state
- locked bootloader state
- verified boot measurements/digests where available
- OS version
- security patch level
- approved policy version
- nonce/challenge freshness

Attestation evidence is an input to authorization, not a permanent certificate of safety.

## 8. Service authorization

A service must never assume:

~~~text
same network = trusted
same user = trusted
same device = trusted
same company = trusted
~~~

Authorization is explicit per resource/action.

## 9. Compromise containment

The architecture is successful when one compromised component does not automatically provide:
- all device credentials
- all applications
- all cloud services
- all user data
- update signing authority

Blast radius is a first-class security metric.
