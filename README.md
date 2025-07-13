# TypeScript（小满ZS教程）

## 基础包

```shell
npm install -g typescript
npm i @types/node --save-dev
npm i ts-node --g # ts-node index.ts //可直接运行ts
```

## 类型

1. top type 顶级类型 any unknown
    1. (any 可以赋值给其他或被赋值， unknown 只能被赋值)
    2. unknown 类型没有办法获取属性，方法也无法调用，通过方括号的形式在非严格模式下，可以取到值
    3. unknown 比 any 安全
2. Object 包装类型 有原型链
3. Number String Boolean
4. number string boolean
5. 1 '小满' false
6. never

```ts
let a:{} = 123 // 等于 new Object
```

## 接口和对象类型

定义接口使用 interface

1. interface 重名 进行 重合(合并)
2. interface 任意 key `[propName: string]: any`
3. interface ? 可选符 readonly 只读不可修改
4. interface 接口继承 接口可以使用 extend 进行继承（会合并属性）
5. interface 定义函数类型

```ts
interface Fn{
  (name:string):number[]
}

const fn:Fn = function () {
  return []
}
```

```ts
// 实例需要与接口定义属性一致，不能多或少
interface Axx {
  name: string
  age?: number // 可选
  [propName: string]: any
}

let a: Axx = {
  name："小满"
}

```

## 数组类型

1. number[] boolean[] string[]
2. `Array<boolean>`
3. 数组普通类型

    ```ts
    interface X {
      name: string
    }

    let arr: X[] = [{ name: '123' }]
    ```

4. 定义对象数组使用 interface
5. 二维数组 number[][] `Array<Array<number>>`
6. any[] 大杂烩
7. 类数组 arguments IArguments

    ```ts
    interface IArguments {
      callee:Function
      length: number
      [index:number]: any
    }
    ```

## 函数扩展

1. 参数不能多传，也不能少传，且需要按照约定的类型来
2. 函数的可选参数

    ```ts
    //通过?表示该参数为可选参数
    const fn = (name: string, age?:number): string => {
        return name + age
    }
    fn('张三')
    ```

3. 函数默认值

    ```ts
    const fn = (name: string = "我是默认值"): string => {
      return name
    }
    fn()
    ```

4. 接口定义函数

    ```ts
    //定义参数 num 和 num2  ：后面定义返回值的类型
    interface Add {
        (num:  number, num2: number): number
    }

    const fn: Add = (num: number, num2: number): number => {
        return num + num2
    }
    fn(5, 5)
    ```


    interface User{
        name: string;
        age: number;
    }
    function getUserInfo(user: User): User {
      return user
    }
    ```

5. 定义剩余参数

    ```ts
    const fn = (array:number[],...items:any[]):any[] => {
          console.log(array,items)
          return items
    }

    let a:number[] = [1,2,3]

    fn(a,'4','5','6')
    ```

6. 函数重载

重载是方法名字相同，而参数不同，返回类型可以相同也可以不同。

如果参数类型不同，则参数类型应设置为 any。

参数数量不同你可以将不同的参数设置为可选。

## 类型断言 | 联合类型 | 交叉类型

### 联合类型

```ts
//例如我们的手机号通常是13XXXXXXX 为数字类型 这时候产品说需要支持座机
//所以我们就可以使用联合类型支持座机字符串
let myPhone: number | string  = '010-820'
```

### 交叉类型

多种类型的集合，联合对象将具有所联合类型的所有成员

```ts
interface People {
  age: number,
  height: number
}
interface Man{
  sex: string
}
const xiaoman = (man: People & Man) => {
  console.log(man.age)
  console.log(man.height)
  console.log(man.sex)
}
xiaoman({age: 18,height: 180,sex: 'male'});
```

### 类型断言

语法：　　值 as 类型　　或　　<类型>值  value as string  <string>value

**需要注意的是，类型断言只能够「欺骗」TypeScript 编译器，无法避免运行时的错误，反而滥用类型断言可能会导致运行时错误**

#### 使用 any 临时断言

```ts
(window as any).abc = 123
```

#### as const

1. 是对字面值的断言，与const直接定义常量是有区别的
2. 如果是普通类型跟直接const 声明是一样的

```ts
// 数组
let a1 = [10, 20] as const;
const a2 = [10, 20];

