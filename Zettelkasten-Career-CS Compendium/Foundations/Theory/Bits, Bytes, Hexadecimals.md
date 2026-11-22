---
tags:
  - #TRUNK
  - ComputerScience
  - DataRepresentation
---

# Bits, Bytes, and Hexadecimals

Every piece of digital information — text, images, code, music — ultimately reduces to two states: voltage on a wire, or no voltage. This note covers the three layers programmers use to read and reason about that raw substrate: the **bit**, the **byte**, and the **hexadecimal** notation that makes both human-readable.

---

## 1. The Bit — The Atom of Computing

The naive view of computing is that data is "characters" and "numbers". At the silicon level, a CPU has no concept of either. It only knows whether a wire is energised. Everything else is built on top of that single distinction.

A **bit** (short for **bi**nary digi**t**) is the smallest unit of information. It holds exactly one of two values: `0` or `1`.

### Physical Reality

Bits aren't abstract — each bit corresponds to a literal voltage on a wire:

| **Logical Value** | **Voltage (typical)** |
|---|---|
| **0** | ~0V (low) |
| **1** | ~3.3V or 5V (high) |

Modern CPUs run at much lower voltages (often around 1V) to save power, but the principle is identical: low = 0, high = 1.

### The Light Switch Analogy

The cleanest mental model: a bit is a single light switch. Off is `0`, on is `1`. A computer's memory is billions of these switches arranged in long rows, with electronics that can flip and read each one in nanoseconds.

### Scale Context

A single bit on its own is almost useless. Real data lives at much larger scales — kilobytes, megabytes, gigabytes — but the unit names are quietly inconsistent.

> ⚠️ **The MB vs MiB trap.** "1 MB" can mean either **8,000,000 bits** (decimal, 10⁶ bytes) or **8,388,608 bits** (binary, 2²⁰ bytes — strictly called a *mebibyte*, MiB). Hard drive manufacturers use decimal. Operating systems often use binary but mislabel it as "MB". This mismatch is why a 1 TB drive shows up as ~931 GB in Windows. When precision matters, use **MiB / GiB / TiB** for the binary versions.

---

## 2. The Byte — Grouping Bits Into Meaning

A single bit can only say yes or no. To represent richer data — letters, colours, prices — we group bits together. A **byte** is a fixed group of **8 bits**, and it is the smallest unit most CPUs and programming languages address directly. When you read or write a single character to a file, you are almost always moving one byte.

### Why 8?

There is no deep mathematical reason. The 8-bit grouping was popularised by IBM's System/360 in the 1960s and stuck because it (a) cleanly holds the entire ASCII character set with one bit to spare, and (b) divides evenly into the larger machine word sizes (16, 32, 64). Convention won.

### Capacity

With 8 independent bits, the total number of combinations is:

$$2^8 = 256$$

