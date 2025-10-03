```mermaid
---
config:
  layout: elk
  look: handDrawn
  theme: dark
  themeVariables:
    background: "#0b1220"
---
graph TB
    subgraph NORMAL["✅ NORMAL TEE OPERATION - Secure & Isolated"]
        direction TB
        A1[👤 User Application<br/>Needs Secret Processing] -->|1. Send Encrypted Data| B1[🔐 TEE Interface]
        B1 -->|2. Enter Secure Zone| C1[🏰 TEE Secure Enclave<br/>Hardware Protected]
        C1 -->|3. Store in Protected RAM| D1[🛡️ Encrypted Memory<br/>Address: 0x1000-0x2000]
        D1 -->|4. Process Secretly| C1
        C1 -->|5. Return Encrypted Result| B1
        B1 -->|6. Deliver to User| A1

        F1[🚫 Operating System] -.->|❌ Blocked| C1
        G1[🚫 Other Apps] -.->|❌ Blocked| C1
        H1[🚫 Admin/Root] -.->|❌ Blocked| C1

        C1 -->|Attestation Report| I1[✅ Verified Genuine TEE<br/>No Tampering Detected]

        style C1 fill:#2d5016,stroke:#4ade80,stroke-width:4px,color:#fff
        style D1 fill:#1e3a8a,stroke:#60a5fa,stroke-width:3px,color:#fff
        style I1 fill:#065f46,stroke:#34d399,stroke-width:2px,color:#fff
        style F1 fill:#7f1d1d,stroke:#ef4444,stroke-dasharray: 5 5,color:#fff
        style G1 fill:#7f1d1d,stroke:#ef4444,stroke-dasharray: 5 5,color:#fff
        style H1 fill:#7f1d1d,stroke:#ef4444,stroke-dasharray: 5 5,color:#fff
    end

    subgraph BADRAM["🚨 BadRAM ATTACK - Memory Aliasing Exploit"]
        direction TB
        A2[👤 Victim TEE App<br/>Storing Secrets] -->|Write to 0x8000| B2[💾 Modified RAM Module<br/>SPD Says: 64GB<br/>Reality: 32GB]
        A3[😈 Attacker App] -->|Read from 0x1000| B2

        B2 -->|Maps 0x8000 → Physical 0x1000| C2[⚠️ Same Physical Location!]
        B2 -->|Maps 0x1000 → Physical 0x1000| C2

        C2 -->|Contains: Private Keys<br/>Credit Cards<br/>Passwords| D2[📤 Data Leaked to Attacker]

        E2[🔧 Attack Setup<br/>Cost: $10] -.->|1. Modify SPD Chip| B2
        F2[💻 Raspberry Pi +<br/>Flashing Tools] -.->|2. Reprogram Memory Size| E2

        G2[✅ System Reports:<br/>All Security OK] -->|False Sense of Security| A2

        style B2 fill:#7f1d1d,stroke:#ef4444,stroke-width:4px,color:#fff
        style C2 fill:#991b1b,stroke:#fca5a5,stroke-width:4px,color:#fff
        style D2 fill:#7c2d12,stroke:#fb923c,stroke-width:3px,color:#fff
        style E2 fill:#431407,stroke:#fdba74,stroke-width:2px,color:#fff
        style F2 fill:#1e3a8a,stroke:#60a5fa,stroke-width:2px,color:#fff
        style G2 fill:#065f46,stroke:#34d399,stroke-width:2px,color:#fff
    end

    subgraph VOLTSCHEMER["⚡ VoltSchemer ATTACK - Voltage Manipulation"]
        direction TB
        A4[📱 Target Device with TEE<br/>Intel SGX or TDX]
        B4[🔌 Power Supply] -->|Normal Voltage 5V| A4

        C4[😈 Attacker Controls<br/>USB Charger or Dock] -->|Inject Malicious Signals| B4
        C4 -->|Voltage Glitching| D4[⚡ Rapid Voltage Changes<br/>Cause Bit Flips]

        D4 -->|Corrupt TEE Memory| E4[🏰 TEE Secure Enclave<br/>Under Attack]
        E4 -->|Bit Flip in Security Check| F4[🚫 Bypass Attestation]
        E4 -->|Corrupt Encryption Keys| G4[🔓 Access Protected Data]
        E4 -->|Timing Attack| H4[📊 Extract Secrets via<br/>Power Analysis]

        F4 --> I4[💀 Complete TEE Compromise]
        G4 --> I4
        H4 --> I4

        J4[🎯 Attack Vector:<br/>Malicious Charger<br/>Evil USB-C Dock<br/>Compromised Power] -.->|Physical Connection| C4

        K4[⏱️ Attack Duration:<br/>Seconds to Minutes] -.->|Fast and Stealthy| D4

        style C4 fill:#7f1d1d,stroke:#ef4444,stroke-width:4px,color:#fff
        style D4 fill:#991b1b,stroke:#fca5a5,stroke-width:4px,color:#fff
        style E4 fill:#7c2d12,stroke:#fb923c,stroke-width:3px,color:#fff
        style I4 fill:#450a0a,stroke:#fca5a5,stroke-width:4px,color:#fff
        style J4 fill:#431407,stroke:#fdba74,stroke-width:2px,color:#fff
        style K4 fill:#1e3a8a,stroke:#60a5fa,stroke-width:2px,color:#fff
    end
