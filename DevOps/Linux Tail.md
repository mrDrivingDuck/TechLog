# Linux - Tail 命令

Created by : Mr Dk.

2018 / 12 / 03 09:32

Nanjing, Jiangsu, China

---

## Use

`tail` 命令本身用于查看文件内容

常用参数 `-f` 用于查阅正在改变的日志文件

`tail -f filename` 会把 `filename` 文件的最尾部内容显示在屏幕上，并不断刷新

只要 `filename` 更新就可以看到最新的文件内容

## Differences

`tail -f filename` 

* 相当于 `--follow=descriptor`
* 根据 **文件描述符** 进行追踪
* 文件 **改名** 或被 **删除** 时，追踪停止

`tail -F filename`

* 相当于 `--follow=name --retry`
* 根据 **文件名** 进行追踪，并保持重试
* 文件 **改名** 或 **删除** 后，若再次创建相同文件名，继续追踪

`tailf`

* 相当于 `tail -f -n 10`
* 如果文件不增长，则不会访问磁盘文件
* 省电

---

