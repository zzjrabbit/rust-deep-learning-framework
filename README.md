# An Idea For Deep Learning Framework In Rust

**Languages:**
[中文](README.zh.md)

> [!NOTE]
> For anyone who want to give this idea a try, just do it!
> I have only one requirement: Add a reference to this repository in your README.md.

Nowadays, there's already a lot of deep learning frameworks in Rust. \
However, most of these frameworks are basically a reimplementation of pytorch. \
Though pytorch is a flexible and powerful framework, and works well for many tasks, \
its performance might not be so good due to its dynamic computation graph. \
Meanwhile, frameworks like TensorFlow use static computation graphs, which can lead to better performance. \
But they are not so convenient as pytorch. 

## Automatic Neural Network Generation
Some people have been working on softwares which allows you to build neural networks like assembling blocks. \
And then it will generate programs that can train the neural network. \
I have to admit that this is a very interesting idea. \
However, I believe that no one wants to read a graph containing thousands of nodes. \
Also, developing plugins for such frameworks can be difficult. \
You need to learn about how to render your nodes. \
And project files of these frameworks are not human-readable, \
they require the exact software that created them. \
So we need a framework that uses a text file, and can be easily read and edited by humans.

## Rust Procedural Macros
Rust procedural macros are a powerful feature that allows you to generate code at compile time. \
Crates like syn, quote and proc-macro2 have made it easier to write procedural macros in Rust. \
Also, Procedural macros are still very readable and easy to understand.

## Introduction: Deep Learning Framework Based On Rust's Procedural Macros
Procedural macros makes it possible to create a framework that uses a text file format, \
while still taking advantage of Rust's type system and safety guarantees. \
Also, for framework developers, you don't need to create a plugin system, \
and manage to find a new way to help new users find and install the plugins they need. \
They can simply let users put their plugins on crates.io, \
and then create some lists of available plugins for users to choose from. \
At the same time, you no longer need to find nodes and edit edges in a huge and complex graph. \
You can even split them into multiple files, \
and jump between the files with the support of LSP. \
In addition, you can write documents right inside the code. 

## Some Details( For those of you who want to try this )
Because procedural macros can't get global information,
generating the computation graph directly is impossible.
So a better idea is to generate a generator which generates a raw graph.
You can put it in build.rs, then optimize it and store it in some file,
then in main.rs, users can use another procedural macro to read the graph from the file
and generate a structure with some functions, like `train` and `forward`.


## Examples
There are some examples to help you understand this idea. 

### MLP
```rust
#[model(in_dim = 10, out_dim = 1)]
struct Mlp {
    #[layer(in_dim = 10, out_dim = 20)]
    layer1: Linear,
    #[layer(foreach)]
    f1: relu,
    #[layer(in_dim = 20, out_dim = 10)]
    layer2: Linear,
    #[layer(foreach)]
    f2: relu,
    #[layer(in_dim = 10, out_dim = 1)]
    layer3: Linear,
    #[layer(foreach)]
    f3: sigmoid,
}
```
