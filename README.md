<h1>Vulnerability Assessment Report</h1>

<b>Target System:</b> Hostname Kioptrix_3, IPv4 Address 192.168.77.102

<b>Date of Assessment:</b> November 11, 2026

<b>Assessed By:</b> Peter Mancina


<h2>1. Executive Summary</h2>

- **Objective:** To identify and evaluate security weaknesses on the Kioptrix_3 system that could lead to unauthorized access, data loss, or service interruption.
- **High-Level Findings:** I identified 24 vulnerabilities, including 3 Critical and 2 High-severity risks.
- **Business Impact:** These vulnerabilities represent a direct risk to data confidentiality, intellectual property, system availability, and brand trust. If exploited, an attacker could potentially gain full administrative control and exfiltrate customer records from the company database.
- **Overall Risk Rating:** Critical, pending complete confirmation of exploitability.
- **Recommendation for Further Assessment:** This report is based primarily on system vulnerability scanning and requires future validation through a full penetration test.


<h2>2. Risk Matrix</h2>

| Severity | Count | Potential Impact |
| --- | ---: | --- |
| **Critical** | 3 | Immediate risk to business continuity and data integrity. |
| **High** | 2 | Could allow unauthorized access or lateral movement. |
| **Medium** | 4 | Requires privileged access; potential for secondary exploitation. |
| **Low** | 15 | Informational; suggests hardening improvements. |




<h2>3. Target System Specifications Identified</h2>

<b>Operating System:</b> Linux Ubuntu 2.6.X (2.6.9-2.6.33)

<b>Open Ports, Service, Service Version:</b>

<pre><b>22/tcp, ssh, OpenSSH 4.7p1 Debian 8ubuntu1.2 (protocol 2.0)
80/tcp, http, Apache httpd 2.2.8 - Ubuntu PHP 5.2.4-2ubuntu5.6 with Suhosin-Patch</b></pre>


<h3>Nmap Port and Service Scan</h3>
<p align="left">
<br/>
<img src="https://i.imgur.com/SRW20An.png" height="60%" width="60%" alt="Nmap port and service scan image"/>
</p>


<h3>Nmap Operating System Enumeration Scan</h3>
<p align="left">
<br/>
<img src="https://i.imgur.com/4GOGIXL.png" height="60%" width="60%" alt="Nmap OS enumeration scan image"/>
</p>

<h2>4. Detailed Vulnerability Findings</h2>

<h3>Critical Vulnerabilities</h3>

<h4>C1. LotusCMS 3.0 eval() Remote Code Execution</h4>

- **Severity:** Critical (No CVSS score available)
- **Description:** Kioptrix3.com publicizes its use of LotusCMS 3.0 on the site's login page. LotusCMS 3.0’s router passes the page request value to PHP’s eval() function without safely handling it. This vulnerability test validated this vulnerability is exploitable and allowed the tester to make the web server run PHP code.
- **Business Impact:** An attacker can run code with the web server’s permissions, potentially accessing or changing website data, disrupting the service, or gaining an initial foothold on the machine.
- **Evidence:** The Kioptrix 3 website uses LotusCMS, and the Metasploit module specifically targets the vulnerable page parameter and eval() behavior. See screen shots below.
- **Remediation:** Remove or replace the vulnerable LotusCMS installation with maintained software. If replacement is not immediately possible, restrict access to the application until it can be remediated.

<p align="left">
<br/>
<img src="https://i.imgur.com/4b5I17K.png" height="30%" width="30%" alt="Lotus 3.0 webapp image"/>
</p>
<b>Image above: Login Lotus 3.0 Web Application</b>
</p>
<p align="left">
<br/>
<img src="https://i.imgur.com/3osXPAP.png" height="80%" width="80%" alt="Searchsploit Lotus 3.0 modules image"/>
</p>
<b>Image above: Searchsploit entries for scripts that exploit Lotus 3.0</b>
</p>
<p align="left">
<br/>
<img src="https://i.imgur.com/zeWTk48.png" height="80%" width="80%" alt="Metasploit Lotus 3.0 script payload image"/>
</p>
<b>Image above: Metasploit payload details to exploit Lotus 3.0</b>
</p>
<p align="center">
<br/>
<img src="https://i.imgur.com/zzT7tAf.png" height="100%" width="100%" alt="Exploitation of Lotus 3.0 image"/>
</p>
<b>Image above: Use of Lotus 3.0 url command injection exploit to set up a shell on Kioptrix3</b>
</p>
<p align="center">
<br/>
<img src="https://i.imgur.com/whVe9vT.png" height="100%" width="100%" alt="Exploitation of Lotus 3.0 image"/>
</p>
<b>Image above: Vulnerability Assessment machine remotely issuing the command whoami on the Kioptrix_3 webserver</b>
</p>

