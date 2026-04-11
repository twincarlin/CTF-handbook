# Classic Crackme 0x100 — picoCTF 2024

Decompiled here:  
https://dogbolt.org/?id=e2d32545-e2f8-4b8f-94a3-fed62f071a80#Ghidra=218

Reverse‑engineered code here:  
https://blog.cx330.tw/posts/461e0005/

Noticed that the password goes through a scrambler three times, each time only rotating each character based on itself.

Therefore printed input and output to see where characters end up.

Observed that characters are only rotated.

Wrote a de‑scrambler based on input using `d` (ASCII 100, easy to work with):
```python
    #include <stddef.h>
    #include <stdio.h>
    #include <string.h>
    int main(void)
    {
      int uVar1;
      int iVar2;
      char input [51];
      char output [51];
      strncpy(output,"apijaczhzgtfnyjgrdvqrjbmcurcmjczsvbwgdelvxxxjkyigy",0x33);
      setvbuf(stdout,(char *)0x0,2,0);
      printf("Enter the secret password: ");
            scanf("%s",input);
      printf("input:  %s\n",input);
      int sVar3 = strlen(output);
      for (int i=0; i < 3; i = i + 1) {
        for (int i_1 = 0; i_1 < (int)sVar3; i_1 = i_1 + 1) {
          uVar1 = (i_1 % 0xff >> 1 & 0x55U) + (i_1 % 0xff & 0x55U);
          uVar1 = ((int)uVar1 >> 2 & 0x33U) + (uVar1 & 0x33);
          iVar2 = ((int)uVar1 >> 4) + input[i_1] + -0x61 + (uVar1 & 0xf);
          input[i_1] = (char)iVar2 + (char)(iVar2 / 0x1a) * -0x1a + 'a';
        }
      }
      iVar2 = memcmp(input,output,(long)(int)sVar3);
      printf("input:  %s\n",input);
      printf("output: %s\n",output);
      for (int j=0;j<(int)sVar3;j++) {
        printf("%c",(char)(100+((int)output[j]-(int)input[j])));
      }
      if (iVar2 == 0) {
        printf("SUCCESS! Here is your flag: %s\n","picoCTF{sample_flag}");
      }
      else {
        puts("FAILED!");
      }
      return 0;
    }
```
Could have improved the code using symbols and simplified expressions, but continued with the approach already taken.

Compiled and ran the file, first with input:

    dddddddddddddddddddddddddddddddddddddddddddddddddd

Saved the resulting password:

    $ cat > pass
    amfd^]t_wan]hpa[o^phlaYa]liWd^Wkpp\na[\`poola_mZap

Tested the password locally:

    $ ./a.out < pass

Then remotely to get the real flag from the server:

    $ nc titan.picoctf.net 57291 < pass
