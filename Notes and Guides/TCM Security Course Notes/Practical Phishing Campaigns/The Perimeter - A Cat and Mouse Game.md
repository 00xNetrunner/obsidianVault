---
sticker: lucide//fish
banner: https://process.fs.teachablecdn.com/ADNupMnWyR7kCWRvm76Laz/resize=width:705/https://cdn.filestackcontent.com/vIuC2QSyRNCclJ8oh3aQ
color: var(--mk-color-red)
tags:
  - TCM
  - Ethical_Hacking
  - "#phishing"
---
## What the Course Covers
- Email Phishing + Password Entry
- MFA Bypass Phishing 
- SMS Phishing

## Tools Used
- AWS Route 53
- AWS EC2
- Mailgun
- Gophish
- Evilginx
- Evilgophish


### MODERN SECURITY ARCHITECTURE - INBOUND
![[Pasted image 20240517202730.png]]

1. **Us (Public IP Address/Domain Name)**:
   - Represents the email sender, identified by their public IP address and domain name.

2. **Firewall**:
   - The first line of defense that filters incoming traffic to protect internal networks from unauthorized access and threats.

3. **Email Gateway/Inspector (Machine Learning)**:
   - Analyses incoming emails using machine learning to detect potential threats, such as phishing attempts, before they reach the spam filter.

4. **Spam Filter**:
   - Further filters emails to identify and block spam messages based on various criteria and rules.

5. **User Inbox**:
   - The final destination where legitimate emails that have passed through all security layers are delivered to the user.

### The Future Concept:
- **Trust Index**:
   - Integrates mailbox inspection, spam filters, and gateways to create a comprehensive trust index, enhancing overall email security by combining these advanced protective measures.
### MODERN SECURITY ARCHITECTURE - OUTBOUND
![[Pasted image 20240517203058.png]]
1. **User Clicks Link**:
   - The user receives an email and clicks on a link that turns out to be a phishing attempt.

2. **User Reports Phish**:
   - The user identifies the phishing attempt and reports it.

3. **Domain Filter Rules**:
   - The reported domain is analysed using several rules:
     - **Domain < 30 days old**: New domains are often flagged as suspicious.
     - **Whitelisting/Blacklisting**: Domains are checked against a whitelist or blacklist.
     - **Blackhole IP Address**: IP addresses associated with known threats are blocked.

4. **Vendor Filter Rules**:
   - Additional filtering is applied by external vendors:
     - **Safe Links/Google Filter**: URLs are checked against known safe link lists and Google’s filtering mechanisms.

5. **Us (Public IP Address/Domain Name)**:
   - The information is routed through the organization's system, identified by its public IP address and domain name, ensuring that such threats are filtered out and reported.


## MODERN SECURITY ARCHITECTURE - POST COMPROMISE (PASSWORD ONLY)

1. **Us (User)**:
   - The user enters their **username and password**.

2. **Service Provider**:
   - The credentials are sent to the service provider.

3. **Session ID**:
   - Upon successful login, a session ID is created and sent back, representing an authenticated session.

4. **Profit**:
   - An attacker uses the session ID to gain unauthorized access, leading to potential profit.

5. **MFA**:
   - The service provider sends an MFA request to the user.

6. **Vishing (Voice Phishing)**:
   - An attacker contacts the user through a vishing attack, pretending to be a legitimate entity.

7. **MFA Code**:
   - The user, tricked by the vishing attempt, provides the MFA code to the attacker.

## MODERN SECURITY ARCHITECTURE - POST COMPROMISE (MFA BYPASS) (EVILjynx)
![[Pasted image 20240517211322.png]]

1. **User Clicks Link**:
   - The user clicks on a phishing link received in their email.

2. **Domain Filter Rules**:
   - The domain of the link is checked against several rules:
     - **Domain < 30 days old**: Flags newly created domains.
     - **Whitelisting/Blacklisting**: Checks if the domain is on a whitelist or blacklist.

3. **Vendor Filter Rules**:
   - External vendor filters such as Safe Links or Google Filter further inspect the link for safety.

4. **Us (Username/Password)**:
   - The user inputs their username and password, which are sent to the service provider.

5. **Service Provider**:
   - The service provider receives the credentials and responds with a session token.

6. **MFA**:
   - The service provider prompts the user for MFA (Multi-Factor Authentication).

7. **Confirmation**:
   - The user confirms the MFA request, which involves receiving and providing an MFA code.

8. **Session Token**:
   - Upon successful MFA, the service provider issues a session token for the authenticated session.

9. **Profit**:
   - An attacker may use the session token to gain unauthorized access, leading to potential profit.
## To successfully phish today its necessary to:
- Have your domain be alive for more than 30 days ( **most companies filter new domains**)
- Build trust for the domain by visiting the site
- Ensure the site is categorised under malicious categories
- Domain takeover could be an option ( **Also why Business email compromise is popular** )

> [!NOTE]
> *Many times, this type of set up isn't possible so you need to work with the client to whitelist the domain and the sender email. this is your job and you don't have all the time in the world, unlike black hat or malicious hackers they have all the time in the world, they may have already set up domains that have been up for months.* 


## MFA CONSIDERATIONS
- If a client allows Text Message based MFA you can attempt to Vish the code from the user
	- Contact the victim saying you are from tech support, tell them they are gonna get a MFA code and for them to give it to you because their account is compromised. 
- While MFA is a strong mechanism, it is NOT infallible.
	- Evilginx will help us bypass MFA
- SIM Swapping attacks and other vectors can lead to MFA codes being intercepted
	- (Taking over a victims phone number)
- MFA Fatigue attacks are also becoming more popular
	- (Spam push notification attack, the user may get sick of these push notifications and hit allow)

## AVOIDING SPAM FILTERS
- When possible ( do not copy Microsoft email templates as this will invoke spam filters)
- if you like HTML editing this can be good as you can make a nice looking email 
	- Avoid urgent words if you can, use spell check software or AI tools
- Try to customise each phish to the client including their dialect and tendencies
- Your compromised is only as good as your OSINT and passive info gathering
	- Gather info on their common IT tools and the emails sent by those tools

## WHITELISTING CONSIDERATIONS

- Work with your client to ensure that the domain/IP is whitelisted in multiple places. 
- Email Gateway 
- Spam Filter
- Domain Whitelist 
- IP Address Whitelist

> [!WARNING]
> *Send the email a few times and test directly with the client to ensure that emails can get past the spam filter and are received*




