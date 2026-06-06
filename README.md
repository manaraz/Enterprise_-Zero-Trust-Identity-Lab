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
    %% Professional Enterprise Security Styling
    classDef source fill:#EAF2F8,stroke:#2980B9,stroke-width:2px,font-family:'Segoe UI';
    classDef tenant fill:#EBDEF0,stroke:#8E44AD,stroke-width:3px,font-family:'Segoe UI';
    classDef group fill:#E8F8F5,stroke:#1ABC9C,stroke-width:2px,font-family:'Segoe UI';
    classDef engine fill:#FEF9E7,stroke:#F1C40F,stroke-width:2px,font-family:'Segoe UI';
    classDef apps fill:#FBEEE6,stroke:#E67E22,stroke-width:2px,font-family:'Segoe UI';

    %% 1. Identity Source & Automation (The Start)
    HR_Data["📋 Enterprise HR Data / Employee Roster<br>(500 Employees Base)"]:::source
    Script["🖥️ PowerShell Automation Script<br><b>[Bulk_Import_Script.ps1]</b>"]:::source

    %% 2. Identity Provider Boundary (The Core Tenant)
    subgraph Entra_ID_Tenant ["🔮 Microsoft Entra ID Tenant Boundary"]
        
        %% Identity Creation & Lifecycle
        Users["👤 Created User Objects<br>(Attributes: Department, JobTitle)"]:::tenant
        
        %% Attribute-Based Dynamic Membership (RBAC)
        subgraph Dynamic_Groups ["📁 Attribute-Based Dynamic Security Groups"]
            G_IT["👥 IT-Dept Group<br><small>Rule: dept -eq 'IT'</small>"]:::group
            G_HR["👥 HR-Dept Group<br><small>Rule: dept -eq 'HR'</small>"]:::group
            G_ENG["👥 Engineering-Dept Group<br><small>Rule: dept -eq 'Eng'</small>"]:::group
            G_SALES["👥 Sales-Dept Group<br><small>Rule: dept -eq 'Sales'</small>"]:::group
            G_FIN["👥 Finance-Dept Group<br><small>Rule: dept -eq 'Finance'</small>"]:::group
        end

        %% Internal Policy Evaluation Engines
        subgraph Security_Engines ["🔒 Entra Security & Governance Engines"]
            CA["🛡️ Conditional Access Policy<br><b>[Evaluate: MFA & Location]</b>"]:::engine
            PIM["🔑 Privileged Identity Management<br><b>[Just-In-Time Role Activation]</b>"]:::engine
        end
    end

    %% 3. Target Cloud Applications
    CloudApps["☁️ Enterprise Cloud Applications<br><b>[Office 365 / SaaS Apps / SSO via SAML-OIDC]</b>"]:::apps

    %% --- Technical Authentication & Provisioning Flows ---
    
    %% Provisioning Pipeline
    HR_Data --> Script
    Script --> |"Microsoft Graph API<br>Bulk User Provisioning"| Users
    
    %% Dynamic Assignment
    Users --> |"Automated Evaluation<br>Based on Attributes"| Dynamic_Groups

    %% User Access Request & Authentication Flow
    Dynamic_Groups --> |"1. Request Access to Apps"| CA
    Users --> |"Admin JIT Request"| PIM
    
    %% Internal Security Token Verification
    PIM --> |"Elevate Session<br>Temporarily"| CA
    CA --> |"2. Verify MFA Compliance<br>& Issue Access Token"| CloudApps
