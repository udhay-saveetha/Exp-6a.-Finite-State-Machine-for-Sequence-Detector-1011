# Exp-6a.-Finite-State-Machine-for-Sequence-Detector-1011
# Aim 
To design and simulate a Finite-State-Machine-for-Sequence-Detector-1011 using Verilog HDL, and verify its functionality through a testbench in the Vivado 2023.1 environment. 

# Apparatus Required 

Vivado 2023.1 

# Procedure

Launch Vivado 2023.1 Open Vivado and create a new project.
Design the Verilog Code Write the Verilog code for the RAM,ROM,FIFO
Create the Testbench Write a testbench to simulate the memory behavior. The testbench should apply various and monitor the corresponding output.
Create the Verilog Files Create both the design module and the testbench in the Vivado project.
Run Simulation Run the behavioral simulation to verify the output.
Observe the Waveforms Analyze the output waveforms in the simulation window, and verify that the correct read and write operation.
Save and Document Results Capture screenshots of the waveform and save the simulation logs. These will be included in the lab report.
# Code

# Mealy 1011

Verilog code
     
    module mealy_fsm_1011(
    input clk,rst,xin,
    output reg zout);
    parameter [2:0] s1 = 3'b000,
                s2 = 3'b001,
                s3 = 3'b010,
                s4 = 3'b011;
    reg [2:0] ps,ns;
    always@(posedge clk)
    begin
    if(rst)
    ps <= s1;
    else
    ps <= ns;
    end      
    always@(xin or ps)
    begin 
    case(ps)
    s1 : if(xin) begin
    ns = s2;
    zout = 0;
    end             
    else  
    begin
    ns = s1;
    zout = 0;
    end 
    s2 : if(xin) begin
    ns = s2;
    zout = 0;
    end             
    else  begin
    ns = s3;
    zout = 0;
    end             
    s3 : if(xin) begin
    ns = s4;
    zout = 0;
    end             
    else  begin
    ns = s1;
    zout = 0;
    end              
    s4 : if(xin) begin
    ns = s1;
    zout = 1;
    end             
    else  
    begin
    ns = s3;
    zout = 0;
    end              
    endcase
    end
    endmodule

 Test bench
 
    module mealy_fsm_1011_tb;
    reg clk_t,rst_t,xin_t;
    wire zout_t;
    mealy_fsm_1011 dut(.clk(clk_t),.rst(rst_t),.xin(xin_t),.zout(zout_t));
    initial
    begin
    clk_t = 1'b1;
    rst_t = 1'b1;
    #100
    rst_t = 1'b0;
    xin_t = 1'b1;
    #100
    xin_t = 1'b0;
    #100
    xin_t = 1'b1;
    #100
    xin_t = 1'b1;
    #100
    xin_t = 1'b1;
    #100
    xin_t = 1'b0;
    #100
    xin_t = 1'b1; 
    #100
    xin_t = 1'b1;              
    end
    always #50  clk_t = ~clk_t;                 
    endmodule


output Waveform
<img width="1919" height="1199" alt="image" src="https://github.com/user-attachments/assets/fabffecf-c58b-445b-a8e6-f593fe2b3bd7" />



# Moore 1011

Verilog code

    module moore (
    input clk, reset, x,        
    output reg z              
    );
    parameter S0 = 3'b000,  
          S1 = 3'b001,  
          S2 = 3'b010,  
          S3 = 3'b011,  
          S4 = 3'b100;  
    reg [2:0] state, next_state;
    always @(posedge clk or posedge reset) 
    begin
    if (reset)
    state <= S0;
    else
    state <= next_state;
    end
    always @(*) begin
    case (state)
    S0: next_state = (x) ? S1 : S0;
    S1: next_state = (x) ? S1 : S2;
    S2: next_state = (x) ? S3 : S0;
    S3: next_state = (x) ? S4 : S2;
    S4: next_state = (x) ? S1 : S2;
    default: next_state = S0;
    endcase
    end
    always @(*) 
    begin
    case (state)
    S4: z = 1'b1;
    default: z = 1'b0;
    endcase
    end
    endmodule
    
Test bench

    `timescale 1ns / 1ps
    module moore_tb;
    reg clk, reset, x;
    wire z;
    moore uut (
        .clk(clk),
        .reset(reset),
        .x(x),
        .z(z)
    );
    initial begin
        clk = 1;
        forever #5 clk = ~clk;   
    end
    initial begin
        reset = 1; x = 0;
        #10 reset = 0;        
        #10 x = 1;
        #10 x = 0;
        #10 x = 1;
        #10 x = 1;
        #10 x = 0;
        #10 x = 1;
        #10 x = 1;
        #20 $finish;             
    end
    initial begin
        $monitor("Time=%0t | clk=%b | reset=%b | x=%b | z=%b | state=%b", 
                  $time, clk, reset, x, z, uut.state);
    end
    endmodule
    
// output Waveform
<img width="1919" height="1199" alt="image" src="https://github.com/user-attachments/assets/cd51b13c-1323-46bf-ac96-704835094c93" />

Conclusion
The Mealy and Moore state machine for sequence 1011 was designed and successfully simulated using Verilog HDL. The testbench verified both the write and read functionalities by simulating the sequence operations and observing the output waveforms.
