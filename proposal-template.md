# Supply Chain Management

**Blockchain-Based Cross-Border Trade Documentation and Execution Layer for
Indian Manufacturing Exports**

Merkantis proposes a permissioned blockchain platform that creates a
decentralized, tamper-proof documentation and verification layer for
cross-border manufacturing supply chains originating from India. The platform
anchors every critical trade document — from RFQ to final delivery — as
cryptographically signed, immutable records on Hyperledger Fabric, giving all
stakeholders real-time access to a single source of truth.

The solution is grounded in Merkantis's live operational experience managing
200+ supplier engagements across India, Europe, and the Middle East.

## Problem Statement

<!--
  - Identify a specific problem within one of the designated domains that can
    be addressed using blockchain technology.
  - Blockchain Technology has to be effectively utilized in capturing detailed
    work flows and transactions, across multiple stakeholders, and also
    implementing value-added features using other emerging technologies.
-->

> Blockchain technology can fundamentally transform cross-border digital trade
> by creating a decentralized, trusted and globally interoperable layer for the
> issuance, exchange and verification of critical documents such as e-Bills of
> Lading, Certificates of Origin, Inspection Certificates etc. By anchoring
> these documents as cryptographically signed, immutable records on a
> permissioned blockchain, all the stakeholders across the value chain can gain
> real-time access to a single, tamper-proof source of truth, eliminating
> fraud, redundancy and delays. Smart contracts can automate document workflows
> and compliance checks against standards, while interoperability protocols can
> ensure seamless cross-border recognition, integrating with national systems
> and global frameworks to drastically reduce transaction costs and clearance
> times for exporters.

### The Operational Reality

Cross-border manufacturing sourcing from India fails not because of bad
suppliers, but because **no intermediary owns the outcome**, and the
documentation infrastructure is fragmented, manual, and trust-deficient.

From Merkantis's direct operational experience executing cross-border component
orders:

- **Document fragmentation across 6-8 stakeholders per order.** A single export
  order involves the manufacturer, quality inspector, freight forwarder, customs
  broker, bank (for LC/payment), insurance provider, and the buyer's receiving
  team. Each maintains separate records. No shared ledger exists.
- **30-45 days average document processing time** for Indian manufacturing
  exports. Manual verification of Certificates of Origin, Inspection
  Certificates, material test reports, and Bills of Lading adds 2-4 weeks to
  clearance.
- **15-20% of Indian MSME export shipments face document discrepancies** at
  destination ports, leading to delays, demurrage charges ($500-2,000/day for
  containers), and disputes.
- **No verifiable chain of custody for quality documentation.** Material test
  certificates, dimensional inspection reports, and compliance certifications
  are PDF files passed via email. Forgery is trivial. Verification requires
  contacting the issuing authority manually.
- **Duplicate financing fraud.** The same Bill of Lading can be presented to
  multiple banks for trade finance. Indian exporters lose an estimated
  $1.5-2B annually to trade document fraud.
- **Communication asymmetry.** Indian suppliers quote optimistic timelines and
  delay problem notification. There is no shared, immutable record of committed
  milestones vs. actual delivery.

These are not technology problems in isolation. They are **trust infrastructure
problems** that blockchain is uniquely positioned to solve.

## Innovative Solution

<!--
  - Describe the proposed solution and how it leverages blockchain technology
    to address the identified problem.
  - Develop and leverage deep tech features (eg. enhanced consensus, AI based
    analytics, enhanced performance, blockchain oracles, intelligent smart
    contracts, tokenisation etc) for permissioned blockchain, to introduce
    Innovation and uniqueness into the solution.
-->

### MerkNet: Progressive Trust Infrastructure for Cross-Border Trade

MerkNet is a permissioned blockchain platform that digitizes the entire
lifecycle of cross-border manufacturing trade — from RFQ issuance through
final delivery and payment — creating an immutable, shared record across all
stakeholders. Built on Hyperledger Fabric 3.0+ with SmartBFT consensus,
FireFly middleware, and IPFS document storage.

### Core Innovation Layers

**1. Progressive decentralization**

