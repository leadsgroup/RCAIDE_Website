TeaVM is an ahead-of-time compiler of Java bytecode to JavaScript.
It's much like GWT, however GWT takes source code, and this limits GWT to Java only.
Unlike GWT, TeaVM relies on existing compilers such as javac, kotlinc and scalac.
These compilers produce bytecode (`*.class` or `*.jar` files),
then TeaVM takes this bytecode and produces JavaScript code.


# Purpose

TeaVM is primarily a web development tool.
It's not for taking your large existing codebase in Java or Kotlin and producing JavaScript.
Unfortunately, Java was not designed to run efficiently in the browser.
There are Java APIs that are impossible to implement without generating inefficient JavaScript.
Some of these APIs are: reflection, resources, class loaders, and JNI.
TeaVM restricts usage of these APIs.
Generally, you'll have to manually rewrite your code to fit into TeaVM constraints.

TeaVM is for you, if:

  * You are a Java developer and you are going to write a web front-end from scratch.
  * You already have a Java-based backend and want to integrate front-end code tightly into your existing development
    infrastructure.
  * You have some Java back-end code you want to reuse in the front end.  
  * You are ready to rewrite your code to work with TeaVM.

If you have tightly-coupled applications that use Swing, you want to run these applications in web,
and you don't care about download size, start-up time and performance, you should probably look elsewhere;
there are more appropriate tools for you, like [CheerpJ](https://www.leaningtech.com/cheerpj/).


# Strong parts

* TeaVM tries to reconstruct original structure of a method, 
  so in most cases it produces JavaScript that you would write manually.
  No bloated while/switch statements, as naive compilers often do.
* TeaVM has a very sophisticated optimizer, which knows a lot about your code. Some examples are:
  * Dead code elimination produces very small JavaScript. 
  * Devirtualization turns virtual calls into static function calls, which makes code faster.
  * TeaVM can reuse one local variables to store several local variables.
  * TeaVM renames methods to as short forms as possible; UglifyJS usually can't perform such optimization.
* TeaVM supports threads. 
  JavaScript does not provide APIs to create threads 
  (WebWorkers are not threads, since they don't allow to share state between workers).
  TeaVM is capable of transforming methods to continuation-passing style.
  This makes possible to emulate multiple logical threads in one physical thread.
  TeaVM threads are, in fact, [green threads](https://en.wikipedia.org/wiki/Green_threads).
* TeaVM is very fast, you don't need to wait for minutes until your application gets recompiled.
* TeaVM produces source maps; TeaVM IDEA plugin allows you to [debug code right from the IDE](/docs/tooling/debugging.html). 
* TeaVM has a nice [JavaScript interop API](/docs/runtime/jso.html).


# Motivation

Why use TeaVM when there are plenty of transpilers and frameworks for web front-end development?

If you are a JavaScript developer who is satisfied with JavaScript, TypeScript or even elm,
you probably won't need TeaVM.

If you are a Java (or Kotlin, or Scala) developer who is used to writing back-end code, TeaVM might be your choice.
It's true that a good developer (including Java developer) can learn JavaScript.
However, to become an expert you have to spend a reasonable amount of your time.

Another drawback of JavaScript for you is it's a different language with different syntax, 
different build tools, different IDE experience.
Say, you have your Java/Maven build set up in Jenkins,
with SonarQube checking your code quality and IDEA code style settings.
You have to repeat all these things for the JavaScript ecosystem.
Finally, you have to "switch context" every time you change the code on back-end and front-end side.

TeaVM allows you to use single ecosystem, and reuse as much as possible of it for
both back-end and front-end worlds.
