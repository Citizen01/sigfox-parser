# sigfox-parser
Sigfox data format parser

## Usage
```javascript
require('sigfox-parser')(<messagebytes>, <formatstring>);
```

**messagebytes**: The bytes of the message encoded in a hexadecimal String.

**formatstring**: A formatstring according to the Sigfox documentation.

Example

```javascript
var parsed = require('sigfox-parser')('C01234','b1::bool:7 b2::bool:6 i1:1:uint:16');
```

The variable parsed now contains:

```javascript
{
  b1: true,
  b2: true,
  i1: 0x1234
}
```

## Formatstring
A formatstring consists of multiple fields. A field is defined by its name, its position in the message bytes, its length and its type :
 * the field name is an identifier including letters, digits and the '-' and '_' characters.
 * the byte index is the offset in the message buffer where the field is to be read from, starting at zero. If omitted, the position used is the current byte for boolean fields and the next byte for all other types. For the first field, an omitted position means zero (start of the message buffer)
 * Next comes the type name and parameters, which varies depending on the type :
 * boolean : parameter is the bit position in the target byte
 * char : parameter is the number of bytes to gather in a string
 * float : parameters are the length in bits of the value, which can be either 32 or 64 bits, and optionally the endianness for multi-bytes floats. Default is big endian. Decoding is done according to the IEEE 754 standard.
 * uint (unsigned integer) : parameters are the number of bits to include in the value, and optionally the endianness for multi-bytes integers. Default is big endian.
 * int (signed integer) : parameters are the number of bits to include in the value, and optionally the endianness for multi-bytes integers. Default is big endian.

_(cfr. Sigfox documentation)_
