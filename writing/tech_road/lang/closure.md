抛开语言去谈闭包，我觉得会是一个很难理解的问题，这里就结合Go，Rust，Pyhton，Java试着从不同的语言来理解闭包。那么，从不同的语言来理解一个概念有什么意义，是不是只有实现不同，其实不是的，每一种语言都有自己独有的设计思路，殊途同归，有时候我们只看重结果，但是通往结果的道路依然重要，我们可以看一下每条道路上的不同风景。



闭包（closure）是一个可调用的对象，它记录了一些信息，这些信息来自于创建它的作用域。

## RUST

rust函数定义:

```rust
fn simulated_expensive_calculation(num: u32) -> u32 {
     println!("calculating slowly...");
     thread::sleep(Duration::from_secs(2));
     num
}

fn generate_workout(intensity: u32, random_number: u32) {
    let expensive_result =
        simulated_expensive_calculation(intensity);
}
```



不同于函数进行函数调用并将其结果存储在变量，定义一个闭包并存储在变量中:

```rust
use std::thread;
use std::time::Duration;

let expensive_closure = |num| {
    println!("calculating slowly...");
    thread::sleep(Duration::from_secs(2));
    num
};
```

`=`	之后的部分是闭包的定义，这个闭包有多于一个参数num，如果有多于一个参数，可以使用逗号分隔，比如 `|param1, param2|`,这个 `let` 语句意味着 `expensive_closure` 包含一个匿名函数的 **定义**，不是调用匿名函数的 **返回值**。调用闭包类似于调用函数，指定存放闭包定义的变量名并后跟包含期望使用的参数:

```rust
expensive_closure(5);
```

这样我们可以解决无效调用的问题，那么如果有连续的闭包调用时，是否也可以将闭包的调用结果存储在变量中以便复用？

### 作用及设计目的

- 作为匿名函数使用
- 访问作用域内的变量

需要注意的是，由于Rust特有的内存管理方式，闭包使用作用域内的变量相应的有三种方式，移动(`move`)，不可变借用(`&`)，可用借用(`& mut`)，与之相匹配的Rust定义了三种`Fn trait`:

* `FnOnce`:消费从周围作用域捕获的变量，闭包周围的作用域被称为其 环境，*environment*。为了消费捕获到的变量，闭包必须获取其所有权并在定义闭包时将其移动进闭包。其名称的 `Once` 部分代表了闭包不能多次获取相同变量的所有权的事实，所以它只能被调用一次。
* `Fn` :从其环境获取不可变的借用值
* `FnMut`: 获取可变的借用值所以可以改变其环境





## JAVA

根据闭包的定义，内部类是面向对象的闭包。

## Python



## Go







