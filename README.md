# 如何理解JS的继承？
①  es5的继承是通过修改子类的原型对象来实现的。
```javascript
function SuperType(name){
    this.name=name;
    this.colors=["red","blue","green"];
}
SuperType.prototype.sayname=function(){
    alert(this.name);
};
function subType(name,age){
    SuperType.call(this,name);
    this.age=age;
}
SubType.prototype=Object.create(SuperType.prototype,{
    constructor:{
        value:SubType,
        enumerable:false,
        writable:true,
        condigurable:true
    }
})
SubType.prototype.sayAge=function(){
    alert(this.age);
};
let ins = new SubType('gim','17');
ins.sayName();//gim
ins.sayAge();//17
```
1. 首先，这段代码声明了父类SuperType。
2. 其次，声明了父类的原型方法sayName。
3. 再次，声明了子类SubType，并在未来创建的SubType实例环境上调用父类SuperType.call,以获取父类中的name和colors属性。
4. 再次，用Object.create()方法把子类的原型对象的__proto__属性指向了父类的原型对象，并把子类构造函数重新赋值为子类。
5. 然后，给子类的原型对象上添加方法sayAge。
6. 最后，初始化实例对象ins。（调用new SubType('gim','17')的时候会生成一个__proto__指向SubType.prototype的空对象，然后把this指向这个空对象，在添加完name、colors、age属性后，返回这个空对象。）

② ES6的继承是通过class...extends实现的。
```javascript
class SuperType{
    constructor(name){
        this.name=name;
        this.colors=["red","blue","green"];
    }
    sayName(){
        alert(this.name);
    }
}
class SubType extends SuperType{
    constructor(name,age){
        super(name);
        this.age=age;
    }
    sayAge(){
        alert(this.age);
    }
}
```
这里可以明显看出，这段代码比之前的更为简短与美观。ES6 class实现继承的核心仔鱼使用关键字extends表明来自哪个父类，并且在子类构造函数中必须调用super关键字，super(name)相当于ES5继承实现中的SuperType.call(this.name)。