Merkantis starts as a trusted principal — buying from suppliers and selling to
buyers — with every transaction recorded on Fabric. As on-chain history builds
trust between parties, they graduate to direct interaction. The identity model
mirrors this: new participants get a managed identity (Merkantis holds keys,
signs on their behalf), transitioning to self-sovereign nodes (own peer, own
keys) after the first successful deal. This eliminates the cold-start problem
that killed TradeLens — participants don't need infrastructure to start.

**2. Full document lifecycle on-chain**

Unlike solutions that only anchor final static documents, MerkNet records the
entire lifecycle: every state change from RFQ to delivery is a Fabric
transaction.

- RFQ issuance and supplier quote submission
- Purchase order confirmation and amendment trail
- In-process quality inspection checkpoints
- Pre-shipment inspection reports with evidence hashes
- Material test certificates, compliance certifications
- Bills of Lading (e-BL as non-fungible token), Certificates of Origin
- Customs declarations and clearance confirmations
- Delivery receipts and acceptance/rejection records

Photos, test reports, and inspection data are hashed on-chain and stored
encrypted on IPFS. Pinned content is hot (CDN-like availability across peers);
unpinned content remains accessible but requires retrieval from cold storage.
UCANs (User Controlled Authorization Networks) authorize which parties can pin,
access, and delegate access to specific documents — tied to the deal's envelope
encryption group. Data is always accessible; pinning determines speed.

Data entry is via the Merkantis web/app interface. The architecture supports
IoT sensor data sources in future without chaincode changes — same schema,
different signing identity.

**3. On-chain reputation system**

Replaces escrow (impractical for manufacturing buyers) with permanent
accountability. Key deal milestones — PO confirmed, delivery complete, invoice
paid — are written as hashes to the network channel, building each
participant's verifiable history. Aggregated scores (payment timeliness, quality
acceptance rate, delivery reliability) are computed from completed deals and
visible to all network participants. Reputation determines access tier:
Merkantis-intermediated for new/low-reputation participants, direct
buyer-supplier deals for established ones.

**4. Cryptographic privacy (envelope encryption + UCANs)**

A two-part privacy model. Envelope encryption handles confidentiality: all deal
data is encrypted with a per-deal AES-256 key, distributed to deal parties via
ECIES. Structured data lives encrypted in Fabric Private Data Collections;
documents live encrypted on IPFS. Peers on the same channel cannot read deals
they are not party to — privacy is enforced by cryptography, not network
topology. UCANs (User Controlled Authorization Networks) handle authorization:
capability-based tokens control who can pin, access, and delegate access to
IPFS documents, with delegation chains (e.g., supplier delegates read access to
their inspector). No central authorization server required.

**5. Rule-based compliance engine**

Automated validation against DGFT export policies, destination country import
regulations (CE marking, REACH, RoHS), and bilateral trade agreements. Runs as
chaincode — results recorded on-chain, failures block document issuance with a
documented reason. Rule definitions are stored on the network channel and
updatable by DGFT/Customs without chaincode upgrade. Evolves into data-driven
anomaly detection as transaction history accumulates.

**6. Smart contract payment tracking**

Records agreed payment milestones (e.g., 30/40/30) and tracks fulfillment.
Chaincode emits events on milestone completion — FireFly pushes notifications
to the buyer's finance system. Late payments are recorded on the public ledger,
feeding the reputation system. No escrow lockup, no funds held on-chain — the
smart contract tracks and records, it does not hold.

**7. Embedded supply chain finance**

A dedicated financing channel lets suppliers present verified on-chain POs and
invoices to banks for short-term lending. Banks verify documents by hashing
them and checking against the network channel's milestone history — instant,
tamper-proof verification. Aligned with India's OCEN (Open Credit Enablement
Network) framework, positioning Merkantis as a Loan Service Provider. Solves
duplicate financing fraud: an e-BL minted as a non-fungible token on Fabric
cannot be presented to multiple banks.

**8. QR-code verification**

Inspectors scan QR codes at factories to pull up inspection checklists and deal
context. Customs house agents scan QR codes to pre-populate filing forms from
on-chain data. Logistics providers scan to confirm pickup/delivery. QR
verification is a hash lookup against the local peer — sub-second, works in
low-connectivity factory and port environments. For managed-identity users who
don't interact with the chain directly, QR is their primary interface.

**9. Modular integration**

MerkNet does not replace existing systems. FireFly exposes REST APIs and SDKs
that integrate with whatever ERP, TMS, banking core, or customs system
participants already use. The blockchain is an interoperability and trust layer,
not a platform lock-in.

