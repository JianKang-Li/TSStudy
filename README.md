# TypeScript

## 基础包
```shell
npm install -g typescript
npm i @types/node --save-dev
npm i ts-node --g
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
函数定义返回promise 语法规则:Promise<T> 类型

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