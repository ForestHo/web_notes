# 零基础学透TypeScript
## 1.TypeScript是是JavaScript的超集
- Babel可以将ES6转换为ES3/5,TS不需要借助babel,就可以编译成指定版本标准的代码
TS作为超集，但是我始终紧跟 ECMAScript 标准，所以 ES6/7/8/9 等新语法标准我都是支持的，而且我还在语言层面上，对一些语法进行拓展。比如新增了枚举(Enum)这种在一些语言中常见的数据类型，对类(Class)实现了一些ES6标准中没有确定的语法标准等等
强大的类型系统,避免JS的运行时错误(对象没有属性， 参数少传，返回值类型错误等)
---
- TS也有有函数重载的概念的，只不过，我的重载是为了帮助编译器知道，你给同一个函数传入不同参数，返回值是什么情况；
TSlint+VSCode
这一切特点使得 JavaScript 的所有调试都需要在运行时才能进行，在编写代码的时候很多问题是无法提前知晓的，而且就JavaScript目前的使用场景来看，它在至少很长一段时间内会保持这样的特点。 
---
那么哪些项目适合用 TypeScript 开发呢，我总结了几类：

需要多人合作开发的项目
开源项目，尤其是工具函数或组件库
对代码质量有很高要求的项目
---
ES6标准对类的相关概念的定义中，并没有私有属性的概念，如果想实现私有属性，需要使用一些方法hack（可以参考阮一峰的《ECMAScript 6 入门》- 私有方法和私有属性）；但是TypeScript是支持私有属性的，可以直接使用 private 指定一个私有属性
---
1.tslint报错 2.TS报错 3.类似JS的运行时报错
---
声明文件我们会在后面讲。我们知道原来没有 TypeScript 的时候，有很多的 JS 插件和 JS 库，如果使用 TypeScript 进行开发再使用这些 JS 编写的插件和库，就得不到类型提示等特性的支持了，所以 TypeScript 支持为 JS 库添加声明文件，以此来提供声明信息。我们使用 TypeScript 编写的库和插件编译后也是 JS 文件，所以在编译的时候可以选择生成声明文件，这样再发布，使用者就依然能得到 TypeScript 特性支持。一些 JS 库的作者已经使用 TypeScript 进行了重写，有些则是提供了声明文件，一些作者没有提供声明文件的，大部分库都有社区的人为他们补充了声明文件，如果使用了自身没有提供声明文件的库时，可以使用npm install @types/{模块名}来安装，或者运用我们后面讲到的知识自行为他们补充
---
.d.ts是什么文件，跟.ts文件的区别是什么？
.d.ts是声明文件，.ts是逻辑文件，用 ts 写的模块在发布的时候仍然是用 js 发布，这就导致一个问题：ts 那么多类型数据都没了，所以需要一个 d.ts 文件来标记某个 js 库里面对象的类型
然后 typings 就是一个网络上的 d.ts 数据库
---
很多知识你只看理论知识，或者看简单的例子，是没法真正理解并深刻记忆的，只有在实际场景中去使用一下，才能加深理解。所以我们可以从这些库的声明文件入手，还有就是从 TypeScript 内置的 lib 声明文件入手。
---
安装好 TypeScript 后，我们可以在 node_modules 文件夹下找到 typescript 文件夹，里面有个 lib 文件夹，lib 文件夹根目录下有很多以 lib. 开头的 .d.ts 文件。这些文件，就是我们在开发时如果需要用到相关内容，需要在 tsconfig.json 文件里配置引入的库的声明文件，这个配置我们后面会讲到。先简单举个例子，比如我们要使用 DOM 操作相关的语法，比如我们获取了一个 button 按钮的节点，那么我们就可以指定它的类型为 HTMLButtonElement，那么我们再访问这个节点的属性的时候，编辑器就会给你列出 button 节点拥有的所有属性方法了；如果我们要用到这个类型接口，那我们就需要引入 lib.dom.d.ts 也就是dom这个 lib。这里如果你对一些提到的概念不明白，你可以先忽略，因为后面都会讲到。这里我要告诉你的就是，你应该学着看这些声明文件，看看它们对于一些内容的声明是如何定义的，能够帮你见识到各种语法的运用。
---
问题解决
1.google 2.看issue 3.去论坛或者交流群提问
---
### 1.2.5 看优秀项目源码
```
src：用来存放项目的开发资源，在 src 下创建如下文件夹：
utils：和业务相关的可复用方法
tools：和业务无关的纯工具函数
assets：图片字体等静态资源
api：可复用的接口请求方法
config：配置文件
typings：模块声明文件
build：webpack 构建配置
```
npm install typescript -g
tsc --init
而且你可能会奇怪，tsconfig.json文件里怎么可以使用//和/**/注释，这个是 TS 在 1.8 版本支持的，我们后面课程讲重要更新的时候会讲到。
---
tsconfig.json 里默认有 4 项没有注释的配置，有一个需要提前讲下，就是"lib"这个配置项，他是一个数组，他用来配置需要引入的声明库文件，我们后面会用到ES6语法，和DOM相关内容，所以我们需要引入两个声明库文件，需要在这个数组中添加"es6"和"dom"，也就是修改数组为[“dom”, “es6”]，其他暂时不用修改，接着往下进行。
---
然后我们还需要在项目里安装一下typescript，因为我们要搭配使用webpack进行编译和本地开发，不是使用tsc指令，所以要在项目安装一下：

npm install typescript
---
### 配置TSLint
运行结束之后，你会发现项目根目录下多了一个tslint.json文件，这个就是TSLint的配置文件了，它会根据这个文件对我们的代码进行检查，生成的tslint.json文件有下面几个字段：

{
  "defaultSeverity": "error",
  "extends": [
    "tslint:recommended"
  ],
  "jsRules": {},
  "rules": {},
  "rulesDirectory": []
}
defaultSeverity是提醒级别，如果为error则会报错，如果为warning则会警告，如果设为off则关闭，那TSLint就关闭了；
extends可指定继承指定的预设配置规则；
jsRules用来配置对.js和.jsx文件的校验，配置规则的方法和下面的rules一样；
rules是重点了，我们要让TSLint根据怎样的规则来检查代码，都是在这个里面配置，比如当我们不允许代码中使用eval方法时，就要在这里配置"no-eval": true；
rulesDirectory可以指定规则配置文件，这里指定相对路径。
---
### 配置webpack
接下来我们要搭配使用 webpack 进行项目的开发和打包，先来安装 webpack、webpack-cli 和 webpack-dev-server：

npm install webpack webpack-cli webpack-dev-server -D
---
这里我们用到了两个webpack插件，第一个clean-webpack-plugin插件用于删除某个文件夹，我们编译项目的时候需要重新清掉上次打包生成的dist文件夹，然后进行重新编译，所以需要用到这个插件将上次打包的dist文件夹清掉。
第二个html-webpack-plugin插件用于指定编译的模板，这里我们指定模板为"./src/template/index.html"文件，打包时会根据此html文件生成页面入口文件。
---
# 基础部分
## 八个JS中你见过的类型
为一个变量指定类型的语法是使用"变量:类型"的形式，如下：
```
let num:number = 123
```
---
如果你没有为这个变量指定类型，编译器会自动根据你赋给这个变量的值来推断这个变量的类型：

let num = 123
num = 'abc' // error 不能将类型“"123"”分配给类型“number”
---
当我们给num赋值为123但没有指定类型时，编译器推断出了num的类型为number数值类型，所以当给num再赋值为字符串"abc"时，就会报错。
```
TS微信收藏教程
https://mp.weixin.qq.com/mp/appmsgalbum?__biz=MzA3MjU5NjU2NA==&action=getalbum&album_id=1415067738527301632&scene=173&from_msgid=2455503753&from_itemidx=1&count=3#wechat_redirect
```
这里还有一点要注意，就是number和Number的区别：TS中指定类型的时候要用number，这个是TypeScript的类型关键字。而Number为JavaScript的原生构造函数，用它来创建数值类型的值，它俩是不一样的。包括你后面见到的string、boolean等都是TypeScript的类型关键字，不是JavaScript语法，这点要区分开。

##### 1.布尔类型(布尔值或者布尔值的表达式)
类型为**布尔类型的变量的值**只能是 true 或 false
let bool: boolean = false;
bool = true;
bool = 123; // error 不能将类型"123"分配给类型"boolean"
当然了，赋给 bool 的值也可以是一个计算之后结果是**布尔值的表达式**，比如：

let bool: boolean = !!0
console.log(bool) // false 
#### 2.数值类型
TypeScript 和 JavaScript 一样，所有数字都是**浮点数**，所以只有一个number类型，而**没有int或者float类型**。而且 TypeScript 还支持 ES6 中新增的二进制和八进制数字字面量，所以 TypeScript 中共支持**二、八、十和十六四种进制的数值**。
```
let num: number;
num = 123;
num = "123"; // error 不能将类型"123"分配给类型"number"
num = 0b1111011; //  二进制的123
num = 0o173; // 八进制的123
num = 0x7b; // 十六进制的123
```
#### 3.字符串类型
字符串类型中你可以使用**单引号和双引号**包裹内容，但是可能你使用的 tslint 规则会对引号进行检测，使用单引号还是双引号可以在 tslint 规则里配置。你还可以使用 ES6 语法——模板字符串，拼接变量和字符串更为方便。
```
let str:string = "Joseph Ho";
str = "Li";
const first = "Lison";
const last = "Li";
str = `${first} ${last}`;
console.log(str) //打印结果为:Lison Li
```
另外还有个和字符串相关的类型：**字符串字面量类型**。即把一个字符串字面量作为一种类型，比如上面的字符串"Lison"，当你把一个变量指定为这个字符串类型的时候，就不能再赋值为其他字符串值了，如：
```
let str:'Lison'
str = 'haha' //error 不能将类型"haha"分配给类型"Lison"
```
#### 4.数组
在TypeScript中有两种定义数组的方式:
```
let list1:number[] = [1,2,3]
let list2:Array<number> = [1,2,3]
```
---
第一种形式通过number[\]的形式来指定这个类型元素均为number类型的数组类型，这种写法是推荐的写法，当然你也可以使用第二种写法。注意，这两种写法中的**number指定的是数组元素的类型**，你也可以在这里将数组的元素指定为任意类型。如果你要指定一个数组里的元素既可以是**数值也可以是字符串**，那么你可以使用这种方式：number|string[]，这种方式我们在后面学习联合类型的时候会讲到。

当你使用第二种形式定义时，tslint 可能会警告让你使用第一种形式定义，如果你就是想用第二种形式，可以通过在 tslint.json 的 rules 中加入"array-type": [false]关闭 tslint 对这条的检测。

后面我们讲**接口**的时候，还会讲到数组的一个特殊类型：**ReadonlyArray，即只读数组**。
---
#### 5.null和undefined
null 和 undefined 有一些共同特点，所以我们放在一起讲。说它们有共同特点，是因为**在 JavaScript 中，undefined 和 null 是两个基本数据类型**。在 TypeScript 中，这两者都有各自的类型即 undefined 和 null，也就是说**它们既是实际的值，也是类型**，来看实际例子：
```
let u:undefined = undefined; 
let n:null = null
// 这里可能会报一个tslint的错误：Unnecessary initialization to 'undefined'，就是不能给一个值赋undefined，但我们知道这是可以的，所以如果你的代码规范想让这种代码合理化，可以配置tslint，将"no-unnecessary-initializer"设为false即可
```
**默认情况下 undefined 和 null 可以赋值给任意类型的值**，也就是说你可以把 undefined 赋值给 void 类型，也可以赋值给 number 类型。当你在 tsconfig.json 的"compilerOptions"里设置了"strictNullChecks": true时，那必须严格对待。undefined 和 null 将只能赋值给它们自身和 void 类型，void类型我们后面会学习。
---
#### 6.object
object 在 JS 中是引用类型，它和 JS 中的其他基本类型不一样，**像 number、string、boolean、undefined、null 这些都是基本类型，这些类型的变量存的是他们的值**，而 **object 类型的变量存的是引用**，看个简单的例子：
```
let strInit = "abc";
let strClone = strInit;
strClone = "efg";
console.log(strInit); // 'abc'

let objInit = { a: "aa" };
let objClone = objInit;
console.log(objClone) // {a:"aa"}
objInit.a = "bb";
console.log(objClone); // { a: 'bb' }
```
通过例子可以看出，我们修改 objInit 时，objClone 也被修改了，是因为 objClone 保存的是 objInit 的引用，实际上 objInit 和 objClone 是同一个对象。
---
当我们希望一个变量或者函数的参数的类型是一个对象的时候，使用这个类型，比如:
```
let obj:object
obj = {name:'bison'}
obj = 123 //error 不能将类型“123”分配给类型“object”
```
这里有一点要注意了，你可能会想到给 obj 指定类型为 object 对象类型，然后给它赋值一个对象，后面通过属性访问操作符访问这个对象的某个属性，实际操作一下你就会发现会报错
```
let obj: object
obj = { name: 'Lison' }
console.log(obj.name) // error 类型“object”上不存在属性“name”
```
这里报错说类型 object 上没有 name 这个属性。如果你想要达到这种需求你应该使用我们后面章节要讲到的接口，那 object 类型适合什么时候使用呢？我们前面说了，当你希望一个值必须是对象而不是数值等类型时，比如我们定义一个函数，参数必须是对象，这个时候就用到object类型了：
```
function getKeys (obj: object) {
    return Object.keys(obj) // 会以列表的形式返回obj中的值
}
getKeys({ a: 'a' }) // ['a']
getKeys(123) // error 类型“123”的参数不能赋给类型“object”的参数
```
#### 6.symbol
Symbol 是 ES6 加入的新的基础数据类型，因为它的知识比较多，所以我们单独在后面的一节进行讲解。
##### 本节小结
本小节我们学习了八个在JavaScript中我们就见过的**数据类型**，它们是：
**布尔类型、数值类型、字符串、数组、null、undefined、object**以及ES6中新增的**symbol**。在TypeScript中它们都有对应的类型关键字，对应关系为：
```
布尔类型：boolean
数值类型：number
字符串类型：string
数组：Array<type>或type[]
对象类型：object
Symbol类型：symbol
null和undefined：null 和 undefined，这个比较特殊，它们自身即是类型
```
## TS中补充的六个类型
上个小节我们学习了八个JavaScript中常见的数据类型，你也学会了如何给一个变量指定类型。
本小节我们将接触几个TypeScript中引入的新类型，这里面可能有你在其他**强类型语言**中见过的概念，
接下来让我们一起来学习
#### 1.元组
元组可以看做是**数组的拓展**，它表示**已知元素数量和类型的数组**。确切地说，是
已知数组中每一个位置上的元素的类型，来看例子:
```
let tuple: [string, number, boolean];
tuple = ["a", 2, false];
tuple = [2, "a", false]; // error 不能将类型“number”分配给类型“string”。 不能将类型“string”分配给类型“number”。
tuple = ["a", 2]; // error Property '2' is missing in type '[string, number]' but required in type '[string, number, boolean]'
```
可以看到，上面我们定义了一个元组 tuple，它包含三个元素，且每个元素的类型是固定的。
当我们为 tuple 赋值时：**各个位置上的元素类型都要对应，元素个数也要一致。**
我们还可以给单个元素赋值：
```
tuple[1] = 3;
//这里我们给元组 tuple 的索引为 1 即第二个元素赋值为 3，第二个元素类型为 number，我们赋值给 3，所以没有问题。
```
当我们**访问元组中元素时**，TypeScript 会对我们**在元素上做的操作进行检查**：
```
tuple[0].split(":"); // right 类型"string"拥有属性"split"
tuple[1].split(":"); // error 类型“number”上不存在属性“split”
```
在 2.6 版本之前，TypeScript ***对于元组长度的校验***和 2.6 之后的版本有所不同，
我们来看下面的例子，前后版本对于该情况的处理：
```
let tuple: [string, number];
tuple = ["a", 2]; // right 类型和个数都对应，没问题
// 2.6版本之前如下也不会报错
tuple = ["a", 2, "b"];
// 2.6版本之后如下会报错
tuple = ["a", 2, "b"]; // error 不能将类型“[string, number, string]”分配给类型“[string, number]”。 属性“length”的类型不兼容。
```
这个赋给元组的值有三个元素，是比我们定义的元组类型元素个数多的：
+ 在 2.6 及之前版本中，超出规定个数的元素称作**越界元素**，
但是只要越界元素的类型是定义的类型中的一种即可。
比如我们定义的类型有两种：string 和 number，越界的元素是 string 类型，
属于联合类型 string | number，所以没问题，联合类型的概念我们后面会讲到。
- 在 2.6 之后的版本，去掉了这个**越界元素是联合类型的子类型即可**的条件，
要求元组赋值必须类型和个数都对应。
---
在 2.6 之后的版本，[string, number]元组类型的声明效果上可以看做等同于下面的声明：
```
interface Tuple extends Array<number | string>{
    0:string;
    1:number;
    length:2;
}
```
上面这个声明中，我们定义接口Tuple，它继承数组类型，并且数组元素的类型是 number 和 string 构成的联合类型，这样接口Tuple 就拥有了数组类型所有的特性。并且我们明确指定索引为0的值为string类型，索引为1的值为number类型，同时我们指定 length 属性的类型字面量为 2，这样当我们再指定一个类型为这个接口Tuple的时候，这个值必须是数组，而且如果元素个数超过2个时，它的length就不是2是大于2的数了，就不满足这个接口定义了，所以就会报错；当然，如果元素个数不够2个也会报错，因为索引为0或1的值缺失。接口我们后面会在后面专门的一节来讲，所以暂时不懂也没关系。
---
如果你想要和 2.6 及之前版本一样的元组特性，那你可以这样定义接口：
```
interface Tuple extends Array<number | string> {
  0: string;
  1: number;
}
```
也就是去掉接口中定义的length: 2，这样Tuple接口的length就是从Array继承过来的number类型，而不用必须是2了。
---
#### 2.枚举
enum类型在C++这些语言中比较常见，，TypeScript 在 ES 原有类型基础上加入枚举类型，
使我们在 TypeScript 中也可以给一组数值赋予名字，这样对开发者来说较为友好。
比如我们要定义一组角色，每一个角色用一个数字代表，就可以使用枚举类型来定义：
```
enum Roles {
    SUPER_ADMIN,
    ADMIN,
    USER
}
```
上面定义的枚举类型 Roles 里面有三个值，TypeScript 会为它们每个值分配编号，
默认从 0 开始，依次排列，所以它们对应的值是：
```
enum Roles {
  SUPER_ADMIN = 0,
  ADMIN = 1,
  USER = 2
}
```
当我们使用的时候，就可以使用名字而不需要记数字和名称的对照关系了：
```
const superAdmin = Roles.SUPER_ADMIN;
console.log(superAdmin); // 0
```
你也可以修改这个数值，比如你想让这个编码从 1 开始而不是 0，可以如下定义：
```
enum Roles {
  SUPER_ADMIN = 1,
  ADMIN,
  USER
}
```
这样当你访问Roles.ADMIN时，它的值就是 2 了。
你也可以为每个值都赋予不同的、不按顺序排列的值：
```
enum Roles {
  SUPER_ADMIN = 1,
  ADMIN = 3,
  USER = 7
}
```
通过名字 Roles.SUPER_ADMIN 可以获取到它对应的值 1，
同时你也可以通过值获取到它的名字，以上面任意数值这个例子为前提：
```
console.log(Roles[3]); // 'ADMIN'
```
#### 3.Any
JavaScript 的类型是灵活的，程序有时也是多变的。
有时，我们在编写代码的时候，并不能清楚地知道一个值到底是什么类型，
这时就需要**用到 any 类型，即任意类型**。我们来看例子：
```
let value:any;
value = 123;
value = "abc";
value = false;
```
你可以看到，我们定义变量value,指定它的类型为any，接下来赋予任何类型的值都是可以的。
我们还可以在定义数组类型时使用any来指定数组中的元素类型为任意类型：
```
const array:any[] = [1,"a",true]
```
**但是请注意，不要滥用 any，如果任何值都指定为 any 类型，那么 TypeScript 将失去它的意义。**
#### 4.void
void和any相反，any是表示任意类型，而**void是表示没有任意类型，就是什么类型都不是**，
这在我们定义函数，函数没有返回值时会用到：
```
const concoleText = (text:string):void =>{
    console.log(text)
}
```
这个函数没有返回任何的值，所以它的返回类型为 void。
现在你只需知道 void 表达的含义即可，后面我们会用专门的一节来学习函数。
**void 类型的变量只能赋值为 undefined 和 null，其他类型不能赋值给 void 类型的变量。**
#### 5.never
**never 类型指那些永不存在的值的类型**，它是那些**总会抛出异常或根本不会有返回值的函数表达式的返回值类型**，
当变量被永不为真的类型保护（后面章节会详细介绍）所约束时，该变量也是 never 类型。
这个类型比较难理解，我们先来看几个例子：
```
const errorFunc = (message: string): never => {
  throw new Error(message);
};
```
这个 errorFunc **函数总是会抛出异常**，所以它的返回值类型是 never，
用来表明它的返回值是永不存在的。
```
const infiniteFunc = (): never => {
  while (true) {}
};
```
+ **infiniteFunc也是根本不会有返回值的函数**，它和之前讲 void 类型时的consoleText函数不同，
consoleText函数没有返回值，是我们在定义函数的时候没有给它返回值，
而infiniteFunc是死循环是根本不会返回值的，所以它们二者还是有区别的。
- never 类型是任何类型的子类型，所以它可以赋值给任何类型；而没有类型是 never 的子类型，
所以除了它自身没有任何类型可以赋值给 never 类型，any 类型也不能赋值给 never 类型。
我们来看例子：
```
let neverVariable = (() => {
  while (true) {}
})();
neverVariable = 123; // error 不能将类型"number"分配给类型"never"
```
+ 上面例子我们定义了一个立即执行函数，也就是"let neverVariable = "右边的内容。
右边的函数体内是一个死循环，所以这个函数调用后的返回值类型为 never，所以赋值之后
 neverVariable 的类型是 never 类型，当我们给 neverVariable 赋值 123 时，
 就会报错，因为除它自身外任何类型都不能赋值给 never 类型。
