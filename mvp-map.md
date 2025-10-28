```
PORTABLEID - PROJECT TIMELINE (Oct 28 - Nov 18, 2025)
═══════════════════════════════════════════════════════════════════════════════

WEEK 1: FOUNDATION & SETUP (Oct 28 - Nov 3)
─────────────────────────────────────────────────────────────────────────────
  MON 28  TUE 29  WED 30  THU 31  FRI 1   SAT 2   SUN 3
  ├─────────────────────────────────────────────────────────┤
  
  ✓ GitHub Organization Setup
    ├─ Create remaining repos (smart-contracts, sdk, docs, tests, devops, 
    │  design-system, parachain, schemas, security, audit-logs)
    └─ Set branch protection & CI/CD hooks
  
  ✓ Team Onboarding
    ├─ Assign roles & responsibilities
    ├─ Set up dev environments
    └─ First team sync
  
  ✓ Infrastructure Setup
    ├─ Docker & Kubernetes config templates
    ├─ Prometheus + Grafana monitoring stack
    └─ Database (Postgres) init scripts

  🎯 DELIVERABLE: All repos created, team ready, dev env working


WEEK 2: DESIGN & SPECIFICATION (Nov 4 - Nov 10)
─────────────────────────────────────────────────────────────────────────────
  MON 4   TUE 5   WED 6   THU 7   FRI 8   SAT 9   SUN 10
  ├─────────────────────────────────────────────────────────┤
  
  ✓ Protocol Specification
    ├─ DID method spec & data models
    ├─ Credential schema definitions (3 initial: ID, Medical, Education)
    ├─ Revocation model (Merkle tree structure)
    ├─ API specs (OpenAPI/Swagger for all endpoints)
    └─ XCM message definitions
  
  ✓ UX/Design
    ├─ Wallet wireframes (DID creation, credential display, presentation)
    ├─ Issuer portal UI mockups
    ├─ Verifier app flow
    └─ Design system components library
  
  ✓ Database Schema Design
    ├─ User credentials schema
    ├─ Issuer registry schema
    ├─ Audit logs schema
    └─ Revocation tracking schema

  🎯 DELIVERABLE: All specs documented, designs approved, DB schemas finalized


WEEK 3: PARACHAIN & SMART CONTRACTS (Nov 11 - Nov 17)
─────────────────────────────────────────────────────────────────────────────
  MON 11  TUE 12  WED 13  THU 14  FRI 15  SAT 16  SUN 17
  ├─────────────────────────────────────────────────────────┤
  
  ✓ Parachain Bootstrap
    ├─ Substrate node template setup
    ├─ Custom pallets scaffolding:
    │  ├─ DID management pallet
    │  ├─ Issuer registry pallet
    │  ├─ Revocation pallet
    │  └─ Governance pallet
    └─ Local testnet running
  
  ✓ Smart Contracts Implementation
    ├─ DID anchoring logic
    ├─ Issuer registration & approval flow
    ├─ Revocation Merkle tree implementation
    ├─ Status check endpoints
    └─ Unit tests for all pallets
  
  ✓ Backend APIs (Issuer Services)
    ├─ Authentication & authorization
    ├─ issueCredential endpoint
    ├─ revokeCredential endpoint
    ├─ getSchema endpoint
    ├─ checkStatus endpoint
    └─ Admin API for issuer management

  🎯 DELIVERABLE: Parachain testnet live, core smart contracts deployed, APIs functional


WEEK 4: MOBILE & WEB (Nov 18)
─────────────────────────────────────────────────────────────────────────────
  MON 18  [DEADLINE]
  
  ✓ Mobile Wallet (iOS/Android)
    ├─ DID creation & storage (encrypted local storage)
    ├─ Credential display & management
    ├─ QR code generation for presentations
    ├─ Offline credential display
    └─ Basic key recovery mechanism
  
  ✓ Web Portal (Issuer Dashboard)
    ├─ Issuer registration flow
    ├─ Credential issuance interface
    ├─ Revocation management
    ├─ Audit logs viewer
    └─ Schema management UI
  
  ✓ Verifier App
    ├─ QR code scanner
    ├─ Credential verification (offline)
    ├─ Status check (online)
    └─ Verification result display
  
  ✓ SDKs & Documentation
    ├─ Mobile SDK (iOS/Android/React Native)
    ├─ Issuer REST API SDK
    ├─ Verifier SDK
    ├─ Complete API documentation
    └─ Integration guides
  
  ✓ Testing & QA
    ├─ End-to-end testing
    ├─ Offline verification tests
    ├─ Cross-platform compatibility
    └─ Performance benchmarks

  🎯 DELIVERABLE: Full MVP ready for pilot launch


═══════════════════════════════════════════════════════════════════════════════
CRITICAL PATH DEPENDENCIES
═══════════════════════════════════════════════════════════════════════════════

Foundation Setup (Week 1)
        ↓
        ├──→ Protocol Spec (Week 2) ──→ Smart Contracts (Week 3)
        │
        └──→ UX/Design (Week 2) ──→ Mobile & Web (Week 4)

Database Schema (Week 2) ──→ Backend APIs (Week 3) ──→ Integration (Week 4)


═══════════════════════════════════════════════════════════════════════════════
KEY MILESTONES & BLOCKERS
═══════════════════════════════════════════════════════════════════════════════

BLOCKER 1: Team not fully assembled by Oct 28
  └─ MITIGATION: Use contractors/consultants for missing roles

BLOCKER 2: Spec delays (causes downstream cascades)
  └─ MITIGATION: Prioritize DID method & credential schema first

BLOCKER 3: Substrate/pallet compilation issues
  └─ MITIGATION: Start with template pallets, customize incrementally

BLOCKER 4: Crypto library integration (BBS+/ZK proofs)
  └─ MITIGATION: Use existing libraries (libsodium, arkworks); avoid custom crypto

BLOCKER 5: Mobile development platform setup
  └─ MITIGATION: Parallel track with React Native to unblock both iOS/Android


═══════════════════════════════════════════════════════════════════════════════
DAILY STAND-UP TOPICS
═══════════════════════════════════════════════════════════════════════════════

→ What got done yesterday?
→ What's blocking progress?
→ What's the plan for today?
→ Are we on track for weekly deliverable?


═══════════════════════════════════════════════════════════════════════════════
NOV 18 LAUNCH READINESS CHECKLIST
═══════════════════════════════════════════════════════════════════════════════

✓ All repos created & populated
✓ Protocol specs finalized & documented
✓ Parachain testnet running with all pallets
✓ Backend APIs fully functional & tested
✓ Mobile wallet functional (both platforms)
✓ Issuer portal usable end-to-end
✓ Verifier app working (QR + offline verification)
✓ SDKs published with examples
✓ Documentation complete
✓ Basic security review done
✓ CI/CD pipelines green
✓ Team trained & ready for pilot

═══════════════════════════════════════════════════════════════════════════════
```