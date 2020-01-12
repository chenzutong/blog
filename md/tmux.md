# tmux
tmux是一个优秀的终端复用软件

## 安装
ubantu：
```buildoutcfg
sudo apt-get install tmux
```

## 基本命令
| 命令 | 说明 |
| --- | --- |
| tmux new -s <name> | 新建名为<name>的会话 |
| tmux new -d | 脱离当前会话（不进入会话） |
| tmux new -c | 创建会话的路径 |
| tmux ls | 列出当前所有的会话 |
| tmux a -t <name> | 进入<name>会话 |
| tmux kill-session -t <name> | 关闭<name>会话 |
| tmux kill-server | 关闭所有会话 |

在窗口中退出，输入
> exit