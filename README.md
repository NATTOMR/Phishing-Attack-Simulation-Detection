![Banner](images/banner.png)

![Security Awareness](https://img.shields.io/badge/Security-Awareness-blue?style=for-the-badge&logo=security&logoColor=white)
![Phishing Simulation](https://img.shields.io/badge/Phishing-Simulation-red?style=for-the-badge)
![GoPhish](https://img.shields.io/badge/Tool-GoPhish-darkgreen?style=for-the-badge)
![MailHog](https://img.shields.io/badge/SMTP-MailHog-orange?style=for-the-badge)

# Phishing Attack Simulation & Detection

## 📌 Project Overview

Phishing remains one of the most prevalent and damaging social engineering tactics used by cyber adversaries to compromise enterprise systems.  
This project demonstrates a **controlled phishing attack simulation** conducted in a secure lab environment to analyze phishing techniques, monitor target interactions, identify key red flags, and document defensive prevention strategies.

---

## 🎯 Objectives

- Understand how phishing attacks are designed, executed, and detected.
- Deploy and configure **GoPhish** and **MailHog** in a controlled lab environment.
- Create custom email templates and landing pages for security awareness training.
- Simulate an end-to-end phishing campaign to analyze user vulnerability.
- Identify common social engineering indicators and technical red flags.
- Formulate practical blue-team defensive and mitigation recommendations.

---

## 🧰 Tools & Technologies

| Tool | Purpose |
|------|---------|
| **GoPhish** | Open-source phishing simulation framework |
| **MailHog** | Developer tool for local SMTP testing and email capture |
| **Kali Linux** | Operating system host environment |
| **VS Code / HTML5** | Template design and custom payload crafting |

---

## 🛠️ Installation & Setup (Kali Linux)

### Step 1: Update System Packages
```bash
sudo apt update && sudo apt upgrade -y
```

### Step 2: Check System Architecture
Verify system architecture compatibility (x86_64 recommended):
```bash
uname -m
```
- ✅ **x86_64** → Fully supported
- ❌ **aarch64 (ARM)** → Use an x86_64 Virtual Machine

### Step 3: Download & Extract GoPhish
Navigate to `/opt` and download the latest release:
```bash
cd /opt
sudo wget https://github.com/gophish/gophish/releases/download/v0.12.1/gophish-v0.12.1-linux-64bit.zip
sudo unzip gophish-v0.12.1-linux-64bit.zip -d gophish
cd gophish
sudo chmod +x gophish
```

### Step 4: Configure Listener Interface
Edit `config.json` to configure the administration interface:
```bash
sudo nano config.json
```
Change `listen_url` under `admin_server` to listen locally:
```json
"admin_server": {
    "listen_url": "127.0.0.1:3333",
    "use_tls": true
}
```

![GoPhish Dashboard](images/GoPhish%20Dashboard.png)

---

## 🚀 Step-by-Step Simulation Campaign Walkthrough

### 🔹 STEP 1: Create an Email Template
1. In GoPhish, navigate to **Email Templates** and click **+ New Template**.
2. Fill out the template parameters:
   - **Name**: `Password Reset Simulation – Test 2`
   - **Envelope Sender**: `IT Support <it-support@lab.local>`
   - **Subject**: `Password Reset Required`
3. HTML payload sample (available in [templates/email_template.html](file:///e:/New%20folder%20%289%29/elevate%20labs%20work/2026/cybersecurity/Task_11-Phishing-Attack-Simulation-Detection/templates/email_template.html)):
   ```html
   <html>
     <body style="font-family: Arial;">
       <h3>Password Reset Notice</h3>
       <p>Hello {{.FirstName}},</p>
       <p>A password reset request was initiated for your account.</p>
       <p>
         <a href="{{.URL}}" style="background:#2980b9;color:white;padding:10px 16px;text-decoration:none;border-radius:4px;">
           Confirm Reset
         </a>
       </p>
       <p style="font-size:12px;color:gray;">This is a cybersecurity awareness simulation.</p>
     </body>
   </html>
   ```
4. Keep **Add Tracking Image** checked and click **Save Template**.

![Email Template Setup](images/GoPhish%20Email-Tamplate.png.jpeg)

---

### 🔹 STEP 2: Create a Landing Page
1. Navigate to **Landing Pages** and click **+ New Page**.
2. Configure parameters:
   - **Name**: `Password Reset Awareness – Test 2`
   - **HTML Content** (available in [templates/landing_page.html](file:///e:/New%20folder%20%289%29/elevate%20labs%20work/2026/cybersecurity/Task_11-Phishing-Attack-Simulation-Detection/templates/landing_page.html)):
   ```html
   <html>
     <body style="font-family: Arial; text-align:center; margin-top:80px;">
       <h2>Password Reset Simulation</h2>
       <p>This was a simulated phishing email designed for security training.</p>
       <p style="color:green;">Always verify reset requests before clicking links.</p>
     </body>
   </html>
   ```
3. Keep **Capture Submitted Data** unchecked (to prevent storing raw credentials). Click **Save Page**.

![Landing Page Setup](images/GoPhish%20Landing-Pages.jpeg.png)

---

### 🔹 STEP 3: Configure Target Users & Groups
1. Navigate to **Users & Groups** and click **+ New Group**.
2. Name the group `Security Awareness Lab – Feb 2026`.
3. Import target users using a CSV file (sample provided in [templates/users_group.csv](file:///e:/New%20folder%20%289%29/elevate%20labs%20work/2026/cybersecurity/Task_11-Phishing-Attack-Simulation-Detection/templates/users_group.csv)).

![Users & Groups Setup](images/gophish%20new%20users%20%26%20group.jpeg)

---

### 🔹 STEP 4: Configure Sending Profile (MailHog)
1. Navigate to **Sending Profiles** and click **+ New Profile**.
2. Configure MailHog SMTP details:
   - **Name**: `MailHog Lab`
   - **Interface Type**: `SMTP`
   - **SMTP From**: `alerts@bank-lab.local`
   - **Host**: `127.0.0.1:1025`
   - **Username / Password**: *(leave blank)*
   - **Ignore Certificate Errors**: ✅ Checked
3. Click **Save Profile**.

![Sending Profile Setup](images/Gophish%20Sending%20profile.png)

---

### 🔹 STEP 5: Launch Phishing Campaign
1. Navigate to **Campaigns** and click **+ New Campaign**.
2. Set configuration:
   - **Name**: `Password Reset Test – Feb 2026`
   - **Email Template**: `Password Reset Simulation – Test 2`
   - **Landing Page**: `Password Reset Awareness – Test 2`
   - **URL**: `http://127.0.0.1`
   - **Sending Profile**: `MailHog Lab`
   - **Groups**: `Security Awareness Lab – Feb 2026`
3. Click **Launch Campaign** and confirm.

![Campaign Setup](images/Campaign.jpeg)

---

### 🔹 STEP 6: Monitor Results & Analyze MailHog Captures
1. Access MailHog web interface at `http://127.0.0.1:8025`.
2. Inspect intercepted simulation emails, tracking pixels, and user interactions.

![MailHog Interface](images/mailHog.jpeg)

---

## 📊 Campaign Metrics & Tracking Results

| Metric | Description | Example Results |
|--------|-------------|-----------------|
| **Emails Sent** | Total phishing emails dispatched | 10 |
| **Emails Opened** | Target opened email (tracking image triggered) | 7 |
| **Links Clicked** | Target clicked simulated phishing link | 4 |
| **Forms Submitted** | High-risk behavior (submitted credentials/form) | 2 |
| **Emails Ignored** | Security-aware behavior | 3 |

---

## 🚩 Identifying Phishing Red Flags

### Email Red Flags
- **Sender Address**: Slight domain misspellings or spoofed internal addresses (`it-support@lab.local`).
- **Urgency & Coercion**: High-pressure language demanding immediate action to avoid account suspension.
- **Generic Greetings**: Missing customized greetings (e.g., "Dear User" or generic placeholders).
- **Unverified Links**: Mismatch between display text and actual link destination domain.

### Link & URL Indicators
- Use of raw IP addresses (`http://127.0.0.1`) or HTTP protocols instead of HTTPS.
- Dynamic URL shorteners masking the actual target domain.

---

## 🛡️ Prevention & Mitigation Strategies

### Technical Controls
- **Email Authentication**: Enforce strict **SPF** (Sender Policy Framework), **DKIM** (DomainKeys Identified Mail), and **DMARC** policy records.
- **Multi-Factor Authentication (MFA)**: Require FIDO2/webauthn hardware tokens or authenticator apps to mitigate credential harvesting.
- **Email Gateway Filtering**: Deploy automated spam and phishing filters with link-rewriting and sandbox detonation features.

### User & Organizational Measures
- **Regular Phishing Simulations**: Conduct periodic simulated phishing campaigns to build organizational resilience.
- **Incident Reporting**: Implement a one-click phishing report button in email clients.
- **Security Awareness Training**: Train users to inspect sender headers, verify URL domains, and report anomalies.

---

## 👤 Author
- **Name**: NATTO MUNI CHAKMA
- **Role**: Cybersecurity Student | SOC Analyst (Aspirant) | Blue Team Learner

---

## 📚 References
- [GoPhish Official Documentation](https://docs.getgophish.com)
- [MITRE ATT&CK – Phishing (T1566)](https://attack.mitre.org/techniques/T1566/)
- [OWASP Top 10 – Social Engineering](https://owasp.org)
- [NIST SP 800-61 – Incident Handling Guide](https://csrc.nist.gov/publications/detail/sp/800-61/rev-2/final)

---

## ❓ Frequently Asked Interview Questions

### 1. What is a phishing attack?
> **Answer**: Phishing is a social engineering attack where bad actors impersonate legitimate organizations or trusted individuals via email, SMS, or phone to deceive victims into revealing sensitive information, such as passwords, banking credentials, or personal data.

### 2. What are the common types of phishing attacks?
> **Answer**:
> - **Spear Phishing**: Targeted phishing aimed at a specific individual or organization.
> - **Whaling**: Phishing targeted specifically at high-profile executives (CEOs, CFOs).
> - **Vishing & Smishing**: Phishing conducted via voice calls (Vishing) or SMS text messages (Smishing).
> - **Watering Hole Attack**: Compromising a website frequently visited by targets to infect visitors.

### 3. How do defenders detect phishing attacks?
> **Answer**: Defenders use automated email security solutions (SPF/DKIM/DMARC validation), dynamic URL scanning, email header analysis, spam threshold scoring, endpoint detection & response (EDR), and user-submitted suspicious email reports.

### 4. Why is phishing considered one of the most dangerous attack vectors?
> **Answer**: Phishing bypasses traditional technical security perimeters by directly targeting human psychological vulnerabilities (trust, urgency, fear). A single compromised credential can grant an adversary initial access to internal networks.

### 5. What are the primary methods to prevent phishing?
> **Answer**: Prevention requires a multi-layered defense model combining strong technical controls (MFA, SPF/DKIM/DMARC, email sandboxing) with continuous security awareness training and regular phishing simulation exercises.

---

## 🔒 Disclaimer
This project is developed strictly for **educational and authorized security awareness testing purposes**. Executing phishing simulations against unauthorized targets without prior explicit written consent is illegal.
