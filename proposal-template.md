# Suppy Chain Management

_todo brief overview_

## Problem Statement

<!-- 
  - Identify a specific problem within one of the designated domains that can be addressed using 
    blockchain technology.
  - Blockchain Technology has to be effectively utilized in capturing detailed work flows and 
    transactions, across multiple stakeholders, and also implementing value-added features using 
    other emerging technologies.
  - Under each defined use case category, additional option is provided for the startups to submit 
    their own idea which is unique, innovative use case, provided the startups can establish 
    collaborations with the relevant State and/or Central Government departments who are keen to 
    take it forward.
-->

> Blockchain technology can fundamentally transform cross-border digital trade by creating a 
> decentralized, trusted and globally interoperable layer for the issuance, exchange and 
> verification of critical documents such as e-Bills of Lading, Certificates of Origin, Inspection 
> Certificates etc. By anchoring these documents as cryptographically signed, immutable records on 
> a permissioned blockchain, all the stakeholders across the value chain can gain real-time access 
> to a single, tamper-proof source of truth, eliminating fraud, redundancy and delays. Smart 
> contracts can automate document workflows and compliance checks against standards, while 
> interoperability protocols can ensure seamless cross-border recognition, integrating with 
> national systems and global frameworks to drastically reduce transaction costs and clearance 
> times for exporters.

## Innovative Solution

<!-- 
  - Describe the proposed solution and how it leverages blockchain technology to address the 
  identified problem.
  - Develop and leverage deep tech features (eg. enhanced consensus, AI based analytics, enhanced 
  performance, blockchain oracles, intelligent smart contracts, tokenisation etc) for permissioned 
  blockchain, to introduce Innovation and uniqueness into the solution.
-->

<!-- 
  MerkNet: Progressive Trust Infrastructure for Cross-Border Trade

  1. Progressive decentralization model — Merkantis starts as a trusted principal (buying/selling), with every transaction recorded on Fabric. As
  on-chain history builds trust, parties graduate to direct interaction. Identity mirrors this: managed DIDs → self-sovereign nodes.
  2. Full document lifecycle on-chain — every state from RFQ to delivery confirmation is a Fabric transaction. Manual data entry now via web/app
  interface, architecture supports IoT data sources in future without chaincode changes.
  3. On-chain reputation system — replaces escrow. Payment timeliness, quality acceptance rates, delivery reliability all recorded immutably. Reputation
  determines access tier (Merkantis-intermediated vs. direct).
  4. Rule-based compliance engine — automated validation against DGFT, destination country regs, bilateral agreements. Evolves into ML-based anomaly
  detection as transaction data accumulates.
  5. Smart contract payment tracking — records agreed milestones and terms, emits events on completion, records late payments on-chain. No escrow lockup,
    but permanent accountability.
  6. evidence anchoring — photos, test reports, inspection data hashed on-chain, stored off-chain with lifecycle management (hot → cold storage
  post-dispute window).
  7. DID-based supplier qualification — Indy VCs for factory certs, past performance, MSME registration. Portable reputation across the network.
-->

## Blockchain Applicability

<!-- 
  - Explain how blockchain technology is applicable to the identified problem.
  - Discuss the benefits of using blockchain, such as decentralization, transparency, and 
    immutability.
-->

_todo_

## Technical Approach and Architecture

<!-- 
  - Outline the technical approach and architecture of the proposed solution.
  - The architecture should be modular with API integration support and replicable.
  - Describe the components, interactions, and data flows.
-->

### Platform Stack

| Layer | Technology | Role |
|---|---|---|
| Consensus | SmartBFT ordering service | Shared BFT consensus across all channels |
| Ledger | Hyperledger Fabric 3.0+ | Transactions, chaincode, world state |
| Privacy | PDCs + Envelope Encryption | Per-deal data isolation via cryptography |
| Identity | Fabric CA (ABAC) | Certificates with role attributes |
| Middleware | Hyperledger FireFly | REST API, event bus, token management |
| Document Storage | IPFS (encrypted) | Peer-pinned documents, content-addressed |
| Application | Merkantis Platform | Web/app interface, QR verification |

### Network Topology

**Ordering Service (SmartBFT)**

The ordering service is the shared consensus layer across all channels. Orderer
nodes sequence transactions into blocks but do not store ledger data or execute
chaincode. They are blind relays with BFT consensus.

Orderer distribution:

- Phase 1 (MVP): Merkantis (2 nodes) + DGFT (1) + Customs (1) = 4 orderers, F=1
- Phase 2: Add large buyer/supplier orgs = 7 orderers, F=2
- Phase 3: Further distribution across trade partners

