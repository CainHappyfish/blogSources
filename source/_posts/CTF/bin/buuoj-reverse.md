---
title: buuoj_reverse
categories:
  - CTF
  - bin
date: 2023-08-07 09:34:07
tags:	
  - CTF	
  - reverse
---

## reverse1

在`strings`窗口发现flag，但直接输入发现flag错误。

![image-20230807093508535](D:\Documents\Desktop\WOW\blog\testBlog\source\_posts\CTF\bin\buuoj-reverse.assets\image-20230807093508535.png)

进行反汇编，发现

![image-20230807093616144](D:\Documents\Desktop\WOW\blog\testBlog\source\_posts\CTF\bin\buuoj-reverse.assets\image-20230807093616144.png)

该`for`循环将所有的o替换成0，所以最后的flag是`flag{hell0_w0rld}`。

## reverse2

打开`strings`窗口，发现flag的一部分

![image-20230807094756816](D:\Documents\Desktop\WOW\blog\testBlog\source\_posts\CTF\bin\buuoj-reverse.assets\image-20230807094756816.png)

类似`reverse1`，该题将 flag 中的 i 和 r 都替换成 1。

![image-20230807100616887](D:\Documents\Desktop\WOW\blog\testBlog\source\_posts\CTF\bin\buuoj-reverse.assets\image-20230807100616887.png)

### 内涵的软件

打开`strings`窗口即可发现flag。

![image-20230807101131735](D:\Documents\Desktop\WOW\blog\testBlog\source\_posts\CTF\bin\buuoj-reverse.assets\image-20230807101131735.png)

## 新年快乐

![image-20230807102545693](D:\Documents\Desktop\WOW\blog\testBlog\source\_posts\CTF\bin\buuoj-reverse.assets\image-20230807102545693.png)

exe 被 upx 压缩，使用命令 `upx.exe -d xnkl.exe -o xnkl_unpack.exe` 解压。

![image-20230807102355739](D:\Documents\Desktop\WOW\blog\testBlog\source\_posts\CTF\bin\buuoj-reverse.assets\image-20230807102355739.png)

## XOR

反汇编，发现flag应该和`global`有关

![image-20230807135830142](D:\Documents\Desktop\WOW\blog\testBlog\source\_posts\CTF\bin\buuoj-reverse.assets\image-20230807135830142.png)

找到对应的数据

![image-20230807135907586](D:\Documents\Desktop\WOW\blog\testBlog\source\_posts\CTF\bin\buuoj-reverse.assets\image-20230807135907586.png)

查看反汇编的代码，发现对flag进行了异或操作，编写脚本：

```
"""
__global
__cstring:0000000100000F6E aFKWOXZUPFVMDGH db 'f',0Ah              ; DATA XREF: __data:_global↓o
__cstring:0000000100000F6E                 db 'k',0Ch,'w&O.@',11h,'x',0Dh,'Z;U',11h,'p',19h,'F',1Fh,'v"M#D',0Eh,'g'
__cstring:0000000100000F6E                 db 6,'h',0Fh,'G2O',0
"""

str_xor = ['f', 0x0A, 'k', 0x0C, 'w', '&', 'O', '.', '@', 0x11, 'x', 0x0D, 'Z', ';', 'U', 0x11, 'p', 0x19, 'F', 0x1F,
           'v', '"', 'M', '#', 'D', 0x0E, 'g', 6, 'h', 0x0F, 'G', '2', 'O', 0]

print(len(str_xor))

for i in range(0, len(str_xor)):
    if isinstance(str_xor[i], int):
        str_xor[i] = chr(str_xor[i])

for i in range(0, len(str_xor) - 1):
    print(str_xor[i], end=" ")

process = ''.join(str_xor)

print(process)

flag = ''
for i in range(0, len(str_xor)-1):
    flag += chr(ord(str_xor[i]) ^ ord(str_xor[i - 1]))

print(flag)
```

得到

![image-20230807140005840](D:\Documents\Desktop\WOW\blog\testBlog\source\_posts\CTF\bin\buuoj-reverse.assets\image-20230807140005840.png)

## reverse3

首先先找到flag

![image-20230807161331053](D:\Documents\Desktop\WOW\blog\testBlog\source\_posts\CTF\bin\buuoj-reverse.assets\image-20230807161331053.png)

进行反编译：

![image-20230807161635865](D:\Documents\Desktop\WOW\blog\testBlog\source\_posts\CTF\bin\buuoj-reverse.assets\image-20230807161635865.png)

