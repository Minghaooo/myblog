---
title: Intro to SystemVerilog
date: 2020-05-30 15:56:41
tags: VLSI
---
记录遇到的 SystemVerilog 中与 Verilog 不同的特性， 内容涉及但不仅限于ECE551，552等课程。
<!--more-->
### SV 中的语法
    “#” 延时控制

###  test bench Components

```
Generator	    Generates different input stimulus to be driven to DUT
Interface	    Contains design signals that can be driven or monitored
Driver	        Drives the generated stimulus to the design
Monitor	        Monitor the design input-output ports to capture design activity
Scoreboard	    Checks output from the design with expected behavior
Environment	    Contains all the verification components mentioned above
Test	        Contains the environment that can be tweaked with different configuration settings
```

### Data Types

#### Nets (wire) and  Variable(reg)

0	represents a logic zero, or a false condition
1	represents a logic one, or a true condition
x	represents an unknown logic value (can be zero or one)
z	represents a high-impedance state

#### Other data types

##### Integer

An integer is a general purpose variable of 32-bits wide that can be used for other purposes while modeling hardware and stores integer values.

##### time

A time variable is unsigned, 64-bits wide and can be used to store simulation time quantities for debugging purposes. A realtime variable simply stores time as a floating point quantity.

##### real
A real variable can store floating point values and can be assigned the same way as integer and reg.

##### Verilog Strings

Strings are stored in reg, and the width of the reg variable has to be large enough to hold the string. Each character in a string represents an ASCII value and requires 1 byte. If the size of the variable is smaller than the string, then Verilog truncates the leftmost bits of the string. If the size of the variable is larger than the string, then Verilog adds zeros to the left of the string.

```
reg [8*11:1] str = "Hello World";         // Variable can store 11 bytes, str = "Hello World"
reg [8*5:1]  str = "Hello World";         // Variable stores only 5 bytes (rest is truncated), str = "World"
reg [8*20:1] str = "Hello World";         // Variable can store 20 bytes (rest is padded with zeros), str = "         Hello World"
```

### convert to System Verilog

<img src="data-types.png" width="90%">

0	    Logic state 0 - element/net is at 0 volts
1	    Logic state 1 - element/net is at some value > 0.7 volts
x or X	Logic state X - element/net has either 0/1 - we just don't know
z or Z	Logic state Z - net has high impedence - maybe the wire is not connected and is floating

#### String

each character contains 8 bits

```
// "Hello World" has 11 characters and each character requires 8-bits
bit [8*17:1] myMessage  = "Hello World";
string       myMessage2 = "Hello World";    // Uses "string" data type
```
#### structures

```
// Create a structure to store "int" and "real" variables
// A name is given to the structure and declared to be a data type so
// that this name "s_money" can be used to create structure variables
typedef struct {
	int 	coins;
	real 	dollars;
} s_money;

// Create a structure variable of type s_money
s_money wallet;

wallet = '{5, 19.75};                       // Assign direct values to a structure variable
wallet = '{coins:5, dollars:19.75};         // Assign values using member names
wallet = '{default:0};                      // Assign all elements of structure to 0
wallet = s_money'{int:1, dollars:2};        // Assign default values to all members of that type

// Create a structure that can hold 3 variables and initialize them with 1
struct {
	int  A, B, C;
} ABC = '{3{1}}; 							// A = B = C = 1

// Assigning an array of structures
s_money purse [1:0] = '{'{2, 4.25}, '{7,1.5}};

```

#### Array

```
module tb;
  	// The following two representations of fixed arrays are the same
  	// myFIFO and urFIFO have 8 locations where each location can hold an integer value
  	// 0,1 | 0,2 | 0,3 | ... | 0,7
  	int   myFIFO  [0:7];
  	int   urFIFO  [8];

   	// Multi-dimensional arrays
  	// 0,0 | 0,1 | 0,2
  	// 1,0 | 1,1 | 1,2
  	int   myArray  [2][3];

  	initial begin
      myFIFO[5] = 32'hface_cafe;     // Assign value to location 5 in 1D array
      myArray [1][1] = 7;	         // Assign to location 1,1 in 2D array

      // Iterate through each element in the array
      foreach (myFIFO[i])
        $display ("myFIFO[%0d] = 0x%0h", i, myFIFO[i]);

      // Iterate through each element in the multidimensional array
      foreach (myArray[i])
        foreach (myArray[i][j])
          $display ("myArray[%0d][%0d] = %0d", i, j, myArray[i][j]);

	end
endmodule
```

