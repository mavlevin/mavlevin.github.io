---
layout: post
title: >
    Mav's 30 Reverse Engineering Tips & Tricks
hide_title: false
tags: ['Reverse Engineering', 'Assembly']
excerpt_separator: <!--more-->
author: guy
redirect_from: /2020/04/guys-30-reverse-engineering-tips-tricks.html
---

During April I challenged myself to tweet 1 reverse engineering tip every day. For your viewing pleasure, here I aggregated all 30 tips.
<!--more-->
Be sure to [follow me @mavlevin](https://twitter.com/mavlevin) for my latest tweets and more reverse engineering extravaganza.

<style>
li {margin-bottom: 1em;}
</style>

# Reverse Engineering Tips
1. long branch-less functions with many xors & rols are usually hash functions.

    {% include image.html url="/assets/img/posts/30-re-tips/ida-view-md5-func.png" caption="IDA view of MD5 function" alt="" %}
    
    [(link to tweet)](https://twitter.com/whtaguy/status/1245197118865846273)

2. Building on the last tip, after finding a hash function, google its constant to identify the exact hash algorithm. 

    {% include image.html url="/assets/img/posts/30-re-tips/find-const-in-hash-function.png" caption="1) select a constant in the hash function" alt="" %}
    {% include image.html url="/assets/img/posts/30-re-tips/search-constant-online.jpg" caption="2) search that constant online to identify the hash function. In this example, MD5" alt="" %}

    [(link to tweet)](https://twitter.com/whtaguy/status/1245656402862854144?s=20)

3. Find the function controlling authentication in any exe by diff'ing the execution trace of a valid vs failed login. Traces will split right after the authentication check

    {% include image.html url="/assets/img/posts/30-re-tips/re-tip-3-pic1.jpg" caption="the execution path will change according to if the authentication succeeded" alt="" %}
   
    You can use Lighthouse to conveniently visualize process execution trace data and operate on multiple traces, showing their shared & unique trace paths
    {% include image.html url="/assets/img/posts/30-re-tips/re-tip-3-2.gif" caption="Lighthouse extension in action in IDA" alt="" %}
    
    [(link to tweet)](https://twitter.com/whtaguy/status/1246018981321900032?s=20)

4. Use a function's neighboring code to understand its functionality:
    Developers group related funcs together in the same files, and compilers like to keep the order of functions from source to compiled binary. Thus, related functions are closely grouped in the exe 
    {% include image.html url="/assets/img/posts/30-re-tips/serv-u-adjacent-startup-funcs.png" caption="" alt="" %}
    
    [(link to tweet)](https://twitter.com/whtaguy/status/1246376443657019393?ref_src=twsrc%5Etfw)
 
5. Hate updating breakpoint addresses each time a module loads with a new base? Patch the PE header of the EXE/DLL to disable the DYNAMIC\_BASE flag for a static base address. [Here's a python script](https://github.com/guywhataguy/DisableDynamicLoadAddress) that will patch files for you.
    {% include image.html url="/assets/img/posts/30-re-tips/dll-chars-pic.png" caption="" alt="" %}

    [(link to tweet)](https://twitter.com/whtaguy/status/1246734891720552454?ref_src=twsrc%5Etfw)

6. When reversing C\+\+: Use [“virtuailor”](https://github.com/0xgalz/Virtuailor) to \*automatically\* create class vtables & add xrefs to virtual functions. It uses runtime inspection to evaluate function addresses to do its magic. Tool written by [@0xGalz](https://twitter.com/0xgalz)
    {% include image.html url="/assets/img/posts/30-re-tips/vtable_in_memory_marked.png" caption="" alt="" %}


    [(link to tweet)](https://twitter.com/whtaguy/status/1247172828987826176?ref_src=twsrc%5Etfw)

7. Reversing is more fun with symbols. So, if the symbols are stripped, try looking for symbols in older versions, versions for other OSs, beta builds, and the mobile app versions. 
    
    If still no symbols are found, check which has the most debug prints.
    
    {% include image.html url="/assets/img/posts/30-re-tips/ida_before_and_after_syms.png" caption="IDA Function list before and after adding symbols" alt="" %}

    [(link to tweet)](https://twitter.com/whtaguy/status/1247387110761512961?ref_src=twsrc%5Etfw)

8. Want a faster way to open IDA on your exe? Add it to the Windows "send to" menu
    1. open Explorer
    2. enter “sendto” in the path bar
    3. drag an IDA Pro shortcut to this folder
    4. You can now right\-click “send to IDA” on any file
    5. Profit!

    {% include image.html url="/assets/img/posts/30-re-tips/marked-sendto-ida.png" caption="IDA in the send to menu" alt="" %}
    
    Bonus: this works for other programs too! Drag shortcuts to your favorite programs into that folder, and they will be available through the “send to” menu too

    [(link to tweet)](https://twitter.com/whtaguy/status/1247850096202481665?ref_src=twsrc%5Etfw)

9. Search [http://magnumdb.com](http://magnumdb.com) to name any unknown Windows constant/guid/error code you come across while reversing
    
    {% include image.html url="/assets/img/posts/30-re-tips/magnum-how-it-works.jpg" caption="How MangNumDB works" alt="" %}
    {% include image.html url="/assets/img/posts/30-re-tips/magnum-search.jpg" caption="Searching in MangNumDB" alt="" %}

    [(link to tweet)](https://twitter.com/whtaguy/status/1248190305251348480?ref_src=twsrc%5Etfw)

10. Pimp your gdb experience with ‘layout asm’ and ‘ layout regs’, or take it a step further by installing [pwndbg](https://github.com/pwndbg/pwndbg)

    {% include image.html url="/assets/img/posts/30-re-tips/layout-asm-layout-regs-marked.png" caption="‘layout asm’ and ‘layout regs’," alt="" %}
    {% include image.html url="/assets/img/posts/30-re-tips/pwndbg-marked.png" caption="gdb with pwndbg installed" alt="" %}

    [(link to tweet)](https://twitter.com/whtaguy/status/1248559742559059969?ref_src=twsrc%5Etfw)

11. Want to reverse engineer code handling a GUI window? Find the window’s Resource ID with ResourceHacker, then search IDA for where that ID is used (alt+I to search in IDA). I used 7zip In the example below

    {% include image.html url="/assets/img/posts/30-re-tips/RH-marked.png" caption="" alt="" %}

    [(link to tweet)](https://twitter.com/whtaguy/status/1248917668062728194?ref_src=twsrc%5Etfw)

12. Get the best from both static & dynamic analysis by sync’ing your debugger \(WinDbg/GDB/x64dbg/ and more\) with your disassembler \(IDA/Ghidra\) through [Ret-Sync](https://github.com/bootleg/ret-sync)
    
    {% include image.html url="/assets/img/posts/30-re-tips/ret-sync-plugin-screenshot-signature-marked.png" caption="" alt="" %}

    [(link to tweet)](https://twitter.com/whtaguy/status/1249233137219108864?ref_src=twsrc%5Etfw)

13. Optimized magic numbers can uncover functionality. For example: 0x7efefeff is used in strlen, and 0x5f3759df is used to find the inverse square root \(1/sqrt\(x\)\)

    Further reading: [http://lrdev.com/lr/c/strlen.c](http://lrdev.com/lr/c/strlen.c), [https://en.wikipedia.org/wiki/Fast_inverse_square_root](https://en.wikipedia.org/wiki/Fast_inverse_square_root)
    
    {% include image.html url="/assets/img/posts/30-re-tips/optimized_strlen_no_comments_marked.png" caption="as the number appears in strlen source code" alt="" %}    

    {% include image.html url="/assets/img/posts/30-re-tips/optimized_inverse_sqrt_no_comments-marked.png" caption="as the number appears in inverse square root source code" alt="" %}


    [(link to tweet)](https://twitter.com/whtaguy/status/1249596654384230401?ref_src=twsrc%5Etfw)    

14. Multiplying by a constant followed by \+’ing then shifting\(\>\>\) can be a sign of optimized division.

    For example: multiplying by 0x92492493 is the first step in efficiently dividing by 7. 

    [More info](https://reverseengineering.stackexchange.com/questions/1397/how-can-i-reverse-optimized-integer-division-modulo-by-constant-operations)

    {% include image.html url="/assets/img/posts/30-re-tips/ida-division_pic_marked.png" caption="optimized division by 7 in assembly" alt="" %}

    Fortunately, IDA’s HexRays usually simplifies this for us in its x64 decompiler. Decompiled view:

    {% include image.html url="/assets/img/posts/30-re-tips/ida-decompiler-division_pic.jpg" caption="IDA decompiling optimized division by a constant" alt="" %}

    [(link to tweet)](https://twitter.com/whtaguy/status/1249960666015969280?ref_src=twsrc%5Etfw)

15. In IDA's disassembler, you can use the numpad \+/\- keys to change the number of args passed to a function with variable arguments such as printf
    
    {% include image.html url="/assets/img/posts/30-re-tips/varags-trimmed-cropped-gif.gif" caption="adding and subtraction arguments to printf call" alt="" %}

    [(link to tweet)](https://twitter.com/whtaguy/status/1250316845879459841?ref_src=twsrc%5Etfw)

16. Running controlled input on a function we're reversing is incredibly useful. You can do this by "converting" the function’s EXE to a DLL, and then invoke the function as if it were a regular exported DLL function. [Explanatory blog post I wrote](https://guywhataguy.github.io/2020/04/11/Calling-Arbitrary-Functions-In-EXEs-3A-Performing-Calls-to-EXE-Functions-Like-DLL-Exports.html)
    {% include image.html url="/assets/img/posts/30-re-tips/patching-exes-load-like-dll.jpg" caption="" alt="" %}

    [(link to tweet)](https://twitter.com/whtaguy/status/1250739565670936580?ref_src=twsrc%5Etfw)

17. Have you extracted embedded firmware but had trouble figuring out its base address to load into IDA?
    Take clues from string ptrs: use a script such as [https://github.com/sgayou/rbasefind](https://github.com/sgayou/rbasefind) to find which base address aligns the most poinetrs to valid strings and go according to that.
    
    {% include image.html url="/assets/img/posts/30-re-tips/good-and-bad-load-base-str-ptrs.png" caption="string pointers give clues to the binary's base offset" alt="" %}
    
    By the way, the linked code is written in Rustlang, a treat for all you rust lovers ;) <3

    [(link to tweet)](https://twitter.com/whtaguy/status/1251044753740881921?ref_src=twsrc%5Etfw)

18. Building on the last tip, another way to find a fw’s base addr is using absolute calls:
    Find which base address results in the most calls “landing” at the beginnings of functions (code init’ing the frame pointer & allocating stack vars)
    
    {% include image.html url="/assets/img/posts/30-re-tips/good-and-bad-load-base-func-calls.png" caption="Absolute function calls algo give clues to the binary's base offset" alt="" %}

    [(link to tweet)](https://twitter.com/whtaguy/status/1251405871927693313?ref_src=twsrc%5Etfw)

19. Did you find a bunch of strings, but no xref to their use? You probably found a str array: the strs are accessed through an offset from the 1st string \(array base\), which _will_ have an xref
    
    
    {% include image.html url="/assets/img/posts/30-re-tips/cuda_error_table-signature-marked.png" caption="" alt="" %}
    {% include image.html url="/assets/img/posts/30-re-tips/accessing_cuda_errors-signature-marked.png" caption="" alt="" %}

    
    Code above is from Nvidia’s NvCamera64.dll. Specifically in this example, I found an array of structs in the form \<ErrorCode, ErrorString, ErrorDescription\> (the example isn't a *pure* string table, but a struct table\). 

    Also, the ErrorCode field is redundant as it can be determined from the struct’s index in the array. Error ID 0 = ErrorTable\[0\]
    <br />
    After creating a struct and defining proper types, this is the decompiled code: 
    {% include image.html url="/assets/img/posts/30-re-tips/accessing-cuda-errors-decompiled.png" caption="" alt="" %}

    [(link to tweet)](https://twitter.com/whtaguy/status/1251775461912449024?ref_src=twsrc%5Etfw)

20. Tips to improve your GDB debugging
    <br />
    * GDB Tip 1:
    
    The ability to break on reading/writing memory is well known, BUT did you know you can break on a write to a register?
    
    {% include image.html url="/assets/img/posts/30-re-tips/register-watchpoint.jpg" caption="register watchpoint in GDB" alt="" %}

    * GDB Tip 2:
    Anytime GDB prints $\<number\>, it is actually creating a new variable you can use:
    
    {% include image.html url="/assets/img/posts/30-re-tips/all_dollars_are_vars.jpg" caption="everything starting with a $ sign is a variable in GDB" alt="" %}

    * GDB Tip 3:
    3 ways to write to memory in GDB \(commands in back\-ticks\):

        1. use `set`
        2. `call` memcpy/strcpy
        3. write data from a file to memory with `restore`
        
    {% include image.html url="/assets/img/posts/30-re-tips/write-to-memory-functions.jpg" caption="different ways of writing to memory" alt="" %}

    [(link to tweet)](https://twitter.com/whtaguy/status/1252170154424913921?ref_src=twsrc%5Etfw)

21. Trick to help find popular libc functions in oceans of code: take advantage of their high xref count. Commonly used function like `memcpy` and `strlen` will be invoked often, while `strtok` for example will be used much less.

    [(link to tweet)](https://twitter.com/whtaguy/status/1252514742196940800?ref_src=twsrc%5Etfw)

22. Reverse Engineering String hunting tips
    <br />
    Strings help identify code functionality: finding the "login successful" string will show you were the login logic is. But how can we search for a string we target program use if it's not IDA’s strings GUI window?

    * The strings window only has auto\-identified strings, which misses some. 
    
        {% include image.html url="/assets/img/posts/30-re-tips/strings-window.png" caption="IDA's string windows only shows automatically identified strings found through heuristics, often missing some" alt="" %}

    * Use ALT\-B to search the whole binary, even places not identified as data regions.

        {% include image.html url="/assets/img/posts/30-re-tips/alt-b-window.png" caption="use ALT-B to search for strings" alt="" %}

    * Sometimes the string isn’t embedded in the binary, but is imported from an external resource file. Find these strings by grepping the program's installation folder (usually that's where the resource files are too)
    
        {% include image.html url="/assets/img/posts/30-re-tips/recrusive-grep.jpg" caption="using recursive grep to find a string in the program's installation folder" alt="" %}

    * Another method is Googling the string to see if it’s system generated. Error strings especially, might be take right from library functions such as perror()/FormatMessageA(), in which case the string won’t be in the exe
    
        {% include image.html url="/assets/img/posts/30-re-tips/google-error-code.jpg" caption="for example, 'The system cannot start another process at this time' is a system error message that likely won't be found in the executable binary" alt="" %}

    * Last resort to find where a used string is loaded from: Time Travel Debugging.
        1. Start debugging the program with Time Travel Debugging
        2. when the string appears, scan the program’s memory for its address in ram
        3. go back in time to find when/where the string was put there
        [Time travel debbugging overview](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/time-travel-debugging-overview)

    [(link to tweet)](https://twitter.com/whtaguy/status/1252875648399335426?ref_src=twsrc%5Etfw)

23. The easiest way to find the `main()` function is by working in reverse: find which function’s return value \(saved in eax\) is passed to `exit()` - that’s `main()`
    
    {% include image.html url="/assets/img/posts/30-re-tips/find-main-sig-marked.png" caption="" alt="" %}

    [(link to tweet)](https://twitter.com/whtaguy/status/1253218484277960707?ref_src=twsrc%5Etfw)

24. Looking to find code handling certain logic?
    <br />
    Search previous versions’ release notes to find when that logic was last updated. Next, bindiff the version before and of that release. The difference will include your target logic.


    {% include image.html url="/assets/img/posts/30-re-tips/bindiff-primary-secondary.jpg" caption="Bindiff tool showing what has changed between two version of a binary" alt="" %}

    [(link to tweet)](https://twitter.com/whtaguy/status/1253788639655575557?ref_src=twsrc%5Etfw)

25. Get the best from both IDA’s decompiler & disassembler by overlaying the C code on the ASM in graph view \(by clicking the “/” key\)
    
    {% include image.html url="/assets/img/posts/30-re-tips/combine-ida-views.png" caption="combining views" alt="" %}

    [(link to tweet)](https://twitter.com/whtaguy/status/1254093626126274560?ref_src=twsrc%5Etfw)

26. Ensure malware you’re researching won’t accidentally run by appending “.dontrunme” to the extension, which will prevent Windows from executing the file when it’s double clicked. On linux just `chmod \-x`
    
    {% include image.html url="/assets/img/posts/30-re-tips/makred-file-extension-handler.png" caption="change the extension to prevent accidental infection" alt="" %}

    Any non-system extension like ".abcdefgh" will work, though I like ".dontrunme" because it's clearer.

    [(link to tweet)](https://twitter.com/whtaguy/status/1254433729449164806?ref_src=twsrc%5Etfw)

27. Sometimes you can reverse a function solely by looking at its call graph. The function calls to/from a target reveal a lot about what the function does. Let’s analyze some examples.
    
    {% include image.html url="/assets/img/posts/30-re-tips/call-graph-example-marked.png" caption="" alt="" %}

    function sub\_4a6c60’s call graph is below. Focus on direct calls \(direct arrow from sub\_4a6c60 to somewhere else\) and make an educated guess about sub\_4a6c60’s functionality 
    
    {% include image.html url="/assets/img/posts/30-re-tips/call-graph-anal2-marked.png" caption="sub_4a6c60 call graph" alt="" %}

    Because sub\_4a6c60 calls `LoadLibraryExW`, `FindResource`, `LoadResource`, `SizeofResource`, and `FreeLibrary` \-all functions related to loading resources\- we can \(safely\) assume sub\_4a6c60 handles loading resources. We figured this all out without looking at one line of code\! 
    
    <br />
    
    Let’s look at another example: sub\_82061A’s call graph is below. 
    {% include image.html url="/assets/img/posts/30-re-tips/call-graph-example-2-marked.png" caption="sub_82061A call graph" alt="" %}
    
    Notice this function is called from *many* places. This limits the logic this function could have handled, since its code must be useful for many other unique functions. With this in mind, further analysis showed this is a panic function.

    <br />

    For this type of analysis use IDA's "Proximity browser" opened from the View menu.
    {% include image.html url="/assets/img/posts/30-re-tips/proximity-view-open-marked.png" caption="opening Proximity browser" alt="" %}

    [(link to tweet)](https://twitter.com/whtaguy/status/1254795152264171525)

28. IDA auto\-analysis missed a function argument because it was passed in an “unexpected” register?
    <br />
    Use the "\_\_usercall” call convention with “@\<register\_name\>” to declare function arguments & their location:
        
    {% include image.html url="/assets/img/posts/30-re-tips/usercall-marked.png" caption="defining a custom calling convention for a function" alt="" %}

    [(link to tweet)](https://twitter.com/whtaguy/status/1255162303823126528?ref_src=twsrc%5Etfw)

29. Buckle up buckaroos! Here's the most useful RE strategy no one talks about:
    <br />
    **Analyze the block layout before diving into ASM code**. Layout view is available on many disassemblers, here’s how to best use it.

    {% include image.html url="/assets/img/posts/30-re-tips/intro-marked.png" caption="" alt="" %}

    Let’s start with an easy example. The 1st image shows the layout of an “if” statement: the code splits to 2 paths.
    {% include image.html url="/assets/img/posts/30-re-tips/if-statement-layout-hidden-marked.png" caption="if statement as seen in assembly block view" alt="" %}

    Exercise: What layout does the picture below show? Answer is below. (And the assembly is purposely hidden! That’s the point of this ;)
    {% include image.html url="/assets/img/posts/30-re-tips/switchcase-layout-hidden-asm-marked.png" caption="what is this layout?" alt="" %}

    If you answered it’s a switch case statement, you’re correct :\) Great\!
    Let’s use our new killer skillz on func\_1. Question: func\_1 is most likely: <br />
        1. Computing a hash
        2. Parsing a format
        3. String comparison
    {% include image.html url="/assets/img/posts/30-re-tips/best-consecutive-checks-marked.png" caption="what is func_1 doing?" alt="" %}

    The answer is \#2, Parsing a format. Func\_1 has many “if”s leading to a return block \(End A\), typical of format parsing code to bail early if it finds a corrupt field/magic value. Here is func\_1 fully exposed to confirm our assumption:
    {% include image.html url="/assets/img/posts/30-re-tips/best-consecutive-checks-exposed-marked.png" caption="func_1 exposed. This function is parsing the header of an MZ exectable" alt="" %}
    
    Last example; what is func\_2 most likely doing? 
        1. Computing a hash
        2. Parsing a format
        3. String comparison
    Answer is below the image.
    {% include image.html url="/assets/img/posts/30-re-tips/raw-check-equal-strs-hidden-asm-marked.png" caption="what is func_2 doing?" alt="" %}


    The answer is \#3, String comparison. Func\_2 has a loop, typical in str related funcs which use the loop to iterate over the str’s chars. Also, we can rule out \#2 with knowledge from the previous exercise (func_1), and we can rule out \#1 (Computing a hash) from the very first reverse engineering tip I gave: has functions are long and branchless.

    {% include image.html url="/assets/img/posts/30-re-tips/cmp-strs-better-view-marked.png" caption="func_2 naked and exposed" alt="" %}    
    
    [(link to tweet)](https://twitter.com/whtaguy/status/1255675856594272258?ref_src=twsrc%5Etfw)

30. The Reverse Engineering tip better than anything technical I can share: Reverse Engineering isn’t a 1337 h4ck3r only reserved field\! Like anything else: It’s open to everyone\! And like any skill, “just” enjoy it, work hard, and you’ll get it.

    Easier said than done, but don’t be discouraged when things are hard. I like to think of [this](https://twitter.com/maddiestone/status/1148264296821903361?s=20) [@maddiestone](https://twitter.com/maddiestone) story where she learned a lot from reversing printf when she was just a begginer in reverse engineering.
    <br />
    A setback is only a waste if you didn't learn anything from it

    <br />
    [(link to tweet)](https://twitter.com/whtaguy/status/1256142278420312064?ref_src=twsrc%5Etfw)


If you think I earned your follow, my twitter is [@whtaguy](https://twitter.com/whtaguy) and I appreciate you showing support. Thank you!

If you have any questions leave a comment on this post or tag me on Twitter \- I reply fairly quickly :\)