#### 6.unknown
**unknown类型是TypeScript在3.0版本新增的类型，它表示未知的类型**，
这样看来它貌似和any很像，但是还是有区别的，也就是所谓的**unknown相对于any是安全的**。
怎么理解呢？我们知道当一个值我们不能确定它的类型的时候，可以指定它是any类型；
但是当指定了any类型之后，这个值基本上是“废”了，你可以**随意对它进行属性方法的访问**，
不管有的还是没有的，可以把它当做任意类型的值来使用，这往往会产生问题，如下：
```
let value:any
console.log(value.name)
console.log(value.toFixed())
console.log(value.length)
```
上面这些语句都不会报错，因为value是any类型，所以后面三个操作都有合法的情况，
当value是一个对象时，访问name属性是没问题的；当value是数值类型的时候，
调用它的toFixed方法没问题；当value是字符串或数组时获取它的length属性是没问题的。
---
而当你指定值为unknown类型的时候，**如果没有通过基于控制流的类型断言来缩小范围的话**，
是**不能对它进行任何操作的**，关于**类型断言**，我们后面小节会讲到。总之这里你知道了，
**unknown类型的值不是可以随便操作的**。

+ 我们这里只是先来了解unknown和any的区别，**unknown还有很多复杂的规则**，
但是涉及到很多后面才学到的知识，所以需要我们学习了高级类型之后才能再讲解。
#### 7.拓展阅读
这要讲的不是TypeScript中新增的基本类型，而是高级类型中的两个比较常用类型：
**联合类型**和**交叉类型**。我们之所以要提前讲解，是因为它俩比较简单，而且很是常用，
所以我们先来学习下。
1. 交叉类型(与，交集)
**交叉类型就是取多个类型的并集**，使用 & 符号定义，**被&符链接的多个类型构成一个交叉类型**，
表示这个类型同时具备这几个连接起来的类型的特点，来看例子：
```
const merge = <T, U>(arg1: T, arg2: U): T & U => {
  let res = <T & U>{}; // 这里指定返回值的类型兼备T和U两个类型变量代表的类型的特点
  res = Object.assign(arg1, arg2); // 这里使用Object.assign方法，返回一个合并后的对象；
                                   // 关于该方法，请在例子下面补充中学习
  return res;
};
const info1 = {
  name: "lison"
};
const info2 = {
  age: 18
};
const lisonInfo = merge(info1, info2);

console.log(lisonInfo.address); // error 类型“{ name: string; } & { age: number; }”上不存在属性“address”
```
补充阅读：Object.assign方法可以合并多个对象，将多个对象的属性添加到一个对象中并返回，
有一点要注意的是，如果属性值是对象或者数组这种保存的是内存引用的引用类型，会保持这个引用，
也就是如果在Object.assign返回的的对象中修改某个对象属性值，原来用来合并的对象也会受到影响。
- 可以看到，传入的两个参数分别是带有属性 name 和 age 的两个对象，所以它俩的交叉类型要求返回的对象既有 name 属性又有 age 属性。
2. 联合类型(或，并集)
联合类型在前面课时中几次提到，现在我们来看一下。联合类型实际是几个类型的结合，
但是和交叉类型不同，**联合类型是要求只要符合联合类型中任意一种类型**即可，
它使用 | 符号定义。当我们的程序具有多样性，元素类型不唯一时，即使用联合类型。
```
const getLength = (content: string | number): number => {
  if (typeof content === "string") return content.length;
  else return content.toString().length;
};
console.log(getLength("abc")); // 3
console.log(getLength(123)); // 3
```
+ 这里我们指定参数**既可以是字符串类型也可以是数值类型**，这个getLength函数的定义中，
其实还涉及到一个知识点，就是**类型保护**，就是typeof content === “string”，后面进阶部分我们会学到。
#### 补充说明 
有一个问题我需要在这里提前声明一下，以免你在自己联系专栏中例子的时候遇到困惑。
在讲解语法知识的时候，会有很多例子，在定义一些类型值，比如枚举，或者后面讲的接口等的时候，
对于他们的命名我并不会考虑重复性，比如我这里讲枚举的定义定义了一个名字叫Status的枚举值，
在别处我又定义了一个同名的接口，那这个时候你可能会看到如下这种错误提示：
***枚举声明只能与命名空间或其他枚举声明合并***
+ 正如你看到的，这里这个错误，是因为你在同一个文件不同地方、或者不同文件中，定义了相同名称的值，
而由于TypeScript的声明合并策略，他会将同名的一些可合并的声明进行合并，当同名的两个值或类型不能合并的时候，
就会报错；或者可以合并的连个同名的值不符合要求，也会有问题。关于声明合并和哪些声明可以合并，
以及声明需要符合的条件等我们会在后面章节学到。这里你只要知道，类似于这种报错中提到“声明合并”的
或者无法重新声明块范围变量，可能都是**因为有相同名称的定义**。
### 本章小结
本小节我们学习了六个TypeScript中新增的数据类型，
它们是：**元组、枚举、Any、void、never和unknown**，其中枚举我们会在后面一个单独的小节
进行详细学习，unknown会在我们学习了高级类型之后再补充。我们还学习了两个简单的
高级类型：**联合类型和交叉类型**。我们还学习了any类型与never类型和unknown类型相比的区别，
简单来说，any和never的概念是对立的，而any和unknown类型相似，但是unknown与any相比是
较为安全的类型，它并不允许无条件地随意操作。我们学习的联合类型和交叉类型，
是各种类型的结合，我们可以使用几乎任何类型，来组成联合类型和交叉类型。

下个小节我们将详细学习Symbol的所有知识，Symbol是ES6标准提出的新概念，TypeScript已经支持了该语法，下节课我们将进行全面学习。
# Symbol-ES6新基础类型
symbol是ES6新增的一种基本数据类型，它和number、string、boolean、undefined和null是同
类型的，object是引用类型。**它用来表示独一无二的值，通过Symbol函数生成。**
```
const s = Symbol();
typeof s; // 'symbol'
```
我们使用Symbol函数生成了一个 symbol 类型的值 s。
**注意：Symbol 前面不能加new关键字，直接调用即可创建一个独一无二的 symbol 类型的值。**
我们可以在使用 Symbol 方法创建 symbol 类型值的时候**传入一个参数，这个参数需要是字符串**的。
如果传入的参数不是字符串，会先调用传入参数的 toString 方法转为字符串。先来看例子：
```
const s1 = Symbol("lison);
const s2 = Symbol("lison");
console.log(s1 === s2);//false
// 补充：这里第三行代码可能会报一个错误：This condition will always return 
//'false' since the types 'unique symbol' and 'unique symbol' have no overlap.
// 这是因为编译器检测到这里的s1 === s2始终是false，所以编译器提醒你这代码写的多余，建议你优化。
```
+ 上面这个例子中使用 Symbol 方法创建了两个 symbol 值，方法中都传入了相同的字符串’lison’，
但是s1 === s2却是 false，这就是我们说的，**Symbol 方法会返回一个独一无二的值**，
这个值和任何一个值都不等，虽然我们传入的标识字符串都是"lison"，但是确实两个不同的值。
- **你可以理解为我们每一个人都是独一无二的，虽然可以有相同的名字，但是名字只是用来方便我们**
**区分的，名字相同但是人还是不同的**。Symbol 方法传入的这个字符串，就是方便我们在控制台或程序中
用来区分 symbol 值的。我们可以调用 symbol 值的toString方法将它转为字符串：
```
const s1 = Symbol("lison");
console.log(s1.toString()); // 'Symbol(lison)'
```
* 你可以简单地理解 symbol 值为字符串类型的值，但是它和字符串有很大的区别，
**它不可以和其他类型的值进行运算**，但是**可以转为字符串和布尔类型值**：
```
let s = Symbol("lison");
console.log(s.toString()); // 'Symbol(lison)'
console.log(Boolean(s)); // true
console.log(!s); // false
```
+ 通过上面的例子可以看出，symbol 类型值和对象相似，本身转为布尔值为 true，取反为 false。
### 1.作为属性名
* **在ES6中，对象的属性名支持表达式，所以你可以使用一个变量作为属性名**，这对于代码的简化很有用处，
但是**表达式必须放到方括号内**:
```
let prop = "name";
const obj = {
    [prop]:"Lison"
};
console.log(obj.name);//"Lison"
```
+ 了解到这个新特性后，我们接着学习。**symbol值可以作为属性名，因为symbol值是独一无二的**，
所以当它作为属性名时，不会和其他任何属性名重复:
```
let name = Symbol();
let obj = {
  [name]: "lison"
};
console.log(obj); // { Symbol(): 'lison' }
```
- 你可以看到，打印出来的对象有一个属性名是 symbol 值。如果我们想访问这个属性值，
就只能使用 name 这个 symbol 值：
```
console.log(obj[name]); // 'lison'
console.log(obj.name); // undefined
```
+ 通过上面的例子可以看到，我们访问属性名为 symbol 类型值的 name 时，我们不能使用点’.‘号访问，
因为obj.name这的name实际上是字符串’name’，这和访问普通字符串类型的属性名一样。
你必须使用方括号的形式，这样obj[name]这的 name 才是我们定义的 symbol 类型的变量name，
之后我们再访问 obj 的[name]属性就必须使用变量 name。
**等我们后面学到 ES6 的类(Class)的时候，会利用此特性实现私有属性和私有方法。**
### 2.属性名的遍历
- 使用 Symbol 类型值作为属性名，这个属性不会被for…in遍历到，也不会被Object.keys()、
Object.getOwnPropertyNames()、JSON.stringify()获取到：
```
const name = Symbol("name");
const obj = {
  [name]: "lison",
  age: 18
};
for (const key in obj) {
  console.log(key);
}
// => 'age'
console.log(Object.keys(obj));
// ['age']
console.log(Object.getOwnPropertyNames(obj));
// ['age']
console.log(JSON.stringify(obj));
// '{ "age": 18 }'
```
+ 虽然这么多方法都无法遍历和访问到Symbol类型的属性名，但是Symbol类型的属性并不是私有属性。
我们可以使用Object.getOwnPropertySymbols方法获取对象的所有symbol类型的属性名:
```
const name = Symbol("name");
const obj = {
  [name]: "lison",
  age: 18
};
const SymbolPropNames = Object.getOwnPropertySymbols(obj);
console.log(SymbolPropNames);
// [ Symbol(name) ]
console.log(obj[SymbolPropNames[0]]);
// 'lison'
// 如果最后一行代码这里报错提示：元素隐式具有 "any" 类型，
因为类型“{ [name]: string; age: number; }”没有索引签名。 
那可能是在tsconfig.json里开启了noImplicitAny。因为这里我们还没有学习接口等高级类型，
所以你可以先忽略这个错误，或者关闭noImplicitAny。
```
+ 除了Object.getOwnPropertySymbols这个方法，还可以用 ES6 新提供的 Reflect 对象
的静态方法Reflect.ownKeys,它可以返回所有类型的属性名，所以 Symbol 类型的也会返回。
```
const name = Symbol("name");
const obj = {
  [name]: "lison",
  age: 18
};
console.log(Reflect.ownKeys(obj));
// [ 'age', Symbol(name) ]
```
### 3.Symbol.for()和Symbol.keyFor(方法)
- Symbol包含两个静态方法，for和keyFor
>>> Symbol.for()
- Symbol.for(key) 方法会根据给定的键 key，来从运行时的 symbol 注册表中找到对应的 symbol，
如果找到了，则返回它，否则，新建一个与该键关联的 symbol，并放入全局 symbol 注册表中。
- 我们使用Symbol方法创建的symbol值是独一无二的，每一个值都不和其他任何值相等，我们来看下
例子:
```
const s1 = Symbol("lison");
const s2 = Symbol("lison");
const s3 = Symbol.for("lison");
const s4 = Symbol.for("lison");
s3 === s4; // true
s1 === s3; // false
// 这里还是会报错误：This condition will always return 'false' 
since the types 'unique symbol' and 'unique symbol' have no overlap.
还是我们说过的，因为这里的表达式始终是true和false，所以编译器会提示我们。
```
+ 直接使用 Symbol 方法，即便传入的字符串是一样的，创建的 symbol 值也是互不相等的。
而使用 Symbol.for方法传入字符串，会先检查有没有使用该字符串调用 Symbol.for 方法创建的
 symbol 值，如果有，返回该值，如果没有，则使用该字符串新创建一个。
