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

### 🔹 **Step 1: Enable Microsoft Intune Enrollment in Entra ID**

**Definition:** Enabling device registration ensures that Windows devices can join or register with Microsoft Entra ID and become managed by Intune.

1. Navigate to [https://entra.microsoft.com](https://entra.microsoft.com)  
2. Go to **Devices** > **Device settings**

![image](https://github.com/user-attachments/assets/7cb6726c-4b67-485e-99fb-2d2c1cdfd0bd)

3. Set **Users may join devices to Azure AD** to **All** or a selected group
4. Configure **Additional local administrators on Azure AD joined devices** if needed

![image](https://github.com/user-attachments/assets/8d759e25-ec5a-48db-8c54-1c89d3ed52b9)

---

### 🔹 **Step 2: Create Intune Enrollment Profile**

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

### 🔹 **Step 3: Configure Compliance Policies**

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

### 🔹 **Step 4: Configure Update Rings (System Patching)**

**Definition:** Update rings manage the deployment of Windows updates and patches.

1. Navigate to **Devices** > **Windows** > **Update rings for Windows 10 and later**
2. Create a profile:
   - Name: `Patch-Ring-Standard`
   - Set installation behavior and active hours
   - Configure restart settings
3. Assign to the correct device group

---

### 🔹 **Step 5: Monitor Device Compliance**

**Definition:** This step checks which devices are compliant and which are not, using dashboards.

1. Navigate to **Devices** > **Monitor** > **Compliance status**
2. Review reports for:
   - Compliant / Non-compliant devices
   - Reasons for non-compliance
3. Export reports if necessary

---

### 🔹 **Step 6: Set Up Remediation via Proactive Remediation Scripts**

**Definition:** Proactive Remediation uses scripts to automatically detect and fix issues before they impact the user.

1. Navigate to **Reports** > **Endpoint analytics** > **Proactive remediations**
2. Create a script package:
   - Detection script: Checks BitLocker status
   - Remediation script: Enables BitLocker if disabled
3. Assign the script package to a device group and schedule execution

---

### 🔹 **Step 7: Integrate Microsoft Defender for Endpoint**

**Definition:** This integration brings in real-time threat and device risk scoring from Defender into Intune.

1. Navigate to **Microsoft 365 Defender** > **Settings** > **Endpoints** > **Onboarding**
2. Choose platform: **Windows 10/11**
3. Select deployment method: **Microsoft Intune**
4. Download onboarding package and deploy using Intune
5. In Intune, navigate to **Endpoint security** > **Microsoft Defender Antivirus**
6. Ensure protection settings are enabled (real-time, cloud, sample submission)

---

### 🔹 **Step 8: Configure Conditional Access Based on Device Risk**

**Definition:** Conditional Access can block or allow access based on real-time risk scores from Defender.

1. Navigate to **Entra ID** > **Security** > **Conditional Access**
2. Create a policy:
   - Target: All users or a selected group
   - Cloud apps: Microsoft 365 apps
   - Conditions: Include non-compliant device state
   - Grant access: Block or require MFA
3. Add device risk as a condition and set threshold (e.g., medium or higher)

---

### 🔹 **Step 9: Monitor Device Risk & Take Action**

1. Open **Microsoft 365 Defender** > **Endpoint** > **Device inventory**
2. Filter devices by **Risk Level**
3. Review security alerts, device timeline, and action center
4. Take action (e.g., isolate device, trigger automated investigation)

---

### 🔹 **Step 10: Protect the Tenant with Risk Score Recommendations**

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