## Blockchain Applicability

<!--
  - Explain how blockchain technology is applicable to the identified problem.
  - Discuss the benefits of using blockchain, such as decentralization,
    transparency, and immutability.
-->

### Why Blockchain is Essential for This Problem

| Property | Supply Chain Pain Point | Blockchain Resolution |
|---|---|---|
| Decentralization | No single party trusted by all 6-8 stakeholders (buyer, supplier, bank, customs, inspector, logistics) | Shared ledger eliminates reliance on any single intermediary for document authenticity |
| Immutability | Documents altered after issuance; certificates backdated; inspection reports modified | Every state change is a permanent, timestamped Fabric block — tampering is cryptographically impossible |
| Smart Contracts | Payment disputes, manual compliance checks, slow document workflows | Automated milestone tracking, compliance validation, and document routing via chaincode |
| Non-repudiation | Suppliers deny commitments; buyers dispute acceptance; timelines retroactively revised | Cryptographically signed transactions attributed to verified identities — cannot be denied post-facto |
| Auditability | Regulators request documents from each party separately; weeks to reconstruct an order's history | DGFT and Customs audit directly from their own peer — single query, full trail, no intermediary |
| Privacy | Commercial terms must stay confidential between deal parties while proving document legitimacy to third parties (banks, regulators) | Envelope encryption: peers on the same channel see hashes proving deals exist, but cannot read commercial data without the deal key |

### Why a Database Cannot Solve This

The fundamental issue is **multi-party trust across organizational and national
boundaries**. A centralized database requires all parties to trust the database
operator. In cross-border trade involving Indian suppliers, European buyers,
banks in both jurisdictions, customs in multiple countries, and independent
inspectors, no single entity commands universal trust.

Merkantis itself is a stakeholder — buyers and suppliers should not have to
trust Merkantis's database for the integrity of their documents. A permissioned
blockchain eliminates this requirement: each party runs (or accesses) a peer
that independently validates every transaction. Merkantis operates the network
but cannot unilaterally alter records.

Additionally, the financing use case specifically requires tamper-proof document
verification. A bank cannot rely on a supplier's database or even Merkantis's
database to verify a PO is legitimate. On-chain hashes on a shared ledger —
verified by the bank's own peer — provide the trust guarantee that enables
supply chain finance without manual verification.

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
| Privacy | PDCs + Envelope Encryption + UCANs | Per-deal data isolation via cryptography |
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
- Phase 2: Add banks + large buyer/supplier orgs = 7 orderers, F=2
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

The network uses three Fabric channels. Privacy within channels is handled by
Private Data Collections (PDCs) with envelope encryption, not by channel
proliferation.

**1. Network Channel** (ledger-only, no documents)

All participants are members (including banks). Entirely self-contained within
the Fabric ledger — no IPFS references, no off-chain document pointers. Stores:

- Identity registry (public keys, org metadata, role attributes)
- Reputation chaincode (aggregated scores from completed deals)
- Compliance rule definitions (DGFT export policies, destination country regs)
- **Milestone history** — key deal events (PO confirmed, delivery complete,
  invoice paid) are written here as hashes only (no amounts, no commercial
  details, no document CIDs). Banks and other participants can verify document
  legitimacy by hashing a document and checking against this channel without
  accessing the trade channel.

Read-heavy, write on milestone events and deal completion.

**2. Trade Channel**

All active trading participants are members. All deal lifecycle transactions
occur here. Privacy is enforced through a combination of PDCs and envelope
encryption — the channel ledger contains only hashes and encrypted blobs,
never plaintext commercial data.

**3. Financing Channel** (Phase 2+)

Banks and financial institutions participate alongside Merkantis and entities
seeking financing. Suppliers present POs or invoices to banks for short-term
lending (supply chain finance). Banks verify document legitimacy by hashing the
submitted document and checking it against the network channel — instant,
tamper-proof verification without accessing the trade channel. Loan terms,
disbursement, and repayment are recorded on this channel. Separated from the
trade channel because loan terms (interest rates, repayment schedules) should
not be visible to buyers or other trade participants.

### Privacy Layer

