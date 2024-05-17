# VLSI-LAB-EXP-4
# DATE:
# SIMULATION AND IMPLEMENTATION OF SEQUENTIAL LOGIC CIRCUITS

# AIM: 
  To simulate and synthesis SR, JK, T, D - FLIPFLOP, COUNTER DESIGN using VIVADO 2023.1

# APPARATUS REQUIRED:
  VIVADO 2023.1

# PROCEDURE:
1.Open Vivado: Launch Xilinx Vivado software on your computer.

2.Create a New Project: Click on "Create Project" from the welcome page or navigate through "File" > "Project" > "New".

3.Project Settings: Follow the prompts to set up your project. Specify the project name, location, and select RTL project type.

4.Add Design Files: Add your Verilog design files to the project. You can do this by right-clicking on "Design Sources" in the Sources window, then selecting "Add Sources". Choose your Verilog files from the file 
  browser.

5.Specify Simulation Settings: Go to "Simulation" > "Simulation Settings". Choose your simulation language (Verilog in this case) and simulation tool (Vivado Simulator).

6.Run Simulation: Go to "Flow" > "Run Simulation" > "Run Behavioral Simulation". This will launch the Vivado Simulator and compile your design for simulation.

7.Set Simulation Time: In the Vivado Simulator window, set the simulation time if it's not set automatically. This determines how long the simulation will run.

8.Run Simulation: Start the simulation by clicking on the "Run" button in the simulation window.

9.View Results: After the simulation completes, you can view waveforms, debug signals, and analyze the behavior of your design.

# LOGIC DIAGRAM:

# SR FLIPFLOP

![image](https://github.com/navaneethans/VLSI-LAB-EXP-4/assets/6987778/77fb7f38-5649-4778-a987-8468df9ea3c3)
```
CODE:

module sr_flipflop(s,r,clk,rst,q);
input s,r,clk,rst;
output reg q;
always@(posedge clk)
begin
if(rst==1)
q=0;
else
begin
case({s,r})
2'b00:q=q;
2'b01:q=0;
2'b10:q=1;
2'b11:q=1'bX;
endcase
end
end
endmodule
```

# OUTPUT:
<img width="962" alt="320568441-95924674-55f0-408b-b91d-5c0abe5a7f7d" src="https://github.com/navaneethans/VLSI-LAB-EXP-4/assets/166889783/2964096a-26cf-476f-b042-dec85ccf7bef">

# JK FLIPFLOP

![image](https://github.com/navaneethans/VLSI-LAB-EXP-4/assets/6987778/1510e030-4ddc-42b1-88ce-d00f6f0dc7e6)
```
CODE:

module JK_flipflop (q, q_bar, j,k, clk,reset);
input j,k,clk, reset;
output reg q;
output q_bar;
always@(posedge clk)
begin
  if(!reset)        
    q <= 0;
  else 
  begin
      case({j,k})
        2'b00: q <= q;    // No change
        2'b01: q <= 1'b0; // reset
        2'b10: q <= 1'b1; // set
        2'b11: q <= ~q; // Toggle
      endcase
  end
end
assign q_bar = ~q;
endmodule
```

# OUTPUT:
<img width="962" alt="320563364-ae673382-8af3-43d2-b5fd-0065ae2ad318" src="https://github.com/navaneethans/VLSI-LAB-EXP-4/assets/166889783/46f701f7-b563-41b4-a419-688d9221659d">

# T FLIPFLOP

![image](https://github.com/navaneethans/VLSI-LAB-EXP-4/assets/6987778/7a020379-efb1-4104-85ee-439d660baa08)
```
CODE:
module T_flipflop(clk,rst,t,q);
input clk,rst,t;
output reg q;
always @(posedge clk)
begin
if (rst==1)
q=1'b0;
else if (t==0)
q=q;
else
q=~q;
end
endmodule
```

# OUTPUT:
<img width="962" alt="320570017-1cc68048-1013-4a9a-831f-53c09e85a9f2" src="https://github.com/navaneethans/VLSI-LAB-EXP-4/assets/166889783/da8b114c-9657-4192-85ea-d34346a239a4">

# D FLIPFLOP

![image](https://github.com/navaneethans/VLSI-LAB-EXP-4/assets/6987778/dda843c5-f0a0-4b51-93a2-eaa4b7fa8aa0)
```
CODE
module Dflipflop(D,clk,reset,Q);
input D;
input clk;
input reset;
output reg Q;
always @(posedge clk)
begin
 if(reset==1'b1)
  Q <= 1'b0;
 else
  Q <= D;
end
endmodule
```

# OUTPUT:
<img width="962" alt="320557855-14d28510-670f-4285-8b8b-2eb32b192557" src="https://github.com/navaneethans/VLSI-LAB-EXP-4/assets/166889783/ba4d85f4-9b7d-4c67-af40-18c3f0a1660d">

# MOD 10 COUNTER

![image](https://github.com/navaneethans/VLSI-LAB-EXP-4/assets/6987778/a1fc5f68-aafb-49a1-93d2-779529f525fa)
```
CODE
module mod10(clk,rst,out);
input clk,rst;
output reg [3:0]out;
always@(posedge clk)
begin
if (rst==1 | out==4'b1001)
out=4'b0000;
else
out=out+1;
end
endmodule
```

# OUTPUT:
<img width="962" alt="320565050-c71dd581-83a7-4c0a-8486-49c17fde037f" src="https://github.com/navaneethans/VLSI-LAB-EXP-4/assets/166889783/12ca0255-bb03-4e17-8f87-9a783d85b9db">

# RIPPLE CARRY COUNTER:
```
CODE

module tff(q,clk,rst);
input clk,rst;
output q;
wire d;
dff df1(q,d,clk,rst);
not n1(d,q);
endmodule
module dff(q,d,clk,rst);
input d,clk,rst;
output q;
reg q;
always @(posedge clk or posedge rst)
begin
if (rst)
q=1'b0;
else 
q=d;
end
endmodule
module ripplecounter(clk,rst,q);
input clk,rst;
output [3:0]q;
tff tf0(q[0],clk,rst);
tff tf1(q[1],q[0],rst);
tff tf2(q[2],q[1],rst);
tff tf3(q[3],q[2],rst);
endmodule
```

# OUTPUT:
<img width="962" alt="320566559-c6c79543-f800-467a-81d7-eb7e67d14cbc" src="https://github.com/navaneethans/VLSI-LAB-EXP-4/assets/166889783/b092bd78-681b-40e5-8833-0466148e4d3c">

# RESULT:
Hence The simulation and synthesis of SR, JK, T, D - FLIPFLOP, COUNTER DESIGN using Vivado 2023 is done and output verified successfully.


