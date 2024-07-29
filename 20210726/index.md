# 删除进程操作


## 删除指定端口的所有进程

```bash
sudo kill -9 $(lsof -i:端口号 -t)
```

## 删除指定名称的所有进程

```bash
sudo kill -9 $(pidof 名字)
```

## 查看指定端口占用情况

```bash
sudo lsof -i:端口号
```

## 删除指定pid的进程

```bash
sudo kill -9 pid号
```
