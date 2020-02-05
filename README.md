# KMS v1.2.329
Latest Korean MapleStory Localhost Project

Taking requests and answering questions, leave an issue on git

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

## OpCode Encryption Removal

```cpp

struct COutPacket
{
	int m_bLoopback;
	unsigned char* m_aSendBuff;
	unsigned int m_uOffset;
	int m_bIsEncryptedByShanda;
};

bool Hook_COutPacket__Init(bool enable)
{
	typedef void(__fastcall* COutPacket__Init_t)(COutPacket* pThis, void* edx, int nType, int bLoopback, int bTypeHeader1Byte);

	static auto COutPacket__Init = reinterpret_cast<COutPacket__Init_t>(0x00774580);

	COutPacket__Init_t Hook = [](COutPacket* pThis, void* edx, int nType, int bLoopback, int bTypeHeader1Byte) -> void
	{
		unsigned int uOffset = pThis->m_uOffset;
		unsigned char uLO = nType & 0xFF;
		unsigned char uHI = (nType >> 8) & 0xFF;
		
		Log("COutPacket__Init: 0x%X", nType);
		
		COutPacket__Init(pThis, edx, nType, bLoopback, bTypeHeader1Byte);
		
		pThis->m_aSendBuff[uOffset + 0] = uLO;
		pThis->m_aSendBuff[uOffset + 1] = uHI;
		
	};

	return SetHook(enable, reinterpret_cast<void**>(&COutPacket__Init), Hook);
}
```
