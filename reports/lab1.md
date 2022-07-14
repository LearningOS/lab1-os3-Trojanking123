# 简答作业

** Trojanking123 **

## 功能总结
实现了查询系统运行时任务状态的接口
1. 状态查询，因为时查询运行时状态，所以总是running
2. 耗时查询，first time 是第一次调度时的时间即 run_first 或者 run next 时初始化，now 即调用接口时时间，相减即可
3：system call 查询次数：放在 trap handler，把 call id 传入即可

## 问答题

### 1
陷入到trap handler 进行异常处理了       

### 2

#### 2.1
a0 指向kernel stack，即保存的寄存器什么的

1. 恢复到另一个运行程序
2. 开始一个程序

#### 2.2

操作了 sstatus，sepc， sscratch。

恢复了了sstatus里保存的中断设置等状态信息，把sepc 设置为了U态下一个应该执行的指令地址

sscratch 设置为了user stack，便于trap 后恢复上下文

#### 2.3

x2: sp需要设置为sscratch寄存器里面的值，但是需要临时寄存器中转，所以要先保存临时寄存器里面的，在通过临时寄存器读取sscratch，然后保存


x4: 没用到

#### 2.4

sp 指向user stack, sscratch 指向kernel stack

#### 2.5

sret 命令

会修改sstatus 寄存器，进而进行状态切换

#### 2.6

sp 指向 kernel stack，ssratch 指向 user stack

#### 2.7

ecall


## 个人看法

简单说下遇到的坑
1. 将`syscall_times: [u32; MAX_SYSCALL_NUM]` 改成`syscall_times: [usize; MAX_SYSCALL_NUM]` 后遇到（LoadFault ）问题，修改回来就好了，原因暂时不明
2. 获取时间ms 的函数，不要仿造获取us 那个函数去写，而是在us 上 除以1000，否则会出现后调用的get time接口时间还会比 获取 task_info接口 的时间提前的问题，可能是因为取整的原因。







