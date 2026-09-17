# Web3 Cryptographic Key Management Policy

## 1. Purpose

This Policy establishes mandatory organizational requirements for managing cryptographic keys and associated keying material used by the Company.

The Company shall preserve confidentiality, integrity, availability, authenticity, authorized use, and accountability.

It shall prevent unauthorized signing, asset movement, protocol control, service compromise, validator penalties, and loss of recoverability.

The Company shall maintain a documented Cryptographic Key Management System (CKMS).

This Policy defines mandatory outcomes and governance. Processes define lifecycle flows, Standards define technical criteria, and Procedures and SOPs define approved actions and evidence.

## 2. Scope

This Policy applies to Company personnel, contractors, third parties, systems, services, environments, and activities that create, receive, derive, store, use, administer, recover, rotate, revoke, or destroy Company keying material.

In-scope material includes private and public keys, symmetric keys, certificates, key-encryption keys, seeds, mnemonics, HD wallet roots, derived private keys, and encrypted keystores.

It also includes threshold or MPC shares, recovery material, and remote-signer credentials.

It also includes validator signing keys, withdrawal credentials, transaction and treasury keys, governance and smart-contract authorities, and service credentials used to access key-management services.

Keying material managed by an approved external provider is in scope.

This Policy applies to production and non-production environments. Production keying material shall not be used outside production unless a time-bound, documented exception establishes a legitimate need and equivalent safeguards.

## 3. Definitions

**Associated keying material** means material that enables generation, derivation, protection, recovery, access, or use of a cryptographic key.

It includes seeds, mnemonics, shares, encrypted keystores, recovery credentials, and protected backups.

**Authorized signing context** means the immutable, approved set of data and constraints for a signature. It includes the final signing payload or digest, network or chain identifier, signer, purpose, authorization, and expiry.

**CKMS** means the people, processes, systems, facilities, controls, and evidence used to manage keys and associated keying material.

**Critical keying material** means keying material classified as Critical under Section 7.

**Cryptographic Baseline** means the approved Standard that defines permitted algorithms, parameters, operations, security strength, cryptoperiods, transition status, and implementation restrictions.

**Key Owner** means the accountable business or security function that authorizes a keying-material set's purpose, classification, lifecycle decisions, recovery, and disposition.

**Keying material** means a cryptographic key and its associated keying material.

**Protection boundary** means the approved logical, physical, or cryptographic environment in which keying material is generated, stored, processed, recovered, or used.

**Security Approval Authority** means the formally appointed, independent role or body authorized to approve Critical-key requirements, material exceptions, and material changes under this Policy.

**Signing authority** means a key, signing service, quorum, or other cryptographic authority able to authorize a signature or protocol action.

**Transaction Initiator**, **Transaction Approver**, and **Signer** mean the separately attributable roles that create a request, authorize or reject it, and produce a signature only after required controls succeed.

**Wallet** means a logical arrangement of keys, addresses, derivation data, authorities, and controls used to manage blockchain assets or authority.

**Web3 keying material** includes blockchain account keys, validator signing keys, withdrawal credentials, treasury keys, contract deployer keys, smart-contract upgrade or governance authority, and related recovery material.

Terms not defined here shall be defined in the applicable Standard where needed for consistent implementation.

## 4. Governance and Control Architecture

The Company shall operate a documented CKMS proportionate to its services, risk, and dependencies.

The CKMS shall include this Policy, key-management Processes, technical Standards, Procedures and SOPs, an authoritative inventory, exception management, audit and incident-response arrangements, and a CKMS design specification.

This Policy is authoritative. Subordinate documents may impose stricter requirements but shall not weaken this Policy.

The Company shall maintain the following document hierarchy:

- **Policy** defines mandatory organizational requirements, governance, and accountability.
- **Process** defines end-to-end flow, state transitions, and control gates.
- **Standard** defines mandatory technical, cryptographic, security, and application-specific requirements.
- **Procedure / SOP** defines approved human or automated actions and required evidence.

For each material Policy requirement, the Company shall maintain traceability to its implementing document, accountable owner, evidence, control source or rationale, known gaps, and approved exceptions.

