HW03
===
This is the hw03 sample. Please follow the steps below.

# Build the Sample Program

1. Fork this repo to your own github account.

2. Clone the repo that you just forked.

3. Under the hw03 dir, use:

	* `make` to build.

	* `make clean` to clean the ouput files.

4. Extract `gnu-mcu-eclipse-qemu.zip` into hw03 dir. Under the path of hw03, start emulation with `make qemu`.

	See [Lecture 02 ─ Emulation with QEMU] for more details.

5. The sample is a minimal program for ARM Cortex-M4 devices, which enters `while(1);` after reset. Use gdb to get more details.

	See [ESEmbedded_HW02_Example] for knowing how to do the observation and how to use markdown for taking notes.

# Build Your Own Program

1. Edit main.c.

2. Make and run like the steps above.

3. Please avoid using hardware dependent C Standard library functions like `printf`, `malloc`, etc.

# HW03 Requirements

1. How do C functions pass and return parameters? Please describe the related standard used by the Application Binary Interface (ABI) for the ARM architecture.

2. Modify main.c to observe what you found.

3. You have to state how you designed the observation (code), and how you performed it.

	Just like how you did in HW02.

3. If there are any official data that define the rules, you can also use them as references.

4. Push your repo to your github. (Use .gitignore to exclude the output files like object files or executable files and the qemu bin folder)

[Lecture 02 ─ Emulation with QEMU]: http://www.nc.es.ncku.edu.tw/course/embedded/02/#Emulation-with-QEMU
[ESEmbedded_HW02_Example]: https://github.com/vwxyzjimmy/ESEmbedded_HW02_Example

--------------------

- [] **If you volunteer to give the presentation next week, check this.**

--------------------

修改main.c，增加三個方程式，單一引數跟多重引數以及回傳多值，透過objdump來驗證。
```c 
int oneint(int a)
{
    return a+a;
}

int manyint(int a, int b, int c, int d, int e, int f)
{
    return a+b+c+d+e+f;
}

void manyreturn(int a, int b, int *add, int *sub, int *mul, int *divi, int *square) 
{
    *add = a+b ;
    *sub = a-b ;
    *mul = a*b ;
    *divi = a/b ;
    *square =a^b;
}
void reset_handler(void)
{
    int add, sub, mul, divi, square ;
    oneint(1);
    manyint(1, 2, 3, 4, 5, 6);
    manyreturn(1, 2, &add, &sub, &mul, &divi, &square);
    while (1)
		;
}
```
we get following:

