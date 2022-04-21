# Fuzzing

## Coverage

Important because it can turn exponential runtimes into linear runtimes

## AddressSanitizer

A tool for helping to amplify a crash into an exploit

Rebuilds the binary with extra markers to tell you where memory was written and what happens to it. (Source code required)

Often binaries will have small read out-of-bounds issues or use-after-frees but the program won't crash right away. Using ASAN makes this more strict so you'll find issues.

## Gynvael’s Magic Numbers

https://h0mbre.github.io/Fuzzing-Like-A-Caveman/

> During the aformentioned GynvaelColdwind [‘Basics of fuzzing’ stream](https://www.youtube.com/watch?v=BrDujogxYSk&t=2545), he enumerates several ‘magic numbers’ which can have devestating effects on programs. Typically, these numbers relate to data type sizes and arithmetic-induced errors. The numbers discussed were:

-   `0xFF`
-   `0x7F`
-   `0x00`
-   `0xFFFF`
-   `0x0000`
-   `0xFFFFFFFF`
-   `0x00000000`
-   `0x80000000` <—- minimum 32-bit int
-   `0x40000000` <—- just half of that amount
-   `0x7FFFFFFF` <—- max 32-bit int

> If there is any kind of arithmetic performed on these types of values in the course of `malloc()` or other types of operations, overflows can be common. For instance if you add `0x1` to `0xFF` on a one-byte register, it would roll over to `0x00` this can be unintended behavior. HEVD actually has an integer overflow bug similar to this concept.