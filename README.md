## A Fixed Width Integer module for javascript
Javascript's Number data type is insufficient for arithmetic that requires specific byte-width accuracy.  Bitwise operations are only permitted on values up to 32 bits, and integer precision is lost for values larger than 2^53-1.  This module provides a FixedInt data type for cases where more precise behavior is required for integers of 8, 16, 32, or 64 bits.


## API

| FixedInt method | Return type | Description |
|-----------------|-------------|-------------|
| `FixedInt(size, lo, hi)` | `FixedInt` | Construct a new FixedInt of `size` from the Number `lo`, (and `hi` if size is 8) |
| `FixedInt(size, buf, offset, le)` | `FixedInt` |Construct a new FixedInt by reading `size` bytes from the ArrayBuffer wrapped by `buf`, starting at `offset`|
| `FixedInt(other)` | `FixedInt` | Construct a new FixedInt form another FixedInt or object containing properites size, lo, (and hi)|
| `isNegative()` | `boolean` |Return true if this object is negative when interpreted as signed|
| `isOdd()` | `boolean` | Return true if the number represented by this object is odd|
| `isSafeInteger()` | `boolean` |Return true if this number is less than 2^53-1|
| `isLessThan(that)` | `boolean` | Return true if the number referenced by that is greater than this, when both are interpretted as unsigned|
| `equals(that)` | `boolean` | Return true if this integer has the same value as that|
| `clone()` | `FixedInt` | Return a new FixedInt with the same value and size as this|
| `toSize(size, signExtend)` | `FixedInt` | Return a new FixedInt of size `size` with the same value as this, truncated if `size < this.size`, or zero or sign extended if not, according to `signExtend`|
| `toBuffer(buf, offset)` | `ArrayBuffer`| Write this to a value |
| `valueOf(signed)` | `Number` | Return a Number with the same value as this, or the Number with the value closest to this if it is 2^53 or greater. |
| `toString(radix, signed)` | `String` | Return a string representation of this, in base `radix`.  If `radix = 10`, decode this as a signed integer according to `signed`|


The other class exported by FixedInt is the ALU.  All of the ALU methods are static and accept `FixedInt`s for both arguments.  If the second argument is an integer, it will be coerced to a `FixedInt` of the same size as the first.

| ALU Method| Description|
|-----------|------------|
| `add(a, b)` | Add `b` to `a`|
| `sub(a, b)` | Subtract `b` from `a`|
| `mul(a, b)` | Multiply `a` by `b`.  If `a` has `size`, then the lower `size` bytes of the result are returned, and the upper `size` bytes are stored on ALU.aux|
| `div(a, b)` | Divide `a` by `b`.  The remainder/modulus is stored on ALU.aux|
| `sar(a, b)` | Shift `a` right by `b` bits, preserving the sign of `a`|
| `shr(a, b)` | Shift `a` right by `b` bits, filling with zeros|
| `shl(a, b)` | Shift `a` left by `b` bits, shifting in zeros|
| `and(a, b)` | Return the bitwise and of `a` and `b`, i.e. `a & b`|
| `xor(a, b)` | Return the bitwise exclusive or of `a` and `b`, i.e. `a ^ b`|
| `or(a, b)`  | Return the bitwise or of `a` and `b`, i.e. `a | b`|
| `not(a)`    | Return the 1s complement of `a`, i.e. `~a`|
| `neg(a)`    | Return the 2s complement of `a`, i.e. `-a = ~a + 1`|

The ALU also exposes 5 properties which are set according to the results of particular operations.

| Property| Description |
|------------|-------------|
| `ALU.aux` | Auxiliary value.  Is set to the remainder after a call to `div`, or the upper half of the result after `mul`. |
| `ALU.CF`  | Carry flag, indicates that the preceding operation caused a carry out of (or borrow into) the most significant bit. |
| `ALU.OF`  | Overflow flag, indicates that the carry into and out of the sign bits were not equal.  |
| `ALU.ZF`  | Zero flag, indicates that the result of the last operation was zero |
| `ALU.SF`  | Sign flag, indicates that the result of the last operation was negative, when interpretted as signed. |
