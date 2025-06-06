## Rebug_1
> [test.out.zip](https://github.com/user-attachments/files/20632293/test.out.zip)

---

# Analysis

![image](https://github.com/user-attachments/assets/bcd0aa93-a9fe-412a-9ea4-f7c19ede951b)

put it in DiE and we can see it is ELF64 file type that means we can decompile it with IDA

![image](https://github.com/user-attachments/assets/959d0ae0-3729-4187-b7aa-6f313faa06f3)

As usual, when open it on IDA we stand at main funct and can see `lea     rax, format     ; "Enter the String: "` line

Generate Pseudocode
```
int __fastcall main(int argc, const char **argv, const char **envp)
{
  __int64 v3; // rax
  char v5[44]; // [rsp+0h] [rbp-440h] BYREF
  int v6; // [rsp+2Ch] [rbp-414h] BYREF
  _BYTE v7[16]; // [rsp+30h] [rbp-410h] BYREF
  _BYTE v8[1008]; // [rsp+40h] [rbp-400h] BYREF
  __int64 v9; // [rsp+430h] [rbp-10h]
  int j; // [rsp+438h] [rbp-8h]
  int i; // [rsp+43Ch] [rbp-4h]

  printf("Enter the String: ");
  __isoc99_scanf("%s", v8);
  for ( i = 0; v8[i]; ++i )
    ;
  if ( i == 12 )
  {
    puts("that's correct!");
    v9 = EVP_MD_CTX_new();            // create MD5 context
    v3 = EVP_md5();                   // choose var3 is where hold MD5 hash
    EVP_DigestInit_ex(v9, v3, 0LL);     // initialize MD5 context
    EVP_DigestUpdate(v9, "12", 2LL);     // hash string '12' are two byte "1" and "2"
    v6 = 16;
    EVP_DigestFinal_ex(v9, v7, &v6);
    EVP_MD_CTX_free(v9);
    for ( j = 0; j <= 15; ++j )
      sprintf(&v5[2 * j], "%02x", (unsigned __int8)v7[j]);   
    printf("csawctf{%s}\n", v5);
  }
  else
  {
    printf("that isn't correct, im sorry!");
  }
  return 0;
}
```
quite clear that is 'c' code in assembly

`v8[1008]` get string input from user

`v7[16]`save hash MD5

`v5[44]` save hex string from hash and print out a ascii string 

if 
- string length is 12
- save hash result on v7 and with `v6 = 16`, `EVP_DigestFinal_ex(v9, v7, &v6);` MD5 will have (always) 16 byte
- v5 contain hex string from hash 

That means we just put a string have 12 characters to input then program will make it into hash '12' and with 16 binary byte in MD5 when go through `sprintf("%02x")` we have 32 chars

# Solution 

so we just put on it 12 chars 
`Enter the String: 123456789012
that's correct!
csawctf{c20ad4d76fe97759aa27a0c99bff6710}`
