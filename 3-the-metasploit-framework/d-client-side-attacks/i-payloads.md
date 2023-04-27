# i - Payloads

## Generating Payloads With Msfvenom

**Client-side Attacks**

▪ A client-side attack is an attack vector that involves coercing a client to execute a malicious payload on their system that consequently connects back to the attacker when executed.

▪ Client-side attacks typically utilize various social engineering techniques like generating malicious documents or portable executables (PEs).

▪ Client-side attacks take advantage of human vulnerabilities as opposed to vulnerabilities in services or software running on the target system.

▪ Given that this attack vector involves the transfer and storage of a malicious payload on the client's system (disk), attackers need to be cognisant of AV detection.

**Msfvenom**

▪ Msfvenom is a command line utility that can be used to generate and encore MSF payloads for various operating systems as well as web servers.

▪ Msfvenom is a combination of two utilities, namely; msfpayload and msfencode.

▪ We can use Msfvenom to generate a malicious meterpreter payload that can be transferred to a client target system. Once executed, it will connect back to our payload handler and provide us with remote access to the target system.

**Practical Demonstration**









