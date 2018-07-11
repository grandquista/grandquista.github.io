---
layout: layout
title: "Overview of chimera virtual machine"
date: 2018-04-04 18:00:00 -0700
categories: chimera python learning design
---

[Home](/) [Personal](/about/)

I have been implementing a Python3 interpreter from using the cPython spec [chimera](https://github.com/grandquista/chimera). In this post I will be looking at the high level status and intended direction around the virtual machine structures.

The entire implementation is pre alpha at the moment so all interfaces shown are subject to change.

At a high level the virtual machine executes an AST that is close to the standard library `ast` module. There is no lower level compiled language for executing. There are lower level internal executor structures that encapsulate subexpressions.

The virtual machine has three scope encapsulation structures. The global context holds only data that is constant for the life of the interpreter. This includes the `builtins` module implementation and the flag for keyboard interrupts. The process context is instantiated for each `Process` in the python `multiprocessing` module. Processes will be implemented as a pool of n threads with an isolated map of module imports and a reference to the global structure. Thread contexts hold a register for function returns and a process reference.

The virtual machine is responsible for creating each of these contexts and running code execution inside the correct context.