使用**该方法创建 symbol 值后会在全局范围进行注册**。
- 注意：**这个注册的范围包括当前页面和页面中包含的 iframe，以及 service sorker**,我们来看个例子：
```
const iframe = document.createElement("iframe");
iframe.src = String(window.location);
document.body.appendChild(iframe);

iframe.contentWindow.Symbol.for("lison") === Symbol.for("lison"); // true
// 注意：如果你在JavaScript环境中这段代码是没有问题的，但是如果在TypeScript开发环境中，可能会报错：类型“Window”上不存在属性“Symbol”。
// 因为这里编译器推断出iframe.contentWindow是Window类型，但是TypeScript的声明文件中，对Window的定义缺少Symbol这个字段，所以会报错，所以你可以这样写：
// (iframe.contentWindow as Window & { Symbol: SymbolConstructor }).Symbol.for("lison") === Symbol.for("lison")
// 这里用到了类型断言和交叉类型，SymbolConstructor是内置的类型。
```
+ 上面这段代码的意思是创建一个 iframe 节点并把它放到 body 中，我们通过这个iframe 对象的 
contentWindow 拿到这个 iframe 的 window 对象，在iframe.contentWindow上添加一个值就相
当于你在当前页面定义一个全局变量一样，我们看到，在iframe 中定义的键为’lison’的 symbol 值
在和在当前页面定义的键为’lison’的 symbol 值相等，说明它们是同一个值。
>>> Symbol.keyFor()
- Symbol.keyFor(sym) 方法用来获取全局symbol 注册表中与某个 symbol 关联的键。
+ 该方法传入一个symbol值，返回该值在全局注册的键名:
```
const sym = Symbol.for("lison");
console.log(Symbol.keyFor(sym)); // 'lison'
```
### 4.11个内置symbol值(属性)
+ ES6 提供了 11 个内置的 Symbol 值，指向 JS 内部使用的属性和方法。看到它们第一眼你可能会有疑惑，
这些不是 Symbol 对象的一个属性值吗？没错，这些内置的 Symbol 值就是保存在 Symbol 上的，
你可以把Symbol.xxx看做一个 symbol 值。接下来我们来挨个学习一下：
>>> Symbol.hasInstanceq
- 对象的Symbol.hasInstance指向一个内部方法，当你给一个对象设置以Symbol.hasInstance为属性名的
方法后，当其他对象使用instanceof判断是否为这个对象的实例时，会调用你定义的这个方法，参数是其他的
这个对象，来看例子:
```
const obj = {
  [Symbol.hasInstance](otherObj) {
    console.log(otherObj);q
  }
};
console.log({ a: "a" } instanceof obj); // false
// 注意：在TypeScript中这会报错，"instanceof" 表达式的右侧必须属于类型 "any"，或属于可分配给 "Function" 接口类型的类型。
// 是要求你instanceof操作符右侧的值只能是构造函数或者类，或者类型是any类型。这里你可以使用类型断言，将obj改为obj as any
```
+ 可以看到当我们使用 instanceof 判断{ a: ‘a’ }是否是 obj 创建的实例的时候，Symbol.hasInstance 这个方法被调用了。
>>> Symbol.isConcatSpreadable
- 这个值是一个可读写布尔值，其值默认是undefined，当一个数组的 Symbol.isConcatSpreadable 
设为 true或者为默认的undefined 时，这个数组在数组的 concat 方法中会被扁平化。我们来看下例子：
```
let arr = [1, 2];
console.log([].concat(arr, [3, 4])); // 打印结果为[1, 2, 3, 4]，length为4
let arr1 = ["a", "b"];
console.log(arr1[Symbol.isConcatSpreadable]); // undefined
arr1[Symbol.isConcatSpreadable] = false;
console.log(arr1[Symbol.isConcatSpreadable]); // false
console.log([].concat(arr1, [3, 4])); // 打印结果如下：
/*
 [ ["a", "b", Symbol(Symbol.isConcatSpreadable): false], 3, 4 ]
 最外层这个数组有三个元素，第一个是一个数组，因为我们设置了arr1[Symbol.isConcatSpreadable] = false
 所以第一个这个数组没有被扁平化，第一个元素这个数组看似是有三个元素，但你在控制台可以看到这个数组的length为2
 Symbol(Symbol.isConcatSpreadable): false不是他的元素，而是他的属性，我们知道数组也是对象，所以我们可以给数组设置属性
 你可以试试如下代码，然后看下打印出来的效果：
  let arr = [1, 2]
  arr.props = 'value'
  console.log(arr)
 */
```
>>> Symbol.species
- 这里我们需要提前使用类的知识来讲解这个 symbol 值的用法，类的详细内容我们会在后面课程里全面讲解。
这个知识你需要在纯JavaScript的开发环境中才能看出效果，你可以在浏览器开发者工具的控制台尝试。
在TypeScript中，下面两个例子都是一样的会报a.getName is not a function错误。
+ 首先我们使用 class 定义一个类 C，使用 extends 继承原生构造函数 Array，
那么类 C 创建的实例就能继承所有 Array 原型对象上的方法，比如 map、filter 等。我们先来看代码：
```
class C extends Array {
  getName() {
    return "lison";
  }
}
const c = new C(1, 2, 3);
const a = c.map(item => item + 1);
console.log(a); // [2, 3, 4]
console.log(a instanceof C); // true
console.log(a instanceof Array); // true
console.log(a.getName()); // "lison
```
- 这个例子中，a 是由 c 通过 map 方法衍生出来的，我们也看到了，a 既是 C 的实例，
也是 Array 的实例。但是如果我们想只让衍生的数组是 Array 的实例，就需要用 Symbol.species，
我们来看下怎么使用：
- 知名的 Symbol.species 是个函数值属性，其被构造函数用以创建派生对象。
```
class C extends Array {
  static get [Symbol.species]() {
    return Array;
  }
  getName() {
    return "lison";
  }
}
const c = new C(1, 2, 3);
const a = c.map(item => item + 1);
console.log(a); // [2, 3, 4]
console.log(a instanceof C); // false
console.log(a instanceof Array); // true
console.log(a.getName()); // error a.getName is not a function
```
+ 就是给类 C 定义一个静态 get 存取器方法，方法名为 Symbol.species，
然后在这个方法中返回要构造衍生数组的构造函数。所以最后我们看到，a instanceof C为 false，
也就是 a 不再是 C 的实例，也无法调用继承自 C 的方法。
>>>  Symbol.match、Symbol.replace、Symbol.search 和 Symbol.split
- 这个 Symbol.match 值指向一个内部方法，当在**字符串 str 上调用 match 方法**时，
会调用这个方法，来看下例子：
```
let obj = {
  [Symbol.match](string) {
    return string.length;
  }
};
console.log("abcde".match(obj)); // 5
```
+ 相同的还有 Symbol.replace、Symbol.search 和 Symbol.split，使用方法和 Symbol.match 是一样的。
>>> Symbol.iterator
- 数组的 Symbol.iterator 属性指向该数组的默认遍历器方法
```
const arr = [1, 2, 3];
const iterator = arr[Symbol.iterator]();
console.log(iterator);
console.log(iterator.next());
console.log(iterator.next());
console.log(iterator.next());
console.log(iterator.next());
```
+ 这个 Symbol.iterator 方法是可写的，我们可以自定义遍历器方法。
>>>  Symbol.toPrimitive
+ 对象的这个属性指向一个方法，当这个对象被转为原始类型值时会调用这个方法，
这个方法有一个参数，是这个对象被转为的类型，我们来看下：
```
let obj = {
  [Symbol.toPrimitive](type) {
    console.log(type);
  }
};
// const b = obj++ // number
const a = `abc${obj}`; // string
```
>>> Symbol.toStringTag
- Symbol.toStringTag 和 Symbol.toPrimitive 相似，对象的这个属性的值可以是一个字符串，
也可以是一个存取器 get 方法，当在对象上调用 toString 方法时调用这个方法，
返回值将作为"[object xxx]"中 xxx 这个值：
```
let obj = {
  [Symbol.toStringTag]: "lison"
};
obj.toString(); // "[object lison]"
let obj2 = {
  get [Symbol.toStringTag]() {
    return "haha";
  }
};
obj2.toString(); // "[object haha]"
```
>>> Symbol.unscopables
- 这个值和 with 命令有关，我们先来看下 with 怎么使用：
```
const obj = {
  a: "a",
  b: "b"
};
with (obj) {
  console.log(a); // "a"
  console.log(b); // "b"
}
// 如果是在TypeScript开发环境中，这段代码可能with会报错：不支持 "with" 语句，这是因为在严格模式下，是不允许使用with的。
```
+ 可以看到，使用 with 传入一个对象后，在代码块中访问对象的属性就不需要写对象了，
直接就可以用它的属性。对象的 Symbol.unscopables 属性指向一个对象，该对象包含了
**当使用 with 关键字时，哪些属性被 with 环境过滤掉**：
```
console.log(Array.prototype[Symbol.unscopables]);
/*
{
    copyWithin: true
    entries: true
    fill: true
    find: true
    findIndex: true
    includes: true
    keys: true
    values: true
}
*/
```
### 在TypeScript中使用symbol类型
1. 基础
+ 学习完ES6标准中Symbol的所有内容后，我们来看下在TypeScript中使用symbol类型值，
很简单。就是制定一个值的类型为symbol类型：
```
let sym: symbol = Symbol()
```
2. unique symbol
+ TypeScript在2.7版本对Symbol做了补充，增加了**unique symbol**这种类型，
他是symbols的**子类型**，这种类型的值只能由**Symbol()**或**Symbol.for()**创建，
或者通过**指定类型来指定一个值是这种类型**。这种类型的值仅可用于**常量的定义**和用于**属性名**。
另外还有一点要注意，定义unique symbol类型的值，**必须用const不能用let**。
我们来看个在TypeScript中使用Symbol值作为属性名的例子：
```
const key1: unique symbol = Symbol()
let key2: symbol = Symbol()
const obj = {
    [key1]: 'value1',
    [key2]: 'value2'
}
console.log(obj[key1])
console.log(obj[key2]) // error 类型“symbol”不能作为索引类型使用。
```
## 小结
本小节我们详细学习了Symbol的全部知识，本小节的内容较多：
我们学习了Symbol值的基本使用，使用Symbol函数创建一个symbol类型值，
可以给它传一个字符串参数，来对symbol值做一个区分，但是即使多次Symbol函数
调用传入的是相同的字符串，创建的symbol值也是彼此不同的。我们还学习了Symbol的
两个静态方法：Symbol.for和Symbol.keyFor，Symbol.for调用时传入一个字符串，
使用此方式创建symbol值时会先在全局范围搜索是否有用此字符串注册的symbol值。
如果没有创建一个新的；如果有返回这个symbol值，Symbol.keyFor则是传入一个
symbol值然后返回该值在全局注册时的标志字符串。我们还学习了11个内置的symbol值，
在设计一些高级逻辑时，可能会用到，大部分业务开发很少用到，你可以了解这些值的用途，
日后如果遇到这个需求可以想到这有这些内容。
# 深入学习枚举
+ **枚举**是 TypeScript **新增加的一种数据类型**，这在其他很多语言中很常见，
但是 JavaScript 却没有。使用枚举，我们可以给一些**难以理解的常量**赋予
**一组具有意义**的直观的名字，使其更为直观，你可以理解**枚举就是一个字典**。
枚举使用 enum 关键字定义，TypeScript 支持**数字和字符串**的枚举。
### 数字枚举
1. 我们先来通过数字枚举的简单例子，来看下枚举是做什么的：
```
enum Status {// 这里你的TSLint可能会报一个：枚举声明只能与命名空间或其他枚举声明合并。这样的错误，这个不影响编译，声明合并的问题我们在后面的小节会讲。
  Uploading,
  Success,
  Failed
}
console.log(Status.Uploading); // 0
console.log(Status["Success"]); // 1
console.log(Status.Failed); // 2
```
+ 我们使用enum关键字定义了一个枚举值 Status，它包含三个字段，每个字段间用逗号隔开。
我们**使用枚举值的元素值时，就像访问对象的属性**一样，你可以使用’.‘操作符和’[]'两种
形式访问里面的值，这和对象一样。
- 再来看输出的结果，Status.Uploading 是 0，Status['Success']是 1，Status.Failed 是2
2. 指定索引号
+ 我们在定义枚举 Status 的时候，并没有指定索引号，是因为这是默认的编号，我们也可以自己指定：
```
// 修改起始编号
enum Color {
  Red = 2,
  Blue,
  Yellow
}
console.log(Color.Red, Color.Blue, Color.Yellow); // 2 3 4
// 指定任意字段的索引值
enum Status {
  Success = 200,
  NotFound = 404,
  Error = 500
}
console.log(Status.Success, Status.NotFound, Status.Error); // 200 404 500
// 指定部分字段，其他使用默认递增索引
enum Status {
  Ok = 200,
  Created,
  Accepted,
  BadRequest = 400,
  Unauthorized
}
console.log(Status.Created, Status.Accepted, Status.Unauthorized); // 201 202 401
```
+ 数字枚举在定义值的时候，可以使用**计算值和常量**。但是要注意，
如果某个字段**使用了计算值或常量**，那么该**字段后面紧接着的字段**
**必须设置初始值，这里不能使用默认的递增值**了，来看例子：
```
const getValue = () => {
  return 0;
};
enum ErrorIndex {
  a = getValue(),
  b, // error 枚举成员必须具有初始化的值,因为a使用了计算值
  c
}
enum RightIndex {
  a = getValue(),
  b = 1,
  c
}
const Start = 1;
enum Index {
  a = Start,
  b, // error 枚举成员必须具有初始化的值，因为a使用了常量
  c
}
```
#### 反向映射(通过值来映射对应的枚举)
+ 我们定义一个枚举值的时候，可以通过 Enum['key']或者 Enum.key 
的形式获取到对应的值 value。TypeScript 还**支持反向映射**，
但是**反向映射只支持数字枚举**，我们后面要讲的**字符串枚举是不支持的**。
来看下反向映射的例子：
```
enum Status {
  Success = 200,
  NotFound = 404,
  Error = 500
}
console.log(Status["Success"]); // 200
console.log(Status[200]); // 'Success'
console.log(Status[Status["Success"]]); // 'Success'
```
+ TypeScript 中**定义的枚举**，编译之后其实是**对象**，我们来看下
上面这个例子中的枚举值 Status 编译后的样子：
- 我们可以直接**使用tsc指定某个文件或者不指定文件直接编译整个目录**，
运行后就会产生相应的编译后的JavaScript文件，你也可以到TypeScript官方
文档提供的在线练习场，在这里你可以编写TypeScript代码，它会同步进行编译。
实时编译为JavaScript代码，是你了解编译后结果的好方式。
* 上面的枚举编译后成为下面的对象，
```
{
    200: "Success",
    404: "NotFound",
    500: "Error",
    Error: 500,
    NotFound: 404,
    Success: 200
}
```
+ 可以看到，TypeScript 会把我们定义的**枚举值的字段名分别作为对象的属性名和值**，
把**枚举值的字段值分别作为对象的值和属性名**，同时添加到对象中。这样我们既可以通过
枚举值的字段名得到值，也可以通过枚举值的值得到字段名。
### 字符串枚举
- TypeScript2.4 版本新增了**字符串枚举**，字符串枚举值要求**每个字段的值**都必须是
**字符串字面量**，或者是**该枚举值中另一个字符串枚举成员**，先来看个简单例子：
```
enum Message {
  Error = "Sorry, error",//这里是字符串字面量
  Success = "Hoho, success"
}
console.log(Message.Error); // 'Sorry, error'
```
- 再来看我们使用枚举值中其他枚举成员的例子：
```
enum Message {
  Error = "error message",
  ServerError = Error,
  ClientError = ServerError
}
console.log(Message.Error); // 'error message'
console.log(Message.ServerError); // 'error message'
```
- 注意，这里的其他枚举成员指的是**同一个枚举值中的枚举成员**，
因为**字符串枚举不能使用常量或者计算值**，所以也**不能使用其他枚举值中的成员**。
### 异构枚举
- 简单来说异构枚举就是枚举值中成员值**既有数字类型又要字符串类型**,如下:
```
enum Result {
  Faild = 0,
  Success = "Success"
}
```
+ 但是这种如果不是真的需要，不建议使用。因为往往我们**将一类值整理为一个枚举值**的时候，
它们的特点是相似的。比如我们在做接口请求时的返回状态码，如果是状态码都是数值，
如果是提示信息，都是字符串，所以在使用枚举的时候，往往是**可以避免使用异构枚举**的，
重点是做好类型的整理。
### 枚举成员类型和联合枚举类型
- **如果枚举值里所有成员的值都是字面量类型的值**，
**那么这个枚举的每个成员和枚举值本身都可以作为类型来使用**，
先来看下满足条件的枚举成员的值有哪些：
+ 不带初始值的枚举成员，例如enum E { A }
+ 值为字符串字面量，例如enum E { A = 'a'}
+ 值为数值字面量，或者带有-符号的数值字面量，例如enum E { A = 1},enum E {A = -1}
-
---
当我们的枚举值的所有成员的值都是上面这三种情况的时候，**枚举值和成员**就可以**作为类型**来用：
1. 枚举成员类型
我们可以把符合条件的**枚举值的成员作为类型**来使用，来看例子：
```
enum Animal {
  Dog = 1,
  Cat = 2
}
interface Dog {
  type: Animal.Dog; // 这里使用Animal.Dog作为类型，指定接口Dog的必须有一个type字段，且类型为Animal.Dog
}
interface Cat {
  type: Animal.Cat; // 这里同上
}
let cat1: Cat = {
  type: Animal.Dog // error [ts] 不能将类型“Animal.Dog”分配给类型“Animal.Cat”
};
let dog: Dog = {
  type: Animal.Dog
};
```
2. 联合枚举类型
- 当我们的枚举值符合条件时，这个枚举值就可以看做是**一个包含所有成员的联合类型**，先来看例子：
```
enum Status {
  Off,
  On
}
interface Light {
  status: Status;
}
enum Animal {
  Dog = 1,
  Cat = 2
}
const light1: Light = {
  status: Animal.Dog // error 不能将类型“Animal.Dog”分配给类型“Status”
};
const light2: Light = {
  status: Status.Off
};
const light3: Light = {
  status: Status.On
};
```
+ 上面例子定义接口 Light 的 status 字段的类型为枚举值 Status，
那么此时 status 的属性值必须**为 Status.Off 和 Status.On 中的一个**，
也就是**相当于status: Status.Off | Status.On**。
### 运行时的枚举
- **枚举**在编译成 JavaScript 之后**实际是一个对象**。这个我们前面讲过了，
既然是对象，那么就**可以当成对象来使用**，我们来看个例子：
```
enum E {
  A,
  B
}
const getIndex = (enumObj: { A: number }): number => {
  return enumObj.A;
};
console.log(getIndex(E)); // 0
```
+ 上面这个例子要求 getIndex 的参数为一个对象，且必须包含一个属性名为’A’的属性，
其值为数值类型，只要有这个属性即可。当我们调用这个函数，把**枚举值 E 作为实参**传入
是可以的，因为**它在运行的时候是一个对象**，包含’A’这个属性，因为它在运行的时候相当
于下面这个对象：
```
{
    0: "A",
    1: "B",
    A: 0,
    B: 1
}
```
### const enum
+ 我们**定义**了枚举值之后，编译成 JavaScript 的代码会**创建一个对应的对象**，
**这个对象**我们可以在**程序运行的时候使用**。但是如果我们使用枚举只是为了让程序可读性好，
并不需要编译后的对象呢？这样会增加一些编译后的代码量。所以 TypeScript 在 1.4 
新增 const enum(完全嵌入的枚举)，在之前讲的定义枚举的语句之前加上const关键字，
这样**编译后的代码不会创建这个对象**，只是会**从枚举里拿到相应的值进行替换**，
来看我们下面的定义：
```
enum Status {
  Off,
  On
}
const enum Animal {
  Dog,
  Cat
}
const status = Status.On;
const animal = Animal.Dog;
```
- 上面的例子编译成 JavaScript 之后是这样的：
```
var Status;
(function(Status) {
  Status[(Status["Off"] = 0)] = "Off";
  Status[(Status["On"] = 1)] = "On";
})(Status || (Status = {}));
var status = Status.On;
var animal = 0; /* Dog */
```
+ 我们来看下 Status 的处理，先是定义一个变量 Status，然后定义一个立即执行函数，
在函数内给 Status 添加对应属性，首先Status[“Off”] = 0是给Status对象设置Off属性，
并且值设为 0，这个**赋值表达式的返回值是等号右边的值**，也就是 0，
所以Status[Status[“Off”] = 0] = "Off"相当于Status[0] = “Off”。创建了这个对象之后，
将 Status 的 On 属性值赋值给 status；再来看下 animal 的处理，我们看到编译后的代码
并没有像Status创建一个Animal对象，而是直接把Animal.Dog的值0替换到了
const animal = Animal.Dog表达式的Animal.Dog位置，这就是const enum的用法了。
### 本章小结
- 本小节我们学习了两种基本的枚举：**数字枚举**和**字符串枚举**，它俩的最主要的区别
就是枚举成员值的类型了，**数字枚举成员的值**必须都是**数值类型**，
而**字符串枚举成员的值**必须都是**字符串**。**数字枚举还有一个概念叫反向映射**，
就是当我们定义了枚举值后，不仅定义了**字段到值的映射**，同时**编译器**根据**反向映射**
定义了**值到字段的映射**。我们还学习了**数字枚举和字符串枚举**的**杂交体**——**异构枚举**，
但是很少用，原因也解释过了；**枚举值和枚举成员**在**作为值使用**的同时，还可以**作为类型**使用，
但是有三个条件，可以回顾下；最后我们还学习了**枚举值在编译后是一个对象**，可以在**运行时**使用，
如果我们在运行时用不到，可以在定义枚举时在前面**加上const来选择不生成对象**(编译时优化)，
而是直接将值替换到响应位置。
下个小节我们将学习类型断言，通过**类型断言**，可以在一些情况**告诉 TypeScript 编译器**，
我们的**逻辑是对的，不是类型错误**，从而达到预期。

