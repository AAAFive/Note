 # let 和 const 变量声明

<br >

## `var`的特性

> #### 1. 可以重复声明

```javascript
var a = 1;
var a = 2;
console.log(a);   // a => 2
```

> #### 2. 无法限制修改(不能定义为常量)

```javascript
var PI = 3.1415926;
PI = 3.14;
console.log(PI);   // PI => 3.14
```
> #### 3. 生存范围在函数级作用域内

```javascript
function  fn1(){
    if(true){
        var b = true;
    }
    console.log(b);   // b => true
}

fn1();
console.log(a);   // Error
```
<br />

## `let`的特性
> #### 1. 不能重复声明

```javascript
let a = 1;
let a = 2;   // Error
```
> #### 2. 生存范围在块级作用域内

```javascript
if( true ){
    let a = true;
}
console.log(a);   // Error
```
<br />

## `const`的特性
> #### 声明的是个常量,不能修改,具有和let相同的特性,

```javascript
const PI = 3.1415926;
console.log(a);   // a => 3.1415926
PI = 3.14;   // Error
```










<br />
<br />
<br />
<br />

---

<br />
<br />
























# 箭头函数

<br >

## 语法

> #### 函数 声明的语法

```javascript
function Name(args){
    
    return 0;
}
```

> #### 箭头函数 声明的语法

```javascript
var Name = (args)=>{

    return 0;
}
```

<br />

## 箭头函数简写
> #### 只有一个参数时,`()`可以省略

```javascript
var ForNum = a =>{
    for(let i = 1;i<=a){
        console.log(i);
    }
}
```

> #### 过程有且仅有一条`return`语句时,`{}`和`return`可以省略

```javascript

var Sum;

Sum = (a,b)=>{
    return a+b;
}

Sum = (a,b) => a+b;
```

<br />

## 实例 : $ 函数封装
```javascript
/* 标准版 */
function $(Id){
    return document.getElementById(Id);
}

/* 箭头函数 --标准版 */
var $ = (Id)=>{
    return document.getElementById(Id);
}

/* 箭头函数 --简化版 */
var $ = id => document.getElementById(id);
```










<br />
<br />
<br />
<br />

---

<br />
<br />










# 函数的参数

<br >

## 默认参数

```javascript
var show = (a=0,b=0){
    console.log(a+b);
}

show();   // a+b => 0;
show(1,2);   // a+b => 3;
```

<br >

## `...`参数收集

```
/* ... 只能方在参数列表的最后 */
var show = (a,b, ...args)=>{
    console.log(a,b);
    console.log(...args);
}

show(1,2);        // a,b => 1,2    ...args => Null
show(1,2,3,4);    // a,b => 1,2   ...args => 3,4 


/* ... 可以将 数组展开 */
var arr1 = [1,2];
var arr2 = [3,4];

var arr3 = [...arr1,...arr2];      //arr3 => [1,2,3,4]

show(...arr3);                      // a,b => 1,2   ...args => 3,4 
```










<br />
<br />
<br />
<br />

---

<br />
<br />











# 解构赋值

<br />

```javascript
let [a,b] = [1,2];
console.log(a,b);    // a => 1  b => 2

let {a,b} = {a:1,b:2};
console.log(a,b);    // a => 1  b => 1

let [{a,b},[n1,n2,n3],num,str] = [{a:1,b:2},[10,11,12],13,'14'];
console.log(a,b,n1,n2,n3,num,str);   // 1 2 10 11 12 13 '14'

let [json,arr,num,str] = [{a:1,b:2},[10,11,12],13,'14'];
console.log(json,arr,num,str);    // {a:1,b:2} [10,11,12] 13 '14'
```










<br />
<br />
<br />
<br />

---

<br />
<br />






# 数组的四个高阶方法

<br>

## `map`映射 
> #### 将数组中的元素变成想要的结果返回出来

```javascript
let arr = [1,2,3,4];

let res = arr.map( el => el*10 );

console.log(res);    // [10,20,30,40]
```

> #### 回调函数说明
>> 函数接受一个参数,代表数组中的元素,函数必须具有返回值

<br>

## `reduce`汇总
> #### 将数组中的元素整合成一个结果

```javascript
let arr = [1,2,3,4];

let res = arr.raduce( (temp,el,next)=>{
  return temp + el;
} );

console.log(res);    // res => 10
```