Executive Management shall approve this Policy and provide implementation resources. The Security Approval Authority shall own this Policy and approve material changes before production use.

Material changes include changes to a Critical key, key-management service, authority model, protection boundary, cryptographic baseline, recovery design, custody arrangement, or exception.

## 5. Roles and Responsibilities

The Company shall assign clear ownership and accountability for each key-management activity. Critical-key activities shall use separation of duties, dual control, or multi-party authorization proportionate to risk.

Executive Management shall provide governance and accept residual risk only when it exceeds delegated authority.

The Security Approval Authority shall maintain Policy governance, approve Critical-key requirements and exceptions, oversee material risk assessments and compromise response, and ensure independent review of findings.

Each keying-material set shall have a Key Owner.

The Key Owner shall authorize its purpose, classification, use, lifecycle decisions, recovery, and disposition. The Key Owner shall identify dependencies and participate in access reviews and compromise response.

The Security Architecture or designated Cryptographic Authority shall own the Cryptographic Baseline. It shall define requirements for key generation, derivation, import, protection, recovery, transition, use, and destruction.

Platform, key-management-service, node-operations, and application owners shall implement approved controls. They shall maintain attributable access and evidence and support lifecycle, audit, recovery, and incident activities.

Key Custodians and Administrators shall perform only approved, attributable actions. They shall not approve their own Critical-key generation, recovery, access elevation, rotation, destruction, or use.

For a Critical transaction or protocol-control action, the initiator, approver, signer, and Critical-key administrator shall be separated by independently controlled identities and approval paths.

One natural person shall not hold more than one of these roles for the same action.

Critical multi-party approval shall not rely solely on identities, devices, providers, administrative planes, or break-glass paths controlled by one person or one trust domain.

The applicable Standard shall define and periodically test the required independence.

Incident Response shall coordinate compromise response. Internal Audit or an independent reviewer shall assess compliance and remediation.

Third parties supporting Company keying material shall undergo due diligence.

Contracts shall require security obligations, attributable access, material-change and incident notification, assurance, recovery support, data return or destruction, and exit planning.

## 6. Key Inventory

The Company shall maintain an accurate, complete, authoritative inventory of in-scope keying material. An inventory record shall be created before production activation or, for discovered assets, as soon as practicable after discovery.

The inventory shall not contain plaintext secret material. It shall be integrity-protected and accessible to authorized personnel during incidents and recovery.

Each record shall include, at minimum:

- a unique inventory identifier, owner, classification, handling attribute, and authority type;
- purpose, authorized operations, environment, lifecycle state, and dependency relationships;
- algorithm, key or domain parameters, security strength, cryptoperiod, and activation, expiry, rotation, and disposition dates;
- generation, import, derivation, and parent-child provenance, without exposing a secret;
- protection boundary, protection or wrapping method, access roles, custody or quorum design, and recovery arrangement;
- approvals, exceptions, compromise or revocation history, evidence references, and required retention;
- for Web3 material, the network, protocol role, public identifier, wallet or authority relationship, and relevant signing or recovery dependency.

The inventory shall represent aggregate authority. It shall identify relationships among seeds, HD roots, derived keys, addresses, controlled assets, validator identities, contract roles, recovery arrangements, and signing authorities.

The Key Owner shall keep inventory information current. The Company shall reconcile inventory records against deployed services, public identifiers, and provider records at a risk-based frequency defined in the applicable Standard.

Unowned, undocumented, or unclassifiable keying material shall be restricted, assessed, and either brought into compliance or retired.

## 7. Key Classification and Handling

The Company shall assign an impact classification to each keying-material set.

Classification shall consider compromise, misuse, modification, unavailability, loss of recoverability, asset exposure, protocol control, validator risk, dependency breadth, and legal or contractual impact.

The Company shall use the following impact classifications:

| Classification | Policy treatment |
|---|---|
| **Critical** | Compromise, misuse, or loss could materially affect assets, protocol control, consensus participation, recovery, service continuity, or organizational trust. |
| **High** | Material protects sensitive services, privileged authentication, high-impact signing, or sensitive data. |
| **Standard** | Material supports ordinary internal authentication, encryption, or integrity services. |