# 使用类型断言达到预期
- 学完前面的小节，你已经学习完了TypeScript的基本类型。从本小节开始，你将**开始接触逻辑**。
在这之前，先来学习一个概念：**类型断言**。
+ 虽然 TypeScript 很强大，但有时它还是不如我们了解一个值的类型，这时候我们更希望
 TypeScript 不要帮我们进行**类型检查**，而是交给我们自己来，所以就用到了类型断言。
 类型断言有点像是一种**类型转换**，它把某个值强行指定为**特定类型**，我们先看个例子：
 ```
 const getLength = target => {
  if (target.length) {
    return target.length;
  } else {
    return target.toString().length;
  }
};
 ```
 + 这个函数能够接收一个参数，并返回它的长度，我们可以传入字符串、数组或数值等类型的值。
 如果有 length 属性，说明参数是数组或字符串类型，如果是数值类型是没有 length 属性的，
 所以需要把数值类型转为字符串然后再获取 length 值。
 现在我们限定传入的值只能是字符串或数值类型的值：
 ```
 const getLength = (target: string | number): number => {
  if (target.length) { //类型"string | number"上不存在属性"length"
    return target.length; // 类型"number"上不存在属性"length"
  } else {
    return target.toString().length;
  }
};
 ```
 - 当 TypeScript 不确定一个联合类型的变量到底是哪个类型的时候，
 我们只能访问此联合类型的所有类型里共有的属性或方法，所以现在加了
 对参数target和返回值的类型定义之后就会报错：
 + 很显然，我们是要做判断的，我们判断如果 target.length 不为 undefined， 
 说明它是有 length 属性的，但我们的参数是string | number联合类型，所以
 在我们开始做判断的时候就会报错。这个时候就要用**类型断言**，将**tagrget的类型断言成string类型**。
 它有两种写法，一种是<type>value，一种是**value as type**，下面例子中我们用**两种形式**都写出来：
 ```
 const getStrLength = (target: string | number): number => {
  if ((<string>target).length) { // 这种形式在JSX代码中不可以使用，而且也是TSLint不建议的写法
    return (target as string).length; // 这种形式是没有任何问题的写法，所以建议大家始终使用这种形式
  } else {
    return target.toString().length;
  }
};
 ```
 - 例子的函数体用到了三次target，前两次都是访问了 target.length 属性，
 所以都要用**类型断言**来表明这个地方是 string 类型；而最后的 target 调用了
    toString方法，因为 number 和 string 类型的值都有 toString 方法，所以没有报错。
+ 这样虽然没问题了，但是**每一处不同值会有不同情况的地方**都需要用**类型断言**，
  后面讲到**高级类型**的时候会讲如何使用**自定义类型保护**来简化这里。
+ 注意了，这两种写法都可以，但是 tslint 推荐使用**as**关键字，而且在 JSX 中只能使用as这种写法。 
### 本章小节
- 本小节我们学习了类型断言的使用。使用类型断言，我们可以**告诉编译器某个值确实是我们所认为的值**，
从而让编译器**进行正确的类型推断**，让**类型检查**符合我们的预期。下个小节我们将学习**接口**，
学习了接口后，我们就可以**定义几乎所有的数据结构**了。
# 使用接口定义几乎任意结构
- 本小节我们来学习接口，正如题目所说的，你可以**使用接口定义几乎任意结构**，
本小节我们先来学习下接口的基本使用方法。
### 1.基本用法
- 我们需要定义这样一个函数，参数是一个对象，里面包含两个字段：firstName 和 lastName，
也就是英文的名和姓，然后返回一个拼接后的完整名字。来看下函数的定义：
```
// 注：这段代码为纯JavaScript代码，请在JavaScript开发环境编写下面代码，在TypeScript环境会报一些类型错误
const getFullName = ({ firstName, lastName }) => {
  return `${firstName} ${lastName}`;
};
```
使用时传入参数:
```
getFullName({
  firstName: "Lison",
  lastName: "Li"
}); // => 'Lison Li'
```
- 没有问题，我们得到了拼接后的完整名字，但是使用这个函数的人如果传入一些不是很理想的参数时，
就会导致各种结果：
```
getFullName(); // Uncaught TypeError: Cannot destructure property `a` of 'undefined' or 'null'.
getFullName({ age: 18, phone: "13312345678" }); // 'undefined undefined'
getFullName({ firstName: "Lison" }); // 'Lison undefined'
```
+ 这些都是我们不想要的，在开发时**难免会传入错误的参数**，所以 TypeScript 能够帮我们
在**编译阶段**就检测到这些错误。我们来完善下这个函数的定义：
```
const getFullName = ({
  firstName,
  lastName,
}: { // 指定这个参数的类型，因为他是一个对象，所以这里来指定对象中每个字段的类型
  firstName: string; // 指定属性名为firstName和lastName的字段的属性值必须为string类型
  lastName: string;
}) => {
  return `${firstName} ${lastName}`;
};
```
+ 我们通过**对象字面量**的形式去限定我们传入的这个对象的结构，现在再来看下之前的调用会出现什么提示：
```
getFullName(); // 应有1个参数，但获得0个
getFullName({ age: 18, phone: 123456789 }); // 类型“{ age: number; phone: number; }”的参数不能赋给类型“{ firstName: string; lastName: string; }”的参数。
getFullName({ firstName: "Lison" }); // 缺少必要属性lastName
```
+ 这些都是在我们**编写代码**的时候 TypeScript **提示**给我们的**错误信息**，
这样就**避免**了在**使用函数**的时候**传入不正确的参数**。
接下来我们用这节课要讲的接口来书写上面的规则，我们使用**interface**来定义接口：
```
interface Info {
  firstName: string;
  lastName: string;
}
const getFullName = ({ firstName, lastName }: Info) =>
  `${firstName} ${lastName}`;
```
+ 注意在**定义接口**的时候，你**不要把它理解为是在定义一个对象**，而要理解**为{}括号包裹的是一个代码块**，
里面是一条条**声明语句**，只不过声明的**不是变量的值而是类型**。声明也不用**等号**赋值，而是**冒号**指定类型。
每条声明之前用换行分隔即可，或者也可以使用分号或者逗号，都是可以的。
### 2.可选属性
+ 当我们定义一些结构的时候，一些结构对于某些字段的要求是**可选的**，有这个字段就做处理，没有就忽略，
所以针对这种情况，typescript为我们提供了可选属性。
我们先定义一个描述传入蔬菜信息的句子的函数：
```
const getVegetables = ({ color, type }) => {
  return `A ${color ? color + " " : ""}${type}`;
};
```
- 我们可以看到这个函数中根据传入对象中的 color 和 type 来进行描述返回一句话，
color 是可选的，所以我们可以**给接口设置可选属性**，在属性名后面加个?即可：
```
interface Vegetables {
  color?: string;
  type: string;
}
```
- 这里可能 tslint 会报一个警告，告诉我们接口应该以大写的i开头，如果你想关闭这条规则，
可以在 tslint.json 的 rules 里添加"interface-name": [true, “never-prefix”]来关闭

### 3.多余属性检查
```
getVegetables({
  type: "tomato",
  size: "big" // 'size'不在类型'Vegetables'中
});
```
+ 我们看到，传入的参数没有 color 属性，但也没有错误，因为它是可选属性。
但是我们多传入了一个 size 属性，这同样会报错，TypeScript 会告诉你，
接口上不存在你多余的这个属性。**只要接口中没有定义这个属性，就会报错**，
但如果你定义了可选属性 size，那么上面的例子就不会报错。
- 这里可能 tslint 会报一个警告，告诉我们属性名没有按开头字母顺序排列属性列表，
如果你想关闭这条规则，可以在 tslint.json 的 rules 里
添加"object-literal-sort-keys": [false]来关闭。
### 4.绕开多余属性检查
+ 有时我们并不希望 TypeScript 这么严格地对我们的数据进行检查，
比如我们只需要保证传入getVegetables的对象有type属性就可以了，
至于实际使用的时候传入对象有没有多余的属性，多余属性的属性值是什么类型，
这些都无所谓，那就需要**绕开多余属性检查**，有如下三个方法：
#### 1)使用类型断言
+ 我们在基础类型中讲过，类型断言就是用来明确告诉 TypeScript，
我们已经**自行进行了检查**，确保这个类型没有问题，希望 TypeScript 
对此不进行检查，所以最简单的方式就是**使用类型断言**：
```
interface Vegetables {
  color?: string;
  type: string;
}
const getVegetables = ({ color, type }: Vegetables) => {
  return `A ${color ? color + " " : ""}${type}`;
};
getVegetables({
  type: "tomato",
  size: 12,
  price: 1.2
} as Vegetables);
```
#### 2)添加索引签名
+ 更好的方式是**添加字符串索引签名**，索引签名我们会在后面讲解，先来看怎么实现：
```
interface Vegetables {
  color: string;
  type: string;
  [prop: string]: any;
}
const getVegetables = ({ color, type }: Vegetables) => {
  return `A ${color ? color + " " : ""}${type}`;
};
getVegetables({
  color: "red",
  type: "tomato",
  size: 12,
  price: 1.2
});
```
#### 3)利用类型兼容性
- 这种方法现在还不是很好理解，也是**不推荐使用的**，先来看写法：
```
interface Vegetables {
  type: string;
}
const getVegetables = ({ type }: Vegetables) => {
  return `A ${type}`;
};

const option = { type: "tomato", size: 12 };
getVegetables(option); 
```
+ 上面这种方法完美通过检查，我们将对象字面量赋给一个变量option，
然后getVegetables传入 option，这时没有报错。是因为直接将对象字面量传入函数，
和先赋给变量再将变量传入函数，这两种检查机制是不一样的，后者是因为**类型兼容性**。
我们后面会有专门一节来讲类型兼容性。简单地来说：如果 b 要赋值给 a，那要求 b 
至少需要与 a 有相同的属性，多了无所谓。

