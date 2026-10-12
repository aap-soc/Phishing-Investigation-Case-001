# Phishing-Investigation-Case-001

# Phishing Investigation – Case 001: Fake Stellar Foundation crypto lure

**Analyst:** Andre Patterson | **Sample:** sample_010.eml (public phishing_pot dataset)
**Verdict:** Malicious | **Severity:** Medium | **Status:** Closed

## Summary
An email pretending to be from the Stellar Foundation offered rewards for"staking" XLM cryptocurrency. The sender's domain has no link to Stellar,
email authentication failed, and the link was hidden behind a Google redirect leading to a lookalike domain. It was most likely sent from a
compromised mailbox at a real organisation. Verdict: malicious.

## Email details
|    Field       |                  Value                            |
|----------------|---------------------------------------------------|
| From           | Stellar Foundation <herb@southernheritagecc[.com> |
| Subject        |        Earn XLM by staking your assets            |
| Date (UTC)     |          Sat 10 Sep 2022, 12:35:20                |
| Sending server | nmtao101.oxsus-vadesecure[.]net - 135.148.117.232 |
| Origin IP      |                45.82.85.109                       |
| X-Mailer       |     PHPMailer 6.1.7 (bulk-mail script)            |
| Message-ID     |   ends @DESKTOP-RQ5VPH2 (a personal Windows PC)   |

## Authentication results
|       Check        |                Result                 |                              Meaning                                     |
|--------------------|---------------------------------------|--------------------------------------------------------------------------|
|       SPF          |             temperror                 |                 Microsoft's DNS lookup timed out                         |
|      DKIM          |         fail, not aligned             |              Signed by the mail host, not the sender's domain            |
|      DMARC         |               none                    |                    The domain has no DMARC policy                        |
| Microsoft verdict  |   compauth=fail, SCL 5, sent to Junk  |               Microsoft's filter agreed it was spam                      |

![Header summary](01-header-summary.png)
![Email route](02-email-route.png)

## Analysis
1. **Brand impersonation:** the display name says Stellar Foundation, but the sender domain is southernheritagecc[.]com.

   
2. **Validated a tool result:** MXToolbox reported SPF failed, but it had tested Microsoft's internal address (::1). The real sending IP (135.148.117.232) is authorised in the domain's SPF record, and the sender logged in (auth=pass). This points to a compromised real mailbox, not a spoofed address.

   
3. **Hidden link:** the link was wrapped in a Google redirect (google.com/url?q=) to hide the real destination: hxxps://blog3.stellarnewsletter[.]org/, a lookalike of Stellar's real
   domain (stellar.org).

   
4. **Reputation checks:** VirusTotal flagged stellarnewsletter.org as [ e.g. 3/94 ] vendors. AbuseIPDB: the sending IP has been reported [ e.g. 12 ] times.

   
5. **Site status:** urlscan.io (11 Oct 2026) could not resolve the domain. The scam site has been taken down.

![AbuseIPDB](05-abuseipdb.png)
![VirusTotal](06-virustotal.png)
![urlscan](07-urlscan.png)

## Indicators of compromise (IOCs)
|     Type         |             Value (defanged)                    |
|------------------|-------------------------------------------------|
| Sender address   |       herb@southernheritagecc[.]com             |
| Sending IP       |         135.148.117.232                     |
| Origin IP        |             45.82.85[.]109                      |
| URL              |    hxxps://blog3.stellarnewsletter[.]org/       |
| Subject          |      Earn XLM by staking your assets            |

## MITRE ATT&CK
T1566.002 - Phishing: Spearphishing Link

## Response (as I would follow a playbook)
1. **Scope:** search all mailboxes for the same sender, subject and URL.
2. **Contain:** remove the email from every mailbox; block the sender
   address and the stellarnewsletter.org domain.
3. **Check impact:** if anyone clicked or entered wallet details, reset
   their credentials, revoke sessions and **escalate** to a senior analyst.
4. **Notify:** tell southernheritagecc.com their mailbox looks compromised.
5. **Close** with notes if nobody interacted.

## Lessons learned
- A real sender can still be malicious if their account has been hacked.
- Don't trust a tool's red X without checking what it actually tested.
- Attackers hide links behind trusted domains like Google- therefore always decode the real destination.
- Block the domain even when the site is offline, as attackers may reuse it.

## Skills demonstrated
Email header analysis (SPF, DKIM, DMARC), IP and domain reputation checks,
safe URL analysis, redirect decoding, IOC extraction, MITRE ATT&CK mapping,
playbook-based response and escalation, clear case documentation.

> Evidence integrity: sample taken from a public phishing dataset; no
> links were clicked; all URLs and IPs are defanged.
