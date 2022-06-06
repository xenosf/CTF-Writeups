# Default Password

## Challenge Description

Members of TheKeepers got their hands on one of the GDC computers. They created a memory dump of the system as they believe it might contain some juicy info. Help them find proof.

Note: You need to insert the correct answer to the flag structure. For example: CDDC21{code}

## Attached files

* data

---

## Solution

Using the `lsadump` plugin in Volatility, we can get the LSA (Local Security Authority) secrets, which includes the default password.

```
$ vol.py -f data  windows.lsadump.Lsadump                    
Volatility 3 Framework 1.0.1
Progress:  100.00		PDB scanning finished                     
Key	Secret	Hex

DefaultPassword	.Th1s_i$_A_L0ng_p@s$w0rd	2e 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 54 00 68 00 31 00 73 00 5f 00 69 00 24 00 5f 00 41 00 5f 00 4c 00 30 00 6e 00 67 00 5f 00 70 00 40 00 73 00 24 00 77 00 30 00 72 00 64 00 00 00
DPAPI_SYSTEM	,+©iJgvsN¸%ð^ºö®þ
qûÒ°	2c 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 01 00 00 00 2b 1e a9 10 69 4a 89 05 67 80 76 73 4e 02 1d b8 25 f0 99 5e 1b 86 ba f6 01 87 ae 06 fe 1d 0a 7a 96 71 84 fb 0f 03 d2 b0 00 00 00 00
```

### Flag

```text
CDDC21{.Th1s_i$_A_L0ng_p@s$w0rd} 
```
