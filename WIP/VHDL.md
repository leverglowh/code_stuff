# VHDL

## Entity: interface

An entity describes a component's interface by specifying its **inputs** and **outputs**: a black box.

```vhdl
entity box is
  generic (
    N : integer := 5 -- A generic parameter
  );
  port(
    -- portName(s) : direction type
    in1, in2, in3 : in std_logic; -- input ports
    in4 : in std_logic_vector(N-1 downto 0);
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
    (31 downto 30 => '1', 0 => '1', others => '0'); -- [1100 0000 0000 0000 0000 0000 0000 0001]
begin
  out1 <= in1 and in2 and in3;
  out2 <= in1 when ctlr1 = '0' else -- An if-else, MUST include all cases
    	  in3;
  out4 <= ('0' & costantName(30 downto 0)); -- [0100 0000 0000 0000 0000 0000 0000 0001]
  with ctrl2 select
    out3 <= in1 when "00" | "10",
    	    in3 when others; -- Must always be added
end dataflow;
```

### Structural architecture

If a signal of a component is irrelevant, we can map `open` to it.

```vhdl
architecture structural of entityName is
  signal tmp : std_logic_vector(31 downto 0);

  component compName is
    generic (N : integer := 5);
    port (
      in1, in2 : in std_logic_vector(N-1 downto 0);
      out1 : out std_logic_vector(N-1 downto 0)
    );
  end component;

begin
  compInstanceName : compName -- Create an instance of the previously declared component
    generic map(32) -- Mapping of the generic value
    port map(in1, tmp, out1); -- Mapping of ports, like function calling in programming
    
  compInstanceName2 : compName -- New instance
    generic map(N => 32) -- Explicit names in mapping
    port map(in1 => open, in2 => tmp, out1 => out1); -- componentSig => localSig
end structural;
```

### Behavioral architecture

In `process (in1, in2, in3)`, `in1`, `in2`, `in3` are elements of the sensibility list. Every time any of them changes, the process is awaken and executed. It's basically build by all signals that are at the right of the `<=` simbol.
The variables are virtual elements, that can be used to temporarily store values. A variable can be reused multiple times inside a process.

In behavioral architecture the instructions are **synchronous**, carried out one by one, **without time delay**.
The update to out ports is completed **only when the process terminates its execution**. This obliges us to store intermediate values only in variables, since signals have no memory and cannot be updated.

If a out port is written multiple times, only the last one will be carried out.

```vhdl
architecture behavioral of entityName is
begin
  process (in1, in2, in3)
    variable tmp1, tmp2 : std_logic;
  begin
    tmp1 := in3 and in2;
    out1 <= tmp1 and in1;
    tmp2 := in1 and in2;
    out2 <= tmp2 or in3;
  end process;
end architecture;
```

Intermediate signals can be writter as out ports:

```vhdl
architecture behavioral of entityName is
signal tmp : std_logic;
begin
  process(in1, in2, in3)
  begin
    tmp <= in1 and in2 and in3;
  end process;
	
  out1 <= tmp or in4; -- Mixing dataflow and behavioral architectures.
end architecture;
```

---

## Conditionals

### if - elsif - else

```vhdl
if in1 = '1' then
  out1 <= '0';
elsif in1 = '0' then
  out1 <= in1;
end if
```

### when - else (conditional assignment)

```vhdl
out2 <= in1 when ctrl1 = '00' else -- Cover all possible cases!
    	in2 when ctrl1 = '01' else
        in3 when ctrl1 = '10' else
        in4;
```

### with - select (conditional assignment)

```vhdl
with ctrl1 select
  out1 <= in1 when "00",
  	  in2 when "01",
  	  in3 when "10",
  	  in4 when others; -- others covers all remaining cases.
```

---

## For loops

```vhdl
for i in 1 to N-1 loop
  tmp := tmp and in1(i);
end loop;
```

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

#### `integer`

```vhdl
N : integer := 5;
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

---

## D type flip-flop (asynchronous reset)

```vhdl
entity myFlipFlop is
  port (
    in1 : in std_logic;
    clk, rst : in std_logic;
    out1 : out std_logic
  );
end myFlipFlop;
```

```vhdl
architecture behavioral of myFlipFlop is
begin
  process(clk, rst)
  begin
    if rst = '1' then
      out1 <= '0';
    -- rising_edge(clk) is the same as clk='1' & clk'event (clk is changed)
    elsif rising_edge(clk) then
      out1 <= in1;
    end if;
  end process;
end behavioral;
```

---

## D type flip-flop (synchronous reset)

```vhdl
entity myFlipFlopSync is
  port (
    in1 : in std_logic;
    clk, rst : in std_logic;
    out1 : out std_logic
  );
end myFlipFlopSync;
```

```vhdl
architecture behavioral of myFlipFlopSync is
begin
  process(clk, rst)
  begin
    if rising_edge(clk) then
      if rst = '1' then
        out1 <= '0';
      else
        out1 <= in1;
      end if;
    end if;
  end process;
end behavioral;
```

---

## Register

```vhdl
entity reg is
  port(
    in1 : in std_logic_vector(31 downto 0);
    clk, rst : in std_logic;
    out1 : out std_logic_vector(31 downto 0)
  );
end reg;
```

```vhdl
architecture behavioral of reg is
begin
  process(clk, rst)
  begin
    if rst = '1' then
      out1 <= (others => '0');
    elsif clk = '1' and clk'event then
      out1 <= in1;
    end if;
  end process;
end behavioral;
```

---

## FSM

The 3 main elements of a FSM are: the memory, the "state", the output.

```vhdl
entity fsmName is
  port(
    i: in std_logic;
    clk: in std_logic;
    rst: in std_logic;
    o: out std_logic
  );
end fsmName;
```

```vhdl
architecture FSM of fsmName is
  type state_type is (S0, S1, S2, S3); -- Declare new type that includes all states
  signal next_state, current_state: state_type;
begin
  state_reg: process(clk, rst) -- state register
  begin
	  if rst='1' then
      current_state <= S0;
    elsif rising_edge(clk) then -- update the current state with the next state
      current_state <= next_state;
    end if;
  end process;
  -- ------------------------------------
  delta: process(current_state, i) -- calculate next state
  begin
    case current_state is
      when S0 =>
        if i='0' then
          next_state <= S1;
        else
          next_state <= S0;
        end if;
      when S1 =>
        if i='0' then
          next_state <= S2;
				else
          next_state <= S0;
        end if;
      when S2 =>
      	if i='0' then
          next_state <= S2;
        else
          next_state <= S3; end if;
    	when S3 =>
        if i='0' then
          next_state <= S1;
        else
          next_state <= S0;
        end if;
    end case;
  end process;
  -- ------------------------------------
  lambda: process(current_state) -- output
  begin
    case current_state is
      when S0 =>
        o <= '0';
      when S1 =>
        o <= '0';
      when S2 =>
        o <= '0';
      when S3 =>
        o <= '1';
    end case;
  end process;
end FSM;
```

Lambda and delta can be unified into a single process, with

```vhdl
lambda_delta: process(current_state, i)
begin
  case current_state is
    when S0 =>
      o <= '0';
      if i='0' then
        next_state <= S1;
      else
        next_state <= S0;
      end if;
    when S1 =>
      o <= '0';
      if i='0' then
        next_state <= S2;
      else
        next_state <= S0;
      end if;
    -- ...
  end case;
end process;
```

Note that the state register should be left as a single process and not be mixed with lambda and/or delta.