<h4>C2. PHP 5.2 End-of-Life Software</h4>

- **Severity:** Critical (based on outdated, unsupported software; not a CVSS score)
- **Description:** The website appears to use PHP 5.2, a version that no longer receives security updates from the PHP project. PHP 5.2 reached end of life on January 6, 2011. 
- **Business Impact:** If the server is running an unpatched version, newly discovered security flaws may remain unfixed, increasing the chance that an attacker could compromise the website, its data, and take control of the entire system itself. The outdated version may also make it harder to maintain or update the website safely.
- **Evidence:** Nmap scans and a Nikto scan confirmed the PHP 5.2 version. See Nikto scan image at the end of this section, result #800262.
- **Remediation:** Upgrade PHP and test the website for compatibility.

<h4>C3. Outdated Linux kernel</h4>

- **Severity:** Critical
- **Description:** Koptrix_3 is running an outdated and unsupported Linux kernel. If it is no longer receiving security updates, known flaws may remain unpatched.
- **Business Impact:** An attacker who can reach the machine may be able to exploit an applicable kernel flaw to disrupt the system or gain greater control. The risk depends on the exact kernel build, distribution patches, and whether an applicable vulnerability can be reached.
- **Evidence:** Nmap scans reported an outdated Linux kernel 
- **Remediation:** Upgrade the operating system to a supported release and install its latest vendor-provided kernel updates, then reboot into the updated kernel.



<h3>High-level Vulnerabilities</h3>

<h4>H1. Outdated Apache HTTP Server (version 2.2.8)</h4>

- **Severity:** High (unsupported software; CVSS not assigned based on version detection alone)
- **Description:** The server reports Apache 2.2.8, an old release from the Apache 2.2 series. The Apache project has ended support for the 2.2 series, so it no longer receives security fixes from the project. 
- **Business Impact:** If this version is running without relevant vendor-provided fixes, known security flaws could put the website at risk of disruption, data exposure, or unauthorized access. The version number alone does not prove that a particular flaw is exploitable.
- **Evidence:** Nmap and Nikto scans identified Apache/2.2.8. See Nikto scan results evidence at the end of this section, result #600050.
- **Remediation:** Confirm the installed package and any vendor backported fixes on the machine. If confirmed, upgrade to a supported Apache release and test the website for compatibility.


<h4>H2. CVE-2011-3192:  Kioptrix's Webserver Software Apache HTTP 2.2.8 Range-header denial of service</h4>

- **Severity:** High  (CVSS 7.8)
- **Description:** An attacker could send a specially crafted web request via a "Range header" asking the server to return many overlapping parts of the same file. 
- **Business Impact: Processing attacker requests via this vulnerability could use excessive memory and computing power, slowing the website or making it unavailable to other users (a "Denial of Service" attack). 
- **Evidence:** This vulnerability could be exploited and pertains to all Apache HTTP Server versions 2.2.X, including the version running on Kioptrix_3. See details at https://nvd.nist.gov/vuln/detail/cve-2011-3192 
- **Remediation:** Upgrade Apache HTTP Server to a currently supported version, as Apache 2.2.8 is outdated and unsupported. Until then, apply an Apache-approved mitigation to limit or reject excessive byte-range requests. 



<h3>Medium Vulnerabilities Related to the Kioptrix.com Webserver</h3>

<h4>M1. CVE-2011-0419: Apache mod_autoindex denial of service</h4>

- **Severity:** Medium  (CVSS 4.3)
- **Description:** A flaw in how certain older versions of Apache and other systems handle particular wildcard patterns. 
- **Business Impact:** This vulnerability known as a "stack consumption vulnerability" could let an attacker send a specially crafted request to the Kioptrix_3 webserver that makes the system use excessive CPU and memory. This could slow down or temporarily prevent the webserver from responding to legitimate users (a "Denial of Service" attack). 
- **Evidence:** This vulnerability could be exploited and pertains to all Apache HTTP Server versions prior to 2.2.18, including the version running on Kioptrix_3. See details at https://nvd.nist.gov/vuln/detail/cve-2011-0419
- **Remediation:** Upgrade Apache HTTP Server to a currently supported version. If upgrading cannot be done immediately, disable mod_autoindex if the website does not require directory listings.

