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


4. Configure **Additional local administrators on Azure AD joined devices** if needed

---

### 🔹 **Step 2: Create Intune Enrollment Profile**

**Definition:** Enrollment profiles configure the experience users get when enrolling their devices into Intune.

1. Navigate to [https://endpoint.microsoft.com](https://endpoint.microsoft.com)  
2. Go to **Devices** > **Windows** > **Windows enrollment** > **Deployment Profiles**
3. Create a **Windows Autopilot profile**
   - Name: `Windows Autopilot Standard`
   - Join to Azure AD: **Azure AD joined**
   - Assign a device naming convention
   - Set **User-driven** mode
4. Assign the profile to the appropriate device group

---

### 🔹 **Step 3: Configure Compliance Policies**

**Definition:** Compliance policies define the rules that a device must meet to be considered compliant.

1. Go to **Endpoint security** > **Compliance policies** > **Create Policy**
2. Platform: **Windows 10 and later**
3. Settings:
   - Require BitLocker: **Yes**
   - Minimum OS version: e.g., `10.0.19044`
   - Require Antivirus: **On**
   - Device health check: Require no TPM errors
4. Assign to the appropriate user or device group

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
