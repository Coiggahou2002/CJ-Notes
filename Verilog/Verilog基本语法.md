# Verilog | 基本语法

## 数据表示

`进制数'b二进制串`

`3'b010` 表示 3 位二进制数 010

`8'b0001_0100` 表示 8 位二进制数 00010100

## 赋值与运算

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

## Vector

### 基本

`type [upper:lower] vector_name`

```verilog
wire [7:0] my_vector;  // 声明
assign out = my_vector[7];  // 取值
wire [7:0] a;
wire [7:0] b;
assign b = a; // 整个赋值
```

### 截断赋值

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

### 区间赋值

```verilog
assign out[31:24] = in[7:0];
assign out[23:16] = in[15:8];
assign out[15:8] = in[23:16];
assign out[7:0] = in[31:24];
```

### 对数组使用布尔运算的区别

> A bitwise operation between two N-bit vectors replicates the operation for each bit of the vector and produces a N-bit output, while a logical operation treats the entire vector as a boolean value (true = non-zero, false = zero) and produces a 1-bit output.

使用按位的布尔运算，则对每一位进行运算；

使用逻辑布尔运算，就将数组整体视为一个布尔值进行运算.

```verilog
module top_module( 
    input [2:0] a,
    input [2:0] b,
    output [2:0] out_or_bitwise,
    output out_or_logical,
    output [5:0] out_not
);
    
    assign out_or_bitwise = a | b;   // 按位与，将两个数组每一位都执行与运算
    assign out_or_logical = a || b;  // 逻辑与，数组视为整体，有值则1，否则0，只输出一个bit
    assign out_not[2:0] = ~a;        // 按位取反，数组每一位都取反
    assign out_not[5:3] = ~b;

endmodule
```

### 解构赋值

例子：

![](https://cjpark-1304138896.cos.ap-guangzhou.myqcloud.com/note_img/20211102150017.png)

```verilog
module top_module (
    input [4:0] a, b, c, d, e, f,
    output [7:0] w, x, y, z );
    
    wire [1:0] tmp;
    assign tmp[0] = 1;
    assign tmp[1] = 1;
    assign {w, x, y, z} = {a, b, c, d, e, f, tmp};

endmodule
```

### 复制操作符

```verilog
{2{a,b,c}}          // The same as {a,b,c,a,b,c}
```

例子：`out` 输出为 `in` 后面跟上 24 个 `in[7]`

```verilog
module top_module (
    input [7:0] in,
    output [31:0] out );
    
    assign out = {{24{in[7]}}, in[7:0]};
    // assign out = { replicate-sign-bit , the-input };
    
endmodule
```

典型例子：

![](https://cjpark-1304138896.cos.ap-guangzhou.myqcloud.com/note_img/20211102152625.png)

```verilog
module top_module (
    input a, b, c, d, e,
    output [24:0] out );//
    
    wire [24:0] repeat_vec;
    assign repeat_vec = {{5{a}},{5{b}},{5{c}},{5{d}},{5{e}}};
    
    wire [24:0] seq_vec;
    assign seq_vec = {5{a,b,c,d,e}};
    
    assign out = ~repeat_vec ^ seq_vec;

endmodule
```