```
Disassembly of section .mytext:

00000000 <oneint-0x8>:
   0:	20000100 	andcs	r0, r0, r0, lsl #2
   4:	000000a1 	andeq	r0, r0, r1, lsr #1

00000008 <oneint>:
   8:	b480      	push	{r7}
   a:	b083      	sub	sp, #12
   c:	af00      	add	r7, sp, #0
   e:	6078      	str	r0, [r7, #4]
  10:	687a      	ldr	r2, [r7, #4]
  12:	687b      	ldr	r3, [r7, #4]
  14:	4413      	add	r3, r2
  16:	4618      	mov	r0, r3
  18:	370c      	adds	r7, #12
  1a:	46bd      	mov	sp, r7
  1c:	f85d 7b04 	ldr.w	r7, [sp], #4
  20:	4770      	bx	lr
  22:	bf00      	nop

00000024 <manyint>:
  24:	b480      	push	{r7}
  26:	b085      	sub	sp, #20
  28:	af00      	add	r7, sp, #0
  2a:	60f8      	str	r0, [r7, #12]
  2c:	60b9      	str	r1, [r7, #8]
  2e:	607a      	str	r2, [r7, #4]
  30:	603b      	str	r3, [r7, #0]
  32:	68fa      	ldr	r2, [r7, #12]
  34:	68bb      	ldr	r3, [r7, #8]
  36:	441a      	add	r2, r3
  38:	687b      	ldr	r3, [r7, #4]
  3a:	441a      	add	r2, r3
  3c:	683b      	ldr	r3, [r7, #0]
  3e:	441a      	add	r2, r3
  40:	69bb      	ldr	r3, [r7, #24]
  42:	4413      	add	r3, r2
  44:	4618      	mov	r0, r3
  46:	3714      	adds	r7, #20
  48:	46bd      	mov	sp, r7
  4a:	f85d 7b04 	ldr.w	r7, [sp], #4
  4e:	4770      	bx	lr

00000050 <manyreturn>:
  50:	b480      	push	{r7}
  52:	b085      	sub	sp, #20
  54:	af00      	add	r7, sp, #0
  56:	60f8      	str	r0, [r7, #12]
  58:	60b9      	str	r1, [r7, #8]
  5a:	607a      	str	r2, [r7, #4]
  5c:	603b      	str	r3, [r7, #0]
  5e:	68fa      	ldr	r2, [r7, #12]
  60:	68bb      	ldr	r3, [r7, #8]
  62:	441a      	add	r2, r3
  64:	687b      	ldr	r3, [r7, #4]
  66:	601a      	str	r2, [r3, #0]
  68:	68fa      	ldr	r2, [r7, #12]
  6a:	68bb      	ldr	r3, [r7, #8]
  6c:	1ad2      	subs	r2, r2, r3
  6e:	683b      	ldr	r3, [r7, #0]
  70:	601a      	str	r2, [r3, #0]
  72:	68fb      	ldr	r3, [r7, #12]
  74:	68ba      	ldr	r2, [r7, #8]
  76:	fb02 f203 	mul.w	r2, r2, r3
  7a:	69bb      	ldr	r3, [r7, #24]
  7c:	601a      	str	r2, [r3, #0]
  7e:	68fa      	ldr	r2, [r7, #12]
  80:	68bb      	ldr	r3, [r7, #8]
  82:	fb92 f2f3 	sdiv	r2, r2, r3
  86:	69fb      	ldr	r3, [r7, #28]
  88:	601a      	str	r2, [r3, #0]
  8a:	68fa      	ldr	r2, [r7, #12]
  8c:	68bb      	ldr	r3, [r7, #8]
  8e:	405a      	eors	r2, r3
  90:	6a3b      	ldr	r3, [r7, #32]
  92:	601a      	str	r2, [r3, #0]
  94:	3714      	adds	r7, #20
  96:	46bd      	mov	sp, r7
  98:	f85d 7b04 	ldr.w	r7, [sp], #4
  9c:	4770      	bx	lr
  9e:	bf00      	nop

000000a0 <reset_handler>:
  a0:	b590      	push	{r4, r7, lr}
  a2:	b08b      	sub	sp, #44	; 0x2c
  a4:	af04      	add	r7, sp, #16
  a6:	2001      	movs	r0, #1
  a8:	f7ff ffae 	bl	8 <oneint>
  ac:	2305      	movs	r3, #5
  ae:	9300      	str	r3, [sp, #0]
  b0:	2306      	movs	r3, #6
  b2:	9301      	str	r3, [sp, #4]
  b4:	2001      	movs	r0, #1
  b6:	2102      	movs	r1, #2
  b8:	2203      	movs	r2, #3
  ba:	2304      	movs	r3, #4
  bc:	f7ff ffb2 	bl	24 <manyint>
  c0:	f107 0214 	add.w	r2, r7, #20
  c4:	f107 0410 	add.w	r4, r7, #16
  c8:	f107 030c 	add.w	r3, r7, #12
  cc:	9300      	str	r3, [sp, #0]
  ce:	f107 0308 	add.w	r3, r7, #8
  d2:	9301      	str	r3, [sp, #4]
  d4:	1d3b      	adds	r3, r7, #4
  d6:	9302      	str	r3, [sp, #8]
  d8:	2001      	movs	r0, #1
  da:	2102      	movs	r1, #2
  dc:	4623      	mov	r3, r4
  de:	f7ff ffb7 	bl	50 <manyreturn>
  e2:	e7fe      	b.n	e2 <reset_handler+0x42>

```
從objdump我們驗證到當引數超過4個時，多餘的參數會以sp來傳輸，以manyint這個function來觀察。
```c 
   ...
   int manyint(int a, int b, int c, int d, int e, int f)
{
    return a+b+c+d+e+f;
}  
   ...
   void reset_handler(void)
{
    int add, sub, mul, divi, square ;
    oneint(1);
    manyint(1, 2, 3, 4, 5, 6);
    manyreturn(1, 2, &add, &sub, &mul, &divi, &square);
    while (1)
		;
}
```
再來觀察有關call manyint方程式的組語。
```
  ...
  ac:	2305      	movs	r3, #5
  ae:	9300      	str	r3, [sp, #0]
  b0:	2306      	movs	r3, #6
  b2:	9301      	str	r3, [sp, #4]
  b4:	2001      	movs	r0, #1
  b6:	2102      	movs	r1, #2
  b8:	2203      	movs	r2, #3
  ba:	2304      	movs	r3, #4
  bc:	f7ff ffb2 	bl	24 <manyint>
  ...
``` 
可以發現，本次傳的參數有六個，會先將5跟6這兩個參數利用**mov**指令分別存到sp跟sp+4的位置，而剩下的1,2,3,4四個參數利用r0~r3來傳遞，接著利用bl跳到manyint方程式。
