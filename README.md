# VLSI-LAB-EXP-4
# DATE:

# SIMULATION AND IMPLEMENTATION OF SEQUENTIAL LOGIC CIRCUITS

# AIM: 
 To simulate and synthesis SR, JK, T, D - FLIPFLOP, COUNTER DESIGN using  Vivado 2023.1.

# APPARATUS REQUIRED:

 Vivado 2023.1.

   
PROCEDURE:
```
1.Open Vivado: Launch Xilinx Vivado software on your computer.
2.Create a New Project: Click on "Create Project" from the welcome page or navigate through "File" > "Project" > "New".
3.Project Settings: Follow the prompts to set up your project. Specify the project name, location, and select RTL project type.
4.Add Design Files: Add your Verilog design files to the project. You can do this by right-clicking on "Design Sources" in the Sources window, then selecting "Add Sources". Choose your Verilog files from the file browser.
5.Specify Simulation Settings: Go to "Simulation" > "Simulation Settings". Choose your simulation language (Verilog in this case) and simulation tool (Vivado Simulator).
6.Run Simulation: Go to "Flow" > "Run Simulation" > "Run Behavioral Simulation". This will launch the Vivado Simulator and compile your design for simulation.
7.Set Simulation Time: In the Vivado Simulator window, set the simulation time if it's not set automatically. This determines how long the simulation will run.
8.Run Simulation: Start the simulation by clicking on the "Run" button in the simulation window.
9.View Results: After the simulation completes, you can view waveforms, debug signals, and analyze the behavior of your design.
```

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
<img width="962" alt="330645714-5e076d15-8161-44ae-b281-88a0986745b9" src="https://github.com/navaneethans/VLSI-LAB-EXP-4/assets/166889783/a86db1f2-eb05-48f1-9c25-1b8d2212b5c1">

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
<img width="962" alt="330645575-5f1c7ee0-2539-4927-971d-17af30a95f6f" src="https://github.com/navaneethans/VLSI-LAB-EXP-4/assets/166889783/446c2ea2-213d-4f41-8379-2e6cde8cf232">

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
<img width="962" alt="330645761-756d6dfa-9934-4e84-a3e8-2a4fff7d4cd0" src="https://github.com/navaneethans/VLSI-LAB-EXP-4/assets/166889783/e8362efa-6ac7-485b-ad4c-8d4ed33a9888">

# D FLIPFLOP

![image](https://github.com/navaneethans/VLSI-LAB-EXP-4/assets/6987778/dda843c5-f0a0-4b51-93a2-eaa4b7fa8aa0)
```
CODE:

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
<img width="962" alt="330645235-efab7dd3-4cfa-4202-a854-f0ff7b761846" src="https://github.com/navaneethans/VLSI-LAB-EXP-4/assets/166889783/ac5332b5-690f-4139-8c91-af3fb64601bd">

# COUNTER

![image](https://github.com/navaneethans/VLSI-LAB-EXP-4/assets/6987778/a1fc5f68-aafb-49a1-93d2-779529f525fa)
```
CODE:

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
<img width="962" alt="330645610-9ac8a668-1324-4080-8cad-907071fe6ad8" src="https://github.com/navaneethans/VLSI-LAB-EXP-4/assets/166889783/7eec172b-e758-455c-856f-b77824003418">

```
CODE:

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
<img width="962" alt="330645673-d8b215f4-cd6c-4c6e-aa96-643f8992aa90" src="https://github.com/navaneethans/VLSI-LAB-EXP-4/assets/166889783/cf9f865a-fd47-450c-bd45-6f4f2fe43b6f">

# RESULT:
 Hence The simulation and synthesis of SR, JK, T, D - FLIPFLOP, COUNTER DESIGN using Vivado 
 2023 is done and output verified successfully.

