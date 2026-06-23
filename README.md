# Enterprise Zero Trust Identity Lab (500+ Users)
## 📌 Project Overview
This project demonstrates the design and implementation of an *Enterprise-grade, Cloud-Only Zero Trust Identity Architecture* using *Microsoft Entra ID. Built to simulate a production-ready corporate environment for a single-location enterprise, the lab orchestrates automated user lifecycle management for **500 employees*, enforces adaptive access controls, and implements strict identity governance.

The architecture strictly adheres to the core pillars of the *Zero Trust Framework: *Explicitly Verify, Use Least Privileged Access, and Assume Breach.

---

## 🏗️ Key Architectural Pillars

### 1. Automated Lifecycle Management & Governance
* *HR-Driven Provisioning:* Automated onboarding and offboarding simulation utilizing an upstream HR data roster (500 employees) processed via a custom *PowerShell automation script* (Bulk_Import_Script.ps1) and integrated via *Microsoft Graph API*.
* *Attribute Validation Layer:* The ingestion pipeline features a data-validation layer that enforces data integrity, automatically rejecting any identity records missing critical attributes such as Department, JobTitle, or Location.
* *Dynamic Group Architecture:* Zero-touch, automated user assignment into departmental security groups (e.g., IT, HR, Engineering, Sales, Finance) leveraging complex Entra ID *Dynamic Membership Rules* based on verified user attributes.

### 2. Adaptive Access Control (Zero Trust)
* *Risk-Based Conditional Access:* Real-time evaluation of security postures utilizing Entra ID Protection. Sign-in attempts are dynamically blocked or forced to remediate based on calculated *User Risk* and *Sign-in Risk* scores.
* *Phishing-Resistant MFA:* Elimination of weak authentication methods by enforcing passwordless *Microsoft Authenticator with number matching* and *FIDO2 security keys* for high-risk access scenarios.
* *Targeted Application Scoping:* Security groups act as the foundational criteria inside Conditional Access policies, ensuring that users only inherit access paths mapped directly to their verified corporate roles.

### 3. Privileged Identity Management (PIM) & Governance
* *Least Privileged Access:* Elimination of permanent administrative assignments. Highly privileged roles (e.g., Global Administrator, Security Administrator) are governed under *Privileged Identity Management (PIM)*.
* *Just-In-Time (JIT) Activation:* Administrators must explicitly request role elevation, requiring multi-factor authentication, documented business justifications, and a strict time-bound window.
* *Automated Approval Workflows:* Integration of JIT requests with *Microsoft Power Automate*, routing high-level approval steps seamlessly to designated security officers before access is provisioned.
* *Entra ID Access Reviews:* Automated, periodic attestation schedules to review and mitigate entitlement creep, ensuring inactive or transferred users lose elevated privileges automatically.

### 4. Enterprise Application SSO & Federation
* *Centralized Identity Provider (IdP):* Federation of critical SaaS applications (e.g., Slack, Zoom) using *SAML 2.0* and *OpenID Connect (OIDC)* to centralize authentication control.
* *Automated App Provisioning:* Implementation of *SCIM-based* synchronization to provision and de-provision identities directly from Entra ID to external enterprise apps.
* *Data Loss Prevention for SaaS:* Conditional Access policies tailored for SaaS workloads to enforce specialized security barriers, such as blocking external storage downloads when accessing corporate resources from unmanaged or non-compliant devices.

---

## 🛠️ Technology Stack
* *Identity Platform:* Microsoft Entra ID (P2)
* *Automation & API:* PowerShell, Microsoft Graph API, Microsoft Power Automate
* *Protocols:* SAML 2.0, OIDC, SCIM, OAuth 2.0
* *Security Framework:* Zero Trust Architecture (ZTA)

---

## 📊 Deployment Architecture

The complete identity provisioning, governance, and authentication validation flow is mapped out in the architectural blueprint below:
## 🏗️ Enterprise Infrastructure Blueprint

To architect an automated, scalable, and secure identity infrastructure, this project implements a defense-in-depth architecture aligned with the Zero Trust framework and Microsoft Entra ID best practices. 