> #### 回调函数说明
>> 函数接受三个参数,分别是：上次计算的结果,本次参与计算的元素，和次数(可选).
>>
>> 在第一次运算的时候， 第一个参数代表的是数组中的第一个元素，第二个参数代表的是数组中的第二个元素.
>>
>> 函数必须返回一个数值,该值将参与下次一的运算.

<br>

## `filter`过滤
> #### 将数组中满足条件的的元素返回出来

```javascript
let arr = [1,2,3,4];

let res = arr.raduce( el => el>2 );

console.log(res);    // res => [3,4]
```

> #### 回调函数说明
>> 函数接受一个参数,代表数组中的元素
>>
>> 函数需要返回一个 逻辑值,过滤器会根据该逻辑值来判断,参与运算的元素是否保留

<br>

## `forEach`迭代
> #### 

```javascript
let arr = [1,2,3,4];

arr.forEach( (el,index)=>{
    console.log(index," : ",el);
} );
```

> #### 回调函数说明
>> 函数接受两个参数,第一个代表数组中的元素，第二个代表元素的下标(可选)











<br />
<br />
<br />
<br />

---

<br />
<br />







# 字符串方法
<br>

## 字符串方法

> ### `startWith`方法
>> 判断字符串是否以指定的字符串开头,返回布尔值

```javascript
var phone = '010-8888-8888';

if(phone.startWith('010')){
    alert("advertising");
}

```

> ### `endWith`方法
>> 判断字符串是否以指定的字符串结尾,返回布尔值

```javascript
var phone = 'Document.txt';

if(phone.endWith('.txt')){
    alert("Text file");
}

```

<br >

## 字符串模版
```javascript
var name = "185";
var sex = "男";
var QQ = "2721033048";

var Info = `姓名:${name},性别:${sex},QQ:${QQ}`;

console.log(Info);    // Info => 姓名:185,性别:男,QQ:2721033048
```











<br />
<br />
<br />
<br />

---
<br />
<br />






# 面向对象

<br />

## 类

> #### 使用 `class` 关键字创建类
> #### 使用 `constructor()` 方法创建构造函数
> #### 使用 `static` 关键字创建静态方法

```javascript
class User{

    // 构造函数
    constructor(name,password){
        this.name = name;
        this.password = password;
    }
    
    // 方法
    showInfo(){
        console.log(`name:${this.name} password:${this.password}`);
    }
    
    // 静态方法
    static Info(){
        console.log(`this is User class`)
    }
    
}
```
## 继承

> #### 使用 `extends` 关键字继承父类
> #### 使用 `super()` 方法继承父类属性;

```javascript
class VipUser extends User{
    
    // 扩展构造函数
    contructor(name,password,level){
        super(name,password);
        this.level = level;
    }
    
    // 添加新方法
    showLevel(){
        console.log(`level:${this.level}`);
    }
    
    // 扩展父类方法
    showInfo(){
        super.showInfo();
        console.log(`level:${this.level}`);
    }
    
    // 扩展父类的静态方法
    static Info2(){
        super.Info();
        console.log("+");
    }
}
```

> ### `supper()` 方法在子类中的特性:
>> 1. 在构造函数中 作为父类的构造函数调用 => 扩展构造函数
>> 2. 在方法中，作为父类的实例调用 => 扩展父类方法
>> 3. 在静态方法中,作为父类名调用 => 扩展父类的静态方法












<br />
<br />
<br />
<br />

---
<br />
<br />



# `JSON` 标准数据格式
### `JSON` 对象的两个方法
1. `JSON.stringify(json)` 将json数据转换成字符串
2. `JSON.parse(json)` 将字符串转换成json数据

### `JSON` 数据的标准语法
```javascript
// 键必须要用 双引号 括起来
let json = {
    "Name" : "185",
    "Age" : 18
}
```

### `JSON` 数据简写
```javascript
let Name = '185';
let Age = 18;
let json = {
    // 键值对简写
    Name,
    Age,
    
    Sex : "男",
    
    //方法简写
    echo(){
        console.log("Name:"+this.Name+" Age:"+this.Age+" Sex: "+this.Sex);
    }
}
```


<br />
<br />
<br />
<br />

---

<br />
<br />















# `Promise`















<br />
<br />
<br />
<br />

---

<br />
<br />








# `Generators`









<br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br>