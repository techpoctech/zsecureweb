ZsecureWeb & CaiberPolice
## System Architecture
ZsecureWeb utilizes a zero-trust multi-process architecture. The rendering and user-facing browser shell run in an isolated user-space process, while the ZTNA and DLP policy engines operate within a completely independent, high-privilege daemon process. Communication between the boundaries is managed via hardware-attested, cryptographically signed local IPC channels, ensuring that an engine-level compromise cannot alter or bypass enterprise compliance hooks.

ZsecureWeb is built with a dual-mode deployment architecture tailored to customer policy. On managed corporate assets, the security layer deploys as an OS-level system service running under elevated privileges, preventing unhooking or process termination by standard users. For BYOD or contractor endpoints, it downgrades to a local user-space background daemon. The browser handles life-cycle orchestration and enforces mutual watchdog monitoring alongside cryptographic policy attestation to prevent local configuration tampering and downgrade attacks.

![CaiberPolice and ZSecureWeb Architecture](docs/assets/main_arch.png)

ZsecureWeb is available under two distinct licenses:

Open Source (AGPLv3): Free for community, personal, and open-source use under the terms of the GNU Affero General Public License v3.0. Any network-hosted modifications or derivative works must be made publicly available under AGPLv3.

Commercial License: For enterprises seeking to embed, modify, or deploy ZsecureWeb and CaiberPolice without the copyleft obligations of AGPLv3. Commercial licenses include enterprise SLAs, dedicated support, and custom deployment options.

To inquire about commercial licensing, contact: golengeeks@gmail.com

🛡 Architectural Overview: The 2-Component SASE Model
ZsecureWeb replaces legacy hairpinned proxies (Zscaler, Netskope, Palo Alto Networks) with a unified, local-first architecture built on a clean 2-component design:

CaiberPolice (caiberd - Native OS Daemon): Handles low-level system enforcement, including system health monitoring, TPM 2.0 / Apple Secure Enclave hardware attestation, and eBPF-based network packet filtering/routing.

ZsecureWeb (Enterprise Browser Enclave): Forked Chromium baseline (techpoctech/chromium) integrated with zsecureweb-core, handling application-layer security such as DOM-level Data Loss Prevention (DLP), real-time AI prompt inspection, and Remote Browser Isolation (RBI) streaming.

```text
zsecureweb/
├── .gitmodules               # Submodule & path routing definitions
├── version.json              # Single source of truth for toolchain & pins
├── caiberpolice/             # Component 1: Native OS Daemon & Posture Engine
├── tools/                    # Build engine, setup scripts & automation
└── thirdParty/
    └── chromium/             # Component 2: Chromium browser baseline
        └── src/
            └── zsecureweb/   # Core DLP C++ module (zsecureweb-core)
```
            
💻 System Prerequisites
Before initializing the workspace, ensure your host environment meets the baseline requirements:



OS: Linux (Ubuntu 22.04 LTS recommended), macOS, or Windows 10/11 (WSL2/Native)

Hardware: x86-64 machine, minimum 16 GB RAM (32 GB+ recommended), and ≥100 GB free disk space

Dependencies: git, python3 (v3.9+), curl

🚀 Quick Start & Environment Setup
The repository uses an automated orchestration engine to handle workspace synchronization, dependency pinning, and submodule routing deterministically.

1. Clone and Set Up Workspace
Clone the parent orchestrator repository and run the automation script:

Bash
git clone https://github.com/techpoctech/zsecureweb.git
cd zsecureweb
python3 tools/automate.py
(This automatically configures gclient, synchronizes submodules, locks dependencies via version.json, and prepares the Chromium tree.)

2. Build the System
Build the Enterprise Browser:

Bash
python3 tools/build.py
Build the CaiberPolice OS Daemon (caiberd):

Bash
cd caiberpolice
mkdir -p build && cd build
cmake .. && cmake --build . --config Release
🧹 Maintenance & Updating
If repository configurations or submodules are updated upstream, re-synchronize your local workspace:

Bash
git pull origin main
python3 tools/automate.py
Copyright (C) 2026 ZsecureWeb Contributors. All product names, logos, and brands are property of their respective owners.
