# Automating Metasploit With Resource Scripts

### **Metasploit Resource Scripts**

* Metasploit resource scripts are a great feature of MSF that allow you to automate repetitive tasks and commands.
* They operate similarly to batch scripts, whereby, you can specify a set of Msfconsole commands that you want to execute sequentially.
* You can then load the script with Msfconsole and automate the execution of the commands you specified in the resource script.
* We can use resource scripts to automate various tasks like setting up multi handlers as well as loading and executing payloads.



## **Practical Demonstration**

<figure><img src="../../../.gitbook/assets/image (8) (2) (1).png" alt=""><figcaption></figcaption></figure>

```bash
/usr/share/metasploit-framework/scripts/resource/ #Location of resource scripts that comes pre-packaged with MSF.

#Creating a resource script to automate the use of a multi/handler.
#This is the code of the script: save into a "handler.rc" file.
use multi/handler
set PAYLOAD <payload>
set LHOST <localIP>
set LPORT <localPort>
run

#Creating a resource script to scan ports:
#This is the code of the script:
use auxiliary/scanner/portscan/tcp
set RHOSTS <targetIP>
run

#You just have to type what you would type in MSF and save the file as a .rc
msfconsole -r <script.rc> #Start the script

# best way to use.
msf6 > resource <pathToScript> #Start a resource script directly from MSF


msf6 > makerc <pathToSave> #Save the previous commands as a resource script
```

<figure><img src="../../../.gitbook/assets/image (16) (2).png" alt=""><figcaption><p>mult handler.rc</p></figcaption></figure>



