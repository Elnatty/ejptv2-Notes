# Alternate Data Streams

### **Alternate Data Streams (ADS)**

* Alternate Data Streams (ADS) is an NTFS (New Technology File System) file attribute and was designed to provide compatibility with the MacOS HFS (Hierarchical File System).
* Any file created on an NTFS formatted drive will have two different forks/streams:
* Data stream - Default stream that contains the data of the file.
* Resource stream - Typically contains the metadata of the file.
* Attackers can use ADS to hide malicious code or executables in legitimate files in order to evade detection.
* This can be done by storing the malicious code or executables in the file attribute resource stream (metadata) of a legitimate file.
* This technique is usually used to evade basic signature based AVs and static scanning stools.



### **Practical Demonstration**

In Windows cmd prompt shell.

```powershell
# we create a test.txt file in the cmd prompt shell
# requires admin priv though.
notepad test.txt

# when we right click on the file, then click the details tab, we see the details/resource stream of the file.
# we can basically hide a malicious payload within the resourse stream.
# so that whenever we execute the file we can specify we want to execute the malicious payload.
notepad test.txt:secret.txt  # Creates a hidden .txt file within the resource stream of test.txt (secret.txt) is the hidden file.
# notice when we launch the "test.txt" file, we dont see the content of the "secret.txt" file.
# and it also doesnt show in the "details" of the file.
# but i can view it by specifying the file in the resourse stream.
notepad test.txt:secret.txt  # outputs the content of the secret file.

# we can redirect the content of our payload file into the resource file of a legitimate file.
type winpeas.exe > file.txt:winpeas.exe

# an attacker will enter some data into the "file.txt" file, then it appears to have some sort of content in it.
# Now we have to create a symboloc link to enable us start the payload file automatically.
# mklink <randomLegitmateFIleName> <path_to_hiddenPayload_file>:<actual_payload_fileName>
mklink wupdate.exe C:\Temp\file.txt:winpeas.exe

# whenever we type "wupdate" it will run the "wineas.exe" file automatically.
wupdate
```



