<h1>Creating and Applying Group Policy Objects (GPOs)</h1>

<b>Project Overview</b>

This project demonstrates how I configured and applied Group Policy Objects (GPOs) within my Active Directory home lab using Windows Server and a Windows 11 domain-joined workstation.

The objective was to practise centrally managing security settings, restricting user access to certain Windows features, and enforcing consistent configurations across domain users and computers.

I configured password and account lockout policies through the Default Domain Policy, then created separate GPOs for computer-level and user-level restrictions. Finally, I used gpupdate and gpresult to refresh and verify the applied policies.

<b>1. Configure the Domain Password Policy</b>

I configured the password policy within the Default Domain Policy, under:

Computer Configuration → Policies → Windows Settings → Security Settings → Account Policies → Password Policy

The following settings were configured:

Minimum password length: 12 characters

Enforce password history: Remember the previous 24 passwords

Minimum password age: 1 day

Maximum password age: 42 days

Password complexity: Enabled

Reversible password encryption: Disabled

These settings establish domain-wide password requirements for domain accounts, helping enforce stronger password practices.


<img src="Screenshots/Password policy.png"/>



<b>2. Configure the Domain Account Lockout Policy</b>

I configured the account lockout settings in the Default Domain Policy, under:

Computer Configuration → Policies → Windows Settings → Security Settings → Account Policies → Account Lockout Policy

The configured settings were:

Account lockout threshold: 3 invalid logon attempts

Account lockout duration: 30 minutes

Reset account lockout counter after: 30 minutes


<img src="Screenshots/Password lock out.png"/>


This configuration demonstrates how an Active Directory domain can respond to repeated failed authentication attempts and reduce the risk of password-guessing attacks.

Password and account lockout policies are configured at the domain level in this lab rather than in individual workstation GPOs.

<b>3. Create Computer-Based GPOs</b>

I created separate computer-focused GPOs to demonstrate how administrators can centrally manage workstation security settings.

<b>Disable USB Storage</b>

Configured the policy:

Computer Configuration → Policies → Administrative Templates → System → Removable Storage Access → All Removable Storage classes: Deny all access


<img src="Screenshots/Disable USB storage.png"/>


Enabled this setting to demonstrate how an organisation can restrict access to removable storage devices on managed computers.

<b>Enable Audit Logs</b>

Configured auditing through:

Computer Configuration → Policies → Windows Settings → Security Settings → Local Policies → Audit Policy


<img src="Screenshots/Audit logs.png"/>


Enabled auditing for system events, with both successful and failed events selected. This provides a basis for investigating system activity and troubleshooting security-related issues.

<b>Screen Lockout</b>

Configured the interactive logon machine inactivity limit to 600 seconds (10 minutes).


<img src="Screenshots/Screen lock out.png"/>


This demonstrates how a computer-level security setting can automatically lock an inactive workstation to reduce the risk of unauthorised access.

<b>Limit Software Installs</b>

Enabled the Prohibit User Installs policy under Windows Installer.


<img src="Screenshots/User installs.png"/>


This demonstrates a way to restrict certain per-user software installations. This policy alone does not prevent every method of installing software, but it provides an example of centrally managed application controls.

These GPOs were designed to apply to computers within the relevant organisational unit.

<b>4. Create User-Based GPOs</b>

I also created separate user-focused policies to restrict specific actions within the Windows user environment.

<b>Control Panel Restrictions</b>

Configured the policy:

User Configuration → Policies → Administrative Templates → Control Panel → Prohibit access to Control Panel and PC settings


<img src="Screenshots/Control panel restriction GPO.png"/>


Enabled the setting to prevent users from opening Control Panel and the Windows Settings app.

<b>Registry Restrictions</b>

Configured the policy:

User Configuration → Policies → Administrative Templates → System → Prevent access to registry editing tools


<img src="Screenshots/Registry restriction.png"/>


Enabled this setting to restrict access to Registry Editor through the standard Windows registry editing tools.

These policies demonstrate how user-based restrictions can follow a domain user account, rather than being configured individually on every workstation.

The user GPOs were linked to the appropriate Users organisational unit (OU).

<b>5. Understand the Difference Between Computer and User Policies</b>

An important part of this project was understanding the difference between computer and user configurations in Group Policy.

Computer configuration applies to the computer account and commonly manages workstation security and system settings.

User configuration applies to the user account and commonly manages the user's Windows environment and restrictions.

Separating these policies makes administration easier and helps ensure that the correct settings are applied to the intended users or computers.

The resulting behaviour depends on OU placement, GPO links, security filtering and policy precedence.

<b>6. Refresh and Verify the Applied Policies</b>

After configuring the GPOs, I refreshed Group Policy on the Windows 11 domain-joined workstation.

Ran the following command in Command Prompt: gpupdate /force. This requests an immediate refresh of computer and user Group Policy settings.

I then used: gpresult /r to review the resulting policy information.

<b>User Policy Verification</b>

The gpresult output under USER SETTINGS showed the following applied GPOs:

Workstation - Control Panel Restrictions

Workstation - Registry Restrictions


<img src="Screenshots/Applied user policies.png"/>


This confirms that the two user-focused GPOs were listed as applied for John Smith.

<b>Computer Policy Verification</b>

The gpresult output under COMPUTER SETTINGS showed the following applied GPOs:

Workstation - Disable USB Storage

Workstation - Enable Audit Logs

Workstation - Screen lockout

Workstation - Limit Software Installs

Default Domain Policy


<img src="Screenshots/Applied computer policies.png"/>


This confirms that the workstation received the listed computer-focused GPOs and the Default Domain Policy.

<b>Outcome</b>

Successfully configured domain password and account lockout requirements, created separate computer and user GPOs, and verified their application using Windows command-line tools.

<b>Skills Demonstrated</b>

Active Directory Group Policy management

Domain password and account lockout policies

Computer versus user policy processing

OU-based policy targeting

Windows security configuration

Basic auditing and workstation restrictions

Group Policy troubleshooting and verification using gpupdate and gpresult

