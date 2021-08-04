---
layout: post
title: >
    Windows Source Code Leaks & A Story Of Lost Source Code
hide_title: false
tags: ['Reverse Engineering', 'C/C++']
excerpt: When researching or just tinkering with Windows and Microsoft executables, having the source code is a great advantage. This short article is a collection of links to Windows and Microsoft code, and a story about it.
author: guy
---
*Disclaimer: The information presented in this blog post is for educational purposes only.*

When researching or just tinkering with Windows and Microsoft executables, having the source code is a great advantage. This short article is a collection of links to Windows and Microsoft code.
## Leaked Windows Source Code
Links to leaked Windows source files:
* [Windows XP \(NT5\) https://github.com/cryptoAlgorithm/nt5src](https://github.com/cryptoAlgorithm/nt5src)
* [Windows NT4 https://github.com/ZoloZiak/WinNT4](https://github.com/ZoloZiak/WinNT4)
* [Windows 2000 https://github.com/pustladi/Windows\-2000](https://github.com/pustladi/Windows-2000)
* [Windows Research Kit https://github.com/Zer0Mem0ry/ntoskrnl](https://github.com/Zer0Mem0ry/ntoskrnl)

## Official Microsoft Published Source Code
Microsoft has recently become significantly more Open Source oriented, and has even started actively developing Open Source projects and publishing some of its own code.
Links to published Microsoft code:
* [Microsoft .NET source https://referencesource.microsoft.com/](https://referencesource.microsoft.com/)
* [Microsoft’s Github https://github.com/Microsoft](https://github.com/Microsoft)

## Other Resources
Other resources to help you with your Windows and Microsoft adventures:
* [ReactOS](https://reactos.org/) \- An Open Source OS implementing Window’s API and functionality
* [Windows Internals Book](https://docs.microsoft.com/en-us/sysinternals/learn/windows-internals) \- Published by Microsoft Press, this book describes Window’s internal mechanisms

## A Note On Lost Source Code
Managing a giant code base \(and the corresponding compilation tool chain\) is a difficult task that gets harder as the code grows and ages, so it is not unexpected that Microsoft, a software giant founded over 40 years ago, occasionally misplaces a file or loses the ability to compile it. This is exactly what happened with the Microsoft Office Equation Editor’s code. 

[CVE\-2017\-11882](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2017-11882) is a vulnerability in the Equation Editor in Microsoft Office that could be exploited by an attacker to run malicious code on a victim’s computer when the victim opens a malicious Word document. Microsoft typically fixes these vulnerabilities by updating the source code, recompiling it, and then publishing a new executable file with the fix. However, this was not the case with the Equation Editor: instead, it seems Microsoft has lost its source code \(or ability to compile it\) and opted to directly patch the assembly of the executable file\! 

Directly patching the assembly proved to be unsustainable – as new vulnerabilities in the Equation Editor were surfaced by security researchers, Microsoft eventually disabled the Equation Editor completely. 

In conclusion, it’s entirely possible that the source code for the module you’re researching, something you dream of attaining, is the same dream of a Microsoft engineer. Think about that. It’s beautiful how common struggles connect people 💘

Read more about the Equation Editor vulnerability [here](https://blog.0patch.com/2017/11/did-microsoft-just-manually-patch-their.html) and [here](https://unit42.paloaltonetworks.com/unit42-analysis-of-cve-2017-11882-exploit-in-the-wild/)
