---
layout: post
title:  "使用AutoHotKey实现快捷键的重新映射"
date:   2022-07-17 23:01:00 +0800
categories: default
---

最近工作电脑换到了Windows, 因此原来在Mac下用Karabiner实现的快捷功能都没有了. 经过试用不用的键盘映射方案, 最后决定使用[AutoHotKey](https://www.autohotkey.com/).

以下是我写配置(第二版):

```ahk
#NoEnv  ; Recommended for performance and compatibility with future AutoHotkey releases.
; #Warn  ; Enable warnings to assist with detecting common errors.
SendMode Input  ; Recommended for new scripts due to its superior speed and reliability.
SetWorkingDir %A_ScriptDir%  ; Ensures a consistent starting directory.
#InstallKeybdHook
#InstallMouseHook

~LShift Up::
if (A_PriorKey = "LShift") {
    Send {(}
}
return

~RShift Up::
if (A_PriorKey = "RShift") {
    Send {)}
}
return

RAlt::
~LAlt Up::
if (A_PriorKey = "LAlt") {
    Send {LAlt down}
    Send {Space}
    Send {LAlt up}
}
return
~RAlt Up::
if (A_PriorKey = "RAlt") {
    Send {LWin down}
    Send {v}
    Send {LWin up}
}
return

CapsLock::Esc
```