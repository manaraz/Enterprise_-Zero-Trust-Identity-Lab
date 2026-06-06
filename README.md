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

<svg width="100%" viewBox="0 0 680 960" role="img">
  <title>Enterprise Identity Infrastructure Blueprint</title>
  <desc>High-contrast flowchart: HR data flows through PowerShell automation into Microsoft Entra ID tenant, through dynamic groups and security engines, to cloud applications.</desc>
  <defs>
    <marker id="arrow" viewBox="0 0 10 10" refX="8" refY="5" markerWidth="6" markerHeight="6" orient="auto-start-reverse">
      <path d="M2 1L8 5L2 9" fill="none" stroke="context-stroke" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
    </marker>
  </defs>

  <!-- LEGEND -->
  <rect x="40" y="16" width="14" height="14" rx="3" fill="#0C447C" stroke="#042C53" stroke-width="0.5"/>
  <text x="60" y="23" font-family="sans-serif" font-size="12" fill="#444" dominant-baseline="central">Identity source</text>
  <rect x="160" y="16" width="14" height="14" rx="3" fill="#3C3489" stroke="#26215C" stroke-width="0.5"/>
  <text x="180" y="23" font-family="sans-serif" font-size="12" fill="#444" dominant-baseline="central">Entra ID / users</text>
  <rect x="296" y="16" width="14" height="14" rx="3" fill="#085041" stroke="#04342C" stroke-width="0.5"/>
  <text x="316" y="23" font-family="sans-serif" font-size="12" fill="#444" dominant-baseline="central">Dynamic groups</text>
  <rect x="420" y="16" width="14" height="14" rx="3" fill="#633806" stroke="#412402" stroke-width="0.5"/>
  <text x="440" y="23" font-family="sans-serif" font-size="12" fill="#444" dominant-baseline="central">Security engines</text>
  <rect x="544" y="16" width="14" height="14" rx="3" fill="#712B13" stroke="#4A1B0C" stroke-width="0.5"/>
  <text x="564" y="23" font-family="sans-serif" font-size="12" fill="#444" dominant-baseline="central">Cloud apps</text>

  <!-- 1. HR DATA -->
  <rect x="170" y="46" width="340" height="56" rx="10" fill="#0C447C" stroke="#042C53" stroke-width="0.5"/>
  <text x="340" y="68" font-family="sans-serif" font-size="14" font-weight="500" fill="#E6F1FB" text-anchor="middle" dominant-baseline="central">Enterprise HR data</text>
  <text x="340" y="88" font-family="sans-serif" font-size="12" fill="#85B7EB" text-anchor="middle" dominant-baseline="central">500-employee roster · joiner / mover / leaver events</text>

  <!-- arrow -->
  <line x1="340" y1="102" x2="340" y2="132" stroke="#378ADD" stroke-width="1.5" marker-end="url(#arrow)"/>

  <!-- 2. VALIDATION -->
  <rect x="200" y="132" width="280" height="56" rx="10" fill="#444441" stroke="#2C2C2A" stroke-width="0.5"/>
  <text x="340" y="154" font-family="sans-serif" font-size="14" font-weight="500" fill="#F1EFE8" text-anchor="middle" dominant-baseline="central">Attribute validation layer</text>
  <text x="340" y="172" font-family="sans-serif" font-size="12" fill="#B4B2A9" text-anchor="middle" dominant-baseline="central">Rejects missing: dept · jobTitle · location</text>

  <!-- arrow -->
  <line x1="340" y1="188" x2="340" y2="222" stroke="#888780" stroke-width="1.5" marker-end="url(#arrow)"/>

  <!-- 3. POWERSHELL -->
  <rect x="170" y="222" width="340" height="56" rx="10" fill="#0C447C" stroke="#042C53" stroke-width="0.5"/>
  <text x="340" y="244" font-family="sans-serif" font-size="14" font-weight="500" fill="#E6F1FB" text-anchor="middle" dominant-baseline="central">PowerShell automation</text>
  <text x="340" y="262" font-family="sans-serif" font-size="12" fill="#85B7EB" text-anchor="middle" dominant-baseline="central">Graph API /users · bulk provisioning</text>

  <!-- arrow into tenant -->
  <line x1="340" y1="278" x2="340" y2="336" stroke="#534AB7" stroke-width="1.5" marker-end="url(#arrow)"/>

  <!-- ENTRA TENANT BOUNDARY -->
  <rect x="24" y="300" width="632" height="580" rx="16" fill="none" stroke="#534AB7" stroke-width="1.5" stroke-dasharray="7 4"/>
  <text x="40" y="322" font-family="sans-serif" font-size="12" fill="#534AB7" dominant-baseline="central">Microsoft Entra ID tenant boundary</text>

  <!-- 4. USER OBJECTS -->
  <rect x="190" y="336" width="300" height="56" rx="10" fill="#3C3489" stroke="#26215C" stroke-width="0.5"/>
  <text x="340" y="358" font-family="sans-serif" font-size="14" font-weight="500" fill="#EEEDFE" text-anchor="middle" dominant-baseline="central">User objects</text>
  <text x="340" y="376" font-family="sans-serif" font-size="12" fill="#AFA9EC" text-anchor="middle" dominant-baseline="central">Department · JobTitle · officeLocation</text>

  <!-- arrow to groups -->
  <line x1="340" y1="392" x2="340" y2="440" stroke="#7F77DD" stroke-width="1.5" marker-end="url(#arrow)"/>

  <!-- DYNAMIC GROUPS CONTAINER -->
  <rect x="38" y="440" width="604" height="148" rx="12" fill="none" stroke="#1D9E75" stroke-width="1" stroke-dasharray="5 3"/>
  <text x="54" y="458" font-family="sans-serif" font-size="12" fill="#0F6E56" dominant-baseline="central">Attribute-driven dynamic security groups · ~5 min re-evaluation latency</text>

  <!-- IT -->
  <rect x="44" y="466" width="110" height="106" rx="8" fill="#085041" stroke="#04342C" stroke-width="0.5"/>
  <text x="99" y="498" font-family="sans-serif" font-size="14" font-weight="500" fill="#9FE1CB" text-anchor="middle" dominant-baseline="central">IT dept</text>
  <text x="99" y="520" font-family="sans-serif" font-size="12" fill="#5DCAA5" text-anchor="middle" dominant-baseline="central">dept -eq</text>
  <text x="99" y="538" font-family="sans-serif" font-size="12" fill="#5DCAA5" text-anchor="middle" dominant-baseline="central">'IT'</text>

  <!-- HR -->
  <rect x="166" y="466" width="110" height="106" rx="8" fill="#085041" stroke="#04342C" stroke-width="0.5"/>
  <text x="221" y="498" font-family="sans-serif" font-size="14" font-weight="500" fill="#9FE1CB" text-anchor="middle" dominant-baseline="central">HR dept</text>
  <text x="221" y="520" font-family="sans-serif" font-size="12" fill="#5DCAA5" text-anchor="middle" dominant-baseline="central">dept -eq</text>
  <text x="221" y="538" font-family="sans-serif" font-size="12" fill="#5DCAA5" text-anchor="middle" dominant-baseline="central">'HR'</text>

  <!-- ENG -->
  <rect x="288" y="466" width="110" height="106" rx="8" fill="#085041" stroke="#04342C" stroke-width="0.5"/>
  <text x="343" y="490" font-family="sans-serif" font-size="14" font-weight="500" fill="#9FE1CB" text-anchor="middle" dominant-baseline="central">Engi-</text>
  <text x="343" y="508" font-family="sans-serif" font-size="14" font-weight="500" fill="#9FE1CB" text-anchor="middle" dominant-baseline="central">neering</text>
  <text x="343" y="528" font-family="sans-serif" font-size="12" fill="#5DCAA5" text-anchor="middle" dominant-baseline="central">dept -eq 'Eng'</text>

  <!-- SALES -->
  <rect x="410" y="466" width="110" height="106" rx="8" fill="#085041" stroke="#04342C" stroke-width="0.5"/>
  <text x="465" y="498" font-family="sans-serif" font-size="14" font-weight="500" fill="#9FE1CB" text-anchor="middle" dominant-baseline="central">Sales</text>
  <text x="465" y="520" font-family="sans-serif" font-size="12" fill="#5DCAA5" text-anchor="middle" dominant-baseline="central">dept -eq</text>
  <text x="465" y="538" font-family="sans-serif" font-size="12" fill="#5DCAA5" text-anchor="middle" dominant-baseline="central">'Sales'</text>

  <!-- FINANCE -->
  <rect x="532" y="466" width="110" height="106" rx="8" fill="#085041" stroke="#04342C" stroke-width="0.5"/>
  <text x="587" y="498" font-family="sans-serif" font-size="14" font-weight="500" fill="#9FE1CB" text-anchor="middle" dominant-baseline="central">Finance</text>
  <text x="587" y="520" font-family="sans-serif" font-size="12" fill="#5DCAA5" text-anchor="middle" dominant-baseline="central">dept -eq</text>
  <text x="587" y="538" font-family="sans-serif" font-size="12" fill="#5DCAA5" text-anchor="middle" dominant-baseline="central">'Finance'</text>

  <!-- arrow groups to CA -->
  <line x1="340" y1="588" x2="340" y2="648" stroke="#1D9E75" stroke-width="1.5" marker-end="url(#arrow)"/>

  <!-- PIM side path -->
  <path d="M190 364 L100 364 L100 726 L192 726" fill="none" stroke="#BA7517" stroke-width="1.2" stroke-dasharray="5 3" marker-end="url(#arrow)"/>
  <text x="55" y="548" font-family="sans-serif" font-size="12" fill="#633806" dominant-baseline="central">Admin</text>
  <text x="55" y="564" font-family="sans-serif" font-size="12" fill="#633806" dominant-baseline="central">JIT req.</text>

  <!-- Device signal path -->
  <path d="M580 364 L610 364 L610 726 L528 726" fill="none" stroke="#27500A" stroke-width="1.2" stroke-dasharray="5 3" marker-end="url(#arrow)"/>
  <text x="616" y="548" font-family="sans-serif" font-size="12" fill="#27500A" dominant-baseline="central">Device</text>
  <text x="616" y="564" font-family="sans-serif" font-size="12" fill="#27500A" dominant-baseline="central">signal</text>

  <!-- INTUNE -->
  <rect x="580" y="336" width="76" height="56" rx="8" fill="#27500A" stroke="#173404" stroke-width="0.5"/>
  <text x="618" y="358" font-family="sans-serif" font-size="14" font-weight="500" fill="#C0DD97" text-anchor="middle" dominant-baseline="central">Intune</text>
  <text x="618" y="376" font-family="sans-serif" font-size="12" fill="#97C459" text-anchor="middle" dominant-baseline="central">Compliance</text>

  <!-- SECURITY ENGINES CONTAINER -->
  <rect x="38" y="648" width="604" height="170" rx="12" fill="none" stroke="#BA7517" stroke-width="1" stroke-dasharray="5 3"/>
  <text x="54" y="666" font-family="sans-serif" font-size="12" fill="#633806" dominant-baseline="central">Security and governance engines</text>

  <!-- CA -->
  <rect x="52" y="678" width="270" height="124" rx="10" fill="#633806" stroke="#412402" stroke-width="0.5"/>
  <text x="187" y="706" font-family="sans-serif" font-size="14" font-weight="500" fill="#FAEEDA" text-anchor="middle" dominant-baseline="central">Conditional Access</text>
  <text x="187" y="726" font-family="sans-serif" font-size="12" fill="#FAC775" text-anchor="middle" dominant-baseline="central">User risk · Sign-in risk</text>
  <text x="187" y="744" font-family="sans-serif" font-size="12" fill="#FAC775" text-anchor="middle" dominant-baseline="central">MFA · Named location</text>
  <text x="187" y="762" font-family="sans-serif" font-size="12" fill="#FAC775" text-anchor="middle" dominant-baseline="central">Device compliance</text>
  <text x="187" y="782" font-family="sans-serif" font-size="12" fill="#FAC775" text-anchor="middle" dominant-baseline="central">Issues access token</text>

  <!-- PIM -->
  <rect x="360" y="678" width="270" height="124" rx="10" fill="#633806" stroke="#412402" stroke-width="0.5"/>
  <text x="495" y="706" font-family="sans-serif" font-size="14" font-weight="500" fill="#FAEEDA" text-anchor="middle" dominant-baseline="central">PIM</text>
  <text x="495" y="726" font-family="sans-serif" font-size="12" fill="#FAC775" text-anchor="middle" dominant-baseline="central">Just-in-time role activation</text>
  <text x="495" y="746" font-family="sans-serif" font-size="12" fill="#FAC775" text-anchor="middle" dominant-baseline="central">Max 4 hr · Approval workflow</text>
  <text x="495" y="766" font-family="sans-serif" font-size="12" fill="#FAC775" text-anchor="middle" dominant-baseline="central">Power Automate approval</text>
  <text x="495" y="786" font-family="sans-serif" font-size="12" fill="#FAC775" text-anchor="middle" dominant-baseline="central">New token issued on elevation</text>

  <!-- PIM to CA -->
  <line x1="360" y1="740" x2="324" y2="740" stroke="#BA7517" stroke-width="1.5" marker-end="url(#arrow)"/>

  <!-- arrow CA to apps -->
  <line x1="340" y1="818" x2="340" y2="868" stroke="#993C1D" stroke-width="1.5" marker-end="url(#arrow)"/>

  <!-- CLOUD APPS -->
  <rect x="100" y="868" width="480" height="68" rx="10" fill="#712B13" stroke="#4A1B0C" stroke-width="0.5"/>
  <text x="340" y="894" font-family="sans-serif" font-size="14" font-weight="500" fill="#FAECE7" text-anchor="middle" dominant-baseline="central">Enterprise cloud applications</text>
  <text x="340" y="916" font-family="sans-serif" font-size="12" fill="#F0997B" text-anchor="middle" dominant-baseline="central">Office 365 · Salesforce · Slack · Zoom · SAML 2.0 / OIDC · SCIM</text>

  <!-- Offboard path -->
  <path d="M480 160 L650 160 L650 920 L582 920" fill="none" stroke="#A32D2D" stroke-width="1" stroke-dasharray="4 3" marker-end="url(#arrow)"/>
  <text x="638" y="820" font-family="sans-serif" font-size="12" fill="#791F1F" text-anchor="middle" dominant-baseline="central">Leaver:</text>
  <text x="638" y="838" font-family="sans-serif" font-size="12" fill="#791F1F" text-anchor="middle" dominant-baseline="central">disable</text>
  <text x="638" y="854" font-family="sans-serif" font-size="12" fill="#791F1F" text-anchor="middle" dominant-baseline="central">→ delete</text>

</svg>