在上面这个例子中，option的类型应该是Vegetables类型，对象{ type: ‘tomato’, size: 12 }
要赋值给 option，option中所有的属性在这个对象字面量中都有，所以这个对象的类型
和option(也就是Vegetables类型)是兼容的，所以上面例子不会报错。如果你现在还想不明白没关系，
我们还会在后面详细去讲。
### 5.只读属性
- 接口也可以设置只读属性，如下:
```
interface Role {
  readonly 0: string;
  readonly 1: string;
}
```
- 这里我们定义了一个**角色字典**，有 0 和 1 两种角色 id。下面我们定义一个实际的角色数据，
然后来试图修改一下它的值：
```
const role: Role = {
  0: "super_admin",
  1: "admin"
};
role[1] = "super_admin"; // Cannot assign to '0' because it is a read-only property
```
- 我们看到 TypeScript 告诉我们不能分配给索引0，因为它是只读属性。设置一个值只读，
我们是否想到ES6里定义常量的关键字const？使用const定义的常量定义之后不能再修改，
这有点只读的意思。那readonly和const在使用时该如何选择呢？这主要看你这个值的用途，
如果是**定义一个常量，那用const**，如果这个值是**作为对象的属性，那请用readonly**。
我们来看下面的代码：
```
const NAME: string = "Lison";
NAME = "Haha"; // Uncaught TypeError: Assignment to constant variable

const obj = {
  name: "lison"
};
obj.name = "Haha";

interface Info {
  readonly name: string;
}
const info: Info = {
  name: "Lison"
};
info["name"] = "Haha"; // Cannot assign to 'name' because it is a read-only property
```
+ 我们可以看到上面使用const定义的常量NAME定义之后再修改会报错，但是如果使用const定义一个对象，
然后修改对象里属性的值是不会报错的。所以如果我们要**保证对象的属性值不可修改，需要使用readonly**。
### 6.函数类型
- **接口**可以**描述普通对象**，还可以**描述函数类型**，我们先看写法:
```
interface AddFunc {
    (num1:number, num2:number):number;
}
```
+ 这里我们定义了一个AddFunc结构，这个结构要求实现这个结构的值，必须包含一个和结构里
**定义的函数一样参数、一样返回值的方法**，或者**这个值就是符合这个函数要求的函数**。
我们管花括号里包着的内容为**调用签名**，它由**带有参数类型的参数列表**和**返回值类型**组成。
后面学到**类型别名**一节时我们还会学习其他写法。来看下如何使用:
```
const add: AddFunc = (n1, n2) => n1 + n2;
const join: AddFunc = (n1, n2) => `${n1} ${n2}`; // 不能将类型'string'分配给类型'number'
add("a", 2); // 类型'string'的参数不能赋给类型'number'的参数
```
- 上面我们**定义的add函数**接收两个数值类型的参数，返回的结果也是数值类型，所以没有问题。
而join函数参数类型没错，但是返回的是字符串，所以会报错。而当我们**调用add函数**时，
传入的参数如果和接口定义的类型不一致，也会报错。
你应该注意到了，**实际定义函数**的时候，**名字是无需和接口中参数名相同的**，只需要位置对应即可。
# 本章小节
- 本小节我们学习了**接口**的一些**基本定义和用法**，通过使用接口，我们可以**定义绝大部分的数据结构**，
从而**限定值的结构**。我们可以通过**修饰符(?,readonly)**来指定结构中某个字段的**可选性和只读性**，
以及默认情况下**必选性**。而接口的校验是严格的，在定义一个实现某个接口的值的时候，
对于接口中没有定义的字段是不允许出现的，我们称这个为**多余属性检查**；同时我们讲了
三种**绕过多余属性检查**的方法(类型断言、索引签名和类型兼容性)，来满足程序的灵活性。
最后我们学习了如何通过**接口**，来**定义函数类型**，
当然我们后面还会学习其他定义函数类型的方法。下个小节，我们将学习**接口的高级用法**，
学习完之后，除了涉及到类的知识的部分外，你就掌握了接口的所有知识。
# 接口的高阶用法
- 学习了上个小节接口的基础用法后，相信你已经能够使用**接口来描述一些结构**了。
本小节我们来继续学习接口，学习**接口的高阶用法**。接口有一小部分知识与类的知识相关，
所以我们放在讲解类的小节后面补充讲解，我们先来学习除了这一小部分之外剩下的接口的知识。
### 1.索引类型
- 我们可以使用**接口**描述**索引的类型**和**通过索引得到的值的类型**，比如一个数组['a','b'],
数组索引0对应的通过索引得到的值为'a',我们可以同时给**索引和值**都设置类型，看下面的示例:
```
interface RoleDic{
    [id:number]: string;
}
const role1: RoleDic = {
    0:"super-admin",
    1:"admin"
}
const role2: RoleDic = {
    s:"super_admin", //error 不能将类型"{ s: string; a: string; }"分配给类型"RoleDic"。
    a:"admin"   
}
const role3: RoleDic = ["super_admin","admin"]; 
```
- 上面的例子中 role3 定义了一个数组，索引为**数值类型**，值为**字符串类型**。
- 你也可以给索引设置readonly，从而防止**索引返回值**被修改。
```
interface RoleDic {
  readonly [id: number]: string;
}
const role: RoleDic = {
  0: "super_admin"
};
role[0] = "admin"; // error 类型"RoleDic"中的索引签名仅允许读取
```
- 这里有的点需要注意，你可以设置索引类型为 number。但是这样如果你将属性名设置为字符串类型，
则会报错；但是如果你设置索引类型为字符串类型，那么即便你的属性名设置的是数值类型，也没问题。
因为 JS 在**访问属性值**的时候，如果属性名是**数值类型**，会先将**数值类型转为字符串**，
然后再去访问。你可以看下这个例子：
```
const obj = {
  123: "a", // 这里定义一个数值类型的123这个属性
  "123": "b" // 这里在定义一个字符串类型的123这个属性，这里会报错：标识符“"123"”重复。
};
console.log(obj); // { '123': 'b' }
```
- 如果数值类型的属性名不会转为字符串类型，那么这里数值123和字符串123是不同的两个值，
则最后对象obj应该同时有这两个属性；但是实际打印出来的obj只有一个属性，属性名为字符串"123"，
而且值为"b"，说明数值类型属性名123**被覆盖掉**了，就是因为它被**转为了字符串类型属性名"123"**；
又因为一个对象中多个相同属性名的属性，定义在后面的会覆盖前面的，所以结果就是obj只保留了
后面定义的属性值。
### 2.继承接口
```
TS的接口只存在于编译时，Java的接口只能约束类，TS接口可以描述普通对象和函数类型**
```
- **接口可以继承**，这和类一样，这提高了**接口的可复用性**。来看一个场景:
我们定义了一个Vegetables接口，它会对color属性进行限制。再定义两个接口，
一个为Tomato,一个为Carrot，这两个类都需要对color进行限制，而各自又有各自独有的
属性限制，我们可以这样定义:
```
interface Vegetables {
  color: string;
}
interface Tomato {
  color: string;
  radius: number;
}
interface Carrot {
  color: string;
  length: number;
}
```
- 三个接口中都有对color的定义，但是这样写很繁琐，所以我们可以用**继承**来改写：
```
interface Vegetables {
  color: string;
}
interface Tomato extends Vegetables{
  radius: number;
}
interface Carrot extends Vegetables{
  length: number;
}
const tomato:Tomato = {
  radius: 1.2 // error  Property 'color' is missing in type '{ radius: number; }'
}
const carrot:Carrot = {
  color:"orange",
  length:20
}
```
- 上面定义的 tomato 变量因为**缺少了**从Vegetables接口**继承**来的 color 属性，从而报错。
- **一个接口可以被多个接口继承，同样，一个接口也可以继承多个接口**，多个接口用**逗号**隔开。
比如我们再定义一个Food接口，Tomato 也可以继承 Food：
```
interface Vegetables {
  color: string;
}
interface Food {
  type: string;
}
interface Tomato extends Food, Vegetables {
  radius: number;
}

const tomato: Tomato = {
  type: "vegetables",
  color: "red",
  radius: 1.2
};  // 在定义tomato变量时将继承过来的color和type属性同时声明
```
### 3.混合类型接口
- JS的类型时灵活的。在JS中，函数是对象类型，对象可以有属性，所以有时我们的一个对象，
它既是一个函数，也包含一些属性。比如我们要实现一个计数器函数，比较直接的做法是
定义一个函数和一个全局变量:
```
let count = 0;
const countUp = () => count++;
```
- 但是这种方法需要在函数外面定义一个变量，更优一点的方法是使用**闭包**:
```
//JavaScript
const countUp = (()=>{
  let count = 0;
  return ()=>{
    return ++count;
  }
})()
console.log(countUp());//1
console.log(countUp());//2
```
- 在 TypeScript3.1 版本之前，我们需要借助**命名空间**来实现。
但是在 3.1 版本，TypeScript 支持直接给**函数添加属性**，**虽然这在 JS 中早就支持了**:
```
// javascript
let countUp = ()=>{
  return ++countUp.count
}
countUp.count = 0;//给函数添加属性
console.log(countUp()); //1
console.log(countUp()); //2
```
- 我们可以看到，我们把一个函数赋值给countUp，又给它绑定了一个属性count,我们的计数器保存
在这个count属性中。
- 我们可以使用**混合类型接口**来指定上面例子中countUp的类型:
```
interface Counter {
  ():void; //这里定义Counter这个结构必须包含一个函数，函数的要求是无参数，返回值为void，既无返回值
  count:number; //而且这个结构还必须包含一个名为count、值的类型为numer类型的属性
}
const getCounter = ():Counter =>{ // 这里定义一个函数用来返回这个计数器
  const c = ()=>{ // 定义一个函数，逻辑和前面例子的一样
    c.count++
  }
  c.count = 0; //再给这个函数添加一个count属性初始值为0
  return c;//最后返回这个函数对象
}
const counter:Counter = getCounter();// 通过getCounter函数得到这个计数器
counter();
console.log(counter.count); // 1
counter();
console.log(counter.count); // 2
```
- 上面的例子中，getCounter函数返回值类型为Counter，它是一个函数，无返回值，既返回值
类型为void，它还包含一个属性count，属性返回值类型为number。
### 本章小结
- 本小节我们在接口基础知识的基础上，学习了**接口的高阶用法**。我们学习了
如何**限定索引的类型**，即使用[]将索引名括起来，然后使用: type来指定索引的类型；
还学习了一种**复用现有接口的接口定义方式**，即**继承**，使用**extends**关键字实现继承；
最后我们通过计数器的例子，学习了如何使用**混合类型接口**实现更**复杂的数据结构**。
还有一些涉及到类的关于接口的知识，我们会在讲了类之后做一个补充。

下个小节我们将学习**函数**的相关内容。**函数是代码里的重头戏**，而且内容较多，
我们会分两个小节来讲解，跟紧别掉队哈。
# 为函数和函数参数定义类型
- 本小节我们来学习**函数类型的定义**，以及对**函数参数**的详细介绍。前面我们在讲object例子
的时候见过简单的函数定义，在那个例子中我们学习了如何简单地为一个参数指定类型。
在本小节你将学习**三种定义函数类型**的方式，以及关于**参数**的三个知识——
即**可选参数、默认参数和剩余参数**。接下来我们开始学习。
### 1. 函数类型
##### 1)为函数定义类型
- 我们可以**给函数定义类型**，这个定义包括对**参数和返回值**的**类型定义**，我们先看简单的定义写法:
```
function add(arg1:number,arg2:number):number{
  return x + y
}
// 或者
const add = (arg1: number,arg2:number):number => {
  return x + y;
};
```
- 在上面的例子中我们用**function和箭头函数**两种形式定义了add函数，以展示如何**定义函数类型**。
这里参数arg1和arg2都是数值类型，最后通过相加得到的结果也是数值类型。
如果在这里**省略参数的类型**，TypeScript 会**默认这个参数**是 **any** 类型；如果**省略返回值的类型**，
如果函数无返回值，那么 TypeScript 会**默认函数返回值**是 **void** 类型；如果函数有返回值，
那么 TypeScript 会根据我们**定义的逻辑推断出返回类型**。
#### 2)完整的函数类型
- **一个函数的定义包括函数名、参数、逻辑和返回值**。我们为一个**函数定义类型**时，完整的定义应该包括
**参数类型和返回值类型**。上面的例子中，我们都是在定义函数的指定参数类型和返回值类型。接下来我们看下，
**如何定义一个完整的函数类型**，以及用这个函数类型来规定一个函数定义时参数和返回值需要符合的类型。
先来看例子然后再进行解释：
```
let add: (x: number, y: number) => number;
add = (arg1: number, arg2: number): number => arg1 + arg2;
add = (arg1: string, arg2: string): string => arg1 + arg2; // error
```
上面这个例子中，我们首先定义了一个变量 add，给它**指定了函数类型**，
也就是(x: number, y: number) => number，这个**函数类型**包含**参数和返回值的类型**。
然后我们给 add 赋了一个实际的函数，这个函数参数类型和返回类型都和函数类型中定义的一致，
所以可以赋值。后面我们又给它赋了一个新函数，而这个函数的参数类型和返回值类型都是 string 类型，
这时就会报如下错误：
```
不能将类型"(arg1: string, arg2: string) => string"分配给类型"(x: number, y: number) => number"。
  参数"arg1"和"x" 的类型不兼容。
    不能将类型"number"分配给类型"string"。
```
- **函数中如果使用了函数体之外定义的变量，这个变量的类型是不体现在函数类型定义的。**
#### 3)使用接口定义函数类型
我们在前面的小节中已经学习了接口，**使用接口可以清晰地定义函数类型**。
还拿上面的 add 函数为例，我们为它使用接口定义函数类型：
```
interface Add {
  (x:number, y:number):number;
}
let add:Add = (arg1:string,arg2:string):string => arg1 + arg2;
// error 不能将类型“(arg1: string, arg2: string) => string”分配给类型“Add”
```
- 这里我们**通过接口的形式定义函数类型**，这个接口Add定义了这个结构是一个函数，
两个参数类型都是number类型，返回值也是number类型。然后我们指定变量add类型为Add时，
再要给add赋值，就必须是一个函数，且参数类型和返回值类型都要满足接口Add，
显然例子中这个函数并不满足条件，所以报错了。
#### 4)使用类型别名
- 我们可以**使用类型别名来定义函数类型**，类型别名我们在后面讲到**高级类型**的时候还会讲到。
使用类型别名定义函数类型更直观易读，我们来看一下具体的写法:
```
type Add = (x:number, y:number) => number;
let add:Add = (arg1: string, arg2: string): string => arg1 + arg2; 
// error 不能将类型“(arg1: string, arg2: string) => string”分配给类型“Add”
```
使用**type**关键字可以**为原始值、联合类型、元组以及任何我们定义的类型**起一个别名。
上面定义了Add这个别名后，Add就成为了一个和(x: number, y: number) => number一致的**类型定义**。
例子中定义了Add类型，指定add类型为Add，但是给add赋的值并不满足Add类型要求，所以报错了。
### 2.参数
##### 1)可选参数
- TypeScript 会帮我们在编写代码的时候就**检查出调用函数时参数中存在的一些错误**，先看下面例子：
```
type Add = (x: number, y: number) => number;
let add: Add = (arg1: string, arg2: string): string => arg1 + arg2;

add(1, 2); // right
add(1, 2, 3); // error 应有 2 个参数，但获得 3 个
add(1); // error 应有 2 个参数，但获得 1 个
```
- 在 JS 中，上面例子中最后两个函数调用都不会报错, 只不过add(1, 2, 3)可以返回正确结果3，
add(1)会返回NaN。
- 但有时候，我们的函数有些参数不是必须的，是可选的。在学习接口的时候
我们学习过，可选参数只需在参数名后跟随一个?即可。但是**接口形式的定义**和今天学到的
**函数类型定义**有一点区别，那就是**参数位置**的要求：
```
接口形式定义的函数类型必选参数和可选参数的位置前后是无所谓的，
但是今天学到的定义形式，可选参数必须放在必选参数后面，这和在 JS 中定义函数是一致的。
```
来看下面的例子:
```
type Add = (x?: number, y: number) => number; // error 必选参数不能位于可选参数后。
```
- 在TypeScript中，**可选参数放到最后才行**，上面例子中把可选参数x放到了必选参数y前面，
所以报错了；但是在 JavaScript 中，其实是**没有可选参数这个概念**的，只不过是我们在写逻辑的时候，
我们可能会判断某个参数是否为undefined，如果是则说明调用该函数的时候没有传这个参数，
要做下兼容处理；而如果几个参数中，前面的参数是可不传的，后面的参数是需要传的，
就需要在该可不传的参数位置传入一个 undefined 占位才行。
#### 2)默认参数
- 在 ES6 标准出来之前，我们的默认参数实现起来比较繁琐：
```
//javascript
var count = 0;
function countUp(step){
  step = step || 1;
  count += step;
}
```
上面我们定义了一个计数器增值函数，这个函数有一个参数 step，即每次增加的步长，
如果不传入参数，那么 step 接受到的就是 undefined，undefined 转换为布尔值是 false，
所以step || 1这里取了 1，从而达到了不传参数默认 step === 1 的效果。
- 在 ES6 中，我们定义函数时给参数设默认值就很方便了，直接在参数后面使用**等号连接默认值**即可：
```
// javascript
const count = 0;
const countUp = (step = 1) => {
  count += step;
}
```
- 你会发现，可选参数和带默认值的参数在函数调用时都是可以不传实参的，
但是区别在于**定义函数的时候**，**可选参数**必须放在**必选参数后面**，
而**带默认值的参数**则可放在**必须参数前后**都可。
- 当我们为参数指定了默认参数的时候，TypeScript 会**识别默认参数的类型**；
当我们在调用函数时，如果给这个带默认值的参数传了别的类型的参数则会报错：
```
const add = (x: number, y = 2) => {
  return x + y;
};
add(1, "a"); // error 类型"string"的参数不能赋给类型"number"的参数
```
当然了，你也可以显式地给 y 设置类型：
```
const add = (x: number, y: number = 2) => {
  return x + y;
};
```
#### 3)剩余参数
- 在 JS 中，如果我们定义一个函数，这个函数可以输入任意个数的参数，
那么我们就无法在定义参数列表的时候挨个定义。在 ES6 发布之前，
我们**需要用到 arguments 来获取参数列表**。arguments 是每一个函数
都包含的一个**类数组对象**，**它包含在函数调用时传入函数的所有实际参数**（简称实参），
它还包含一个 length 属性，记录参数个数。来看下面的例子，我们来模拟实现函数的重载：
```
// javascript
function handleData() {
  if (arguments.length === 1) return arguments[0] * 2;
  else if (arguments.length === 2) return arguments[0] * arguments[1];
  else return Array.prototype.slice.apply(arguments).join("_");
}
handleData(2); // 4
handleData(2, 3); // 6
handleData(1, 2, 3, 4, 5); // '1_2_3_4_5'
// 这段代码如果在TypeScript环境中，三个对handleData函数的调用都会报错，因为handleData函数定义的时候没有参数。
```
- 上面这个函数通过判断传入实参的个数，做出不同的处理并返回结果。
else 后面的逻辑是如果实参个数不为 1 和 2，那么将这些参数拼接成以"_"连接的字符串。
>>> arguments类数组对象通过apply方法借用数组对象Array的slice方法
+ 你应该注意到了我们使用Array.prototype.slice.apply(arguments)对 arguments 做了处理，
前面我们讲过 arguments 不是数组，而是类数组对象，如果直接在 arguments 调用 join 方法，
它是没有这个方法的。所以我们通过这个处理得到一个包含 arguments 中所有元素的真实数组。
- 在 ES6 中，**加入"…"扩展运算符**，它可以将一个**函数或对象进行拆解**。
它还支持用在**函数的参数列表**中，用来**处理任意数量的参数**：
```
const handleData = (arg1, ...args) => {
  // 这里省略逻辑
  console.log(args);
};
handleData(1, 2, 3, 4, 5); // [ 2, 3, 4, 5 ]
```
- 可以看到，args 是除了 arg1 之外的所有实参的集合，它是一个数组。
+ 补充："…"扩展运算符可以**拆解数组和对象**，比如：arr1 = [1, 2]，arr2 = [3, 4]，
那么[…arr1, …arr2]的结果就是[1, 2, 3, 4]，他还可以用在方法的参数中：
如果使用 arr1.push(arr2)，则 arr1 结果是[1, 2, [3, 4]]，如果你想让他们
合并成一个数组而不使用 concat 方法，就可以使用 arr1.push(…arr2)。
还有对象的使用方法：obj1 = { a: ‘aa’ }，obj2 = { b: ‘bb’ }，
则{ …obj1, …obj2 }的结果是{ a: ‘aa’, b: ‘bb’ }。
- 在 TypeScript 中你可以**为剩余参数指定类型**，先来看例子：
```
const handleData = (arg1: number, ...args: number[]) => {
  //
};
handleData(1, "a"); // error 类型"string"的参数不能赋给类型"number"的参数
```
### 3.函数重载，此重载vs彼重载
在其他一些强类型语言中，**函数重载**是指**定义**几个**函数名相同，但参数个数或类型不同的函数**，
在**调用时**传入**不同的参数**，编译器会**自动调用适合的函数**。但是 
JavaScript 作为一个**动态语言**是**没有函数重载的**，
只能我们自己在函数体内通过**判断参数的个数、类型**来指定**不同的处理逻辑**。
来看个简单的例子：
```
const handleData = value => {
  if (typeof value === "string") {
    return value.split("");
  } else {
    return value
      .toString()
      .split("")
      .join("_");
  }
};
```
- 这个例子中，当传入的参数为字符串时，将它进行切割，比如传入的是’abc’，
返回的将是数组[‘a’, ‘b’, ‘c’]；如果传入的是一个数值类型，
则将数字转为字符串然后切割成单个数字然后拼接成字符串，比如传入的是123，
则返回的是’1_2_3’。你可以看到传入的参数类型不同，返回的值的类型是不同的，

