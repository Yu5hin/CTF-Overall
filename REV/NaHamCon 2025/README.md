## Decription
Did you see the strings? One of those is right, I can just feel it.

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

quite long but we pay attention on 2 main func and first at main_generateFlag like its name, it generateFlag but it's fakeflag and there is a bunch of them

so maybe we can solve that at main_main func 

![image](https://github.com/user-attachments/assets/5d50c02d-dfff-4881-b426-1785dea224ba)

like you see this chall is very long and i won't not read all the psedocode so now i will pay attention on key points.

![image](https://github.com/user-attachments/assets/cd2fdb05-a927-4f09-8029-d7a9172ce941)

in pseudocode i can see flag is get the return value of main_generate function. but when i move the mouse pointer to this function then it warning like you see

![image](https://github.com/user-attachments/assets/9d37d5d5-8e6b-4718-a196-2f6e8f322a67)
![image](https://github.com/user-attachments/assets/c3b5abad-8eac-43a2-95b6-8578064e93d8)

with 2 images above iam sure that rdx register contain flag :(((







