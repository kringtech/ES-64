# ES-64
An open hardware ISA

Producing an ISA is often hard and has little success per production. The aim is 64 bit native, 16 bit word addressing (no bytes directly) and an emphasis on code density. It's a work in progress and is free under MIT license.

BigRISC is the design ideal, with reducing the instruction set duplication to achieve computational use density. For example the WTO instruction makes a write target override of a 2 operand instruction unlike the RISC of long 32 bit opcodes first and then on to thumb like. Start small at 16 bit opcodes and expand some to 32 bit.

Given progress in caching and OoO it is no longer wrote practice that a load store architecture is superior. As code density can improve with memory to memory architecture and as a result more can be fit in a cache, the CISC is comming back but without inefficient junk such as too many addressing modes and infrequently used silicon area committals.

## Vectors
For example a floating point register set is not included as a separate set, and the main registers are used. This prevents them being left empty as wide SIMD registers become normal for floating point. 64 opcodes are reserved as prefixes for vector processing.

## Registers
There are basically 20 registers, of which 8 are full function addressing registers (An), 8 are non addressing (Dn) but have a modulo carry at the instruction operation width to use the high part independently. The remaining 4 are the program counter (PC) and 3 other registers with automated indexing (IP, SP and RP). This is a good compromise on efficient opcode size. If all 16 main registers were fully addressing the last 4 could not exist and many opcodes would be removed. This would leave 15 plus the program counter, and code density would suffer badly. An extra 8 registers could be provided if addressing was only done through the 3 special registers (and fetches through the PC), but code density would again suffer.

## Yes, it's 68k inspired
A number of overly complex variant instructions were dropped, a new opcode format was decided and all operations can be performed on the An registers. This makes the Dn registers the lesser, but not having carrying into upper parts makes up for not needing the register to linearly address. It is reasonably easy to use them as 16 register halves at 32 bits each. As the only thing Dn registers can't do is address, this is not too big a restriction. If you overflow the An registers at a narrow width it will affect the high order and besides that they too can be used in halves or quarters.

## Addressing
There are no complex modes on An registers to mess with compiler writer's heads. There are pre and post inc or dec to some extent on the 4 other registers, but in strange and situation useful ways. In fact it is impossible to get normal addressing with these. Floats and doubles will not load into these special 4 as the instruction space is better used. The PC as a source or target is specially handled for situations where it would make no sense to do an opcode. These opcodes become other operations making a 16 bit opcode quite possibility dense. This density wouldn't be possible with the 16 addressing register alternative but would with the 28 data register alternative. Faster multiple indexing looses 8 registers.

## Caching
As yet nothing but a CFR (cache flush then read address) instruction has been decided. Being able to store floats and doubles in the An registers provides utility, and cache delays might make the 28 register variant better. Flushing the 3 non PC special registers to address might be hidden to some extent by cache delay, but code density would suffer pushing occasional data stalls into occasional fetch stalls. Neat subroutine handling and compact direct threading of code address can substantially decrease code size, so changing the preferred automated index modes was not really an option. Keeping the cache bus open for data reads is where the gain is imagined in the linked list pointer chasing.

## Data formats
SIMD is just more a thing than MISD oppertunity. Using compact data forms was the reason word, long and quad is available on almost all instructions. Byte might have been good but the bit could not be wasted. Along with Unicode and half size manipulations this is something that is still reasonable to do. A fast indexed form of UTF-8 is more of a software issue. Code pages will still be a compressed solution for some data processing tasks, even if the underlying disk is compressed.


