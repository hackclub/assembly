# Assembly Language
### Talking to the CPU

<p align="center">
  <img width="460" height="300" src="https://cdn.vox-cdn.com/uploads/chorus_asset/file/8688491/hiE5vMs.gif">
</p>

<p align="center">
  <em>This guide is currently under construction. Read at your own risk of me explaining math wrong!</em>
</p>

Resources:

X86
- https://rderik.com/blog/let-s-write-some-assembly-code-in-macos-for-intel-x86-64/
- https://github.com/0xAX/asm
- https://web.stanford.edu/class/archive/cs/cs107/cs107.1222/guide/x86-64.html
- https://en.wikibooks.org/wiki/X86_Assembly/X86_Architecture
- The "Getting Started Writing Assembly Language" series
  - https://blog.devgenius.io/getting-started-writing-assembly-language-8ecc116f3627
  - https://blog.devgenius.io/finding-an-efficient-development-cycle-for-writing-assembly-language-be2092e6db6a
  - https://blog.devgenius.io/writing-an-x86-64-assembly-language-program-648b6005e8e (ESPECIALLY this one, since it had a lot of the command line arg info, although some of it works differently on mac)
- https://filippo.io/making-system-calls-from-assembly-in-mac-os-x/
- http://dustin.schultz.io/mac-os-x-64-bit-assembly-system-calls.html
- https://0xax.github.io/asm_1/
- https://github.com/0xAX/asm
- https://cs61.seas.harvard.edu/site/2018/Asm1/
- https://www.dreamincode.net/forums/topic/285550-nasm-linux-getting-command-line-parameters/
- https://stackoverflow.com/questions/53555298/how-to-get-arguments-from-the-command-lineassembly-nasm-ubuntu-32bit
- https://cs.lmu.edu/~ray/notes/nasmtutorial/
- https://www.figma.com/file/Uhd6Qe3lzmJ60bBQmkx7Ch/cpu
- https://gist.github.com/FiloSottile/7125822
- https://death-of-rats.github.io/posts/yasm-hello-world/
- https://dev.to/tomassirio/hello-world-in-asm-x8664-jg7
- https://jameshfisher.com/2018/03/10/linux-assembly-hello-world/
- https://stackoverflow.com/questions/52830484/nasm-cant-link-object-file-with-ld-on-macos-mojave

Debugger
- https://github.com/hugsy/gef
- https://www.sourceware.org/gdb/download/

6502
- https://skilldrick.github.io/easy6502/
- https://www.nesdev.com/6502guid.txt
- https://people.cs.umass.edu/~verts/cmpsci201/cmpsci201.html
- https://www.masswerk.at/6502/6502_instruction_set.html
- https://www.masswerk.at/6502/
- http://6502.org/tutorials/

Z80
- https://tutorials.eeems.ca/Z80ASM/index.htm
- http://www.z80.info/z80emu.htm

RISC-V
- https://medium.com/swlh/risc-v-assembly-for-beginners-387c6cd02c49
- https://www.cs.cornell.edu/courses/cs3410/2019sp/riscv/interpreter/
- https://github.com/jameslzhu/riscv-card/blob/master/riscv-card.pdf
- https://github.com/riscv/riscv-isa-manual/releases/download/Ratified-IMAFDQC/riscv-spec-20191213.pdf

Misc
- https://godbolt.org/
- https://www.asciitable.com/
- https://stackoverflow.com/questions/4552905/what-is-the-difference-between-a-32-bit-and-64-bit-processor

