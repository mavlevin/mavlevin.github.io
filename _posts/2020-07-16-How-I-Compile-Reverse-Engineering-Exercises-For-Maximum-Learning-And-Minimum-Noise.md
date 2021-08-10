---
layout: post
title: >
    How I Compile Reverse Engineering Exercises For Maximum Learning And Minimum Noise
hide_title: false
tags: ['Reverse Engineering', 'exercise', 'Assembly', 'C/C++']
excerpt_separator: <!--more-->
author: guy
redirect_from: /2020/08/how-i-compile-reverse-engineering.html
---
Imagine if your first reverse engineering exercise was to reconstruct an encrypted IAT – if you don’t fully know what that means, that’s the point: beginner reverse engineering exercises should be clear (and fun)!
<!--more-->
Anything that can throw off the analysis of a reverse engineer, such as optimized inlined functions, shouldn’t be in beginner exercises. Secondly, I would like my exercises to run on as many students' computers as possible. These are the goals I strive for when creating [CyberQueens](https://cyberqueens.org/) exercises, and here is how I configure my compiler to meet those goals.
# Ensuring A Clear And Concise Executable is Compiled
To prevent the compiler from adding any unintended opcodes or logic, which could confuse aspiring reverse engineers, set all of the following build properties:
1. **To disable automatic uninitialized memory checks \(and other debugging\) checks compiled into the code**, set the compilation target to Release. This can be done from VS’s main page, as seen in Figure 1. 

    {% include image.html url="/assets/img/posts\selecting+release-marked.png" caption="Figure 1" alt="" %}

2. **To obtain the most straightforward compilation from C/C\+\+ to Assembly**,  disable optimizations. Certain optimizations, such as inlining  functions, can really confuse beginner reverse engineers that haven’t  yet familiarized themselves with more fundamental concepts. As seen in  Figure 2, Disable optimizations from Configuration Properties > C/C\+\+ > Optimizations

    {% include image.html url="/assets/img/posts\select+optimizations+disabled-marked.png" caption="Figure 2" alt="" %}


Depending on the intended difficulty, it is also possible to change the optimization settings. The more optimizations enabled, the more convoluted the generated code, creating a harder reverse engineering exercise.
# Ensuring Executables Run On All Windows Versions \(Compiling For Compatibility\)
To ensure an executable can run on any version of windows \(from Windows XP to the latest and greatest version of Windows 10\), set all of the following build properties:
1. **For cross compatibility with older 32\-bit computers**, I set the target architecture to 32 bits. 64\-bit Windows can run 32\-bit applications, but 32\-bit Windows can’t run 64 bit applications, making 32\-bit the most compatible. Change this setting from the VS’s main page, as seen in Figure 3.

    {% include image.html url="/assets/img/posts\selecting+32+bit-marked.png" caption="Figure 3" alt="" %}

2. **For cross compatibility with any Windows MFC Version**, unintuitively, set the MFC to be “in a Shared DLL”. This will allow you to publish the target .exe as is, without additional dependent libraries. Change this setting from the project Configuration Properties \(open by right clicking the project name in the Solution Explorer\) > Advanced > Use of MFC, as shown in figure 4.

    {% include image.html url="/assets/img/posts\selected+shared+library+mfc-marked.png" caption="Figure 4" alt="" %}

3. **For cross compatibility with any Windows Platform Toolset**, I select the oldest available toolset. This should be Windows XP. Change this setting from the project Configuration properties > General > Platform Toolset, as shown in figure 5.

    {% include image.html url="/assets/img/posts\selecting+platform+toolset-marked.png" caption="Figure 5" alt="" %}


There. That's all the magic. Now you too can compile and create reverse engineering exercise\-worthy executables 🥰