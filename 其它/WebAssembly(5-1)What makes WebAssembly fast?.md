# [翻译]WebAssembly(5-1) What makes WebAssembly fast?

In the last article, I explained that programming with WebAssembly or JavaScript is not an either/or choice. We don’t expect that too many developers will be writing full WebAssembly code bases.

So developers don’t need to choose between WebAssembly and JavaScript for their applications. However, we do expect that developers will swap out parts of their JavaScript code for WebAssembly.

For example, the team working on React could replace their reconciler code (aka the virtual DOM) with a WebAssembly version. People who use React wouldn’t have to do anything… their apps would work exactly as before, except they’d get the benefits of WebAssembly.

The reason developers like those on the React team would make this swap is because WebAssembly is faster. But what makes it faster?

# What does JavaScript performance look like today?

Before we can understand the differences in performance between JavaScript and WebAssembly, we need to understand the work that the JS engine does.

This diagram gives a rough picture of what the start-up performance of an application might look like today.

The time that the JS engine spends doing any one of these tasks depends on the JavaScript the page uses. This diagram isn’t meant to represent precise performance numbers. Instead, it’s meant to provide a high-level model of how performance for the same functionality would be different in JS vs WebAssembly.

![](media/14933755565734.png)

Each bar shows the time spent doing a particular task.

	•	Parsing—the time it takes to process the source code into something that the interpreter can run.
	•	Compiling + optimizing—the time that is spent in the baseline compiler and optimizing compiler. Some of the optimizing compiler’s work is not on the main thread, so it is not included here.
	•	Re-optimizing—the time the JIT spends readjusting when its assumptions have failed, both re-optimizing code and bailing out of optimized code back to the baseline code.
	•	Execution—the time it takes to run the code.
	•	Garbage collection—the time spent cleaning up memory.

One important thing to note: these tasks don’t happen in discrete chunks or in a particular sequence. Instead, they will be interleaved. A little bit of parsing will happen, then some execution, then some compiling, then some more parsing, then some more execution, etc.

The performance this breakdown brings is a big improvement from the early days of JavaScript, which would have looked more like this:

![](media/14933755837728.png)

In the beginning, when it was just an interpreter running the JavaScript, execution was pretty slow. When JITs were introduced, it drastically sped up execution time.

The tradeoff is the overhead of monitoring and compiling the code. If JavaScript developers kept writing JavaScript in the same way that they did then, the parse and compile times would be tiny. But the improved performance led developers to create larger JavaScript applications.

This means there’s still room for improvement.

# How does WebAssembly compare?

Here’s an approximation of how WebAssembly would compare for a typical web application.

![](media/14933756436344.png)

There are slight variations between browsers in how they handle all of these phases. I’m using SpiderMonkey as my model here.

# Fetching

This isn’t shown in the diagram, but one thing that takes up time is simply fetching the file from the server.

Because WebAssembly is more compact than JavaScript, fetching it is faster. Even though compaction algorithms can significantly reduce the size of a JavaScript bundle, the compressed binary representation of WebAssembly is still smaller.

This means it takes less time to transfer it between the server and the client. This is especially true over slow networks.

# Parsing

Once it reaches the browser, JavaScript source gets parsed into an Abstract Syntax Tree.

Browsers often do this lazily, only parsing what they really need to at first and just creating stubs for functions which haven’t been called yet.

From there, the AST is converted to an intermediate representation (called bytecode) that is specific to that JS engine.

In contrast, WebAssembly doesn’t need to go through this transformation because it is already an intermediate representation. It just needs to be decoded and validated to make sure there aren’t any errors in it.

![](media/14933756962625.png)

# Compiling + optimizing

As I explained in the article about the JIT, JavaScript is compiled during the execution of the code. Depending on what types are used at runtime, multiple versions of the same code may need to be compiled.

Different browsers handle compiling WebAssembly differently. Some browsers do a baseline compilation of WebAssembly before starting to execute it, and others use a JIT.

