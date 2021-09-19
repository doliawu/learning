## Preprocessing
``` console
$gcc -E hello.c -o hello.i
```

## Complication
``` console
$ gcc -S hello.i -o hello.s
$ file hello.s
hello.s: assembler source text, ASCII text
```

## Assembly
``` console
$ gcc -c hello.s -o hello.o` or `as hello.s -o hello.o
$ file hello.o
hello.o: Mach-O 64-bit object arm64
```
> macOS uses Mach-O instead of ELF as its binary format

```
$ gcc -c SimpleSection.c 
$ objdump -h SimpleSection.o  

SimpleSection.o:        file format mach-o arm64

Sections:
Idx Name             Size     VMA              Type
  0 __text           00000088 0000000000000000 TEXT
  1 __data           00000008 0000000000000088 DATA
  2 __cstring        00000004 0000000000000090 DATA
  3 __bss            00000004 00000000000000d8 BSS
  4 __compact_unwind 00000040 0000000000000098 DATA
```

```
$ objdump -s -d SimpleSection.o 

SimpleSection.o:        file format mach-o arm64

Contents of section __text:
 0000 ff8300d1 fd7b01a9 fd430091 a0c31fb8  .....{...C......
 0010 a8c35fb8 e10308aa 00000090 00000091  .._.............
 0020 e9030091 210100f9 00000094 fd7b41a9  ....!........{A.
 0030 ff830091 c0035fd6 ff8300d1 fd7b01a9  ......_......{..
 0040 fd430091 bfc31fb8 28008052 e80b00b9  .C......(..R....
 0050 09000090 280140b9 09000090 2a0140b9  ....(.@.....*.@.
 0060 08010a0b ea0b40b9 08010a0b ea0740b9  ......@.......@.
 0070 00010a0b 00000094 e00b40b9 fd7b41a9  ..........@..{A.
 0080 ff830091 c0035fd6                    ......_.
Contents of section __data:
 0088 54000000 55000000                    T...U...
Contents of section __cstring:
 0090 25640a00                             %d..
Contents of section __bss:
<skipping contents of bss section at [00d8, 00dc)>
Contents of section __compact_unwind:
 0098 00000000 00000000 38000000 00000004  ........8.......
 00a8 00000000 00000000 00000000 00000000  ................
 00b8 38000000 00000000 50000000 00000004  8.......P.......
 00c8 00000000 00000000 00000000 00000000  ................

Disassembly of section __TEXT,__text:

0000000000000000 <ltmp0>:
       0: ff 83 00 d1   sub     sp, sp, #32
       4: fd 7b 01 a9   stp     x29, x30, [sp, #16]
       8: fd 43 00 91   add     x29, sp, #16
       c: a0 c3 1f b8   stur    w0, [x29, #-4]
      10: a8 c3 5f b8   ldur    w8, [x29, #-4]
      14: e1 03 08 aa   mov     x1, x8
      18: 00 00 00 90   adrp    x0, #0
      1c: 00 00 00 91   add     x0, x0, #0
      20: e9 03 00 91   mov     x9, sp
      24: 21 01 00 f9   str     x1, [x9]
      28: 00 00 00 94   bl      0x28 <ltmp0+0x28>
      2c: fd 7b 41 a9   ldp     x29, x30, [sp, #16]
      30: ff 83 00 91   add     sp, sp, #32
      34: c0 03 5f d6   ret

0000000000000038 <_main>:
      38: ff 83 00 d1   sub     sp, sp, #32
      3c: fd 7b 01 a9   stp     x29, x30, [sp, #16]
      40: fd 43 00 91   add     x29, sp, #16
      44: bf c3 1f b8   stur    wzr, [x29, #-4]
      48: 28 00 80 52   mov     w8, #1
      4c: e8 0b 00 b9   str     w8, [sp, #8]
      50: 09 00 00 90   adrp    x9, #0
      54: 28 01 40 b9   ldr     w8, [x9]
      58: 09 00 00 90   adrp    x9, #0
      5c: 2a 01 40 b9   ldr     w10, [x9]
      60: 08 01 0a 0b   add     w8, w8, w10
      64: ea 0b 40 b9   ldr     w10, [sp, #8]
      68: 08 01 0a 0b   add     w8, w8, w10
      6c: ea 07 40 b9   ldr     w10, [sp, #4]
      70: 00 01 0a 0b   add     w0, w8, w10
      74: 00 00 00 94   bl      0x74 <_main+0x3c>
      78: e0 0b 40 b9   ldr     w0, [sp, #8]
      7c: fd 7b 41 a9   ldp     x29, x30, [sp, #16]
      80: ff 83 00 91   add     sp, sp, #32
      84: c0 03 5f d6   ret
```
We can see the disassembled code of the __text section matches the 'Contents of the section __text'. In the __data section, we can see the two values 0x84 and 0x85 (little-endian), these are the initialized global variables `global_init_var` and `static_var`.

## Linking
``` console
$ld -static hello.o balabala.o ...
```

### Address and Storage Allocation
### Symbol Resolution
### Relocation