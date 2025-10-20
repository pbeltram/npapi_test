## FPGA designs

**RedPitaya**

[Block design for pitaya_rec FPGA_Rev=9293](./pitaya_rec.blockd_design.pdf)
Data 32bit `ADC[32-1:0] = { ChA[16-1:0], ChB[16-1:0] }`.

**ZyboZ7**

[Block design for zyboz7_rec FPGA_Rev=10191](./zyboz7_rec.block_design.pdf)
Mapping of [PMOD](./pmod_connector.jpeg) DIO pins to FPGA input `DIO[32-1:0] =  { JD[8-1:0], JE[8-1:0], JB[8-1:0], JC[8-1:0] }`.