a1.unshift(30); // 错误，此时已经断言字面量为[10, 20],数据无法做任何修改
a2.unshift(30); // 通过，没有修改指针
```

#### 类型断言是不具影响力的

虽然可以通过编译，但是并没有什么用 并不会影响结果, 因为编译过程中会删除类型断言

## 内置对象

### ECMAScript 的内置对象

Boolean、Number、string、RegExp、Date、Error

### DOM 和 BOM 的内置对象

Document、HTMLElement、Event、NodeList 等

### 定义Promise

如果我们不指定返回的类型TS是推断不出来返回的是什么类型
指定返回的类型
函数定义返回promise 语法规则:`Promise<T>` 类型

## Class 类

### 定义类

1. 在TypeScript是不允许直接在constructor 定义变量的 需要在constructor上面先声明
2. 如果了定义了变量不用 也会报错 通常是给个默认值 或者 进行赋值

### 类的修饰符

总共有三个 public private protected

1. 使用 public 修饰符 可以让你定义的变量 内部访问 也可以外部访问 如果不写默认就是 public
2. 使用 private 修饰符 代表定义的变量私有的只能在内部访问 不能在外部访问
3. 使用 protected 修饰符 代表定义的变量私有的只能在内部和继承的子类中访问 不能在外部访问

### static 静态属性和静态方法

1. 用static 定义的属性 不可以通过 this 去访问 只能通过类名去调用
2. static 静态函数 同样也是不能通过 this 去调用 也是通过类名去调用
3. 如果两个函数都是static 静态的是可以通过this互相调用

## interface 定义 类

1. ts interface 定义类 使用关键字 implements
2. 后面跟interface的名字多个用逗号隔开
3. 继承还是用extends

## 抽象类

### 应用

应用场景如果你写的类实例化之后毫无用处此时我可以把他定义为抽象类
或者你也可以把他作为一个基类-> 通过继承一个派生类去实现基类的一些方法

### 语法

abstract class | abstract getName(): string

**我们定义的抽象方法必须在派生类实现**

## 元组类型

如果需要一个固定大小的不同类型值的集合，我们需要使用元组。

**元组就是数组的变种**

元组（Tuple）是固定数量的不同类型的元素的组合。

元组与集合的不同之处在于：

1. 元组中的元素类型可以是不同的，而且数量固定。
2. 元组的好处在于可以把多个元素作为一个单元传递。
3. 如果一个方法需要返回多个值，可以把这多个值作为元组返回，而不需要创建额外的类来表示。
4. 元组类型还可以支持自定义名称和变为可选的
5. 对于越界的元素他的类型被限制为 联合类型（不能是元组中没有的类型）

## 枚举类型

enum 关键字定义

### 数字枚举

代表的值从 0 开始可以不写值

#### 增长枚举

需要写第一个的值（注意 step 都是 1）

### 字符串枚举

值都是字符串

### 异构枚举

枚举可以混合字符串和数字 (不能是 boolean)

### 接口枚举

定义一个枚举 Types 定义一个接口 A 他有一个属性red 值为 Types.a

声明对象的时候要遵循这个规则

```ts
enum Types {
  a,
  dddd
}
interface A {
  red:Types.a
}

let obj:A = {
  red:Types.a
}
```

### const 枚举

let  和 var 都是不允许的声明只能使用const
大多数情况下，枚举是十分有效的方案。 然而在某些情况下需求很严格。

1. 为了避免在额外生成的代码上的开销和额外的非直接的对枚举成员的访问，我们可以使用 const枚举。
2. 常量枚举通过在枚举上使用 const修饰符来定义

#### 区别

1. const 声明的枚举会被编译成常量
2. 普通声明的枚举编译完后是个对象

### 反向映射

它包含了正向映射（ name -> value）和反向映射（ value -> name）

**不会为字符串枚举成员生成反向映射**

```ts
enum Enum {
   fall
}
let a = Enum.fall
console.log(a) //0
let nameOfA = Enum[a]
console.log(nameOfA) //fall
```

## 类型推论 | 类型别名

### 类型推论

1. TypeScript 会在没有明确的指定类型的时候推测出一个类型，这就是类型推论
2. 如果你声明变量没有定义类型也没有赋值这时候TS会推断成**any类型**可以进行任何操作

### 类型别名

type 关键字（可以给一个类型定义一个名字）多用于复合类型

```ts
type str = string

