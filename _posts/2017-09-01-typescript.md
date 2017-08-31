## typescript 优点、缺点
- 优点:
  - JavaScript 的超集，支持所有的 JavaScript 语法
  - 强类型语言-静态类型检查
  - Classes、Interfaces、Enums、Generics
  - 限制存在范围：public、private、protected

- 缺点:
  - 有一定的学习成本，需要理解Classes、Interfaces、Enums、Generics
  - 短期会增加一些开发成本，多写一些类型的定义
  - 集成到构建流程需要一些工作量

## typescript基础类型
  - 布尔值(boolean)
  - 数字(number)
  - 字符串(string)
    - 模板字符串: `，并且以${ expr }这种形式嵌入表达式
     ```
    let name: string = "lili";
    let template = `name is ${name}`; //js: 'name is' + name
     ```
  - null
  - undefined
  - 数组([])
    - 已知元素数量和类型的数组，各元素的类型不必相同: [string, number]
    ```
    let arry1: [string, number] = ['1', 1]
    ===>let arry1: any[]
    ```
  - 枚举(enum)
    - 未初始化:第一个元素默认0,其他成员的值为上一个枚举成员的值加1
    - 初始化: 常数枚举表达式(数字字面量、需要计算表达式)
    - 枚举是在运行时真正存在的一个对象, 编译js
    ```
    enum Color{
       RED = 1, //默认0
       YELLOW
    };
    ```
 - any: 不清楚类型的变量指定类型
 - void: 没有任何类型、函数没有返回值-->any相反
    
## 接口: 属性抽象、行为抽象, 规范类型
   不检查属性的顺序，只要相应的属性存在并且类型匹配
```
export const updateAttention = ({id, level}) => {
    //请求参数id, level
}
const attentionParams = {
    id: '1',
    level: '2'
}
updateAttention(attentionParams);

