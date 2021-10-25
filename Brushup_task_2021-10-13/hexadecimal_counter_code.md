```
\m4_TLV_version 1d: tl-x.org
// Lines 1 to 15 are template needed to run in this platform. Leave it as of now
\SV
	m4_include_lib(['https://raw.githubusercontent.com/BalaDhinesh/Virtual-FPGA-Lab/main/tlv_lib/fpga_includes.tlv'])
   m4_makerchip_module   // (Expanded in Nav-TLV pane.)
   	logic [3:0] digit;
      logic [6:0] sseg;
      logic dp;
      top1 dut (clk, reset, digit, sseg, dp);   
\TLV
   m4_define(M4_BOARD, 2)  
   m4+fpga_init()
   m4+fpga_sseg(*digit, *sseg, *dp)
\SV
   endmodule

                   
\SV
	//module seven_segment_counter(input clk, input reset, output [3:0] digit, output [6:0] sseg, output dp);
		// Write your logic here
     // assign digit = 4'b0000;
      //assign dp = 1;
      //assign sseg = 7'b0000000;
      //counter c (clk, digit);
   //endmodule
   
   module top1(input clk, input reset, output [3:0] digit, output [6:0] sseg, output dp);
      reg [3:0] num ; 
      counter c (clk, num);
      display d (clk, reset, num, digit, sseg, dp);
   endmodule
                   
   module counter(input clk, output [3:0] num);
      reg [3:0] t = 4'b0000;
      always @(posedge clk)
      begin
      	t = t + 1;
      end 
      assign num = t;
  endmodule
                   
  module display(input clk, input reset, input [3:0] num, output [3:0] digit, output [6:0] sseg, output dp);
     assign sseg = (num == 4'b0000) ? 7'b0000001 : 
                   (num == 4'b0001) ? 7'b1001111 :
                   (num == 4'b0010) ? 7'b0010011 :
                   (num == 4'b0011) ? 7'b0000110 :
                   (num == 4'b0100) ? 7'b1001100 :
                   (num == 4'b0101) ? 7'b0100100 :
                   (num == 4'b0110) ? 7'b0100000 :
                   (num == 4'b0111) ? 7'b0001111 :
                   (num == 4'b1000) ? 7'b0000000 :
                   (num == 4'b1001) ? 7'b0000100 :
                   (num == 4'b1010) ? 7'b0000010 :
                   (num == 4'b1011) ? 7'b1100000 :
                   (num == 4'b1100) ? 7'b0110001 :
                   (num == 4'b1101) ? 7'b1000010 :
                   (num == 4'b1110) ? 7'b0010000 :
                   (num == 4'b1111) ? 7'b0111000 :
                   7'b1111111;
     assign digit = 4'b0000;
     assign dp = 1;
   endmodule
```
