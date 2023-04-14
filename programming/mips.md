### Decoding
*   32-bit instructions
*   `opcode` = 6 most significant bits = `(i >> 26) & 0x3F`

> Look at opcode: 0 means R-Format, 2 or 3 mean J-Format, otherwise I-Format
> <https://courses.cs.washington.edu/courses/cse378/10sp/lectures/lec06.pdf#page=16>

```text
  |   6    |   5   |   5   |   5   |   5   |   6    | bit width
  | 31  26 | 25 21 | 20 16 | 15 11 | 10  6 | 5    0 | bit index
  |--------|-------|-------|-------|-------|--------|
R | 000000 | rs    | rt    | rd    | shamt | funct  | opcode = 0, "register to register"
I | opcode | rs    | rt    | immediate (16 bits)    | opcode = 1 or 4+, "register immediate"
J | opcode | target address (24 bits)               | opcode = 2 or 3, "jump format"
```

### References
*   <https://inst.eecs.berkeley.edu/~cs61c/resources/MIPS_Green_Sheet.pdf>
*   <https://courses.cs.washington.edu/courses/cse378/10sp/lectures/lec06.pdf>
*   <https://www.mips.com/products/architectures/mips32-2/>
