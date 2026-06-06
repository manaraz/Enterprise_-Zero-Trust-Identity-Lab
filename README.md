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

![Enterprise Identity Blueprint](architecture.svg)