let s:str = "我是小满"

console.log(s)
```

可定义的别名

1. 类型别名
2. 函数返回值别名
3. 联合类型别名
4. 值别名

#### type 和 interface 的区别

type 和 interface 还是一些区别的 虽然都可以定义类型

1.interface可以继承  type 只能通过 & 交叉类型合并

2.type 可以定义 联合类型 和 可以使用一些操作符 interface不行

3.interface 遇到重名的会合并 type 不行

##### type 高级用法

```ts
type a = 1 extends number ? 1 : 0 //1

type a = 1 extends Number ? 1 : 0 //1

type a = 1 extends Object ? 1 : 0 //1

type a = 1 extends any ? 1 : 0 //1

type a = 1 extends unknown ? 1 : 0 //1

type a = 1 extends never ? 1 : 0 //0
```

## never 类型

TypeScript 将使用 never 类型来表示不应该存在的状态

### 应用

1. 函数必定报错时
2. 存在死循环无返回时

### never 和 void 的差异

1. void 类型只是没有返回值 但本身不会出错
2. never 只会抛出异常没有返回值
3. never在联合类型中会被直接移除 `type A = void | number | never`

### 应用场景

```ts
type A = '小满' | '大满' | '超大满'

function isXiaoMan(value:A) {
   switch (value) {
       case "小满":
           break
       case "大满":
          break
       case "超大满":
          break
       default:
          //是用于场景兜底逻辑
          const error:never = value
          return error
   }
}
```

## Symbol 类型

可以传递参做为唯一标识 只支持 string 和 number类型的参数

特点：

1. Symbol 值是唯一的
2. 可用作对象属性的键
3. 使用 Symbol 定义的属性不能通过以下遍历拿到
    1. for in 遍历
    2. Object.keys
    3. getOwnPropertyNames
    4. JSON.stringify
    需要 Symbol 属性：使用 Object.getOwnPropertySymbols() 或 Reflect.ownKeys()

### Symbol.iterator 迭代器 和 生成器 for of

支持遍历大部分类型迭代器 arr nodeList argumetns set map 等

```ts
var arr = [1,2,3,4];
let iterator = arr[Symbol.iterator]();

console.log(iterator.next());  //{ value: 1, done: false }
console.log(iterator.next());  //{ value: 2, done: false }
console.log(iterator.next());  //{ value: 3, done: false }
console.log(iterator.next());  //{ value: 4, done: false }
console.log(iterator.next());  //{ value: undefined, done: true }
```

手动实现迭代器

```ts
const obj = {
    max: 5,
    current: 0,
    [Symbol.iterator]() {
        return {
            max: this.max,
            current: this.current,
            next() {
                if (this.current == this.max) {
                    return {
                        value: undefined,
                        done: true
                    }
                } else {
                    return {
                        value: this.current++,
                        done: false
                    }
                }
            }
        }
    }
}
console.log([...obj])
 
for (let val of obj) {
   console.log(val)
}
```

以下为这些symbols的列表：

Symbol.hasInstance
方法，会被instanceof运算符调用。构造器对象用来识别一个对象是否是其实例。

Symbol.isConcatSpreadable
布尔值，表示当在一个对象上调用Array.prototype.concat时，这个对象的数组元素是否可展开。

Symbol.iterator
方法，被for-of语句调用。返回对象的默认迭代器。

Symbol.match
方法，被String.prototype.match调用。正则表达式用来匹配字符串。

Symbol.replace
方法，被String.prototype.replace调用。正则表达式用来替换字符串中匹配的子串。

Symbol.search
方法，被String.prototype.search调用。正则表达式返回被匹配部分在字符串中的索引。

Symbol.species
函数值，为一个构造函数。用来创建派生对象。

Symbol.split
方法，被String.prototype.split调用。正则表达式来用分割字符串。

Symbol.toPrimitive
方法，被ToPrimitive抽象操作调用。把对象转换为相应的原始值。

Symbol.toStringTag
方法，被内置方法Object.prototype.toString调用。返回创建对象时默认的字符串描述。

Symbol.unscopables
对象，它自己拥有的属性会被with作用域排除在外。

## 泛型

```ts
function Add<T>(a: T, b: T): Array<T>  {
    return [a,b]
}
// <T> 代替需要传入的类型，类似声明
 