================ interface============
interface IAttention {
    id: string,
    level: string
}
export const updateAttention = (attention: IAttention) => {
    //请求参数id, level
}
const attentionParams = {
    id: '1',
    level: '2'
}
updateAttention(attentionParams);
```

 - 可选属性(?): 属性不是必需的
 - 只读属性(readonly): 创建时初始化(vs  const用于变量, readonly用于属性)
   ```
    export interface AvatarProps {
      className?: string--->可选属性, 属性名后?
    }
   ```
   ```
     class Component<P, S> {
            props: Readonly<{ children?: ReactNode }> & Readonly<P>;
            state: Readonly<S>;
    }
    type Readonly<T> = {
        readonly [P in keyof T]: T[P];--->可选属性, 属性前加readonly
    };
    ```
- #### 接口-继承接口extends
  - 一个接口可以继承多个接口: 用,分割接口组合

    ```
    interface ComponentLifecycle {
        componentDidMount?(): void;
        shouldComponentUpdate?():boolean;
    }
    
    interface Component extends ComponentLifecycle { 
             
    }
    ```
- #### 类类型-实现接口implements
  - 实现接口的类需实现接口方法

    ```
     class Component implements ComponentLifecycle {
           componentDidMount() {
              //impl
           }
           shouldComponentUpdate() {
              //impl
           }
      }
    ```
// TODO(zhoulj)
-  [Relationship between a TypeScript class and an interface with the same name](https://stackoverflow.com/questions/43055682/relationship-between-a-typescript-class-and-an-interface-with-the-same-name)

## 类
- 传统: JavaScript程序使用函数和基于原型的继承来创建可重用的组件
- es6: 基于类的面向对象的方式,基于类的继承并且对象是由类构建出来的
- ts: 使用es6特性,并且编译后的JavaScript可以在所有主流浏览器和平台上运行

    ```
    export default class Avatar {
        //constructor  super
    }
    ```
- #### 构造函数
  - new创建类实例的时候被调用
      
    ```
    export default class Avatar {
       constructor(props) {
         // 默认空构造函数
       }
    }
    ```
- #### 继承 extends
    ```
    export default class Avatar extends Component  {
       constructor(props) {
          super(props);  //super调用父构造函数
       }
       render() {
          //impl
       }
    }
    ```
- #### 公共，私有与受保护的修饰符
  - public: 默认, 可以类内、外访问
  - private: 类的内部访问
  - protected: 类的内部访问、继承类可以访问
  
    ```
    export default class Avatar extends Component  {
       private getRenderImage(src: string): any {
         //impl
       }
       public render() {
         //impl
       }
    }
    ```

- #### readonly修饰符
  只读属性必须在声明时或构造函数里被初始化

- #### 存取器 get、set--控制对对象成员的访问
  - 支持通过getters/setters来截取对对象成员的访问
  - 只带有 get不带有set的存取器自动被推断为readonly
 
    ```
    class Person { 
        constructor() {
           this.name = name;
        }
        getName() {
            return this.name;
        }
        setName(name) {
          this.name = name;
        }
    }
    var p = new Person('test');
    p.name;
    ```
- #### 静态属性
  - 类的实例成员，那些仅当类被实例化的时候才会被初始化的属性。
  -静态成员，属性存在于类本身上面而不是类的实例上
  - 要访问这个属性的时候，类名. 调用
      ```
        export default class Avatar extends Component  {
           static defaultColors = [
            '#61c5f9'
           ]
        }
        访问: Avatar.defaultColors
        ```
- ####  抽象类
  -   抽象类做为其它派生类的基类使用。 它们一般不会直接被实例化。 不同于接口，抽象类可以包含成员的实现细节。 abstract关键字是用于定义抽象类和在抽象类内部定义抽象方法
  -   抽象类中的抽象方法不包含具体实现并且必须在派生类中实现。 抽象方法的语法与接口方法相似。 两者都是定义方法签名但不包含方法体。 然而，抽象方法必须包含 abstract关键字并且可以包含访问修饰符。
  -   抽象类中的抽象方法不包含具体实现并且必须在派生类中实现。 抽象方法的语法与接口方法相似。 两者都是定义方法签名但不包含方法体。 然而，抽象方法必须包含 abstract关键字并且可以包含访问修饰符
      
    ```
    abstract class Department {
        constructor(public name: string) {
        }
    
        printName(): void {
            console.log('Department name: ' + this.name);
        }
    
        abstract printMeeting(): void; // 必须在派生类中实现
    }
    
    class AccountingDepartment extends Department {
        constructor() {
            super('Accounting and Auditing'); // constructors in derived classes must call super()
        }
    
        printMeeting(): void {
            console.log('The Accounting Department meets each Monday at 10am.');
        }
    }
    ```

## 函数
  TypeScript为JavaScript函数添加了额外的功能
- #### 为函数定义类型:参数、返回值. 
  - 没有返回任何值，void

    ```
    function add(x: number, y: number): number {
        return x + y;
    }
    
    ```
- #### 可选参数和默认参数
传递给一个函数的参数个数必须与函数期望的参数个数一致
 - javaScript里，每个参数都是可选的，可传可不传。 没传参的时候，它的值就是undefined
 - TypeScript里我们可以在参数名旁使用 ?实现可选参数的功能
 - 在TypeScript里，我们也可以为参数提供一个默认值当用户没有传递这个参数或传递的值是undefined时。 它们叫做有默认初始化值的参数
 - 与普通可选参数不同的是，带默认值的参数不需要放在必须参数的后面。 如果带默认值的参数出现在必须参数前面，用户必须明确的传入 undefined值来获得默认值
     
    ```
    function buildName(firstName: string, lastName?: string) {
      //    
    } 
    function buildName(firstName: string, lastName = "Smith") {
        // ...
    }
    ```

- #### 剩余参数
它们表示某一个参数。 有时，你想同时操作多个参数，或者你并不知道会有多少参数传递进来。 在JavaScript里，你可以使用 arguments来访问所有传入的参数. 在TypeScript里，你可以把所有参数收集到一个变量里：

```
function buildName(firstName: string, ...restOfName: string[]) {
  return firstName + " " + restOfName.join(" ");
}
```

## 泛型
- 考虑可重用性。 组件不仅能够支持当前的数据类型，同时也能支持未来的数据类型
- 它是一种特殊的变量，只用于表示类型而不是值。

    ```
    export interface AvatarProps {
        className?: string,
        id: string
    }
    export default class Avatar extends React.PureComponent<AvatarProps, any> {
        //any、void、{}-->shouldComponentUpdate
    }
    ```
## 实战完整实例

```
interface ComponentLifecycle<P, S> {
    componentWillMount?(): void;
    componentDidMount?(): void;
    componentWillReceiveProps?(nextProps: Readonly<P>, nextContext: any): void;
    shouldComponentUpdate?(nextProps: Readonly<P>, nextState: Readonly<S>, nextContext: any): boolean;
    componentWillUpdate?(nextProps: Readonly<P>, nextState: Readonly<S>, nextContext: any): void;
    componentDidUpdate?(prevProps: Readonly<P>, prevState: Readonly<S>, prevContext: any): void;
    componentWillUnmount?(): void;
}

interface ReactComponent<P = {}, S = {}> extends ComponentLifecycle<P, S> { }
class ReactComponent<P, S> { 
    constructor(props?: P, context?: any) { }
    setState<K extends keyof S>(f: (prevState: S, props: P) => Pick<S, K>, callback?: () => any): void { }
    render() { }
    props: Readonly<{ children?: any }> & Readonly<P>;
    state: Readonly<S>;
}


interface AvatarProps { 
    prefixCls: string;
}

interface AvatarStates { 
    size: number;
}

class Avatar extends ReactComponent<AvatarProps, AvatarStates>{ 
    static defaultColor = [
        '#61c5f9',
        '#a1d759'
    ];
    static defaultProps = {
        prefixCls: 'ant-avatar'
    }
    constructor(props: any) { 
        super(props);
        this.state = {
            size: 12
        }
    }
    private getRenderColor(id: any): string { 
        return Avatar.defaultColor[id % 10];
    }
    public render() { 

    }
}
```


