# pubg-dma-offset
###  我因为工作问题和时间问题，随时停止更新这个offset.txt，如果你想获取新的offset，请移步Uc

### Due to work and time issues, I will stop updating this offset.exe at any time. If you want to obtain a new offset, please move to Uc

```
2025.10.23
DWORD Decrypt::CIndex(DWORD encrypted_id)
{
    // Define the hardcoded constants directly from the IDA code
    const DWORD keyA = 0x5;
    const DWORD keyB = 0x50200000;
    const DWORD shiftL = 27;
    const DWORD shiftR = 5;
    const DWORD mask = 0x7FF0000;

    const DWORD key1_rot = 0x502E3065;
    const DWORD rval = 21;
    const DWORD key2_final = 0x1AD9281;

    // 1. Calculate the complex part
    // Corresponds to: (((a1 ^ 5) << 27) | ((a1 ^ 0x50200000u) >> 5) & 0x7FF0000)
    DWORD partA = (encrypted_id ^ keyA) << shiftL;
    DWORD partB = ((encrypted_id ^ keyB) >> shiftR) & mask;
    DWORD complex_part = partA | partB;

    // 2. Calculate the rotated part
    // Corresponds to: __ROR4__(a1 ^ 0x502E3065, 21)
    DWORD rotated_part = _rotr(encrypted_id ^ key1_rot, rval);

    // 3. Combine the parts with the final XOR key
    // Corresponds to: complex_part ^ rotated_part ^ 0x1AD9281
    return complex_part ^ rotated_part ^ key2_final;
}
2025.11.1
DWORD Decrypt::CIndex(DWORD value)
{
    return (((value ^ 0xC7C846D) << 26) | ((value ^ 0xC7C846Du) >> 6) & 0x3FF0000) ^ _rotr(value ^ 0xC7C846D, 22) ^ 0xCE1353CB;
}
2025.11.5
DWORD Decrypt::CIndex(DWORD value)
{
    return (16 * value) & 0xFFFF0000 ^ _rotr(value ^ 0x426749E3, 12) ^ 0xDF75B4A6;
}
```

