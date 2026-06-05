# Enterprise Zero Trust Identity Lab (500+ Users)
## 📌 Project Overview
This repository contains the full architecture, automation scripts, and governance policies for a production-ready *Zero Trust Identity Infrastructure* designed for a hybrid enterprise of 500+ users (*CloudVanguard Technologies). Built entirely within **Microsoft Entra ID*, this lab eliminates manual overhead through advanced automation and enforces strict cryptographic and behavioral access controls.

---

## 🛠️ Key Architectural Pillars

### 1. Automated Lifecycle Management & Governance
* *HR-Driven Provisioning:* Automated onboarding and offboarding using *Power Automate* and *Microsoft Graph API*.
* *Dynamic Group Architecture:* Zero-touch user assignment based on multi-value department and location attributes.
* *Entra ID Governance:* Automated Access Reviews enforced by *Microsoft Security Copilot* analytics to mitigate entitlement creep.

### 2. Adaptive Access Control (Zero Trust)
* *Risk-Based Conditional Access:* Real-time evaluation of User Risk and Sign-in Risk scores.
* *Phishing-Resistant MFA:* Enforcement of FIDO2 security keys and passwordless Microsoft Authenticator with number matching.
* *Privileged Identity Management (PIM):* Just-In-Time (JIT) activation for high-privilege roles (Global Admin, Security Admin) with automated approval workflows routed through Power Automate.

### 3. Enterprise Application SSO & Federation 🌟
* *Centralized Identity Provider (IdP):* Implementation of SAML 2.0 and OpenID Connect (OIDC) federated Single Sign-On (SSO) for critical SaaS applications (e.g., Slack, Salesforce, Zoom).
* *Automated App Provisioning:* SCIM-based user lifecycle synchronization directly from Entra ID to external enterprise apps.
* *Conditional Access for SaaS:* Enforcing specialized security barriers (e.g., blocking external storage downloads on unmanaged devices) specifically for non-Microsoft enterprise apps.

---

```mermaid
graph LR
    %% Professional Enterprise Styling
    classDef script fill:#EAF2F8,stroke:#2980B9,stroke-width:2px,font-family:'Segoe UI';
    classDef tenant fill:#EBDEF0,stroke:#8E44AD,stroke-width:3px,font-family:'Segoe UI';
    classDef group fill:#E8F8F5,stroke:#1ABC9C,stroke-width:2px,font-family:'Segoe UI';
    classDef policy fill:#FEF9E7,stroke:#F1C40F,stroke-width:2px,font-family:'Segoe UI';
    classDef apps fill:#FBEEE6,stroke:#E67E22,stroke-width:2px,font-family:'Segoe UI';

    %% 1. Automation Layer (Left)
    Script["🖥️ PowerShell Automation Script<br><b>[Bulk_Import_Script.ps1]</b>"]:::script

    %% 2. Identity Provider Core (Center)
    Tenant["🔮 Microsoft Entra ID Tenant<br><b>(Identity Provider)</b>"]:::tenant

    %% 3. Target Security Groups (Vertical Alignment)
    subgraph Security_Groups ["📁 Dynamic Security Groups"]
        G_IT["👥 IT-Dept Dynamic Group"]:::group
        G_HR["👥 HR-Dept Group"]:::group
        G_ENG["👥 Engineering-Dept Group"]:::group
        G_SALES["👥 Sales-Dept Group"]:::group
        G_FIN["👥 Finance-Dept Group"]:::group
    end

    %% 4. Zero Trust Control Gates
    CA["🛡️ Conditional Access Policy<br><b>[MFA & Location Check]</b>"]:::policy
    PIM["🔑 Privileged Identity Management<br><b>[PIM / Just-In-Time]</b>"]:::policy

    %% 5. Target Cloud Applications (Right)
    CloudApps["☁️ Enterprise Cloud Applications<br><b>[Office 365 / SaaS Apps / SSO]</b>"]:::apps

    %% --- Architectural Relationships & Flow ---
    
    %% User Provisioning Flow
    Script --> |"Graph API / Bulk Import"| Tenant

    %% Internal Tenant Hierarchy
    Tenant -.-> G_IT
    Tenant -.-> G_HR
    Tenant -.-> G_ENG
    Tenant -.-> G_SALES
    Tenant -.-> G_FIN

    %% Security Enforcement Gates
    CA ==> Tenant
    PIM ==> Tenant

    %% Conditional Access Routing
    CA --> |"🔒 Enforce MFA (Verified)"| CloudApps
    PIM --> |"⏳ JIT Activation (Approved)"| CloudApps