Add<number>(1,2)
Add<string>('1','2')
````

### 泛型约束

```ts
interface Len {
   length:number
}
 
function getLegnth<T extends Len>(arg:T) {
  return arg.length
}
 
getLegnth<string>('123')
```

### 使用 keyof 约束对象

其中使用了TS泛型和泛型约束。首先定义了T类型并使用extends关键字继承object类型的子类型，然后使用keyof操作符获取T类型的所有键，它的返回 类型是联合 类型，最后利用extends关键字约束 K类型必须为keyof T联合类型的子类型

```ts
function prop<T, K extends keyof T>(obj: T, key: K) {
   return obj[key]
}
 
let o = { a: 1, b: 2, c: 3 }
 
prop(o, 'a') 
prop(o, 'd') //此时就会报错发现找不到
```

### 泛型类

声明方法跟函数类似名称后面定义<类型>

使用的时候确定类型`new Sub<number>()`

```ts
class Sub<T>{
   attr: T[] = [];
   add (a:T):T[] {
      return [a]
   }
}
 
let s = new Sub<number>()
s.attr = [1,2,3]
s.add(123)
 
let str = new Sub<string>()
str.attr = ['1','2','3']
str.add('123')
```

## ts 配置文件（tsconfig.json）

命令`tsc --init`

