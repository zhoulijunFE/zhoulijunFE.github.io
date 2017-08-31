## typescript 优点、缺点
- 优点:
  - JavaScript 的超集，支持所有的 JavaScript 语法
  - 强类型语言-静态类型检查
  - Classes、Interfaces、Enums、Generics
  - 限制访问范围：public、private、protected

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
     let str: string = "lili";
     let template = `name is ${str}`;
     ```
  - null
  - undefined
  - 数组([])
  - any: 任何指定类型
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
const updateAttention = (attention: IAttention) => {
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
    class Component {
        props: Readonly<{ children?: ReactNode }> & Readonly<P>;
        state: Readonly<S>;
    }
    type Readonly<T> = {
        readonly [P in keyof T]: T[P];--->只读属性, 属性前加readonly
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
- #### TODO
  -  [Relationship between a TypeScript class and an interface with the same name](https://stackoverflow.com/questions/43055682/relationship-between-a-typescript-class-and-an-interface-with-the-same-name)

## 类
- JavaScript: 使用函数和prototype
- es6: class
- ts: 使用es6 class

    ```
    class Avatar {
        //属性方法
    }
    ```
- #### 继承 extends
    ```
    class Avatar extends Component  {
       render() {
          //impl
       }
    }
    ```
- #### 构造函数
  - new创建类实例的时候被调用
      
    ```
    export default class Avatar {
       constructor(props) {
         // 默认空构造函数
         // super(props);  super调用父构造函数
       }
    }
    ```
- #### 限制访问范围
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

- #### 只读属性(readonly)
  只读属性必须在声明时或初始化

- #### 存取器 get、set--控制对象成员访问
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
  - 实例属性: 存在类对象,被实例化的时候才会被初始化的属性
  - 静态属性: 存在类本身上面而不是类的实例上
  - 访问属性: 类名. 调用
   
      ```
        class Avatar extends Component  {
           static defaultColors = [
            '#61c5f9'
           ]
        }
        访问: Avatar.defaultColors
    ```
- ####  抽象类
  - 不同于接口，抽象类可以包含实现细节
  - 抽象方法abstract必须在子类中实现
      
    ```
    abstract class Department {
        abstract printMeeting(): void;
    }
    
    class AccountingDepartment extends Department {
        printMeeting(): void {
           //impl
        }
    }
    ```
## 枚举(enum)
  - 未初始化:第一个元素默认0,其他成员的值为上一个枚举成员的值加1
  - 初始化: 常数枚举表达式(数字字面量、需要计算表达式)
    
    ```
    enum Color{
       RED = 1, //默认0
       YELLOW
    };
    ```
    
## 泛型
- 考虑可重用性。 组件不仅能够支持当前的数据类型，同时也能支持未来的数据类型
- 它是一种特殊的变量，只用于表示类型而不是值。

    ```
    class Component<P, S> {
       //parent  
    }
    
    interface AvatarProps {
        className?: string,
        id: string
    }
    default class Avatar extends Component<AvatarProps, any> {
        //any、void、{}-->shouldComponentUpdate
    }
    ```

## 函数
- #### 定义类型:参数、返回值. 
  - 没有返回任何值，void

    ```
    function add(x1: number, x2: number): number {
        return x1 + x2;
    }
    
    ```
- #### 可选参数和默认参数
    ```
    function append(str1: string='s', str2?: string) {
      // ?可选参数
      // = 默认值
    } 
    ```

- #### 多个未知参数
  - js: arguments
  - ts: 所有参数收集到一个变量里

    ```
    function append(str: string, ...strs: string[]) {
      return str + " " + strs.join(" ");
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