A byte can therefore represent any unsigned integer from **0 to 255**. (Signed bytes use one bit to indicate sign, giving the range −128 to 127 — a topic for a separate note on two's complement.)

### Worked Example: ASCII 'A'

The **ASCII** standard maps each character to a number 0–127, which fits inside a byte:

```
Character: 'A'
Byte:      0 1 0 0 0 0 0 1
Place:    128 64 32 16 8 4 2 1
```

Read each `1` and add its place value:

- Bit at place 64 is on → **+64**
- Bit at place 1 is on → **+1**
- All others off → +0
- **Total: 65**

So `'A'` in memory is the byte `01000001`, which is the decimal number `65`, which the ASCII standard tells us to render as the glyph `A`.

> 💡 **Why this matters:** when you read text from any file in any language, you are really reading a sequence of bytes. The interpretation as "letters" happens at a higher layer (ASCII, UTF-8, etc.). The bytes themselves are just numbers.

---

## 3. Hexadecimal — Base-16 Shorthand

Once you start working with multi-byte values — memory addresses, colour codes, binary protocols — reading raw binary becomes brutal:

```
01101111 00110010 11010101 10101010
```

Quick: is the 19th bit set? Painful. Hex solves this.

### What Hex Is

**Hexadecimal** (often shortened to **hex**) is a base-16 number system. It uses 16 distinct symbols — the familiar 0–9 plus the letters A–F to cover the values 10 through 15:

| **Decimal** | **Hex** | **Binary (4 bits)** |
|---|---|---|
| 0 | 0 | 0000 |
| 1 | 1 | 0001 |
| 5 | 5 | 0101 |
| 9 | 9 | 1001 |
| 10 | A | 1010 |
| 11 | B | 1011 |
| 14 | E | 1110 |
| 15 | F | 1111 |

### The Crucial Detail: Why Base-16, Not Base-12 or Base-20?

Hex isn't an arbitrary choice. It exists because **2⁴ = 16**. That single equation is what makes hex the perfect binary shorthand:

- Every group of **4 bits** maps to **exactly 1 hex digit**.
- Every **byte (8 bits)** maps to **exactly 2 hex digits**.
- Conversion between binary and hex requires no arithmetic — just a lookup.

A 4-bit group has its own name: a **nibble** (yes, really — half a byte).

### Binary → Hex Without Doing Maths

Take the ASCII 'A' byte from the previous section:

```
Binary:   0100  0001
            ↓     ↓
Nibbles:    4     1
            ↓     ↓
Hex:        4     1

Result:   0x41
```

Split the byte down the middle, look up each nibble in the table above, write the two hex digits side by side. That's the entire algorithm. This is why every memory dump, debugger output, MAC address, and colour code in the wild is written in hex.

> **Therefore:** binary tells you the truth about what's in memory; hex makes that truth fit on one line.

---

## 4. Numeric Literal Prefixes in Code

Once a programmer writes a number directly in source code (a **literal**), the compiler needs to know which base to read it in. The number `100` could mean one hundred (decimal), four (binary), sixty-four (octal), or two-hundred-and-fifty-six (hex) depending on intent. Languages solve this with **base prefixes**.

### The Standard Prefixes

| **Base** | **Prefix** | **Example** | **Decimal Value** |
|---|---|---|---|
| **Decimal (10)** | none | `65` | 65 |
| **Hexadecimal (16)** | `0x` or `0X` | `0x41` | 65 |
| **Binary (2)** | `0b` or `0B` | `0b01000001` | 65 |
| **Octal (8)** | `0o` or `0O` | `0o101` | 65 |

This convention is shared across C, C++, Java, JavaScript, Python, Rust, Go, Swift, and most other modern languages.

### Why `0x` and Not `1x`?

The prefix is two characters, and each character has a job:

1. **The leading `0`** — tells the parser "what follows is a numeric literal, not an identifier." Identifiers (variable names, function names) cannot start with a digit, so a leading `0` removes any ambiguity immediately.
2. **The letter `x`** — short for he**x**adecimal. It tells the parser which base to read.

If you wrote `1x41`, the parser would lex `1` as a complete decimal integer, then see `x41` and try to interpret it as a separate identifier or operator — producing a syntax error. There is no programming language where `1x` means hex.

### The Octal Danger Zone

> ⚠️ **A leading `0` alone means octal in C, C++, and Java.** This is a notorious bug source.
>
> ```c
> int x = 0123;   // This is OCTAL — value is 83, not 123
> int y = 123;    // This is decimal — value is 123
> ```
>
> A common case: padding numbers for visual alignment (`0123`, `0089`) silently changes their meaning, and `0089` won't even compile because `8` and `9` are not valid octal digits. Modern languages like Python 3 fixed this by *requiring* `0o` for octal — a bare `0089` is just a syntax error.

---

## 5. Practical Conversion: Decimal ↔ Hexadecimal

Most of the time you will let the language convert for you (`printf("%X", n)` in C, `hex(n)` in Python). But knowing the manual algorithm matters because it is what's happening under the hood, and because it gives you an instinct for what hex values "feel" the right size.

### The Algorithm: Decimal → Hex

Repeatedly divide by 16. The remainders, read in reverse, are the hex digits.

```
1. Divide N by 16. Record the integer quotient (Q) and integer remainder (R).
2. Convert R to its hex symbol (0–9 stay the same; 10→A, ..., 15→F).
3. If Q is 0, stop. Otherwise, set N = Q and repeat.
4. Read the recorded remainders bottom-to-top.
```

### Worked Example 1: 42 → `0x2A`

```
42 ÷ 16  =  2  remainder 10   →  A     (least-significant digit)
 2 ÷ 16  =  0  remainder  2   →  2     (most-significant digit)
                                  ↑
                              Read upwards: 2, A
```

Result: **`0x2A`**.

### Worked Example 2: 171 → `0xAB`

```
171 ÷ 16  =  10  remainder 11  →  B
 10 ÷ 16  =   0  remainder 10  →  A

Read upwards: A, B  →  0xAB
```

> ⚠️ **The Common Mistake — Decimal Remainder vs Integer Remainder.** A natural temptation is to do `171 / 16 = 10.6875` and then treat `0.6875` or `6` as the remainder. **This is wrong.** The conversion algorithm requires the *integer* remainder.
>
> The correct extraction:
> ```
>   integer quotient    = 10        ( ⌊171 / 16⌋ )
>   integer remainder   = 171 − (10 × 16) = 171 − 160 = 11
> ```
>
> If you see a calculator showing `0.6875`, multiply that fractional part back by 16 to recover the true remainder: `0.6875 × 16 = 11`. Then convert `11 → B`.

### Verification by Place Value

Hex digits, like decimal digits, have place values — but the places are powers of 16 instead of powers of 10:

$$\text{0xAB} = (\text{A} \times 16^1) + (\text{B} \times 16^0) = (10 \times 16) + (11 \times 1) = 160 + 11 = 171$$

This is also how you read hex in your head: take each digit, multiply by its power of 16, add. For a two-digit hex byte, that's just `(first digit × 16) + second digit`.

### In Code

The conversion is built into every language's standard library:

```c
// C — using printf format specifiers
unsigned char my_byte = 171;
printf("%X\n", my_byte);     // Output: AB
printf("0x%02X\n", my_byte); // Output: 0xAB  (zero-padded to 2 digits)
```

```python
# Python — built-in hex() returns a string with the 0x prefix
n = 171
print(hex(n))       # Output: 0xab
print(f"{n:#04X}")  # Output: 0xAB  (zero-padded, uppercase)
```

> 💡 **Practical tip:** the `%02X` / `:02X` format specifier (zero-padded to 2 digits) is what you want when dumping bytes — it keeps every byte exactly two characters wide so columns line up:
> ```
> 0x00 0x41 0x42 0x0A    // good — uniform width
> 0x0  0x41 0x42 0xA     // bad  — hard to scan
> ```

---

## Summary

| **Layer** | **Definition** | **Range / Capacity** | **Notation** |
|---|---|---|---|
| **Bit** | Single binary digit (one wire's voltage state) | 0 or 1 | `0`, `1` |
| **Byte** | Group of 8 bits | 0–255 (unsigned) | `01000001` |
| **Nibble** | Group of 4 bits (half a byte) | 0–15 | `0100` |
| **Hex digit** | One of 16 symbols representing a nibble | 0–F | `4`, `A`, `F` |
| **Hex literal** | Full hex number in source code | depends on width | `0x41` |

The chain works in both directions: a byte splits into two nibbles, each nibble is one hex digit, two hex digits write a byte. Once internalised, you can read a hex dump fluently and translate to/from binary without thinking.
