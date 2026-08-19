# Week 2 Notes: Security+ SY0-701 (Sessions 1 & 2)

**Dates covered:** August 10 and August 12, 2026
**Source:** Professor Messer's SY0-701 course, Section 1 (General Security Concepts)

## Security Control Categories & Types

**Four Categories**
- Technical: implemented via systems (firewalls, antivirus, encryption)
- Managerial: administrative and policy level (security policies, risk assessments)
- Operational: day to day human processes (security awareness training)
- Physical: tangible barriers (fences, badge readers, guards)

**Six Types (mnemonic: PDDCC-D)**
- Preventive: blocks access outright before it happens
- Deterrent: discourages an attempt without directly blocking it
- Detective: identifies that something happened, does not prevent it
- Corrective: fixes or reverses damage after an event is detected
- Compensating: substitute control used when the ideal control is not available
- Directive: instructs people what they must do (policies, procedures, signage)

A single control can span more than one category. For example, a badge reader is Technical but also serves a Physical purpose.

## CIA Triad (AIC Triad)

- **Confidentiality:** preventing unauthorized disclosure. Achieved via encryption, access controls, MFA.
- **Integrity:** ensuring data has not been modified in storage or transit. Achieved via hashing, digital signatures, certificates.
- **Availability:** ensuring authorized users can access systems when needed. Achieved via redundancy, fault tolerance, patching.

Redundancy is having backup or duplicate components (the tool). Fault tolerance is the system's ability to keep running through a failure (the outcome).

## Non-Repudiation

The guarantee that someone cannot deny having sent a message or performed an action. Combines:
- Proof of integrity: the data was not altered (hash comparison)
- Proof of origin: the data really came from the claimed sender (private key encryption or digital signature)

A hash alone only proves integrity. Non-repudiation requires the added layer of a digital signature, since only the sender's private key could have produced it.

## Hashing

Hashing works like blending ingredients into a smoothie. Same ingredients blended the same way produce the same smoothie every time: same input, same output.

Change one ingredient, even slightly, and the result is a completely different smoothie, not a slightly different one. This is the **avalanche effect**: a small change in input produces a drastically different hash output, by design, so tampering can never hide.

Practical example: an 8.1 MB encyclopedia file was hashed. Changing a single character anywhere in the file, invisible to the human eye, produced a completely different hash. Hashing acts as a tripwire. It tells you THAT something changed, not WHAT changed or where. A separate diff tool is needed to locate the actual difference.

## Digital Signatures

A digital signature is a hash encrypted with the sender's private key. It provides both integrity and proof of origin.

**Alice and Bob walkthrough:**
1. Alice writes a message.
2. Alice hashes the message.
3. Alice encrypts that hash using her private key. This encrypted hash is the digital signature.
4. Alice sends Bob the plain text message plus the digital signature.
5. Bob decrypts the signature using Alice's public key. Successful decryption proves it was created with Alice's private key: proof of origin.
6. Bob independently hashes the message he received.
7. Bob compares his hash to the decrypted hash. A match means the message was not altered (integrity) and it really came from Alice (non-repudiation).

Unlocking the signature with the public key proves WHO sent it. Comparing the two hashes proves nothing was changed. Two separate checks combined into one process.

## Certificates

A certificate works like a passport for a website or device. Anyone can claim to be "amazon.com," just as anyone can claim to be a person. A certificate is issued by a trusted Certificate Authority (CA) that vouches a specific public key really belongs to the claimed identity.

## AAA Framework

- **Identification:** the claim of who you are (a username). Not yet proven.
- **Authentication:** proving that claim is true, via password or other factors.
- **Authorization:** once verified, determines what resources or actions you are permitted to access.
- **Accounting:** the record keeping: login and logout times, resource usage, actions taken. Used for tracing activity after the fact.

## Certificate-Based Device Authentication

Devices cannot type a password, so they carry a digitally signed certificate instead, issued by the organization's own trusted CA. When the device connects (for example, to a VPN), the system verifies the CA's digital signature to confirm the device is trusted, without any human credential entry.

## Authorization Models: RBAC

Direct user to resource permissions do not scale. Assigning individual rights to hundreds of employees is unmanageable and hard to audit.

**Role-Based Access Control (RBAC)** groups users into roles and assigns permissions to the role rather than the individual. Example: a "Shipping and Receiving" role is granted rights to create shipping labels, track shipments, and view monthly reports. Any employee added to that role instantly inherits those permissions.

