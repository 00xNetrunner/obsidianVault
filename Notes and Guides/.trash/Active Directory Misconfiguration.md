---
sticker: emoji//1f1e6-1f1e9
---
Certainly, Netrunner! I'll reformat the text you provided into a more structured format suitable for Obsidian Notes. Here's the reformatted version:

---

### 3. ACTIVE DIRECTORY MISCONFIGURATIONS
#### Overview:
There are numerous vulnerabilities in various applications and operating systems. It's crucial to report these to clients even if they can't be exploited directly. Below are some common misconfigurations in Active Directory:

---
#### 3.1 ABUSE DNSADMINS
- **3.1.1 Brief Description:** 
  - Members of the DNSAdmins group manage DNS and can easily add users under Windows Server.
	![[Pasted image 20231221230156.png|500]]
- **3.1.2 Attack:** 
  - This allows code execution as SYSTEM on domain controllers without needing domain admin rights.
- **3.1.3 Mitigation:** 
  - Restrict DNSAdmins group to admin accounts and limit DNS administration to admin systems. Include DNSAdmins in group audits.
- **3.1.4 References:** 
  - [DNSAdmin to DC Compromise](https://medium.com/@esnesenon/feature-not-bug-dnsadmin-to-dc-compromise-in-one-line-a0f779b8dc83)
  - [DNS Admin PrivEsc](https://medium.com/techzap/dns-admin-privesc-in-active-directory-ad-windows-ecc7ed5a21a2)
  - [Abusing DNSAdmins Privilege](http://www.labofapenetrationtester.com/2017/05/abusing-dnsadmins-privilege-for-escalation-in-active-directory.html)
  - [YouTube Video](https://www.youtube.com/watch?v=qrrEOkeukjk)

---
- [ ] Done
#### 3.2 VULNERABLE DCSYNC
- **3.2.1 Brief Description:** 
  - Domain Controllers in AD contain extensive network data, including user details, which they replicate regularly.
	![[Pasted image 20231221230428.png]]
- **3.2.2 Attack:** 
  - DCSync involves impersonating a DC to obtain credentials from other DCs without direct access.
- **3.2.3 Mitigation:** 
  - Identify and manage accounts with domain replication permissions.
- **3.2.4 References:** 
  - [DCSync and DCShadow Attacks](https://www.lepide.com/blog/what-are-dcsync-and-dcshadow-active-directory-attacks/)
  - [Kerberos & DCSync Attacks](https://www.qomplx.com/kerberos_dcsync_attacks_explained/)
  - [Dump Hashes with DCSync](https://www.ired.team/offensive-security-experiments/active-directory-kerberos-abuse/dump-password-hashes-from-domain-controller-with-dcsync)

---
- [ ] Done
#### 3.3 AS-REP ROASTING
- **3.3.1 Brief Description:** 
  - Targets Kerberos accounts without preauthentication, enabling offline brute-forcing of TGTs.
- **3.3.2 Attack:** 
  - Exploits disabled preauthentication to request TGTs for brute-forcing.
- **3.3.3 Mitigation:** 
  - Identify and fix accounts set to not require pre-authentication.
- **3.3.4 References:** 
  - [Exploiting AS-REP Roasting](https://akimbocore.com/article/asrep-roasting/)
  - [AS-REP Roasting Detection and Mitigation](https://stealthbits.com/blog/cracking-active-directory-passwords-with-as-rep-roasting/)

---
- [ ] Done
#### 3.4 ANONYMOUS LDAP QUERY
- **3.4.1 Brief Description:** 
  - LDAP protocol in AD can be queried anonymously if not secured.
- **3.4.2 Attack:** 
  - Enables dumping of password hashes by anonymous users.
- **3.4.3 Mitigation:** 
  - Disable anonymous LDAP queries.
- **3.4.4 References:** 
  - [Enabling Null Bind on LDAP](https://www.pwndefend.com/2021/02/25/how-to-enable-null-bind-on-ldap-with-windows-server-2019/)
  - [Disabling Unauthenticated Binds](https://blog.lithnet.io/2018/12/disabling-unauthenticated-binds-in.html)

---
- [ ] Done
#### 3.5 KERBEROASTING
- **3.5.1 Brief Description:** 
  - Attacks aimed at extracting password hashes through SPN tickets in AD.
- **3.5.2 Attack:** 
  - Targets service accounts to compromise administrator privileges.
- **3.5.3 Mitigation:** 
  - Use long and complex passwords for service accounts.
- **3.5.4 References:** 
  - [Kerberoasting Article](https://akimbocore.com/article/kerberoasting/)
  - [Kerberoasting Exploitation](https://www.ired.team/offensive-security-experiments