The structural blueprint below maps out the production-ready identity lifecycle management (ILM), attribute-based access control (ABAC), and the exact authentication flow enforced across the 500-employee enterprise lab:

```mermaid
graph TD
    %% Custom Styling to Match Claude's Bright/Clean Palette 100%
    classDef source fill:#EAF2F8,stroke:#2980B9,stroke-width:2px,font-family:'Segoe UI';
    classDef validation fill:#F4F6F7,stroke:#7F8C8D,stroke-width:2px,font-family:'Segoe UI';
    classDef automation fill:#EAF2F8,stroke:#2980B9,stroke-width:2px,font-family:'Segoe UI';
    classDef tenant fill:#F5EEF8,stroke:#8E44AD,stroke-width:2px,font-family:'Segoe UI';
    classDef userObj fill:#EEF2F7,stroke:#2F5597,stroke-width:2px,font-family:'Segoe UI';
    classDef group fill:#E8F8F5,stroke:#1ABC9C,stroke-width:2px,font-family:'Segoe UI';
    classDef engine fill:#FEF9E7,stroke:#F1C40F,stroke-width:2px,font-family:'Segoe UI';
    classDef apps fill:#FBEEE6,stroke:#E67E22,stroke-width:2px,font-family:'Segoe UI';

    %% 1. Identity Source & Automation Layer (Top)
    HR_Data["📋 Enterprise HR Data / Employee Roster<br>(500 Employees Base)"]:::source
    Validation["⚙️ Attribute Validation Layer<br>Rejects missing: dept • jobTitle • location"]:::validation
    Script["🖥️ PowerShell Automation Script<br><b>[Bulk_Import_Script.ps1]</b>"]:::automation

    %% 2. Microsoft Entra ID Tenant Boundary (Center)
    subgraph Entra_ID_Tenant ["🔮 Microsoft Entra ID Tenant Boundary"]
        Users["👤 Created User Objects<br>(Attributes: Department, JobTitle)"]:::userObj
        
        %% Internal Governance & Security Subgraph
        subgraph Security_Engines ["🔒 Entra Security & Governance Engines"]
            PIM["🔑 Privileged Identity Management<br><b>[Just-In-Time Role Activation]</b>"]:::engine
            CA["🛡️ Conditional Access Policy<br><b>[Evaluate: MFA & Location]</b>"]:::engine
        end

        %% Dynamic Security Groups Subgraph
        subgraph Dynamic_Groups ["📁 Attribute-Based Dynamic Security Groups"]
            G_IT["👥 IT-Dept Group<br><small>Rule: dept -eq 'IT'</small>"]:::group
            G_HR["👥 HR-Dept Group<br><small>Rule: dept -eq 'HR'</small>"]:::group
            G_ENG["👥 Engineering-Dept Group<br><small>Rule: dept -eq 'Eng'</small>"]:::group
            G_SALES["👥 Sales-Dept Group<br><small>Rule: dept -eq 'Sales'</small>"]:::group
            G_FIN["👥 Finance-Dept Group<br><small>Rule: dept -eq 'Finance'</small>"]:::group
        end
    end
    %% 3. Target Applications (Bottom)
    CloudApps["☁️ Enterprise Cloud Applications<br><b>[Office 365 / SaaS Apps / SSO via SAML-OIDC]</b>"]:::apps

    %% --- Architectural Relationships & Flow (Exactly like the photo) ---
    HR_Data --> Validation
    Validation --> Script
    Script --> |"Microsoft Graph API<br>Bulk User Provisioning"| Users
    
    %% Split Flows from User Objects
    Users --> |"Admin JIT Request"| PIM
    Users --> |"Automated Evaluation<br>Based on Attributes"| Dynamic_Groups
    
    %% Policy Evaluation Flow
    PIM --> |"Elevate Session<br>Temporarily"| CA
    Dynamic_Groups --> |"1. Request Access to Apps"| CA
    
    %% Final Enforced Routing to Cloud
    CA --> |"2. Verify MFA Compliance & Issue Access Token"| CloudApps
