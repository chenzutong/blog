## 从服务器下载文件
```buildoutcfg
scp username@servername:/path/filename /tmp/local_destination
```
> 例如：scp bowei@192.168.1.110 /home/bowei/lotus.tar ~/Downloads

## 上传本地文件到服务器
```buildoutcfg
scp /path/local_filename username@servername:/path
```

## 从服务器下载整个目录
```buildoutcfg
scp -r username@servername:remote_dir/ /tmp/local_dir
```

## 上传目录到服务器
```buildoutcfg
scp  -r /tmp/local_dir username@servername:remote_dir
```