<!---
This file is used to generate your project datasheet. Please fill in the information below and delete any unused
sections.

You can also include images in this folder and reference them in the markdown. Each image must be less than
512 kb in size, and the combined size of all images must be less than 1 MB.
-->

## How it works

The design is written in [DSLX](https://google.github.io/xls/dslx_reference/) and compiled to
SystemVerilog with [google/xls](https://github.com/google/xls) (`main.x` → `src/main.sv`, see the
top-level Makefile). A thin hand-written wrapper (`src/project.sv`) maps the generated module onto
the Tiny Tapeout interface.

The current design is a placeholder: a 4-bit adder. `uo_out = ui_in[3:0] + ui_in[7:4]`, with a
two-cycle latency (registered inputs and outputs, `--pipeline_stages=1`).

## How to test

Apply operand A on `ui[3:0]` and operand B on `ui[7:4]`; read the sum on `uo[7:0]` two clock
cycles later.

## External hardware

None.
