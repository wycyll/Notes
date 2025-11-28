# Counter
```verilog
// 先定义带异步复位的D触发器
module D_ff (q, data_in, clk, rst);
    input  data_in, clk, rst;
    output reg q;
    // 敏感列表：时钟上升沿 或 复位上升沿（异步复位）
    always @(posedge clk or posedge rst) begin
        if (rst == 1'b1) q <= 1'b0;  // 复位时置0
        else q <= data_in;            // 时钟边沿采样输入
    end
endmodule

// 3位同步二进制计数器（调用D_ff）
module counter_3_bit (clock, reset, Q);
    input        clock, reset;
    output [2:0] Q;  // 3位输出（Q[2]=Q₂, Q[1]=Q₁, Q[0]=Q₀）
    wire         D2, D1, D0, and_out;  // 内部组合逻辑信号

    // 1. 组合逻辑：推导D2、D1、D0（对应之前的表达式）
    and  (and_out, Q[1], Q[0]);  // and_out = Q₁·Q₀
    xor  (D2, Q[2], and_out);    // D2 = Q₂ ⊕ (Q₁·Q₀)
    xor  (D1, Q[1], Q[0]);       // D1 = Q₁ ⊕ Q₀
    not  (D0, Q[0]);             // D0 = Q₀'

    // 2. 时序逻辑：实例化3个D触发器
    D_ff DFF2 (Q[2], D2, clock, reset);  // 最高位触发器
    D_ff DFF1 (Q[1], D1, clock, reset);  // 中间位触发器
    D_ff DFF0 (Q[0], D0, clock, reset);  // 最低位触发器
endmodule
```

用`parameter`定义位宽，代码可复用（改 N 即可变 8 位、16 位计数器），无需实例化 D 触发器
```verilog
module counter_N_bit (clock, reset, Q);
    parameter N = 3;  // 可配置位宽，默认3位
    input        clock, reset;
    output reg [N-1:0] Q;  // N位输出（如N=3时为[2:0]）

    // 敏感列表：复位上升沿（优先）或时钟上升沿
    always @(posedge reset or posedge clock) begin
        if (reset == 1'b1) begin
            Q <= {N{1'b0}};  // 复位时置0（N位全0，如3位→000）
        end else begin
            Q <= Q + 1'b1;   // 行为级描述，无需推导D输入
        end
    end
endmodule
```
## Load
Note: Load is a synchronous control signal
```verilog
module counter_N_bit_load (clock, reset, load, Dat, Q);
    parameter N = 3;
    input        clock, reset, load;
    input  [N-1:0] Dat;  // 外部加载数据
    output reg [N-1:0] Q;

    always @(posedge reset or posedge clock) begin
        if (reset == 1'b1)
            Q <= {N{1'b0}};  // reset（优先级1）
        else if (load == 1'b1)
            Q <= Dat;        // load（优先级2）
        else 
            Q <= Q + 1'b1;   // count（优先级3）
    end
endmodule
```
## CE & CEO
```Verilog
module Counter(Q,CEO,CE,load,reset,clk,Data);
parameter N=3;
output reg [N-1:0] Q;
output CEO;
input CE,load,reset,clk;
input [N-1:0] Data;

always @ (posedge clk or posedge reset) begin
if (reset == 1'b1) Q< ={N{1'b0}}; //🐾
else if (load == 1'b1) Q< = Data;
else if (CE==1'b1) Q< =Q + 1 //🐾always里用< = 
else Q< = Q;
end

assign CEO= CE & (&Q); //🐾
endmodule
```
### Test Bench
```verilog
module Test_Bench;
    parameter half_period = 50; 
    parameter counter_size = 4;    //🐾

    // 信号定义：wire接UUT输出，reg接UUT输入
    wire [counter_size-1:0] Q;
    wire CEO;
    reg [counter_size-1:0] Din;
    reg clock, load, reset, CE;

    // 实例化被测试模块（UUT：Unit Under Test）
    counter_N_bit_ce #(counter_size) UUT (clock,reset,load,CE,Din,Q,CEO);
    
    always #half_period clock = ~clock;

    // 2. 生成激励信号（复位→加载→计数→暂停→再计数）
    initial begin
        clock = 0; Din = 0; load = 0; CE = 1; reset = 1;  
        #100 reset = 0;                                  
        #200 Din = 8; load = 1;                          
        #100 load = 0;                                   
        #300 CE = 0;                                     
        #200 CE = 1;                                     
        #1000 $stop;                                     
    end
endmodule
```

# Register & Shifter
## Register

```verilog
   module Reg_N_bits (Q, Din, clock);
   parameter size = 4;  // 默认4位，可修改
   input  [size-1:0] Din;  // n位并行数据输入
   input  clock;            // 时钟（上升沿触发）
   output reg [size-1:0] Q; // n位输出（reg类型存储状态） 
  // 时序逻辑：时钟上升沿更新状态
   always @(posedge clock) begin
   Q <= Din;  // 非阻塞赋值（时序逻辑必用）
   end
  endmodule
   ```
