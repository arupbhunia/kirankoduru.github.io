---
layout: post
title:  "7 shortcuts of a highly effective Sublime Text user"
date:   2017-10-24 11:08:00
updated: 2017-10-31 12:04:45
categories: python
description : Start using sublime text like a ninja, well sort-of.
author: Kiran Koduru
image: /img/sublime-text-ninja.png
---

Through my career as a software developer, I have appreciated one text editor the most, [Sublime Text](https://www.sublimetext.com/). I began with writing code in *Notepad++* long long time ago, then tried IDEs as well but nothing came as close to working smoothly as Sublime Text. *This blog is also written using Sublime Text*.

Once you have imported your project into Sublime Text workspace, using the following 7 shortcuts will help you get close to being a Sublime Text power user.

### Goto Anywhere

IDEs have a shortcut key to goto a class name, a different one to goto a function name and another one for symbols. This can cause a lot of confusion on how to navigate a file. And if you are anything like me, you'd end up carrying a cheat sheet to look up each time.

For Sublime Text, all you have to do is hit `Ctrl + R` or `⌘ + R` on Mac OS to navigate to any function/class/symbol in the file you are currently editing.

### Open any file

While you could search for a filename through your operating system easily, Sublime Text allows you to goto any filename fairly quickly. Just hit `Ctrl + P` or `⌘ + P` to open any file in your workspace.

### Show/Hide sidebar

Some people prefer working with more than one monitor. I personally like having a distraction free screen and seldom resort to using more than one screen. For times like these, I need more screen space for code.

I toggle my Sublime Text sidebar by hitting the following keys in succession `Ctrl + K` and `Ctrl + B` or `⌘ + K` and `⌘ + B` on Mac OS.

### Duplicate lines

Though being [DRY](https://en.wikipedia.org/wiki/Don%27t_repeat_yourself) is a very good software practice, you may have had to copy and paste a line of code/text numerous times when working on a project.

For this, the combination of the following keys will make your life easier. Hit `Ctrl + Shift + D`*(Duplicate line)* then `Ctrl + X`*(Cut)* and `Ctrl + V`*(Paste)* or `⌘ + ⇧ + D`*(Duplicate Line)* then `⌘ + X`*(Cut)* and `⌘ + V`*(Paste)*.

### Goto line number

Error reporters or loggers always direct you to a specific line number in any given file. Hit `Ctrl + G` or `^ + G` on Mac OS to goto any given line number. Use this in combination with the __[Open any file](#open-any-file)__ shortcut to open a file in your Sublime Text workspace.

### Multiple cursors

This feature has been the a major selling point of Sublime Text<sup><span title="I base this claim on no research evidence, just pure instinct">*</span></sup>. I do believe it's the first thing you see when you are on the [sublimetext.com](https://www.sublimetext.com/) site.

First select any given word or words you'd like to edit. Then hit the `Ctrl + D` or `⌘ + D` to select occurrence of the selected word one by one. You can also hit `Alt + F3` or `⌃ + ⌘ + G` to select all occurrences of the word in a given file.

### Spell Check

Authors, bloggers, tech writers and programmers interact with text for documentation and code. One big feature they forget to turn on in Sublime Text is using spell check. Make less typos when fixing your code or documentation. 

Hit function `F6` to toggle spell check in Sublime Text.

### Do Anything [BONUS]

Added bonus from reddit user [/u/LightShadow](https://www.reddit.com/r/Python/comments/78gkgn/7_shortcuts_of_a_highly_effective_sublime_text/dotnitp/). You can do literally anything by hitting `Ctrl + Shift + P`. This opens the command pallete or a autocomplete like dialog that allows you to type or do anything in Sublime Text.

### Useful packages to install

Working as a python developer, I have found the following Sublime Text plugins the most useful.

- __[Sublime Linter](http://www.sublimelinter.com/en/latest/)__: For your linting needs. Configure once and forget. This allows you to do static code analysis on most languages. Works really well for python.
- __[editorconfig](http://editorconfig.org/)__: Another gem that will take care if configured correctly to delete any trailing lines, inserting final new line to any given file, picking tabs or spaces when editing and more. This will run each time you save a file in Sublime Text.
- __[Mark down](https://github.com/revolunet/sublimetext-markdown-preview)__: For when you would like to preview your markdown files as you are writing them. 

Here's the summary of shortcut keys if you prefer to print it out this section as a cheat sheet.

| Sublime Text Shortcut Commands| Mac OS  | Windows/Linux  |
|:--------------:|:-------:|:-------------------:|
|Goto Anywhere|`⌘ + R`|`Ctrl + R`|
|Open any file|`⌘ + P`|`Ctrl + P`|
|Show/Hide sidebar|`⌘ + K` and `⌘ + B`|`Ctrl + K` and `Ctrl + B`|
|Duplicate lines|`⌘ + ⇧ + D`(Duplicate Lines), `⌘ + X`(Cut) and `⌘ + V`(Paste)|`Ctrl + Shift + D`(Duplicate line), `Ctrl + X`(Cut) and `Ctrl + V`|
|Goto line number|`⌘ + G`|`Ctrl + G`|
|Multiple cursors|`⌘ + D`|`Ctrl + D`|
|Spell check|`F6`|`F6`|
|Do Anything|`Ctrl + Shift + P`|`Ctrl + Shift + P`|


__*__ I base this claim on no research evidence, just pure instinct
