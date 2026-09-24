# MITRE ATT&CK index

Techniques referenced across this repo, mapped to the pages that cover them.

| Tactic | ID | Technique | Where |
|--------|----|-----------|-------|
| Initial Access | T1078 | Valid Accounts | [Brutus](ctf/htb-sherlocks/brutus.md) |
| Execution | T1218.011 | Rundll32 | [FiveM COM bypass](cases/fivem-com-bypass.md) |
| Persistence | T1136.001 | Create Local Account | [Brutus](ctf/htb-sherlocks/brutus.md) |
| Persistence / Priv Esc | T1546.015 | COM Hijacking | [FiveM COM bypass](cases/fivem-com-bypass.md), [COM/CLSID](techniques/com-clsid-execution.md) |
| Persistence / Defense Evasion | T1574.001 / .002 | DLL Search Order Hijacking / Sideloading | [winmm loader](cases/winmm-dll-loader.md), [drop chain](cases/winmm-dll-drop-chain.md), [DLL sideloading](techniques/dll-sideloading.md) |
| Defense Evasion | T1036 | Masquerading | [winmm loader](cases/winmm-dll-loader.md) |
| Defense Evasion | T1070 | Indicator Removal | [FiveM COM bypass](cases/fivem-com-bypass.md), [drop chain](cases/winmm-dll-drop-chain.md), [anti-forensics](techniques/anti-forensics.md) |
| Defense Evasion | T1222.001 | Windows File and Directory Permissions Modification | [drop chain](cases/winmm-dll-drop-chain.md) |
| Credential Access | T1003.008 | /etc/passwd and /etc/shadow | [Brutus](ctf/htb-sherlocks/brutus.md) |
| Credential Access | T1110 | Brute Force | [Brutus](ctf/htb-sherlocks/brutus.md) |
| Collection | T1056.001 | Keylogging | [SneakyKeys](cases/sneakykeys.md) |
| Command and Control | T1071 / .001 | Application Layer Protocol / Web | [SneakyKeys](cases/sneakykeys.md), [winmm loader](cases/winmm-dll-loader.md) |
| Command and Control | T1105 | Ingress Tool Transfer | [drop chain](cases/winmm-dll-drop-chain.md), [Brutus](ctf/htb-sherlocks/brutus.md) |
| Command and Control | T1573.001 | Symmetric Cryptography | [SneakyKeys](cases/sneakykeys.md) |

Reference: <https://attack.mitre.org/>
