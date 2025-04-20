# **End-to-End Intune Device Management with Compliance, Updates, Risk Scoring & Defender Integration**

---

## **Lab Overview:**  
This lab walks through the complete process of setting up modern device management using Microsoft Intune and Entra ID. It includes device enrollment, configuring compliance and update policies, monitoring compliance, remediating non-compliance, and enforcing security with Microsoft Defender for Endpoint and conditional access.

---

## **Who, What, When, Where, Why**

- **Who:** IT Administrators managing Windows 10/11 devices in Microsoft 365 environments.  
- **What:** Configure an end-to-end device management pipeline using Intune, Entra ID, and Defender.  
- **When:** During initial Intune setup or when enhancing device compliance and security.  
- **Where:** Microsoft Intune (Endpoint Manager admin center), Entra ID, Microsoft 365 Defender portal.  
- **Why:** To automate device onboarding, enforce compliance, ensure patching, monitor health, and secure the tenant using real-time risk scoring.

---

## **Lab Requirements**
- Microsoft 365 E5 or equivalent trial with Intune + Defender for Endpoint
- At least one Windows 10 or 11 test VM
- Access to Microsoft Entra ID and Intune Admin Center

---

## **Step-by-Step Instructions**

---

### **Step 1: Enable Microsoft Intune Enrollment in Entra ID**

**Definition:** Enabling device registration ensures that Windows devices can join or register with Microsoft Entra ID and become managed by Intune.

