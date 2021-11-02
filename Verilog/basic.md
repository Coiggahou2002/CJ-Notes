# Verilog | 基本语法

声明模块 + 赋值
```verilog
module top_module( input in, output out );
    assign out = in;
endmodule
```

简单运算

```verilog
assign out = !in;  // 整个取反
assign out = ~in;  // 按位取反
assign out = a && b;  // 逻辑与
assign out = a & b;   // 按位与
assign out = a ^ b;
assign out = a || b; // 逻辑或
assign out = a | b; // 按位或
```

创建中间件

```verilog
wire a;
assign a = in;
```

Vector

`type [upper:lower] vector_name`

```verilog
wire [7:0] my_vector;  // 声明
assign out = my_vector[7];  // 取值
wire [7:0] a;
wire [7:0] b;
assign b = a; // 整个赋值
```

Vector 截断赋值

```verilog
`default_nettype none     // Disable implicit nets. Reduces some types of bugs.
module top_module( 
    input wire [15:0] in,
    output wire [7:0] out_hi,
    output wire [7:0] out_lo );
    assign out_hi = in[15:8];
    assign out_lo = in[7:0];
endmodule
```