```json
"compilerOptions": {
  "incremental": true, // TS编译器在第一次编译之后会生成一个存储编译信息的文件，第二次编译会在第一次的基础上进行增量编译，可以提高编译的速度
  "tsBuildInfoFile": "./buildFile", // 增量编译文件的存储位置
  "diagnostics": true, // 打印诊断信息 
  "target": "ES5", // 目标语言的版本
  "module": "CommonJS", // 生成代码的模板标准
  "outFile": "./app.js", // 将多个相互依赖的文件生成一个文件，可以用在AMD模块中，即开启时应设置"module": "AMD",
  "lib": ["DOM", "ES2015", "ScriptHost", "ES2019.Array"], // TS需要引用的库，即声明文件，es5 默认引用dom、es5、scripthost,如需要使用es的高级版本特性，通常都需要配置，如es8的数组新特性需要引入"ES2019.Array",
  "allowJS": true, // 允许编译器编译JS，JSX文件
  "checkJs": true, // 允许在JS文件中报错，通常与allowJS一起使用
  "outDir": "./dist", // 指定输出目录
  "rootDir": "./", // 指定输出文件目录(用于输出)，用于控制输出目录结构
  "declaration": true, // 生成声明文件，开启后会自动生成声明文件
  "declarationDir": "./file", // 指定生成声明文件存放目录
  "emitDeclarationOnly": true, // 只生成声明文件，而不会生成js文件
  "sourceMap": true, // 生成目标文件的sourceMap文件
  "inlineSourceMap": true, // 生成目标文件的inline SourceMap，inline SourceMap会包含在生成的js文件中
  "declarationMap": true, // 为声明文件生成sourceMap
  "typeRoots": [], // 声明文件目录，默认时node_modules/@types
  "types": [], // 加载的声明文件包
  "removeComments":true, // 删除注释 
  "noEmit": true, // 不输出文件,即编译后不会生成任何js文件
  "noEmitOnError": true, // 发送错误时不输出任何文件
  "noEmitHelpers": true, // 不生成helper函数，减小体积，需要额外安装，常配合importHelpers一起使用
  "importHelpers": true, // 通过tslib引入helper函数，文件必须是模块
  "downlevelIteration": true, // 降级遍历器实现，如果目标源是es3/5，那么遍历器会有降级的实现
  "strict": true, // 开启所有严格的类型检查
  "alwaysStrict": true, // 在代码中注入'use strict'
  "noImplicitAny": true, // 不允许隐式的any类型
  "strictNullChecks": true, // 不允许把null、undefined赋值给其他类型的变量
  "strictFunctionTypes": true, // 不允许函数参数双向协变
  "strictPropertyInitialization": true, // 类的实例属性必须初始化
  "strictBindCallApply": true, // 严格的bind/call/apply检查
  "noImplicitThis": true, // 不允许this有隐式的any类型
  "noUnusedLocals": true, // 检查只声明、未使用的局部变量(只提示不报错)
  "noUnusedParameters": true, // 检查未使用的函数参数(只提示不报错)
  "noFallthroughCasesInSwitch": true, // 防止switch语句贯穿(即如果没有break语句后面不会执行)
  "noImplicitReturns": true, //每个分支都会有返回值
  "esModuleInterop": true, // 允许export=导出，由import from 导入
  "allowUmdGlobalAccess": true, // 允许在模块中全局变量的方式访问umd模块
  "moduleResolution": "node", // 模块解析策略，ts默认用node的解析策略，即相对的方式导入
  "baseUrl": "./", // 解析非相对模块的基地址，默认是当前目录
  "paths": { // 路径映射，相对于baseUrl
    // 如使用jq时不想使用默认版本，而需要手动指定版本，可进行如下配置
    "jquery": ["node_modules/jquery/dist/jquery.min.js"]
  },
  "rootDirs": ["src","out"], // 将多个目录放在一个虚拟目录下，用于运行时，即编译后引入文件的位置可能发生变化，这也设置可以虚拟src和out在同一个目录下，不用再去改变路径也不会报错
  "listEmittedFiles": true, // 打印输出文件
  "listFiles": true// 打印编译的文件(包括引用的声明文件)
}
 
// 指定一个匹配列表（属于自动指定该路径下的所有ts相关文件）
"include": [
   "src/**/*"
],
// 指定一个排除列表（include的反向操作）
 "exclude": [
   "demo.ts"
],
// 指定哪些文件使用该配置（属于手动一个个指定文件）
 "files": [
   "demo.ts"
]
```



## 命名空间（namespace）

我们在工作中无法避免`全局变量`造成的污染，TypeScript提供了namespace 避免这个问题出现

- 内部模块，主要用于组织代码，避免命名冲突。
- 命名空间内的类默认私有
- 通过 `export` 暴露
- 通过 `namespace` 关键字定义

命名空间中通过`export`将想要暴露的部分导出

如果不用export 导出是无法读取其值的

使用类型

1. 嵌套命名空间

   ```ts
   namespace a {
       export namespace b {
           export class Vue {
               parameters: string
               constructor(parameters: string) {
                   this.parameters = parameters
               }
           }
       }
   }
    
   let v = a.b.Vue
    
   new v('1')
   ```

   

2. 抽离命名空间（命名空间单独一个文件）

3. 简化命名空间

   ```ts
   namespace A  {
       export namespace B {
           export const C = 1
       }
   }
    
   import X = A.B.C
    
   console.log(X);
   ```

4. 会自动合并命名空间（重名的命名空间会合并）

## 模块解析

1. 默认导入和导出

   ```js
   //导出
   export default {
       a:1,
   }
   //引入
   import test from "./test";
   ```

2. 分别导出

3. 重名问题 如果 导入的时候叫add但是已经有变量占用了可以用as重命名

4. 动态引入

## 声明文件（d.ts）

### 声明文件 declare

当使用第三方库时，我们需要引用它的声明文件，才能获得对应的代码补全、接口提示等功能。

