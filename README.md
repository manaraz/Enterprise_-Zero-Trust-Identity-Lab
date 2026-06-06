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
## 🏗️ Enterprise Infrastructure Blueprint

To architect an automated, scalable, and secure identity infrastructure, this project implements a defense-in-depth architecture aligned with the Zero Trust framework and Microsoft Entra ID best practices. 

The structural blueprint below maps out the production-ready identity lifecycle management (ILM), attribute-based access control (ABAC), and the exact authentication flow enforced across the 500-employee enterprise lab:

```mermaid
graph TD
    %% Advanced Enterprise Styling
    classDef source fill:#1F4E79,stroke:#002060,stroke-width:2px,font-color:#FFFFFF,font-family:'Segoe UI';
    classDef validation fill:#7F7F7F,stroke:#3B3B3B,stroke-width:2px,font-color:#FFFFFF,font-family:'Segoe UI';
    classDef automation fill:#2F5597,stroke:#1F3864,stroke-width:2px,font-color:#FFFFFF,font-family:'Segoe UI';
    classDef tenant fill:#7030A0,stroke:#3B1754,stroke-width:2px,font-color:#FFFFFF,font-family:'Segoe UI';
    classDef userObj fill:#8FAADC,stroke:#2F5597,stroke-width:2px,font-color:#1F3864,font-family:'Segoe UI';
    classDef group fill:#A9D18E,stroke:#385723,stroke-width:2px,font-color:#385723,font-family:'Segoe UI';
    classDef engine fill:#F8CBAD,stroke:#C65911,stroke-width:2px,font-color:#C65911,font-family:'Segoe UI';
    classDef compliance fill:#E2EFDA,stroke:#375623,stroke-width:2px,font-color:#375623,font-family:'Segoe UI';
    classDef apps fill:#FCE4D6,stroke:#C65911,stroke-width:2px,font-color:#C65911,font-family:'Segoe UI';

    %% 1. Identity Source & Validation Layer
    HR_Data["📋 Enterprise HR Data<br><small>500-employee roster • joiner / mover / leaver events</small>"]:::source
    Validation["⚙️ Attribute Validation Layer<br><small>Rejects missing: dept • jobTitle • location</small>"]:::validation
    Script["🖥️ PowerShell Automation<br><small>Graph API /users • bulk provisioning</small>"]:::automation

    %% 2. Core Tenant Boundary
    subgraph Entra_Tenant ["🔮 Microsoft Entra ID Tenant Boundary"]
        Users["👤 User Objects<br><small>Department • JobTitle • officeLocation</small>"]:::userObj
        Intune["📱 Intune Compliance<br><small>Device Health & Signals</small>"]:::compliance
        
        %% Dynamic Attribute Security Groups
        subgraph Dynamic_Groups ["📁 Attribute-Driven Dynamic Security Groups (~5 min re-evaluation latency)"]
            G_IT["👥 IT Dept<br><small>dept -eq 'IT'<br>Admin JIT Req.</small>"]:::group
            G_HR["👥 HR Dept<br><small>dept -eq 'HR'</small>"]:::group
            G_ENG["👥 Engineering<br><small>dept -eq 'Eng'</small>"]:::group
            G_SALES["👥 Sales<br><small>dept -eq 'Sales'</small>"]:::group
            G_FIN["👥 Finance<br><small>dept -eq 'Finance'</small>"]:::group
        end

        %% Security & Governance Engines
        subgraph Security_Engines ["🛡️ Security and Governance Engines"]
            CA["🔐 Conditional Access<br><small>User risk • Sign-in risk<br>MFA • Named location<br>Device compliance<br>Issues access token</small>"]:::engine
            PIM["🔑 PIM<br><small>Just-in-time role activation<br>Max 4 hr window<br>Approval via Power Automate<br>New token issued on elevation</small>"]:::engine
        end
    end

    %% 3. Target Cloud Applications
    CloudApps["☁️ Enterprise Cloud Applications<br><small>Office 365 • Salesforce • Slack • Zoom • SAML 2.0 / OIDC • SCIM</small>"]:::apps

    %% --- Identity Flows ---
    HR_Data --> Validation
    Validation --> Script
    Script --> Users
    Users --> Dynamic_Groups
    
    %% Governance Signaling
    G_IT -.-> |"Admin JIT Request"| PIM
    PIM --> |"Elevate Session Temporarily"| CA
    
    %% Compliance & Signals
    Intune -.-> |"Device Signal"| CA
    Dynamic_Groups --> |"1. Request Access to Apps"| CA
    
    %% Final Token Delivery
    CA --> |"2. Verify MFA Compliance & Issue Access Token"| CloudApps
    
    %% Lifecycle Operations
    HR_Data -.-> |"Leaver: disable -> delete"| CloudApps