**Peer Nodes**

Each participating org runs one or more peer nodes. Peers join channels, store
the channel ledger in CouchDB, and execute chaincode (endorsement). A single
peer can participate in multiple channels.

- Merkantis: 2 peers (HA) + FireFly + IPFS node
- Government (DGFT, Customs): 1 peer + FireFly
- Large suppliers/buyers (Phase 2+): 1 peer + optional FireFly
- Small suppliers/buyers: no own infrastructure, managed identity via Merkantis

### Channel Architecture

The network uses two Fabric channels. Privacy within channels is handled by
Private Data Collections (PDCs) with envelope encryption, not by channel
proliferation.

**1. Network Channel**

All participants are members. Stores:

- Identity registry (public keys, org metadata, role attributes)
- Reputation chaincode (aggregated scores from completed deals)
- Compliance rule definitions (DGFT export policies, destination country regs)

Read-heavy, write on deal completion or identity events.

**2. Trade Channel**

All active trading participants are members. All deal lifecycle transactions
occur here. Privacy is enforced through a combination of PDCs and envelope
encryption — the channel ledger contains only hashes and encrypted blobs,
never plaintext commercial data.

### Privacy Layer

A single PDC named `tradedata` on the trade channel with broad membership.
Access control is cryptographic via envelope encryption — not infrastructural.
Structured deal data lives encrypted in the PDC; documents live encrypted on
IPFS. The Fabric public ledger stores only hashes, IPFS CIDs, and deal
metadata. See **Security, Privacy, Scalability and Performance** for the full
encryption mechanism.

### Identity and Access Control

Fabric CA issues X.509 certificates with embedded role attributes (ABAC).
Chaincode checks roles before every operation. Government participants with
existing Indian Digital Signature Certificates (DSCs) can use them via Fabric's
external CA support. See **Permissioned Blockchain and Smart Contracts** for
the full access control model.

### Data Flow

**Pre-Order (hub-and-spoke through Merkantis):**

```
1. Buyer onboards → Merkantis provisions managed identity
   on trade channel

2. Buyer submits RFQ via Merkantis platform
   → encrypted in PDC (Buyer + Merkantis only)
   → hash on public ledger

3. Merkantis analyzes RFQ, contacts relevant suppliers
   → each supplier gets the RFQ in their own PDC entry
     (Supplier + Merkantis only)

4. Suppliers respond with quotes/feedback
   → encrypted in PDC (Supplier + Merkantis)

5. Merkantis aggregates responses, sends options to buyer
   → encrypted in PDC (Buyer + Merkantis)

6. Buyer selects supplier
   → new PDC entry created for the deal
     (Buyer + Supplier + Merkantis)
   → deal key generated + distributed via envelope encryption
```

**Post-Order (deal lifecycle on trade channel):**

```
7. Commercial + technical negotiation
   → each revision is a Fabric transaction
   → specifications, drawings stored encrypted on IPFS
   → hashes + CIDs on public ledger

8. Order acceptance → PO recorded on Fabric
   → payment terms chaincode activated
   → milestones defined in PDC

9. Manufacturing + quality checkpoints
   → inspector/factory worker enters data via Merkantis
     interface
   → recorded on Fabric, encrypted in deal PDC
   → [Future: IoT sensors with own Fabric CA identity
     write directly via FireFly API — same chaincode,
     same schema, signing identity changes from person
     to device]

10. Pre-shipment inspection
    → report + optional photos encrypted on IPFS
    → hash + CID on Fabric

11. Shipping + customs
    → e-BL minted as token via FireFly token connector
    → customs node added to deal PDC (deal key encrypted
      with customs pubkey)
    → rule-based compliance checks against DGFT/destination
      regs

12. Delivery + acceptance/rejection
    → buyer confirms via platform
    → recorded on Fabric

13. Payment milestone tracking
    → chaincode emits events on milestone completion
    → FireFly pushes to buyer's finance system
    → late payments recorded on public ledger

14. Deal completion → reputation update
    → deal chaincode emits completion event
    → reputation chaincode on network channel aggregates:
      payment timeliness, quality acceptance rate,
      delivery reliability
```

### Deployment Topology

```
Phase 1 (MVP):
  Merkantis: 1 orderer + 1 peer + FireFly + IPFS
  DGFT/Customs: 1 orderer + 1 peer
  All others: managed identities via Merkantis

Phase 2 (growth):
  Merkantis: 2 orderers + 2 peers (HA) + FireFly + IPFS
  + Large suppliers/buyers: own peer + optional FireFly
  + Additional government orderers
  Orderer set expands to 7 (F=2)

Phase 3 (mature network):
  Each major org: peer + FireFly + IPFS
  Orderers distributed across 7+ independent orgs
  Merkantis transitions from principal to platform operator
```