+ 在 TypeScript 中有**函数重载**的概念，**但并不是**定义几个同名实体函数，
然后根据不同的参数个数或类型来自动调用相应的函数。TypeScript的函数重载是在
**类型系统层面**的，是为了更好地进行**类型推断**。
TypeScript的函数重载通过为一个函数**指定多个函数类型定义**，
从而**对函数调用的返回值进行检查**。来看例子：
```
function handleData(x: string): string[]; // 这个是重载的一部分，指定当参数类型为string时，返回值为string类型的元素构成的数组
function handleData(x: number): string; // 这个也是重载的一部分，指定当参数类型为number时，返回值类型为string
function handleData(x: any): any { // 这个就是重载的内容了，他是实体函数，不算做重载的部分
  if (typeof x === "string") {
    return x.split("");
  } else {
    return x
      .toString()
      .split("")
      .join("_");
  }
}
handleData("abc").join("_");
handleData(123).join("_"); // error 类型"string"上不存在属性"join"
handleData(false); // error 类型"boolean"的参数不能赋给类型"number"的参数。
```
- 首先我们使用function关键字定义了两个同名的函数，但不同的是，
函数没有实际的函数体逻辑，而是只定义**函数名、参数及参数类型以及函数的返回值类型**；
而第三个使用function定义的同名函数，是一个完整的**实体函数，包含函数名、参数及参数类型、返回值类型和函数体**；
**这三个定义组成了一个函数**——**完整的带有类型定义的函数**，前两个function定义的就称为函数重载，而第三个function并不算重载；
+ 然后我们来看下匹配规则，当调用这个函数并且传入参数的时候，会从上而下在函数重载里匹配和这个参数个数和类型匹配的重载。
如例子中第一个调用，传入了一个字符串"abc"，它符合第一个重载，所以它的返回值应该是一个字符串组成的数组，
数组是可以调用join方法的，所以这里没问题；
第二个调用传入的是一个数值类型的123，从上到下匹配重载是符合第二个的，返回值应该是字符串类型。但这里拿到返回值后调用了数组方法join，这肯定会报错了，因为字符串无法调用这个方法；
最后调用时传入了一个布尔类型值false，**匹配不到重载**，所以会报错；

最后还有一点要注意的是，这里**重载只能用 function 来定义**，不能使用**接口、类型别名**等。
### 本章小结
本小节我们学习了函数类型的三种定义方式：
- 基本方式：直接在定义函数实体语句中，指定参数和返回值类型；
- 接口形式：这种方式我们在讲接口的时候已经学习过了；
- 类型别名：这种方式是比较推荐的写法，比较简洁清晰。
我们还详细学习了函数参数的三个知识点：
- 可选参数：可选参数在JavaScript中可以实现，TypeScript中需要在该参数后面加个?，且**可选参数必须位于必选参数后面**；
- 默认参数：这是在ES6标准中添加的语法，为函数参数指定默认参数，写法就是在参数名后面使用=连接默认参数
- 剩余参数：这也是在ES6中添加的语法，可以使用...参数名来获取剩余任意多个参数，获取的是一个数组。
最后我们学习了**函数重载**。着重强调的是，这里的函数重载**区别于其他语言中的重载**，
TypeScript中的**重载**是为了**针对不同参数个数和类型**，**推断**返回值类型。
下个小节我们将学习**泛型**，来弥补你使用**any**时**丢失的类型校验**。
# 12.使用泛型拯救你的any
- 在前面的小节中我们学习了any类型，当我们要表示**一个值可以为任意类型的时候**，
则指定它的类型为any，比如下面这个例子：
```
const getArray = (value: any, times: number = 5): any[] => {
  return new Array(times).fill(value);
};
```
- 这个函数接受两个参数。第一个参数为**任意类型的值**，第二个参数为**数值类型的值**，
默认为 5。函数的功能是返回一个以 times 为元素个数，每个元素都是 value 的数组。
这个函数我们从逻辑上可以知道，传入的 value 是什么类型，那么返回的数组的每个元素也应该是什么类型。
接下来我们实际用一下这个函数：
```
getArray([1], 2).forEach(item => {
  console.log(item.length);
});
getArray(2, 3).forEach(item => {
  console.log(item.length);
});
```
- 我们调用了两次这个方法，使用 forEach 方法遍历得到的数组，在传入 forEach 的函数中
获取当前遍历到的数组元素的 length 属性。第一次调用这个方法是没问题的，因为我们第一次
传入的值为数组，得到的会是一个二维数组[ [1], [1] ]。每次遍历的元素为[1]，它也是数组，
所以打印它的 length 属性是可以的。而我们第二次传入的是一个数字 2，生成的数组是[2, 2, 2]，
访问 2 的 length 属性是没有的，所以应该报错，但是这里却不会报错，因为我们在定义getArray函数
的时候，指定了返回值是any类型的元素组成的数组，所以这里遍历其返回值中每一个元素的时候，
类型都是any，所以不管做任何操作都是可以的，因此，上面例子中第二次调用getArray的返回值
每个元素应该是数值类型，遍历这个数组时我们获取数值类型的length属性也没报错，
因为这里item的类型是any。

- 所以要解决这种情况，**泛型**就可以搞定，接下来我们来学习泛型。
### 1.简单使用
要解决上面这个场景的问题，就需要使用泛型了。
**泛型**（Generics）是指在**定义****函数、接口或类**的时候，**不预先指定具体的类型**，而在**使用的时候再指定类型**的一种特性。
还拿上面这个例子中的逻辑来举例，我们既要**允许传入任意类型的值**，
又要**正确指定返回值类型**，就要使用泛型。我们先来看怎么改写：
```
const getArray = <T>(value: T, times: number = 5): T[] => {
  return new Array(times).fill(value);
};
```
- 我们在**定义函数**之前，使用<>符号**定义**了一个**泛型变量 T**，这个 T 在这次函数定义中
就**代表某一种类型**，它可以是**基础类型**，也可以是**联合类型等高级类型**。定义了泛型变量之后，
你在函数中任何需要指定类型的地方使用 T 都代表这一种类型。比如当我们传入 value 的类型
为数值类型，那么返回的数组类型T[]就表示number[]。现在我们再来调用一下这个 getArray 函数：
```
getArray<number[]>([1, 2], 3).forEach(item => {
  console.log(item.length);
});
getArray<number>(2, 3).forEach(item => {
  console.log(item.length); // 类型“number”上不存在属性“length”
});
```
### 3.泛型变量
- 当我们**使用泛型**的时候，你必须在处理类型涉及到泛型的数据的时候，
**把这个数据当做任意类型**来处理。这就意味着**不是所有类型都能做的操作不能做**，
**不是所有类型都能调用的方法不能调用**。可能会有点绕口，我们来看个例子：
```
const getLength = <T>(param: T): number => {
  return param.length; // error 类型“T”上不存在属性“length”
};
```
- 当我们获取一个类型为泛型的变量 param 的 length 属性值时，如果 param 的类型
为数组 Array 或字符串 string 类型是没问题的，它们有 length 属性。但是如果此时
传入的 param 是数值 number 类型，那这里就会有问题了。
- 这里的T并不是固定的，你可以写为A、B或者其他名字，而且还可以在一个函数中**定义多个泛型变量**。
我们来看个复杂点的例子：
```
const getArray = <T, U>(param1: T, param2: U, times: number): [T, U][] => {
  return new Array(times).fill([param1, param2]);
};
getArray(1, "a", 3).forEach(item => {
  console.log(item[0].length); // error 类型“number”上不存在属性“length”
  console.log(item[1].toFixed(2)); // error 属性“toFixed”在类型“string”上不存在
});
```
- 这个例子中，我们**定义**了两个泛型变量T和U。第一个参数的类型为 T，第二个参数的类型为 U，
最后函数返回一个二维数组，函数返回类型我们指定是一个元素类型为[T, U]的数组。
所以当我们**调用**函数，最后遍历结果时，遍历到的每个元素都是一个第一个元素是数值类型、
第二个元素是字符串类型的数组。
### 4.泛型函数类型
- 我们可以定义一个**泛型函数类型**，还记得我们之前学习函数一节时，**给一个函数定义函数类型**，
现在我们可以使用**泛型定义函数类型**：
```
// ex1: 简单定义
const getArray: <T>(arg: T, times: number) => T[] = (arg, times) => {
  return new Array(times).fill(arg);
};
// =等号右边是一个匿名函数，赋值给左边的泛型函数定义
// ex2: 使用类型别名type
type GetArray = <T>(arg: T, times: number) => T[];
const getArray: GetArray = <T>(arg: T, times: number): T[] => {
  return new Array(times).fill(arg);
};
```
- 当然了，我们也可以使用**接口的形式**来定义**泛型函数类型**：
```
interface GetArray {
  <T>(arg: T, times: number): T[];
}
const getArray: GetArray = <T>(arg: T, times: number): T[] => {
  return new Array(times).fill(arg);
};
```
```***********************************************************
 <T>(arg: T, times: number): T[] 是调用签名或者说函数类型定义，而
 <T>(arg: T, times: number) => T[]是泛型函数定义的实现
```
- 你还可以把接口中**泛型变量提升到接口最外层**，这样接口中所有属性和方法都能使用这个泛型变量了。
我们先来看怎么用：
```
interface GetArray[T] {
  (arg:T,times:number):T[];
  tag:T;
}
const getArray: GetArray<number> = <T>(arg: T, times: number): T[] => {
  // error 不能将类型“{ <T>(arg: T, times: number): T[]; tag: string; }”分配给类型“GetArray<number>”。
  // 属性“tag”的类型不兼容。
  return new Array(times).fill(arg);
};
getArray.tag = "a"; // 不能将类型“"a"”分配给类型“number”
getArray("a", 1); // 不能将类型“"a"”分配给类型“number”
```
- 上面例子中将泛型变量定义在接口最外层，所以不仅函数的类型中可以使用 T，
在属性 tag 的定义中也可以使用。但在使用接口的时候，要在接口名后面明确传入一个类型，
也就是这里的GetArray<number>，那么后面的 arg 和 tag 的类型都得是 number 类型。
当然了，如果你还是希望 T 可以是任何类型，你可以把GetArray<number>换成GetArray<any>。
### 5.泛型约束
当我们使用了**泛型**时，就意味着这个这个类型是**任意类型**。但在大多数情况下，
我们的逻辑是**对特定类型处理的**。还记得我们前面讲泛型变量时举的那个例子——
当访问一个泛型类型的参数的 length 属性时，会报错"类型“T”上不存在属性“length”"，是因为并不是所有类型都有 length 属性。

所以我们在这里应该对 T 有要求，那就是类型为 T 的值应该包含 length 属性。
说到这个需求，你应该能想到接口的使用，我们可以**使用接口定义一个对象必须有哪些属性**：
```
interface ValueWithLength {
  length:number;
}
const v:ValueWithLength = {};
// error Property 'length' is missing in type '{}' but required in type 'ValueWithLength'
```
- **泛型约束**就是使用**一个类型**和**extends**对泛型进行**约束**，之前的例子就可以改为下面这样：
```
interface ValueWithLength {
  length:number;
}
const getLength = <T extends ValueWithLength>(param:T):number=>{
  return param.length;
}
getLength("abc"); // 3
getLength([1, 2, 3]); // 3
getLength({ length: 3 }); // 3
getLength(123); // error 类型“123”的参数不能赋给类型“ValueWithLength”的参数
```
- 这个例子中，**泛型变量T受到约束**。它必须**满足接口ValueWithLength**，也就是不管它是什么类型，但必须有一个**length属性，且类型为数值类型**。例子中后面四次调用getLength方法，传入了不同的值，传入字符串"abc"、数组[1, 2, 3]和一个包含length属性的对象{ length: 3 }都是可以的，但是传入数值123不行，因为它没有length属性。
### 6.在泛型约束中使用类型参数
- 当我们定义一个对象，想要对只能**访问对象上存在的属性**做要求时，该怎么办？先来看下这个需求是什么样子：
```
const getProps = (object, propName) => {
  return object[propName];
};
const obj = { a: "aa", b: "bb" };
getProps(obj, "c"); // undefined
```
- 当我们访问这个对象的’c’属性时，这个属性是没有的。这里我们需要用到**索引类型keyof**结合**泛型**来实现对这个问题的**检查**。**索引类型**在高级类型一节会详细讲解，这里你只要知道这个例子就可以了：
```
const getProp = <T, K extends keyof T>(object: T, propName: K) => {
  return object[propName];
};
const obj = { a: "aa", b: "bb" };
getProp(obj, "c"); // 类型“"c"”的参数不能赋给类型“"a" | "b"”的参数
```
+ 这里我们使用让**K来继承索引类型keyof T**，你可以理解为keyof T相当于**一个由泛型变量T的属性名构成的联合类型**，在这里 K 就被约束为了只能是"a"或"b"，所以当我们传入字符串"c"想要获取对象obj的属性"c"时就会报错。
---
### 本章小结
- 本小节我们学习了**泛型**的相关知识；学习了使用泛型来**弥补使用any造成的类型信息缺失**；当我们的类型是**灵活任意的**，又要**准确使用类型信息**时，就需要使用泛型来**关联类型信息**，其中离不开的是**泛型变量**；泛型变量可以是**多个**，且命名随意；如果需要对**泛型变量的类型**做进一步的限制，则需要用到我们最后讲的**泛型约束**；使用泛型约束通过**extends**关键字指定要**符合的类型**，从而满足更多场景的需求。

