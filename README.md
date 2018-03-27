## A Fixed Width Integer module for javascript
Javascript's Number data type is insufficient for arithmetic that requires specific byte-width accuracy.  Bitwise operations are only permitted on values up to 32 bits, and integer precision is lost for values larger than 2^53-1.  This module provides a FixedInt data type for cases where more precise behavior is required for integers of 8, 16, 32, or 64 bits.