Public verification material is a handling attribute, not an impact classification.

A public key, address, certificate, trust anchor, or verification record may still be Critical or High when its integrity, binding, or authority relationship is material.

Classification shall determine protection, authorization, separation of duties, monitoring, recovery, review cadence, cryptoperiod, and audit depth.

Every Critical keying-material set shall have named ownership, individually attributable access, independent approval for material lifecycle changes, protected audit logging, and documented recovery.

It shall also have periodic access review and documented compromise response.

The Company shall distinguish impact classification from authority type. Authority types include validator, asset-control, protocol-control, application-security, infrastructure-security, and recovery authority.

## 8. Key Lifecycle Management

The Company shall manage each keying-material set throughout its lifecycle.

The lifecycle includes identification, planning, classification, cryptographic selection, generation, derivation, import or receipt, registration, provisioning, activation, and authorized use.

It also includes backup and recovery, rotation and migration, suspension, deactivation, revocation, archival, and destruction.

Before production activation, the Company shall approve the owner, purpose, classification, authorized operations, cryptographic selection, protection boundary, access model, dependencies, and recovery method.

It shall also approve the lifecycle and transition plan, cryptoperiod, and disposition requirements.

Secret material shall be generated or derived only by an approved method within an approved protection boundary.

The method shall use an approved CSPRNG or RBG with adequate security strength and shall protect entropy inputs and derivation material.

Imported keying material shall have documented provenance, integrity, cryptographic suitability, and transfer protections before production activation.

HD roots, mnemonics, seeds, and derived keys shall follow an approved derivation and generation Standard.

Lifecycle changes shall be authorized, recorded in the inventory, supported by proportionate evidence, and preserve required continuity, historical verification, decryption, recovery, and retention capabilities.

No key shall be used outside its authorized purpose, environment, lifecycle state, operation, or cryptoperiod.

A deactivated or archived key shall not provide new cryptographic protection or authority unless reauthorized through documented risk assessment and approval.

Confirmed compromised secret material shall not be returned to active use. It may be retained only for an approved, controlled historical-verification, decryption, evidence, or transition purpose.

## 9. Key Protection and Cryptographic Selection

The Company shall protect secret keying material against unauthorized disclosure, modification, substitution, destruction, and use throughout generation, storage, processing, backup, recovery, transfer, and retirement.

Secret material shall remain within an approved protection boundary.

It shall be subject to least privilege, attributable identity, strong authentication for privileged use, access review, and protection against unauthorized export, copying, and persistence.

Secret material shall not be stored in source code, container images, build artifacts, tickets, chat systems, ordinary collaboration storage, logs, or other unapproved locations.

Critical secret material, including Critical seeds, HD roots, private keys, and reconstructable shares, shall be non-exportable by default. It shall be protected by approved hardware, threshold, or equivalent mechanisms.

An exception involving plaintext Critical secret material shall be time-bound and approved under Section 16.

It shall use a documented dual-control ceremony, approved encrypted transport, ephemeral handling, required evidence, and post-ceremony rotation or equivalent risk treatment.

The Company shall protect public keys, certificates, addresses, trust anchors, and verification metadata from unauthorized modification, substitution, misbinding, and untrusted publication.

The Company shall use separate keys for separate security purposes unless an approved cryptographic design permits otherwise.

The Cryptographic Baseline shall define the approved algorithm, parameters, operation, security strength, cryptoperiod, status, permitted use, owner, and migration treatment for each cryptographic profile.

The Baseline shall distinguish acceptable, deprecated, legacy-use, and disallowed mechanisms.

Legacy-use mechanisms shall not create new cryptographic protection, signatures, encryption, or key wrapping.

Deprecated mechanisms shall have a time-bound risk acceptance and transition plan. Disallowed mechanisms shall not be used.

Protocol-mandated algorithms or formats shall be documented as interoperability constraints with risk, compensating controls, and feasible migration or exit treatment.