Either way, the WebAssembly starts off much closer to machine code. For example, the types are part of the program. This is faster for a few reasons:

	1.	The compiler doesn’t have to spend time running the code to observe what types are being used before it starts compiling optimized code.
	2.	The compiler doesn’t have to compile different versions of the same code based on those different types it observes.
	3.	More optimizations have already been done ahead of time in LLVM. So less work is needed to compile and optimize it.

![](media/14933757335448.png)

# Reoptimizing

Sometimes the JIT has to throw out an optimized version of the code and retry it.

This happens when assumptions that the JIT makes based on running code turn out to be incorrect. For example, deoptimization happens when the variables coming into a loop are different than they were in previous iterations, or when a new function is inserted in the prototype chain.

There are two costs to deoptimization. First, it takes some time to bail out of the optimized code and go back to the baseline version. Second, if that function is still being called a lot, the JIT may decide to send it through the optimizing compiler again, so there’s the cost of compiling it a second time.

In WebAssembly, things like types are explicit, so the JIT doesn’t need to make assumptions about types based on data it gathers during runtime. This means it doesn’t have to go through reoptimization cycles.

![](media/14933757656495.png)

# Executing

It is possible to write JavaScript that executes performantly. To do it, you need to know about the optimizations that the JIT makes. For example, you need to know how to write code so that the compiler can type specialize it, as explained in the article on the JIT.

However, most developers don’t know about JIT internals. Even for those developers who do know about JIT internals, it can be hard to hit the sweet spot. Many coding patterns that people use to make their code more readable (such as abstracting common tasks into functions that work across types) get in the way of the compiler when it’s trying to optimize the code.

Plus, the optimizations a JIT uses are different between browsers, so coding to the internals of one browser can make your code less performant in another.
Because of this, executing code in WebAssembly is generally faster. Many of the optimizations that JITs make to JavaScript (such as type specialization) just aren’t necessary with WebAssembly.

In addition, WebAssembly was designed as a compiler target. This means it was designed for compilers to generate, and not for human programmers to write.

Since human programmers don’t need to program it directly, WebAssembly can provide a set of instructions that are more ideal for machines. Depending on what kind of work your code is doing, these instructions run anywhere from 10% to 800% faster.

![](media/14933758010185.png)

# Garbage collection

In JavaScript, the developer doesn’t have to worry about clearing out old variables from memory when they aren’t needed anymore. Instead, the JS engine does that automatically using something called a garbage collector.

This can be a problem if you want predictable performance, though. You don’t control when the garbage collector does its work, so it may come at an inconvenient time. Most browsers have gotten pretty good at scheduling it, but it’s still overhead that can get in the way of your code’s execution.

At least for now, WebAssembly does not support garbage collection at all. Memory is managed manually (as it is in languages like C and C++). While this can make programming more difficult for the developer, it does also make performance more consistent.

![](media/14933758261491.png)

# Conclusion

WebAssembly is faster than JavaScript in many cases because:

	•	fetching WebAssembly takes less time because it is more compact than JavaScript, even when compressed.
	•	decoding WebAssembly takes less time than parsing JavaScript.
	•	compiling and optimizing takes less time because WebAssembly is closer to machine code than JavaScript and already has gone through optimization on the server side.
	•	reoptimizing doesn’t need to happen because WebAssembly has types and other information built in, so the JS engine doesn’t need to speculate when it optimizes the way it does with JavaScript.
	•	executing often takes less time because there are fewer compiler tricks and gotchas that the developer needs to know to write consistently performant code, plus WebAssembly’s set of instructions are more ideal for machines.
	•	garbage collection is not required since the memory is managed manually.

This is why, in many cases, WebAssembly will outperform JavaScript when doing the same task.

There are some cases where WebAssembly doesn’t perform as well as expected, and there are also some changes on the horizon that will make it faster. I’ll cover those in the next article.