A single PDC named `tradedata` on the trade channel with broad membership.
Access control is cryptographic — envelope encryption for confidentiality (who
can read), UCANs for authorization (who can pin, access, delegate on IPFS).
Structured deal data lives encrypted in the PDC; documents live encrypted on
IPFS. The Fabric public ledger stores only hashes, IPFS CIDs, and deal
metadata. See **Security, Privacy, Scalability and Performance** for the full
encryption and authorization mechanism.

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
   → PO hash written to network channel (milestone history)

9. Manufacturing + quality checkpoints
   → inspector scans QR code at factory → pulls up
     inspection checklist and deal context
   → enters data via Merkantis interface
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
    → CHA scans QR → pre-populates customs filing forms
      from on-chain deal data
    → rule-based compliance checks against DGFT/destination
      regs

12. Delivery + acceptance/rejection
    → buyer confirms via platform
    → recorded on Fabric
    → delivery hash written to network channel

13. Payment milestone tracking
    → chaincode emits events on milestone completion
    → FireFly pushes to buyer's finance system
    → late payments recorded on public ledger
    → payment hash written to network channel

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
  - Highlight the differentiation factors, such as unique features, improved
    efficiency, or enhanced security.
-->

### Comparable Solutions

| Dimension | TradeLens | Marco Polo | CargoX | Govt Pilots | MerkNet |
|---|---|---|---|---|---|
| Status | Defunct (2022) | Defunct (2023) | Active | Pilot only | Proposed |
| Scope | Shipping only | Trade finance | e-BL only | Single-dept PoC | Full lifecycle: RFQ → delivery → payment |
| Stakeholders | Carriers, ports | Banks | Customs, shippers | Single govt entity | 6-8 per transaction |
| Manufacturing | None | None | None | None | QC workflow, inspection anchoring |
| India-specific | No | No | No | Yes but narrow | 200+ India supplier engagements |
| Privacy model | Channel isolation | Need-to-know | Public chain | Basic | Envelope encryption + PDCs |
| Identity | Platform accounts | Bank KYC | Wallet-based | Aadhaar-linked | Fabric CA + ABAC + DSC support |
| Onboarding | Required own node | Bank-mediated | Wallet setup | Govt-provisioned | Managed identity, zero infra |
| Supply chain finance | None | Bank-to-bank only | None | None | OCEN-aligned, on-chain PO/invoice verification |
| Platform | Fabric 1.x | Corda | Ethereum | Fabric | Fabric 3.0+ SmartBFT |

**TradeLens (Maersk/IBM)** — shut down November 2022. Covered shipping and
logistics only. Failed partly because it required participants to run their own
infrastructure and couldn't convince Maersk's competitors to join a
Maersk-controlled network.

**Marco Polo (R3 Corda)** — shut down late 2023. Trade finance only
(bank-to-bank). No document lifecycle, no manufacturing integration. Never
bridged the gap between trade execution and trade finance.

**CargoX** — active, used by Egyptian customs (NAFEZA) for electronic Bill of
Lading. Covers e-BL only, not the full trade lifecycle. Built on Ethereum
(public chain) — carries crypto-adjacency concerns.

**Indian government pilots (NIC/NITI Aayog)** — small-scale proofs of concept.
Single-department, narrow scope, not multi-stakeholder.

**Raymond x Spydra** — product identity on Fabric, 15M assets tokenized for
Raymond textiles. QR-based ownership tracking and counterfeit detection. Focused
on product authentication, not cross-border trade documentation.

### Key Differentiators

**1. Operational-first, not tech-first**

Merkantis already executes cross-border manufacturing orders daily. The
blockchain layer digitizes existing workflows, not theoretical ones. The pain
points — document fragmentation, verification delays, trust gaps — come from
200+ real engagements, not market research.

**2. Full lifecycle coverage**

No existing solution covers RFQ through quality verification through shipping
through payment in a single platform. TradeLens covered shipping. Marco Polo
covered finance. Both missed the manufacturing and quality layer where trust
actually breaks down. MerkNet captures the entire order lifecycle from first
inquiry to final payment.

**3. Progressive decentralization**

Managed identities for zero-friction onboarding on day one; self-sovereign
nodes as trust builds. TradeLens failed partly because it required participants
to run infrastructure upfront. MerkNet removes that barrier entirely — new
participants interact via the Merkantis platform with no blockchain knowledge or
infrastructure required.