Contributors:
- [@jessicard](https://github.com/jessicard)
- [@TheOneKevin](https://github.com/theonekevin)
- [@exu3](https://github.com/exu3)
- [@jakeboxer](https://github.com/jakeboxer)
- [@bellesea](https://github.com/bellesea)


This may sound counterintuitive, but computers are simple. I know you may be shaking your head, insisting that isn't true, but I promise you that computers truly do boil down to only a few basic concepts. At their core, everything they’re doing can be represented by two values: 0 and 1. However, even though computers are fundamentally doing simple tasks, they can be confusing and tedious to learn about. This is because computers have been built up layer by layer over time. These layers have produced the amazing, efficient, incredible machines that sit on our lap (or desktop!) today. But, these layers also make learning about computers feel like an impenetrable black hole.

I hope this guide helps you to demystify some of the lowest layers, and hopefully turn it from something that feels like magic to something that feels graspable. I personally didn’t know how these things worked before I started writing this guide, so I hope this helps you learn the things I’ve pieced together on my journey to understanding my computer better.

In this guide, we are going to cover what the CPU is, how we can communicate with the CPU, and why any of this matters. I will say, communicating with your CPU directly is usually unnecessary, as we now have higher level languages that are fast enough for most of our needs. That being said, the game [RollerCoaster Tycoon is written 99% in assembly language](https://en.wikipedia.org/wiki/RollerCoaster_Tycoon_(video_game)#:~:text=Sawyer%20wrote%2099%25%20of%20the,%2C%20rendering%2C%20and%20paint%20programs.). Not only that, but if you're writing operating systems or other low level systems, you're sometimes [writing assembly directly](https://www.youtube.com/watch?v=rX0ItVEVjHc) because you need things to be lightning fast. Even though you or I may never need to write assembly, I think that building an understanding of how your computer works at a fundamental level can be eye opening and make you appreciate all of the other tasks you perform on your computer.

## The CPU

<p align="center">
  <img width="460" height="300" src="https://www.pcworld.com/wp-content/uploads/2021/10/intel-cpu-rocket-lake-rear.jpg?quality=50&strip=all&w=1024">
</p>

Have you heard of the companies Intel or AMD? These are two popular companies that manufacture the CPUs that go into our computers. All of the computers we use contain something called a central processing unit, known as the CPU or the processor, which effectively acts as the brain of the computer. Computers contain other processing units (like the graphics card!) that are responsible for processing more specific things, but the CPU is your general powerhouse for all computing tasks. That being said, the CPU can do shockingly little: it can read values, set values, and perform simple math calculations like addition and subtraction. You hand it numbers, and it’s put to work crunching the data however you’d like.

One way we can communicate directly with the CPU is by writing instructions for it in a format called assembly language. Assembly language is the lowest level of abstraction in computers where the code you write is still human readable. An abstraction means that it’s a layer above some other layer that makes that thing easier to do.

<p align="center">
  <img width="460" height="300" src="https://www.familyhandyman.com/wp-content/uploads/2019/05/08.jpg">
</p>

For example, let's take a steering wheel. A steering wheel makes driving simple - you just turn left and right, and the amount you turn maps to how much your tires turn. But, what’s happening underneath? The steering wheel is an abstraction layer on top of rods, levers, and whatever else is happening inside that car, simplifying the act of turning for you.

In our case, assembly code is the human-readable layer above something called machine code. CPUs can only understand numbers, so machine code is just a list of numbers that the CPU reads to figure out what instructions to execute and what data they should operate on. Assembly, on the other hand, is a text based language, consisting of acronyms that represent instructions to the computer. Since they are text, they are not directly readable by the CPU. So that text file gets translated, through something called the assembler, into the numbers that the computer can then read.

It’s like if you have a cake recipe written in imperial measurements, and you want to convert it to metric for your Canadian friend. Line by line you’d translate the recipe until you have a new recipe for your friend to use. You’d take the first measurement, 2 cups of flour (assembly language), convert it to grams (the assembler), and then write the converted recipe to use 68 grams of flour (machine code). Look at you go - you’re the assembler here!

You could skip all of this assembly shenanigans by writing the machine code directly, but machine code looks something like:

```
01000111 00000000 11110010 10101110 11110010 00000001 11000011 11100010 00001011
```

Machine code can also look like this, if you viewed it with a hex editor (we will talk about what hex is a little bit later):

```
F8 12 01 9A DE B6 77 1C E3 28 6A BB 07 07 00 F2 E4 10 DD D0 EF 36 2A 3A 5F AB C4 44
```

So you’d have to painstakingly convert numbers mapped to instructions (the number of instructions per processor varies, but it’s somewhere between 50-1000). Assembly, on the other hand, looks something like:

```asm
; Written in ARM Cortex-A Assembly
LD x8, 24 ; Load the number stored in memory address 24 into register 8
LD x9, 48 ; Load the number stored in memory address 48 into register 9
ADD x3, x8, x9 ; Add the contents of registers x8 and x9 and save that value into register 3
```

_A note: everything after the ; are comments for other humans, not code to execute, so the computer ignores it._

I know this doesn’t look extremely friendly, especially compared to the high level programming languages we have today. However, it's far friendlier than just writing a big list of numbers, and that's the real purpose of assembly language: to allow human beings to basically write machine code without just writing a big list of numbers.

All programming languages are some level of abstraction above machine code, but in the end, all human written code has to be converted into numbers for your CPU to be able to read.

You may be wondering what machine code actually looks like. If you are, you’re in luck! Let’s map our last ADD line to machine code.

```asm
ADD x3, x8, x9
```

In machine code, this may end up looking like binary, or base 2 (we will talk about what binary is a little bit later):

```
00000001 00000011 00001000 00001001
```

Which, in base 10 (how we normally talk about numbers!), is:

```
1 3 8 9
```

The decoder would see the first 1, and it would know that the first number it receives should map to an instruction, and instruction 1 is ADD. It knows that the ADD instruction's first argument is the save destination, and it sees that the next byte has the value 3, so it knows that its save destination is register 3. Then it knows that the next 2 arguments are the registers that it needs to go retrieve data from, 8 and 9. It retrieves the data from there, adds them together, and saves that new value to register 3.

### Electricity and the physical world
The CPU is able to interpret machine code, which is just numbers, as instructions. We can represent these instructions as 1s and 0s, also known as binary. In the physical world, we represent binary with electrical circuits. A single circuit may contain an electrical signal, or it may not. 1 represents the presence of electricity, and 0 the absence of electricity. Multiple circuits can be arranged in a group to represent binary numbers. For example, 8 circuits could be grouped together to represent a byte.

<p align="center">
  <img width="460" height="300" src="https://industrytoday.com/wp-content/uploads/2021/02/safe-business-conveyor-belt-operations.jpg">
</p>

Imagine a warehouse where we are packing boxes. In this metaphor, the warehouse is your CPU, and a box is a grouping of 8 electrical signals (a byte). A single box will travel through the warehouse on conveyor belts in order to make it from one working station to another. The conveyor belts, in the CPU world, are known as buses. Buses are effectively just wires that allow electricity to travel from one place to another, and there are different types of wires depending on what kind of data you want to send around.

As a box travels around the warehouse on conveyor belts, it will be stopped at different working stations. Some stations may check inside the box and send it elsewhere based on what it finds. Other stations may add or remove stuff to or from the box. This is just like in a CPU: a byte travels around the CPU on buses, and when it reaches different parts of the CPU, it may have its value checked or modified.

### Clock ticks
WRITING NOTE: Flesh out how this connects to the warehouse metaphor.

Every processor has its own clock. It's not a clock that would be useful for you or me, but instead is a material that oscillates at a certain frequency, giving you a certain number of vibrations per second. These vibrations help the processor keep track of time. This clock is going _fast_. You're seeing something like one vibration every microsecond, which is about 1000000 vibrations per second. We call each one of these vibrations a "clock tick". These are important for us because for every clock tick, the CPU reads one instruction.

### Input and Output
_Note: mention how data gets into the computer and how it comes back out_
_Note: Memory-mapped I/O (MMIO) vs port-mapped I/O (PMIO)_

### Decoder
WRITING NOTE: Flesh out how this connects to the warehouse metaphor.

A decoder in a CPU is a specialized device that takes in an input, in the form of a byte, and decodes what it’s trying to do. These tasks are represented as our assembly instructions. So the decoder sees a specific number, and it’s like oh! I know what the number 2 maps to! It means I want to subtract numbers. So now the decoder can send the data along to the right places to do the things it needs to do.

How does the decoder know how to decode? It’s built physically into the chip itself, where the circuitry determines the instruction set.

### Registers
WRITING NOTE: interleave this stuff in with the assembly stuff

You may have heard the term “memory” thrown around when talking about computers. Usually when people use that term, they’re referring to random access memory, or RAM, which is a type of short term storage your computer has.

<p align="center">
  <img width="460" height="300" src="https://i.pinimg.com/originals/31/03/a1/3103a18819a867fbe8b4808b4c197692.jpg">
</p>

Accessing your RAM is kind of like going to the post office. Each piece of data (mail, in our metaphor) has an "address" (mailbox number) where you can view the contents (mail). You can also clear out the contents (take the mail out of the box), and then store new pieces of data (get new pieces of mail).

Our pieces of mail are actually just electrical currents. Because we store data as electricity, when your computer turns off and no more electricity is traveling to it, all of the things you have stored get cleared out! It’s kind of like if every night when your post office closed, all of the mail was thrown out. That’s why we refer to it as short term memory - we want to make sure to store important things in the hard drive, which is our longer term storage, lest it be thrown away!

Our RAM, or post office, has quite a bit of room to store our things - enough to hold entire packages! But, visiting the post office and carrying mail around can be slow and cumbersome. So, for faster (but smaller) storage, we have a set of tiny mailboxes outside the post office that can just hold letters. Those are our registers.

<p align="center">
  <img height="300" src="https://m.media-amazon.com/images/I/51BkYo7G7jL._AC_SX522_.jpg">
</p>

Registers are where the CPU can store small pieces of data so that it can keep interacting with them. For example, let’s say we need to add two numbers together. First, the CPU retrieves the first number it needs for the equation. Since the CPU can really only do one thing at a time, it needs to put this number down in order to grab the next number. So it stores this first number into a register for the time being. Next, the CPU grabs the second number in the equation. The CPU now has all the information it needs to add the two numbers together. It goes ahead and executes the adding instruction, passing that new number along, and then moves on to the next instruction it’s given.

A nice thing about registers is that processors have a few of them, each one being available for different purposes. Some of them are directly accessible by you for setting values and reading values through assembly instructions, but some are used internally and can’t be directly accessed.

Now you may be asking yourself - why don’t we store everything in the registers, since memory is slower? Well, we only have a limited amount of space in our registers. The actual size depends on your computers hardware, but RAM can easily hold over 15 million times the amount that registers can! Since computers have to process so much data, we can very quickly run out of space in our registers. So any data that we need to hold onto for a bit while we calculate other things, we throw into memory.

#### Program counter
WRITING NOTE: Wait to talk about this part until you get to jump/call instructions in the assembly

The CPU has many specialized registers, which we don't access directly. One of them is the program counter, which I wanted to mention specifically because I personally wondered how the computer keeps track of the code it's executing. This stores the memory address of the current line of the current program it's executing, and updates itself automatically. For example, let’s say we are running an assembly program. There's an instruction for adding two numbers together. Once that instruction finishes running, the program counter increments to the memory address of the next instruction of the program.

## The Math Section
WRITING NOTE: This part can all probably be moved after a lot of the assembly, maybe if there's a spot where we talk about bitmasking and stuff. I say this because I know math can be intimidating to people who are otherwise interested in programming, so the math stuff would probably be more helpful as an appendix.

If you thought you'd get through this without doing any math, well, I'm sorry. We have to do a little bit so that we can understand what the computer is doing, because like I said, it's all just basic math underneath. Now, I promise you it won't be too hard. You may get a little confused and your brain may hurt, but just stick with me here and we'll make it through to the assembly section.

### Binary

<p align="center">
  <img width="460" height="300" src="https://images.easytechjunkie.com/green-lit-numbers.jpg">
</p>

When people hear that I program for a living, they think that I stare at 0s and 1s all day. Luckily I do not, because that would give me a migraine. However, binary is important to talk about, because everything the computer is doing can be represented by these digits. These digits are referred to as binary, which is a number system that has 2 as its base.

When we think of numbers in the human world, we think of them in base 10. Base 10 means that each digit of a number can be represented with the digits 0-9. Each digit over we move (for example 1 vs 10 vs 100) is 10 times the value to the right of it.

_Note: Finish filling this in_

### Boolean logic
Boolean is a very cute word for a very simple concept! A boolean is something that can only have one of two values - true or false. True or false can also be represented as 1 for true, 0 for false.

Since we represent data in the physical world with the inclusion or absence of electrical signals, we can use something called boolean logic to determine whether a “statement” is true or false. A statement here is just boolean values, and we pass them to operations we can use depending on our use case.

Why would we ever use this? Great question! Let me let you in on a secret - everything your computer is doing is actually just composed of these logical operations. Everything. All the math your processor can do, it’s done by combining a few of these operations together. So you send it some electrical signals, it goes through a few of these “logic gates”, in the form of transistors, and BAM! You have an answer at the end. You combine a bunch of these small answers through more transistors, and then you have a larger answer!

Let’s talk through these logical operations a bit.

#### AND
![Screen Shot 2022-05-17 at 10 53 15 AM](https://user-images.githubusercontent.com/621904/168841849-865fc1ae-091c-4723-b0b0-cdba50ef22a6.png)

And is always false unless both inputs are true.

| In | Out |
| -- | --- |
| 00 |  0  |
| 10 |  0  |
| 01 |  0  |
| 11 |  1  |

#### OR
![Screen Shot 2022-05-17 at 10 53 20 AM](https://user-images.githubusercontent.com/621904/168842251-b04cdc4d-6c3e-458b-8968-c1eef1fc2b0c.png)

OR is always false unless one of the inputs is true.

| In | Out |
| -- | --- |
| 00 |  0  |
| 10 |  1  |
| 01 |  1  |
| 11 |  1  |

#### NOT
![Screen Shot 2022-05-17 at 10 53 29 AM](https://user-images.githubusercontent.com/621904/168842309-66cc8fe3-d4e5-41e0-9ea1-8bc4e2a9bd21.png)

NOT only requires 1 input, and it flips the input.

| In | Out |
| -- | --- |
| 0  |  1  |
| 1  |  0  |

#### NAND
![Screen Shot 2022-05-17 at 10 53 35 AM](https://user-images.githubusercontent.com/621904/168842382-d854d2f1-c20d-46fc-a302-4972343305ca.png)

NAND is always true unless both inputs are true.

| In | Out |
| -- | --- |
| 00 |  1  |
| 10 |  1  |
| 01 |  1  |
| 11 |  0  |

#### NOR
![Screen Shot 2022-05-17 at 10 53 25 AM](https://user-images.githubusercontent.com/621904/168842428-dd63a889-4774-4f7b-845e-3ae43c7c9b75.png)

NOR is always false unless both inputs are false.

| In | Out |
| -- | --- |
| 00 |  1  |
| 10 |  0  |
| 01 |  0  |
| 11 |  0  |

#### XOR
![Screen Shot 2022-05-17 at 10 53 38 AM](https://user-images.githubusercontent.com/621904/168842465-f8251a20-8962-4221-9146-93e4e6b01908.png)

XOR is always false unless the inputs are different.

| In | Out |
| -- | --- |
| 00 |  0  |
| 10 |  1  |
| 01 |  1  |
| 11 |  0  |

#### XNOR
![xnor](https://user-images.githubusercontent.com/621904/168848632-9e62e088-3dbf-4007-a435-fe34e6fb4c8b.png)

XNOR is always false unless the inputs are the same.

| In | Out |
| -- | --- |
| 00 |  1  |
| 10 |  0  |
| 01 |  0  |
| 11 |  1  |

Fun fact: You only need the NAND gate (AND gate followed by NOT) to do every single possible logic operation ever. That means that every possible logic circuit can be made to use only NAND! In fact, a physical NAND transistor takes up less area than an AND transistor. To make an AND, you’d actually make a NAND and then invert the output.

In real circuits, you would even see amalgamations of gates (like AND+OR+NOT+OR+AND) as a single "standard cell". It’s like stacking lego bricks, but each brick is a logical operation.

### Hexadecimal
_Note: fill this out_

All numbers in assembly language are represented by hexadecimal
Our usual numbers are base 10

When you see 125 as a number, you can think of that as:
(10 * 10^2) + (2 * 10^1) + (5 * 10^0)
100 + 20 + 5 = 125
Hex is base 16, which means is 0-9, A-F for 10-15
When you see 7D, you can think of that as:
D = 13
(7 * 16^1) + 13
112 + 13 = 125

## Assembly language
There are many different assembly languages, depending on the processor you want to talk to. X86 is the most useful but the hardest to write. It's used for Intel processors, which have to process a lot of data. 6502 is another popular assembly language, simpler than X86, and these processors are still used in many small devices today. Z80 is another one you might know - remember those TI-8X calculators you may have used in school? Well, to program those, you'd use the Z80 assembly language!

Each 1 or 0 in binary is called a bit (or binary digit). We can’t store much information in a single bit, so bytes were invented to be the standard data size that a computer works with. A byte is composed of 8 bits, because a single letter like “a” takes 1 byte, or 8 bits to represent. It’s not because “a” is complicated to represent, but instead it’s that we’re mapping some set of characters to numbers (in this case, we’re referring to A-Z, 0-9, and so on), and we want the number of possibilities that 8 bits affords us.

So if we only have 1 bit, we can represent 2 numbers (0 and 1). If we have 2 bits, 4 numbers (0-3). 3 bits, 8 numbers (0-7). n bits, 2^n numbers. That means that a byte can hold 2^8 numbers, since a byte is 8 bits. That leaves us with 256 different values we can use!

Let’s say you need to store a number that is higher than 255, since 256 values mean we can store numbers 0 - 255 (since we want to include 0). We then need multiple bytes to represent that number.

Have you heard of 32 bit or 64 bit processors? That’s referring to the same thing! So, instead of their registers holding a single byte, a 32 bit processor holds 4 bytes (32 / 8, since a byte is 8 bits), and a 64 bit processor holds 8 bytes.

In our examples, each line we write in assembly maps to a single instruction to the CPU. For example, an instruction could be to add two numbers together. A caveat here is that in more complex processors, instructions can be mapped to multiple instructions to the CPU, but we are going to keep it simple in our examples.

A computer can only process one byte at a time.

_Note: potentially fill this out_
Note: Can multi-core processors process more than one byte at a time?
I’ll add some notes here for a brief overview:
We start by asking: Can 1 core execute more than 1 instruction at a time? Yes! How?
Memory access takes time (sometimes up to 200 “processor ticks”!!), during this time the CPU core is “stalled” and is doing nothing at all. Can we find something useful for the CPU core to do in the meantime? Yes!
Look ahead for instructions to execute (out-of-order)
Execute more than 1 instruction at a time (superscalar) by combining instructions
Can more than 1 core access memory?
No! But… if memory access is not immediately granted, that core can find something useful to do in the meantime (using aforementioned techniques)
But now instructions are out-of-order! The programmer expects their program to execute in-order. Will there be consequences? Yes! This is a veryyyy complex problem to solve.
What if instead of accessing 1 byte at a time, we access 16 bytes? If 1 memory chip can pump out 4 bytes per tick, what if you have 4 memory chips? They can pump out 4x4 bytes per “processor tick”!
Look at the M1 Pro. How many memory chips can you count? 2
Each chip has 2 interfaces
Each interface can pump out 2 bytes/tick (aka Double Data Rate, DDR)
There are 3.2 GT/s (3.2 billion “ticks” per second)
Total, 3.2 * 16 * 2 * 2 around 200 GBps (gigaBITS/second), as advertised! Very cool.
