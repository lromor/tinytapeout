<!---

This file is used to generate your project datasheet. Please fill in the information below and delete any unused
sections.

You can also include images in this folder and reference them in the markdown. Each image must be less than
512 kb in size, and the combined size of all images must be less than 1 MB.
-->

## How it works

This is a 4-bit combination lock built from discrete gates and flip-flops in
[Wokwi](https://wokwi.com/projects/469807785832820737).

A 4-bit register holds the secret code, and its current value is always shown on
`STORED[3:0]` (`uo[3:0]`). The design is fully synchronous to `clk` — any slow
clock (or a debounced push button) works.

- **PROGRAM** (`ui[0]`): while high, the code register loads `CODE[3:0]`
  (`ui[4:1]`) on every rising clock edge. Programming is disabled while the lock
  is armed.
- **ARM** (`ui[5]`): arms the comparator. While armed, **UNLOCKED** (`uo[4]`)
  goes high whenever `CODE[3:0]` matches the stored code.
- **LOCKOUT_N** (`uo[5]`): high after reset. A small counter tracks failed
  attempts (arming the lock with a wrong code counts as one attempt when the
  lock is disarmed again). After three failed attempts, `LOCKOUT_N` goes low and
  `UNLOCKED` can no longer assert — even for the correct code — until the design
  is reset.

`rst_n` clears the attempt counter and re-enables the lock. Note that the code
register itself has no reset, so always program a known code after power-up.

## How to test

1. Apply a clock and reset the design (`rst_n` low for one clock, then high).
   `LOCKOUT_N` (`uo[5]`) should be high.
2. Program a code: set your 4-bit code on `CODE[3:0]` (`ui[4:1]`), raise
   `PROGRAM` (`ui[0]`) for at least one clock edge, then lower it. The stored
   code appears on `STORED[3:0]` (`uo[3:0]`).
3. Guess the code: set a value on `CODE[3:0]` and raise `ARM` (`ui[5]`). If the
   guess matches the stored code, `UNLOCKED` (`uo[4]`) goes high.
4. Trigger the lockout: arm the lock with a wrong code and disarm it again,
   three times in a row. `LOCKOUT_N` goes low and the correct code no longer
   unlocks the lock until you reset the design.

You can also run the design interactively in the
[Wokwi simulator](https://wokwi.com/projects/469807785832820737): the DIP switch
drives the inputs, the LEDs show the outputs, and the slide switch selects
between the clock generator and the push button as the clock source.

## External hardware

None — the on-board DIP switches and LEDs of the Tiny Tapeout demo board are
sufficient.
