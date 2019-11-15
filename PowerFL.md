# PowerFL 利用AFL+QEMU Fuzz VxWorks

[论文链接](https://www.petergoodman.me/docs/qpss-2019-slides.pdf?nsukey=TBovba%2BolVBQqaFuRmCzJX%2BbpKrVZbol7n2pMc5UL6CA9CdcNlXiu30ErSTAcnO9VjgU%2F0QVAL9%2FBgym%2B81hspRc5Y1TF3ZvZHsjBCgDYyhl%2BLu%2FWiKHJrUlgaQLb%2FXlnrlLdA%2Fcq9aqJkDh0xaQ3sOXkd8TGMJRAlgMBMuI7%2BNTFdQr91YjrxMSyGLTJferG6C4hCoVw%2B6G7fq0v087xA%3D%3D)

![PowerFL_Mode.jpg](http://ww1.sinaimg.cn/large/006EbuzUly1g8xj3ub6jrj30it06t40v.jpg)

1. 缺乏对Vxworks 和 PowerPC结构的认识

    先Fuzz VxWorks x86 的架构 然后移植 （我觉得有问题的点：AFL插桩代码的改变）

2. AFL 是一个用户模式的Fuzz 但VxWorks 运行在内核

    在Qemu中模拟VxWorks 运行在用户模式中
    
    AFL 作为 QEMU的单独线程运行 （虚拟机的稳定性 虚拟机开启浪费时间）

3. 程序运行前 首先会进行bootloader、kernel init 等操作

    制作快照在目标进程运行的时刻 （什么时候）

4. 要获取QEMU中虚拟机的动作 要做到任意位置进行Hook

    - 在JIT翻译阶段进行Hook
    - 或者进行pre post callback 功能在函数执行前 或执行后进行 hook
    - stripped 之后的文件如何处理

5. 通过函数名称hook任意函数 存在有函数名stripped 的情况

    通过bindiff 进行恢复

6. 程序运行需要其他硬件设备的支持 模拟其他硬件设备

    patch 有问题的函数 有些有问题的函数可以通过函数名看出  powerfl_suppress_N（patch之后 会不会影响正常的程序功能）

7. 将AFL的case 提供给目标程序

    解决文件映射的问题
    
8. 代码覆盖率的检测 中断会产生误报

    hook中断
    
9. 何时内核空间处于闲置状态

    重复执行相同的代码路径