## Gap Analysis

Compares where an organization currently stands against a target baseline, and defines the path needed to close that distance.

**Choosing a framework:** organizations typically measure against an established standard such as NIST SP 800-171 Rev. 2 (protecting controlled unclassified information in non-federal systems) or ISO/IEC 27001 (information security management systems). Organizations may also define their own baseline.

**Evaluating people and processes:** assessing staff experience and training levels, reviewing existing security policies, and examining current IT systems to identify weaknesses.

**Analysis and reporting:** the final output includes a baseline of objectives, a clear view of the current state, and a defined path to reach the goal, typically involving time, cost, and change management.

**Red, Yellow, Green scale:**
- Red: furthest from meeting the baseline, most work required
- Yellow: midpoint progress toward the baseline
- Green: closest to, or already meeting, the baseline

## Zero Trust

Traditional network security is often described as "castle and moat": strong perimeter defenses, but relatively open and trusted movement once inside. Zero Trust replaces this with a model where every device, process, and person must be continuously verified. Nothing is inherently trusted, even traffic already inside the network.

**Planes of operation:**
- Data plane: handles actual traffic (processing, forwarding, trunking, encrypting, NAT)
- Control plane: makes the decisions that govern the data plane (policy, routing tables, session tables)

**Adaptive identity:** authentication that considers contextual risk indicators beyond credentials, such as physical location, connection type, IP address, and relationship to the organization. Can require stronger authentication when risk signals are elevated.

**Scope reduction and policy-driven access control:** scope reduction limits the number of possible entry points into a system. Policy-driven access control combines adaptive identity with a defined rule set to determine access.

**Security zones:** broad categories (trusted/untrusted, internal/external network, VPN segments, departmental zones) provide a foundational classification of where traffic is coming from and going to.

**The four core components:**
- **PE (Policy Engine):** evaluates each access request against policy and contextual information, issues the decision to allow, deny, or revoke.
- **PA (Policy Administrator):** takes the Policy Engine's decision and communicates it to the enforcement point, issuing access tokens or credentials.
- **PDP (Policy Decision Point):** the Policy Engine and Policy Administrator working together form the PDP as a whole. This is not a separate third component.
- **PEP (Policy Enforcement Point):** sits in the data plane, in the actual path of traffic. Does not make decisions itself, enforces whatever the PDP instructs.

**Request flow:** a subject (user or system) attempts to reach an enterprise resource. The request passes through the PEP first. The PEP queries the PDP for a decision. The Policy Engine evaluates the request, the Policy Administrator relays the resulting instruction back to the PEP. The PEP then allows or blocks access accordingly. This is a continuous two-way exchange for every access attempt, not a one-time check.

## Terminology Log

| Term | Definition |
|---|---|
| Hash / fingerprint / message digest | Fixed-length output representing data |
| Avalanche effect | Small input change causes drastically different hash output |
| Digital signature | Encrypted hash, proves integrity and origin |
| Certificate | Trusted verification of identity tied to a public key |
| Non-repudiation | Cannot deny having sent or signed something |
| MFA | Multi-Factor Authentication, requires 2+ proof types |
| Jump box | A hardened intermediary server used to control and log access to sensitive systems |
| Redundancy | Backup or duplicate components for availability |
| Fault tolerance | System's ability to keep running through failure |
| AAA | Authentication, Authorization, Accounting |
| CA | Certificate Authority, trusted issuer of digital certificates |
| RBAC | Role-Based Access Control |
| Gap Analysis | Comparison of current state vs. target baseline, with a plan to close the difference |
| Zero Trust | Security model where nothing is inherently trusted, all access is continuously verified |
| Control plane | The decision-making layer (policy, routing rules) |
| Data plane | The traffic-handling layer (forwarding, encrypting, processing) |
| Adaptive identity | Authentication that factors in contextual risk signals |
| PE | Policy Engine, evaluates access requests and issues decisions |
| PA | Policy Administrator, relays PE's decision and issues credentials or tokens |
| PDP | Policy Decision Point, the combined PE and PA decision-making unit |
| PEP | Policy Enforcement Point, enforces the PDP's decision at the point of access |

## Next Session

Continue Section 1 (General Security Concepts), picking up from where Zero Trust left off.
