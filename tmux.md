## Tmux 概念和使用

tmux 中有几个重要概念：

- 会话(session): 建立一个 tmux 工作区会话，会话可以长期驻留，重新连接服务器不会丢失，我们只需重新 tmux attach 到之前的工作区就可以恢复会话，这样你的工作区就可以常驻服务器了，非常方便
- 窗口(window): 容纳多个窗格
- 窗格(pane): 可以在窗口中分成多个窗格，每个窗格都可以运行各种命令