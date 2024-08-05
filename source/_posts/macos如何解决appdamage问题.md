---
title: MacOS:如何解决"This app is damaged and can’t be opened. You should move it to the Trash"
date: 2024-08-05 18:06:16
tags: [macos]
mathjax: true
comments: true
---

Launch Terminal and then issue the following command:

> xattr -cr /path/to/application.app

For example:

> xattr -cr /Applications/Signal.app

The -c flag removes all attributes, whereas -r applies recursively for the entire targeted .app directory contents.

reference: https://www.cnblogs.com/chester-cs/p/13780730.html