The use of an HSM, MPC, multisig, hardware wallet, cold storage, remote signer, or similar technology shall not by itself satisfy authorization or governance requirements.

## 10. Key Access, Usage, and Transaction Authorization

The Company shall authorize key access and use by purpose, classification, authority type, role, environment, lifecycle state, and approved operation.

Access shall be individually or workload-attributable, least privileged, periodically reviewed, logged, and promptly removed when no longer required.

Key use shall be limited to approved cryptographic operations. A key shall not be repurposed for unrelated signing, encryption, authentication, derivation, recovery, or protocol control without documented risk assessment and approval.

Critical transactions and protocol-control actions shall require an approved authorization decision before signing.

A signer shall fail closed and shall not generate a signature when required authorization or transaction-policy controls do not succeed.

The authorization decision shall be bound to the exact final signing payload or digest.

It shall be bound to the applicable network or chain identifier, signing domain, source authority, sender, nonce or equivalent replay control, recipient or counterparty, asset, value, contract and function restrictions, and expiry.

The applicable Standard shall define fee limits, frequency or velocity limits, transaction simulation or semantic validation where feasible, typed-data validation, and signer-side controls.

Blind or opaque signing of Critical transactions is prohibited unless a documented exception defines compensating controls.

For each Critical signing authority, the Company shall define approval thresholds, permitted network, source authority, permitted destination, permitted asset, value limits, and contract and function restrictions.

It shall also define emergency authority and monitoring requirements.

Remote signers shall use mutually authenticated, attributable workloads, network isolation, per-key authorization, and signer-side validation. A remote signer shall not rely solely on an upstream client for Critical policy enforcement.

## 11. Backup and Recovery

The Company shall maintain documented, tested, classification-appropriate backup and recovery arrangements.

These arrangements apply to material whose loss would impair required operations, security services, asset control, protocol participation, or required records.

Backup and recovery material shall be protected at least as strongly as the authority it enables. It shall not create untracked, persistent, or routine operational copies of secret material.

Recovery shall require documented authorization, integrity validation, attributable access, and evidence.

For Critical material, recovery shall require Key Owner authorization and independent approval, except under the narrowly defined emergency authority in Section 16.

Critical recovery design shall avoid a single person, identity system, administrative plane, provider, or physical event being able to reconstruct the material or bypass the required approval path.

Recovery testing shall occur at a risk-based frequency defined in the applicable Standard. Tests shall demonstrate safe recovery without unnecessary exposure and shall produce evidence of the result, gaps, and remediation.

The Backup and Recovery Standard shall define recovery objectives, quorum, geographic or custody separation, backup integrity, emergency recovery, physical handling, transportation, and implementation methods.

## 12. Key Compromise Management

Suspected or confirmed compromise of keying material shall be treated as a security incident.

Compromise includes unauthorized disclosure, acquisition, access, copying, export, derivation, use, modification, loss, destruction, loss of control, or use of an unapproved protection boundary or signing path.

For Critical material, a suspected compromise shall trigger immediate restriction or suspension of use unless Incident Response documents why suspension would create greater risk and specifies compensating controls.

The Company shall detect, contain, assess, and record the event. It shall preserve evidence and identify dependencies and related keying material.

It shall determine required replacement, rotation, migration, revocation, or notification and complete post-incident remediation.

The Critical-key compromise process shall identify notification recipients and content, assigned responders, and the re-key or authority-transfer method.

It shall identify affected signatures, transactions, data, and protocol authorities, completion criteria, and post-incident monitoring.

Compromise response for Critical material shall prioritize prevention of unauthorized signing, asset loss, protocol-control loss, validator penalties, continued compromise, and loss of forensic evidence.

The Company shall exercise Critical compromise and recovery plans at the frequency defined by the applicable Standard and retain after-action evidence.

## 13. Key Rotation and Migration

The Company shall rotate, renew, replace, or migrate keying material before continued use no longer meets approved cryptographic, operational, or risk requirements.

Rotation or migration shall be triggered by cryptoperiod expiry, cryptographic transition, suspected compromise, or material change in authority or exposure.

