# ES-64
An open hardware ISA

Producing an ISA is often hard and has little success per production. The aim is 64 bit native, 16 bit word addressing (no bytes directly) and a emphasis on code density. It's a work in progress and is free under MIT license.

BigRISC is the design ideal, with reducing the instruction set duplication to achieve computational use density. For example the WTO instruction makes a write target override of a 2 operand instruction unlike the RISC of long 32 bit opcodes first and then on to thumb like. Start small at 16 bit opcodes and expand some to 32 bit.

Given progress in caching and OoO it is no longer wrote practice that a load store architecture is superior. As code density can improve with memory to memory architecture and as a result more can be fit in a cache, the CISC is comming back but with inefficient junk such as too many addressing modes and infrequently used silicon area committals.
