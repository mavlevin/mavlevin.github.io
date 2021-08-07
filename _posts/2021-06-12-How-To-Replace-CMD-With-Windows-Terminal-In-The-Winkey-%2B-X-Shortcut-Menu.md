---
layout: post
title: >
    How To Replace CMD With Windows Terminal In The Winkey + X Shortcut Menu
hide_title: false
tags: ['Tools', 'not security']
excerpt: I instincitvely use the Winkey+X menu to open cmd, but I want to upgrade to Windows Terminal. This is my hacky solution to add Windows Terminal to the Winkey+X menu (before it was natively supported in Windows 11).
# thumbnail: "assets/img/thumbnails/feature-img/desk-messy.jpeg"
author: guy
---

# Update: Windows 11 Now natively supports this!

{% include image.html url="/assets/img/posts/Windows-terminal-in-winkey-x-menu.jpg" %}

Windows 11 now natively supports the Windows Terminal in the winkey + X menu! So there's no need for this post, but I'll leave it for the curious readers and those of us on Windows 10.

---

I want to upgrade from CMD to Microsoft's fancy & shiny Windows Terminal. However, the Windows Key\+X then C shortcut I use to open a shell \(or Windows Key \+ X then A for Powershell by default\) doesn’t support opening Windows Terminal. Here’s how I patched that keyboard shortcut to open Windows Terminal.

*Disclaimer: This is my own patchy fix found for fun at 4am after a night of hacking. Proceed at your own risk.*

{% include image.html url="/assets/img/posts\trimmed+with+subtitles.gif" caption="" alt="" %}
# How To Replace CMD With Windows Terminal In The Winkey \+ X Menu Shortcut
## Option 1: Via Powershell

1. Ensure the `Winkey+X C`  shortcut opens cmd \(and not powershell\) by going to Settings >  Personalization > Taskbar. Set “Replace Command Prompt with  PowerShell …” to Off
	{% include image.html url="/assets/img/posts\make+sure+using+cmd+and+not+powershell.jpg" caption="" alt="" %}

2. Open Powershell. I use Winkey \+ R, then type “powershell” and enter. 
	{% include image.html url="/assets/img/posts\opening+powershell.jpg" caption="" alt="" %}

3. Copy and paste the following code snippet into the powershell window
	```
	# modify Winkey + X, C
	$shortcut = (New-Object -ComObject 'WScript.Shell').CreateShortCut("C:\Users\$env:UserName\AppData\Local\Microsoft\Windows\WinX\Group3\02 - Command Prompt.lnk")
	$shortcut.TargetPath = (where.exe wt) -join "`n"
	$shortcut.Save()

	# modify Winkey + X, A (shortcut to open administrator shell)
	$shortcut = (New-Object -ComObject 'WScript.Shell').CreateShortCut("C:\Users\$env:UserName\AppData\Local\Microsoft\Windows\WinX\Group3\01 - Command Prompt.lnk")
	$shortcut.TargetPath = (where.exe wt) -join "`n"
	$shortcut.Save()
	```

## Option 2: Manually

1. Ensure the `Winkey+X C` shortcut opens cmd \(and not powershell\) by going to Settings > Personalization > Taskbar and setting “Replace Command Prompt with PowerShell …” to Off. See step 1 in the powershell method above for a screenshot of the relevant settings pane.

2. Open a regular cmd and enter `where wt` to find the Windows Terminal executable path. This will be used later

	{% include image.html url="/assets/img/posts\where+wt+for+cmd.jpg" caption="" alt="" %}

3. Navigate with explorer to `C:\Users\*your_username*\AppData\Local\Microsoft\Windows\WinX\Group3`. The folder should look similar to mine below

	{% include image.html url="/assets/img/posts\winX+group3.jpg" caption="" alt="" %}

4. Backup both “Command Prompt” files by copying them to somewhere safe
5. Right\-click and open the Properties of the first Command Prompt shortcut file

	{% include image.html url="/assets/img/posts\right+click+and+then+open+properites+window+of+shorctu+file.jpg" caption="" alt="" %}

6. Change the Target to the Windows Terminal path we got from Step 2. Then click OK to close the window

	{% include image.html url="/assets/img/posts\change+target+of+shorcut+to+windows+terminal+wt.jpg" caption="" alt="" %}

7. Repeat the previous step for the other Command Prompt shortcut file. This will ensure both opening a regular shell \(winkey \+ X, C\) and an administrator shell \(winkey \+ X, A\) use Windows Terminal instead of cmd.

8. Bask in the joy of using Windows Terminal each time you Winkey\+X C 


## How To Revert The Winkey \+ X Menu to use CMD

### Option1: Via Powershell

1. Open Powershell. I use Winkey \+ R, then type “powershell” and enter.
2. Copy and paste the following code
	```
	# modify Winkey + X, C
	$shortcut = (New-Object -ComObject 'WScript.Shell').CreateShortCut("C:\Users\$env:UserName\AppData\Local\Microsoft\Windows\WinX\Group3\02  - Command Prompt.lnk")
	$shortcut.TargetPath = (where.exe cmd) -join "`n"
	$shortcut.Save()
	# modify Winkey + X, A (shortcut to open administrator shell)
	$shortcut = (New-Object -ComObject 'WScript.Shell').CreateShortCut("C:\Users\$env:UserName\AppData\Local\Microsoft\Windows\WinX\Group3\01 - Command Prompt.lnk")
	$shortcut.TargetPath = (where.exe cmd) -join "`n"$shortcut.Save()
	```


### Option 2: Manually
1. Navigate to `C:\Users\*your_username*\AppData\Local\Microsoft\Windows\WinX\Group3`.
2. Replace the modified Command Prompt files with the original ones backed up in step 4


## How I Reached This Solution
Obviously, it’s unacceptable that Winkey \+ X, C doesn’t support opening Windows Terminal. It was time I did something, and at 4am with nothing better to do, nothing could stop me.
### Trying The Simple, Stupid Way \(And Solving Permission Issues\)
My naive approach was to simply overwrite `C:\Windows\System32\cmd.exe` with the contents of `C:\Users\Guy\AppData\Local\Microsoft\WindowsApps\wt.exe`. The plan: when the shortcut triggers cmd.exe, it will actually run wt.exe.
I first needed to wrestle with file permissions because cmd.exe \(and most things in System32\) is owned by Windows `TrustedInstaller`, who won’t let me modify the file. To solve this, I smugly changed cmd.exe’s owner to the Windows `everyone` user and overwrote the file’s data. Awesome possum – now everything should work. 
Except – when I ran wt.exe independently \(just to check I didn’t break anything\), I got a peculiar error:

{% include image.html url="/assets/img/posts\error+launching+cmd+when+it+is+actually+wt.jpg" caption="" alt="" %}
Apparently, we can’t just override the data in cmd.exe because Windows Terminal seems to use the original cmd somewhere in its internals. Let’s use the ostrich approach \(stick our heads in the ground and ignore the problem\) and keep going: I entered the Winkey \+ X, C shortcut and got a more interesting error:

{% include image.html url="/assets/img/posts\link+failing.jpg" caption="" alt="" %}
### Error\-based Discovery
The error window shows it’s coming from “02 \- Command Prompt.lnk”\! This is exciting\! It reveals part of the mechanism’s internals: an .lnk file that is used in the process of opening the cmd. Can we use this to our advantage?
I inspected the .lnk file in the title of the error window, changed it’s “target” property to Windows Terminal, and when I reentered the Winkey \+ X, C shortcut Windows Terminal popped open\! Success\!

{% include image.html url="/assets/img/posts\trimmed+with+subtitles.gif" caption="" alt="" %}
