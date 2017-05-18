# [翻译]WebAssembly(6-2) Where is WebAssembly now and what’s next? WebAssembly的现在和未来


# Adding post-MVP features to the spec

One of the goals of WebAssembly is to specify in small chunks and test along the way, rather than designing everything up front.

This means there are lots of features that are expected, but haven’t been 100% thought-through yet. They will have to go through the specification process, which all of the browser vendors are active in.

These features are called future features. Here are just a few.

### Working directly with the DOM

Currently, there’s no way to interact with the DOM. This means you can’t do something like element.innerHTML to update a node from WebAssembly.

Instead, you have to go through JS to set the value. This can mean passing a value back to the JavaScript caller. On the other hand, it can mean calling a JavaScript function from within WebAssembly—both JavaScript and WebAssembly functions can be used as imports in a WebAssembly module.

![](media/14945854095569.png)

Either way, it is likely that going through JavaScript is slower than direct access would be. Some applications of WebAssembly may be held up until this is resolved.

### Shared memory concurrency

One way to speed up code is to make it possible for different parts of the code to run at the same time, in parallel. This can sometimes backfire, though, since the overhead of communication between threads can take up more time than the task would have in the first place.

But if you can share memory between threads, it reduces this overhead. To do this, WebAssembly will use JavaScript’s new SharedArrayBuffer. Once that is in place in the browsers, the working group can start specifying how WebAssembly should work with them.

### SIMD

If you read other posts or watch talks about WebAssembly, you may hear about SIMD support. The acronym stands for single instruction, multiple data. It’s another way of running things in parallel.

SIMD makes it possible to take a large data structure, like a vector of different numbers, and apply the same instruction to different parts at the same time. In this way, it can drastically speed up the kinds of complex computations you need for games or VR.

This is not too important for the average web app developer. But it is very important to developers working with multimedia, such as game developers.
 
### Exception handling

Many code bases in languages like C++ use exceptions. However, exceptions aren’t yet specified as part of WebAssembly.

If you are compiling your code with Emscripten, it will emulate exception handling for some compiler optimization levels. This is pretty slow, though, so you may want to use the DISABLE_EXCEPTION_CATCHING flag to turn it off.

Once exceptions are handled natively in WebAssembly, this emulation won’t be necessary.

### Other improvements—making things easier for developers

Some future features don’t affect performance, but will make it easier for developers to work with WebAssembly.

	•	First-class source-level developer tools. Currently, debugging WebAssembly in the browser is like debugging raw assembly. Very few developers can mentally map their source code to assembly, though. We’re looking at how we can improve tooling support so that developers can debug their source code.
	•	Garbage collection. If you can define your types ahead of time, you should be able to turn your code into WebAssembly. So code using something like TypeScript should be compilable to WebAssembly. The only hitch currently, though, is that WebAssembly doesn’t know how to interact with existing garbage collectors, like the one built in to the JS engine. The idea of this future feature is to give WebAssembly first-class access to the builtin GC with a set of low-level GC primitive types and operations.
	•	ES6 Module integration. Browsers are currently adding support for loading JavaScript modules using the script tag. Once this feature is added, a tag like <script src=url type="module"> could work even if url points to a WebAssembly module.

# Conclusion

WebAssembly is fast today, and with new features and improvements to the implementation in browsers, it should get even faster.