**4. Cryptographic privacy beyond channel isolation**

Envelope encryption means peers on the same channel cannot read deals they are
not party to. This is stronger than TradeLens's channel-based privacy (all
channel members see everything) or Corda's need-to-know model. Privacy is
enforced by cryptography, not by network topology.

**5. Modular integration, not platform lock-in**

MerkNet does not compete with existing ERP, TMS, or banking systems. FireFly
exposes REST APIs and SDKs that integrate with whatever systems participants
already use. A supplier's existing ERP can push data to the chain; a bank's
core system can query deal status; customs systems can pull compliance data.
The blockchain is an interoperability layer, not a replacement for anything.

**6. Embedded supply chain finance**

Unlike platforms that separate trade execution from trade finance, MerkNet's
financing channel lets banks verify POs and invoices directly against on-chain
records — hash lookup, instant, tamper-proof. Aligned with India's OCEN (Open
Credit Enablement Network) framework, positioning Merkantis as a Loan Service
Provider that connects suppliers with lenders using verified on-chain data.
Neither TradeLens nor Marco Polo achieved this bridge.

**7. MSME-accessible**

Designed for India's 63M+ MSMEs, not just large corporates. Zero-infrastructure
onboarding via managed identities. QR-code-based verification for quick
document lookups by customs house agents, inspectors, and logistics providers.
Mobile-first interfaces for factory-floor data entry.

**8. India supplier intelligence**

Proprietary dataset from 200+ engagements: actual vs. quoted lead times, defect
rates, communication responsiveness. This feeds the compliance and analytics
layer and cannot be replicated by a new entrant without years of operational
work in the same trade corridors.

**9. IoT-ready architecture**

The chaincode and FireFly API layer are designed so that IoT sensors on the
factory floor can write directly to the trade channel using their own Fabric CA
identities — same schema, same validation, different signing identity. When
Indian manufacturing facilities adopt IoT (an ongoing trend), MerkNet is ready
without architectural changes.

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
- **Channels and PDCs** — three channels (network, trade, financing) with
  Private Data Collections for per-deal encryption. Provides complete data
  isolation without channel proliferation.
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
not use it). Every participant that runs a peer also runs an IPFS node: banks
pin financial documents, government pins compliance documents, buyers and
sellers pin trade documents.

- Content addressing — each document's CID is derived from its hash, providing
  built-in integrity verification
- Pinning as CDN — pinned content is hot (fast, available across peers).
  Unpinned content remains accessible via cold retrieval. Data is always
  available; pinning determines speed.
- Envelope encryption — documents are stored encrypted on IPFS, only deal
  parties with the deal key can decrypt
- **UCANs** (User Controlled Authorization Networks) — capability-based
  authorization tied to participant identities. UCANs control who can pin,
  access, and delegate access to specific documents. Issued by the deal creator
  to deal parties, with delegation chains (e.g., supplier delegates inspection
  report access to their inspector). No central authorization server — the UCAN
  token itself is the proof of capability.

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
| 2 | DGFT | Export licensing, compliance | Orderer + peer + FireFly + IPFS |
| 3 | Customs (CBIC) | Clearance, filing | Orderer + peer + FireFly + IPFS |
| 4 | Suppliers | Component manufacturers | Managed or own peer + IPFS |
| 5 | Buyers | Procurement entities | Managed or own peer + IPFS |
| 6 | Quality Inspectors | Third-party or Merkantis-affiliated | Managed identity |
| 7 | Logistics | Freight, shipping (Phase 2+) | Own peer + IPFS |
| 8 | Banks | Supply chain finance, PO/invoice verification (Phase 2+) | Orderer + peer + FireFly + IPFS |

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

- `role`: buyer, supplier, inspector, customs_officer, logistics, bank_officer, merkantis_admin
- `org`: organization MSP ID
- `certifications`: ISO9001-auditor, DGFT-authorized, CBIC-officer, etc.

**Chaincode access control examples:**

- `SubmitRFQ` — requires `role=buyer`
- `RespondToRFQ` — requires `role=supplier` or `role=merkantis_admin`
- `RecordInspection` — requires `role=inspector` + relevant `certifications`
- `FileCustomsDeclaration` — requires `role=customs_officer`
- `ConfirmDelivery` — requires `role=buyer` + membership in the deal's PDC
- `UpdateReputation` — system-only, triggered by deal completion event
- `RequestFinancing` — requires `role=supplier` or `role=merkantis_admin`
- `VerifyDocument` — requires `role=bank_officer` (hash check on network channel)

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

