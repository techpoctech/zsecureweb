CaiberPolice (caiberd)
CaiberPolice is a high-performance, native endpoint SASE (Secure Access Service Edge) security agent and policy enforcement daemon. It acts as the local trust anchor and guardian for enterprise browsing environments, seamlessly integrating zero-trust networking, data loss prevention, and threat isolation directly at the OS level.

Architecture Overview
CaiberPolice operates as a background system daemon (caiberd), communicating via secure local IPC channels to enforce real-time security policies across multiple domains:

caiberpolice/
├── CMakeLists.txt
├── include/
│   ├── daemon_core.hpp
│   ├── itpm_provider.hpp
│   ├── iipc_server.hpp
│   ├── swg/                # Web Gateway & DLP Engine
│   ├── ztna/               # Zero Trust Private Access & Posture
│   ├── casb/               # SaaS Governance & Data Security
│   ├── rbi/                # Remote Browser Isolation Handler
│   └── fwaas/              # Layer 4 Network Firewall & Posture
└── src/
    ├── main.cpp
    ├── daemon_core.cpp
    ├── platform/           # TPM 2.0 & Apple Secure Enclave hooks
    └── ipc/                # Asynchronous IPC Server
Core SASE Modules
SWG (Secure Web Gateway & DLP): Intercepts web requests, scans outbound traffic, and prevents corporate data leakage.

ZTNA (Zero Trust Network Access): Evaluates device posture and manages secure tunnels to internal private applications.

CASB (Cloud Access Security Broker): Monitors and governs data interactions with authorized and unauthorized SaaS platforms.

RBI (Remote Browser Isolation): Offloads high-risk browsing sessions to isolated remote execution containers.

FWaaS (Firewall-as-a-Service): Enforces Layer 4 network filtering and stateful packet inspection at the endpoint.

Hardware Root-of-Trust
CaiberPolice ties its operational integrity directly to hardware security modules:

Linux: Integrates with TPM 2.0 chips for cryptographic attestation and boot integrity validation.

macOS: Utilizes the Apple Secure Enclave for secure key storage and hardware-backed device posture verification.