It shall also be triggered by a protection-boundary or provider change, recovery-design inadequacy, certificate or trust transition, or protocol lifecycle requirement.

The Cryptographic Baseline shall define maximum cryptoperiods and, where applicable, originator- and recipient-usage periods by key type, algorithm, operation, and authority type.

It shall identify the responsible owner, advance notification, and required rotation evidence.

The Company shall authorize and validate successor keying material and identify dependencies and transition requirements.

It shall update access, inventory, monitoring, and recovery arrangements. It shall retire predecessor material only after required continuity and retention obligations are resolved.

Applicable Processes and Standards shall define sequencing, compatibility handling, validator migration, authority transfer, asset migration, client checks, rollback, and evidence requirements.

## 14. Key Revocation, Archival, and Destruction

The Company shall suspend, deactivate, revoke, archive, or destroy keying material when purpose ends, authorization is withdrawn, or compromise occurs.

It shall also do so when a successor is activated, cryptoperiod or retention requirements require transition, or risk otherwise requires disposition.

For certificates and other relying-party trust material, revocation shall include prompt, attributable notification to affected relying parties or services.

The notification shall identify the material, effective time, and reason without exposing secret material.

For an on-chain private key or authority, revocation shall not be treated as sufficient by itself.

The Company shall disable the signing path and create and validate successor authority.

It shall remove or replace on-chain roles, quorum members, withdrawal authorities, delegations, and allowlists. It shall reconcile the resulting on-chain state.

The Company shall identify required asset movement, authority transfer, validator exit or migration, counterparty notification, monitoring of predecessor authority, and pre-cutover transaction impact before final disposal.

Before disposition, the Company shall assess dependencies, backups, recovery material, certificates, trust relationships, retention needs, historical verification, legal or contractual obligations, and protocol-specific requirements.

Destruction shall use an approved method intended to render secret material and recovery copies infeasible to recover.

Evidence shall identify the inventory record, responsible roles, approvals, method, date, verification result, retained metadata, and residual risk.

## 15. Logging, Monitoring, Audit, and Compliance

The Company shall generate, protect, retain, and review security-relevant key-management records sufficient to establish accountability, detect unauthorized activity, support incident response, and demonstrate compliance.

Records shall cover lifecycle events, access and administrative activity, authorization and key use, backup and recovery, configuration changes, compromise response, exceptions, and destruction.

Records shall identify time, inventory identifier, attributable actor or workload, action, outcome, authorization context, and evidence reference.

Critical signing records shall identify the approved signing context or a non-secret, verifiable reference to it.

Records shall not expose plaintext secret material. Logging and monitoring shall be protected against unauthorized access, alteration, deletion, suppression, and disclosure.

Critical-key logs shall use protected time synchronization and a tamper-evident or independently retained record. Material logging or monitoring failures shall be detected and escalated according to risk.

The Company shall conduct risk-based audits and independent reviews of inventory, classification, access, lifecycle evidence, recovery readiness, protection, baseline adherence, exceptions, third-party arrangements, and remediation.

Critical-key domains shall receive heightened independent review.

## 16. Exception Management and Emergency Authority

An exception is permitted only when a Policy requirement cannot be met, the need is legitimate, and the resulting risk is explicitly assessed, accepted, and managed.

Each exception shall identify the affected requirement and material; accountable owner; justification; classification and authority type; risk; and compensating controls.

It shall also identify monitoring, remediation owner and plan, approvals, review frequency, and expiry date.

Exceptions affecting Critical material, protocol-control authority, validator authority, recovery material, plaintext secret handling, or deprecated cryptography require Key Owner and Security Approval Authority approval.

Executive risk acceptance is required when delegated authority is exceeded.

An exception shall not authorize unbounded plaintext secret handling, untraceable privileged access, uncontrolled Critical-key copies, or indefinite deprecated cryptography.

It shall not authorize known unsafe validator operation or bypass of required transaction authorization.

Emergency signing or key-management action may occur only under a predefined emergency authority.

It shall define the trigger, authorized identities, permitted actions, transaction, asset, value, destination, duration, automatic expiry, and required audit record.

