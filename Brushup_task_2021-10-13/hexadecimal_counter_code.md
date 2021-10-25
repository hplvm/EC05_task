Project link : https://makerchip.com/sandbox/0ERfWhK8n/076hWWg

### Code
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
	
   module top1(input clk, input reset, output [3:0] digit, output [6:0] sseg, output dp); //combine counter n display modules
      reg [3:0] num ; 
      counter c (clk, num);
      display d (clk, reset, num, digit, sseg, dp);
   endmodule
                   
   module counter(input clk, output [3:0] num); //4-bit counter
      reg [3:0] t = 4'b0000;
      always @(posedge clk)
      begin
      	t = t + 1;
      end 
      assign num = t;
  endmodule
                   
  module display(input clk, input reset, input [3:0] num, output [3:0] digit, output [6:0] sseg, output dp); //seven segment display 
     assign sseg = (num == 4'b0000) ? 7'b0000001 : //0
                   (num == 4'b0001) ? 7'b1001111 : //1
                   (num == 4'b0010) ? 7'b0010011 : //2
                   (num == 4'b0011) ? 7'b0000110 : //3
                   (num == 4'b0100) ? 7'b1001100 : //4
                   (num == 4'b0101) ? 7'b0100100 : //5
                   (num == 4'b0110) ? 7'b0100000 : //6
                   (num == 4'b0111) ? 7'b0001111 : //7
                   (num == 4'b1000) ? 7'b0000000 : //8
                   (num == 4'b1001) ? 7'b0000100 : //9
                   (num == 4'b1010) ? 7'b0000010 : //a
                   (num == 4'b1011) ? 7'b1100000 : //b
                   (num == 4'b1100) ? 7'b0110001 : //c
                   (num == 4'b1101) ? 7'b1000010 : //d
                   (num == 4'b1110) ? 7'b0010000 : //d
                   (num == 4'b1111) ? 7'b0111000 : //d
                   7'b1111111;                     //e
     assign digit = 4'b0000;
     assign dp = 1;
   endmodule
```