<h4>M2. phpMyAdmin files exposed</h4>

- **Severity:** Medium if the phpMyAdmin login or administrative functions are publicly accessible and inadequately protected; otherwise Low/Informational based on these scan results alone.
- **Description:** Nikto found /phpmyadmin/changelog.php and /phpmyadmin/Documentation.html. These pages suggest phpMyAdmin files are present and reachable, but the results do not confirm that its database-management interface is accessible or that anyone can log in.
- **Business Impact:** If the administrative interface is reachable and poorly protected, or administrative credentials to the database are stolen, an attacker may attempt to access or alter databases directly from the internet. The changelog and documentation pages alone may disclose software information but do not demonstrate database access. 
- **Evidence:** Nikto results #001795 (/phpmyadmin/changelog.php) and #006672 (/phpmyadmin/Documentation.html). See the Nikto scan results evidence at the end of this section, results #001795 and 006672.
- **Remediation:** Confirm whether the phpMyAdmin login and administrative functions are accessible from the internet. If the tool is not needed, remove it. Otherwise, restrict access to authorized users and trusted networks, and keep it updated. Remove public internet access to phpMyAdmin files.



<h3>Medium Vulnerabilities Related to Kioptrix_3's SSH Software OpenSSH version 4.7p1</h3>

<h4>M3. CVE-2018-15473: OpenSSH user enumeration</h4>

- **Severity:** Medium  (CVSS 5.3)
- **Description:** An attacker may be able to check whether a username exists on the SSH server by observing how it responds to login attempts. 
- **Business Impact:** This can reveal valid account names, but it does not let the attacker log in or access the account by itself. However, once usernames are known, the attacker can use other tools to try to determine the password ("Brute force attacks") and gain access to company systems.
- **Evidence:** This vulnerability could be exploited and is present in all OpenSSH versions through version 7.7, including the version on Kioptrix_3. See details at https://nvd.nist.gov/vuln/detail/cve-2018-15473
- **Remediation:** Upgrade OpenSSH to a version that includes a fix for CVE-2018-15473. Also use strong passwords and limit repeated login attempts to reduce the risk of password-guessing attacks. 

<h4>M4. OpenSSH Privilege Separation Disabled Vulnerability</h4>

- **Severity:** Medium  (No CVSS score available)
- **Description:** Under a specific configuration—"SSH privilege separation disabled"—a person who already has compromised a local user account on Kioptrix_3, could potentially use SSH forwarding behavior to gain more privileges, possibly administrator (root)
- **Business Impact:** If exploited, an intruder with a compromised user account could take control of the system, access or alter data, and use the machine to reach other systems.
- **Evidence:** Searchsploit entry 40962 "UsePrivilegeSeparationDisabled" script
- **Remediation:** Upgrade OpenSSH to a currently supported version. Until then, ensure privilege separation is enabled and restrict SSH access to trusted users. 

Image: Searchsploit scripts for exploiting OpenSSH 4.7p1

<p align="center">
<br/>
<img src="https://i.imgur.com/LtOfEjP.png" height="60%" width="60%" alt="Searchsploit scripts for exploiting OpenSSH 4.7p1"/>
</p>


  
<h3>Low and Informational Vulnerabilities Related to Kioptrix's Use of OpenSSH version 4.7p1</h3>

<h4>L1 CVE-2008-5161: OpenSSH CBC plaintext recovery</h4>