1. Navigate to [https://entra.microsoft.com](https://entra.microsoft.com)  
2. Go to **Devices** > **Device settings**

![image](https://github.com/user-attachments/assets/7cb6726c-4b67-485e-99fb-2d2c1cdfd0bd)

3. Set **Users may join devices to Azure AD** to **All** or a selected group
4. Configure **Additional local administrators on Azure AD joined devices** if needed

![image](https://github.com/user-attachments/assets/8d759e25-ec5a-48db-8c54-1c89d3ed52b9)

---

### **Step 2: Create Intune Enrollment Profile**

**Definition:** Enrollment profiles configure the experience users get when enrolling their devices into Intune.

1. Navigate to [https://endpoint.microsoft.com](https://endpoint.microsoft.com)  
2. Go to **Devices** > **Enrollment** > **Deployment Profiles**

![image](https://github.com/user-attachments/assets/168a2ee7-9264-4105-91e5-da468963b0aa)



3. Create a **Windows Autopilot profile**

![image](https://github.com/user-attachments/assets/2dfcdc47-97ac-47f5-b7e9-cd42c9d783e8)


   - Name: `Windows Autopilot Standard`
   
   ![image](https://github.com/user-attachments/assets/5eeec53e-3082-457d-8535-0fcc5de26a70)

   - Set **User-driven** mode
   - Join to Entra ID as: **Azure AD joined**
   - Assign a device naming convention

## Why Unique Device Names Matter

### 1. Avoid Naming Conflicts
- Duplicate device names can cause issues during registration in Entra ID (Azure AD), Intune, or traditional Active Directory.
- Unique names help prevent conflicts and ensure accurate policy application.

### 2. Improve Device Management & Searchability
- Unique and structured names make it easier for administrators to locate, filter, and manage devices in Intune.
- Clear naming conventions support faster identification of devices based on users, locations, or departments.

### 3. Support Compliance & Auditability
- Many organizations require device tracking for compliance, security, and audits.
- Unique names assist with logging, asset inventory, and incident response processes.

### 4. Enable Automation & Scripting
- Scripts for automation, patching, or monitoring often rely on consistent naming.
- Unique names prevent errors in automated processes.

## Device Naming Rules and Constraints

| Rule                            | Reason                                                                 |
|---------------------------------|------------------------------------------------------------------------|
| Maximum of 15 characters        | Ensures compatibility with legacy NetBIOS naming requirements.        |
| Use only letters, numbers, and hyphens | Complies with Windows naming and DNS standards.                     |
| Cannot consist of only numbers  | Prevents conflicts with numeric-only identifiers.                     |
| No blank spaces                 | Avoids issues in scripts, parsing, and legacy system compatibility.   |

## Using Naming Macros in Intune

| Macro         | Description                                                                             |
|---------------|-----------------------------------------------------------------------------------------|
| `%SERIAL%`    | Inserts the device's hardware serial number for unique, hardware-based naming.          |
| `%RAND:x%`    | Inserts a random alphanumeric string of `x` characters to ensure unique device names.   |

### Examples

- `LAPTOP-%SERIAL%` → Generates names like `LAPTOP-ABC12345`
- `DESKTOP-%RAND5%` → Generates names like `DESKTOP-K7X2B`
     
  ![image](https://github.com/user-attachments/assets/caefd5ae-e10a-4fc3-bf64-dadee38706ce)


5. Assign the profile to the appropriate device group
![image](https://github.com/user-attachments/assets/47a0b58f-43f1-44f4-ad2f-360d70795c44)
![image](https://github.com/user-attachments/assets/895c804e-6efc-4cd8-831a-205ec2c277cd)

---

### **Step 3: Configure Compliance Policies**

**Definition:** Compliance policies define the rules that a device must meet to be considered compliant.

1. Go to **Endpoint security** > **Compliance policies** > **Create Policy**
2. Platform: **Windows 10 and later**

![image](https://github.com/user-attachments/assets/dd12aae6-a227-41ac-afe1-b863f6d2766d)
![image](https://github.com/user-attachments/assets/6cfe3604-c884-4bbd-821b-ebcc469df5d2)


3. Settings:
   - Require BitLocker: **Yes**
   - Minimum OS version: e.g., `10.0.19044`
   - Assign to the appropriate user or device group


![image](https://github.com/user-attachments/assets/e8919d71-3734-45c0-9e1b-f313d85ecd19)
![image](https://github.com/user-attachments/assets/394f1f4c-98bb-47ff-9253-7efb3f1c58b4)
![image](https://github.com/user-attachments/assets/c95ba074-0ec6-4d87-9533-f02fd02de68c)
![image](https://github.com/user-attachments/assets/9d0265c1-5590-412f-b11c-1f84e31ccd9a)

---

### **Step 4: Configure Update Rings (System Patching)**

**Definition:** Update rings manage the deployment of Windows updates and patches.

1. Navigate to **Devices** > **Windows Updates** > **Update rings** > **Create update ring policy**

![image](https://github.com/user-attachments/assets/9caec93c-20e1-43a2-b698-d21b1b8790f7)
![image](https://github.com/user-attachments/assets/dc09648b-694d-4282-89c0-456d370a91f0)

2. Create a profile:
   - Name: `Patch-Ring-Standard`
   - Set installation behavior and active hours
   - Configure restart settings

![image](https://github.com/user-attachments/assets/de3bb9f9-032f-4e5a-bed9-7b838e96c37c)
![image](https://github.com/user-attachments/assets/ed523d39-819d-4592-a9a3-93e59c076dd0)
![image](https://github.com/user-attachments/assets/d370e1cc-1f22-468e-b2b1-e36c6537f6c9)


4. Assign to the correct device group
![image](https://github.com/user-attachments/assets/4a777351-19ac-46e9-b59b-61d330831211)
![image](https://github.com/user-attachments/assets/0f4ff3c6-2d76-4461-b45f-ec6261d138de)

---

### **Step 5: Monitor Device Compliance**

**Definition:** This step checks which devices are compliant and which are not, using dashboards.

1. Navigate to **Devices** > **Windows**

![image](https://github.com/user-attachments/assets/4ed8c3c8-7891-42a2-8974-eea819a09f1a)
![image](https://github.com/user-attachments/assets/64eb9f03-164a-47c2-b4e0-372b1a48a92f)
![image](https://github.com/user-attachments/assets/99307ffe-e536-4899-801f-647139ee4927)

2. Review reports for:
   - Compliant / Non-compliant devices
   - Reasons for non-compliance
  
![image](https://github.com/user-attachments/assets/b2d68df8-3c0b-42d5-8376-8b8e7c404d29)

3. Export reports if necessary

---

### **Step 6: Integrate Microsoft Defender for Endpoint**

**Definition:** This integration brings in real-time threat and device risk scoring from Defender into Intune.

1. Navigate to **Microsoft 365 Defender** > **Settings** > **Endpoints** > **Onboarding**

![image](https://github.com/user-attachments/assets/60927fef-cf7f-4986-9fdc-1e4d755f1a06)
![image](https://github.com/user-attachments/assets/7d7d41e1-9929-4365-8305-5ea9321c87d6)

2. Choose platform: **Windows 10/11**
3. Select deployment method: **Microsoft Intune**
4. Download onboarding package and deploy using Intune

![image](https://github.com/user-attachments/assets/bf8b8adb-2136-4f02-8834-4d9142768c5f)

### Step 7: Onboard Devices to Microsoft Defender for Endpoint via Intune Script

In the Intune admin center: [https://endpoint.microsoft.com](https://endpoint.microsoft.com)

1. Navigate to:  
   **Devices** > **Windows** > **Scripts and Remediations** > **Platform Scripts**
2. Click **+ Add**


![image](https://github.com/user-attachments/assets/9fe17900-e2ea-481d-a427-084f91466aa2)
![image](https://github.com/user-attachments/assets/81b19af3-a3bd-4382-8a02-851ce867e6e3)


4. Enter a Name for the **Add PowerShell script** `Microsoft Defender Antivirus Policy`

![image](https://github.com/user-attachments/assets/84b477b3-cc8b-4769-af58-3e9eda959c1d)


5. The `.cmd` file extension needs to be changed to `.ps1` or Intune will not accept the file for deployment from the onboarding package.

![image](https://github.com/user-attachments/assets/effa0d7e-4ccd-4b8b-b049-7a49e084af38)

6. Assign the script to the device group used in the lab.

![image](https://github.com/user-attachments/assets/1058a6c1-7302-494c-9147-ae4fe36fd57d)
![image](https://github.com/user-attachments/assets/ec85e1a5-fa86-42aa-b40d-cd2176cb5257)

---

Once assigned, the script will execute on devices in the targeted group. Upon successful onboarding:

- Devices will begin reporting to **Microsoft Defender for Endpoint**
- Visibility will appear in **Microsoft 365 Defender** under:  
  **Assets** > **Devices**

![image](https://github.com/user-attachments/assets/9e81cc87-05fd-448c-a3a9-22b39d43e29f)
![image](https://github.com/user-attachments/assets/da90f846-2dc4-4809-98e0-43e856fd314a)
![image](https://github.com/user-attachments/assets/2ac61d82-f5d7-432f-af9f-69f9c5aa1a39)

Following successful onboarding of devices to Microsoft Defender for Endpoint via Intune script deployment, a security alert was generated in Microsoft Defender and an automated email notification was sent to the designated administrator. If the alert corresponds to a confirmed threat, remediation actions can be initiated immediately by the security team.

---

### **Step 8: Configure Conditional Access Based on Device Risk**

**Definition:** Conditional Access can block or allow access based on real-time risk scores from Defender.

1. Navigate to **Entra ID** > **Security** > **Conditional Access**
2. Create a policy:
   - Target: All users or a selected group
   - Cloud apps: Microsoft 365 apps
   - Conditions: Include non-compliant device state
   - Grant access: Block or require MFA
3. Add device risk as a condition and set threshold (e.g., medium or higher)

![image](https://github.com/user-attachments/assets/e4bd51ab-1e69-42a4-938a-838e3d33cd80)
![image](https://github.com/user-attachments/assets/fac0e175-568c-4cf6-82a6-530f91a1365b)

---

### **Step 9: Monitor Device Risk & Take Action**

1. Open **Microsoft 365 Defender** > **Endpoint** > **Device inventory**
2. Filter devices by **Risk Level**
3. Review security alerts, device timeline, and action center
4. Take action (e.g., isolate device, trigger automated investigation)

![image](https://github.com/user-attachments/assets/f5e02a68-d190-48ae-9108-0888b4afb841)
![image](https://github.com/user-attachments/assets/83664655-2011-4553-95fc-a830e0ce6e8a)
![image](https://github.com/user-attachments/assets/386a0938-f6d7-47b7-965a-ef56f989b421)


A simulated threat that would be detected as a suspicious PowerShell command typically falls under the "Suspicious PowerShell behavior" or "Suspicious script detected" alert categories.

## Is the Microsoft Defender Antivirus Scan Appropriate for Suspicious PowerShell Scenario?

Yes, Microsoft Defender Antivirus can detect and block many forms of malicious or suspicious PowerShell activity, especially when the **Real-Time Protection**, **Cloud Protection**, and **Behavioral Analysis** features are enabled. However, it’s not the only layer involved.

## What Defender Antivirus Does Cover in This Scenario

- Scans the script file (e.g., `.ps1`) for known signatures  
- Detects known malicious commands or behavior patterns  
- Monitors real-time execution of PowerShell using behavioral heuristics  
- Triggers alerts like:
  - *Suspicious PowerShell command line*
  - *Behavior of known malware families (e.g., Emotet, TrickBot)*
- Works with Windows Defender SmartScreen, AMSI, and Microsoft Defender for Endpoint to provide deeper visibility  

![image](https://github.com/user-attachments/assets/5b98180a-8752-48a9-b244-39b725705060)

---

### **Step 10: Protect the Tenant with Risk Score Recommendations**

**Definition:** Automatically enforce best practices using Microsoft Defender recommendations.

1. Open **Microsoft 365 Defender** > **Recommendations**
2. Review items like:
   - "Enable attack surface reduction rules"
   - "Configure exploit protection"
   - "Use tamper protection"
3. Select each recommendation and choose **Remediate** or deploy through Intune

---

## 🧾 **Conclusion**

This end-to-end configuration establishes a modern, secure, and automated management framework for Windows devices. With Microsoft Intune and Entra ID, devices can be enrolled seamlessly, evaluated for compliance, automatically updated, and monitored continuously. Defender for Endpoint adds intelligent risk scoring, and Conditional Access ensures that access to resources is governed by real-time security posture. This setup strengthens organizational security while simplifying endpoint management across the tenant.
