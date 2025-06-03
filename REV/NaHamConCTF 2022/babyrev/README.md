## babyrev

> Aw look! Baby is using a disassembler!  
> **Author:** @birch#9901
> 
> babyrev.zip

Check what content this file has.
```bash
babyrev: ELF 64-bit LSB pie executable, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, BuildID[sha1]=1a48d52c4e5d664115f6cd11651f9c688e8198db, for GNU/Linux 3.2.0, stripped
```

This is a ELF file that means i can execute it.

```bash
$ ./babyrev
Welcome to baby's first rev! :>
Please enter your username: hi
Please enter your password: hi
hi? I don't know you... stranger danger...
```

sure I need to decompile it to see more about how programm work. I'll use IDA 

![image](https://github.com/user-attachments/assets/76fc9d03-b5c8-4cfd-ab19-0351e7bb152c)

it quite clearly and i see at line 12 programm compare s1 vs bossbaby . s1 is a string which we put into. 

```bash
phongkuro@MrPhong:~/ctf/rev$ ./babyrev
Welcome to baby's first rev! :>
Please enter your username: bossbaby
Please enter your password: hi
b�ossbaby? I don't know you... stranger danger...phongkuro@MrPhong:~/ctf/rev$ ^C
phongkuro@MrPhong:~/ctf/rev$ ./babyrev
Welcome to baby's first rev! :>
Please enter your username: bossbaby
Please enter your password: hi
You're almost there!
```

yeah look like i come closer to flag but there have a little remarkable point is when i write username "bossbaby" from keyboard then it down't work but when i copy&paste from IDA, it work.I think they were do something with the username

now what i need to do is find the password 
```
 if ( (unsigned int)sub_12B2(v5) == 38 )
    printf("You're boss baby!");
  return 0LL;
```
i will check offset 0x12B2 

```
 s = a1;
  v12 = __readfsqword(0x28u);
  v1 = strlen(a1);
  v10 = v1 - 1;
  v6[0] = v1;
  v6[1] = 0LL;
  v2 = 16 * ((4 * v1 + 15) / 0x10);
  while ( v6 != (v6 - (v2 & 0xFFFFFFFFFFFFF000LL)) )
    ;
  v3 = alloca(v2 & 0xFFF);
  if ( (v2 & 0xFFF) != 0 )
    *(&v6[-1] + (v2 & 0xFFF)) = *(&v6[-1] + (v2 & 0xFFF));
  v11 = v6;
  v9 = 0;
  sub_1209(s, v6);
  for ( i = 0; ; ++i )
  {
    v4 = i;
    if ( v4 >= strlen(s) )
      break;
    if ( dword_4020[i] == *(v11 + i) )
      ++v9;
  }
  return v9;
}
```

quite clear :))) 

I move the mouse pointer to dword_4020 cuz it get [i] an array and the rest is write script to convert data in dword_4020 

![image](https://github.com/user-attachments/assets/d9d30dfc-af21-489a-bfbd-6529052bd989)

as you see in dword array has 39 elements include "\0" so there is possibility that flag has 39 characters. and this is script decode it
``` c
#include <stdio.h>
#include <stdlib.h>

const int data[] = 
{
    0x66, 0xD9, 0x188, 0x341, 0x7C0, 0x6F9, 0x18A4, 0x95, 0x10A, 0x1D5, 
    0x37C, 0x3A9, 0x7B0, 0x1969, 0x127, 0x1A3, 0x1C4, 0x2B9, 0x754, 0x889, 
    0xF50, 0x1F0, 0x254, 0x2D9, 0x558, 0x571, 0x924, 0x1019, 0x342, 0x3AD, 
    0x508, 0x6E9, 0xA30, 0x10E1, 0x1284, 0x500, 0x5D2, 0x74D 
};

void decode(char* password, const int* buffer, int length)
{
    for (int i = 0; i < length; ++i)
    {
        password[i] = (char)((buffer[i] - i * i) >> (i % 7));
    }
}

int main()
{
    char password[512] = {0};  
    int data_length = sizeof(data) / sizeof(data[0]);  
    
    decode(password, data, data_length);
    printf("%s\n", password);
    
    return 0;
}
```


if you want to check your flag right or not -->


```bash
phongkuro@MrPhong:~/ctf/rev$ ./babyrev
Welcome to baby's first rev! :>
Please enter your username: bossbaby
Please enter your password: flag{7bdeac39cca13a97782c04522aece87a}
You're almost there!
You're boss baby!
```