- 下个小节我们将学习**类**的知识，学习TypeScript中的类的知识之前，你需要先详细学习ES6标准中新增的类的知识，建议你先学习下阮一峰的《ECMAScript 6 入门》中类的部分。之所以要学习ES6中的类，是因为TypeScript中类的语法基本上是遵循ES6标准的，但是有一些区别，我们会在下个小节学习。
# TS中的类，小心它与ES标准的差异
- 虽然说**类**是 ES6 中新增的概念，但是在这里讲 TS 中的类，是因为在语法的实现上 TS 和 ES6 规范的，还是有点区别。在学习本节课之前，你要确定你已经详细学习了ES6标准的类的全部知识，如果没有学习，建议你先学习下阮一峰的《ECMAScript 6 入门》，学习完后再来学习本节课你会发现，一些同样的功能写法上却不同。
### 1.基础
- 类的所有知识我们已经在 ES6 中的类两个课时学过了，现在我们先来看下在 TS 中定义类的一个简单例子：
```
class Point {
  x: number;
  y: number;
  constructor(x:number,y:number){
    this.x = x;
    this.y = y;
  }
  getPosition(){
    return `(${this.x},${this.y})`
  }
  const point = new Point(1,2);
}
```
- 我们首先在定义类的代码块的顶部定义**两个实例属性**，并且指定类型为 number 类型。构造函数 constructor 需要传入两个参数，都是 number 类型，并且把这两个参数分别赋值给两个实例属性。最后定义了一个定义在**类的原型对象上**的方法 getPosition。

同样你也可以使用**继承来复用**一些特性：
```
class Parent {
  name:string;
  constructor(name:string){
    this.name = name;
  }
}
class Child extends Parent{
  constructor(name:string){
    super(name);//调用父类的构造函数
  }
}
```
- 这些和 ES6 标准中的类没什么区别，如果大家不了解ES6标准中类关于这块的内容，建议大家先去学习ES6类的知识。
### 2.修饰符
- 在 ES6 标准类的定义中，默认情况下，**定义在实例的属性和方法会在创建实例后添加到实例上**；而如果是**定义在类里没有定义在 this 上的方法，实例可以继承这个方法(原型方法)**；而如果使用 static 修饰符定义的属性和方法，是**静态属性和静态方法**，实例是没法访问和继承到的；我们还通过一些手段，实现了私有方法，但是私有属性的实现还不好实现。

接下来我们来看下 TS 中的**公共、私有和受保护**的修饰符：
#### 1) public
- **public表示公共的**，用来指定在创建实例后可以通过实例访问的，也就是**类定义的外部可以访问的属性和方法**。默认是 public，但是 TSLint 可能会要求你必须用修饰符来表明这个属性或方法是什么类型的。
```
class Point{
  public x:number;
  public y:number;
  constructor(x:number,y:number){
    this.x = x;
    this.y = y;
  }
  public getPosition(){
    return `(${this.x},${this.y})`;
  }
}
```
#### 2)private
- **private修饰符表示私有的，它修饰的属性在类的定义外面是没法访问的**:
```
class Parent {
  private age:number;
  constructor(age:number){
    this.age = age;
  }
}
const p = new Parent(18);
console.log(p); //{age:18}
console.log(p.age);//error 属性"age"为私有属性，只能在类"parent"中访问
console.log(Parent.age);//error 类型“typeof ParentA”上不存在属性“age”
class Child extends Parent {
  constructor(age: number) {
    super(age);
    console.log(super.age); // 通过 "super" 关键字只能访问基类的公共方法和受保护方法
  }
}
```
- 这里你可以看到，age 属性使用 **private** 修饰符修饰，说明他是**私有属性**，我们打印创建的实例对象 p，发现他是有属性 age 的，但是当试图访问 p 的 age 属性时，编译器会报错，告诉我们私有属性只能**在类 Parent 中访问**。

这里我们需要特别说下 super.age 这里的报错，我们在之前学习 ES6 的类的时候，讲过在不同类型的方法里 super 作为对象代表着不同的含义，这里在 constructor 中访问 super，这的 super 相当于父类本身，这里我们看到使用 private 修饰的属性，在子类中是没法访问的。
#### 3)protected
- **protected**修饰符是**受保护修饰符**，和private有些相似，但有一点不同，**protected修饰的成员在继承该类的子类中可以访问**，我们再来看下上面那个例子，把父类 Parent 的 age 属性的修饰符 private 替换为 protected：
```
class Parent {
  protected age: number;
  constructor(age: number) {
    this.age = age;
  }
  protected getAge() {
    return this.age;
  }
}
const p = new Parent(18);
console.log(p.age); // error 属性“age”为私有属性，只能在类“ParentA”中访问
console.log(Parent.age); // error 类型“typeof ParentA”上不存在属性“age”
class Child extends Parent {
  constructor(age: number) {
    super(age);
    console.log(super.age); // undefined
    console.log(super.getAge());
  }
}
new Child(18)
```
- protected还能用来**修饰 constructor 构造函数**，加了protected修饰符之后，这个类就**不能再用来创建实例**，只能**被子类继承**，这个需求我们在讲 ES6 的类的时候讲过，需要用new.target来自行判断，而 TS 则只需用 protected 修饰符即可：
```
class Parent {
  protected constructor() {
    //
  }
}
const p = new Parent(); // error 类“Parent”的构造函数是受保护的，仅可在类声明中访问
class Child extends Parent {
  constructor() {
    super();
  }
}
const c = new Child();
```
#### 4) readonly修饰符
- 在**类**里可以使用**readonly**关键字将**属性**设置为**只读**。
```
class UserInfo {
  readonly name:string;
  constructor(name:string){
    this.name = name;
  }
}
const user = new UserInfo("Lison");
user.name = "haha";
// error Cannot assign to 'name' because it is a read-only property
```
- 设置为只读的属性，实例只能读取这个属性值，但不能修改。
### 5.参数属性
- 之前的例子中，我们都是在类的定义的顶部**初始化实例属性**，在 constructor 里**接收参数**然后对实力属性进行**赋值**，我们可以使用**参数属性**来**简化**这个过程。参数属性简单来说就是在 constructor **构造函数的参数**前面加上**访问限定符**，也就是前面讲的 public、private、protected 和 readonly 中的任意一个，我们来看例子：
```
class A{
  constructor(name:string){}
}
const a = new A("aaa");
console.log(a.name); // error 类型“A”上不存在属性“name”

class B{
  constructor(public name:string) {}
}
const b = new B("bbb");
console.log(b.name);//"bbb"
```
- 可以看到，在定义类 B 时，构造函数有一个参数 name，这个 name 使用访问修饰符 public 修饰，此时即为 **name 声明了参数属性**，也就无需再显示地在类中初始化这个属性了。
### 6.静态属性
- 和 ES6 的类一样，在 TS 中一样使用**static**关键字来指定**属性或方法是静态的**，实例将
**不会添加这个静态属性**，也**不会继承这个静态方法**，你可以使用修饰符和 static 关键字来指定一个属性或方法：
```
class Parent {
  public static age:number = 18;
  public static getAge(){
    return Parent.age;
  }
  constructor(){
    //
  }
}
const p = new Parent();
console.log(p.age);// error  Property 'age' is a static member of type 'Parent'
console.log(Parent.age);// 18 
```
- **静态属性只能通过类本身来访问**
- 如果使用了private修饰道理和之前的一样:
```
class Parent {
  public static getAge() {
    return Parent.age;
  }
  private static age: number = 18;
  constructor() {
    //
  }
}
const p = new Parent();
console.log(p.age); // error Property 'age' is a static member of type 'Parent'
console.log(Parent.age); // error 属性“age”为私有属性，只能在类“Parent”中访问。
```
---
- public static age:number = 18; 
1. public,private,protected是访问修饰符(代表访问权限)
2. staic是静态修饰符(static是控制属性为实例属性还是类的静态属性)
3. 非static的称为实例属性，static修饰的称为类静态属性，区别在于不用static修饰的属性，每次new都会生成一个，绑定在实例上
---
### 7.可选类属性
- TS 在 2.0 版本，支持**可选类属性**，也是使用?符号来标记，来看例子：
```typescript
class Info {
  name:string;
  age?:number;
  constructor(name:string,age?:number,public sex?:string){
    this.name = name;
    this.age = age;
  }
}
const info1 = new Info("lison");
const info2 = new Info("lison",18);
const info3 = new Info("lison",18,"man");
```
### 8.存取器
- 这个也就 ES6 标准中的**存值函数**和**取值函数**，也就是在**设置属性值**的时候调用的函数，和在**访问属性值**的时候调用的函数，用法和写法和 ES6 的没有区别：
```javascript
class UserInfo{
  private _fullName: string;
  constructor(){}
  get fullName(){
    return this._fullName;
  }
  set fullName(value){
    console.log(`setter:${value}`);
    this._fullName = value;
  }
}
const user = new UserInfo();
user.fullName = "Lison Li"; //"setter: Lison Li"
console.log(user.fullName); //"Lison Li"
```
### 9. 抽象类
- **抽象类一般用来被其他类继承**，而**不直接用它创建实例**。抽象类和类内部定义抽象方法，使用**abstract**关键字，我们先来看个例子：
```javascript
abstract class People{
 	constructor(public name:string) {}
  abstract printName():void;
}
// People抽象类只能被其他类继承
class Man extends People{
 constructor(name:string){
 	super(name);
 	this.name = name;
 }
 printName(){
 console.log(this.name);
 }
}
const m = new Man();//error 应有 1 个参数，但获得 0 个
const man = new Man("lison");
man.printName();//'lison'
const p = new People("lison");// error 无法创建抽象类的实例
```

- 上面例子中我们定义了一个**抽象类 People**，在抽象类里我们定义 constructor 方法必须传入一个字符串类型参数，并把这个 name 参数值绑定在创建的实例上；使用`abstract`关键字定义一个**抽象方法 printName**，这个定义可以指定参数，指定参数类型，指定返回类型。当我们直接使用抽象类 People 实例化的时候，就会报错，我们只能创建一个继承抽象类的子类，使用子类来实例化。

  我们再来看个例子:

  ```typescript
  abstract class People{
    constructor(public name:string) {}
    abstract printName():void;
  }
  class Man extends People{
    //error 非抽象类"Man"不会实现继承自"People"类的抽象成员"printName"
    constructor(name:string){
      super(name)；
      this.name = name;
    }
  }
  const m = new Man("lison");
  m.printName();//error m.printName is not a function
  ```

  - 通过上面的例子我们可以看到，**在抽象类里定义的抽象方法，在子类是不会继承的**，所以在子类中必须实现该方法的定义。

    - 2.0版本开始，abstract关键字不仅可以标记类和类里面的方法，还可以标记类中定义的**属性和存取器**:

      ```typescript
      abstract class People{
        abstract _name:string;
        abstract get insideName():string;
        abstract set insideName(value:string);
      }
      class Pp extends People{
        _name:string;
        insideName:string;
      }
      ```

      - 但是要记住，抽象方法和抽象存取器都不能包含实际的代码块。

### 实例属性

- **当我们定义一个类，并创建实例后，这个实例的类型就是创建他的类**:

  ```typescript
  class People {
    constructor (public name:string) {}
  }
  let p:People = new People("lison");
  ```

  - 当然了，创建实例的时候这**指定 p 的类型为 People 并不是必须的**，TS 会推断出他的类型。虽然指定了类型，但是当我们再定义一个和 People 类同样实现的类 Animal，并且创建实例赋值给 p 的时候，是没有问题的：

    ```typescript
    class Animal{
      constructor (public name:string) {}
    }
    let p = new Animal("lark");
    ```

    - 所以，如果你想实现对创建实例的类的判断，还是需要用到**instanceof**关键字
### 对前面跳过知识的补充
- 现在我们把之前因为没有学习类的使用，所以暂时跳过的内容补回来。
#### 1.类类型接口 (使用接口可以强制一个类的定义必须包含某些内容)
- 使用**接口**可以强制一个**类的定义**必须包含某些内容，先来看个例子:
```typescript
interface FoodInterface{
  type:sting;
}
class FoolClass implements FoodInterface{
  // error Property 'type' is missing in type 'FoodClass' but required in type 'FoodInterface'
  static type:string;
  constructor() {}
}
```
- 上面接口 FoodInterface 要求使用该接口的值必须有一个 type 属性，定义的类 FoodClass 要使用接口，需要使用关键字**implements**。implements关键字用来**指定一个类要继承的接口**，如果是**接口和接口、类和类直接的继承**，使用**extends**，如果是**类继承接口**，则用**implements**。

有一点需要注意，**接口检测的是使用该接口定义的类创建的实例**，所以上面例子中虽然定义了**静态属性 type**，但静态属性不会添加到实例上，所以还是报错，所以我们可以这样改：
```typescript
interface FoodInterface{
  type:string;
}
class FoodClass implements FoodInterface{
  constructor(public type:string){}
}
```
- 上面使用到了**参数属性**
- 当然这个需求你也可以使用本节课学习的抽象类实现:
```typescript
abstract class FoodAbstractClass{
  abstract type:string;
}
class Food extends FoodAbstractClass{
  constructor (public type:string){
    super();
  }
}
```
- **TS中的抽象类的抽象属性和抽象方法必须在继承它的类中定义和实现**
#### 2.接口继承类 (1.接口可以继承一个类，当接口继承了该类后，会继承类的成员，但是不包括其实现，也就是只继承成员以及成员类型 2.接口还回继承类的private和protected修饰的成员，当接口继承的的这个类中包含这两个修饰符修饰的成员时，这个接口只可被这个类或他的类实现implements)
- **接口可以继承一个类，当接口继承了该类后，会继承类的成员，但是不包括其实现，也就是只继承成员以及成员类型**。**接口**还会**继承类的private和protected修饰的成员**，当接口继承的这个类中**包含**这两个修饰符修饰**的成员时**，这个接口只可**被这个类或他的子类实现**。
```typescript
class A{
  protected name:string;
}
interface I extends A{}
class B implements I{} 
//error  Property 'name' is missing in type 'B' but required in type 'I'
class C implements I{
  //error 属性"name"受保护，但类型"C"并不是"A"派生的类
  name:string;
}
class D extends A implements I{
  getName(){
    // 被protected修饰的name属性只可以在子类
    return this.name;
  }
}
```
- **ES里面还没有实现私有属性，但是TS里面实现了，关键字private**
#### 3.在泛型中使用 类类型
这里我们先来看个例子:
```typescript
const create = <T>(c: { new (): T }): T => {
  return new c();
};
class Info {
  age: number;
}
create(Info).age;
create(Info).name; // error 类型“Info”上不存在属性“name”
```
- 在这个例子里，我们创建了一个 create 函数，传入的参数是一个**类**，返回的是一个**类创建的实例**，这里有几个点要讲：

参数 c 的**类型定义**中，new()代表调用类的构造函数，他的类型也就是类创建实例后的实例的类型。
return new c()这里使用传进来的类 c 创建一个实例并返回，返回的**实例类型**也就是**函数的返回值类型**。
所以通过这个定义，TS 就知道，调用 create 函数，&&传入的和返回的值**都应该是**同一个类类型**。
### 本章小结
- 本小节我们详细学习了**类**的知识，因为TypeScript中类的概念是**遵循ES6标准的同时，添加了新语法的**，所以学习完本小节后，你应该记住ES6标准和TypeScript中**类的区别**，**避免在纯JavaScript中使用了TypeScript的语法**。我们学习了三个类的修饰符：

public：**公有属性方法修饰符**，这是**默认修饰符**；
private：私有修饰符，它修饰的属性**在类的定义外面无法访问**；
protected：和private相似，区别在于他修饰的成员**在继承该类的子类中可以访问**。
还有一个**readonly**修饰符，他在讲前面知识的时候就遇到过，**只读修饰符**。我们还学习了如何使用**参数属性**来**简化实例属性的初始化过程**，还有使用定义**函数可选参数**同样的方式来定义**构造函数可选参数**。我们学习了如何定义**抽象类**，使用**abstract**关键字修饰类定义，抽象类一般**用来被其他类继承，而不直接用它创建实例**。我们还学习了，**类既是值，也是类型**，当我们**使用类创建一个实例的时候，这个实例的类型也就是这个创建这个实例的类**。最后我们对前面讲接口和泛型时涉及到类跳过的知识进行补充讲解：**类类型接口、接口继承类和在泛型中使用类类型**。

