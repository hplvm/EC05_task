Project link : http://makerchip.com/sandbox/0J6f8hnZW/08qhwY3

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
	module counter(input clk, output [7:0] count);
      reg [7:0] c = 0;
      always @(posedge clk)
         c = c + 1;
      assign count = c;
	endmodule
                   
	module character(input clk, input [7:0] count, output [6:0] char1, output [6:0] char2, output [6:0] char3, output [6:0] char4);
      assign char1 = (count % 24 < 4) ? 7'b0010000 :
                     (count % 24 < 8) ? 7'b0110001 :
                     (count % 24 < 12) ? 7'b1111110 :
                     (count % 24 < 16) ? 7'b0000001 :
                     (count % 24 < 20) ? 7'b0100100 :
                                          7'b1111111 ;
      
     assign char2 = (count % 24 < 4) ? 7'b0110001 :
                     (count % 24 < 8) ? 7'b1111110 :
                     (count % 24 < 12) ? 7'b0000001 :
                     (count % 24 < 16) ? 7'b0100100 :
                     (count % 24 < 20) ? 7'b1111111 :
                                          7'b0010000 ;
      
      assign char3 = (count % 24 < 4) ? 7'b1111110 :
                     (count % 24 < 8) ? 7'b0000001 :
                     (count % 24 < 12) ? 7'b0100100 :
                     (count % 24 < 16) ? 7'b1111111 :
                     (count % 24 < 20) ? 7'b0010000 :
                                          7'b0110001 ;
      
      assign char4 = (count % 24 < 4) ? 7'b0000001 :
                     (count % 24 < 8) ? 7'b0100100 :
                     (count % 24 < 12) ? 7'b1111111 :
                     (count % 24 < 16) ? 7'b0010000 :
                     (count % 24 < 20) ? 7'b0110001 :
                                          7'b1111110 ;
	endmodule
                   
	module disp_val(input clk, input [7:0] count, input [7:0] char1, input [7:0] char2, input [7:0] char3, input [7:0] char4, output [3:0] digit, output [6:0] sseg);
      assign digit = (count % 4 == 0) ? 4'b0111 :
                     (count % 4 == 1) ? 4'b1011 :
                     (count % 4 == 2) ? 4'b1101 :
                                          4'b1110 ;
      
      assign sseg = (count % 4 == 0) ? char1 :
                    (count % 4 == 1) ? char2 :
                    (count % 4 == 2) ? char3 :
                                         char4 ;
	endmodule
                   
	module top1(input clk, input reset, output [3:0] digit, output [6:0] sseg, output dp);
      reg [7:0] count;
      reg [6:0] char1;
      reg [6:0] char2;
      reg [6:0] char3;
      reg [6:0] char4;
      counter cou(clk, count);
      character char (clk, count, char1, char2, char3, char4);
      disp_val disp (clk, count, char1, char2, char3, char4, digit, sseg);
      assign dp=1;
	endmodule
```

### Output

Eg: to display `ec-05`

`ec-0`

`c-05`

`-05 `

`05 e`

`5 ec`

` ec-`

`ec-0`

![](https://github.com/hplvm/EC05_task/blob/main/media-attachments/scroll_text.gif)
