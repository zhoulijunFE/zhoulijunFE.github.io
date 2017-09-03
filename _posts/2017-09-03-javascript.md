### 第一章 JavaScript简介

---

- #####  javascript组成:
   - ==ECMAScript==:  提供核心的语言功能， 版本升级x, 目前升级到6
   - ==文档对象模型(DOM)==:  提供访问和操作网页内容的方法和接口, dynamic html
   -  ==浏览器对象模型(BOM)==:  提供与浏览器交互的方法和接口
   
- ##### js宿主
   - web
   - Flash
   - node服务端
   

### 第二章 在HTML中使用JavaScript

---

- ##### html、xml、xhtml、html5 区别
  - ==html==: 超文本标记语言 (Hyper Text Markup Language),用标记描述网页， 旨在显示信息
  - ==xml==: 可扩展标记语言（EXtensible Markup Language），  旨在传输信息
  - ==xhtml==: 可扩展超文本标记语言,严格的html语法，结合 XML 和 HTML 的长处，开发出的
    - 页面的第一行添加 XHTML <!DOCTYPE>
    - 元素必须闭合
    - 所有元素名改为小写
    - 所有属性名改为小写，且值加引号 
  - ==html5==: 成为 HTML、XHTML 以及 HTML DOM 的新标准
  
- ##### html中使用javascript方式
    - ======嵌入======:
-     <script type="text/javascript" src=""></script>
    - ==外部引用==：
      - 可维护性：所有js代码写到一起
      - 可缓存性：浏览器能缓存
      - src可加载同域或者不同域都可以
-      <script type="text/javascript"></script>

- ##### 标签的位置 及 执行过程
  - js下载、解析、执行完，才开始呈现页面内容(浏览器遇到<body标签才开始呈现内容) 导致页面延迟期间浏览器空白
  - js引入放到body页面后面<script>></body>
  
- ##### 文档模式
  -   ==混杂模式==：
  -   ==标准模式==：使用页面第一句文档声明<!DOCTYPE html>实现， 没有声明默认开启混杂模式，不同浏览器css解析差异大
  
### 第三章 基本概念

---
- ##### 语法
  - ==标识符==： 指变量、函数、属性、函数参数
      - 第一个字符: 必须字母、下划线、美元符号$
      - 其他字符可以是:  字母、数字、下划线、美元符号$
      -  采用驼峰大小格式，第一个字母小写，剩下每个首字母大写
  - ==关键字==：执行特定操作
    break、do、instanceof等
  - ==保留字==：将来可能被用作关键字
    abstract、int、import等
  - ==变量==：
    - 是松散类型，可以保存任何类型数据，每个变量仅用户值的占位符 
    - 定义变量时用var 
    - 可以一句话定义多个变量, 变量逗号分割
    - 函数调用创建变量并赋值，此后销毁
-      var message = 'test',
           age = 1;

- ##### 数据类型
  - ==基本数据类型==：
    - Undefined
    - Null
    - Boolean
    - Number
    - String
  - ==复杂数据类型==
    - Object 无序名值对

- ##### typeof操作符
  检测变量的数据类型，是操作符而不是函数，圆括号可以使用但不是必须的
   - undefined: 这个var声明未赋值、变量未定义
   - boolean: 这个值布尔值
   - string: 这个值字符串
   - number: 这个值数值
   - object: 这个值对象或null(null被认为是空对象引用)
   - function: 这个值函数(函数其实是对象，不是数据类型，但为了区分)
-     typeof message; 返回undefined
   
- ##### Undefined类型
     - 只有一个值的数据类型，即特殊的undefined
     - var声明但未初始化，默认值undefined

- ##### Null类型
     - 只有一个值的数据类型, 即特殊的null
     - 表示空对象指针，所以typeof null返回object
     - 定义变量将来要保存对象， 最好变量初始化null, 便于区分null跟undefined
-      null == undefined 为true, undefined派生自null值

- ##### Boolean类型
数据类型 | 转化为true的值 | 转化为false的值
----|------|----
Boolean | true  | false
String | 任何非空字符串  | ""(空字符串)
Number | 任何非零(包含无穷大)  | 0、NaN
Object | 任何对象  | null
Undefined | 没有 | undefined

- ##### Number类型
  - 整数和浮点数值
  - NaN: 一个特殊数值，数值除以非数据js语法避免报错会返回NaN, 不影响其他代码执行
  - isNaN() 函数: 判断是否可以转化数值
    ```
    console.log(isNaN('10'))  false
    console.log(isNaN(true))  false
    console.log(isNaN(NaN))  true
    ```