#### real to int

Real numbers will be converted to integers by rounding the real number to the nearest integer instead of truncating it. If the fractional part is exactly 0.5, it will be rounded away from zero. Explicit conversion can be specified using casting or using system tasks. Directly assigning a real value to an integral type will also round instead of truncate.s


#### 4 and 2 state data types

4 sate data type : reg, wire, logic

2-state data types : bit

* Integer   :
* signed    :   si = signed' (ubyte);s
* unsigned  :
* byte      : default is signed, can be changed to unsigned

#### String
<img src="string-operators.png" width="90%">

<img src="stringOperation.png" width="90%">
<img src="stringConversation.png" width="90%">

#### Enumeration
name	        The next number will be associated with name
name = C	    Associates the constant C to name
name[N]	        Generates N named constants : name0, name1, ..., nameN-1
name[N] = C	    First named constant gets value C and subsequent ones are associated to consecutive values
name[N:M]	    First named constant will be nameN and last named constant nameM, where N and M are integers
name[N:M] = C	First named constant, nameN will get C and subsequent ones are associated to consecutive values until nameM

#### Static Arrays

#####  Packed Arrays

定义在变量名之后的是 unpacked，此时是跟其他语言中一样的。
定义在变量名字之前的 packed， 此时是定义变量的位宽，无论多少维都可以抽象成一条连续的。
packed array 就算是 多维定义，其在地址上也是连续的， 跟一维的实际上没啥区别，只是索引更方便了点而已，在内存中还是连续的。相当于cache line 中的不同word。
而不同的unpacked 相当于，不同的cache line。

* A one-dimensional packed array is also called as a vector.
* A multidimensional packed array is still a set of contiguous bits but are also segmented into smaller groups.

```
	bit [7:0] 	m_data; 	// A vector or 1D packed array
    bit [3:0][7:0] 	m_data; 	// A Multidimensional Packed Arrays, 4 bytes
    bit [2:0][3:0][7:0] 	m_data; 	// An MDA, 12 bytes
```
#### unpacked array

An unpacked array is used to refer to dimensions declared after the variable name.
Unpacked arrays may be fixed-size arrays, dynamic arrays, associative arrays or queues.

```
byte 	stack [8]; 		// depth = 8, 1 byte wide variable
byte 	stack [2][4]; 		// 2 rows, 4 cols
```


#### Packed + Unpacked Array

多重素组时，索引是逆时针的。
<img src="001.png" width="90%">
<img src="002.png" width="90%">

```
 bit [3:0][7:0] 	stack [2][4]; 		// 2 rows, 4 cols

\\\In a multidimensional declaration, the dimensions declared before the name vary more faster than the dimensions following the name.
bit 	[1:4] 		m_var 	[1:5]			// 1:4 varies faster than 1:5
bit 				m_var2 	[1:5] [1:3]  	// 1:3 varies faster than 1:5
bit 	[1:3] [1:7] m_var3; 				// 1:7 varies faster than 1:3

bit 	[1:3] [1:2] m_var4 [1:7] [0:2] 		// 1:2 varies most rapidly, followed by 1:3, then 0:2 and then 1:7

```

经观察具有以下性质：
* 组合和非组合混合时，名称前面的优先填充，而名字后面的选择名字前面的
* 当多个[]叠加时，前面的 选择后面的。变化慢的选择变化的。 


#### Dynamic Arrays

A dynamic array is one whose size is not known during compilation, but instead is defined and expanded as needed during runtime. A dynamic array is easily recognized by its empty square brackets [ ].

```
int 		m_mem []; 	// Dynamic array, size unknown but it holds integer

function int size ();	Returns the current size of the array, 0 if array has not been created
function void delete ();	Empties the array resulting in a zero-sized array
```

#### Associative Arrays

```
int 		m_data [int]; 			// Key is of type int, and data is also of type int
int 		m_name [string]; 		// Key is of type string, and data is of type int

m_name ["Rachel"] = 30;
m_name ["Orange"] = 2;

m_data [32'h123] = 3333;
```

#### Queues
A queue is a data type where data can be either pushed into the queue or popped from the array. It is easily recognized by the $ symbol inside square brackets [ ].
```
int 	m_queue [$]; 		// Unbound queue, no size

m_queue.push_back(23); 		// Push into the queue
int data = m_queue.pop_front(); // Pop from the queue

```

#### structure 和 typedef

跟 C 语言差不多，问题不大。

