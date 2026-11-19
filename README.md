# Integer Overflow and Memory Vulnerabilities
In my presentation, I chose to explore buffer overflows and memory vulnerabilities, specifically looking at integer overflow and how this affects security. 

## Background + Vocab
- Stack holds local variables and return addresses
- Heap holds memory which is requested with malloc
- Buffer: a fixed-size block of memory (like an array)
- Buffer Overflow: writing past the end of a buffer into neighboring memory
- Integer Overflow: occurs when a calculation goes past the largest value a type can hold and wraps around

## Why is C particularly vulnerable?
- No automatic bounds checking
- Manual memory management
- Fixed-size integers that wrap silently
- C was designed for speed and control, so safety is left to the programmer

## Real World Application: Stagefright (Android, 2015)
- Stagefright was Android’s native media library written in C++. 
- With this, MP4 files were computed as a series of boxes with a certain allocation size.
  - Ex: allocation_size = entry_count * entry_size
- They can be written as shown which means if a hacker puts a large entry_count in the header, the number wraps around to a small number and there is an instance of heap buffer overflow. 
- Overflows give hackers read and write capabilities so they could, for example, write over something important in the heap or read data that is supposed to be private.
- In practice, it is a little more complicated than this as techniques have developed to specifically prevent hacking.
    - As a result, hackers usually chain multiple bugs together to overcome such defenses.

## Prevention in C
### How can these issues be avoided in practice?
1. Write safer arithmetic
- Use checked arithmetic: __builtin_add_overflow / __builtin_mul_overflow
- Reports overflow instead of wrapping
- Use calloc(count, size) instead of malloc(count*size)
- Range check and reject values which may seem unrealistic for certain formats
2. Be careful with types
- Use size_t for sizes and indices
- Be consistent + avoid mixing signed and unsigned
3. Use tooling
- Turn on compiler warnings: -Wall -Wextra
- Run with sanitizers while testing: -fsanitize=undefined, fsanitize=integer,  -fsanitize=address
