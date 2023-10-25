# es1234567891011
从 es1 到 es11

## 数据类型

### 值类型(基本值类型): 
String
Number
Boolean
Null 类型只有一个值。其特殊值就是Null。从逻辑角度上看，null是一个空的对象指针。而这也正是使用typeof操作符检测null值，会返回“object”的原因。
Undefined 类型只有一个值，即特殊值undefined。在使用var声明变量，但未对其加以初始化时，这个变量值就是undefined。
Symbol (es2015)  let symbol = Symbol(“aaa”);　没有构造函数，不能被new typeof 操作符，其意义是生成一个全局唯一的值,作用非常的专一，其设计出来就只有一个目的——作为对象属性的唯一标识符，防止对象属性冲突发生
BigInt(es2019 es10)

### 引用数据类型(对象类型):
Object
Array
Function

RegExp
Date

#### 对象常见操作
##### Array 
push
pop
shift
unshift
forEach
concat
sort
indexOf
reverse
splice
join


## 语句
break	用于跳出其所处那层循环，如果外层还有循环则外层继续运行。
continue	跳过循环中的一个迭代，进行下一次循环。
  continue 语句（带有或不带标签引用）只能用在循环中。
  break 语句（不带标签引用），只能用在循环或 switch 中。
  通过标签引用，break 语句可用于跳出任何 JavaScript 代码块；
  
catch	语句块，在 try 语句块执行出错时执行 catch 语句块。
do ... while	执行一个语句块，在条件语句为 true 时继续执行该语句块。
for	在条件语句为 true 时，可以将代码块执行指定的次数。
for ... in	用于遍历数组或者对象的属性（对数组或者对象的属性进行循环操作）。
function	定义一个函数
if ... else	用于基于不同的条件来执行不同的动作。
return	退出函数
switch	用于基于不同的条件来执行不同的动作。
throw	抛出（生成）错误 。
try	实现错误处理，与 catch 一同使用。
var	声明一个变量。
while	当条件语句为 true 时，执行语句块。
for of 

while (i > 100) {
  i++;
  if (i > 88) break;
}
// 判断条件前至少会执行一次循环体
do
{
    需要执行的代码
}
while (条件);

for (let i = 0; i < 100; i++) {
  // ...
}

try {
  
} catch (e) {
  throw new Error("error");
}

for (const match of matches) {
  console.log(match);
}

switch(n)
{
    case 1:
        执行代码块 1
        break;
    case 2:
        执行代码块 2
        break;
    default:
        与 case 1 和 case 2 不同时执行的代码
}