### Loops
forever	    Runs the given set of statements forever
repeat	    Repeats the given set of statements for a given number of times
while	    Repeats the given set of statements as long as given condition is true
for	        Similar to while loop, but more condense and popular form
do while	Repeats the given set of statements at least once, and then loops as long as condition is true
foreach	    Used mainly to iterate through all elements in an array

### Multi threads
<img src="003.png" width="90%">

for 可以在最后被 disable。disable 以后kill 在运行的子线程。

### Interprocess Communication
线程之间的互动一共有3种。 
#### Event
```
module tb_top;
	event eventA; 		// Declare an event handle called  "eventA"

	initial begin
		fork
			waitForTrigger (eventA);    // Task waits for eventA to happen
			#5 ->eventA;                // Triggers eventA
		join
	end

	// The event is passed as an argument to this task. It simply waits for the event
	// to be triggered
	task waitForTrigger (event eventA);
		$display ("[%0t] Waiting for EventA to be triggered", $time);
		wait (eventA.triggered);
		$display ("[%0t] EventA has triggered", $time);
	endtask
endmodule
```
Event 更像是信号量，只能互相告知是否已经trigger。有点像C语言里面的 信号量。
使用@event 的时候注意trigger 时，wait 是否已经开始，不然的话就要使用 wait(event.triggered)

同时还可以有 wait_order() 来确保 trigger 的顺序是你想要的顺序。

#### Semaphore
之前在Operation system 课上用的比较多，就不多说了。

```
 semaphore key;
  key = new (1); 
  key.get (1);
  key.put (1);
```
#### Mailbox
A mailbox is like a dedicated channel established to send data between two components.
用来两个线程之间互相发送信息。
```
// Create a new general mailbox that can hold utmost 2 items
  	mailbox 	mbx = new(2);

// with 
typedef mailbox #(string) s_mbox;

```

### Interface

Interface 是将一个系列接口的信号打包在一起，方便链接等。
Interface 中使用 logic 来定义信号，用 modport 来定义信号的方向。
Interface 使用是要先例化: module dut (myBus busIf);
```
interface myBus (input clk);
  logic [7:0]  data;
  logic      enable;

  // From TestBench perspective, 'data' is input and 'write' is output
  modport TB  (input data, clk, output enable);

  // From DUT perspective, 'data' is output and 'enable' is input
  modport DUT (output data, input enable, clk);
endinterface
```
#### clocking blocks
用来给clk 增加延时等，用于仿真等操作。

Signals that are specified inside a clocking block will be sampled/driven with respect to that clock. There can be mulitple clocking blocks in an interface. Note that this is for testbench related signals. You want to control when the TB drives and samples signals from DUT. Solves some part of the race condition, but not entirely. You can also parameterize the skew values.

```
[default] clocking [identifier_name] @ [event_or_identifier]
	default input #[delay_or_edge] output #[delay_or_edge]
	[list of signals]
endclocking

// example of input skew
clocking cb @(posedge clk);
	input #1step req;
endclocking

// output skew
clocking cb_1 @(posedge clk);
    output #2 req;
endclocking
```

### Class
class is a user-defined datatype, an OOP construct, that can be used to encapsulate data (property) and tasks/functions (methods) which operate on the data.

创建一个class 的array
```
module tb_top;
	myPacket pkt0 [3];

	initial begin
    	for(int i = 0; i < $size (pkt0); i++) begin
   	   		pkt0[i] = new ();
       		pkt0[i].display ();
   		end
   	end
endmodule
```
#### Inheritance 
新的pkt 继承了myPacket， 用过super关键词可以使用my packet 里面的内容
```
class networkPkt extends myPacket;
	bit        parity;
	bit [1:0]  crc;

	function new ();
		super.new ();
		this.parity = 1;
		this.crc = 3;
	endfunction

	function display ();
		super.display();
		$display ("Parity = %0b, CRC = 0x%0h", this.parity, this.crc);
	endfunction
endclass
```

#### abstract/virtual class
不可以被例化

```
virtual class Base;
   bit [7:0]   data;
   bit         enable;
endclass
```

#### Constructors
这个比较有意思，，函数名字竟然叫 new。 super.new 是parent class 的constructor。 
```
class Packet;
  bit [31:0] addr;

  function new ();
  super.new();
    addr = 32'hfade_cafe;
  endfunction
endclass
```
#### Shallow Copy

浅拷贝只copy ，没有新的对象。更改 pkt1， 2 也会改变。 
```
Packet pkt, pkt2;

pkt = new;
pkt2 = new pkt;
```