All components are containerized (Docker/Kubernetes) with standardized
deployment templates. Managed participants share Merkantis infrastructure.
Self-sovereign participants can deploy on their own infra or have Merkantis
provision a cloud instance (as a paid service). See **Security, Privacy,
Scalability and Performance** for resource estimates.

## Benchmarking and Differentiation

<!-- 
  - Compare the proposed solution with existing solutions.
  - Highlight the differentiation factors, such as unique features, improved efficiency, or 
    enhanced security.
-->

_todo_

## Non-Crypto Blockchain Platform

<!--
  - Specify the non-crypto blockchain platform planned for
    development (e.g., Hyperledger Fabric, Corda).
  - Explain how the chosen platform effectively addresses the
    identified problem.
-->

### Selected Stack

**Hyperledger Fabric 3.0+** — Enterprise Ledger

The core distributed ledger. Fabric is a permissioned blockchain with no
cryptocurrency, no tokens, and no mining. Key properties for this use case:

- **SmartBFT consensus** (new in 3.0) — Byzantine fault tolerant ordering.
  Required because orderer nodes are run by independent, mutually untrusting
  organizations (Merkantis, DGFT, Customs). Tolerates up to F malicious nodes
  with 3F+1 total orderers.
- **Channels and PDCs** — two channels (network + trade) with Private Data
  Collections for per-deal encryption. Provides complete data isolation without
  channel proliferation.
- **Fabric CA with ABAC** — X.509 certificate-based identity with embedded role
  attributes. Chaincode enforces access control on every operation. No separate
  identity layer needed.
- **Execute-order-validate** model — chaincode executes on endorsing peers
  before transactions reach the ordering service. Orderers never see plaintext
  transaction data, only pre-endorsed proposals.
- **Chaincode in Go** — production-grade smart contract execution in isolated
  Docker containers.
- **Indian government precedent** — Fabric is used in NIC pilots, NITI Aayog
  blockchain initiatives, and is the de facto standard for Indian government
  blockchain deployments.

**Hyperledger FireFly** — Middleware

A self-hosted middleware server that sits between the application layer and
Fabric. Eliminates the need for custom API and indexer layers.

- REST API gateway — any HTTP client or existing ERP system can interact with
  the blockchain without Fabric SDK knowledge
