# 对象类别
* **普通(Ordinary)对象** 具有JavaScript对象所有的默认内部行为
* **特异(Exotic)对象** 具有某些与默认行为不符的内部行为
* **标准(Standard)对象** ECMAScript 6规范中定义的对象，例如Array、Date等。标准对象既可以是普通对象，也可以是特异对象。
* **内建对象** 脚本开始执行时存在于JavaScript执行环境中的对象，所有标准对象都是内建对象。

# 对象字面量语法扩展
## 属性初始值的简写
``` JavaScript
let obj = { name: name }; 
```
可简写为
``` JavaScript
let obj = { name };
```
## 对象方法的简写语法

``` JavaScript
let person = {
    name: 'Anker',
    sayName: function() {
        console.log(this.name);
    }
 }
```
 可简写为
``` JavaScript
let person = {
    name: 'Anker',
    sayName() {
        console.log(this.name);
    }
 }
```
简写方法可以使用`super`关键字

<font color="red" size="3">注意：</font>通过对象方法简写语法创建的方法有一个name属性，其值为小括号前的名称。在上述示例中，person.sayName()方法的name属性的值为"sayName"。

## 可计算属性名(Computed Property Name)
在ES6之前，如果想要通过计算得到属性名，就需要用方括号代替点记法
``` JavaScript
var person = {}, lastName = 'last name';
person['first name'] = 'Anker';
person[lastName] = 'Xiao';
console.log(person['first name']);
console.log(person[lastName]);
```
在对象字面量中，可以直接使用字符串字面量作为属性名称
``` JavaScript
var person = {
    'first name': 'Anker'
};
console.log(person['first name']);
console.log(person[lastName]);
```
ES6中可在对象字面量中使用可计算属性名称，其语法与引用对象实例的可计算属性名称相同，也是使用方括号
``` JavaScript
let lastName = 'last name';
let person = {
    'first name': 'Anker',
    [lastName]: 'Xiao'
}
console.log(person['first name']);
console.log(person[lastName]);
```

# 新增方法
## Object.is()方法
此方法接受两个参数，这两个参数类型相同且值也相同，则返回ture，否则返回false
\
`+0 === -0` 为`true`和 `NaN === NaN` 为`false`

而用此方法，则`Object.is(+0, -0)`为`false`，`Object.is(NaN, NaN)`为`true`

## Object.assign()方法
合并两个或多个对象并返回一个新对象，此方法可以接受任意数量的源对象，并按指定的顺序将属性复制到接收对象中。所以如果多个源对象具有同名属性，则排位靠后的源对象会覆盖排位靠前的
``` JavaScript
var receiver = {};
Object.assign(receiver, {
    type: 'js',
    name: 'file.js'
}, {
    type: 'css'
});
console.log(receiver.type); //css
console.log(receiver.name); //file.js
```
访问器属性

# 重复的对象字面量属性
在严格模式下，ES6之前会检查重复属性，ES6之后就不检查了，直接使用最后一个属性的值

# 自有属性枚举顺序
ES6之前由JavaScript引擎厂商自行决定，但ES6及之后严格规定了顺序

基本规则：
1. 所以数字键按升序排序
2. 所欲字符串键按照它们被加入对象的顺序排序
3. 所以Symbol键按照它们被加入对象的顺序排序

``` JavaScript
var obj = {
    a: 1,
    0: 1,
    c: 1,
    2: 1,
    b: 1,
    1: 1
};
obj.d = 1;
console.log(Object.getOwnPropertyNames(obj).join(""));
```

# 增强对象原型
## 改变对象的原型
通过`Object,setPrototypeOf()`方法可以改变任意指定对象的原型
``` JavaScript
let person = {
    getGreeting() {
        return 'Hello';
    };
};
let dog = {
    getGreeting() {
        return 'Woof';
    }
};
let friend = Object.create(person);
console.log(firend.getGreeting()); //'Hello'
console.log(Object.getPrototypeOf(friend) === person); //true

//将原型设置为dog
Object.setPrototypeOf(friend, dog);
console.log(firend.getGreeting()); //'Woof'
console.log(Object.getPrototypeOf(friend) === dog); //true
```
## 简化原型访问的Super引用