#### Deep Copy
两个不想关的obj
```
Packet p1 = new;
Packet p2 = new;
p2.copy (p1);
```

### Parameterized Classes

```
// Declare parameterized class
class <name_of_class> #(<parameters>);
class Trans #(addr = 32);

// Override class parameter
<name_of_class>  #(<parameters>) <name_of_inst>;
Trans #(.addr(16)) obj;


example: 

class stack #(type T = int);
  T item;

  function T add_a (T a);
    return item + a;
  endfunction
endclass

stack 			 	st; 	// st.item is by default of int type
stack #(bit[3:0]) 	bs; 	// bs.item will become a 4-bit vector
stack #(real) 		rs; 	// rs.item will become a real number
```

#### Extern

表示这个函数是定义在class 之外的。

```
class ABC;

  // Let this function be declared here and defined later
  // by "extern" qualifier
  extern function void display();

endclass

// Outside the class body, we have the implementation of the
// function declared as "extern"
function void ABC::display();

   $display ("Hello world");

endfunction

module tb;

  // Lets simply create a class object and call the display method
  initial begin
    ABC abc = new();
    abc.display();
  end
endmodule
```

#### local
相当于private？（This is because the keyword local is used to keep members local and visible only within the same class.）

#### pure virtual function
相当于java 抽象类里面的，只定义函数命。
The pure virtual method prototype and its implementation should have the same arguments and return type.

### Randomization
废话不想说了。

#### Constraints

```
class Pkt;
	rand bit [7:0] addr;
	rand bit [7:0] data;

	constraint addr_limit { addr <= 8'hB; }
endclass
```

#### rand Variables

##### rand & randc

rand  用来修饰 class 中的变量名字，然后通过randomize 来随机化

```
class Packet;
	rand bit [2:0] data;
endclass

module tb;
	initial begin
		Packet pkt = new ();
			pkt.randomize ();
			$display ("itr=%0d data=0x%0h", i, pkt.data);
	end
endmodule
```
randc 是 rand cycle 的意思，首先遍历所有变量，等到完全遍历过以后才会有重复值。

#### Constraint Blocks
这玩意跟 class 里面task， function 什么基本上处于同等地位。
constrain 可以放在class 外面或者里面。
当放在外面时，需要用:: 来搞。

```
constraint  [name_of_constraint] {  [expression 1];[expression N]; }


//external 
class ABC;
	rand bit [3:0] mode;

	constraint c_implicit; 				// An empty constraint without "extern" is implicit
	extern constraint c_explicit; 		// An empty constraint with "extern" is explicit
endclass

// This is an external constraint because it is outside
// the class-endclass body of the class. The constraint
// is reference using ::
constraint ABC::c_implicit { mode > 2; };
constraint ABC::c_explicit { mode <= 6; };
```

constrain 也有各种写法比较有意思

#####  out side a specific range 
```
constrain inv_range {!(typ inside {[3:6]})}
```

##### weighted distributions 


```
// dist + := 直接指定权重
rand bit [2:0] typ;
constraint dist1 	{  typ dist { 0:=20, [1:5]:=50, 6:=40, 7:=10}; } //total 320
```

```
// :/ 总权重是100， 1:5 share 50%
rand bit [2:0] typ;
constraint dist2  	{  typ dist { 0:/20, [1:5]:/50, 6:/10, 7:/20}; }
```



#### Array Randomization

```
rand bit [3:0] 	s_array [7]; 	// Declare a static array with "rand"

 rand bit [3:0] 	d_array []; 	// Declare a dynamic array with "rand"
 constraint c_val   { foreach (d_array[i]) // constrains 的时候需要
    					d_array[i] == i;
                     }


 rand bit [3:0] 	queue [$]; 	// Declare a queue with "rand"

```

#### Implication Operator ->
```
 // If 5 <= mode <= 11, mod_en should be 1 
 constraint c_mode {	mode inside {[4'h5:4'hB]} -> mod_en == 1; }

 constraint c_ab { a -> b == 3'h3; }
```

#### if-else Constraint

```
	if (mode inside {[4'h5:4'hB]})
  							mod_en == 1;
```
#### foreach
```
  constraint c_array { foreach (array[i]) {
    					array[i] == i;
  						}
                     }
// 多维数组

class ABC;
  rand bit[3:0] md_array [][]; 	// Multidimansional Arrays with unknown size

  constraint c_md_array {
     // First assign the size of the first dimension of md_array
     md_array.size() == 2;

     // Then for each sub-array in the first dimension do the following:
     foreach (md_array[i]) {

        // Randomize size of the sub-array to a value within the range
        md_array[i].size() inside {[1:5]};

        // Iterate over the second dimension
        foreach (md_array[i][j]) {
           // Assign constraints for values to the second dimension
           md_array[i][j] inside {[1:10]};
         }
      }
   }
```