发现重要函数`sub_4110BE`，而该函数返回的是`sub_411AB0`：

```
void *__cdecl sub_411AB0(char *a1, unsigned int a2, int *a3)
{
  int v4; // [esp+D4h] [ebp-38h]
  int v5; // [esp+D4h] [ebp-38h]
  int v6; // [esp+D4h] [ebp-38h]
  int v7; // [esp+D4h] [ebp-38h]
  int i; // [esp+E0h] [ebp-2Ch]
  unsigned int v9; // [esp+ECh] [ebp-20h]
  int v10; // [esp+ECh] [ebp-20h]
  int v11; // [esp+ECh] [ebp-20h]
  void *v12; // [esp+F8h] [ebp-14h]
  char *v13; // [esp+104h] [ebp-8h]

  if ( !a1 || !a2 )
    return 0;
  v9 = a2 / 3;
  if ( (int)(a2 / 3) % 3 )
    ++v9;
  v10 = 4 * v9;
  *a3 = v10;
  v12 = malloc(v10 + 1);
  if ( !v12 )
    return 0;
  j_memset(v12, 0, v10 + 1);
  v13 = a1;
  v11 = a2;
  v4 = 0;
  while ( v11 > 0 )
  {
    byte_41A144[2] = 0;
    byte_41A144[1] = 0;
    byte_41A144[0] = 0;
    for ( i = 0; i < 3 && v11 >= 1; ++i )
    {
      byte_41A144[i] = *v13;
      --v11;
      ++v13;
    }
    if ( !i )
      break;
    switch ( i )
    {
      case 1:
        *((_BYTE *)v12 + v4) = aAbcdefghijklmn[(int)(unsigned __int8)byte_41A144[0] >> 2];
        v5 = v4 + 1;
        *((_BYTE *)v12 + v5) = aAbcdefghijklmn[((byte_41A144[1] & 0xF0) >> 4) | (16 * (byte_41A144[0] & 3))];
        *((_BYTE *)v12 + ++v5) = aAbcdefghijklmn[64];
        *((_BYTE *)v12 + ++v5) = aAbcdefghijklmn[64];
        v4 = v5 + 1;
        break;
      case 2:
        *((_BYTE *)v12 + v4) = aAbcdefghijklmn[(int)(unsigned __int8)byte_41A144[0] >> 2];
        v6 = v4 + 1;
        *((_BYTE *)v12 + v6) = aAbcdefghijklmn[((byte_41A144[1] & 0xF0) >> 4) | (16 * (byte_41A144[0] & 3))];
        *((_BYTE *)v12 + ++v6) = aAbcdefghijklmn[((byte_41A144[2] & 0xC0) >> 6) | (4 * (byte_41A144[1] & 0xF))];
        *((_BYTE *)v12 + ++v6) = aAbcdefghijklmn[64];
        v4 = v6 + 1;
        break;
      case 3:
        *((_BYTE *)v12 + v4) = aAbcdefghijklmn[(int)(unsigned __int8)byte_41A144[0] >> 2];
        v7 = v4 + 1;
        *((_BYTE *)v12 + v7) = aAbcdefghijklmn[((byte_41A144[1] & 0xF0) >> 4) | (16 * (byte_41A144[0] & 3))];
        *((_BYTE *)v12 + ++v7) = aAbcdefghijklmn[((byte_41A144[2] & 0xC0) >> 6) | (4 * (byte_41A144[1] & 0xF))];
        *((_BYTE *)v12 + ++v7) = aAbcdefghijklmn[byte_41A144[2] & 0x3F];
        v4 = v7 + 1;
        break;
    }
  }
  *((_BYTE *)v12 + v4) = 0;
  return v12;
}
```

查看`aAbcdefghijklmn`：

![image-20230807163407122](D:\Documents\Desktop\WOW\blog\testBlog\source\_posts\CTF\bin\buuoj-reverse.assets\image-20230807163407122.png)

发现是常用的base64编码表，编写脚本：

```
import base64

_str = 'e3nifIH9b_C@n@dH'

rawDest = ""
for i in range(len(_str)):
    rawDest += chr(ord(_str[i]) - i)
print(rawDest)

flag = base64.b64decode(rawDest)
print(flag)
```

得到flag：

![image-20230807164001958](D:\Documents\Desktop\WOW\blog\testBlog\source\_posts\CTF\bin\buuoj-reverse.assets\image-20230807164001958.png)

## 不一样的flag