Seven chaincode packages deployed across the three channels.

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

Aggregates performance metrics and records milestone history.

```
Milestone history writes:
  Key deal events are written to the network channel as
  hashes (no amounts or commercial details):
  - PO_CONFIRMED: hash of PO document
  - DELIVERY_COMPLETE: hash of delivery confirmation
  - INVOICE_PAID: hash of payment record
  These hashes allow banks and other participants to
  verify document legitimacy without accessing the
  trade channel.

Reputation inputs (from deal completion events):
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

**7. SupplyChainFinance** (financing channel)

Manages loan lifecycle for supply chain finance. Aligned with India's OCEN
(Open Credit Enablement Network) framework — Merkantis acts as a Loan
Service Provider connecting suppliers with bank lenders using verified
on-chain data.

```
States:
  FINANCING_REQUESTED
  → VERIFIED (bank hashes submitted PO/invoice,
    checks against network channel milestone history)
  → TERMS_OFFERED (bank proposes loan terms)
  → TERMS_ACCEPTED (supplier accepts)
  → DISBURSED (loan amount released)
  → REPAID | DEFAULTED

The bank monitors the network channel for deal
milestones as risk signals — but these are
informational, not triggers. The loan lifecycle is
independent of payment tracking on the trade channel.
When buyer pays supplier (visible as a milestone hash
on network channel), the bank knows the supplier has
funds to repay, but repayment terms and schedule are
governed by the loan agreement on this channel.
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

**Envelope encryption + UCANs — the two-part privacy model**

Confidentiality (who can read) is handled by envelope encryption.
Authorization (who can pin, access, delegate on IPFS) is handled by UCANs.

**Envelope encryption:**

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

**UCANs (User Controlled Authorization Networks):**

- Deal creator issues UCAN tokens to deal parties, authorizing them to pin and
  access specific IPFS content
- Delegation chains: a supplier can delegate a sub-capability to their
  inspector (e.g., read-only access to inspection reports for a specific deal)
- No central authorization server — the UCAN token itself is cryptographic
  proof of capability, verifiable by any IPFS node
- Pinned content = hot (CDN-like, fast retrieval across peers). Unpinned =
  cold (still accessible, slower retrieval). Data is always available;
  pinning determines speed.

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
| Network channel | Milestone hashes (no amounts/docs) | Deal details, document contents |
| PDC | Encrypted blob on all peers | Plaintext (only deal parties decrypt) |
| IPFS | Encrypted documents exist | Content (envelope encryption) + access (UCANs) |
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

The network uses three channels (network, trade, financing). This keeps channel
management overhead constant regardless of deal volume. PDCs scale cheaply —
they are access-controlled data entries within a channel, not separate ledgers.

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
| Government node | 1 orderer + 1 peer + CouchDB + FireFly + IPFS | 4 cores | 16 GB | 200 GB SSD | $80-150 |
| Bank node | 1 orderer + 1 peer + CouchDB + FireFly + IPFS | 4 cores | 16 GB | 200 GB SSD | $80-150 |
| Self-sovereign entity | 1 peer + CouchDB + FireFly + IPFS | 2 cores | 8 GB | 100 GB SSD | $40-80 |
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

Document verification (hash lookup against the ledger) is sub-second. QR-code
verification — scanning a QR to check a document's on-chain status — is the
same hash lookup, sub-second, and works in low-connectivity environments
(factory floors, port areas) since it's a lightweight query against the local
peer. IPFS retrieval of encrypted documents depends on network conditions but is
typically under 5 seconds for pinned content.

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
- **Supply chain finance facilitation**: Merkantis as OCEN-aligned Loan Service
  Provider — banks pay per-verification or subscription fee for access to
  on-chain PO/invoice verification via the financing channel
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
- **Indian banks (SBI, ICICI)** — financing channel partners, orderer nodes (Phase 2+)
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
- **Advanced trade finance**: LCs, bank guarantees, and multi-bank syndication
  as on-chain workflows. The financing channel provides the foundation; advanced
  instruments build on top.
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
