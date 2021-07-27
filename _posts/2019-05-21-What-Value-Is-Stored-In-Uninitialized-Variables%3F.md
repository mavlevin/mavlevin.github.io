---
layout: post
title: >
    What Value Is Stored In Uninitialized Variables?
hide_title: false
tags: ['Reverse Engineering', 'Uninitialized Variables', 'safe code', 'C/C++', 'Uninitialized Memory Vulnerability']
excerpt: >
    The value in an uninitialized variable is one of: zero, a compiler dependent value \(such as 0xCC's in visual studio\), or data previously stored in that memory location (old data). Let's examine why.
---
The value in an uninitialized variable is one of: zero, a compiler dependent value \(such as 0xCC's in visual studio\), or data previously stored in that memory location \(old data\). 
## Types of Uninitialized Variables And Their Values
### Classic C/C\+\+ Uninitialized Stack Variables 
The classic type of uninitialized variables are local function variables written in a low level language \(such as C/C\+\+\). You would think when these variables are left uninitalized they would simply save the last value they were give. However, there is a catch: when code is compiled in debug mode, the compiler may inject its own code that initializes empty variables to a default value.

This is done to protect against vulnerabilities \(more on this later\), and to more easily detect bugs by giving the variable a bogus value that can be easily identified as uninitialized if it is for example printed to the screen.

Below, a program compiled with Visual Studio in debug mode prints an uninitialized variable. Code can be found [here.](https://gist.github.com/guywhataguy/831c3dd2289f128739610709ad14c869)

{% include image.html url="/assets/img/posts\visual_studio_uninitialized_variable_debug_value.png" caption="A program compiled in debug prints an initialized and an uninitialized  variable. The uninitialized variable is filled with 0xCC&#x27;s." alt="uninialized variable in visual studio debug compilation is 0xCC&#x27;s" %}
How exactly does the uninitialized variable get 0xCC's if we never put them there? As with *everything*, the answer is in the assembly. Let's take a look at the assembly with IDA.

{% include image.html url="/assets/img/posts\visual_studio_debug_uninitialized_variable_under_ida.png" caption="Uninitialized variables have 0xCC&#x27;s when compiling in debug mode" alt="Uninitialized variables have 0xCC&#x27;s when compiling in debug mode" %}
With Ida we can see that Visual Studio is adding code to our program to initialize all our variables. The variables are initialized with the "rep stosb" assembly opcode. This opcode copies the value in eax \(here 0xCCCCCCCC\) to the address in edi \(in our case to the start of our stack variables\) ecx \(0x37\) times. Simply put, the opcodes in the purple rectangle are added by Visual Studio to initialize all our stack variables. Therefore, when compiling in debug mode in Visual Studio, uninitialized variables have 0xCC's in them.

Let's take a look at what happens in release mode.

{% include image.html url="/assets/img/posts\visual_studio_uninitialized_variable_release_value.png" caption="A program compiled in release prints an initialized and an uninitialized  variable. The uninitialized variable has its previous value. " alt="uninialized variable in visual studio release compilations simply have their preivous value" %}
Woah\! That's even weirder: how did the uninitialized variable get the same value?
The answer is that the value it was storing never changed\! Since we never put anything new in the variable, it was still holding its old value. Again, let's see how this looks in IDA.

{% include image.html url="/assets/img/posts\visual_studio_release_uninitialized_variable_under_ida.png" caption="An uninitialized variable will hold the value it was previously given when compiling in release mode" alt="" %}
In the disassembly of the code compiled in release, we can see our variable isn't set unless we explicitly set it.
### C/C\+\+ Uninitialized Heap Variables
Uninitialized heap variables act similar to stack variables when they're uninitialized: when compiling in debug mode, there are tricks to initialize the variables, and in release mode, uninitialized heap variables store their old value until explicitly changed. The only exception is that each new page we get in the heap is given to us by the operating system, and is therefore "clean" by having all zeros. But keep in mind this is only for new pages. So an uninitialized variable that was allocated on a page from the heap that used to have data, will not be all zeros.
## Security and Vulnerability Implications
Having uninitialized variables can lead to security vulnerabilities since when we use unexpected data, we get unexpected results, which is what a vulnerability boils down too.
### Memory Disclosure
This is the classic example of the consequences of using an uninitialized variable. If from kernel mode, we return a struct with a field we forgot to initialize, that field will be returned to user mode with the original data it was holding. Then, user mode can read that data and learn things about kernel mode it wasn't supposed to know. For example if the data returned to user mode is a pointer, the user mode can learn about the memory layout of kernel mode which can help in a later attack. [CVE\-2017\-8684](https://www.exploit-db.com/exploits/42747) is an exact example of this where the windows kernel returned a struct to user mode without initializing all of it.
### Uninitialized Variable Usage
Uninitialized variable usage is when uninitialized variables are used for the values they hold. This is more dangerous than just a memory disclosure as it can lead to memory corruption. For example if an attacker manages to reach a flow where an "array size" variable isn't initialized than it is pretty easy from there to write or read out of bounds from that array.
### Final Notes
Hope you learned something new from this post :\). I did ~~lie~~ make things more abstract in some areas and if there are any questions/comments as usual feel free to post them or DM me on twitter.