- ##### 数值转换
  - ==Number()== : 可以转换任何数据类型
    - ==boolean类型==：返回 0、1
    - ==number类型==：直接返回传入
    - ==null类型==：返回0
    - ==undefined类型==：返回NaN
    - ==string类型==：
    - ==object类型==：先调用valueOf()依照前方法返回值如果NaN, 再调用toString()再依照规则
    ```    
    Number(true); //1
    Number("123"); //123 只包含数字，转化十进制
    Number("00011");  //去掉前导0
    Number("Hello"); //NaN
    Number(""); //0 空字符，转化0
    ```
  - ==parseInt()== : 专门字符串转化数据类型
    ``` 
    parseInt(""); //NaN空字符串，返回NaN
    parseInt("1234ASDF"); //1234
    ```    
    
- ##### String类型
  - ==字符串表示==： " 或 '
  - ==字符字面量==: 特殊字符也叫转义序列，非打印字符。都表示一个字符
  
    字面量 | 含义
    ---|---
    \n | 换行
    \t | 制表
    \\ | 斜杠
    \' | '
    \" | "
    ```   
    let s = "ss\nss";     
    console.log(s.length);// 5
    ```
   - ==字符串特点==: 创建后不可变的，要改变字符串先销毁，再用另一个新字符串填充
   - ==转化字符串==
     - ==toString()==: 数值、布尔值、对象、字符串都有， null undefined没有
     - ==String()==: 任意类型转化字符串
    ```   
    true.toString(); // "true"
    null.toString(); //js error
    console.log(String(null)); // "null"
    ```
- ##### Object类型
  是功能、数据集合。 通过new操作符后跟要创建的对象类型名称
  
   ```
   var o = new Object();
   ```
   object实例属性方法
     - ==constructor==: 保存用户创建当前对象的函数
     - ==hasOwnProperty(propertyName)==: 检查属性在当前对象实例中(而不是实例原型中)
     - ==isPrototypeOf(object)==: 检查传入的对象是否当前对象的原型
     - ==toString()==: 返回对象字符串表示

- 操作符
  - ==布尔操作符==
    - 逻辑非: 用!表示,任何数据类型都返回布尔值，然后取反
      - 对象类型 //false
      - 空字符串 //false
      - 数值0 //true
      - 非0数字，包含Infinity //false
      - null、undefined //true
      - NaN //true
  - ==全等和不全等==
    - 与想等不想等区别：===表示两个操作符未经转化
    ```
    "55"==55 //true
    "55"===55 //false
    null==undefined //true
    null===undedined //false
    ```
- 语句
  - ==for-in==: 迭代语句，可以遍历对象属性
    - js属性是无序的,每次遍历返回结果不一致
    - null、undefined不执行循环体
   ```
   for(var propName in window) //遍历window对象属性
   ```
- 函数
  - 使用function声明，后跟一组参数以及函数体。
  - 定义时不需要指定是否返回值，任何函数任何时候都可以通过return + 返回值
   ```
   function sayHi(name) {
       return;//return不带任何返回值，表示停止函数执行又不需要返回值情况
   }
   ```
  - 理解参数
    - 参数个数、参数数据类型无要求
    - ==参数内部用一个数组 arguments表示==，
    - 获取每一个参数：arguments[下标]访问每一个参数
    - 获取多少个参数：length属性
    - arguments对象的长度由传入的参数个数决定，非定义函数时个数决定
    - 没有传递的参数自动undefined
  - 没有重载
    - 不能实现重载，定义同名的两个函数，后定义的生效
  
### 第四章 变量、作用域、内存问题
---
- 基本类型和引用类型的值
   - 基本类型值：指的是简单数据类型
   - 引用类型值：指那些可有多个值构成的对象
     - 可以动态添加、改变、删除属性
     - 保存在内存中的对象
     - js不允许直接访问内存位置，不能直接操作对象内存空间。(c中指针操作内存) 操作对象时实际操作对象引用而不是实际对象
   ```
   var person = new Object();
   person.name = "li";
   alert(person.name)-->li
   
   var pserson = "zhang";
   person.name = 'li'; 
   alert(person.name) --->undefined, 基本类型不支持动态设置属性
   ```
-  复制变量值
   - 基本类型赋值: 新变量对象复制，任何操作互不影响
   
   - 引用类型赋值: 新变量的值实际是指针,指向存储在堆中一个对象，两个变量引用同一对象