- **Severity:** Low  (CVSS 3.7)
- **Description:** A flaw in how certain older SSH software, including the version running on Kioptrix_3, handles encrypted network traffic (only when using block cipher algorithm in "Cipher Block Chaining" (CBC) mode. 
- **Business Impact:** This vulnerability could allow a remote attacker who can observe and interfere with SSH traffic to recover some of the information being sent in "plaintext" (not encoded or encrypted). This may expose parts of sensitive data exchanged during an SSH session and give the attacker remote access to internal systems and data. 
- **Evidence:** This vulnerability could be exploited and pertains to the OpenSSH 4.7p1 version running on Kioptrix_3. See details at https://nvd.nist.gov/vuln/detail/cve-2008-5161
- **Remediation:** Upgrade OpenSSH to a currently supported version. Until then, disable CBC-mode ciphers and use supported, secure cipher modes instead. 



<h3>Low and Informational Vulnerabilities Related to the Kioptrix3.com Webserver</h3>

<h4>L2. PHP session cookie missing HttpOnly flag</h4>

- **Severity:** Low (CVSS not assigned; impact depends on the application and other vulnerabilities) 
- **Description:** The website sets its PHPSESSID session cookie without the HttpOnly setting. This setting tells browsers not to expose the cookie to webpage scripts. 
- **Business Impact:** If an attacker can get malicious script to run in a user’s browser—for example, by exploiting a separate cross-site scripting flaw—the script may be able to read the session cookie. If stolen, the cookie could potentially be used to impersonate that user until the session expires or is invalidated. This finding alone does not show that an attacker can steal a cookie or access an account. 
- **Evidence:** Nikto reported that PHPSESSID cookies are created without the HttpOnly flag. See Nikto scan image at the end of this section, result #000029.
- **Remediation:** Configure the application to set HttpOnly on session cookies; also use Secure when the site is served over HTTPS and review for cross-site scripting vulnerabilities. 

<h4>L3. CWE-548: Directory Listing enabled</h4>

- **Severity:** Low (impact depends on what the directory contains) 
- **Description:** The web server allows visitors to view a list of files in the /icons/ directory instead of restricting access to the directory contents. 
- **Business Impact:** A visitor may learn what files are present and access files that are publicly available there. If sensitive or unintended files are stored in the directory, this could reveal information useful for further attacks. This finding alone does not show that sensitive data is exposed. 
- **Evidence:** Nikto reported directory indexing at /icons/. See Nikto scan image at the end of this section, result #750500.
- **Remediation:** Disable directory listings on the web server and ensure that only intended, public files are stored in web-accessible directories. 

<h4>L4. HTTP TRACE method enabled (potential Cross-Site Tracing exposure)</h4>

- **Severity:** Low (TRACE being enabled alone does not confirm a usable attack) 
- **Description:** The web server accepts the HTTP TRACE method and echoes the request back in its response. In some circumstances, this behavior could expose information included in request headers. Cross-Site Tracing would generally require additional weaknesses, such as a way to make a user’s browser send the request; modern browsers restrict this kind of use. 
- **Business Impact:** If the necessary conditions were present, an attacker might be able to access sensitive information from a user’s request, potentially including session-related data. The Nikto result by itself does not show that this information can be stolen or that an account can be compromised. 
- **Evidence:** Nikto reported that TRACE is active and receives a response. See Nikto scan results evidence at the end of this section, result #000434. For more details on Cross Site Tracing visit: https://owasp.org/www-community/attacks/Cross_Site_Tracing 
- **Remediation:** Disable the method if the site does not require it, then retest 

<h4>L5. CVE-2003-1418: Inode information exposed in ETag header</h4>

- **Severity:** Low (limited information disclosure)
- **Description:** The server’s ETag response for /favicon.ico includes the file’s internal filesystem number (inode), along with its size and modification time. This reveals some system and file metadata, but does not by itself provide access to the file or other data. 
- **Business Impact:** The exposed details could give an attacker minor information about the server and its files. In unusual setups involving older network file-sharing services, inode information may contribute to a larger risk; this finding alone does not establish that such a service is present or accessible. 
- **Evidence:** Nikto reported an ETag for /favicon.ico containing inode 631780, size 23126, and modification time Fri June 5 15:22:00 2009. See Nikto scan results evidence at the end of this section, result #999984. For more details see: https://nvd.nist.gov/vuln/detail/CVE-2003-1418 
- **Remediation:** Configure the web server’s ETag behavior to omit inode information, then retest. 

<h4>L6. Missing X-Content-Type-Options security header</h4>

- **Severity:**  Low (defense-in-depth issue; risk depends on how the site serves content)
- **Description:** The website does not send the X-Content-Type-Options: nosniff header. Without it, browsers may try to guess a file’s type instead of following the type declared by the server. This can create risk if content is mislabeled, especially if users can upload files.
- **Business Impact:** In certain circumstances, a browser might interpret a file as executable content when it should be treated as something else. This could contribute to a cross-site scripting attack, but the missing header alone does not demonstrate that such an attack is possible. 
- **Evidence:** Nikto reported that the X-Content-Type-Options header is missing. See Nikto scan results evidence at the end of this section, results # 013587 and 007352. For more details see: https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/X-Content-Type-Options
- **Remediation:** Configure the web server to send X-Content-Type-Options: nosniff and ensure responses use the correct content type. 

<h4>L7. Missing Strict-Transport-Security (HSTS) header</h4>

- **Severity:** Low (applies only if the site supports HTTPS; header absence alone does not confirm an attack)
- **Description:** The server does not send an HSTS header. When received over a secure HTTPS connection, this header tells browsers to use HTTPS for future visits, rather than allowing an insecure HTTP connection. 
- **Business Impact:** If the site supports both HTTP and HTTPS, a user’s initial visit over HTTP could be vulnerable to interception or redirection to an insecure version of the site. HSTS can reduce this risk after the browser has received the header over HTTPS. The header cannot protect a site that does not use HTTPS. 
- **Evidence:** Nikto reported that the Strict-Transport-Security header is missing. See Nikto scan results evidence at the end of this section, result #013587.  See: https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Strict-Transport-Security
- **Remediation:** If the site is intended to support HTTPS, configure TLS and send HSTS on HTTPS responses. 

<h4>L8. PHP Easter Egg information disclosure</h4>

- **Severity:** Low
- **Description:** Certain special query strings cause older PHP versions to return built-in content, such as PHP or Zend logos and PHP credits. The credits response may reveal details that help identify the PHP version or configuration. This feature was removed in PHP 5.5, so it may be present in the PHP 5.2 version reported on Kioptrix 3. 
- **Business Impact:** An attacker could use the exposed information to identify the software and narrow down which known weaknesses to investigate. This finding alone does not allow them to log in, run commands, or access protected data. 
- **Evidence:** Nikto reported responses to the four PHP Easter Egg query strings [001384] through [001387]. See Nikto scan results evidence at the end of this section, results #001384,  001385,  001386,  001387. See: https://labs.detectify.com/writeups/do-you-dare-to-show-your-php-easter-egg/
- **Remediation:** Confirm what each request actually returns; the logo responses may disclose little beyond confirming the feature is enabled. Disable PHP Easter Egg responses or turn off expose_php if the application does not require it.

<h4></h4>

- **Severity:** Informational / Low 
- **Description:** The website does not send a Permissions-Policy header. This header lets a site limit which browser features—such as the camera, microphone, or location—its pages and embedded content are allowed to use. 
- **Business Impact:** Without a policy, the site is not explicitly restricting these features through this header. The missing header alone does not let an attacker access a user’s camera, microphone, or location; browser permissions and other protections still apply.
- **Evidence:** Nikto reported that the Permissions-Policy header is missing. See Nikto scan results evidence at the end of this section, results #013587. For more details see:** https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Permissions-Policy
- **Remediation:**  Configure a policy that allows only the browser features it needs.

<h4>L10. Missing Referrer-Policy header</h4>

- **Severity:** Informational / Low 
- **Description:** The website does not set a Referrer-Policy header to control what page information a browser shares when following links or requesting resources. Depending on the browser’s default behavior, some URL details—such as a page path or query string—may be sent to another site. 
- **Business Impact:** If sensitive information is included in a page’s URL, it could be disclosed to another site when the browser loads that site’s content or follows a link. The missing header alone does not show that sensitive information is being exposed.
- **Evidence:** Nikto reported that the Referrer-Policy header is missing. See Nikto scan results evidence at the end of this section, results #013587. For more details see: https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Referrer-Policy 
- **Remediation:** Configure a suitable policy, such as strict-origin-when-cross-origin, or a stricter policy if appropriate for the site.

<h4>L11. Missing Content-Security-Policy (CSP) header</h4>

- **Severity:** Informational / Low 
- **Description:** The website does not send a CSP header telling browsers which sources of scripts and other content are allowed. A suitable policy can help limit the damage from certain attacks, including cross-site scripting (XSS). 
- **Business Impact:** If a separate flaw lets an attacker inject content into a page, the lack of CSP removes one potential layer of protection against that content running in visitors’ browsers. The missing header alone does not mean the site has an XSS flaw or that an attack is possible. 
- **Evidence:** Nikto reported that the Content-Security-Policy header is missing. See Nikto scan results evidence at the end of this section, results #013587. For more details see: https://developer.mozilla.org/en-US/docs/Web/HTTP/CSP
- **Remediation:** If the site is maintained, develop and test a policy that permits only the content the site needs; an overly restrictive policy can break site functionality.

<h4>L12.  Unexpected responses to invalid HTTP methods</h4>

- **Severity:** Informational 
- **Description:** The web server returned a valid response when Nikto tried unusual or invalid HTTP request methods. This behavior can cause scanners to report false positives; by itself, it does not demonstrate a security weakness. 
- **Business Impact:** No direct business impact is established from this result alone. A concern would depend on whether the server, a proxy, or an application handles these methods in a way that bypasses security controls. 
- **Evidence:** Nikto reported a valid response to “junk” HTTP methods. See Nikto scan results evidence at the end of this section, result# 999967.
- **Remediation:** Treat this as an observation, not a confirmed vulnerability, unless further testing shows that an unsupported method can access or change protected data. 

<h4>L13. Apache default README file exposed</h4>

- **Severity:** Informational / Low 
- **Description:** The default Apache README file is accessible at /icons/README. It contains general information about Apache’s icon files and may help identify default web-server content, but it does not by itself expose sensitive data or provide access to the server. 
- **Business Impact:** An attacker could use the file as a small clue when identifying the web server and its default configuration. No direct impact to data or service availability is established by this finding alone.
- **Evidence:** Nikto reported /icons/README as an Apache default file.  See Nikto scan results evidence at the end of this section, results #003584. For more details see: https://www.vntweb.co.uk/apache-restricting-ac
- **Remediation:** Remove the file or restrict access to unnecessary default content if it is not required.

<h4>L14. X-Frame-Options / frame-ancestors configuration</h4>

- **Severity:** Informational 
- **Description:** Nikto flagged the use of X-Frame-Options and recommends using CSP’s frame-ancestors directive. These controls help prevent a site from being embedded in another site’s frame, which can reduce clickjacking risk.
- **Business Impact:** If the site has no effective framing restriction, an attacker could potentially embed a page in a deceptive site and trick users into interacting with it. This scan message alone does not establish that the site is unprotected; the X-Frame-Options value matters.
- **Evidence:** Nikto reported this at /.  See Nikto scan results evidence at the end of this section, result #007342. For more details see: https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Headers/X-Frame-Options
- **Remediation:** Check the response headers: if X-Frame-Options is set to DENY or SAMEORIGIN, it provides framing protection. Consider adding an appropriate CSP frame-ancestors directive as well.

<h4>L15. Username of an employee (loneferret) is displayed on the website blog</h4>

- **Severity:** Informational / Low (a username alone does not provide account access) 
- **Description:** A public blog post on the Kioptrix 3 site identifies loneferret as an employee. This discloses a username that may also correspond to a system account. The username is not a secret credential, but revealing it can make account-targeted attacks easier. 
- **Business Impact:** An attacker could use the known username when attempting to guess passwords, exploit weak or reused credentials, or use tools to perform an automated brute force password cracking attack. This finding alone does not allow the attacker to log in or establish that the account’s password is weak.
- **Evidence:** The blog post publicly mentions loneferret. See the website screenshot see image below
- **Remediation:** Avoid publishing system account names where they are not needed. Use strong, unique passwords, rate-limit or otherwise protect login attempts, and avoid reusing passwords between services.

<p align="left">
<br/>
<img src="https://i.imgur.com/maPE9lo.png" height="70%" width="70%" alt="Webserver Blog page publicizing employee username"/>
</p>
<b>Image above: Webserver Blog page publicizing employee username loneferret</b>


<h3>Nikto Scan Results Source Evidence</h3>

<p align="left">
<br/>
<img src="https://i.imgur.com/49PyeXh.png" height="80%" width="80%" alt="Nikto Scan Results"/>
</p>

<h2>4. Remediation Plan</h2>

| Vulnerability | Recommended Correction | Priority |
| --- | --- | --- |
| C1 | Remove or replace the vulnerable LotusCMS installation with maintained software. If replacement is not immediately possible, restrict access to the application until it can be remediated. | Immediate |
| C2 | Upgrade PHP and test the website for compatibility. | Immediate |
| C3 | Upgrade the operating system to a supported Linux release and install its latest vendor-provided kernel updates, then reboot into the updated kernel.| Immediate |
| H1 | Confirm the installed Apache HTTP Server package and any vendor back-ported fixes on the machine. If confirmed, upgrade to a supported Apache release and test the website for compatibility. | Short-term |
| H2 | Remediation consistent with H1 | Short-term |
| M1 | Remediation consistent with H1. If upgrading the Apache server cannot be done in the short-term, disable mod_autoindex if the website does not require directory listings. | Short-term |
| M2 | Confirm whether the webserver's phpMyAdmin login and administrative functions are accessible from the internet. If the tool is not needed, remove it. Otherwise, restrict access to authorized users and trusted networks, and keep it updated. Remove public internet access to phpMyAdmin files. | Short-term | 
| M3 | Upgrade OpenSSH to a version that includes a fix for CVE-2018-15473. Also use strong passwords and limit repeated login attempts to reduce the risk of password-guessing attacks. | Short-term |
| M4 | Upgrade OpenSSH to a currently supported version. Until then, ensure privilege separation is enabled and restrict SSH access to trusted users. | Short-term |
| L1 | Upgrade OpenSSH to a currently supported version. Until then, disable CBC-mode ciphers and use supported, secure cipher modes instead. | As soon as possible |
| L2 | Configure the application to set HttpOnly on session cookies; also use Secure when the site is served over HTTPS and review for cross-site scripting vulnerabilities. | As soon as possible |
| L3 | Disable directory listings on the web server and ensure that only intended, public files are stored in web-accessible directories. | As soon as possible |
| L4 | Disable the HTTP TRACE method if the site does not require it, then retest | When possible |
| L5 | Configure the web server’s ETag behavior to omit inode information, then retest. | When possible |
| L6 | Configure the web server to send X-Content-Type-Options: nosniff and ensure responses use the correct content type. | When possible |
| L7 | If the site is intended to support HTTPS, configure TLS and send HSTS on HTTPS responses. | When possible |
| L8 | Confirm what each query string request actually returns. Disable PHP Easter Egg responses or turn off expose_php if the application does not require it. | When possible |
| L9 | Configure a permissions policy that allows only the browser features it needs. | When possible |
| L10 | Configure a suitable policy, such as strict-origin-when-cross-origin, or a stricter policy if appropriate for the site. | When possible |
| L11 | If the site is maintained, develop and test a policy that permits only the content the site needs; an overly restrictive policy can break site functionality. | When possible |
| L12 | Treat this as an observation, not a confirmed vulnerability, unless further testing shows that an unsupported method can access or change protected data. | When possible |
| L13 | Remove the README file or restrict access to unnecessary default content if it is not required. | When possible |
| L14 | Check the response headers: if X-Frame-Options is set to DENY or SAMEORIGIN, it provides framing protection. Consider adding an appropriate CSP frame-ancestors directive as well. | When possible |
| L15 | Informational: Avoid publishing system account names where they are not needed. Use strong, unique passwords, rate-limit or otherwise protect login attempts, and avoid reusing passwords between services. | When possible |

<h2>5. Security Recommendations: General Hardening</h2>

<h3>1. Least Privilege</h3>
Ensure users/services on this machine only have the permissions necessary for their function. Further protect user accounts by implementing Multi-factor Authentication (MFA) such as a smart-phone Authenticator application (Duo, Authy, Microsoft Authenticator, Google Authenticator) and a secure password manager application.  Remove internet-facing administrative login portals, and protect staff usernames by refraining from posting them publicly online. 
<h3>2. Patch Management</h3>
Implement an automated cadence for security updates for application software, operating system software, and firmware. Create a patch management approval process for emergency situations in which new vulnerabilities for critical systems can be patched quickly albeit with clear roll-back procedures and backup management protocols. 
<h3>3. Monitoring</h3>
Install Endpoint Detection and Response with User Behavior Analytics and a Host-based Intrusion Detection System on Kioptrix_3 to ensure that AI-supported monitoring prevents anomalous activity, changes to core system files, prevents malware execution, and alerts security staff of unauthorized intrusions. Additionally, ensure this host contains a SIEM forwarder client to send logs to a centralized SIEM to detect future unauthorized attempts.
<h3>4. Training</h3>
Train all employees on secure password creation, password manager and MFA usage, and policies for public positing of company and user information online

<h2>6. Conclusion</h2>

Addressing the critical and high-severity findings detailed in this report will significantly mitigate the risk of compromise for Kioptrix_3. We recommend immediate scheduling of the remediation tasks provided in Section 4.

<!--
 ```diff
- text in red
+ text in green
! text in orange

