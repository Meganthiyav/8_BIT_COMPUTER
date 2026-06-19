# 📘 Instruction Register (IR) – Verilog Design

## 📌 Description

Instruction Register (IR) stores an 8-bit instruction fetched from memory and splits it into opcode and operand fields. It is controlled by a load signal (`ctrl_ir`) and used in basic CPU datapath design.

---

## ⚙️ Specification

* Data width: 8-bit instruction
* Clock: Positive edge triggered
* Reset: Synchronous reset (active high)
* Control signal: `ctrl_ir` (load enable)
* Output split:

  * Opcode = upper 4 bits
  * Operand = lower 4 bits

---

## 💻 RTL Code

```verilog
module ir(
    input clk,
    input rst,
    input ctrl_ir,
    input [7:0] d_in,
    output [3:0] opc,
    output [3:0] opr
);

reg [7:0] ir;

always @(posedge clk)
begin
    if (rst)
        ir <= 8'b0;
    else if (ctrl_ir)
        ir <= d_in;
end

assign opc = ir[7:4];
assign opr = ir[3:0];

endmodule
```

---

## 🧪 Testbench

```verilog
module ir_tb;

reg clk, rst, ctrl_ir;
reg [7:0] d_in;
wire [3:0] opc, opr;

ir DUT (
    .clk(clk),
    .rst(rst),
    .ctrl_ir(ctrl_ir),
    .d_in(d_in),
    .opc(opc),
    .opr(opr)
);

always #5 clk = ~clk;

initial begin
    clk = 0;
    rst = 1;
    ctrl_ir = 0;
    d_in = 8'h00;

    #10 rst = 0;

    #10 ctrl_ir = 1; d_in = 8'hAB;
    #10 ctrl_ir = 0;

    #20 ctrl_ir = 1; d_in = 8'h3C;

    #20 $finish;
end

endmodule
```

---

## 🧪 Simulation

---

## 🧱 Block Diagram


