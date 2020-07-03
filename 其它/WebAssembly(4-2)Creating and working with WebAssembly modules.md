# [翻译] WebAssembly(4-2) Creating and working with WebAssembly modules

原文地址：https://hacks.mozilla.org/2017/02/creating-and-working-with-webassembly-modules/

# Loading a .wasm module in JavaScript 用js加载wasm模块

The .wasm file is the WebAssembly module, and it can be loaded in JavaScript. As of this moment, the loading process is a little bit complicated.

.wasm文件是WebAssembly模块，他可以被js加载。目前，加载的过程稍微有点复杂。

```
function fetchAndInstantiate(url, importObject) {
  return fetch(url).then(response =>
    response.arrayBuffer()
  ).then(bytes =>
    WebAssembly.instantiate(bytes, importObject)
  ).then(results =>
    results.instance
  );
}

```

We’re working on making this process easier. We expect to make improvements to the toolchain and integrate with existing module bundlers like webpack or loaders like SystemJS. We believe that loading WebAssembly modules can be as easy as as loading JavaScript ones.

我们正努力使这个过程变得更简单。我们准备提供更多更好的工具，也准备将其加载过程和目前的模块打包工具比如webpack进行整合。我们相信在未来加载WebAssembly模块就像加载js模块那样简单。

There is a major difference between WebAssembly modules and JS modules, though. Currently, functions in WebAssembly can only use numbers (integers or floating point numbers) as parameters or return values.

WebAssembly模块和js模块有一个最主要的不同。目前，WebAssembly的只能使用数字类型作为方法的参数和返回值。

![](media/14924147235980.png)

For any data types that are more complex, like strings, you have to use the WebAssembly module’s memory.

当用到更复杂的数据类型，比如字符串，你必须使用WebAssembly的模块内存操作。

If you’ve mostly worked with JavaScript, having direct access to memory isn’t so familiar. More performant languages like C, C++, and Rust, tend to have manual memory management. The WebAssembly module’s memory simulates the heap that you would find in those languages.

如果你是js的开发人员，那么你对直接内存操作可能不太熟悉。一些更底层的语言，比如c，c++，rust，都有手动操作内存的功能。WebAssembly模块内存就像是上述语言中使用的堆内存。

To do this, it uses something in JavaScript called an ArrayBuffer. The array buffer is an array of bytes. So the indexes of the array serve as memory addresses.

为了在js中实现内存操作，使用了js的arraybuffer。array buffer 是一个字节数组。数组的索引映射了内存的地址。

If you want to pass a string between the JavaScript and the WebAssembly, you convert the characters to their character code equivalent. Then you write that into the memory array. Since indexes are integers, an index can be passed in to the WebAssembly function. Thus, the index of the first character of the string can be used as a pointer.

如果你想让WebAssembly和js交换一个字符串，可以利用 ArrayBuffer 将其写入内存中，这时候 ArrayBuffer的索引就是整型了，可以把它传递给 WebAssembly 函数。此时，第一个字符的索引就可以当做指针来使用。

![](media/14924147439939.png)

It’s likely that anybody who’s developing a WebAssembly module to be used by web developers is going to create a wrapper around that module. That way, you as a consumer of the module don’t need to know about memory management.

这就好像一个web开发者在开发WebAssembly模块时，把这个模块包装了一层。这样其他使用者在使用这个模块的时候，就不用关心内存管理的细节。

If you want to learn more, check out our docs on working with WebAssembly’s memory.

如果你想了解更多的内存管理，看一下我们写的WebAssembly的内存操作。

# The structure of a .wasm file .wasm文件结构

If you are writing code in a higher level language and then compiling it to WebAssembly, you don’t need to know how the WebAssembly module is structured. But it can help to understand the basics.

如果你是写高级语言的开发者，并且通过编译器编译成WebAssembly，那你不用关心WebAssembly模块的结构。但是了解它的结构有助于你理解一些基本问题。

If you haven’t already, we suggest reading the article on assembly (part 3 of the series).

如果你对编译器还不了解，建议先读一下“WebAssembly 系列（三）”这篇文章。

Here’s a C function that we’ll turn into WebAssembly:

这段代码是生成 WebAssembly 的 C 代码：

```
int add42(int num) {
  return num + 42;
}

```

You can try using the WASM Explorer to compile this function.

你可以使用 WASM Explorer 来编译这个函数。

If you open up the .wasm file (and if your editor supports displaying it), you’ll see something like this.

打开.wasm 文件（假设你的编辑器支持的话），可以看到下面代码：

```
00 61 73 6D 0D 00 00 00 01 86 80 80 80 00 01 60
01 7F 01 7F 03 82 80 80 80 00 01 00 04 84 80 80
80 00 01 70 00 00 05 83 80 80 80 00 01 00 01 06
81 80 80 80 00 00 07 96 80 80 80 00 02 06 6D 65
6D 6F 72 79 02 00 09 5F 5A 35 61 64 64 34 32 69
00 00 0A 8D 80 80 80 00 01 87 80 80 80 00 00 20
00 41 2A 6A 0B
```

