---
layout: post
title: Run grunt from Intellij
---

I don't like to have 7 console windows/tabs open. Each time i can get something running inside an IDE i do that. Until
now grunt was an exception a I couldn't get it to run from Intellij. Here is my config for it:

1. Install Intellij Node.js plugin
2. Open task configuration editor
3. Add new node.js task
4. Set 'Path to node:' to you node executable eg. D:\Dev\nodejs\0.10.16\node.exe
5. Set 'Working directory' to a directory where your Gruntfile.js or Gruntfile.coffee is
6. Set 'Path to Node App JS file' to your grunt-cli exec file eg. C:\Users\catty\AppData\Roaming\npm\node_modules\grunt-cli\bin\grunt
7. Set 'Application parameters' to whatever grunt task you want to run or leave it blank to run default task
8. Leave everything else empty, even if you have your grunt confguration in Coffeescript, like me, don't set the
Coffeescript configuration

Done. Now You can build with grunt without leaving Intellij :)