Emergency action shall be recorded contemporaneously.

It shall receive independent review within one business day.

It shall reconcile affected keys, transactions, assets, and protocol authorities within the period defined by the applicable Standard.

## 17. Web3-Specific Key Management Principles

The following controls address Web3 and protocol-specific risk. They are not represented as direct NIST requirements unless separately mapped as such.

### 17.1 Authority Separation

Validator signing, withdrawal, treasury, transaction, contract deployment, contract upgrade, governance, and recovery authority shall be separately inventoried, classified, and governed according to distinct purpose and impact.

A shared design requires documented risk assessment and approval. Shared cryptographic material shall not silently expand authorization beyond its documented aggregate authority.

### 17.2 Aggregate Authority and Wallet Governance

The Company shall assess aggregate authority enabled by related material rather than evaluating each cryptographic object in isolation.

This assessment shall cover seeds, mnemonics, HD roots, derived keys, wallets, addresses, threshold or MPC shares, recovery arrangements, controlled assets, and protocol authorities.

### 17.3 Validator Safety

Validator signing authority shall be bound to its intended network, validator identity, and signing domain. The Company shall prevent unsafe or conflicting signing during normal operation, failover, recovery, and migration.

Where a protocol penalizes conflicting signatures, the Company shall permit only one active signing authority for a validator and network unless an approved design provides equivalent exactly-once signing safety.

Slashing-protection or equivalent safety state shall be durable, integrity-protected, and monotonic. A replacement signer shall not activate until the required safety state has been verified and transferred.

If signer state, network identity, or prior signing safety is uncertain, the Company shall fail closed and escalate the event. The Validator Key Management Standard shall define protocol-specific client and operational requirements.

### 17.4 Protocol and Asset-Control Authority

Treasury, withdrawal, governance, contract deployment, contract upgrade, and comparable protocol-control authorities shall use transaction authorization controls proportionate to classification and authority type.

The Company shall validate the resulting asset, role, or control destination before completing a material authority transfer or migration.

### 17.5 Threshold, MPC, Multisig, and Remote Signing

Threshold, MPC, multisig, and remote-signing designs shall document custody, participants, quorum, independent trust domains, authorization, recovery, replacement, compromise, monitoring, lifecycle, and exit arrangements.

These mechanisms shall not be treated as interchangeable or as substitutes for accountable governance and transaction authorization.

A shared root administrator, endpoint, or recovery path shall not defeat the intended quorum without explicit risk treatment.

### 17.6 Protocol-Mandated Cryptography and Lifecycle

Protocol-mandated algorithms, key formats, signing conventions, and authority transitions shall be documented as interoperability constraints.

The Company shall assess their security and lifecycle implications and maintain feasible protocol-specific migration, exit, authority-transfer, or compensating-control arrangements.

## 18. References and Document Maintenance

This Policy is informed by NIST SP 800-57 Part 1 Rev. 5, NIST SP 800-57 Part 2 Rev. 1, NIST SP 800-57 Part 3 Rev. 1, NIST SP 800-130, and NIST SP 800-131A Rev. 2.

The Company shall use SP 800-57 Part 2 for key-management policy and organizational governance. It shall use Part 1 for general management, protection, lifecycle, inventory, and cryptoperiods.

It shall use SP 800-130 for CKMS design, Part 3 for application guidance, and SP 800-131A for cryptographic transition.

The Company shall maintain a mapping that identifies every material control as NIST Direct, NIST-derived, Web3-specific, Organization-specific, Protocol-mandated, or No clear basis.

The mapping shall identify the source and revision, implementing document, owner, evidence, gaps, exceptions, compensating controls, and remediation.

The Security Approval Authority shall review this Policy and its mapping at least annually and after a material incident.

It shall also review the Policy when relevant cryptographic publications, algorithms, protocol requirements, custody model, architecture, or operating model materially change.

The review shall consider new, revised, superseded, withdrawn, or draft NIST publications and determine required updates.

Revisions shall identify the reason for change, affected controls, approval, effective date, and required implementation, migration, communication, training, or exception actions.
