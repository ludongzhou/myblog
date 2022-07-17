---
layout: post
title:  "使用AutoHotKey实现快捷键的重新映射"
date:   2022-07-17 23:01:00 +0800
categories: default
---

最近工作电脑换到了Windows, 因此原来在Mac下用Karabiner实现的快捷功能都没有了. 经过试用不用的键盘映射方案, 最后决定使用[AutoHotKey](https://www.autohotkey.com/).

以下是我写配置(第一版):

```ahk
#NoEnv  ; Recommended for performance and compatibility with future AutoHotkey releases.
; #Warn  ; Enable warnings to assist with detecting common errors.
SendMode Input  ; Recommended for new scripts due to its superior speed and reliability.
SetWorkingDir %A_ScriptDir%  ; Ensures a consistent starting directory.

~LShift Up:: ; 左shift单击时输出(
if (A_PriorKey = "LShift") {
    Send {(}
}
return

~RShift Up:: ; 右shift单击时输出)
if (A_PriorKey = "RShift") {
    Send {)}
}
return

RAlt:: ; 取消右alt的功能, 因为和单击alt功能冲突
~LAlt Up:: ; 单击左alt调出搜索框, 模仿alfred
if (A_PriorKey = "LAlt") {
    Send {LAlt down}
    Send {Space}
    Send {LAlt up}
}
return
~RAlt Up:: ; 单击右alt调出剪贴板历史
if (A_PriorKey = "RAlt") {
    Send {LWin down}
    Send {v}
    Send {LWin up}
}
return

CapsLock::Esc ; capslock重映射为esc(vim用户福音)

d::Send {d} ; 点击d键输出d. 因为下面的规则使用d作为组合键的前缀, 按照ahk的规定, 组合键的前缀会失去原功能, 因此需要重新声明一下
+d::Send {D} ; ditto
d & h::Left ; d + h为方向键←
d & j::Down ; d + j为方向键↓
d & k::Up ; d + k为方向键↑
d & l::Right ; d + l为方向键→
d & w:: ; 防止按键过快, 导致d被识别为组合键前缀, 导致无法输出
Send {d}
Send {w}
return
d & a::
Send {d}
Send {a}
return
d & i::
Send {d}
Send {i}
return
d & u::
Send {d}
Send {u}
return
d & o::
Send {d}
Send {o}
return

!w:: ; alt + w为关闭tab
Send {LControl down}
Send {w}
Send {LControl up}
return
!e:: ; alt + e为idea展示最近使用的文件
Send {LControl down}
Send {e}
Send {LControl up}
return
!r:: ; alt + r为chrome刷新页面
Send {LControl down}
Send {r}
Send {LControl up}
return
!t:: ; alt + t为打开新的tab
Send {LControl down}
Send {t}
Send {LControl up}
return
!a:: ; alt + a为选中所有
Send {LControl down}
Send {a}
Send {LControl up}
return
!s:: ; alt + s为保存
Send {LControl down}
Send {s}
Send {LControl up}
return
!d:: ; alt + d为新建书签
Send {LControl down}
Send {d}
Send {LControl up}
return
!f:: ; alt + f为搜索
Send {LControl down}
Send {f}
Send {LControl up}
return
!l:: ; alt + l为focus在地址栏
Send {LControl down}
Send {l}
Send {LControl up}
return
RAlt & l:: ; 右alt + l为focus在地址栏(因为上面把右alt的功能取消了, 在这里重新显式设置)
Send {LControl down}
Send {l}
Send {LControl up}
return
!z:: ; alt + z为撤销更改
Send {LControl down}
Send {z}
Send {LControl up}
return
!x:: ; alt + x为剪切
Send {LControl down}
Send {x}
Send {LControl up}
return
!c:: ; alt + c为复制
Send {LControl down}
Send {c}
Send {LControl up}
return
!v:: ; alt + v为粘贴
Send {LControl down}
Send {v}
Send {LControl up}
return
!Space:: ; alt + space为切换输入法
Send {LWin down}
Send {Space}
Send {LWin up}
return
```