### Load
```verilog
module Reg_N_bits (Q, Din, clock, load);
parameter size = 4;
input  [size-1:0] Din;
input  clock, load;
output reg [size-1:0] Q;
always @(posedge clock) begin
if (load) Q <= Din;  
// else Q <= Q;  
end
endmodule
```
> 本来需要写 else 防止 unwanted latchm，但是此时没关系，因为其他情况都需保持 Q
### Testbench
```verilog
module Test_Bench;
parameter half_period = 50;  
parameter reg_size = 3;      
wire [reg_size-1:0] Q;       
reg [reg_size-1:0] Din;     
reg clock, load;             

Reg_N_bits #(reg_size) UUT (Q, Din, clock, load);

always #half_period clock = ~clock;

initial begin
#0  clock = 0; Din = 0; load = 0;  
#200 Din = 5; load = 1;         
#100 load = 0;                 
#200 Din = 3;                     
#200 load = 1;                  
#1000 $stop;                       
end
endmodule
```

## Register Files
```verilog
module memory (R_data, W_data, W_addr, R_addr, W_en, R_en, clock);
    parameter width = 32;       // register位宽（32位）
    parameter addr_width = 2;   // address有几个bit（2位→4个寄存器）
    parameter number = 2**addr_width;  // register数量（4）

    output reg [width-1:0] R_data;  // 读数据输出
    input  [width-1:0] W_data;      // 写数据输入
    input  [addr_width-1:0] W_addr, R_addr;  // 写/读地址
    input  W_en, R_en, clock;

    reg [width-1:0] memory [number-1:0]; //🐾columns rows

    always @(R_en, R_addr, memory) begin
        R_data = 'bz;  // 读使能无效时，输出高阻（避免总线冲突）
        if (R_en) R_data = memory[R_addr];  // 读使能有效，输出目标寄存器值
    end

    // 写操作（时序逻辑：仅时钟上升沿生效）
    always @(posedge clock) begin
        if (W_en) memory[W_addr] = W_data; 
    end
endmodule
```
### Testbench
```verilog
module test_RF;
parameter width = 32, addr_width = 2, number = 2**addr_width;
parameter duty_c = 20, period = 100;
reg  [width-1:0]        W_data;
reg  [addr_width-1:0]   W_addr, R_addr;
reg                     W_en, R_en, clock;
wire [width-1:0]        R_data;

memory UUT (R_data, W_data, W_addr, R_addr, W_en, R_en, clock);

initial begin
#0    clock=0; W_data=0; W_addr=0; R_addr=0; W_en=0; R_en=0;
#100 W_data=9; W_addr=3; W_en=1;    #100 W_data=22; W_addr=1; W_en=1;
#100 W_en=0; R_en=1; R_addr=3;      #100 R_en = 0;
#100 W_data=177; W_addr=2; W_en=1;  #50  R_addr=1; R_en=1;
#50  W_data=555; W_addr=3;          #50  R_addr=3;
end

always begin #(period-duty_c) clock=~clock; #duty_c clock=~clock; end

initial #1000 $stop;
endmodule
```
## Shift Register
```verilog
module Shift_Reg(Q,Dout,Din,clock);
input clock,Din;
output Dout; //serial out
output reg [3:0] Q; //parallel out
always @ (posedge clock) begin
Q[2:0] < = Q[3:1];
Q[3] < = Din;
end
assign Dout = Q[0];
endmodule
```
### Testbench
```verilog
module Test_Bench;
  parameter half_period = 50;
  wire [3:0] Q;  wire Dout;  reg  Din, clock;

  Shift_Reg UUT (Q, Dout, Din, clock);

  initial begin
    $display("**********************************************************");
    $display("The textual simulation results:");
$display("**********************************************************");
    $monitor($time,,"clock = %d  Din = %b  Q = %b  Dout = %d",
             clock, Din, Q, Dout);
  end

  initial begin
    #0    clock = 0; Din = 0;
    #300 Din = 1;
    #400 Din = 0;
  end

  always #half_period clock = ~clock;

  initial #1000 $stop;
endmodule
```

## Rotate Register
```verilog
  module Barral_Shift_Reg (Q, clock);
  input  clock;
  output [3:0] Q;      
  reg [3:0] Q;      
  always @(posedge clock) begin
  Q[2:0] <= Q[3:1];  // 右移1位
  Q[3] <= Q[0];      // 最低位Q0反馈到最高位Q3
  end
  endmodule
```

### Modified Rotate Register
![image.png](https://raw.githubusercontent.com/wycyll/obsidian-images/master/20251120230036806.png)

# Shifter

```verilog
module Shifter_0_or_1 (Q, I, in, sh);
    parameter size = 4;  // 4位
    input  [size-1:0] I;  // 输入数据
    input  in;            // 移位补值（如左移补0）
    input  sh;            // 移位控制（1=移位，0=保持）
    output reg [size-1:0] Q;  // 输出数据

    // 组合逻辑：输入变化立即输出
    always @(sh, in, I) begin
        if (sh) begin
            Q[size-1:1] = I[size-2:0];  // 左移1位
            Q[0] = in;                  // 最低位补in
        end 
    	else begin
            Q = I; 
        end
    end
endmodule
```