第三方包声明文件npm地址   [npm](https://www.npmjs.com/~types?activeTab=packages)

导入声明文件 `/// <reference path = "runoob.d.ts" />`

## Mixins 混入

### 对象混入

可以使用es6的Object.assign 合并多个对象，会被推断为交叉类型

### 类的混入

首先声明两个`mixins`类 （严格模式要关闭不然编译不过）

```ts
class A {
    type: boolean = false;
    changeType() {
        this.type = !this.type
    }
}
 
 
class B {
    name: string = '张三';
    getName(): string {
        return this.name;
    }
}


// 首先应该注意到的是，没使用extends而是使用implements。 把类当成了接口
// 为将要mixin进来的属性方法创建出占位属性。 这告诉编译器这些成员在运行时是可用的。 这样就能使用mixin带来的便利，虽说需要提前定义一些占位属性
class C implements A,B{
    type:boolean
    changeType:()=>void;
    name: string;
    getName:()=> string
}

// 最后，创建这个帮助函数，帮我们做混入操作。 它会遍历mixins上的所有属性，并复制到目标上去，把之前的占位属性替换成真正的实现代码

// Object.getOwnPropertyNames()可以获取对象自身的属性，除去他继承来的属性，对它所有的属性遍历，它是一个数组，遍历一下它所有的属性名

Mixins(C, [A, B])
function Mixins(curCls: any, itemCls: any[]) {
    itemCls.forEach(item => {
        Object.getOwnPropertyNames(item.prototype).forEach(name => {
            curCls.prototype[name] = item.prototype[name]
        })
    })
}
```

#### 更优雅的实现（TypeScript 2.2+）

```ts
// 混入函数类型
type Constructor<T = {}> = new (...args: any[]) => T;

// 可跳转的混入
function Jumpable<TBase extends Constructor>(Base: TBase) {
  return class extends Base {
    jump() {
      console.log("Jumping!");
    }
  };
}

// 可游泳的混入
function Swimmable<TBase extends Constructor>(Base: TBase) {
  return class extends Base {
    swim() {
      console.log("Swimming!");
    }
  };
}

// 基础类
class Animal {
  constructor(public name: string) {}
}

// 应用混入
const JumpingSwimmingAnimal = Swimmable(Jumpable(Animal));
const myPet = new JumpingSwimmingAnimal("Fido");

myPet.jump(); // "Jumping!"
myPet.swim(); // "Swimming!"
```



## 装饰器（Decorator）

1. Decorator 装饰器是一项实验性特性，在未来的版本中可能会发生改变

2. 它们不仅增加了代码的可读性，清晰地表达了意图，而且提供一种方便的手段，增加或修改类的功能

3. 若要启用实验性的装饰器特性，你必须在命令行或`tsconfig.json`里启用编译器选项`experimentalDecorators: true`

### 装饰器

*装饰器*是一种特殊类型的声明，它能够被附加到[类声明](https://www.tslang.cn/docs/handbook/decorators.html#class-decorators)，[方法](https://www.tslang.cn/docs/handbook/decorators.html#method-decorators)， [访问符](https://www.tslang.cn/docs/handbook/decorators.html#accessor-decorators)，[属性](https://www.tslang.cn/docs/handbook/decorators.html#property-decorators)或[参数](https://www.tslang.cn/docs/handbook/decorators.html#parameter-decorators)上。

1. 类装饰器`ClassDecorator `  修改或增强类的行为（如添加元数据、扩展功能、替换构造函数等）
2. 方法装饰器`MethodDecorator ` 修改或增强类方法（如日志记录、权限控制、性能监控）。 `PropertyDescriptor` 属性描述符类型
3. 属性装饰器`PropertyDecorator ` 修改或增强类属性（如自动绑定、数据校验）
4. 参数装饰器`ParameterDecorator ` 修改或增强方法参数（如依赖注入、参数校验）

装饰器的执行顺序遵循以下规则：

1. **参数装饰器** -> **方法装饰器** -> **访问器装饰器** -> **属性装饰器** -> **类装饰器**（从内到外）。
2. 同一类型的多个装饰器，**从上到下执行**。

### 类装饰器

```ts
//class A {
//    constructor() {
//    }
//}

// 定义类装饰器函数 他会把ClassA的构造函数传入你的watcher函数当做第一个参数
const watcher: ClassDecorator = (target: Function) => {
    target.prototype.getParams = <T>(params: T):T => {
        return params
    }
}

// 通过@函数名使用
@watcher
class A {
    constructor() {
 
    }
}

const a = new A();
console.log((a as any).getParams('123'));
```

### 装饰器工厂

其实也就是一个`高阶函数` 外层的函数接受值 里层的函数最终接受类的构造函数

```ts
const watcher = (name: string): ClassDecorator => {
    return (target: Function) => {
        target.prototype.getParams = <T>(params: T): T => {
            return params
        }
        target.prototype.getOptions = (): string => {
            return name
        }
    }
}
 
@watcher('name')
class A {
    constructor() {
 
    }
}
 
const a = new A();
console.log((a as any).getParams('123'));
```



### 装饰器组合

可以使用多个装饰器

```ts
const watcher = (name: string): ClassDecorator => {
    return (target: Function) => {
        target.prototype.getParams = <T>(params: T): T => {
            return params
        }
        target.prototype.getOptions = (): string => {
            return name
        }
    }
}
const watcher2 = (name: string): ClassDecorator => {
    return (target: Function) => {
        target.prototype.getNames = ():string => {
            return name
        }
    }
}
 
@watcher2('name2')
@watcher('name')
class A {
    constructor() {
 
    }
}
 
 
const a = new A();
console.log((a as any).getOptions());
console.log((a as any).getNames());
```



### 方法装饰器

返回三个参数

1. 对于静态成员来说是类的构造函数，对于实例成员是类的原型对象。
2. 成员的名字。
3. 成员的*属性描述符*。

```js
const met:MethodDecorator = (...args) => {
    console.log(args);
}

// 打印结果
// [
//   {},
//   'setParams',
//   {
//     value: [Function: setParams],
//     writable: true,
//     enumerable: false,
//     configurable: true
//   }
// ]
```

### 属性装饰器

返回两个参数

1. 对于静态成员来说是类的构造函数，对于实例成员是类的原型对象。
2. 属性的名字。

```ts
const met:PropertyDecorator = (...args) => {
    console.log(args);
}
 
class A {
    @met
    name:string = '123'
    constructor() {
 
    }
   
}
 
const a = new A();// [ {}, 'name', undefined ]
```



### 参数装饰器

返回三个参数

1. 对于静态成员来说是类的构造函数，对于实例成员是类的原型对象。
2. 成员的名字。
3. 参数在函数参数列表中的索引。

```ts
const met:ParameterDecorator = (...args) => {
    console.log(args);
}

class A {
    constructor() {
 
    }
    setParasm (@met name:string = '213') {
 
    }
}

const a = new A();
```

### 元数据存储

```ts
import 'reflect-metadata'
```

可以快速存储元数据然后在用到的地方取出来 defineMetadata getMetadata

```ts
//1.类装饰器 ClassDecorator 
//2.属性装饰器 PropertyDecorator
//3.参数装饰器 ParameterDecorator
//4.方法装饰器 MethodDecorator PropertyDescriptor 'https://api.apiopen.top/api/getHaoKanVideo?page=0&size=10'
//5.装饰器工厂
import axios from 'axios'
import 'reflect-metadata'
const Base  = (base:string) => {
    const fn:ClassDecorator = (target) => {
        target.prototype.base = base;
    }
    return fn
} 
 
const Get = (url:string) => {
   const fn:MethodDecorator = (target:any,key,descriptor:PropertyDescriptor) => {
        axios.get(url).then(res=>{
            const key = Reflect.getMetadata('key',target)
            descriptor.value(key ? res.data[key] : res.data)
        })
        
   }
   return fn
}
 
const result = () => {
    const fn:ParameterDecorator = (target:any,key,index) => {
        Reflect.defineMetadata('key','result',target)
    }
    return fn
}
 
const Bt:PropertyDecorator = (target,key) => {
   console.log(target,key)
}
 
@Base('/api')
class Http {
    @Bt
    xiaoman:string
    constructor () {
        this.xiaoman = 'xiaoman'
    }
    @Get('https://api.apiopen.top/api/getHaoKanVideo?page=0&size=10')
    getList (@result() data:any) {
        console.log(data)
         
    }
    // @Post('/aaaa')
    create () {
 
    }
}
 
const http = new Http() as any
 
console.log(http)
```