That is the module in its “binary” representation. I put quotes around binary because it’s usually displayed in hexadecimal notation, but that can be easily converted to binary notation, or to a human readable format.

这是模块的“二进制”表示。之所以用引号把“二进制”引起来，是因为上面其实是用十六进制表示的，不过把它变成二进制或者人们能看懂的十进制表示也很容易。

For example, here’s what num + 42 looks like.

例如，下面是 num + 42 的各种表示方法。

![](media/14924148174927.png)


# How the code works: a stack machine 代码如何工作：基于栈的机器

In case you’re wondering, here’s what those instructions would do.

如果你对具体的操作过程很好奇，那么这幅图可以告诉你指令都做了什么。

![](media/14924148445982.png)

You might have noticed that the add operation didn’t say where its values should come from. This is because WebAssembly is an example of something called a stack machine. This means that all of the values an operation needs are queued up on the stack before the operation is performed.

从图中我们可以注意到“加”操作并没有指定哪两个数字进行加。这是因为WebAssembly 是采用“基于栈的机器”的机制。即一个操作符所需要的所有值，在操作进行之前都已经存放在堆栈中。

Operations like add know how many values they need. Since add needs two, it will take two values from the top of the stack. This means that theadd instruction can be short (a single byte), because the instruction doesn’t need to specify source or destination registers. This reduces the size of the .wasm file, which means it takes less time to download.

所有的操作符，比如加法，都知道自己需要多少个值。”加“需要两个值，所以它从堆栈顶部取两个值就可以了。那么加指令就可以变的更短（单字节），因为指令不需要指定源寄存器和目的寄存器。这也使得 .wasm 文件变得更小，进而使得加载 .wasm 文件更快。

Even though WebAssembly is specified in terms of a stack machine, that’s not how it works on the physical machine. When the browser translates WebAssembly to the machine code for the machine the browser is running on, it will use registers. Since the WebAssembly code doesn’t specify registers, it gives the browser more flexibility to use the best register allocation for that machine.

尽管 WebAssembly 使用基于栈的机制，但是并不是说在实际的物理机器上它就是这么生效的。当浏览器翻译 WebAssembly到机器码时，浏览器会使用寄存器， WebAssembly 代码并不指定用哪些寄存器，这样做的好处是给浏览器最大的自由度，让其自己来进行寄存器的最佳分配。

# Sections of the module 模块的其它部分

Besides the add42 function itself, there are other parts in the .wasm file. These are called sections. Some of the sections are required for any module, and some are optional.

除了上面介绍的，.wasm 文件还有其他部分，通常把它们叫做部件。一些部件对于模块来讲是必须的，一些是可选的。

Required:

	1.	Type. Contains the function signatures for functions defined in this module and any imported functions.
	2.	Function. Gives an index to each function defined in this module.
	3.	Code. The actual function bodies for each function in this module.

必须部分：

	1.	Type。在模块中定义的函数的函数声明和所有引入函数的函数声明。
	2.	Function。给出模块中每个函数一个索引。
	3.	Code。模块中每个函数的实际函数体。

Optional:

	1.	Export. Makes functions, memories, tables, and globals available to other WebAssembly modules and JavaScript. This allows separately-compiled modules to be dynamically linked together. This is WebAssembly’s version of a .dll.
	2.	Import. Specifies functions, memories, tables, and globals to import from other WebAssembly modules or JavaScript.
	3.	Start. A function that will automatically run when the WebAssembly module is loaded (basically like a main function).
	4.	Global. Declares global variables for the module.
	5.	Memory. Defines the memory this module will use.
	6.	Table. Makes it possible to map to values outside of the WebAssembly module, such as JavaScript objects. This is especially useful for allowing indirect function calls.
	7.	Data. Initializes imported or local memory.
	8.	Element. Initializes an imported or local table.

可选部分：

	1.	Export。使函数、内存、表单（table）、全局变量等对其他 WebAssembly 或 JavaScript 可见，允许动态链接一些分开编译的组件，即 .dll 的WebAssembly 版本。
	2.	Import。允许从其他 WebAssembly 或者 JavaScript 中引入指定的函数、内存、表单或者全局变量。
	3.	Start。当 WebAssembly 模块加载进来的时候，可以自动运行的函数（类似于 main 函数）。
	4.	Global。声明模块的全局变量。
	5.	Memory。定义模块用到的内存。
	6.	Table。使得可以映射到 WebAssembly 模块以外的值，如映射到 JavaScript 对象中。这在间接函数调用时很有用。
	7.	Data。初始化内存。
	8.	Element。初始化表单（table）。

For more on sections, here’s a great in-depth explanation of how these sections work.

如果想要了解更多的部件，可以在“如何使用部件”中深入了解。

# Coming up next 后续

Now that you know how to work with WebAssembly modules, let’s look atwhy WebAssembly is fast.

现在你已经了解了WebAssembly模块的工作原理，下面将会介绍为什么WebAssembly运行的更快。






