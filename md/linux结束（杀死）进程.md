## OSError: [Errno 98] Address already in use解决办法
如要结束tmux
输入：ps -fA|grep tmux
> bowei     9569  9555  0 14:08 pts/5    00:00:00 tmux new  
> bowei     9571  1622  0 14:08 ?        00:00:00 tmux new  
> bowei     9606  9589  0 14:09 pts/7    00:00:00 grep --color=auto tmux  

输入：kill -9 9606   pid为9569