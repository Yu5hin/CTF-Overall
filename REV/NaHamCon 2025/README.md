---
## Decription
> Did you see the strings? One of those is right, I can just feel it
------
```
flagsflagsflags: ELF 64-bit LSB executable, x86-64, version 1 (SYSV), statically linked, BuildID[sha1]=8bbcb5450afeba98d27154e01464d3e4888218b7, stripped
```
we have a ELF file and i tried to execute it
```
phongkuro@MrPhong:~/ctf/rev$ chmod +x flagsflagsflags
phongkuro@MrPhong:~/ctf/rev$ ./flagsflagsflags
Enter the flag:
23
Incorrect flag!
```
not much to do so i convert the file to .exe and decompile it in IDA

![image](https://github.com/user-attachments/assets/0bc5c64b-d673-4203-9725-476bbb0314ed)

quite long but we pay attention on 2 main func and first at *main_generateFlag* like its name, it generateFlag but it's fakeflag and there is a bunch of them

so maybe we can solve that at main_main func 

![image](https://github.com/user-attachments/assets/5d50c02d-dfff-4881-b426-1785dea224ba)

like you see this chall is very long and i won't not read all the psedocode so now i will pay attention on key points.

![image](https://github.com/user-attachments/assets/cd2fdb05-a927-4f09-8029-d7a9172ce941)

in pseudocode i can see flag is get the return value of main_generate function. but when i move the mouse pointer to this function then it warning like you see

![image](https://github.com/user-attachments/assets/9d37d5d5-8e6b-4718-a196-2f6e8f322a67)
![image](https://github.com/user-attachments/assets/c3b5abad-8eac-43a2-95b6-8578064e93d8)

with 2 images above iam sure that **rdx register** contain flag :(((

## Solution

um i need to set breakpoint , so i have to use GDB or x64dbg ... **(with breakpoint you can stop the program at your desired location and check the actual value "correct flag"at runtime)**
```bash
(gdb) b *0x49431C
Breakpoint 1 at 0x49431c
(gdb) run
Starting program: /home/phongkuro/ctf/rev/flagsflagsflags
Downloading separate debug info for system-supplied DSO at 0x7ffff7ffd000
[New LWP 636]
[New LWP 637]
[New LWP 638]
[New LWP 639]
Enter the flag:
flag{01234567890123456789012345678901}
Incorrect flag!
[LWP 639 exited]
[LWP 638 exited]
[LWP 637 exited]
[LWP 636 exited]
[Inferior 1 (process 632) exited normally]
(gdb)
```

iam seted breakpoint at offset 0x49431c which contains correct flag but it didn't break at this offset when i run. That means we will run until it doesn't display incorect flag. Cuz the offset cointain correct flag isn't rigth, this offset is incorrect cuz it stay in the flag processing  branch ==> it will not break. So i will try the previous offset which lead to offset cointain flag is 0x49423F .

the flag must have the correct number of charecters with the flags we see in strings, cuz the program will check length of string and after that is content.   EX: flag{01234567890123456789012345678901}

 

![image](https://github.com/user-attachments/assets/20ec01a6-c85f-4a3c-9a45-b1a0e918136f)

```bash
(gdb) b *0x4942C7
Breakpoint 2 at 0x4942c7
(gdb) run
Starting program: /home/phongkuro/ctf/rev/flagsflagsflags
[New LWP 647]
[New LWP 648]
[New LWP 649]
[New LWP 651]
[New LWP 650]
Enter the flag:
flag{01234567890123456789012345678901}

Thread 1 "flagsflagsflags" hit Breakpoint 2, 0x00000000004942c7 in ?? ()
(gdb) x/s 0x49e7e0
0x49e7e0:       "\020"
(gdb) x/s 0x8897e8
0x8897e8:       "`\207K"
(gdb) x/s $rdx
0x519e85:       "flag{20dec9363618a397e9b2a0bf5a35b2e3}flag{84d71da4745dc317b652a33ceea58b21}flag{0513b7f4a42830a4be9e52a357302dcb}flag{ccc071739856887b278bbc9c6949951a}flag{a358342b5c331455083b43f7a5a2ac8d}flag{d6178"...
````
 ok it shows "hit breakpoint" that means we did it right, after that is check flag at rdx register which hold the correct flag by command **x/s $rdx** which help us can see the contents of the memory that program is about to print
 **flag{20dec9363618a397e9b2a0bf5a35b2e3}flag{84d71da4745dc317b652a33ceea58b21}flag{0513b7f4a42830a4be9e52a357302dcb}flag{ccc071739856887b278bbc9c6949951a}flag{a358342b5c331455083b43f7a5a2ac8d}flag{d6178** 
 and usually the first flag is true and it's the first flag that rdx register pointer to.  

 ### Note:
 Some strings like "correct flag and in..." usually not statically available in the file but is dynamically created in memory (heap or stack) during the program's execution

 And some registers like rsi, rdi,rdx,rax .... often used to assign arguments ,strings before print out  







