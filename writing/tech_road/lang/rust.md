相关内容会在用rust写os的时候发现问题及时补充在[rust_write_os](./rust_write_os.md)中，之后整理同步



Rust 有一个叫做 `!` 的特殊类型。在类型理论术语中，它被称为 *empty type*，因为它没有值。我们更倾向于称之为 *never type*。这个名字描述了它的作用：在函数从不返回的时候充当返回值。



