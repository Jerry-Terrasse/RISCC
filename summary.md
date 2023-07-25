# ���������Ľӿ��źš�λ����������

## �����߼���Ԫ ALU

```verilog
module ALU(
    input wire [31: 0] A,
    input wire [31: 0] B,
    input wire [3: 0] op,

    output reg [31: 0] C,
    output reg zf,
    output reg sf
);
```

|�ӿ�|λ��|��������|
|-|-|-|
|A|32|����˿�A|
|B|32|����˿�B|
|op|4|������|
|C|32|����˿�C|
|zf|1|���־λ|
|sf|1|���ű�־λ|

## ������ Controller

```verilog
module Controller(
    input wire [10: 0] inst,
    
    output wire pc_sel,
    output wire [1: 0] npc_op,
    output wire br_sel,
    output wire rf_we,
    output wire [2: 0] rf_wsel,
    output wire [2: 0] sext_op,
    output wire [3: 0] alu_op,
    output wire alub_sel,
    output wire [2: 0] ram_mode
);
```

|�ӿ�|λ��|��������|
|-|-|-|
|inst|11|��Ҫ��ָ�����λ|
|pc_sel|1|PCѡ���ź�|
|npc_op|2|NPC�����ź�|
|br_sel|1|��֧ѡ���ź�|
|rf_we|1|�Ĵ���дʹ��|
|rf_wsel|3|�Ĵ���дѡ���ź�|
|sext_op|3|������չ�����ź�|
|alu_op|4|ALU������|
|alub_sel|1|ALU�ڶ���������ѡ���ź�|
|ram_mode|3|RAM��д�����ź�|

## ��������� DM

```verilog
module DM(
    input wire [2: 0] op,
    input wire [31: 0] rdo,
    input wire [31: 0] a_i,
    input wire [31: 0] wdi,

    output wire [31: 0] a_o,
    output reg [3: 0] wen,
    output reg [31: 0] wdo,
    output reg [31: 0] rdo_ext
);
```

|�ӿ�|λ��|��������|
|-|-|-|
|op|3|������|
|rdo|32|�����ݣ���DRAM��|
|a_i|32|��ַ���루δ���룩|
|wdi|32|д���ݣ���CPU��|
|a_o|32|��ַ��������룩|
|wen|4|дʹ��|
|wdo|32|д���ݣ���DRAM��|
|rdo_ext|32|�����ݣ�������չ����CPU��|

## �Ĵ����� Register File

```verilog
module RF(
    input wire rst,
    input wire clk,
    input wire [4: 0] rR1,
    input wire [4: 0] rR2,

    input wire [4: 0] wR,
    input wire we,
    input wire [31: 0] wD,

    output reg [31: 0] rD1,
    output reg [31: 0] rD2
);
```

|�ӿ�|λ��|��������|
|-|-|-|
|rst|1|��λ�ź�|
|clk|1|ʱ���ź�|
|rR1|5|���˿�1��ַ|
|rR2|5|���˿�2��ַ|
|wR|5|д�˿ڵ�ַ|
|we|1|дʹ��|
|wD|32|д����|
|rD1|32|���˿�1����|
|rD2|32|���˿�2����|

## PC

```verilog
module PC(
    input wire rst,
    input wire clk,
    input wire [29: 0] din,

    output reg [31: 0] pc
);
```

|�ӿ�|λ��|��������|
|-|-|-|
|rst|1|��λ�ź�|
|clk|1|ʱ���ź�|
|din|30|�������ݣ���2λĬ��Ϊ0��|
|pc|32|�������|

## ��������������� NPC

```verilog
module NPC(
    input wire [29: 0] pc,
    input wire [29: 0] offset,
    input wire br,
    input wire [1: 0] op,

    output reg [29: 0] npc,
    output wire [31: 0] pc4,
    output wire [31: 0] pcb
);
```

|�ӿ�|λ��|��������|
|-|-|-|
|pc|30|����PC|
|offset|30|����ƫ����|
|br|1|��֧�ź�|
|op|2|NPC�����ź�|
|npc|30|���NPC|
|pc4|32|���PC+4|
|pcb|32|���PC+ƫ����|

## ������չ�� SEXT

```verilog
module SEXT(
    input wire [2: 0] op,
    input wire [24: 0] din, // inst[31: 7]

    output reg [31: 0] ext
);
```

|�ӿ�|λ��|��������|
|-|-|-|
|op|3|������|
|din|25|��������|
|ext|32|�������|