#### static constrain

static constrain 在不同class中间可以被共享。可以用 constraint_mode() 打开或者关闭。

non-static 被关闭的时候，只在单个instance 中生效，而static 则是在所有instance 中生效。

#### pre/post_randomize();
```
function void pre_randomize(); //字面意思
function void post_randomize();


```
#### Inline constrains
过于简单了  itm.randomize() with { id == 10; }; 

#### Soft constrains
有了soft 关键词以后，就可以被覆盖。

####  rand case 
    
    这个比较有意思。

```
module tb;
	initial begin
      for (int i = 0; i < 10; i++)
      	randcase
        	1 	: 	$display ("Wt 1");
      		5 	: 	$display ("Wt 5");
      		3 	: 	$display ("Wt 3");
      	endcase
    end
endmodule
```

 ### program block
* To provide an entry point to the execution of testbenches
* To create a container to hold all other testbench data such as tasks, class objects and functions
* Avoid race conditions with the design by getting executed during the reactive region of a simulation cycle

```
module tb;
	bit [3:0] mode;

	program p1;
		...
	endprogram

	program p2;
		...
	endprogram
endmodule
```

### Command Line Arguments

```
module tb;
  initial begin
      if ($value$plusargs ("STRING=%s", var1))
      $display ("STRING with FS has a value %s", var1);

    if ($value$plusargs ("NUMBER=%0d", data))
      $display ("NUMBER with %%0d has a value %0d", data);

    if ($value$plusargs ("NUMBER=%0h", data))
      $display ("NUMBER with %%0h has a value %0d", data);
end
endmodule
```


### Functional Coverage

coverpoint 是需要覆盖的点。
多个 coverpoint 可以 组成一个covergroup
bins are said to be "hit/covered" when the variable reaches the corresponding values. So, the bin featureB is hit when mode takes either 1,2 or 3.

开启sample 的几种方法

```
class myTrns;
	rand bit [3:0] 	mode;
	rand bit [1:0] 	key;

   function display ();
      $display ("[%0tns] mode = 0x%0h, key = 0x%0h", $time, mode, key);
   endfunction

	covergroup CovGrp;
		coverpoint mode {
			bins featureA 	= {0};
			bins featureB 	= {[1:3]};
			bins common [] 	= {4:$};
			bins reserve	= default;
		}
		coverpoint key;
	endgroup
endclass
```
```
sample() 开启sample
covergroup CovGrp @ (posedge clk); 	// Sample coverpoints at posedge clk
covergroup CovGrp @ (eventA); 			// eventA can be triggered with ->eventA;

/// iff  开启
covergroup CovGrp;
	coverpoint mode iff (!_if.reset) {
	    // bins for mode
	}
endgroup

/// start， end
CovGrp cg = new;

initial begin
	#1 _if.reset = 0;
	cg.stop ();
	#10 _if.reset = 1;
	cg.start();
end
```
### Assertion

#### sequence

```
// Sequence syntax
sequence <name_of_sequence>
  <test expression>
endsequence

// Assert the sequence
assert property (<name_of_sequence>);
```

#### property

```
// Property syntax
property <name_of_property>
  <test expression> or
  <sequence expressions>
endproperty

// Assert the property
assert property (<name_of_property>);
```

#### Immediate and Concurrent

Immediate 的情况下，就是在同一个块里面直接 assert。跟 if xx 没啥太大的区别。
assert 还可以把warning 改成 error
```
always @ (<some_event>) begin
	...
	// This is an immediate assertion executed only
	// at this point in the execution flow
	$assert(!fifo_empty);      // Assert that fifo is not empty at this point
	...
end
```


```
// Define a property to specify that an ack should be
// returned for every grant within 1:4 clocks
property p_ack;
	@(posedge clk) gnt ##[1:4] ack;
endproperty

assert property(p_ack);    // Assert the given property is true always
```

#### $rose, $fell, $stable
分别是 上升沿， 下降沿，和没有变化。就是字面意思。

### Test bench
主要分成 DUT，Transaction object, Driver, Monitor, Scoreboard, Environment, Test, Interface, Testbench Top
