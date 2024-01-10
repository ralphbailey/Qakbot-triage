# Qakbot Triage
The goal of this lab is to triage a malicious PCAP using Wireshark and build a report based off CISA standards. Showcasing indicators and technical details , executive summary, technical summary, findings and analysis, and remediations. 

## Indicators and Technical Details
![Table 1](https://i.imgur.com/2cDsIEr.png)
![Table 2](https://i.imgur.com/aH1kipq.png)

## Executive Summary
On December 27, 2019 at approximately 16:06 the security operations team was notified of a possible malicious event on a network computer. Our protection software alerted us that a known network address associated with malware named “Qakbot” was spotted on our network. The malware opened a few different web pages, sent spam email traffic, sent various email credentials, and created a lot of unencrypted and encrypted web traffic. Upon further investigation, only the single network computer was affected, and it did not spread to any other computers on the network. The security operations team is confident that no company data was harmed or lost by this incident.  

## Technical Summary
On December 27, 2019 at approximately 16:06 the security operations team was notified of a possible Qakbot infection on host 10.12.27.101. The initial GET request to 164.132.235.17 with a URL of centre-de-conduite-roannais[.]com/wp-content/uploads/2019/12/last/444444.png at 16:06:16 resulted in the download of a .PNG file, which contained the Qakbot Trojan. The SHA256 hash was extracted from the file and run through Virus Total returning a score of 61/72 and indicating Qakbot trojan. 

After infection, the host started communicating with various IP addresses over SSL/TLS traffic using port 443 with no associated domain. When observing certificates for other encrypted traffic with associated domains, the certificates were highly suspicious. The locality name, common name, and organization name were all unreadable text. Qakbot also opened various web pages such as redir_ie.html inside Internet Explorer 11, redir_chrome.html inside Chrome 79 and favicon.ico inside Chrome 79. The redir_ie.html opened up msn[.]com and redir_chrome.html opened up the wikipedia page for Google Chrome. 

Finally, various email traffic utilizing SMTP, IMAP and POP was observed to varying IP addresses. Also, the varying IPs were requesting login credentials, which the host computer complied with and sent the credentials. 

## Findings and Analysis

To start, we see the initial GET request of **4444444.png**, Figure 1. We can see the **User-Agent** is candyshop, Figure 2. In Figure 3 is the host of **centre-de-conduite-roannais.com** and finally in Figure 4 is an **HTTP Code** of 200, showing a successful connection. 

![get2](https://i.imgur.com/ElARXrD.png)

The SHA256 hash of **4444444.png** was dumped via powershell using the **Get-FileHash** cmdlet, Figure 5.

![444 hash](https://i.imgur.com/EYycM53.png)


The hash was ran through **Virus Total** showcasing a score of **62/72**, Figure 6. We can notice some interesting info here, in Figure 9 is the **Popular threat label** of **trojan.qbot/mkey**. In Figure 10, **Threat categories**, as a **trojan and banker**. Finally, in Figure 11 **Family labels**, of **qbot, mkey, and qakbot**.

![444 VT](https://i.imgur.com/iZs1pFA.png)


After infection, there were several sites that downloaded some file. In figure 12, is a GET request for **redir_chrome.html**. Figure 14, shows us the **User-Agent** string that was used and finally a succesful **HTTP code** of 200, Figure 15. 

![redir chrome](https://i.imgur.com/gYh5oqw.png)


The **User-Agent** string was ran through a parsing agent, returning the **browser** of **Chrome version 79**, Figure 17. The **Operating System** was **Windows 7**, Figure 18. 

![redir chrome UA](https://i.imgur.com/f6q2IoD.png)

The .HTML file was opened via a code editor, and **redir_chrome.html** opens a Wikipedia page for Google Chrome. 

![redir chrome html](https://i.imgur.com/FF6Z4Z8.png)


Another .HTML file was downloaded, **redir_ie.html**, Figure 19. Again, successfully returning a **HTTP code** of 200, Figure 21.

![redir ie](https://i.imgur.com/UNyqk9c.png)


The browser of **Internet Explorer version 11** was used, Figure 23 and **Opearting System** of **Windows 7**, Figure 24.

![redir ie UA](https://i.imgur.com/Ux0WES7.png)

**redir_ie.html** opens the homepage of **MSN**.

![redir ie html](https://i.imgur.com/Rz3iFwN.png)

In Figure 25, various **IP addresses** are being contacted with Figure 26, showing us a list of **Client Hello's** with no associated domain. This indicates **C2 beaconing**. 

![tls traffic](https://i.imgur.com/O3ChxK7.png)

The certificate information was viewed for **IP Addresses** 70[.]120[.]151[.]69 and 174[.]48[.]72[.]160, which is quite unusual. There are unreadable organization names, common names, and locality names, Figures 27 and 28. 

![certificate1](https://i.imgur.com/b2sC815.png)


![certificate2](https://i.imgur.com/tBGGeaN.png)

Next, Qakbot was utilziing all email protocols, IMAP, SMTP, and POP, Figure 29. External IPs would request login credentials, and the client would accept and return the login credentials, Figure 30. 

![email traffic](https://i.imgur.com/ozXdOcR.png)

![login info](https://i.imgur.com/efn2ct5.png)


## Remediations
1. Remove computer from the network to ensure Qakbot doens't spread.
2. Network traffic was discovered to various IP addresses and hosts, these need to be blocked. 
3. Since user credentials were sent, all users will need to rotate their credentials.
4. Re-image the computer
5. Once the computer is reimaged, scan the computer with anti-virus to ensure no Qakbot presense is remaining. Once scanning is complete, apply system updates and patches.
6. Restore files back back-ups before the attack took place.

## MITRE ATT&CK Navigator
![mitre1](https://i.imgur.com/2T63LIW.png)
![mitre2](https://i.imgur.com/fkXA4sy.png)

