# VHDL

## Entity: interface

An entity describes a component's interface by specifying its **inputs** and **outputs**: a black box.

```vhdl
entity box is
  port(
    -- portName(s) : direction type
  	in1, in2, in3: in std_logic; -- input ports
    out1 : out std_logic; -- output port
    out2 : out std_logic -- output port
  );
end box;
```

---

## Architecture: internal implementation

The architecture describes the internal implementation of an entity.

### Dataflow architecture

The instructions that follow the keyword `begin` are concurrent, their order is uninfluential.
Each time a signal at the right of the symbol `<=` changes (in this case `in1`, `in2`, `in3`), all instructions are revaluated.
The update of the values is considered *instantaneous*, virtually.

```vhdl
architecture dataflow of entityName is
  -- An intermediate signal of type std_logic initialized to 0
  signal tmp: std_logic := '0'; -- := assignment
	constant constantName: std_logic_vector(31 downto 0) :=
    (31 downto 30 => '1', 0 => '1', others => '0');
    -- [1100 0000 0000 0000 0000 0000 0000 0001]
begin
  out1 <= in1 and in2 and in3;
	out2 <= in1 when ctlr1 = '0' else -- A ifelse, MUST include all cases
    			in3;
  out4 <= ('0' & costantName(30 downto 0)); -- [0100 0000 0000 0000 0000 0000 0000 0001]
  with ctrl2 select
    out3 <= in1 when "00" | "10",
    				in3 when others; -- Must always be added
end dataflow;
```

### Structural architecture



### Behavioral architecture



---

## Data types

### Logic types

#### `std_logic`

##### Usage

Add type declaration at the start:

```vhdl
LIBRARY ieee;
USE ieee.std_logic_1164.ALL;
```

##### Possible values

A `std_logic` value is enclosed in **single** quotes.

| Value | Description                             |
| ----- | --------------------------------------- |
| '0'   | zero (SYNTHESIZABLE)                    |
| '1'   | one (SYNTHESIZABLE)                     |
| '-'   | don't care, indifferent (SYNTHESIZABLE) |
| 'Z'   | high impedance (SYNTHESIZABLE)          |
| 'U'   | uninitialized (flip-flop initial value) |
| 'X'   | unknown                                 |

#### `std_logic_vector`

##### Usage

```vhdl
in1: in std_logic_vector(31 downto 0) -- An input port of a 32 bit bus type [31, 30, .., 0]
in2: in std_logic_vector(0 to 31) -- The usage of to instead of downto [0, 1, .., 31]
```

##### Values

A `std_logic_vector` value is enclosed in **double** quotes.

```vhdl
signal tmp: std_logic_vector(1 downto 0) := "01";
```

#### Operations

Logic data types support these relational operators: `=`, `/=`, `<`, `>`, `<=`, `>=`.
In case of vector types, the operation is executed *one bit at a time*, from *left to right*. They MUST be of the same dimension (same number of bits).

---

### Numbers

Must include library

```vhdl
use ieee.numeric_std.all;
```

#### `UNSIGNED`

Natural, binary integer numbers.

#### `SIGNED`

Binary integer numbers in two's complement.

```vhdl
signal sum : SIGNED(31 downto 0); -- A declaration of a signed type vector of 32 bits.
sum <= SIGNED(in1) + SIGNED(in2); -- Type casting
out1 <= std_logic_vector(sum); -- Type casting
out2 <= std_logic(sum(31)); -- Type casting and value indixing
```

#### Operations

Signed and unsigned values support arithmetic operations such as `+`, `-`, `*`, and `/`.
The division `/` operator is the only one **not synthesizable**.