- Event bus — real-time notifications when document states change (e.g., notify
  buyer's finance system when a milestone is hit)
- Token connector — e-BL minted and transferred as non-fungible tokens without
  cryptocurrency
- Off-chain data exchange — messaging with on-chain proof anchoring for large
  payloads
- SDK available for participants with their own software stacks (suppliers,
  government systems)

**IPFS** — Document Storage

Content-addressed distributed storage for trade documents. IPFS is a protocol
— it has no cryptocurrency (Filecoin is a separate token layer on top; we do
not use it).

- Content addressing — each document's CID is derived from its hash, providing
  built-in integrity verification
- Peer pinning — buyer and supplier IPFS nodes pin each other's deal documents
  for redundancy without central storage
- Access control via envelope encryption — documents are stored encrypted on
  IPFS, only deal parties with the deal key can decrypt. IPFS itself is open;
  cryptography enforces privacy.

### Alternatives Considered

**Hyperledger Fabric 2.x with Raft consensus** — Raft is crash fault tolerant
(CFT) only. It tolerates node crashes but not malicious behavior. Suitable when
all orderers are run by a single trusted org. Not suitable for MerkNet where
orderers are distributed across Merkantis, DGFT, and Customs — independent
entities that do not fully trust each other. SmartBFT (Fabric 3.0+) provides
the correct Byzantine fault tolerant model for multi-org government-involved
deployments.

**Hyperledger Indy** — Considered for decentralized identity with Verifiable
Credentials and zero-knowledge proof (ZKP) selective disclosure. Dropped
because: (a) Fabric CA with ABAC covers all identity and access control needs
for supply chain — parties in a deal know each other, ZKP selective disclosure
is unnecessary; (b) running a separate Indy network adds operational complexity
(separate ledger, separate consensus, separate nodes) with no proportional
benefit for this use case. Portable cross-platform credentials via Indy/Aries
remain a future roadmap item.

**R3 Corda** — Designed for financial services. Lacks multi-channel
architecture and native middleware like FireFly. No ABAC or built-in identity
framework beyond basic X.509.

**Hyperledger Besu / Polygon / Ethereum L2s** — Public chain derivatives. Even
as permissioned variants, they carry crypto-adjacency risk with Indian
regulators where non-crypto platforms are explicitly preferred.

**Hyperledger Sawtooth** — Deprecated; inactive community and limited enterprise
adoption.

**Cosmos SDK** — Framework for building sovereign chains. Significant
engineering overhead and crypto-adjacent ecosystem (IBC, staking). Overkill for
a permissioned enterprise deployment.

## Permissioned Blockchain and Smart Contracts

<!--
  - Describe the use of permissioned blockchain and smart
    contracts in the proposed solution.
  - Explain how these features enhance the solution's
    functionality and security.
-->

### Network Participants

Every participant is an authenticated organization with a Membership Service
Provider (MSP) identity issued by Fabric CA. No anonymous participation is
possible.

| # | Organization | Role | Infrastructure |
|---|---|---|---|
| 1 | Merkantis | Network operator, trade execution | Orderer(s) + peer(s) + FireFly + IPFS |
| 2 | DGFT | Export licensing, compliance | Orderer + peer + FireFly |
| 3 | Customs (CBIC) | Clearance, filing | Orderer + peer + FireFly |
| 4 | Suppliers | Component manufacturers | Managed or own peer |
| 5 | Buyers | Procurement entities | Managed or own peer |
| 6 | Quality Inspectors | Third-party or Merkantis-affiliated | Managed identity |
| 7 | Logistics | Freight, shipping (Phase 2+) | Own peer |
| 8 | Banks | Trade finance (Phase 2+) | Own peer |

### Progressive Identity Model

New buyers and suppliers are onboarded with a **managed identity** — Merkantis
holds their Fabric CA credentials and signs transactions on their behalf. Every
platform interaction is still recorded as a Fabric transaction attributed to
the managed identity.

After the first successful deal, a participant may be provisioned their own
Fabric peer and CA credentials, transitioning to a **self-sovereign** identity.
Merkantis can also provision a cloud instance for them as a paid service. This
managed-to-self-sovereign transition mirrors the business model: Merkantis as
principal (Phase 1) to Merkantis as platform operator (Phase 2+).

Government participants with existing Indian Digital Signature Certificates
(DSCs) — X.509 certificates issued by CCA-accredited authorities (eMudhra,
Sify, NCode) under the IT Act 2000 — can use them via Fabric's external CA
support in MSP configuration.

### Access Control (ABAC)

Fabric CA embeds role attributes directly in each participant's X.509
certificate. Chaincode enforces Attribute-Based Access Control (ABAC) on every
operation by reading these attributes at invocation time.

**Certificate attributes:**

- `role`: buyer, supplier, inspector, customs_officer, logistics, merkantis_admin
- `org`: organization MSP ID
- `certifications`: ISO9001-auditor, DGFT-authorized, CBIC-officer, etc.

**Chaincode access control examples:**

- `SubmitRFQ` — requires `role=buyer`
- `RespondToRFQ` — requires `role=supplier` or `role=merkantis_admin`
- `RecordInspection` — requires `role=inspector` + relevant `certifications`
- `FileCustomsDeclaration` — requires `role=customs_officer`
- `ConfirmDelivery` — requires `role=buyer` + membership in the deal's PDC
- `UpdateReputation` — system-only, triggered by deal completion event

**Endorsement policies per chaincode:**

- PO confirmation: buyer peer + supplier peer + Merkantis peer
- Inspection approval: inspector peer + buyer peer
- Customs filing: Merkantis peer + customs peer
- Reputation update: Merkantis peer only (reads from completed deal data)

### Multi-Party Signature Thresholds

High-value transactions require approval from higher decision authorities
within an organization. During onboarding, each org configures a decision-maker
hierarchy (names, roles, approval thresholds).

A `ThresholdPolicy` chaincode runs on the org's own peer with an org-specific
endorsement policy (e.g., `OR('MerkantisMSP.member')`). Threshold configuration
is stored in the org's implicit private collection (`_implicit_org_<MSPID>`) —
invisible to other network participants.

```
Flow:
1. User initiates high-value action (e.g., PO > 10L INR)
2. Org's peer executes ThresholdPolicy chaincode
   → reads threshold config from implicit collection
   → determines additional signatures needed
3. FireFly emits event → platform sends review links
   to required decision makers
4. Decision makers review and sign via platform
5. Once threshold met, main transaction is submitted
   with normal multi-org endorsement
```

This keeps governance rules private to each org while ensuring the final Fabric
transaction is properly authorized. Self-sovereign participants configure their
own thresholds in their own implicit collection.

### Smart Contract Suite

Six chaincode packages deployed across the two channels.

**1. DealLifecycle** (trade channel)

Manages the full order lifecycle from RFQ to completion.

```
States:
  RFQ_SUBMITTED
  → QUOTES_RECEIVED
  → SUPPLIER_SELECTED
  → NEGOTIATION
  → PO_CONFIRMED
  → IN_PRODUCTION
  → SHIPPED
  → DELIVERED
  → COMPLETED | DISPUTED

Triggers:
  RFQ_SUBMITTED: buyer submits via platform
  QUOTES_RECEIVED: supplier(s) respond
  SUPPLIER_SELECTED: buyer selects, deal PDC created
  PO_CONFIRMED: buyer + supplier + Merkantis endorse
    (ThresholdPolicy checked if value exceeds org limit)
  IN_PRODUCTION: supplier confirms manufacturing start
  SHIPPED: logistics confirms pickup, e-BL minted
  DELIVERED: buyer confirms receipt
  COMPLETED: all milestones settled
  DISPUTED: either party raises within dispute window
```

**2. QualityVerification** (trade channel)

Manages inspection checkpoints and acceptance workflow.

```
States:
  SCHEDULED
  → IN_PROGRESS
  → REPORT_SUBMITTED
  → APPROVED | REJECTED | REWORK

Triggers:
  SCHEDULED: deal reaches inspection milestone
  IN_PROGRESS: inspector begins (via platform)
  REPORT_SUBMITTED: inspector uploads report,
    document hash + IPFS CID anchored
  APPROVED: buyer confirms acceptance
    → emits event to PaymentTracking
  REJECTED: buyer rejects with documented reasons
  REWORK: supplier acknowledges, re-enters production
```

**3. DocumentManagement** (trade channel)

Handles trade documents as tokens via FireFly's token connector.

```
States:
  DRAFTED → ISSUED → TRANSFERRED → RETIRED

Document types:
  e-BL (Bill of Lading): minted as non-fungible token.
    Transfer preserves endorsement chain in token
    metadata — mirrors physical BL endorsement process.
  CoO (Certificate of Origin): issued after compliance
    chaincode validates against DGFT rules.
  Packing List, Material Test Certs, Inspection Reports:
    hashed on-chain, encrypted document on IPFS.

Each state transition requires digital signature from
an authorized party (ABAC-enforced).
```

**4. PaymentTracking** (trade channel)

Records agreed milestones and tracks payment status. Does not hold funds or
implement escrow.

```
States:
  TERMS_DEFINED
  → MILESTONE_DUE
  → PAYMENT_RECORDED
  → LATE_PAYMENT_FLAGGED
  → SETTLED

Triggers:
  TERMS_DEFINED: PO confirmation, milestones written
    to deal PDC (e.g., 30/40/30 split)
  MILESTONE_DUE: QualityVerification or DealLifecycle
    emits milestone event
  PAYMENT_RECORDED: buyer confirms payment via platform
    (or bank integration in Phase 2+)
  LATE_PAYMENT_FLAGGED: payment not recorded within
    agreed window — written to public ledger
  SETTLED: all milestones paid

Late payments are recorded on the public ledger (not in
the PDC) — visible to all network participants as input
to the reputation system.
```

**5. Compliance** (trade channel)

Rule-based validation against trade regulations.

```
Checks:
  - DGFT export policy (IEC validity, product HS code
    against restricted/prohibited lists)
  - Destination country import regulations
    (CE marking, REACH, RoHS as applicable)
  - Bilateral trade agreement requirements
  - Sanctions screening

Invoked automatically before DocumentManagement issues
a CoO or customs-related document. Results recorded
on-chain. Failures block document issuance with a
documented reason.

Rule definitions stored on the network channel —
updatable by DGFT/Customs without chaincode upgrade.
```

**6. Reputation** (network channel)

Aggregates performance metrics from completed deals.

```
Inputs (from deal completion events):
  - Payment timeliness (on-time vs. late, from
    PaymentTracking)
  - Quality acceptance rate (approved vs. rejected,
    from QualityVerification)
  - Delivery reliability (on-time vs. delayed, from
    DealLifecycle)
  - Communication responsiveness (response times
    between state transitions)

Output:
  Aggregated scores per participant, visible to all
  network channel members. Individual deal details
  remain encrypted in deal PDCs — only aggregate
  metrics are published.

Reputation scores determine access tier:
  - Low reputation → Merkantis-intermediated only
  - High reputation → direct buyer-supplier deals
```

## Regulatory Framework

<!-- 
  - Discuss how the proposed solution complies with the regulatory framework of the domain.
  - Highlight any relevant laws, regulations, or standards.
-->

_todo_

## Security, Privacy, Scalability and Performance

<!--
  - Explain how the proposed solution addresses security,
    privacy, scalability and performance requirements.
  - Describe the measures taken to ensure the solution's
    robustness and reliability.
-->

### Security

**Consensus-level protection (SmartBFT)**

The ordering service tolerates up to F Byzantine (malicious) nodes with 3F+1
total orderers. Phase 1 deploys 4 orderers (F=1) across Merkantis, DGFT, and
Customs. Phase 2 expands to 7 orderers (F=2) with additional independent orgs.
No single org can unilaterally manipulate transaction ordering.

**Identity and authentication**

All network participants hold X.509 certificates issued by Fabric CA with
embedded role attributes (ABAC). No anonymous participation. Government
participants with existing Indian Digital Signature Certificates (DSCs) —
issued by CCA-accredited authorities under the IT Act 2000 — can use them
directly via Fabric's external CA support, reusing India's existing digital
signature infrastructure for network authentication.

**Mutual TLS (mTLS)**

All peer-to-peer and client-to-peer communication is mTLS-encrypted. Unlike
standard HTTPS where only the server authenticates, mTLS requires both sides to
present and verify certificates. Every connection between orderers, peers, and
clients is mutually authenticated using the same X.509 / DSC certificates used
for transaction signing.

**Execute-order-validate model**

Fabric's transaction pipeline ensures orderers never see plaintext transaction
data. Chaincode executes on endorsing peers, producing a signed read-write set.
The ordering service only sequences these pre-endorsed proposals into blocks —
it cannot read or modify transaction content.

**Key management**

- Orderer signing keys: HSM-backed (cloud HSM in cloud deployments — AWS
  CloudHSM, Azure Dedicated HSM; or hardware appliance such as YubiHSM for
  on-prem nodes)
- CA root key: HSM-backed
- Peer and participant keys: software-based encrypted-at-rest key stores,
  upgradeable to HSM
- On-prem deployment option: locked-down Linux appliance with integrated HSM
  chip, shipped as plug-and-play hardware to participants who require
  on-premises infrastructure

**Chaincode isolation**

All chaincode executes in isolated Docker containers on endorsing peers. A
compromised chaincode cannot access peer state outside its designated channel
and collections.

**Immutable audit trail**

Every state change is a Fabric block committed to all channel peers. Regulators
(DGFT, Customs) audit directly from their own peer — no data requests needed,
no intermediary involved.

**Multi-party signature thresholds**

High-value transactions require additional internal approvals via the
ThresholdPolicy chaincode (see Permissioned Blockchain and Smart Contracts).
Threshold configs are private to each org via implicit collections.

### Privacy

**Envelope encryption — the core mechanism**

All deal data is encrypted using a consistent envelope encryption pattern
across both structured data (PDC) and documents (IPFS).

1. Deal creator generates a random AES-256 symmetric key (the "deal key")
2. Structured deal data (terms, pricing, milestones) is encrypted with the deal
   key and stored in the PDC
3. Documents (PDFs, images, test reports) are encrypted with the same deal key
   and stored on IPFS
4. The deal key is encrypted separately with each group member's public key
   using ECIES (Elliptic Curve Integrated Encryption Scheme)
5. Per-member encrypted key shares are stored in the PDC
6. The Fabric public ledger stores only: deal ID, hash of plaintext data, IPFS
   CIDs, deal status, and member list

```
Fabric Public Ledger (visible to all channel members):
  { deal_id: "DEAL-456",
    data_hash: "sha256(plaintext)",
    ipfs_cid: "Qm...",
    members: ["BuyerA", "SupplierX", "Merkantis"],
    status: "active" }

PDC "tradedata" (stored on all peers, encrypted):
  { _id: "DEAL-456",
    encrypted_payload: "<AES-256 ciphertext>",
    encrypted_keys: {
      "BuyerAMSP":    "<deal key enc w/ BuyerA pubkey>",
      "SupplierXMSP": "<deal key enc w/ SupplierX pubkey>",
      "MerkantisMSP": "<deal key enc w/ Merkantis pubkey>"
    }}

IPFS (encrypted, pinned by deal parties):
  encrypted(PO.pdf), encrypted(COO.pdf),
  encrypted(inspection_report.pdf)
```

**Adding a party** (e.g., customs joins a deal): encrypt the deal key with their
public key and append to encrypted_keys.

**Revoking access**: re-encrypt all deal data with a new deal key and
redistribute to remaining members.

**Privacy guarantees at each layer:**

| Layer | What's visible | What's hidden |
|---|---|---|
| Public ledger | Deal exists, hash, status, members | All commercial data |
| PDC | Encrypted blob on all peers | Plaintext (only deal parties decrypt) |
| IPFS | Encrypted documents (anyone can pin) | Content (only deal parties decrypt) |
| ABAC | Operation was authorized | Internal org governance rules |

**DPDP Act 2023 compliance**

- Data minimization: only hashes on shared ledger, no PII
- Consent management at application layer (Merkantis platform)
- Right to erasure: off-chain data (IPFS, PDC) can be deleted; on-chain hashes
  are non-personal and do not constitute personal data
- Key destruction: deleting all copies of the deal key renders encrypted data
  permanently unreadable even if the ciphertext persists

### Scalability

**Channel architecture**

The network uses two channels (network + trade). This keeps channel management
overhead constant regardless of deal volume. PDCs scale cheaply — they are
access-controlled data entries within a channel, not separate ledgers.

**Vertical scaling**

A single Fabric peer's throughput exceeds projected order volume by orders of
magnitude (see Performance below). Vertical scaling (larger VM) is the primary
scaling lever for individual nodes.

**Redundant peers for availability**

Merkantis runs 2 peers in Phase 2+ for high availability. If one peer goes
down, the other continues serving. This is replica redundancy, not data
sharding — both peers hold identical ledger state. The 3F+1 formula applies to
orderers only; peers can be added freely without affecting consensus.

**Off-chain processing**

FireFly handles API caching, event indexing, and data exchange off-chain. IPFS
stores documents independently of Fabric. Only hashes and state transitions
touch the ledger.

**Projected volume and capacity:**

| Metric | Estimate |
|---|---|
| Transactions per order | ~100-300 |
| Concurrent orders (Phase 1) | ~50-100 |
| Concurrent orders (Phase 2) | ~500-1,000 |
| Peak TPS required (Phase 2) | ~5-10 |
| Fabric SmartBFT capacity | ~3,000 TPS |
| Headroom at Phase 2 | ~300-600x |
| Ledger growth per 1,000 orders | ~1-5 GB |
| IPFS storage per 1,000 orders | ~50-100 GB |

**Resource requirements per participant type:**

| Participant | Components | Compute | RAM | Storage | Est. cost/mo |
|---|---|---|---|---|---|
| Merkantis (MVP) | 1 orderer + 1 peer + CouchDB + FireFly + IPFS | 4 cores | 16 GB | 200 GB SSD | $80-150 |
| Merkantis (growth) | 2 orderers + 2 peers + CouchDB + FireFly + IPFS | 12 cores | 32 GB | 500 GB SSD | $250-400 |
| Government node | 1 orderer + 1 peer + CouchDB + FireFly | 4 cores | 12 GB | 200 GB SSD | $80-120 |
| Self-sovereign entity | 1 peer + CouchDB + FireFly | 2 cores | 8 GB | 100 GB SSD | $40-80 |
| Managed identity | Shared on Merkantis infra | — | — | — | $0 |

SSD is required for CouchDB random reads (world state queries) and Fabric block
commits. All components run as Docker containers — deployable on any cloud
provider (AWS, Azure, GCP, Hetzner, Linode) or locally for development and
demos.

### Performance

Fabric 3.0 with SmartBFT benchmarks at ~3,000 TPS. A cross-border
manufacturing order generates ~100-300 transactions over its multi-week
lifecycle. Even at 1,000 concurrent orders, peak throughput demand is ~5-10 TPS
— well within a single peer's capacity.

Document verification (hash lookup against the ledger) is sub-second. IPFS
retrieval of encrypted documents depends on network conditions but is typically
under 5 seconds for pinned content.

## Execution Plan

<!-- 
  - Provide a detailed execution plan, including milestones, timelines, and resource allocation.
  - Outline the development, testing, and deployment phases.
-->

_todo_

## Expected Outcomes and Pilot Deployment

<!-- 
- Describe the expected outcomes of the proposed solution.
- Outline the plan for pilot deployment, including the scope, timeline, and evaluation metrics.
-->

_todo_

## Government Department Collaboration

<!-- 
  - Identify the government departments with which the startup plans to collaborate.
  - Explain how the proposed solution is relevant to these departments and how it can benefit them.
  - Define a viable Business Model and plan for a Wider Proliferation Strategy across India for 
    different states and UTs.
-->

_todo_

## Additional Details

<!--
  - Provide any additional details relevant to the idea, such as market analysis,
    user-adoption strategies, or potential partnerships.
-->

### Success Metrics

| Metric | Current (Manual) | Target | Reasoning |
|---|---|---|---|
| Document verification | 3-5 days | <5 seconds | Hash lookup against Fabric ledger — near-instant |
| Export clearance cycle | 30-45 days | Platform-side: <24 hours | End-to-end improvement depends on ICEGATE integration; platform eliminates document prep delays |
| Payment release post-milestone | 15-30 days | <24 hours | Chaincode emits milestone event, FireFly notifies buyer's finance system immediately |
| Document discrepancy rate | 15-20% | <2% | Compliance chaincode validates before document issuance — errors caught at source |
| Supplier onboarding time | 2-4 weeks | Hours | Managed identity + certificate upload via platform; no manual qualification paperwork |
| Dispute resolution time | Weeks of email | Days | Full on-chain evidence trail — no "he said / she said" |
| Cross-border doc acceptance at first submission | ~80-85% | 98%+ | Compliance chaincode ensures docs meet destination regs before submission |
| Audit compliance time (per order) | Days of document collection | Minutes | Regulator queries their own peer — single query, full trail |
| Repeat order rate | Baseline TBD | Measurable increase | On-chain trust history reduces switching friction, encourages repeat business |

### Business Model

- **Transaction fee**: per-order fee for blockchain anchoring and smart contract
  execution
- **Subscription**: monthly access for suppliers/buyers based on volume tier
- **Managed infrastructure**: Merkantis provisions and operates cloud instances
  for self-sovereign participants who prefer not to manage their own nodes
  (paid service)
- **Analytics and intelligence**: subscription access to aggregated supplier
  performance data, predictive analytics, and benchmarking — built on
  Merkantis's proprietary dataset from transaction history. This is a Merkantis
  product layer on top of the network, not a blockchain feature.
- **Government licensing**: platform-as-a-service for state trade facilitation
  bodies

### Market Size

- India's merchandise exports: $437B (FY2024)
- MSME contribution to exports: ~45% ($197B)
- Cross-border trade documentation market (India): estimated $2-3B annually
- Addressable market for MerkNet (precision manufacturing exports): $500M+ in
  documentation and trade facilitation services

### Why Merkantis is Uniquely Positioned

1. **Live operational data from 200+ cross-border engagements** — the platform
   digitizes workflows Merkantis already runs manually, not theoretical
   processes
2. **Existing supplier network** in Coimbatore, Bangalore, and Chennai
   manufacturing clusters — pilot-ready on day one
3. **Active buyer relationships in Europe** (Switzerland, Germany) — demand-side
   validation
4. **Domain expertise in precision manufacturing** — understands the specific
   documentation, quality, and compliance requirements that generic blockchain
   platforms miss
5. **Cross-border entity structure** (India + Switzerland) — firsthand
   understanding of the regulatory landscape on both sides

### Potential Partnerships

- **ICEGATE / NIC** — customs integration for electronic filing
- **Indian banks (SBI, ICICI)** — trade finance module (Phase 2+)
- **CODISSIA** (Coimbatore District Small Industries Association) — supplier onboarding at scale
- **Swiss-Indian Chamber of Commerce (SICC)** — cross-border trade corridor validation
- **CII / FICCI** — pan-India industry rollout through association partnerships

### Future Roadmap

- **IoT integration**: factory-floor sensors with own Fabric CA identities
  writing directly to the trade channel via FireFly API. Same chaincode, same
  schema — signing identity changes from person to device. Architecture is
  ready; adoption depends on factory digitization.
- **Portable credentials via Hyperledger Indy/Aries**: supplier certifications
  and performance attestations as Verifiable Credentials that travel across
  platforms and buyers. Enables cross-network reputation portability.
- **Bank integration**: trade finance instruments (LCs, bank guarantees) as
  on-chain workflows with partner banks. Payment milestones trigger automated
  finance events.
- **Multi-corridor expansion**: India-EU → India-UAE → India-US → India-Africa
  trade corridors
- **Data-driven compliance evolution**: as transaction history accumulates,
  rule-based compliance engine evolves to incorporate ML-based anomaly
  detection (flagging statistically improbable quality claims, timeline
  commitments inconsistent with historical data)

## Signature and Team Information

<!-- 
  - Include the signatures of the startup founder and team members.
  - Provide contact information and relevant details about the team members.
-->

_todo_
