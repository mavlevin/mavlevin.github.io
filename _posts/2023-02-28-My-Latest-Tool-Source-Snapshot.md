---
layout: post
title: >
    My Latest Tool: Source Snapshot – Get An Overview of Source Code At A Glance
hide_title: false
tags: ['Tools', 'safe code']
author: guy
excerpt_separator: <!--more-->
---

<div>
<video muted loop autoplay width="100%" hieght="100%">
  <source src="/assets/video/posts/Source-Snapshot-Teaser.mp4" type="video/mp4" style="outline: none">
</video>
</div>

Announcing my latest developer tool: [Source Snapshot](https://source-snapshot.mavlevin.com/) – A source code directory visualizer for your browser.

<!--more-->

# Source Snapshot Use Case

**tl;dr: Use Source Snapshot to get an overview of a new codebase**

This tool was born out of a personal need. I often found myself trying to get a bird's eye view of a new codebase I had been introduced to, but couldn't find a free, online tool to do so! So I created Source Snapshot. Now, understanding source code file structure is just a drag away on [Source Snapshot](https://source-snapshot.mavlevin.com/).

# How To Use Source Snapshot

1. Go to [https://source-snapshot.mavlevin.com](https://source-snapshot.mavlevin.com/)
1. Drag in a source code folder ([privacy statement](https://source-snapshot.mavlevin.com/about))
1. The folder will be processed by your browser
1. Done! You can now hover, zoom, click, and explore the code structure



# Source Snapshot Development Story

Source Snapshot is an improvement on my previous tool, [SourceCodeVisualizer](https://github.com/mavlevin/SourceCodeVisualizer), which had to be installed and run locally. The inconvenient installation requirement limited the tool's use and popularity. 

Alternatively, Source Snapshot is a fully online tool that can be used by anyone, anywhere, as long as they have access to a browser and internet. 

For privacy and ease of deployment, I chose all logic to happen client-side in JavaScript. That way there would be no server that needs to scale to handle requests, and no user would be required to share their code with an external party.

Working on Source Snapshot was interesting since this was my first time writing more than 10 lines of client-side JavaScript. (I wrote in-browser client-side scrapers in js before, but those were small). It was a fun experience learning JavaScript's unique features, and I recruited my friend Emanuel Adamiak to help with the code. Fixing many bugs along the way, we eventually ended up with the functional, responsive, and lightweight code we have in the current version. I hope you enjoy it!

# Privacy

It important for me to give a small note on my tool's privacy: This tool was purposely built with privacy in mind. Files never leave your device and all processing and visualizing happen only on your device, never reaching the server.



Until next time!!