学习完本小节后，第二章基础部分就学习完了，这部分知识是学习后面章节的重要基础，所以你一定要多看多练多理解，下一章我们开始学习进阶部分，别掉队哦。
# 类型推论，看TS有多懂你
- 在学习基础部分的章节时，我们讲过，在一些定义中如果你**没有明确指定类型**，编译器会**自动推断出适合的类型**；比如下面的这个简单例子：
  ```typescript
  let name = "lison";
  name = 123;// error 不能讲类型"123分配给类型"string"
  ```
- 我们看到，在定义变量 name 的时候我们并没有指定 name 的类型，而是直接给它赋一个字符串。当我们再给 name 赋一个数值的时候，就会报错。在这里，TypeScript 根据我们赋给 name 的值的类型，推断出我们的 name 的类型，这里是 string 类型，当我们再给 string 类型的 name 赋其他类型值的时候就会报错。

这个是最基本的类型推论，**根据右侧的值推断左侧变量的类型**，接下来我们看两个更复杂的推论。
### 1.多类型联合
- 当我们定义一个数组或元组这种包含多个元素的值的时候，多个元素可以有不同的类型，这种时候 TypeScript 会**将多个类型合并起来**，组成一个**联合类型**，来看例子：
  ```typescript
  let arr = [1,"a"];
  arr = ["b",2,false];//error 不能将类型“false”分配给类型“string | number”
  ```
- 可以看到，此时的 arr 的元素被推断为**string | number**，也就是元素可以是 string 类型也可以是 number 类型，除此两种类型外的类型是不可以的。再来看个例子：
  ```typescript
  let value = Math.random() * 10 > 5 ? 'abc' : 123
  value = false // error 不能将类型“false”分配给类型“string | number”
  ```
- 这里我们给value赋值为一个三元操作符表达式，Math.random() * 10的值为0-10的随机数。这里判断，如果这个随机值大于5，则赋给value的值为字符串’abc’，否则为数值123，所以最后编译器推断出的类型为**联合类型string | number**，当给它再赋值为false的时候就会报错。
### 2.上下文类型
我们上面讲的两个例子都是根据**=符号右边值的类型，推断左侧值的类型**。现在要讲的**上下文类型**则相反，它是**根据左侧的类型推断右侧的一些类型**，先来看例子：
```typescript
window.onmousedown = function(mouseEvent) {
  console.log(mouseEvent.a); // error 类型“MouseEvent”上不存在属性“a”
}
```
- 我们可以看到，表达式左侧是 window.onmousedown(鼠标按下时发生事件)，因此 TypeScript 会**推断赋值表达式右侧函数的参数是事件对象**，因为左侧是 mousedown 事件，所以 TypeScript 推断 mouseEvent 的类型是 MouseEvent。在回调函数中使用 mouseEvent 的时候，你可以访问鼠标事件对象的所有属性和方法，当访问不存在属性的时候，就会报错。

以上便是我要讲的三种常见的类型推论。在我们日常开发中，必写的类型还是要明确指定的，这样我们才能更准确地得到类型信息和开发辅助。
### 本章小结
本小节我们学习了TypeScript编译器进行类型推断的论据，其中有**两种**是**由右推左**的，也就是在**赋值时根据右侧要赋的具体值，推断左侧要赋值的目标的类型**，包括**基本推论**和**多类型联合推论**。基础推论是最基础的推论，**多类型联合推论**是根据**数组、代码逻辑**等，推断出多个符合的类型，然后组成联合类型的推论。还有一种**由左推右**的推论，我们是通过给元素绑定事件来讲解的，根据左侧要赋值的目标，来推断出右侧要赋的值中的一些类型信息。

下个小节我们将学习**类型兼容性**，我们知道JavaScript是灵活的，所以TypeScript通过**类型兼容性来满足它的灵活特点**，下个小节我们将介绍多种情况的兼容性表现。
# 类型兼容性，开放心态满足灵活的JS
- 我们知道JavaScript是**弱类型语言**，它**对类型是弱校验**，正因为这个特点，所以才有了TypeScript这个**强类型语言系统**的出现，来**弥补类型检查的短板**。TypeScript在**实现类型强校验的同时，还要满足JavaScript灵活**的特点，所以就有了**类型兼容性**这个概念。本小节我们就来全面学习一下TypeScript的类型兼容性。
### 1.函数兼容性
函数的兼容性简单总结就是如下六点:
#### 1)函数参数个数
- 函数参数个数如果要兼容，需要满足一个要求: **如果对函数y进行赋值，那么要求x中的每个参数都应在y中有对应，也就是x的参数个数小于等于y的参数个数**，来看例子:
```typescript
let x = (a:number) => 0;
let y = (b:number,c:string) => 0;
```
上面定义的两个函数，如果进行赋值的话，来看下两种情况的结果:
```typescript
y = x; //没问题，x对y赋值，参数个数 小于等于 y的参数个数
```
- 将 x 赋值给 y 是可以的，因为如果对函数 y 进行赋值，那么要求 x 中的每个参数都应在 y 中有对应，也就是 x 的参数个数**小于等于** y 的参数个数，而至于**参数名是否相同**是无所谓的。
```typescript
x = y;
// error Type '(b: number, s: string) => number' is not assignable to type '(a: number) => number'
```
这个例子中，y 要赋值给 x，但是 y 的**参数个数要大于** x，所以报错。
这可能不好理解，我们用另一个例子来解释下:
```typescript
const arr = [1,2,3];
arr.forEach((item,index,array)=>{
  console.log(item);
})
arr.forEach(item=>{
  console.log(item);
})
```
- 这个例子中，传给 forEach 的回调函数的参数是三个，但是可以只用一个，这样就只需写一个参数。我们传入的 forEach 的函数是 forEach 的参数，它是一个函数，这个函数的参数列表是定义在 forEach 方法内的，我们可以传入一个参数少于等于参数列表的函数，但是不能传入一个比参数列表参数个数还多的函数。
#### 2.函数参数类型(这一章有疑问)
除了参数个数，参数的类型需要对应:
```typescript
let x = (a:number) => 0;
let y = (b:number) => 0;
let z = (c:number) => false;
x = y; //error 不能将类型“(b: string) => number”分配给类型“(a: number) => number”。
x = z; // error 不能将类型“(c: string) => boolean”分配给类型“(a: number) => number”
```
- 我们看到 x 和 y 两个函数的**参数个数和返回值都相同**，只是**参数类型对不上**，所以也是不行的。
如果函数 z 想要赋值给 x，要求 y 的返回值类型必须是 x 的返回值类型的子类型，这个例子中 x 函数的返回值是**联合类型**，也就是返回值既可以是 string 类型也可以是 number 类型。而 y 的返回值类型是 number 类型，参数个数和类型也没问题，所以可以赋值给 x。而 z 的返回值类型 false 并不是 string 也不是 number，所以不能赋值。
#### 3.剩余参数和可选参数
- **当要被赋值的函数参数中包含剩余参数（…args）时，赋值的函数可以用任意个数参数代替，但是类型需要对应**。来看例子：
```typescript
const getNum = ( // 这里定义一个getNum函数，他有两个参数
  arr: number[], // 第一个参数是一个数组
  callback: (...args: number[]) => number // 第二个参数是一个函数，这个函数的类型要求可以传入任意多个参数，但是类型必须是数值类型，返回值必须是数值类型
): number => {
  return callback(...arr); // 这个getNum函数直接返回调用传入的第二个参数这个函数，以第一个参数这个数组作为参数的函数返回值
};
getNum(
  [1, 2],
  (...args: number[]): number => args.length // 这里传入一个函数，逻辑是返回参数的个数
);
```
- **剩余参数**其实可以看做**无数个可选参数**，所以在兼容性方面是差不多的，我们来看个**可选参数**和**剩余参数**结合的例子：
```typescript
const getNum = (
  arr: number[],
  callback: (arg1: number, arg2?: number) => number // 这里指定第二个参数callback是一个函数，函数的第二个参数为可选参数
): number => {
  return callback(...arr); // error 应有 1-2 个参数，但获得的数量大于等于 0
};
```
- 这里因为arr可能为空数组或不为空，如果为空数组则…arr不会给callback传入任何实际参数，所以这里报错。如果我们换成return callback(arr[0], …arr)就没问题了。
#### 4.函数参数双向协变
- **函数参数双向协变**即**参数类型无需绝对相同**，来看个例子：
  ```typescript
  let funcA = function(arg:number | string): void{};
  let funcB = function(arg:number):void{};
  // funcA = funcB 和funcB = funcA都可以
  ```
- 在这个例子中，funcA 和 funcB 的参数类型并不完全一样，funcA 的参数类型为一个联合类型 number | string，而 funcB 的参数类型为 number | string 中的 number，他们两个函数也是兼容的。
- 注：要允许双向协变兼容，需要配置tsconfig.json文件的"strictFunctionTypes"选项为false，默认为false，但是如果你设置了"strict"为true，需要显式设置"strictFunctionTypes"为false。
#### 5.函数返回值类型
```typescript
let x = (a: number): string | number => 0;
let y = (b: number) => "a";
let z = (c: number) => false;
x = y;
x = z; // 不能将类型“(c: number) => boolean”分配给类型“(a: number) => string | number”
```
#### 6.函数重载
- 带有**重载**的函数，要求**被赋值的函数的每个重载**都能在**用来赋值的函数**上找到对应的**签名**，来看例子：
```typescript
function merge(arg1: number, arg2: number): number; // 这是merge函数重载的一部分
function merge(arg1: string, arg2: string): string; // 这也是merge函数重载的一部分
function merge(arg1: any, arg2: any) { // 这是merge函数实体
  return arg1 + arg2;
}
function sum(arg1: number, arg2: number): number; // 这是sum函数重载的一部分
function sum(arg1: any, arg2: any): any { // 这是sum函数实体
  return arg1 + arg2;
}
let func = merge;
func = sum; // error 不能将类型“(arg1: number, arg2: number) => number”分配给类型“{ (arg1: number, arg2: number): number; (arg1: string, arg2: string): string; }”
```
- 上面例子中，sum函数的重载**缺少参数都为string返回值为string的情况**，与merge函数不兼容，所以赋值时会报错。
### 2.枚举
- **数字枚举成员类型**与**数字类型**互相兼容，来看例子：
```typescript
enum Status{
  On,
  Off
}
let s = Status.On;
s = 1;
s = 3;
```
- 虽然Status.On的值是0，但是这里数字枚举成员类型和数值类型互相兼容，所以这里给s赋值为3也没问题。
但是不同枚举值之间是不兼容的:
```typescript
enum Status {
  On,
  Off
}
enum Color{
  White,
  Black
}
let s = Status.On;
s = Color.White;// error Type 'Color.White' is not assignable to type 'Status'
```
- 可以看到，虽然 Status.On 和 Color.White 的值都是 0，但它们是不兼容的。
- **字符串枚举成员类型**和**字符串类型**是不兼容的，来看例子：
```typescript
enum Status{
  On = 'on',
  Off = 'off'
}
let s = Status.On
s = 'Lison'// error 不能将类型“"Lison"”分配给类型“Status”
```
- 这里会报错，因为**字符串字面量类型'Lison'**和**Status.On字符串枚举成员类型**是不兼容的。
### 3.类
基本比较
- 比较**两个类类型的值的兼容性**时，**只比较实例的成员，类的静态成员和构造函数不进行比较**：
```typescript
class Animal{
  static age:number;
  constructor (public name:string) {}
}
class People{
  static age:string;
  constructor (public name:string) {}
}
class Food{
  constructor (public name:number) {}
}
let a:Animal;
let p:People;
let f:Food;
a = p;// right
a = f;// error Type 'Food' is not assignable to type 'Animal'
```
- 上面例子中，Animal类和People类都有一个**age静态属性**，它们都定义了**实例属性name**，且name的类型都是string。我们看到把类型为People的p赋值给类型为Animal的a没有问题，因为我们讲了，类类型比较兼容性时，**只比较实例的成员**，这两个变量虽然类型是不同的类类型，但是它们都有相同字段和类型的实例属性name，**而类的静态成员是不影响兼容性的**，所以它俩兼容。而类Food定义了一个实例属性name，类型为number，所以类型为Food的f与类型为Animal的a类型不兼容，不能赋值。
- 类的私有成员和受保护成员
- **类的私有成员和受保护成员**会影响兼容性。当检查**类的实例兼容性**时，如果目标（也就是要被赋值的那个值）类型（这里实例类型就是创建它的类）包含一个私有成员，那么源（也就是用来赋值的值）类型必须包含来自同一个类的这个私有成员，这就允许子类赋值给父类。先来看例子：
```typescript
class Parent {
  private age: number;
  constructor() {}
}
class Children extends Parent {
  constructor() {
    super();
  }
}
class Other {
  private age: number;
  constructor() {}
}

const children: Parent = new Children();
const other: Parent = new Other(); // 不能将类型“Other”分配给类型“Parent”。类型具有私有属性“age”的单独声明
```
- 可以看到，当指定 other 为 Parent 类类型，给 other 赋值 Other 创建的实例的时候，会报错。因为 Parent 的 age 属性是**私有成员**，外界是无法访问到的，所以会类型不兼容。而children的类型我们指定为了Parent类类型，然后给它赋值为Children类的实例，没有问题，是因为Children类继承Parent类，且实例属性没有差异，Parent类有私有属性age，但是因为Children类继承了Parent类，所以可以赋值。
- 同样，使用 **protected 受保护修饰符修饰**的属性，也是一样的。
```typescript
class Parent {
  protected age: number;
  constructor() {}
}
class Children extends Parent {
  constructor() {
    super();
  }
}
class Other {
  protected age: number;
  constructor() {}
}
const children: Parent = new Children();
const other: Parent = new Other(); // 不能将类型“Other”分配给类型“Parent”。属性“age”受保护，但类型“Other”并不是从“Parent”派生的类
```
### 4.泛型
- 泛型包含类型参数，这个类型参数可能是任意类型，使用时类型参数会被指定为特定的类型，而这个类型只影响使用了类型参数的部分。来看例子：
```typescript
interface Data<T> {}
let data1: Data<number>;
let data2: Data<string>;

data1 = data2;
```
- 在这个例子中，data1 和 data2 都是 Data 接口的实现，但是**指定的泛型参数的类型**不同，TS 是**结构性类型系统**，所以上面将 data2 赋值给 data1 是兼容的，因为 data2 指定了类型参数为 string 类型，但是接口里没有用到参数 T，所以传入 string 类型还是传入 number 类型并没有影响。我们再来举个例子看下：
```typescript
interface Data<T> {
  data: T;
}
let data1: Data<number>;
let data2: Data<string>;

data1 = data2; // error 不能将类型“Data<string>”分配给类型“Data<number>”。不能将类型“string”分配给类型“number”
```
- 现在结果就不一样了，赋值时报错，因为 data1 和 data2 传入的泛型参数类型不同，生成的结果结构是不兼容的。
### 本章小结
本小节我们学习了TypeScript的**类型兼容性**，学习了**各种情况下赋值的可行性**。这里面函数的兼容性最为复杂，能够影响函数兼容性的因素有：
- **函数参数个数**： 如果对函数 y 进行赋值，那么要求 x 中的每个参数都应在 y 中有对应，也就是 x 的**参数个数小于等于** y 的参数个数；
- **函数参数类型**： 这一点其实和基本的赋值兼容性没差别，只不过比较的不是变量之间而是参数之间；
- **剩余参数和可选参数**： 当要被赋值的函数参数中包含剩余参数（…args）时，赋值的函数可以用任意个数参数代替，但是类型需要对应，可选参数效果相似；
- **函数参数双向协变**： 即参数类型无需绝对相同；
- **函数返回值类型**： 这一点和函数参数类型的兼容性差不多，都是基础的类型比较；
- **函数重载**： 要求被赋值的函数每个重载都能在用来赋值的函数上找到对应的签名。

**枚举**较为简单，**数字枚举成员类型与数值类型兼容**，**字符串枚举成员与字符串类型不兼容**。类的兼容性比较的主要依据是 **实例成员**，但是 **私有成员**和 **受保护成员**也会影响兼容性。最后是涉及到 **泛型的类型兼容性**，一定要记住一点的是 **使用时指定的特定类型**只会影响使用了 **类型参数的部分**。

下个小节我们学习 **类型保护**，还记得前面讲TS中补充的六个类型和类型断言的时候，都提到过类型保护，**使用类型保护，可以明确告诉编译器某个值是某种类型**，虽然听起来 **和类型断言一样**，但是它要 **比类型断言更便捷**，我们下节课来进行详细学习。
# 使用类型保护让TS更聪明
