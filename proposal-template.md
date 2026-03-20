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

The ordering service is the shared consensus layer across all
channels. Orderer nodes sequence transactions into blocks but
do not store ledger data or execute chaincode. They are blind
relays with BFT consensus.

Orderer distribution:

- Phase 1 (MVP): Merkantis (2 nodes) + DGFT (1) + Customs (1)
  = 4 orderers, F=1
- Phase 2: Add large buyer/supplier orgs = 7 orderers, F=2
- Phase 3: Further distribution across trade partners

**Peer Nodes**

Each participating org runs one or more peer nodes. Peers join
channels, store the channel ledger in CouchDB, and execute
chaincode (endorsement). A single peer can participate in
multiple channels.

- Merkantis: 2 peers (HA) + FireFly + IPFS node
- Government (DGFT, Customs): 1 peer + FireFly
- Large suppliers/buyers (Phase 2+): 1 peer + optional FireFly
- Small suppliers/buyers: no own infrastructure, managed
  identity through Merkantis

### Channel Architecture

The network uses two Fabric channels. Privacy within channels
is handled by Private Data Collections (PDCs) with envelope
encryption, not by channel proliferation.

**1. Network Channel**

All participants are members. Stores:

- Identity registry (public keys, org metadata, role
  attributes)
- Reputation chaincode (aggregated scores from completed deals)
- Compliance rule definitions (DGFT export policies,
  destination country regs)

Read-heavy, write on deal completion or identity events.

**2. Trade Channel**

All active trading participants are members. All deal lifecycle
transactions occur here. Privacy is enforced through a
combination of PDCs and envelope encryption — the channel
ledger contains only hashes and encrypted blobs, never
plaintext commercial data.

### Privacy Layer — PDCs with Envelope Encryption

A single PDC named `tradedata` is defined on the trade channel
with broad membership (all trade channel participants can
store data). Access control is cryptographic, not
infrastructural.

**How it works:**

1. Deal creator generates a random AES-256 symmetric key (the
   "deal key")
2. Structured deal data (terms, pricing, milestones) is
   encrypted with the deal key
3. The deal key is encrypted separately with each group
   member's public key (ECIES — Elliptic Curve Integrated
   Encryption Scheme)
4. The PDC stores the encrypted payload + per-member encrypted
   key shares
5. The Fabric public ledger stores only: deal ID, hash of
   plaintext data, status, and member list

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
      "BuyerAMSP":    "<deal key encrypted w/ BuyerA pubkey>",
      "SupplierXMSP": "<deal key encrypted w/ SupplierX pubkey>",
      "MerkantisMSP": "<deal key encrypted w/ Merkantis pubkey>"
    }}

IPFS (encrypted, pinned by deal parties):
  encrypted(PO.pdf), encrypted(COO.pdf),
  encrypted(inspection_report.pdf)
  — same deal key, same envelope pattern
```

Adding a new party (e.g., customs) to a deal: encrypt the
deal key with their public key and append to the encrypted_keys
map. Revoking access: re-encrypt with a new deal key and
redistribute to remaining members.

Documents (PDFs, images, test reports) follow the same
envelope encryption pattern but are stored on IPFS rather
than in the PDC. The Fabric ledger stores the IPFS CID
alongside the hash of the unencrypted document for
verification.

### Identity and Access Control

Fabric CA issues X.509 certificates with embedded role
attributes. Chaincode enforces Attribute-Based Access Control
(ABAC) on every operation.

Example attributes:

- `role`: buyer, supplier, inspector, customs_officer,
  merkantis_admin
- `org`: organization MSP ID
- `certifications`: ISO9001-auditor, DGFT-authorized, etc.

Chaincode checks attributes before allowing operations:

- `SubmitRFQ` — requires `role=buyer`
- `ApproveInspection` — requires `role=inspector` +
  `certifications` containing the relevant standard
- `FileCustomsDeclaration` — requires `role=customs_officer`
- `UpdateReputation` — requires deal completion event,
  called by system chaincode only

**Progressive identity model:**

- New buyers/suppliers get a managed identity — Merkantis
  holds their Fabric CA credentials and signs on their behalf.
  Every platform interaction is still a Fabric transaction
  attributed to the managed identity.
- After first successful deal, the party is provisioned their
  own Fabric peer and CA credentials. Merkantis migrates their
  transaction history.
- Managed → self-sovereign transition mirrors the business
  model: Merkantis as principal (Phase 1) → Merkantis as
  platform (Phase 2+).

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
  Merkantis: 2 orderers + 2 peers + FireFly + IPFS
  DGFT/Customs: 1 orderer + 1 peer
  All others: managed identities via Merkantis
  Resources: ~10 cores, 20 GB RAM, 500 GB SSD

Phase 2 (growth):
  + Large suppliers/buyers: own peer + optional FireFly
  + Additional government orderers
  Orderer set expands to 7 (F=2)
  Merkantis: ~20 cores, 40 GB RAM, 1 TB SSD

Phase 3 (mature network):
  Each major org: peer + FireFly + IPFS
  Orderers distributed across 7+ independent orgs
  Merkantis transitions from principal to platform operator
```

## Benchmarking and Differentiation

<!-- 
  - Compare the proposed solution with existing solutions.
  - Highlight the differentiation factors, such as unique features, improved efficiency, or 
    enhanced security.
-->

_todo_

## Non-Crypto Blockchain Platform

<!-- 
  - Specify the non-crypto blockchain platform planned for development (e.g., Hyperledger Fabric, 
    Corda).
  - Explain how the chosen platform effectively addresses the identified problem.
-->

_todo_

## Permissioned Blockchain and Smart Contracts

<!-- 
  - Describe the use of permissioned blockchain and smart contracts in the proposed solution.
  - Explain how these features enhance the solution's functionality and security.
-->

_todo_

## Regulatory Framework

<!-- 
  - Discuss how the proposed solution complies with the regulatory framework of the domain.
  - Highlight any relevant laws, regulations, or standards.
-->

_todo_

## Security, Privacy,Scalability and Performance

<!-- 
  - Explain how the proposed solution addresses security, privacy,scalability and performance 
    requirements.
  - Describe the measures taken to ensure the solution's robustness and reliability.
-->

_todo_

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
  - Provide any additional details relevant to the idea, such as market analysis, user-adoption 
    strategies, or potential partnerships.
-->

_todo_

## Signature and Team Information

<!-- 
  - Include the signatures of the startup founder and team members.
  - Provide contact information and relevant details about the team members.
-->

_todo_
