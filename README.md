# JS-this
## The complete rules to 'this' in JavaScript.

Explore the five key rules that determine how this is assigned inside JavaScript functions. Understand 
how new, call, bind, and dot notation affect this. Learn why arrow functions use lexical this and how 
libraries may bind this differently. This lesson helps you predict and control this in your code.

<span>this</span> is set in several different ways inside functions. This lesson will make you a master of <span>this</span>.

### Rules

<p id="rule01">1 - If the <span>new</span> keyword is used when calling the function, <span>this</span> inside the function is a brand new object created by the JavaScript engine.</p>

```
function ConstructorExample() {
    console.log(this);
    this.value = 10;
    console.log(this);
}

new ConstructorExample();

// -> ConstructorExample {}
// -> ConstructorExample { value: 10 }
```

<p id="rule02">2 - If apply, call, or bind are used to call a function, <span>this</span> inside the function is the object that is passed in as the argument.</p>

```
function fn() {
    console.log(this);
}

var obj = {
    value: 5
};

var boundFn = fn.bind(obj);

boundFn(); // -> { value: 5 }
fn.call(obj); // -> { value: 5 }
fn.apply(obj); // -> { value: 5 }
```

<p id="rule03">3 - If a function is called as a method — that is, if dot notation is used to invoke the function — <span>this</span> 
is the object that the function is a property of. In other words, when a dot is to the left of a function invocation, 
<span>this</span> is the object to the left of the dot. (ƒ symbolizes function in the code blocks)</p>

```
const obj = {
    value: 5,
    printThis: function() {
      console.log(this);
    }
};

obj.printThis(); // -> { value: 5, printThis: ƒ }
```

<p id="rule04"4 - If a function is invoked as a free function invocation, meaning it was invoked without any of the conditions 
present above, <span>this</span> is the global object. In a browser, it’s <span>window</span>.</p>

Note that this rule is the same as rule 3 — the difference is that a function that is not declared as a method 
automatically becomes a property of the global object, <span>window</span>. This is therefore an implicit method invocation. 
When we call <span>fn()</span>, it’s interpreted as <span>window.fn()</span>, so <span>this</span> is <span>window</span>.

```
() {
    console.log(this);
}

// In browser:
console.log(fn === window.fn); // -> true
```

<p id="rule05"5 - If multiple of the above rules apply, the rule that is higher wins and will set the <span>this</span> value.</p>

### Applying the Rules
Let’s go over a code example and apply our rules. Try figuring out what <span>this</span> will be with the two 
different function calls.

#### Determining Which Rule Applies

```
const obj = {
    value: 'hi',
    printThis: function() {
        console.log(this);
    }
};

const print = obj.printThis;
obj.printThis(); // -> {value: "hi", printThis: ƒ}
print(); // -> Window {stop: ƒ, open: ƒ, alert: ƒ, ...}
```

<span>obj.printThis()</span> falls under rule 3 — invocation using dot notation. On the other hand, <span>print()</span> 
falls under rule 4 as a free function invocation. For <span>print()</span> we don’t use <span>new</span>, 
<span>bind/call/apply</span>, or dot notation when we invoke it, so we go to rule 4 and <span>this</span> is the global 
object, <span>window</span>.

This goes back to value vs. reference. The value of <span>printThis</span> on the object is a reference to the function. 
When we assign <span>obj.printThis</span> to <span>print</span>, <span>print</span> receives the reference of the function. 
It’s not bound to <span>obj </span>in any way - <span>obj</span> just happens to have a reference to it.

When we invoke it without <span>obj</span>, it’s an FFI. It really is the use of the dot <span>(.)</span> that makes rule 3 apply.

### When Multiple Rules Apply
When multiple rules apply, the rule higher on the list wins. If rules 2 and 3 both apply, rule 2 takes precedence.

```
const obj1 = {
    value: 'hi',
    print: function() {
        console.log(this);
    },
};

const obj2 = { value: 17 };

obj1.print.call(obj2); // -> { value: 17 }
```

If rules 1 and 3 both apply, rule 1 takes precendence.

```
const obj1 = {
    value: 'hi',
    print: function() {
        console.log(this);
    },
};

new obj1.print(); // -> print {}
```

### Libraries
Libraries will sometimes intentionally bind the value of <span>this</span> inside their functions. <span>this</span> 
is bound to the most useful value for use in the function. jQuery, for example, binds <span>this</span> to the DOM 
element triggering an event in the callback to that event. If a library has an unexpected <span>this</span> value 
that doesn’t seem to follow the rules, check its documentation. It’s likely being bound using <span>bind</span>.

### Arrow Functions
ES2015 arrow functions get their <span>this</span> value lexically.













