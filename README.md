# KMS v1.2.329
Latest Korean MapleStory Localhost Project

## NGS Removal
```
[enable]

0256AEE0:
xor eax,eax
ret

[disable]

0256AEE0: //TSingleton<CSecurityClient>::CreateInstance();
db 55 8B EC
```
