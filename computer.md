[TOC]

#  计算机基础知识

本节记录平时用到或者学到的计算机的基础知识。

## [操作系统](https://mp.weixin.qq.com/s/oz1c3dhFiR9HpIgG5AEsQw)

### 冯诺伊曼体系

#### 冯诺伊曼体系简介

现代计算机之父`冯诺伊曼`最先提出程序存储的思想，并成功将其运用在计算机的设计之中，该思想约定了用二进制进行计算和存储，还定义计算机基本结构为 5 个部分，分别是**中央处理器（CPU）、内存、输入设备、输出设备、总线**。

![图片](https://mmbiz.qpic.cn/mmbiz_png/wJvXicD0z2dWtyQ6fLJR8SwofF59o2Iq2eIYXKsrAhOQfL6icNs8uUj0uGHNTVF9ZaMhSxWiaYAqrrhib56cwvC7dw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

1. **存储器**：代码跟数据在RAM跟ROM中是线性存储， 数据存储的单位是一个二进制位。最小的存储单位是字节。
2. **总线**：总线是用于 CPU 和内存以及其他设备之间的通信，总线主要有三种：

> 1. `地址总线`：用于指定 CPU 将要操作的内存地址。
> 2. `数据总线`：用于读写内存的数据。
> 3. `控制总线`：用于发送和接收信号，比如中断、设备复位等信号，CPU 收到信号后响应，这时也需要控制总线。

1. **输入/输出设备**：输入设备向计算机输入数据，计算机经过计算后，把数据输出给输出设备。比如键盘按键时需要和 CPU 进行交互，这时就需要用到控制总线。
2. **CPU**：中央处理器，类比人脑，作为计算机系统的运算和控制核心，是信息处理、程序运行的最终执行单元。CPU用寄存器存储计算时所需数据，寄存器一般有三种：

> 1. `通用寄存器`：用来存放需要进行运算的数据，比如需进行加法运算的两个数据。
> 2. `程序计数器`：用来存储 CPU 要执行下一条指令所在的内存地址。
> 3. `指令寄存器`：用来存放程序计数器指向的指令本身。

**在冯诺伊曼体系下电脑指令执行的过程**：

1. CPU读取程序计数器获得指令内存地址，CPU控制单元操作地址总线从内存地址拿到数据，数据通过数据总线到达CPU被存入指令寄存器。
2. CPU分析指令寄存器中的指令，如果是计算类型的指令交给逻辑运算单元，如果是存储类型的指令交给控制单元执行。
3. CPU 执行完指令后程序计数器的值通过自增指向下个指令，比如32位CPU会自增4。
4. 自增后开始顺序执行下一条指令，不断循环执行直到程序结束。

**CPU位宽**：32位CPU一次可操作计算4个字节，64位CPU一次可操作计算8个字节，这个是硬件级别的。平常我们说的32位或64位操作系统指的是软件级别的，指的是程序中指令多少位。
**线路位宽**：CPU操作指令数据通过高低电压变化进行数据传输，传输时候可以串行传输，也可以并行传输，多少个并行等于多少个位宽。

####  CPU 简介

`Central Processing Unit 中央处理器，作为计算机系统的运算和控制核心，是信息处理、程序运行的最终执行单元`。

![图片](https://mmbiz.qpic.cn/mmbiz_png/wJvXicD0z2dWtyQ6fLJR8SwofF59o2Iq2Bv5K6BQqo8OkqVYIVibyDUM5rnPicCibkqhWl2OVqY9kWmwvicHpZyKd5Q/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)CPU

1. `CPU核心`：一般一个CPU会有多个CPU核心，平常说的多核是指在一枚处理器中集成两个或多个完整的计算引擎。核跟CPU的关系是：核属于CPU的一部分。

2. `寄存器`：最靠近CPU对存储单元，32位CPU寄存器可存储4字节，64位寄存器可存储8字节。寄存器访问速度一般是半个CPU时钟周期，属于纳秒级别，

3. `L1缓存`：每个CPU核心都有，用来缓存数据跟指令，访问空间大小一般在32～256KB，访问速度一般是2～4个CPU时钟周期。

   ```
   cat /sys/devices/system/cpu/cpu0/cache/index0/size # L1 数据缓存
   cat /sys/devices/system/cpu/cpu0/cache/index1/size # L1 指令缓存
   ```

4. `L2缓存`：每个CPU核心都有，访问空间大小在128KB～2MB，访问速度一般是10～20个CPU时钟周期。

   ```
   cat /sys/devices/system/cpu/cpu0/cache/index2/size # L2 缓存容量大小
   ```

5. `L3缓存`：多个CPU核心共用，访问空间大小在2MB～64MB，访问速度一般是20～60个CPU时钟周期。

   ```
   cat /sys/devices/system/cpu/cpu0/cache/index3/size # L3 缓存容量大小
   ```

6. `内存`：多个CPU共用，现在一般是4G～512G，访问速度一般是200～300个CPU时钟周期。

7. `固体硬盘SSD`：现在台式机主流都会配备，上述的寄存器、缓存、内存都是断电数据立马丢失的，而SSD里不会丢失，大小一般是128G～1T，比内存慢10～1000倍。

8. `机械盘HDD`：很早以前流行的硬盘了，容量可在512G～8T不等，访问速度比内存慢10W倍不等。

9. `访问数据顺序`：CPU在拿数据处理的时候几乎也是按照上面说得流程来操纵的，只有上面一层找不到才会找下一层。

10. `Cache Line` :  CPU读取数据时会按照 Cache Line 方式把数据加载到缓存中，每个Cacheline = 64KB，因为L1、L2是每个核独有到可能会触发**伪共享**，就是 所以可能会将数据划分到不同到CacheLine中来避免伪共享，比如在JDK8 新增加的 LongAdder 就涉及到此知识点。

> 1. **伪共享**：缓存系统中是以缓存行（cache line）为单位存储的，当多线程修改互相独立的变量时，如果这些变量共享同一个缓存行，就会无意中影响彼此的性能，这就是伪共享。

1. `JMM`: 数据经过种种分层会导致访问速度在不断提升，同时也带来了各种问题，多个CPU同时操作相同数据可能会造成各种BU个，需要加锁，这里在[JUC](http://mp.weixin.qq.com/s?__biz=MzI4NjI1OTI4Nw==&mid=2247489439&idx=1&sn=df404e70a8e55b4019317ef2036fbe7d&chksm=ebdef6a7dca97fb1e1a0dfd2eab194fa87f4971cd6b88645db072bcc9c98614b0ad30dc43399&scene=21#wechat_redirect)并发已详细探讨过。

#### CPU 访问方式

![图片](https://mmbiz.qpic.cn/mmbiz_png/wJvXicD0z2dWtyQ6fLJR8SwofF59o2Iq2B39VcZznickRyQS9PibbDFZMJ98RL6wc1zTlAPlkch1GUyOhCApcbtVQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)CPU访问方式


内存数据映射到CPU Cache 时通过公式`Block N % CacheLineMax`决定内存Block数据放到那个CPU Cache Line 里。CPU Cache 主要有4部分组成。



1. **Cache Line Index** ：CPU缓存读取数据时不是按照字节来读取的，而是按照CacheLine方式存储跟读取数据的。
2. **Valid Bit** : 有效位标志符，值为0时表示无论 CPU Line 中是否有数据，CPU 都会直接访问内存，重新加载数据。
3. **Tag**：组标记，用来标记内存中不同BLock映射到相同CacheLine，用Tag来区分不同的内存Block。
4. **Data**：真实到内存数据信息。

CPU真实访问内存数据时只需要指定三个部分即可。

1. **Cache Line Index** ：要访问到Cache Line 位置。
2. **Tag**：表示用那个数据块。
3. **Offset**：CPU从CPU Cache 读取数据时不是直接读取Cache Line整个数据块，而是读取CPU所需的数据片段，称为Word。如何找到Word就需要个偏移量Offset。

#### CPU 访问速度

![图片](https://mmbiz.qpic.cn/mmbiz_png/wJvXicD0z2dWtyQ6fLJR8SwofF59o2Iq2CYlQNYQ0AdLGbicYzpEGUcjk4lWvzEtibM0NCNgjzCSib56RG9zkadHuw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)访问耗时对比


如上图所示，CPU访问速度是逐步变慢，所以CPU访问数据时需尽量在距离CPU近的高速缓存区访问，根据摩尔定律CPU访问速度每18个月就会翻倍，而内存的访问每18个月也就增长10% 左右，导致的结果就是CPU跟内存访问性能差距逐步变大，那如何尽可能提高CPU缓存命中率呢？



1. `数据缓存`：遍历数据时候按照内存布局顺序访问，因为CPU Cache是根据Cache Line批量操作数据的，所以你顺序读取数据会提速，道理跟磁盘顺序写一样。

2. `指令缓存`：尽可能的提供有规律的条件分支语句，让 CPU 的分支预测器发挥作用，进一步提高执行的效率，因为CPU是自带`分支预测器`，自动提前将可能需要的指令放到指令缓存区。

3. `线程绑定到CPU`：一个任务A在前一个时间片用CPU核心1 运行，后一个时间片用CPU核心2 运行，这样缓存L1、L2就浪费了。因此操作系统提供了将进程或者线程绑定到某一颗 CPU 上运行的能力。如 Linux 上提供了 sched_setaffinity 方法实现这一功能，其他操作系统也有类似功能的 API 可用。当多线程同时执行密集计算，且 CPU 缓存命中率很高时，如果将每个线程分别绑定在不同的 CPU 核心上，性能便会获得非常可观的提升。

####  操作系统

![图片](https://mmbiz.qpic.cn/mmbiz_png/wJvXicD0z2dWtyQ6fLJR8SwofF59o2Iq25DAXc3iczEmfxxnoZVfFq233VmCxgH0l23V06jjKg1VASckPOoKb06w/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)计算机结构


有了冯诺伊曼计算机体系后，电脑想要为用户提供便捷的服务还需要安装个操作系统`Operation System`，操作系统是覆盖在硬件上的一层特殊软件，它管理计算机的硬件和软件资源，为其他应用程序提供大量服务。可以理解为操作系统是日常应用程序跟硬件之间的接口。日常你经常在用Windows/Linux 系统，操作系统给我们提供了超级大的便利，但是你了解操作系统么？操作系统是如何进行**内存管理**、**进程管理**、**文件管理**、**输入输出管理**的呢？





###  内存管理

你的电脑是32位操作系统，那可支持的最大内存就是4G，你有没有好奇为什么可以同时运行2个以上的2G内存的程序。应用程序不是直接使用的物理地址，操作系统为每个运行的进程分配了一套`虚拟地址`，每个进程都有自己的`虚拟内存地址`，进程是无法直接进行`物理内存地址`的访问的。至于虚拟地址跟物理地址的映射，进程是感知不到的！`操作系统自身会提供一套机制将不同进程的虚拟地址和不同内存的物理地址进行映射`。

![图片](https://mmbiz.qpic.cn/mmbiz_png/wJvXicD0z2dWtyQ6fLJR8SwofF59o2Iq2I68npzHnicKpJo6S1hQa6Ejn4NKBySKNLkXhUwMnGRUvAtlAnfvqnFA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)虚拟内存

####  MMU

`Memory Management Unit 内存管理单元`是一种负责处理CPU内存访问请求的计算机硬件。它的功能包括**虚拟地址到物理地址的转换、内存保护、中央处理器高速缓存的控制**。现代 CPU 基本上都选择了使用 MMU。

当进程持有虚拟内存地址的时候，CPU执行该进程时会操作虚拟内存，而MMU会自动的将虚拟内存的操作映射到物理内存上。

![图片](https://mmbiz.qpic.cn/mmbiz_png/wJvXicD0z2dWtyQ6fLJR8SwofF59o2Iq2icGIg94QWaC6ibQAXqIrMYpvwHVstgGvqO9OUXqkV8cB5VPvvuxzGiceA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)


这里提一下，Java操作的时候你看到的地址是`JVM地址`，不是真正的物理地址。

#### 内存管理方式

操作系统主要采用`内存分段`和`内存分页`来管理虚拟地址与物理地址之间的关系，其中分段是很早前的方法了，现在大部分用的是分页，不过分页也不是完全的分页，是在分段的基础上再分页。

##### 内存分段

![JVM内存模型](https://mmbiz.qpic.cn/mmbiz_png/wJvXicD0z2dWtyQ6fLJR8SwofF59o2Iq2mDiacAKAzUEHd8z2ojKUXFoeLjOYnicBCxvzw7vak0E3FDD8fneuqB1g/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

我们以上图的[JVM](https://mp.weixin.qq.com/s?__biz=MzI4NjI1OTI4Nw==&mid=2247489183&idx=1&sn=02ab3551c473bd2c8429862e3689a94b&scene=21#wechat_redirect)内存模型举例，程序员会认为我们的代码是由代码段、数据段、栈段、堆段组成。不同的段是有不同的属性的，用户并不关心这些元素所在内存的位置，而分段就是支持这种用户视图的内存管理方案。逻辑地址空间是由一组段构成。每个段都有名称和长度。地址指定了段名称和段内偏移。因此用户`段编号`和`段偏移`来指定不同属性的地址。而虚拟内存地址跟物理内存地址中间是通过段表进行映射的，口说无凭，看图吧。

![内存分段管理](https://mmbiz.qpic.cn/mmbiz_png/wJvXicD0z2dWtyQ6fLJR8SwofF59o2Iq2AqGiaD0VZdayNgYFpjzBB4PpTBribunovZnuS1U3S803fcBF2UjA12pg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

如上虚拟地址有 5 个段，各段按如图所示来存储。每个段都在段表中有一个条目，它包括段在物理内存内的开始的基地址和该段的界限长度。例如段 2 为 400 字节长，开始于位置 4300。因此对段 2 字节 53 的引用映射成位置 4300 + 53 = 4353。对段 3 字节 852 的引用映射成位置 3200 + 852 = 4052。

分段映射很简单，但是会导致`内存碎片`跟`内存交互效率低`。这里先普及下在内存管理中主要有`内部内存碎片`跟`外部内存碎片`。

1. **内部碎片**：已经被分配出去的的内存空间不经常使用，并且分配出去的内存空间大于请求所需的内存空间。
2. **外部碎片**：指可用空间还没有分配出去，但是可用空间由于大小太小而无法分配给申请空间的新进程的内存空间空闲块。

以上图为例，现在系统空闲是1400 +  800 + 600 = 2800。那如果有个程序想要连续的使用2000，内存分段模式下提供不了啊！上述三个是`外部内存碎片`。当然可以使用系统的`Swap`空间，先把段0写入到磁盘，然后再重新给段0分配空间。这样可以实现最终可用，可是但凡涉及到磁盘读写就会导致`内存交互效率低`。

![swap空间利用](https://mmbiz.qpic.cn/mmbiz_png/wJvXicD0z2dWtyQ6fLJR8SwofF59o2Iq2uOHajAXwmDWUzEx2x7Ys54Iibr2TCDq9jGQg8YzEbDSmib4B511nZ0jw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

##### 内存分页

内存分页，`整个虚拟内存和物理内存切成一段段固定尺寸的大小`。每个固定大小的尺寸称之为`页Page`，在 Linux 系统中Page = 4KB。然后虚拟内存跟物理内存之间通过`页表`来实现映射。

采用**内存分页**时内存的释放跟使用都是以页为单位的，也就不会产生内存碎片了。当空间还不够时根据操作系统调度算法，可能将最少用的内存页面 swap-out换出到磁盘，用时候再swap-in换入，尽可能的减少磁盘刷写量，提高内存交互效率。

分页模式下虚拟地址主要有`页号`跟`页内偏移量`两部分组成。通过页号查询页表找到物理内存地址，然后再配合页内偏移量就找到了真正的物理内存地址。

![图片](https://mmbiz.qpic.cn/mmbiz_png/wJvXicD0z2dWtyQ6fLJR8SwofF59o2Iq29qvsyrPuLA1WwFibiaA0QDazkIsjFJEyNXpibUB0tyW3ASoZuhQ7r9DMA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)分页内存寻址

32位操作系统环境下进程可操作的虚拟地址是4GB，假设一个虚拟页大小为4KB，那需要4GB/4KB = `2^20` 个页信息。一行页表记录为4字节，`2^20`等价于4MB页表存储信息。这只是一个进程需要的，如果10个、100个、1000个呢？仅仅是页表存储都占据超大内存了。

为了解决这个问题就需要用到 `多级页表`，核心思想就是**局部性**分配。在32位的操作系统中将将4G空间分为 1024 行页目录项目(4KB)，每个页目录项又对应1024行页表项。如下图所示：

![图片](https://mmbiz.qpic.cn/mmbiz_png/wJvXicD0z2dWtyQ6fLJR8SwofF59o2Iq2eNQ86SBSq45RDDfwmM3wRerUFdyGV5icgzgStY6GxeHWCDctRqhA24w/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)32位系统二级分页


控制寄存器cr3中存放了页目录的物理地址，通过cr3寄存器可以找到页目录，而32位线性地址中的Directory部分决定页目录中的目录项，而页目录项中存放了要找的页表的物理基地址，再结合线性地址中的中间10位页表项，就可以找到页框的页表项。线性地址中的Offset部分占12位，因此页框的物理地址 + 线性地址Offset部分 = 页框中的任何一个字节。

分页后一级页就等价于4G虚拟地址空间，并且如果一级页表中那些地址没有就不需要再创建二级页表了！核心思想就是按需创建，当系统给每个进程分配4G空间，进程不可能占据全部内存的，如果一级目录页只有10%用到了，此时页表空间 = 一级页表4KB + 0.1 * 4MB  。这比单独的每个进程占据4M好用多了！

多层分页的弊端就是访问时间的**增加**。

1. 使用页表时读取内存中一页内容需要2次访问内存，访问页表项 + 并读取的一页数据。
2. 使用二级页表的话需要三次访问，访问页目录项 + 访问页表项 + 访问并读取的一页数据。访存次数的增加也就意味着访问数据所花费的总时间增加。

而对于64位系统，二级分页就无法满足了，Linux 从2.6.11开始采用四级分页模型。

1. Page Global Directory 全局页目录项
2. Page Upper Directory 上层页目录项
3. Page Middle Directory 中间页目录项
4. Page Table Entry 页表项
5. Offset 偏移量。

![图片](https://mmbiz.qpic.cn/mmbiz_png/wJvXicD0z2dWtyQ6fLJR8SwofF59o2Iq22eGic2tkyBTL5ppBybXiboDojAE5qv4NmGQE9fRk9SIJVEGQmr1A2D4Q/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)64位寻址

##### TLB

Translation Lookaside Buffer 可翻译为`地址转换后援缓冲器`，简称为`快表`，属于CPU内部的一个模块，TLB是MMU的一部分，实质是cache，它所缓存的是最近使用的数据的页表项（虚拟地址到物理地址的映射）。他的出现是为了加快访问数据（内存）的速度，减少重复的页表查找。当然它不是必须要有的，但有它，速度就更快。

![TLB](https://mmbiz.qpic.cn/mmbiz_png/wJvXicD0z2dWtyQ6fLJR8SwofF59o2Iq2ATOjEdH3ZGJlNy88S2Osib4Wql40hZzarJGx3u7ibwu1oQkOhSqSRyBA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)


TLB很小，因此缓存的东西也不多。主要缓存最近使用的数据的数据映射。TLB结构如下图：

![TLB查询](https://mmbiz.qpic.cn/mmbiz_png/wJvXicD0z2dWtyQ6fLJR8SwofF59o2Iq2vy6gVXKibMflaNyvkwPVfuG4s5nGibVy0fHXDn8YdMQGfjlE7JuFc8hw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)


如果一个需要访问内存中的一个数据，给定这个数据的虚拟地址，查询TLB，发现有hit，直接得到物理地址，在内存根据物理地址取数据。如果TLB没有这个虚拟地址miss，那么只能费力的通过页表来查找了。日常CPU读取一个数据的流程如下：

![CPU读取数据流程图](https://mmbiz.qpic.cn/mmbiz_png/wJvXicD0z2dWtyQ6fLJR8SwofF59o2Iq2PiaCdlFOtF0znDK1mA6soTr58eC4Tib2GssBDtATFD4WicGbTZyjTVsXA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)


当进程地址空间进行了`上下文切换`时，比如现在是进程1运行，TLB中放的是进程1的相关数据的地址，突然切换到进程2，TLB中原有的数据不是进程2相关的，此时TLB刷新数据有两种办法。



1. **全部刷新**：很简单，但花销大，很多不必刷新的数据也进行刷新，增加了无畏的花销。
2. **部分刷新**：根据标志位，刷新需要刷新的数据，保留不需要刷新的数据。

#####   段页式管理

`内存分段`跟`内存分页`不是对立的，这俩可以组合起来在同一个系统中使用的，那么组合起来后通常称为`段页式内存管理`。段页式内存管理实现的方式：

1. 先对数据不同划分出不同的段，也就是前面说的分段机制。
2. 然后再把每一个段进行分页操作，也就是前面说的分页机制。
3. 此时 地址结构 = 段号 + 段内页号 + 页内位移。

每一个进程有一张段表，每个段又建立一张页表，段表中的地址是页表的起始地址，而页表中的地址则为某页的物理页号。

![图片](https://mmbiz.qpic.cn/mmbiz_png/wJvXicD0z2dWtyQ6fLJR8SwofF59o2Iq209HlCEIaRZ7icDWv4b3rWhPv8x4VJfLLwibsRRkc3XgYpiaAdpkYkN7fQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)段页式管理


同时我们经常看到两个专业词`逻辑地址`跟`线性地址`。



1. `逻辑地址`：指的是没被段式内存管理映射的地址。
2. `线性地址`：通过段式内存管理映射且页式内存管理转换前的地址，俗称虚拟地址。

目前 Intel X86 CPU 采用的是内存分段 +  内存分页的管理方式，其中分页的意思是在由段式内存管理所映射而成的的地址上再加上一层地址映射。

![图片](https://mmbiz.qpic.cn/mmbiz_png/wJvXicD0z2dWtyQ6fLJR8SwofF59o2Iq2iaYkCrgL9iaKdQlyG8iaS2AP1ErZicNxKqL7WbsVXdgv0miaTVP9YTMvgZw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)X86内存管理方式

#####   Linux 内存管理

先说结论：Linux系统基于X86 CPU 而做的操作系统，所以也是用的段页式内存管理方式。

![图片](https://mmbiz.qpic.cn/mmbiz_png/wJvXicD0z2dWtyQ6fLJR8SwofF59o2Iq2Dr2eAEUzH4S5OMzT6XOmBRyzhCcibcjpmxy0jQvSGhn6XMY2EDbgVxA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)


我们知道32位的操作系统可寻址范围是4G，操作系统会将4G的可访问内存空间分为**用户空间**跟**内核空间**。



1. `内核空间`：操作系统内核访问的区域，独立于普通的应用程序，是受保护的内存空间。内核态下CPU可执行任何指令，可自由访问任何有效地址。
2. `用户空间`：普通应用程序可访问的内存区域。被执行代码会受到CPU众多限制，进程只能访问映射其地址空间的页表项中规定的在用户态下可访问页面的虚拟地址。

那为啥要搞俩空间呢?现在嵌入式环境跟以前的WIN98系统是没有区分俩空间的，须知俩空间是CPU分的，而操作系统是在上面运行的，单一用户、单一任务服务的操作系统，是没有分所谓用户态和内核态的必要。用户态和内核态是因为有多用户，多任务的需求，然后在CPU硬件厂商配合之后，产生的一个操作系统解决多用户多任务需求的方案。方案就是**限制**，通过硬件手段（也只能硬件手段才能做到），限制某些代码，使其无法控制整个物理硬件，进而使各个不同用户，不同任务的代码，无权修改整个物理硬件，再进而保护操作系统的核心底层代码和其他用户的数据不被无意或者有意地破坏和盗取。

![图片](https://mmbiz.qpic.cn/mmbiz_png/wJvXicD0z2dWtyQ6fLJR8SwofF59o2Iq2BlpRLP9bgLy0uQKgLVBQm6O79WHwenmBo2vJSAPrb5Jue2YymnyAuQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)


后来研究者根据**CPU**的运行级别，分成了Ring0~Ring3四个级别。Ring0是最高级别，Ring1次之，Rng2更次之，拿Linux+x86来说，  操作系统内核的代码运行在最高运行级别Ring0上，可以使用特权指令，控制中断、修改页表、访问设备等。 应用程序的代码运行在最低运行级别上Ring3上，不能做受控操作，只能访问用户被分配的空间。如果要做访问磁盘跟写文件等操作，那就要通过执行系统调用函数，执行系统调用的时候，CPU的运行级别会发生从Ring3到Ring0的切换，并跳转到系统调用对应的内核代码位置执行，这样内核就为你完成了设备访问，完成之后再从Ring0返回Ring3。这个过程也称作**用户态和内核态的切换**。

用户态想要使用计算机设备或IO需通过系统调用完成sys call，系统调用就是让内核来做这些操作。而系统调用是影响整个当前进程上下文的，ＣＰＵ提供了个软中断来是实现保护线程，获取系统调用号跟参数，交给内核对应系统调用函数执行。

![图片](https://mmbiz.qpic.cn/mmbiz_png/wJvXicD0z2dWtyQ6fLJR8SwofF59o2Iq22LZoTkuvQuswwic1YAicfia6umNN7MQ2t3ORT030GbwbGS2AIkichbw84w/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)Linux系统结构


可以看到每个应用程序都各自有独立的虚拟内存地址，但每个虚拟内存中对应的内核地址其实是相同的一大块，这样当进程切换到内核态后可以很方便地访问内核空间内存。比如Java代码创建线程`new Thread`调用`start`方法后跟踪[`JVM`](http://mp.weixin.qq.com/s?__biz=MzI4NjI1OTI4Nw==&mid=2247489183&idx=1&sn=02ab3551c473bd2c8429862e3689a94b&chksm=ebdef7a7dca97eb17194c3d935c86ade240d3d96bbeaf036233a712832fb94af07adeafa098b&scene=21#wechat_redirect)源码你会发现是调用`pthread_create`来创建线程的，这就涉及到了用户态到内核态的切换。



### 进程管理

####  进程基础知识

进程是程序的一次执行，是一个程序及其数据在机器上顺序执行时所发生的活动，是具有独立功能的程序在一个数据集合上的一次运行过程，是系统进行资源分配和调度的一个基本单位。进程的[调度状态](http://mp.weixin.qq.com/s?__biz=MzI4NjI1OTI4Nw==&mid=2247489439&idx=1&sn=df404e70a8e55b4019317ef2036fbe7d&chksm=ebdef6a7dca97fb1e1a0dfd2eab194fa87f4971cd6b88645db072bcc9c98614b0ad30dc43399&scene=21#wechat_redirect)如下：

![图片](https://mmbiz.qpic.cn/mmbiz_png/wJvXicD0z2dWtyQ6fLJR8SwofF59o2Iq2KZtVY1lO1N7jJVNJ0ywSVTtcOKeJr3lyBR67ibR5jGGyIDVu2gf8mQg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)状态变化图


重点说下`挂起`跟`阻塞`：



1. 阻塞一般是当系统执行IO操作时，此时进程进入阻塞状态，等待某个事件的返回。
2. 挂起是指进程没有占有物理内存，被写到磁盘上了。这时进程状态是挂起状态。

> 1. `阻塞挂起`：进程被写入硬盘并等待某个事件的出现。
> 2. `就绪挂起`：进程被写入硬盘，进入内存可直接进入就绪状态。

####  PCB

为了描述跟控制进程的运行，系统为每个进程定义了一个数据结构——`进程控制块 Process Control Block`，它是进程实体的一部分，是操作系统中最重要的记录型数据结构。

PCB 的作用是**使一个在多道程序环境下不能独立运行的程序，成为一个能独立运行的基本单位，一个能与其它进程并发执行的进程** :

1. 作为独立运行基本单位的标志
2. 实现间断性的运行方式
3. 提供进程管理所需要的信息
4. 提供进程调度所需要的信息
5. 实现与其他进程的同步与通信

##### PCB 信息

PCB为实现上述功能，内部包含众多信息：

1. **进程标识符**：用于唯一地标识一个进程，一个进程通常有两种标识符：

> 1. `内部进程标识符`：标识各个进程，每个进程都有一个并且唯一的标识符，设置内部标识符主要是为了方便系统使用。
> 2. `外部进程标识符`：它由创建者提供，可设置用户标识，以指示拥有该进程的用户。往往是由用户进程在访问该进程时使用。一般为了描述进程的家族关系，还应设置父进程标识及子进程标识。

1. **处理机状态**：由各种寄存器组成。包含许多信息都放在寄存器中，方便程序restart。

> 1. 通用寄存器、指令计数器、程序状态字PSW、用户栈指针等信息。

1. **进程调度信息**

> 1. 进程状态：指明进程的当前状态，作为进程调度和对换时的依据。
> 2. 进程优先级：用于描述进程使用处理机的优先级别的一个整数，优先级高的进程应优先获得处理机
> 3. 进程调度所需的其它信息：与所采用的进程调度算法有关，如进程已等待CPU的时间总和、进程已执行的时间总和等。
> 4. 事件：指进程由执行状态转变为阻塞状态所等待发生的事件，即阻塞原因。

1. **资源清单**

> 有关内存地址空间或虚拟地址空间的信息，所打开文件的列表和所使用的 I/O 设备信息。

##### PCB 组织方式

操作系统中有太多 PCB，如何管理是个问题，一般有如下方式。

![图片](https://mmbiz.qpic.cn/mmbiz_png/wJvXicD0z2dWtyQ6fLJR8SwofF59o2Iq2UDmiblzcvIsZQ0Xib2OOtmfO6LehRAta09ibs4RXiazicOfolgugAuxES0g/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)线下数组

1. **线性方式**：

> 1. 将系统所有PCB都组织在一张线性表中，将该表首地址存在内存的一个专用区域
> 2. 实现简单，开销小，但是每次都需要扫描整张表，适合进程数目不多的系统

![图片](https://mmbiz.qpic.cn/mmbiz_png/wJvXicD0z2dWtyQ6fLJR8SwofF59o2Iq2n5vJ6pUI1KMSwTYyicA3f5FXYvniaZGljgHBmjuMe6bJ89libYTicRQicxg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)索引方式

1. **索引方式**：

> 1. 将同一状态的进程组织在一个索引表中，索引表项指向相应的 PCB，不同状态对应不同的索引表。

![图片](https://mmbiz.qpic.cn/mmbiz_png/wJvXicD0z2dWtyQ6fLJR8SwofF59o2Iq22QzHGn49zCjI03Assib9rNREGRX91CZuuGk3anYSCodq5mvbbrIar3w/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)链表方式

1. **链接方式**：

> 1. 把同一状态的PCB链接成一个队列，形成就绪队列、阻塞队列、空白队列等。对其中的就绪队列常按进程优先级的高低排列，优先级高排在队前。
> 2. 因为进程创建、销毁、调度频繁，所以**一般采用此模式**。

#### 进程控制

进程控制是进程管理最基本的功能，主要包括`创建新进程`，`终止已完成的进程`，`将发生异常的进程置于阻塞状态`，`将进程唤醒`等。

#####  进程创建

父进程可创建子进程，父进程终止后子进程也会被终止。子进程可继承父进程所有资源，子进程终止需将自己所继承的资源归还父进程。接下来看下创建的大致流程。

1. 为新进程分配唯一进件标识号，然后创建一个空白PCB，需注意PCB数量是有限的，所以可能会创建失败。
2. 尝试为新进程分配所需资源，如果资源不足进程会进入等待状态。
3. 初始化PCB，有如下几个操作。

> 1. 标识信息：将系统分配的标识符和父进程标识符填入新PCB
> 2. 处理机状态信息：使程序计数器指向程序入口地址，使栈指针指向栈顶
> 3. 处理机控制信息：将进程设为就绪/静止状态，通常设为最低优先级

1. 如果进程调度队列能接纳新进程，就将进程插入到就绪队列，等待被调度运行。

#####  进程终止

进程终止情况一般分为正常结束、异常结束、外界干预三种。

1. 正常结束
2. 异常结束

> 1. 越界错：访问的存储区越出该进程的区域
> 2. 保护错：试图访问不允许访问的资源，或以不适当的方式访问（写只读）
> 3. 非法指令：试图执行不存在的指令（可能是程序错误地转移到数据区，数据当成了指令）
> 4. 特权指令出错：用户进程试图执行一条只允许OS执行的指令
> 5. 运行超时：执行时间超过指定的最大值
> 6. 等待超时：进程等待某件事超过指定的最大值
> 7. 算数运算错：试图执行被禁止的运算（被0除）
> 8. I/O故障

1. 外界干预

> 1. 操作员或OS干预（死锁）
> 2. 父进程请求，子进程完成父进程指定的任务时
> 3. 父进程终止，所有子进程都应该结束

**终止过程**：

1. 根据被终止进程的标识符，从PCB集合中检索出该PCB，读取进程状态
2. 若处于执行状态则立即终止执行，将CPU资源分配给其他进程。
3. 若进程有子孙进程则将其所有子孙进程终止。
4. 全部资源还给父进程或操作系统。
5. 该进程的PCB从所在队列/链表中移出。

#####  进程阻塞

意思是该进程执行半路被阻塞，必须由某个事件进程唤醒该进程。常见的就是IO读取操作。常见阻塞时机/事件如下：

1. 请求共享资源失败，系统无足够资源分配
2. 等待某种操作完成
3. 新数据尚未到达（相互合作的进程）
4. 等待新任务

**阻塞流程**：

1. 找到要被阻塞进程标识号对应的 PCB。
2. 将该进程由运行状态转换为阻塞状态。
3. 将该 进程PCB 插入的阻塞队列中去。

#####   进程唤醒

唤醒 原语 wake up，一般和阻塞成对使用。唤醒过程如下：

1. 从阻塞队列找到所需PCB。
2. PCB从阻塞队列溢出，然后变为就绪状态。
3. 从阻塞队列溢出该PCB然后插入到就绪状态队列等待被分配CPU资源。

####   进程调度

进程数一般会大于CPU个数，进程状态切换主要由调度程序进行调度。一般情况下CPU调度时主要分为`抢占式调度`跟`非抢占式调度`。

1. `非抢占式`：让进程运行直到结束或阻塞的调度方式， 容易实现，适合专用系统。
2. `抢占式`：每个进程获得时间片才可以被CPU调度运行， 可防止单一进程长时间独占CPU 系统开销大。

#####   进程调度原则

1. **CPU 利用率**

> 1. CPU利用率 = 忙碌时间 / 总时间。
> 2. 调度程序应该尽量让 CPU 始终处于忙碌的状态，这可提高 CPU 的利用率。比如当发生IO读取时候，不要傻傻等待，去执行下别的进程。

1. **系统吞吐量**

> 1. 系统吞吐量 = 总共完成多少个作业 / 总共花费时间。
> 2. 长作业的进程会占用较长的 CPU 资源导致降低吞吐量，相反短作业的进程会提升系统吞吐量。

1. **周转时间**

> 1. 周转时间 = 作业完成时间 - 作业提交时间。
> 2. 平均周转时间 = 各作业周转时间和 / 作业数
> 3. 带权周转时间 = 作业周转时间 / 作业实际运行时间
> 4. 平均带权周转时间 = 各作业带权周转时间之和 / 作业数
> 5. 尽可能使周转时间降低。

1. **等待时间**

> 1. 指的是进程在等待队列中等待的时间，一般也需要尽可能短。
> 2. **响应时间**
>    响应时间 = 系统第一次响应时间 - 用户提交时间，在交互式系统中响应时间是衡量调度算法好坏的主要标准。

#####  调度算法

**FCFS 算法**

1. First Come First Severd 先来先服务算法，遵循先来后端原则，每次从就绪队列拿等待时间最久的，运行完毕后再拿下一个。
2. 该模式对长作业有利，适用 CPU 繁忙型作业的系统，不适用 I/O 型作业，因为会导致进程CPU利用率很低。

**SJF 算法**

1. Shortest Job First 最短作业优先算法，该算法会优先选择运行所需时间最短的进程执行，可提高吞吐量。
2. 跟FCFS正好相反，对长作业很不利。

**SRTN 算法**

1. Shortest Remaining Time Next 最短剩余时间优先算法，可以认为是SJF的抢占式版本，当一个新就绪的进程比当前运行进程具有更短完成时间时，系统抢占当前进程，选择新就绪的进程执行。
2. 有最短的平均周转时间，但不公平，源源不断的短任务到来，可能使长的任务长时间得不到运行。

**HRRN 算法**

1. Highest Response Ratio Next 最高响应比优先算法，为了平衡前面俩而生，按照响应优先权从高到低依次执行。属于前面俩的折中权衡。
2. 优先权 = (等待时间 + 要求服务时间) / 要求服务时间

**RR 算法**

1. Round Robin 时间片轮转算法，操作系统设定了个时间片Quantum，时间片导致每个进程只有在该时间片内才可以运行，这种方式导致每个进程都会均匀的获得执行权。
2. 时间片一般20ms~50ms，如果太小会导致系统频繁进行上下文切换，太大又可能引起对短的交互请求的响应变差。

**HPF 算法**

1. Highest Priority First 最高优先级调度算法，从就绪队列中选择最高优先级的进程先执行。
2. 优先级的设置有初始化固定死的那种，也有在代码运转过程中根据等待时间或性能动态调整 这两种思路。
3. 缺点是可能导致低优先级的一直无法被执行。

**MFQ 算法**

1. Multilevel Feedback Queue 多级反馈队列调度算法 ，可以认为是 RR 算法 跟 HPF 算法 的综合体。

2. 系统会同时存在多个就绪队列，每个队列优先级从高到低排列，同时优先级越高获得是时间片越短。

3. 新进程会先加入到最高优先级队列，如果新进程优先级高于当前在执行的进程，会停止当前进程转而去执行新进程。新进程如果在时间片内没执行完毕需下移到次优先级队列。

   ![多级反馈队列调度算法](https://mmbiz.qpic.cn/mmbiz_png/wJvXicD0z2dWtyQ6fLJR8SwofF59o2Iq2u6O7d6j81zGa6icUa3HQ4NeJ6NbGwwiaeOQrtgA2nMe72MYtANl1K8BQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

####  线程

##### 线程定义

早期操作系统是没有线程概念的，线程是后来加进来的。为啥会有线程呢？那是因为以前在多进程阶段，经常会涉及到进程之间如何通讯，如何共享数据的问题。并且进程关联到PCB的生命周期，管理起来开销较大。为了解决这个问题引入了线程。

线程是进程当中的一个执行流程。同一个进程内的多个线程之间可以共享进程的代码段、数据段、打开的文件等资源。同时每个线程又都有一套独立的寄存器和栈来确保线程的控制流是独立的。

进程有个`PCB`来管理，同理操作系统通过 `Thread Control Block`线程控制块来实现线程的管控。

##### 线程优缺点

**优点**

1. 一个进程中可以同时存在1~N个线程，这些线程可以并发的执行。
2. 各个线程之间可以共享地址空间和文件等资源。

**缺点**

1. 当进程中的一个线程奔溃时，会导致其所属进程的所有线程奔溃。
2. 多线程编程，让人头大的东西。
3. 线程执行开销小，但不利于资源的隔离管理和保护，而进程正相反。

#####  进程跟线程关联

**进程**：

1. 是系统进行资源分配和调度的一个独立单位.
2. 是程序的一次执行，每个进程都有自己的地址空间、内存、数据栈及其他辅助记录运行轨迹的数据

**线程**：

1. 是进程的一个实体，是CPU调度和分派的基本单位,它是比进程更小的能独立运行的基本单位
2. 所有的线程运行在同一个进程中，共享相同的运行资源和环境
3. 线程一般是并发执行的，使得实现了多任务的并行和数据共享。

**进程线程区别**：

1. 一个线程只能属于一个进程，而一个进程可以有多个线程，但至少有一个线程。
2. 线程的划分尺度小于进程(资源比进程少)，使得多线程程序的并发性高。
3. 进程在执行过程中拥有独立的内存单元，而多个线程共享内存，从而极大地提高了程序的运行效率。
4. 资源分配给进程，同一进程的所有线程共享该进程的所有资源。
5. CPU分配资源给进程，但真正在CPU上运行的是线程。
6. 线程不能够独立执行，必须依存在进程中。

**线程快在哪儿**？

1. 线程创建的时有些资源不需要自己管理，直接从进程拿即可，线程管理寄存器跟栈的生命周期即可。
2. 同进程内多线程共享数据，所以进程数据传输可以用zero copy技术，不需要经过内核了。
3. 进程使用一个虚拟内存跟页表，然后多线程共用这些虚拟内存，如果同进程内两个线程进行上下文切换比进程提速很多。

#####  线程实现

在前面的内存管理中说到了内核态跟用户态。相对应的线程的创建也分为`用户态线程`跟`内核态线程`。

######  用户态线程

在用户空间实现的线程，由用户态的线程库来完成线程的管理。操作系统按进程维度进行调度，**当线程在用户态创建时应用程序在用户空间内要实现线程的创建、维护和调度。操作系统对线程的存在一无所知**！操作系统只能看到进程看不到线程。所有的线程都是在用户空间实现。在操作系统看来，每一个进程只有一个线程。

![用户态线程](https://mmbiz.qpic.cn/mmbiz_png/wJvXicD0z2dWtyQ6fLJR8SwofF59o2Iq23xQnu6hzPlFspx4lLK09r9siaviafyhvAiajLWa1nv7sIptedxwJAUtUQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

**好处**：

1. 及时操作系统不支持线程模式也可以通过用户层库函数来支持线程模式，TCB 由用户级线程库函数来维护。
2. 使用库函数模式实现线程可以避免用户态到内核态的切换。

**坏处**：

1. CPU不知道线程存在，CPU的时间片切换是以进程为维度的，某个线程因为IO等操作导致线程阻塞，操作系统会阻塞整个进程，即使这个进程中其它线程还在工作。
2. 用户态线程没法打断正在运行中的线程，除非线程主动交出CPU使用权。

######   内核态线程

在内核中实现的线程，是由内核管理的线程，线程对应的 TCB 在操作系统里，这样线程的创建、终止和管理都是由操作系统负责。内线程模式下一个用户线程对应一个内核线程。

![图片](https://mmbiz.qpic.cn/mmbiz_png/wJvXicD0z2dWtyQ6fLJR8SwofF59o2Iq2qZFwKzVsABy8MefTQEm6yaqKNiccibImL3gWtkicNg06ML7s4rFeHEAGw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)内核态线程


**注意**：Linux中的JVM从1.2版以后是基于pthread实现的，`所以现在Java中线程的本质就是操作系统中的线程`。

**优点**：

1. 一个进程中某个线程阻塞不会影响其他内核线程运行。
2. 用户态模式一个时间片分给多个线程，内核态模式直接分配给线程的时间片增加。

**缺点**：

1. 内核级线程调度开销较大。调度内核线程的代价可能和调度进程差不多昂贵，代价要比用户级线程大很多。一个线程默认栈=1M，线程多了会导致内存消耗很大。
2. 线程表是存放在操作系统固定的表格空间或者堆栈空间里，所以内核级线程的数量是有限的。

######   轻量级进程

最初的进程定义都包含程序、资源及其执行三部分，其中程序通常指代码，资源在操作系统层面上通常包括内存资源、IO资源、信号处理等部分，而程序的执行通常理解为执行上下文，包括对CPU的占用，后来发展为线程。在线程概念出现以前，为了减小进程切换的开销，操作系统设计者逐渐修正进程的概念，逐渐允许将进程所占有的资源从其主体剥离出来，允许某些进程共享一部分资源，例如文件、信号，数据内存，甚至代码，这就发展出轻量进程的概念。

Light-weight process **轻量级进程是内核支持的用户线程**，它是基于内核线程的高级抽象，系统只有先支持内核线程才能有 LWP。一个进程可有1~N个LWP，每个 LWP 是跟内核线程一对一映射的，也就是 LWP 都是由一个内核线程支持。

![图片](https://mmbiz.qpic.cn/mmbiz_png/wJvXicD0z2dWtyQ6fLJR8SwofF59o2Iq2RodUPgibhpAx0HNIfLSsmic57f2ue7fUspDKP6LDFkxUeFDSFzJSDpbA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)LWP模式

**轻量级进程本质还是进程**，只是跟普通进程相比LWP跟其他进程共享大部分逻辑地址空间跟系统资源，LWP轻量体现在它只有一个最小的执行上下文和调度程序所需的统计信息。他是进程的执行部分，只带有执行相关的信息。

**Linux特性**：

1. Linux中没有真正的线程，因为Linux并没有为线程准备特定的数据结构。在内核看来只有进程而没有线程，在调度时也是当做进程来调度。Linux所谓的线程其实是与其他进程共享资源的进程。但windows中确实有线程。
2. Linux中没有的线程，线程是由进程来模拟实现的。
3. 所以在Linux中在CPU角度看，进程被称作轻量级进程LWP。

#####  协程

######  协程定义

大多数web服务跟互联网服务本质上大部分都是 IO 密集型服务，IO 密集型服务的瓶颈不在CPU处理速度，而在于尽可能快速的完成高并发、多连接下的数据读写。以前有两种解决方案：

1. `多进程`：存在频繁调度切换问题，同时还会存在每个进程资源不共享的问题，需要额外引入进程间通信机制来解决。
2. `多线程`：高并发场景的大量 IO 等待会导致多线程被频繁挂起和切换，非常消耗系统资源，同时多线程访问共享资源存在竞争问题。

此时协程出现了，协程 Coroutines 是一种比线程更加轻量级的微线程。类比一个进程可以拥有多个线程，一个线程也可以拥有多个协程。可以简单的把协程理解成子程序调用，每个子程序都可以在一个单独的协程内执行。

![图片](https://mmbiz.qpic.cn/mmbiz_png/wJvXicD0z2dWtyQ6fLJR8SwofF59o2Iq2l9MoroCgR4lH2jz5DZ9u3f5NZHDu8ibic9iaXxMhRBrThUAqbmABNNfCg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)协程


协程运行在线程之上，当一个协程执行完成后，可以选择主动让出，让另一个协程运行在当前线程之上。**协程并没有增加线程数量，只是在线程的基础之上通过分时复用的方式运行多个协程**，而且协程的切换在用户态完成，切换的代价比线程从用户态到内核态的代价小很多，一般在Python、Go中会涉及到协程的知识，尤其是现在高性能的脚本Go。

######  协程注意事项

协程运行在线程之上，并且协程调用了一个阻塞IO操作，此时操作系统并不知道协程的存在，它只知道线程，因此在协程调用阻塞IO操作时，操作系统会让线程进入阻塞状态，当前的协程和其它绑定在该线程之上的协程都会陷入阻塞而得不到调度。

因此在协程中不能调用导致线程阻塞的操作，比如打印、读取文件、Socket接口等。`协程只有和异步IO结合`起来才能发挥最大的威力。并且**协程只有在IO密集型的任务中才会发挥作用**。

#### 进程通信

进程的用户地址空间是相互独立的，不可以互相访问，但内核空间是进程都共享的，所以进程之间要通信必须通过内核。**进程间通信主要通过管道、消息队列、共享内存、信号量、信号、Socket编程**。

#####  管道

管道主要分为匿名管道跟命名管道两种，可以实现数据的单向流动性。**使用起来很简单，但是管道这种通信方式效率低，不适合进程间频繁地交换数据**。

**匿名管道**：

1. 日常Linux系统中的`|`就是匿名管道。指令的前一个输入是后一个指令的输出。

**命名管道**：

1. 一般通过`mkfifo SoWhatPipe`创建管道。通过`echo "sw" > SoWhatPipe`跟`cat < SoWhatPipe` 实现输入跟输出。

匿名管道的实现依赖`int pipe(int fd[2])`函数，其中`fd[0]`是读取断描述符，`fd[1]`是管道写入端描述符。它的本质就是在内核中创建个属于内存的缓存，从一端输入无格式数据一端输出无格式数据，需注意管道传输大小是有限的。

![管道通信底层](https://mmbiz.qpic.cn/mmbiz_png/wJvXicD0z2dWtyQ6fLJR8SwofF59o2Iq2T9SSHwtlBxlGqklTk0oDHQXwOiaNiad1L6ia2M4icvTd183A4HN8ZcPTxQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)


匿名管道的通信范围是存在父子关系的进程。由于管道没有实体，也就是没有管道文件，不会涉及到文件系统。只能通过`fork`子进程来复制父进程 fd 文件描述符，父子进程通过共用特殊的管道文件实现跨进程通信，并且因为管道只能一端写入，另一端读出，所以通常父子进程遵从如下要求：



1. 父进程关闭读取的 fd[0]，只保留写入的 fd[1]。
2. 子进程关闭写入的 fd[1]，只保留读取的 fd[0]。

![shell管道通信](https://mmbiz.qpic.cn/mmbiz_png/wJvXicD0z2dWtyQ6fLJR8SwofF59o2Iq25yWanEPCZbnr5CN6qgufBug19VnuzJJZ9ZN9YibCRvrTqKJkN8KoicDg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)


需注意Shell执行匿名管道 a | b其实是通过Shell父进程fork出了两个子进程来实现通信的，而ab之间是不存在父子进程关系的。而命名管道是可以直接在不想关进程间通信的，因为有管道文件。

#####   消息队列

![图片](https://mmbiz.qpic.cn/mmbiz_png/wJvXicD0z2dWtyQ6fLJR8SwofF59o2Iq2ic79Oel4bn9SicQf1SiaCEic3od1I7ibyoKB7taDRmDFzDaQ1Qs3fZhWfMA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)消息队列

消息队列是保存在**内核**中的消息链表，**会涉及到用户态跟内核态到来回切换**，双方约定好消息体到数据结构，然后发送数据时将数据分成一个个独立的数据单元消息体，需注意消息队列及单个消息都有上限，日常我们到[RabbitMQ](http://mp.weixin.qq.com/s?__biz=MzI4NjI1OTI4Nw==&mid=2247490325&idx=1&sn=ab7cfedc7b8f2361cc1fab9418b314b8&chksm=ebdefa2ddca9733b76cffbcba2d5f8c0c61bd36d5ebba20aaecb07148aa0fd7a5781e16ab03e&scene=21#wechat_redirect)、[Redis ](http://mp.weixin.qq.com/s?__biz=MzI4NjI1OTI4Nw==&mid=2247488832&idx=1&sn=5999893d7fe773f54f7d097ac1c2074d&chksm=ebdef478dca97d6e2433abdeecf600669ffbb1b68eb2b744e7ed72aac4cd5c4cabf19b0d8f19&scene=21#wechat_redirect)都涉及到消息队列。

#####   共享内存

![共享空间](https://mmbiz.qpic.cn/mmbiz_png/wJvXicD0z2dWtyQ6fLJR8SwofF59o2Iq2Tzibe1KsJyiaEZ6oW5WR9PZpC12ibeRXwmARX0ujibPHJ58cziaQXZQNWLg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)


现代操作系统对内存管理采用的是虚拟内存技术，也就是每个进程都有自己独立的虚拟内存空间，不同进程的虚拟内存映射到不同的物理内存中。所以，即使进程A和进程B虚拟地址是一样的，真正访问的也是不同的物理内存地址，该模式不涉及到用户态跟内核态来回切换，JVM 就是用的共享内存模式。并且并发编程也是个难点。

#####  信号量

既然共享内存容易造成数据紊乱，那为了简单的实现共享数据在任意时刻只能被一个进程访问，此时需要信号量。

**信号量其实是一个整型的计数器，主要用于实现进程间的互斥与同步，而不是用于缓存进程间通信的数据**。

信号量表示资源的数量，核心点在于原子性的控制一个数据的值，控制信号量的方式有**PV两种原子操作**：

1. P 操作会把信号量减去 -1，相减后如果信号量 < 0，则表明资源已被占用，进程需阻塞等待。相减后如果信号量 >= 0，则表明还有资源可使用，进程可正常继续执行。
2. V 操作会把信号量加上 1，相加后如果信号量 <= 0，则表明当前有阻塞中的进程，于是会将该进程唤醒运行。相加后如果信号量 > 0，则表明当前没有阻塞中的进程。

#####  <span id="信号1">信号</span>

对于异常状态下进程工作模式需要用到信号工作方式来通知进程。比如Linux系统为了响应各种事件提供了很多异常信号`kill -l`，**信号是进程间通信机制中唯一的异步通信机制**，可以在任何时候发送信号给某一进程。比如：

1. kill -9 1412 ，表示给 PID 为 1412 的进程发送 SIGKILL 信号，用来立即结束该进程。
2. 键盘 Ctrl+C 产生 SIGINT 信号，表示终止该进程。
3. 键盘 Ctrl+Z 产生 SIGTSTP 信号，表示停止该进程，但还未结束。

有信号发生时，进程一般有三种方式响应信号：

1. 执行默认操作：Linux操作系统为众多信号配备了专门的处理操作。
2. 捕捉信号：给捕捉到的信号配备专门的信号处理函数。
3. 忽略信号：专门用来忽略某些信号，但 SIGKILL 和 SEGSTOP是无法被忽略的，为了能在任何时候结束或停止某个进程而存在。

#####   Socket编程

前面提到的管道、消息队列、共享内存、信号量和信号都是在同一台主机上进行进程间通信，**那要想跨网络与不同主机上的进程之间通信，就需要 Socket 通信**。

```
int socket(int domain, int type, int protocal)
```

上面是socket编程的核心函数，可以指定IPV4或IPV6类型，TCP或UDP类型。比如[**TCP**](http://mp.weixin.qq.com/s?__biz=MzI4NjI1OTI4Nw==&mid=2247490719&idx=1&sn=9590fea26b75698ddb37b24ef34e0c8c&chksm=ebdefda7dca974b16ac1e3ae78ff0222c4ad4bd181a70a233df8683cb3fb6199395e14bd65e6&scene=21#wechat_redirect)协议通信的 socket 编程模型如下：

![Socket编程](https://mmbiz.qpic.cn/mmbiz_png/wJvXicD0z2dWtyQ6fLJR8SwofF59o2Iq275p0TZemjpDZ6rFaqicMlVRsRibmcKQm7bz90XBhmdCY23xdsucUdPoQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

1. 服务端和客户端初始化 `socket`，得到文件描述符。
2. 服务端调用`bind`，将绑定在 IP 地址和端口。
3. 服务端调用 `listen`，进行监听。
4. 服务端调用 `accept`，等待客户端连接。
5. 客户端调用 `connect`，向服务器端的地址和端口发起连接请求。
6. 服务端 `accept` 返回用于传输的 `socket` 的文件描述符。
7. 客户端调用 `write` 写入数据，服务端调用 `read` 读取数据。
8. 客户端断开连接时，会调用 `close`，那么服务端 `read` 读取数据的时候，就会读取到了`EOF`，待处理完数据后，服务端调用 close，表示连接关闭。
9. 服务端调用 `accept`时，连接成功会返回一个已完成连接的 `socket`，后续用来传输数据。服务端有俩`socket`，一个叫作监听 `socket`，一个叫作已完成连接 `socket`。
10. 成功连接建立之后双方开始通过 read 和 write 函数来读写数据。

![UDP传输](https://mmbiz.qpic.cn/mmbiz_png/wJvXicD0z2dWtyQ6fLJR8SwofF59o2Iq2ICvtYfABibGtczmvtf4jf5nz7Ab8gykHt3E7PGUCicC2Lc2YgCf0vOIA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)


UDP比较简单，属于类似广播性质的传输，不需要维护连接。但也需要 bind，每次通信时调用 sendto 和 recvfrom 都要传入目标主机的 IP 地址和端口。

####  多线程编程

既然多进程开销过大，那平常我们经常使用到的就是多线程编程了。期间可能涉及到内存模型、JMM、Volatile、临界区等等。这些在[Java并发编程专栏](https://mp.weixin.qq.com/mp/appmsgalbum?action=getalbum&album_id=1663052626025840644&__biz=MzI4NjI1OTI4Nw==&uin=&key=&devicetype=Windows+7+x64&version=63010043&lang=zh_CN&ascene=7&fontgear=2)有讲。



### 文件管理





####  VFS 虚拟文件系统

文件系统在操作系统中主要负责将文件数据信息存储到磁盘中，起到持久化文件的作用。文件系统的基本组成单元就是文件，文件组成方式不同就会形成不同的文件系统。

文件系统有很多种而不同的文件系统应用到操作系统后需要提供统一的对外接口，此时用到了一个设计理念`没有什么是加一层解决不了的`，在用户层跟不同的文件系统之间加入一个虚拟文件系统层 `Virtual File System`。

虚拟文件系统层`定义了一组所有文件系统都支持的数据结构和标准接口`，这样程序员不需要了解文件系统的工作原理，只需要了解 VFS 提供的统一接口即可。

![图片](https://mmbiz.qpic.cn/mmbiz_png/wJvXicD0z2dWtyQ6fLJR8SwofF59o2Iq2BTTIOe5hfKdOb7EGFRducFeWIm4fe4u6QJEocFTIdfgjQgmpmTMJsw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)虚拟文件系统


日常的文件系统一般有如下三种：



1. `磁盘文件系统`：就是我们常见的EXT 2/3/4系列。
2. `内存文件系统`：数据没存储到磁盘，占用内存数据，比如`/sys`、`/proc`。进程中的一些数据映射到/proc中了。
3. `网络文件系统`：常见的网盘挂载NFS等，通过访问其他主机数据实现。

####  文件组成

以Linux系统为例，在Linux系统中一切皆文件，Linux文件系统会为每个文件分配`索引节点 inode`跟`目录项directory entry`来记录文件内容跟目录层次结构。

#####   inode

要理解`inode`要从文件储存说起。文件存储在硬盘上，硬盘的最小存储单位叫做扇区。每个扇区储存512字节。操作系统读取硬盘的时候，不会一个个扇区的读取，这样效率太低，一般一次性连续读取8个扇区(4KB)来当做一块，这种由多个扇区组成的**块**，是文件存取的最小单位。

文件数据都储存在块中，我们还必须找到一个地方储存文件的元信息，比如inode编号、文件大小、创建时间、修改时间、磁盘位置、访问权限等。几乎除了文件名以为的所有文件元数据信息都存储在一个叫叫索引节点inode的地方。可通过`stat 文件名`查看 inode 信息

每个inode都有一个号码，操作系统用inode号码来识别不同的文件。Unix/Linux系统内部不使用文件名，而使用inode号码来识别文件，用户可通过`ls -i`查看每个文件对应编号。对于系统来说文件名只是inode号码便于识别的别称或者绰号。特殊名字的文件不好删除时可以尝试用inode号删除，移动跟重命名不会导致文件inode变化，当用户尝试根据文件名打开文件时，实际上系统内部将这个过程分成三步：

1. 系统找到这个文件名对应的inode号码。
2. 通过inode号码，获取inode信息，进行权限验证等操作。
3. 根据inode信息，找到文件数据所在的block，读出数据。

需注意 inode也会消耗硬盘空间，硬盘格式化后会被分成**超级块**、**索引节点区**和**数据块区**三个区域：

1. `超级块区`：用来存储文件系统的详细信息，比如块大小，块个数等信息。一般文件系统挂载后就会将数据信息同步到内存。

2. `索引节点区`：用来存储索引节点 inode  table。每个inode一般为128字节或256字节，一般每1KB或2KB数据就需设置一个inode。一般为了加速查询会把索引数据缓存到内存。

3. `数据块区`：真正存储磁盘数据的地方。

   ```
   df -i # 查看每个硬盘分区的inode总数和已经使用的数量
   sudo dumpe2fs -h /dev/hda | grep "Inode size" # 查看每个inode节点的大小
   ```

#####   目录

Unix/Linux系统中**目录directory也是一种文件**，打开目录实际上就是打开目录文件。目录文件内容就是一系列目录项的列，目录项的内容包含**文件的名字、文件类型、索引节点指针以及与其他目录项的层级关系**。

为避免频繁读取磁盘里的目录文件，内核会把已经读过的目录文件用`目录项`这个数据结构缓存在内存，方便用户下次读取目录信息，目录项可包含目录或文件，不要惊讶于可以保存目录，目录格式的目录项里面保存的是目录里面一项一项的文件信息。

#####  软连接跟硬链接

![软连接跟硬链接](https://mmbiz.qpic.cn/mmbiz_png/wJvXicD0z2dWtyQ6fLJR8SwofF59o2Iq2SjKA2B06sYNDmHHnlur9IQ1PqaLJpzV5SiaMz6eUFyZEhInyibh6DqRQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)


**硬链接**：老文件A被创建若干个硬链接B、C后。A、B、C三个文件的inode是相同的，所以不能跨文件系统。同时只有ABC全部删除，系统才会删除源文件。
**软链接**：相当于基于老文件A新建了个文件B，该文件B有新的inode，不过文件B内容是老文件A的路径。所以软链接可以跨文件系统。当老文件A删除后，文件B仍然存在，不过找不到指定文件了。

```bash
[sowhat@localhost ~]$ ln [选项] 源文件 目标文件
选项：
-s：建立软链接文件。如果不加 "-s" 选项，则建立硬链接文件；
-f：强制。如果目标文件已经存在，则删除目标文件后再建立链接文件；
```

####    文件存储

说文件存储前需了解**文件系统操作基本单位是数据块**，而平常用户操作字节到数据块之间是需要转换的，当然这些文件系统都帮我们对接好了。接下来看文件系统是如何按照数据块， 文件在磁盘中存储时候主要分为`连续空间存储`跟`非连续空间存储`

#####  连续空间存储

1. `实现`：连续空间存储的意思就跟数组存储一样，找个连续的空间一次性把数据存储进去，**文件头**存储起始位置跟数据长度即可。

2. `优势`：读写效率高，磁盘寻址一次即可。

3. `劣势`：容易产生空间碎片，并且文件扩容不方便。

   ![图片](https://mmbiz.qpic.cn/mmbiz_png/wJvXicD0z2dWtyQ6fLJR8SwofF59o2Iq2MichpPAB24cqkKBlu2Xxguq9ric4mo2BlFiafWFFGlibf40RDkexEC3r6A/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)连续存储

#####   非连续空间存储之链表

**隐式链表**

1. `实现`：文件头包含StartBlock、EndBlock。每个BLock有隐藏的next指针，跟单向链表一样。
2. `缺点`：只能通过链式不断往下查找数据，不支持快速直接访问。

![隐式链表](https://mmbiz.qpic.cn/mmbiz_png/wJvXicD0z2dWtyQ6fLJR8SwofF59o2Iq287ibqYp3pFSV4sWFJp9HzDYIrWTb34av259CicZxdMjTgtXmg9uITovg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

****

1. `实现`：把每个Block中的next指针存储到内存`文件分配表`中，通过遍历数组方式实现拿到全部数据。

2. `缺点`：前面说1KB就有个inode指针，如果磁盘数据很大那就需要很大的**文件分配表**来存储映射关系了，

   ![显示链表](https://mmbiz.qpic.cn/mmbiz_png/wJvXicD0z2dWtyQ6fLJR8SwofF59o2Iq2FWRaUE8OiaPqDW5IzHYJt7MJmQ930cnuKianSdDoaBp7X4ibn78luk98w/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

##### 非连续空间存储之索引

1. `实现`：整个文件类型一本新华字典，真实的数据块在词典实际位置存储着，但文件所需数据块的索引位置会被汇总起来形成目录索引放在字典前头。
2. `优势`：不会产生碎片，文件可动态扩容，并且支持顺序跟随机读写。
3. `劣势`：可能一个小文件都要占用一个目录索引，文件过大导致索引指针一个容不下，可能还需要有`多级索引`或`索引+链表`模式。

![图片](https://mmbiz.qpic.cn/mmbiz_png/wJvXicD0z2dWtyQ6fLJR8SwofF59o2Iq2dQ8gWZ4dR8JdBS8xZpPgZJPbtsBvdulOotSOSCibwPpm2lrhztXNYxA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)索引存储


这些存储方式各有利弊，所以操作系统才存储的时候一般是根据文件的大小进行动态的变化存储方式的，跟STL中的快排底层 = 快排 + 插入排序 + 堆排 一样的道理。

#####   空闲空间管理

为了避免用户存储数据时候遍历全部磁盘空间来寻找可以数据块，一般有如下几种记录方法。

1. `空闲表`：动态的维护一个空闲数据块列表，每行存储空闲块的开始位置跟空闲长度。适合少量有少量空闲数据块时。

   ![空闲表](https://mmbiz.qpic.cn/mmbiz_png/wJvXicD0z2dWtyQ6fLJR8SwofF59o2Iq2Q8jLy4xaQQx4TVE1XcYuN6R8fTMgicjd6wnLOLEqlvQtjBbl9KwHA8A/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

2. `空闲链表`：将空闲的数据库用next指针串联起来，缺点是不能随机访问。

   ![空闲链表](https://mmbiz.qpic.cn/mmbiz_png/wJvXicD0z2dWtyQ6fLJR8SwofF59o2Iq2PdK0qQ9D0xIY8D3e5tiaiaHGJG4BVWmpicberdSDa7l0SCL1BkzZqh5Ng/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

3. `位图法`：利用Bit的 01 表示数据块可用跟不可用，简单方便，**inode跟空闲数据库都用的此方法**。

   ![位图法](https://mmbiz.qpic.cn/mmbiz_png/wJvXicD0z2dWtyQ6fLJR8SwofF59o2Iq2nQHcAG3ApMgUWGFHkpwhx1wezVS3TrtG3p5xU0iaxiaQKZhYK8lFDIHA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)



### 输入输出管理

####  设备控制器跟驱动程序

#####  设备控制器

![设备控制器](https://mmbiz.qpic.cn/mmbiz_png/wJvXicD0z2dWtyQ6fLJR8SwofF59o2Iq2aiaN8HEFnlD0hGVyOzbN2o2ou3x0uSDrJibSKWsjv37UBpmJ42JI3aJQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)


操作系统为统一管理众多的设备并且屏蔽设备之间的差异，给每个设备都安装了个小CPU叫**设备控制器**。每个设备控制器都知道自己对应外设的功能跟用法，并且每个**设备控制器**都有独有的寄存器用来跟CPU通信。



1. 读设备寄存器值了解设备状态，是否可以接收新指令。
2. 操作系统给设备寄存器写入一些指令可以实现发送数据、接收数据等等操作。

控制器一般分为**数据寄存器、命令寄存器跟状态寄存器**，CPU 通过读、写设备控制器中的寄存器来便捷的控制设备：

1. `数据寄存器`：CPU 向 I/O 设备写入需要传输的数据，比如打印what，CPU 就要先发送一个w字符给到对应的 I/O 设备。
2. `命令寄存器`：CPU 发送命令来告诉 I/O 设备要进行输入/输出操作，于是就会交给 I/O 设备去工作，任务完成后，会把状态寄存器里面的状态标记为完成。
3. `状态寄存器`：用来告诉 CPU 现在已经在工作或工作已经完成，只有状态寄存标记成已完成，CPU 才能发送下一个字符和命令。

同时输入输出设备可分为`块设备`跟`字符设备`。

1. `块设备`：用来把数据存储在固定大小的块中，每个块有自己的地址，硬盘、U盘等是常见的块设备。块设备一般数据传输较大为避免频繁IO，控制器中有个可读写等**数据缓冲区**。Linux操作系统为屏蔽不同块设备带来的差异引入了**通用块层**，**通用块层**是处于文件系统和磁盘驱动中间的一个块设备抽象层，主要提供如下俩功能：

> 1. 向上为文件系统和应用程序，提供访问块设备的标准接口，向下把各种不同的磁盘设备抽象为统一的块设备，并在内核层面提供一个框架来管理这些设备的驱动程序。
> 2. 通用层还会给文件系统和应用程序发来的 I/O进行**调度**，主要目的是为了提高磁盘读写的效率。

1. `字符设备`：以字符为单位发送或接收一个字符流，字符设备是不可寻址的，也没有任何寻道操作，鼠标是常见的字符设备。

CPU一般通过**IO端口**跟**内存映射IO**来跟设备的控制寄存器和数据缓冲区进行通信

1. `IO端口`：每个控制寄存器被分配一个 I/O 端口，可以通过特殊的汇编指令操作这些寄存器，比如 in/out 类似的指令。
2. `内存映射IO`：将所有控制寄存器映射到内存空间中，这样就可以像读写内存一样读写数据缓冲区。

#####  驱动接口

![驱动程序](https://mmbiz.qpic.cn/mmbiz_png/wJvXicD0z2dWtyQ6fLJR8SwofF59o2Iq2Cic1HIGee0USUmtB8vhLaIgcnr9s1zbSEqiaibCc2rQS7rGiaI2YOpUINQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

设备控制器屏蔽了设备细节，但每种设备的控制器的寄存器、缓冲区等使用模式都是不同的，它属于硬件。在操作系统图范畴内为了屏蔽设备控制器的差异，引入了**设备驱动程序**，**不同设备到驱动程序会提供统一接口给操作系统来调用**，这样操作系统内核会像调用本地代码一样使用设备驱动程序接口。

设备发出IO请求就是在**设备驱动程序**中来响应到，它会根据中断类型调用响应到中断处理程序进行处理。

![中断请求流程](https://mmbiz.qpic.cn/mmbiz_png/wJvXicD0z2dWtyQ6fLJR8SwofF59o2Iq2lyHEp0ZiaiaBkibUlcPEUiaQ12KFF9qfnweEJvlQPX3QsS1CfVGicR0k2hw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

####   IO 控制

CPU发送指令让那个设备控制器去读写数据，完毕后如何通知CPU呢？

#####  轮询模式

控制器中有个**状态寄存器**，CPU不断**轮**询查看寄存器状态，该模式会傻瓜式的一直占用CPU。

![图片](https://mmbiz.qpic.cn/mmbiz_png/wJvXicD0z2dWtyQ6fLJR8SwofF59o2Iq2ts7us3oGfQdVHbmY7ibgrCxC8ygLiaCtS56ThmvcZVZ7fRt9md9M6uBA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)轮询模式

#####   IO 中断请求

![中断模式](https://mmbiz.qpic.cn/mmbiz_png/wJvXicD0z2dWtyQ6fLJR8SwofF59o2Iq2PbodOEygWeVFtdXQd38WoEn26IZpEoVJSaIiamrZXdmOrykzj2Elw8Q/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)


控制器有个中断控制器，当设备完成任务后触发中断到中断控制器，中断控制器就通知 CPU来处理中断请求。中断有两种，一种是**软中断**，比如代码调用 INT 指令触发。一种是**硬件中断**，硬件通过中断控制器触发的。但中断方式对于频繁读写磁盘数据的操作就不太友好了，会频繁打断CPU。

这里说下磁盘高速缓存 **PageCache**，它是用来缓存最近被CPU访问的数据到内存中，并且还具有预读功能，可能你读前16KB数据，已经把后16KB数据给你缓存好了。

> 1. **pagecache** : 页缓存，当进程需读取磁盘文件时，linux先分配一些内存，将数据从磁盘读区到内存中，然后再将数据传给进程。当进程需写数据到磁盘时，linux先分配内存接收用户数据，然后再将数据从内存写到磁盘。同时pagecache由于大小受限，所以一般只缓存最近被访问的数据，数据不足时还需访问磁盘。

#####   DMA 模式

`Direct Memory Access` 直接内存访问，在硬件DMA控制器的支持下，**在进行 I/O 设备和内存的数据传输的时候，数据搬运的工作全部交给 DMA 控制器，而 CPU 不再参与任何与数据搬运相关的事情，让CPU 去处理别的事**。

![图片](https://mmbiz.qpic.cn/mmbiz_png/wJvXicD0z2dWtyQ6fLJR8SwofF59o2Iq2rLcVK2ac1ZWuvUFUKowSLlianj0bklZ5ElE4mnXn2G8eq2r3m8fhRCA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)DMA模式

可以发现整个数据传输过程中CPU是不会直接参与数据搬运工作，由DMA来直接负责数据读取工作，现如今每个IO设备一般都自带DMA控制器。读数据时候仅仅在传送开始跟结束时需要CPU干预。

#####   Zero Copy

Zero Copy 全程不会通过 CPU 来搬运数据，所有的数据都是通过 DMA 来进行传输的，中间只需要经过2次上下文切换跟2次DMA数据拷贝，相比最原始读写方式至少速度翻倍。其实在Kafka中已经讲过Zero Copy了。

######  老版本读写

老版本的简单读写操作中间不对数据做任何操作。期间会发生**4次用户态跟内核态的切换。2次DMA数据拷贝，2次CPU数据拷贝**。

![老式读写](https://mmbiz.qpic.cn/mmbiz_png/wJvXicD0z2dWtyQ6fLJR8SwofF59o2Iq2ic61AurriamwAQMneiawhQ7w9bkIuVfyVt5XhCFKun8SqsQ3aBHQBzy7w/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)


提速方法就是需减少用户态与内核态的上下文切换和内存拷贝的次数。数据传输时从内核的读缓冲区拷贝到用户的缓冲区，再从用户缓冲区拷贝到 socket 缓冲区的这个过程是没有必要的。接下来

接下来按照三个版本说下Zero Copy 发展史。

######   mmap 跟 write

![mmap + write](https://mmbiz.qpic.cn/mmbiz_png/wJvXicD0z2dWtyQ6fLJR8SwofF59o2Iq2johrpubN4b8EMyKG4sYyIAjibBsKJTzYmmMJIGPd3hmor9xdCefpibAw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)


思路就是用**mmap**替代read函数，mmap调用时会**直接把内核缓冲区里的数据映射到用户空间**，此时减少了一次数据拷贝，但仍然需要通过 CPU 把内核缓冲区的数据拷贝到 socket 缓冲区里，而且仍然需要 4 次上下文切换，因为系统调用还是 2 次。

```c
buf = mmap(file, len);
write(sockfd, buf, len);
```

######   sendfile

Linux 内核版本 2.1版本提供了函数 **sendfile()**。

```c
ssize_t sendfile(int out_fd, int in_fd, off_t *offset, size_t count);
out_fd : 目的文件描述符
in_fd:源文件描述符
offset:源文件内偏移量
count:打算复制数据长度
ssize_t:实际上复制数据的长度
```

可以发现一个 sendfile = read + write，避免了2次用户态跟内核态来回切换，并且可以直接把内核缓冲区里的数据拷贝到 socket 缓冲区里，这样就只有 2 次上下文切换，和 3 次数据拷贝。

![图片](https://mmbiz.qpic.cn/mmbiz_png/wJvXicD0z2dWtyQ6fLJR8SwofF59o2Iq2bicp2pAamEuiaUBCicokYCSSvh1b3kQeBibAbjp1CjhPkoqqJfWwn7vGUw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)sendfile模式

######   真正的零拷贝

Linux 内核 2.4如果网卡支持SG-DMA 技术，可以减少通过 CPU 把内核缓冲区里的数据拷贝到 socket 缓冲区的过程。

```
$ ethtool -k eth0 | grep scatter-gather
scatter-gather: on
```

SG-DMA 技术可以直接将内核缓存中的数据拷贝到网卡的缓冲区里，此过程不需要将数据从操作系统内核缓冲区拷贝到 socket 缓冲区中，这样就减少了一次数据拷贝。

![ZeroCopy](https://mmbiz.qpic.cn/mmbiz_png/wJvXicD0z2dWtyQ6fLJR8SwofF59o2Iq2uDxicTRj2iaoO1emHgugDsibIkR8NqUWIubrRcibL9503NKKht9gm6OP3Q/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

######  文件传输规则

不要以为会了Zero Copy后，无论大小文件都用Zero Copy。实际工作中一般小文件采用Zero Copy技术，而大文件会用异步IO。至于为啥，且看如下分析：

前面说的数据从磁盘读到内核缓冲区就是读到PageCache中，PageCache具有缓存跟预读功能。但当传输超大文件时PageCache会不失效，因为大文件会快速占满PageCache区，但这些文件又只是一次访问，会造成其他热点小文件无法使用PageCache，所以索性不用PageCache，使用异步IO的了。至于异步IO是啥呢？下文在说。

#### IO分层

![IO分层](https://mmbiz.qpic.cn/mmbiz_png/wJvXicD0z2dWtyQ6fLJR8SwofF59o2Iq2Ku5cdvy2oAibt1QjjlFT7mFJfEnyagY27bLx4Ff3PHIfvJK04u0MvfQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)


Linux 存储系统的 I/O 由上到下可以分为**文件系统层**、**通用块层**、**设备层**。

1. 文件系统层向上为应用程序统一提供了标准的文件访问接口，向下会通过通用块层来存储和管理磁盘数据。
2. 通用块层包括块设备的 I/O 队列和 I/O 调度器，通过IO调度器处理IO请求。
3. 设备层包括硬件设备、设备控制器和驱动程序，负责最终物理设备的 I/O 操作。

Linux系统中的IO**读取提速**：

1. 为提高文件访问效率会使用页缓存、索引节点缓存、目录项缓存等多种缓存机制，目的是为了减少对块设备的直接调用。
2. 为了提高块设备的访问效率， 会使用缓冲区，来缓存块设备的数据。

## [寄存器](https://mp.weixin.qq.com/s/wWbj7aek4lo9gbBgzrn7ww)

下面我们就来介绍一下关于寄存器的相关内容。我们知道，`寄存器`是 CPU 内部的构造，它主要用于信息的存储。除此之外，CPU 内部还有`运算器`，负责处理数据；`控制器`控制其他组件；`外部总线`连接 CPU 和各种部件，进行数据传输；`内部总线`负责 CPU 内部各种组件的数据处理。

那么对于我们所了解的汇编语言来说，我们的主要关注点就是 `寄存器`。

为什么会出现寄存器？因为我们知道，程序在内存中装载，由 CPU 来运行，CPU 的主要职责就是用来处理数据。那么这个过程势必涉及到从存储器中读取和写入数据，因为它涉及通过控制总线发送数据请求并进入存储器存储单元，通过同一通道获取数据，这个过程非常的繁琐并且会涉及到大量的内存占用，而且有一些常用的内存页存在，其实是没有必要的，因此出现了寄存器，存储在 CPU 内部。

寄存器的官方叫法有很多，Wiki 上面的叫法是 `Processing Register`， 也可以称为 `CPU Register`，计算机中经常有一个东西多种叫法的情况，反正你知道都说的是寄存器就可以了。

认识寄存器之前，我们首先先来看一下 CPU 内部的构造。

![图片](https://mmbiz.qpic.cn/mmbiz_png/A3ibcic1Xe0iaT0AoicUdQic4AmqNbUhxmGibkReTiaOS7vW7ocM1uXsccCu5VqTb1gN1lpicu9MpwN1WgQsciabI5ibtNmQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

CPU 从逻辑上可以分为 3 个模块，分别是控制单元、运算单元和存储单元，这三部分由 CPU 内部总线连接起来。

几乎所有的冯·诺伊曼型计算机的 CPU，其工作都可以分为5个阶段：**「取指令、指令译码、执行指令、访存取数、结果写回」**。

- `取指令`阶段是将内存中的指令读取到 CPU 中寄存器的过程，程序寄存器用于存储下一条指令所在的地址
- `指令译码`阶段，在取指令完成后，立马进入指令译码阶段，在指令译码阶段，指令译码器按照预定的指令格式，对取回的指令进行拆分和解释，识别区分出不同的指令类别以及各种获取操作数的方法。
- `执行指令`阶段，译码完成后，就需要执行这一条指令了，此阶段的任务是完成指令所规定的各种操作，具体实现指令的功能。
- `访问取数`阶段，根据指令的需要，有可能需要从内存中提取数据，此阶段的任务是：根据指令地址码，得到操作数在主存中的地址，并从主存中读取该操作数用于运算。
- `结果写回`阶段，作为最后一个阶段，结果写回（Write Back，WB）阶段把执行指令阶段的运行结果数据写回到 CPU 的内部寄存器中，以便被后续的指令快速地存取；

### 计算机架构中的寄存器

寄存器是一块速度非常快的计算机内存，下面是现代计算机中具有存储功能的部件比对，可以看到，寄存器的速度是最快的，同时也是造价最高昂的。

![图片](https://mmbiz.qpic.cn/mmbiz_png/A3ibcic1Xe0iaT0AoicUdQic4AmqNbUhxmGibkp4SEm10I17OhzkhpOczqUrSHRAWInyNh9uBPwORnQwYhZ71rEkZzVA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

我们以 intel 8086 处理器为例来进行探讨，8086 处理器是 x86 架构的前身。在 8086 后面又衍生出来了 8088 。

在 8086 CPU 中，地址总线达到 20 根，因此最大寻址能力是 2^20 次幂也就是 1MB 的寻址能力，8088 也是如此。

在 8086 架构中，所有的内部寄存器、内部以及外部总线都是 16 位宽，可以存储两个字节，因为是完全的 16 位微处理器。8086 处理器有 14 个寄存器，每个寄存器都有一个特有的名称，即

**「AX，BX，CX，DX，SP，BP，SI，DI，IP，FLAG，CS，DS，SS，ES」**

这 14 个寄存器有可能进行具体的划分，按照功能可以分为三种

- 通用寄存器
- 控制寄存器
- 段寄存器

下面我们分别介绍一下这几种寄存器

### 通用寄存器

通用寄存器主要有四种 ，即 **「AX、BX、CX、DX」** 同样的，这四个寄存器也是 16 位的，能存放两个字节。AX、BX、CX、DX 这四个寄存器一般用来存放数据，也被称为 `数据寄存器`。它们的结构如下

![图片](https://mmbiz.qpic.cn/mmbiz_png/A3ibcic1Xe0iaT0AoicUdQic4AmqNbUhxmGibkngevRMlwdicpNKLx0ZZXFQ8sR77efw72X8J71DbkicibZNx9wzzS98vSQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

8086 CPU 的上一代寄存器是 8080 ，它是一类 8 位的 CPU，为了保证兼容性，8086 在 8080 上做了很小的修改，8086 中的通用寄存器 AX、BX、CX、DX 都可以独立使用两个 8 位寄存器来使用。

在细节方面，AX、BX、CX、DX 可以再向下进行划分

- `AX(Accumulator Register)` ：累加寄存器，它主要用于输入/输出和大规模的指令运算。
- `BX(Base Register)`：基址寄存器，用来存储基础访问地址
- `CX(Count Register)`：计数寄存器，CX 寄存器在迭代的操作中会循环计数
- `DX(data Register)`：数据寄存器，它也用于输入/输出操作。它还与 AX 寄存器以及 DX 一起使用，用于涉及大数值的乘法和除法运算。

这四种寄存器可以分为上半部分和下半部分，用作八个 8 位数据寄存器

- **「AX 寄存器可以分为两个独立的 8 位的 AH 和 AL 寄存器；」**
- **「BX 寄存器可以分为两个独立的 8 位的 BH 和 BL 寄存器；」**
- **「CX 寄存器可以分为两个独立的 8 位的 CH 和 CL 寄存器；」**
- **「DX 寄存器可以分为两个独立的 8 位的 DH 和 DL 寄存器；」**

除了上面 AX、BX、CX、DX 寄存器以外，其他寄存器均不可以分为两个独立的 8 位寄存器

如下图所示。

![图片](https://mmbiz.qpic.cn/mmbiz_png/A3ibcic1Xe0iaT0AoicUdQic4AmqNbUhxmGibkIqb27wvz5qfMUIdAl8pqBK3omSPBXZOPVUV5nvjSvYw5YtfaKL4PBQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

合起来就是

![图片](https://mmbiz.qpic.cn/mmbiz_png/A3ibcic1Xe0iaT0AoicUdQic4AmqNbUhxmGibkN54zPvR1CtgIueJOlp1W1CouC1HulW2ichicfr4gQILpsntaeE01XACA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

AX 的低位（0 - 7）位构成了 AL 寄存器，高 8 位（8 - 15）位构成了 AH 寄存器。AH 和 AL 寄存器是可以使用的 8 位寄存器，其他同理。

在认识了寄存器之后，我们通过一个示例来看一下数据的具体存储方式。

比如数据 19 ，它在 16 位存储器中所存储的表示如下

![图片](https://mmbiz.qpic.cn/mmbiz_png/A3ibcic1Xe0iaT0AoicUdQic4AmqNbUhxmGibkAe2Y53SNwiaeWyYM4PYStm3G0xmOXJ8slKhicf2n4ibbq4Bpte57cfY7g/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

寄存器的存储方式是先存储低位，如果低位满足不了就存储高位，如果低位能够满足，高位用 0 补全，在其他低位能满足的情况下，其余位也用 0 补全。

8086 CPU 可以一次存储两种类型的数据

- `字节(byte)`：一个字节由 8 bit 组成，这是一种恒定不变的存储方式
- `字(word)`：字是由指令集或处理器硬件作为单元处理的固定大小的数据，对于 intel 来说，一个字长就是两个字节，字是计算机一个非常重要的特征，针对不同的指令集架构来说，计算机一次处理的数据也是不同的。也就是说，针对不同指令集的机器，一次能处理不用的字长，有字、双字（32位）、四字（64位）等。

#### AX 寄存器

我们上面探讨过，AX 的另外一个名字叫做累加寄存器或者简称为累加器，其可以分为 2 个独立的 8 位寄存器 AH 和 AL；在编写汇编程序中，AX 寄存器可以说是使用频率最高的寄存器。

下面是几段汇编代码

```
mov ax,20   /* 将 20 送入寄存器 AX*/
mov ah,80   /* 将 80 送入寄存器 AH*/
add ax,10   /* 将寄存器 AX 中的数值加上 8 */
```

> ❝
>
> 这里注意下：上面代码中出现的是 ax、ah ，而注释中确是 AX、AH ，其实含义是一样的，不区分大小写。
>
> ❞

AX 相比于其他通用寄存器来说，有一点比较特殊，AX 具有一种特殊功能的使用，那就是使用 DIV 和 MUL 指令式使用。

> ❝
>
> DIV 是 8086 CPU 中的`除法`指令。
>
> MUL 是 8086 CPU 中的`乘法`指令。
>
> ❞

#### BX 寄存器

BX 被称为数据寄存器，即表明其能够暂存一般数据。同样为了适应以前的 8 位 CPU ，而可以将 BX 当做两个独立的 8 位寄存器使用，即有 BH 和 BL。BX 除了具有暂存数据的功能外，还用于 `寻址`，即寻找物理内存地址。BX 寄存器中存放的数据一般是用来作为`偏移地址` 使用的，因为偏移地址当然是在基址地址上的偏移了。偏移地址是在段寄存器中存储的，关于段寄存器的介绍，我们后面再说。

#### CX 寄存器

CX 也是数据寄存器，能够暂存一般性数据。同样为了适应以前的 8 位 CPU ，而可以将 CX 当做两个独立的 8 位寄存器使用，即有 CH 和 CL。除此之外，CX 也是有其专门的用途的，CX 中的 C 被翻译为 Counting 也就是计数器的功能。当在汇编指令中使用循环 LOOP 指令时，可以通过 CX 来指定需要循环的次数，每次执行循环 LOOP 时候，CPU 会做两件事

- 一件事是计数器自动减 1

- 还有一件就是判断 CX 中的值，如果 CX 中的值为 0 则会跳出循环，而继续执行循环下面的指令，

  当然如果 CX 中的值不为 0 ，则会继续执行循环中所指定的指令 。

#### DX 寄存器

DX 也是数据寄存器，能够暂存一般性数据。同样为了适应以前的 8 位 CPU ，DX 的用途其实在前面介绍 AX 寄存器时便已经有所介绍了，那就是支持 MUL 和 DIV 指令。同时也支持数值溢出等。

### 段寄存器

CPU 包含四个段寄存器，用作程序指令，数据或栈的基础位置。实际上，对 IBM PC 上所有内存的引用都包含一个段寄存器作为基本位置。

段寄存器主要包含

- `CS(Code Segment)` ：代码寄存器，程序代码的基础位置
- `DS(Data Segment)`：数据寄存器，变量的基本位置
- `SS(Stack Segment)`：栈寄存器，栈的基础位置
- `ES(Extra Segment)`：其他寄存器，内存中变量的其他基本位置。

### 索引寄存器

索引寄存器主要包含段地址的偏移量，索引寄存器主要分为

- `BP(Base Pointer)`：基础指针，它是栈寄存器上的偏移量，用来定位栈上变量
- `SP(Stack Pointer)`: 栈指针，它是栈寄存器上的偏移量，用来定位栈顶
- `SI(Source Index)`: 变址寄存器，用来拷贝源字符串
- `DI(Destination Index)`: 目标变址寄存器，用来复制到目标字符串

### 状态和控制寄存器

就剩下两种寄存器还没聊了，这两种寄存器是指令指针寄存器和标志寄存器：

- `IP(Instruction Pointer)`：指令指针寄存器，它是从 Code Segment 代码寄存器处的偏移来存储执行的下一条指令

- `FLAG` : Flag 寄存器用于存储当前进程的状态，这些状态有

- - 位置 (Direction)：用于数据块的传输方向，是向上传输还是向下传输
  - 中断标志位 (Interrupt) ：1 - 允许；0 - 禁止
  - 陷入位 (Trap) ：确定每条指令执行完成后，CPU 是否应该停止。1 - 开启，0 - 关闭
  - 进位 (Carry) : 设置最后一个无符号算术运算是否带有进位
  - 溢出 (Overflow) : 设置最后一个有符号运算是否溢出
  - 符号 (Sign) : 如果最后一次算术运算为负，则设置  1 =负，0 =正
  - 零位 (Zero) : 如果最后一次算术运算结果为零，1 = 零
  - 辅助进位 (Aux Carry) ：用于第三位到第四位的进位
  - 奇偶校验 (Parity) : 用于奇偶校验

### 物理地址

我们大家都知道， CPU 访问内存时，需要知道访问内存的具体地址，内存单元是内存的基本单位，每一个内存单元在内存中都有唯一的地址，这个地址即是 `物理地址`。而 CPU 和内存之间的交互有三条总线，即数据总线、控制总线和地址总线。

![图片](https://mmbiz.qpic.cn/mmbiz_png/A3ibcic1Xe0iaT0AoicUdQic4AmqNbUhxmGibknwXa4GW5xib7q9N3ereVdWPWY6AxBMdMiapkMVBWCBLF5F9uSzkibYDkA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

CPU 通过地址总线将物理地址送入存储器，那么 CPU 是如何形成的物理地址呢？这将是我们接下来的讨论重点。

现在，我们先来讨论一下和 8086 CPU 有关的结构问题。

cxuan 和你聊了这么久，你应该知道 8086 CPU 是 16 位的 CPU 了，那么，什么是 16 位的 CPU 呢？

你可能大致听过这个回答，16 位 CPU 指的是 CPU 一次能处理的数据是 16 位的，能回答这个问题代表你的底层还不错，但是不够全面，其实，16 位的 CPU 指的是

- CPU 内部的运算器一次最多能处理 16 位的数据

> ❝
>
> 运算器其实就是 ALU，运算控制单元，它是 CPU 内部的三大核心器件之一，主要负责数据的运算。
>
> ❞

- 寄存器的最大宽度为 16 位

> ❝
>
> 这个寄存器的最大宽度值就是通用寄存器能处理的二进制数的最大位数
>
> ❞

- 寄存器和运算器之间的通路为 16 位

> ❝
>
> 这个指的是寄存器和运算器之间的总线，一次能传输 16 位的数据
>
> ❞

好了，现在你应该知道为什么叫做 16 位 CPU 了吧。

在你知道上面这个问题的答案之后，我们下面就来聊一聊如何计算物理地址。

8086 CPU 有 20 位地址总线，每一条总线都可以传输一位的地址，所以 8086 CPU 可以传送 20 位地址，也就是说，8086 CPU 可以达到 2^20 次幂的寻址能力，也就是 1MB。8086 CPU 又是 16 位的结构，从 8086 CPU 的结构看，它只能传输 16 位的地址，也就是 2^16 次幂也就是 64 KB，那么它如何达到 1MB 的寻址能力呢？

原来，8086 CPU 的内部采用两个 16 位地址合成的方式来传输一个 20 位的物理地址，如下图所示

![图片](https://mmbiz.qpic.cn/mmbiz_png/A3ibcic1Xe0iaT0AoicUdQic4AmqNbUhxmGibkib4glAFnWXrL4IEb3QCcGrSAwSFv4icVxOanVr25Pf4hn4pg9VMW1q3A/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

叙述一下上图描述的过程

CPU 相关组件提供两个地址：段地址和偏移地址，这两个地址都是 16 位的，他们经由`地址加法器`变为 20 位的物理地址，这个地址即是输入输出控制电路传递给内存的物理地址，由此完成物理地址的转换。

地址加法器采用 **「物理地址 = 段地址 \* 16 + 偏移地址」** 的方法用段地址和偏移地址合成物理地址。

下面是地址加法器的工作流程

![图片](https://mmbiz.qpic.cn/mmbiz_png/A3ibcic1Xe0iaT0AoicUdQic4AmqNbUhxmGibkia02l1dhMq85l47g1B9YHmODgGziaianMW8pwb01koaOu3AlvysErvDSw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

其实段地址 * 16 ，就是左移 4 位。在上面的叙述中，物理地址 = 段地址 * 16 + 偏移地址，其实就是**「基础地址 + 偏移地址 = 物理地址」** 寻址模式的一种具体实现方案。基础地址其实就等于段地址 * 16。

你可能不太清楚 `段` 的概念，下面我们就来探讨一下。

#### 什么是段

段这个概念经常出现在操作系统中，比如在内存管理中，操作系统会把不同的数据分成 `段`来存储，比如 **「代码段、数据段、bss 段、rodata 段」** 等。

但是这些的划分并不是内存干的，cxuan 告诉你是谁干的，这其实是幕后 Boss CPU 搞的，内存当作了声讨的对象。

其实，内存没有进行分段，分段完全是由 CPU 搞的，上面聊过的通过基础地址 + 偏移地址 = 物理地址的方式给出内存单元的物理地址，使得我们可以分段管理 CPU。

如图所示

![图片](https://mmbiz.qpic.cn/mmbiz_png/A3ibcic1Xe0iaT0AoicUdQic4AmqNbUhxmGibkOJiaoniax1JQniasYpFFTtQmuGFXCnRp8LEXRibLJsLULRh2FYLW8M5ezw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

这是两个 16 KB 的程序分别被装载进内存的示意图，可以看到，这两个程序的段地址的大小都是 16380。

> ❝
>
> 这里需要注意一点， 8086 CPU 段地址的计算方式是段地址 * 16，所以，16 位的寻址能力是 2^16 次方，所以一个段的长度是 64 KB。
>
> ❞

#### 段寄存器

cxuan 在上面只是简单为你介绍了一下段寄存器的概念，介绍的有些浅，而且介绍段寄存器不介绍段也有**「不知庐山真面目」**的感觉，现在为你详细的介绍一下，相信看完上面的段的概念之后，段寄存器也是手到擒来。

我们在合成物理地址的那张图提到了 `相关部件` 的概念，这个相关部件其实就是`段寄存器`，即 **「CS、DS、SS、ES」** 。8086 的 CPU 在访问内存时，由这四个寄存器提供内存单元的段地址。

##### CS 寄存器

要聊 CS 寄存器，那么 IP 寄存器是你绕不过去的曾经。CS 和 IP 都是 8086 CPU 非常重要的寄存器，它们指出了 CPU 当前需要读取指令的地址。

> ❝
>
> CS 的全称是 Code Segment，即代码寄存器；而 IP 的全称是 Instruction Pointer ，即指令指针。现在知道这两个为什么一起出现了吧！
>
> ❞

在 8086 CPU 中，由 `CS:IP` 指向的内容当作指令执行。如下图所示

![图片](https://mmbiz.qpic.cn/mmbiz_png/A3ibcic1Xe0iaT0AoicUdQic4AmqNbUhxmGibkX2vNW1DUA2un36gmxm9DD0mc2NwCIibuIUfP6USOA1GkpHsg2WqCr7g/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

说明一下上图

在 CPU 内部，由 CS、IP 提供段地址，由加法器负责转换为物理地址，输入输出控制电路负责输入/输出数据，指令缓冲器负责缓冲指令，指令执行器负责执行指令。在内存中有一段连续存储的区域，区域内部存储的是机器码、外面是地址和汇编指令。

上面这幅图的段地址和偏移地址分别是 2000 和 0000，当这两个地址进入地址加法器后，会由地址加法器负责将这两个地址转换为物理地址

![图片](https://mmbiz.qpic.cn/mmbiz_png/A3ibcic1Xe0iaT0AoicUdQic4AmqNbUhxmGibko4ib1gKiaOZ7Pk6yibApb0NSYsoQR3P066xcjNMdt7AVVeKtyzUtZrl9Q/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

然后地址加法器负责将指令输送到输入输出控制电路中

![图片](https://mmbiz.qpic.cn/mmbiz_png/A3ibcic1Xe0iaT0AoicUdQic4AmqNbUhxmGibkrAFP2ePxFkkiaOTGHTjcib9rDyibhnZcqV0gwAbZlOvxYlB0AAxq4Yf9Q/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

输入输出控制电路将 20 位的地址总线送到内存中。

![图片](https://mmbiz.qpic.cn/mmbiz_png/A3ibcic1Xe0iaT0AoicUdQic4AmqNbUhxmGibkbkYXhknc8HwtbAXicCdIMd14MU4dfcsh9jtsIf6tvJmjNLuJHIL8Bvg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

然后取出对应的数据，也就是 **「B8、23、01」**，图中的 B8、BB 都是操作数。

![图片](https://mmbiz.qpic.cn/mmbiz_png/A3ibcic1Xe0iaT0AoicUdQic4AmqNbUhxmGibk3u7htgmVIeboOctGl5Yic92njD105Bt84omE3zJ47DxahV9G5LsRWxA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

控制输入/输出电路会将 B8 23 01 送入指令缓存器中。

![图片](https://mmbiz.qpic.cn/mmbiz_png/A3ibcic1Xe0iaT0AoicUdQic4AmqNbUhxmGibkOutFicGU7H8so5pGUDicHqtdKRznm1RZSBNodXaR565AMllbFAOvTECw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

此时这个指令就已经具备执行条件，此时 IP 也就是指令指针会自动增加。我们上面说到 IP 其实就是从 Code Segment 也就是 CS 处偏移的地址，也就是偏移地址。它会知道下一个需要读取指令的地址，如下图所示

![图片](https://mmbiz.qpic.cn/mmbiz_png/A3ibcic1Xe0iaT0AoicUdQic4AmqNbUhxmGibkNZm73zkbgibPrL1kdoYSUWbD6ichrmwIqUvribDvE5tHf2Foo3gC0doUw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

在这之后，指令执行执行取出的 B8 23 01 这条指令。

然后下面再把 2000 和 0003 送到地址加法器中再进行后续指令的读取。后面的指令读取过程和我们上面探讨的如出一辙，这里 cxuan 就不再赘述啦。

通过对上面的描述，我们能总结一下 8086 CPU 的工作过程

- 段寄存器提供段地址和偏移地址给地址加法器
- 由地址加法器计算出物理地址通过输入输出控制电路将物理地址送到内存中
- 提取物理地址对应的指令，经由控制电路取回并送到指令缓存器中
- IP 继续指向下一条指令的地址，同时指令执行器执行指令缓冲器中的指令

##### 什么是 Code Segment

Code Segment 即代码段，它就是我们上面聊到就是 CS 寄存器中存储的基础地址，也就是段地址，段地址其本质上就是一组内存单元的地址，例如上面的 **「mov ax,0123H 、mov bx, 0003H」**。我们可以将长度为 N 的一组代码，存放在一组连续地址、其实地址为 16 的倍数的内存单元中，我们可以认为，这段内存就是用来存放代码的。

##### DS 寄存器

CPU 在读写一个内存单元的时候，需要知道这个内存单元的地址。在 8086 CPU 中，有一个 `DS 寄存器`，通常用来存放访问数据的段地址。如果你想要读取一个 10000H 的数据，你可能会需要下面这段代码

```
mov bx,10000H
mov ds,bx
mov a1,[0]
```

上面这三条指令就把 10000H 读取到了 a1 中。

在上面汇编代码中，mov 指令有两种传送方式

- 一种是把数据直接送入寄存器
- 一种是将一个寄存器的内容送入另一个寄存器

但是不仅仅如此，mov 指令还具有下面这几种表达方式

| 描述                 | 举例             |
| :------------------- | :--------------- |
| mov 寄存器，数据     | 比如：mov ax,8   |
| mov 寄存器，寄存器   | 比如：mov ax,bx  |
| mov 寄存器，内存单元 | 比如：mov ax,[0] |
| mov 内存单元，寄存器 | 比如：mov[0], ax |
| mov 段寄存器，寄存器 | 比如：mov ds,ax  |

##### 栈

栈我相信大部分小伙伴已经非常熟悉了，`栈`是一种具有特殊的访问方式的存储空间。它的特殊性就在于，先进入栈的元素，最后才出去，也就是我们常说的 `先入后出`。

它就像一个大的收纳箱，你可以往里面放相同类型的东西，比如书，最先放进收纳箱的书在最下面，最后放进收纳箱的书在最上面，如果你想拿书的话， 必须从最上面开始取，否则是无法取出最下面的书籍的。

栈的数据结构就是这样，你把书籍压入收纳箱的操作叫做`压入（push）`，你把书籍从收纳箱取出的操作叫做`弹出（pop）`，它的模型图大概是这样

![图片](https://mmbiz.qpic.cn/mmbiz_png/A3ibcic1Xe0iaT0AoicUdQic4AmqNbUhxmGibkhOciaBWFxF3jqic6XXvF7t72k5ibiaj6XBaJbF3cqlkYl4VFN9iahYLh21Q/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

入栈相当于是增加操作，出栈相当于是删除操作，只不过叫法不一样。栈和内存不同，它不需要指定元素的地址。它的大概使用如下

```
// 压入数据
Push(123);
Push(456);
Push(789);

// 弹出数据
j = Pop();
k = Pop();
l = Pop();
```

在栈中，LIFO 方式表示栈的数组中所保存的最后面的数据（Last In）会被最先读取出来（First Out）。

![图片](https://mmbiz.qpic.cn/mmbiz_png/A3ibcic1Xe0iaT0AoicUdQic4AmqNbUhxmGibkGQrhvUJFIBbKNGGibRbOamZ61XCVbamgBlnfbTweur4oicu3kp9dUK5g/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

##### 栈和 SS 寄存器

下面我们就通过一段汇编代码来描述一下栈的压入弹出的过程

8086 CPU 提供入栈和出栈指令，最基本的两个是 `PUSH(入栈)` 和 `POP(出栈)`。比如 push ax 会把 ax 寄存器中的数据压入栈中，pop ax 表示从栈顶取出数据送入 ax 寄存器中。

> ❝
>
> 这里注意一点：8086 CPU 中的入栈和出栈都是以字为单位进行的。
>
> ❞

我这里首先有一个初始的栈，没有任何指令和数据。

![图片](https://mmbiz.qpic.cn/mmbiz_png/A3ibcic1Xe0iaT0AoicUdQic4AmqNbUhxmGibkUbzxxaXMVlORpYhp9xqxcBcKW0tEfxnVL1wH7wgvb6uGFOSf0MJ2kg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

然后我们向栈中 push 数据后，栈中数据如下

![图片](https://mmbiz.qpic.cn/mmbiz_png/A3ibcic1Xe0iaT0AoicUdQic4AmqNbUhxmGibklDXJYE78LkzG79FtU96Ztvmk0ehibqUkch7qjHRRbJXebvmunTK4EWw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

涉及的指令有

```
mov ax,2345H
push ax
```

> ❝
>
> 注意，数据会用两个单元存放，高地址单元存放高 8 位地址，低地址单元存放低 8 位。
>
> ❞

再向栈中 push 数据

![图片](https://mmbiz.qpic.cn/mmbiz_png/A3ibcic1Xe0iaT0AoicUdQic4AmqNbUhxmGibk45lRx2k87mJp3aH4quxWIs5IYiaYKATCd5KDdyE7voUiauwpA3ibMiamug/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

其中涉及的指令有

```
mov bx,0132H
push bx
```

现在栈中有两条数据，现在我们执行出栈操作

![图片](https://mmbiz.qpic.cn/mmbiz_png/A3ibcic1Xe0iaT0AoicUdQic4AmqNbUhxmGibkv7WDT4y5MibAClJm9PwlRkRzP5HiakKicicfEkWyICLKZIWwJIkQSzB05w/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

其中涉及的指令有

```
pop ax
/* ax = 0132H */
```

再继续取出数据

![图片](https://mmbiz.qpic.cn/mmbiz_png/A3ibcic1Xe0iaT0AoicUdQic4AmqNbUhxmGibkLphW220kXPgOr5E6nETKBryoJ2REiaNGicnDvxq9Eb5oI3FGm02a3I4g/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

涉及的指令有

```
pop bx
/* bx = */
```

完整的 push 和 pop 过程如下

![图片](https://mmbiz.qpic.cn/mmbiz_png/A3ibcic1Xe0iaT0AoicUdQic4AmqNbUhxmGibkb3tvydibEIdQpmX12s4Nnu9vnyzzqWseLTlERicW5WnoAruVhmLUTxyg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

现在 cxuan 问你一个问题，我们上面描述的是 10000H ~ 1000FH 这段空间来作为 push 和 pop 指令的存取单元。但是，你怎么知道这个栈单元就是 10000H ~ 1000FH 呢？也就是说，你如何选择指定的栈单元进行存取？

事实上，8086 CPU 有一组关于栈的寄存器 `SS` 和 `SP`。SS 是段寄存器，它存储的是栈的基础位置，也就是栈顶的位置，而 SP 是栈指针，它存储的是偏移地址。在任意时刻，`SS:SP` 都指向栈顶元素。push 和 pop 指令执行时，CPU 从 SS 和 SP 中得到栈顶的地址。

现在，我们可以完整的描述一下 push 和 pop 过程了，下面 cxuan 就给你推导一下这个过程。

![图片](https://mmbiz.qpic.cn/mmbiz_png/A3ibcic1Xe0iaT0AoicUdQic4AmqNbUhxmGibk61PEtIRjsakjk70qvSEyS0K8Z2cTVibeLKkbOOzaGCep6OIUsctd67g/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

上面这个过程主要涉及到的关键变化如下。

当使用 **「PUSH」** 指令向栈中压入 1 个字节单元时，SP = SP - 1；即栈顶元素会发生变化；

而当使用 **「PUSH」** 指令向栈中压入 2 个字节的字单元时，SP = SP – 2 ；即栈顶元素也要发生变化；

当使用 **「POP」** 指令从栈中弹出 1 个字节单元时， SP = SP + 1；即栈顶元素会发生变化；

当使用 **「POP」** 指令从栈中弹出 2 个字节单元的字单元时， SP = SP + 2 ；即栈顶元素会发生变化；

##### 栈顶越界问题

现在我们知道，8086 CPU 可以使用 SS 和 SP 指示栈顶的地址，并且提供 PUSH 和 POP 指令实现入栈和出栈，所以，你现在知道了如何能够找到栈顶位置，但是你如何能保证栈顶的位置不会越界呢？栈顶越界会产生什么影响呢？

比如如下是一个栈顶越界的示意图

![图片](https://mmbiz.qpic.cn/mmbiz_png/A3ibcic1Xe0iaT0AoicUdQic4AmqNbUhxmGibksZcnqHkwF5V99Te9ZG54GVgGgPWTX9IWFaQkYDtibCvyDmSsJS6VfFQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

第一开始，SS：SP 寄存器指向了栈顶，然后向栈空间 push 一定数量的元素后，SS:SP 位于栈空间顶部，此时再向栈空间内部 push 元素，就会出现栈顶越界问题。

栈顶越界是危险的，因为我们既然将一块区域空间安排为栈，那么在栈空间外部也可能存放了其他指令和数据，这些指令和数据有可能是其他程序的，所以如此操作会让计算机`懵逼`。

我们希望 8086 CPU 能自己解决问题，毕竟 8086 CPU 已经是个成熟的 CPU 了，要学会自己解决问题了。

![图片](https://mmbiz.qpic.cn/mmbiz_jpg/A3ibcic1Xe0iaT0AoicUdQic4AmqNbUhxmGibkExH7XDxibibREuIM2fic3AyZxVticIyC6E0JZ9iasLrIricELHdBmpLBeqicw/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

然鹅（故意的），这对于 8086 CPU 来说，这可能是它一辈子的 `夙愿` 了，真实情况是，8086 CPU 不会保证栈顶越界问题，也就是说 8086 CPU 只会告诉你栈顶在哪，并不会知道栈空间有多大，所以需要程序员自己手动去保证。





##  系统调用

### [系统调用是如何实现的](https://mp.weixin.qq.com/s?__biz=MzU2NTYyOTQ4OQ==&mid=2247483868&idx=1&sn=d85d5f4138b8e9f67031315074e0bfe1&chksm=fcb98682cbce0f947777b4f25b5a36506c615543f3d3d5356f839e708ffc30f1eb39361faae3&scene=178&cur_album_id=1433368223499796481#rd)

进程可以通过系统调用向操作系统发起请求，希望操作系统替自己去完成某项操作，但是**操作系统怎么完成取决于自己而且不受进程控制，进程也不知道操作系统是如何完成请求的，这是现代操作系统实现进程、多任务、虚拟内存等重要功能一个基本前提。**

因为操作系统是整个计算机系统的控制者(操作系统能访问整个内存地址空间，能够决定CPU分配给哪个进程，可以控制各种设备，比如磁盘，网卡，键盘，鼠标，等等)，如果某个进程能控制操作系统的话，那么这个进程就绕过了操作系统进而控制了整个计算机系统，这显然是不合理的(比如该进程一直使用CPU而不分配给其它进程)。

因此操作系统必须通过某种方法来限制进程。该怎么做到呢？

> 操作系统如何限制用户程序

我们知道操作系统其实也是一个大的C程序，本质上和我们写的C程序没有任何区别。操作系统和用户程序都需要被编译成机器指令才能被CPU执行。因此当CPU执行的是来自操作系统的机器指令时，计算机表现出来的就是操作系统正在运行(比如操作系统收发网络数据)。当CPU执行的是来自用户程序的机器指令时，计算机表现出来的就是用户程序正在运行(比如浏览器正在加载页面)。

从这里我们可以看出，单纯的依靠软件是没有办法来区分操作系统和用户程序的，因为这二者本质上都是机器指令，因此要想限制进程必须还要依靠硬件的帮助。

> 软件不够硬件来凑

原来CPU在执行机器指令时是有各个工作状态的，比如当CPU工作在A模式时会受到限制，不能执行所有的机器指令(比如I/O操作类指令，这类指令被称为特权指令)，只能访问部分的内存空间，但当切换到B工作模式下CPU满血复活，在该工作状态下，CPU可以执行所有指令类型包括特权指令，可以访问所有的内存地址空间，在这种工作状态下CPU可以解锁全部功能。CPU在A工作模式下被称为用户模式(User mode)，在B工作模式下被称为内核模式(Kernel mode)，CPU中有专门的寄存器用来记录当前CPU的工作状态。

**操作系统正是利用了CPU区分工作模式的功能来达到限制进程的目的的**。当CPU在执行用户程序的指令时，操作系统把CPU的工作模式设置为用户模式，在这种模式下，用户程序不能直接控制外部设备，不能直接发起I/O请求，不能访问超过自己范围的内存空间(被关在监牢中)等等。当用户程序需要进行I/O操作需要向操作系统发起请求时(监狱上的窗口)会进行系统调用，此时CPU开始执行一种特殊的机器指令，这种特殊指令是专门为实现系统调用而设计的。

在一般的CPU中有一种被称为trap的指令，执行trap指令后，CPU开始切换到内核模式执行操作系统指令。CPU会跳转到提前定义好的内存地址中，这里保存的是操作系统的代码，专门用来处理trap指令。接下来的过程就不受用户程序控制了，操作系统开始接管计算机，当操作系统接管计算机后，操作系统是信任自己的，因此操作系统可以控制所有计算机中的硬件(CPU、内存、磁盘、网卡等外设)，可以访问整个内存的地址空间，可以决定是否继续让某个进程使用CPU(控制进程)，可以向外部设备发起I/O命令。通过检查trap指令中请求类型，操作系统首先判断进程请求是否合法，如果请求合法，那么操作系统就替用户进程完成请求，把执行结果返回给进程，之后CPU再次切换回用户模式，在得到操作系统的返回结果后用户进程继续运行。

> 系统调用是如何实现的

在上一节中我们已经知道了，操作系统利用CPU不同的工作模式来限制用户进程。

在这种机制，用户进程完全无法控制操作系统如何来完成进程的请求。进程知道自己的代码段在哪里，自己的数据段在哪里，自己的函数调用栈在哪里，但是进程不知道操作系统的代码段在哪里，不知道操作系统的函数调用栈在哪里。进程也不能访问超出自己范围的内存空间(32位系统是用户进程可用的内存空间是3G)，而且一旦进程访问了超出自己范围的内存空间，CPU就能检测出来进程在用户模式下试图访问超过自己权限的内存地址（好比越狱），这有可能是程序员的bug也有可能是恶意病毒，CPU检测到进程切图越权后就会开始执行提前准备好的一段操作系统代码，这段代码专门用来处理进程越界访问，操作系统开始接管计算机，不管进程出于什么样的原因越界，操作系统都会无情的终结掉当前进程，这就是著名的**segmentation fault**。所以下次你再看到这个错误就应该明白，是你的程序企图越狱而被操作系统kill掉了。

**以trap指令为交界点，CPU执行该指令前处于用户模式，CPU执行该指令后处于内核模式**。

有了这些基础，系统调用就很简单啦，作为用户程序向操作系统发起请求的窗口，系统调用在程序员眼里就一个普通的C语言函数，只不过系统调用被编译成的机器指令时和普通函数的机器指令不太一样。系统调用被编译出来的机器指令中包含trap指令，CPU在执行到trap指令时就知道这是用户进程在向操作系统发起请求。

### [程序员应如何理解系统调用：上篇](https://mp.weixin.qq.com/s?__biz=MzU2NTYyOTQ4OQ==&mid=2247483880&idx=1&sn=26ab417ffdd46b2956e5dc07516477af&chksm=fcb986b6cbce0fa0e0959341ec9c7a0c2db0acd9f5a1250e5cbe33306da2f10f1f3cd08152aa&scene=178&cur_album_id=1433368223499796481#rd)

承接上文《[系统调用是如何实现的](http://mp.weixin.qq.com/s?__biz=MzU2NTYyOTQ4OQ==&mid=2247483868&idx=1&sn=d85d5f4138b8e9f67031315074e0bfe1&chksm=fcb98682cbce0f947777b4f25b5a36506c615543f3d3d5356f839e708ffc30f1eb39361faae3&scene=21#wechat_redirect)》

操作系统主要有两项功能：

- 向用户程序提供一个友好的编程接口，即系统调用
- 管理计算机资源(包括CPU、内存、磁盘、网卡等外设，以及进程管理，线程管理，文件管理等)

通常操作系统如何管理计算资源对于程序员来说是不可见的，应用程序想要使用系统资源必须通过操作系统，从这个角度讲操作系统更像是server，我们的应用程序是client，client只需要向server发出request然后得到response，至于server如何处理请求并不要client关心。同样的道理，应用程序只需要进行系统调用，而操作系统通过系统调用来屏蔽了处理细节。

作为程序员应该意识到，我们的程序在运行时，CPU不仅仅在执行我们的程序，当涉及到文件、网络、进程控制、线程控制、I/O等时，单单依靠我们的代码是没有办法来完成这些操作的，这些只能依靠操作系统，对于程序员来说就是调用系统调用。当系统调用开始执行时，CPU从用户模式切换到内存模式并开始运行操作系统的代码，即操作系统开始运行来完成上述用户程序请求。

虽然系统调用在程序员眼里仅仅是一个普通的函数调用，但是深刻理解系统调用对于理解操作系统的运行方式来说是非常重要的。简而言之，要想成为编程高手，你需要理解系统调用。

> API与系统调用

一般情况下，程序员不会直接使用系统调用进行编程，而是使用**对系统调用进行了封装的API来编程**，这个API在Unix/Linux下是libc来提供的，也就是我们熟悉的C标准库；在Windows下这个API叫做Win32 API，相信在Windows下编程的同学对此不会陌生。

**实际上作为程序员我们使用的基本上是使用C标准库或Win32 API进行编程，而这些API又封装了系统调用(所谓封装，也就是这些API最终会调用到系统调用)，这也是为什么很多程序员根本没有意识到自己的程序在进行系统调用的原因。**

你可能会想，为什么我们要使用C标准库或者Win32 API(以下统称API)而不直接使用系统调用来进行编程呢？

一方面是因为这些系统调用的接口不是很易用；另一方面比较重要的是，API的使用对程序员屏蔽了系统调用，也就是说我们的程序并不**直接**依赖系统调用，这一点是极为重要，因此这些API可以选择使用某个系统调用或者使用多个系统调用或者不使用系统调用。这就给API的设计带了了极大的灵活性。同时只要API的接口是不变的，那么如果两个不同的操作系统提供了同样的API，我们的程序就可以不加修改的在另一种操作系统上运行了，这是非常棒的一种设计。比如，如果Windows上实现了C标准库，那么我们在Unix/Linux上基于C标准库的程序就可以不加修改的在Windows上运行。

通常来说一个系统调用会对应一个API，但是反过来不一定正确，也就是说一个API中不一定会调用系统调用，比如我们使用的memcpy，这里面就没有调用任何系统调用。而且多个API可能会调用同一个系统调用，比如在Linux下我们进行内存分配释放常用的几个函数malloc()，calloc()，free()，这些函数实际上都是调用的一个叫做brk()的系统调用来完成的。

![图片](https://mmbiz.qpic.cn/mmbiz_png/8g3rwJPmya1oFfABWyDBHk6PV8MwiaX7uCN306zvTKWfuXewxPotSCHEGJBTUaje3zzGYhpofyBJ23JLzGLAicwA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

由于Windows是闭源的商业操作系统，因此Win32 API有很好的兼容性，很古老的Windows程序放到现在的Win10上依然可以运行的好好的。

但是对于Unix来说情况就不一样了，由于历史原因，现存有很多基于Unix的操作系统，但是又包含了自己的实现。因此为了方便在这些系统是进行软件开发，提出了POSIX标准，POSIX标准主要是用来统一各个Unix平台上的API而不是这些平台上的系统调用，这些Unix系统可以有不同的系统调用，但是对外的API要提供一个大家都认可的统一的格式，POSIX就是来规定这些格式的。有了POSIX标准，**基于该标准的编写的程序就可以运行在不同的Unix平台上了**。顺便说一下，虽然我们经常把Unix和Linux放在一起阐述，但是Linux是和Unix完全不同的一个操作系统，仅仅是Linux在设计哲学上借鉴了Unix，Linux上已经提供了符合POSIX规范的API。

一般来说这些API(Unix/Linux下的C标准库或者Windows下的Win32 API)已经足够大部分程序员使用了，因此作为程序员很少会遇到需要直接使用系统调用的情况。

接下来我们就来看看一个系统调用的完整过程。



>  系统调用的过程

在这里我们依然以我们熟悉的HelloWorld程序为例来说明，同时我们假定运行的操作系统是Linux(其它系统下这个过程依然适用)。在Linux下printf其实是C标准库中的函数，当调用printf时最终会调用到一个叫做write()的系统调用。

```c

include <stdio.h>

int main(){
   printf("Hello World."); //调用系统调用write
   return 0;
}
```



我们的HelloWorld程序在被加载到内存后，操作系统把CPU的程序计数器指向第一条HelloWorld程序指令所在的内存地址，这样我们的程序开始在用户模式下运行，由于printf仅仅是一个定义在C标准库中的普通的函数，因此当执行到该函数时CPU跳转到C标准库中去执行命令，此时CPU依然工作在用户模式下。由于C标准库中的printf函数最终会调用write系统调用，因此CPU最终会执行到trap命令，这时CPU开始由用户模式转变为内核模式，CPU跳转到提前定义好的内存地址开始执行操作系统的代码。就好比用户程序向操作系统喊了一句：“Hey，操作系统老兄，帮我执行一下你的write函数吧。”当操作系统在替用户完成任务后，依次返回到C标准库中的函数以及用户程序，最终我们的HelloWorld程序得以继续运行，如下图所示。

所有的系统调用都是按照这种过程完成的。

![图片](https://mmbiz.qpic.cn/mmbiz_png/8g3rwJPmya1oFfABWyDBHk6PV8MwiaX7usqtdOaozS8ORpLP9HM8cFkRnxNpVaRDD2iaQHuYhUxIQSsWiciaTCPAmg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

我们知道操作系统提供了很多功能，因此有很多系统调用，Windows中有上千个，Linux中较有几百个。不知道大家有没有注意到一点，那就是操作系统怎么知道要执行的是write系统调用呢？

原来这些系统调用都有一个唯一的编号，这样当CPU执行trap命令切换的内核模式后，操作系统就能通过系统调用编号知道需要执行什么样的函数啦。

因此你会看到，这里有个通过系统调用编号来查找具体处理函数的过程，这个过程在Linux下是由一个被称之为System Call Handler的函数来实现的，CPU在从用户模式切换到内核模式后，跳转的内存地址就是System Call Hander所在的位置。System Call Handler通过系统调用号找到具体的处理代码后开始调用这段C代码来处理用户程序的请求：如下图所示，从这里应该也能看出来，普通的函数在被调用CPU不会进行模式切换，普通的函数调用只在用户模式下就可以完成。

![图片](https://mmbiz.qpic.cn/mmbiz_png/8g3rwJPmya1oFfABWyDBHk6PV8MwiaX7uAPPgicvAwpad3wkejaaJQcSppBH5mAaa8JEdbibiavDjWTcpPJzibzUd0A/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)





后续内容将在《程序员应如何理解系统调用：下篇》中继续。

### [程序员应如何理解系统调用：下篇](https://mp.weixin.qq.com/s?__biz=MzU2NTYyOTQ4OQ==&mid=2247483891&idx=1&sn=a527f49994e43b9304f5f4f963c771c5&chksm=fcb986adcbce0fbbde17020a7009abbaf61ade3275ac17768d58b3e34965dd619e7c3c438508&scene=178&cur_album_id=1433368223499796481#rd)

承接上文《[程序员应如何理解系统调用：](http://mp.weixin.qq.com/s?__biz=MzU2NTYyOTQ4OQ==&mid=2247483880&idx=1&sn=26ab417ffdd46b2956e5dc07516477af&chksm=fcb986b6cbce0fa0e0959341ec9c7a0c2db0acd9f5a1250e5cbe33306da2f10f1f3cd08152aa&scene=21#wechat_redirect)[上篇](http://mp.weixin.qq.com/s?__biz=MzU2NTYyOTQ4OQ==&mid=2247483880&idx=1&sn=26ab417ffdd46b2956e5dc07516477af&chksm=fcb986b6cbce0fa0e0959341ec9c7a0c2db0acd9f5a1250e5cbe33306da2f10f1f3cd08152aa&scene=21#wechat_redirect)》

> 系统调用类型

由于一个系统的功能都是通过系统调用对外提供的，因此应用程序能够实现的最强大的功能不会超过系统调用提供给应用程序的能力。

根据类型系统调用大体可以划分为以下几类：进程控制：一个运行中的进程可以创建另外一个进程去完成某项工作，这样当前进程就有机会去处理自己感兴趣的事情，这类系统调用在Linux中是fork()，在Windows下CreateProcess()。当我们创建新的进程后，可能需要等待其运行完成，这时我们需要的系统调用是Linux下的wait()或者Windows下的WaitForSingleObject()。至于操作系统如何来管理进程将是后续章节中的重要内容。

文件管理：我们通常需要创建文件create()来持久化的保存信息，文件创建完毕后，通常在使用前我们要首先打开文件open()，然后才能进行读写read()、write()。文件使用完毕后，通常需要关闭文件close()。关于文件的这些操作都是通过系统调用来完成的。

设备管理：我们的程序在运行过程中需要使用很多系统资源才能完成任务，比如请求分配内存、访问磁盘、通过网卡收发网络数据等等，如果这些资源当前是可用的，那么我们的程序在得到这些资源后可以继续运行，否则只能暂停运行我们的程序直到这些资源可用为止。因此用户程序通常需要首先请求资源request()，使用完毕后释放资源release()。由于在Unix/Linux系统中将这些硬件资源抽象成了文件(file)，因此我们可以通过对文件的读写read()、write()就能实现操作设备的目的。

信息维护：通常情况下我们需要向操作系统请求一些只有操作系统才知道的信息，比如当前的日期date()，时间time()，或者操作系统的版本信息，当前可用内存大小，剩余磁盘大小等等。另一种比较有用的信息是程序在内存中的运行数据，通过dump()进程在内存中的数据以及进程中函数的调用信息trace()，我们就可以利用调试器(比如Linux下的gdb)来调试有问题的程序。

通信：我们的进程可能需要与其它进程通信才能完成某项功能，在这里通常有两种通信模型。一种是消息传递类比如常用的网络通信，在网络通信中我们通过读写socket来实现网络数据的接收recv()和发送send()，本地的进程之间通信会通过比如Linux下的pipe()这类系统调用来完成。另一种是共享内存。在后续章节中我们会讲解这些进程间通信模型。

你会发现其实这些类型基本上涵盖了操作系统的方方面面，**如果你能自己写代码实现这些系统调用，那么本质上你已经自己完成了一个操作系统。**

下图是一张Windows和Linux下这几类系统调用的示例，注意这里仅仅挑选出一部分进行说明，详细的系统调需要参考具体的操作系统。



![图片](https://mmbiz.qpic.cn/mmbiz_png/8g3rwJPmya18icIZF6AmHh8rLx1ex4mmO60IVibFTEa6wo6e2PYUY0tKnBBrW477jRwQeyvdhUUXoEcH2Ttdr16Q/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1 =150x150)



> 系统调用带来的好处

**释放了程序员生产力**

由于系统调用对程序员屏蔽了操作系统对计算机资源管理的细节，因此程序员在进行比如文件读写这样的操作时根本无需关心这些文件放在了磁盘中的什么位置上，这些文件是通过什么样的文件系统来管理的等。再者比如程序员需要进行网络通信，通过系统调用，程序员无需关心这些数据是如何被封装成TCP/IP能认识的协议数据的以及是如何通过驱动程序把数据从网卡上发送出去的。同时程序员也无需关心如何从网卡上接收数据、如何进行数据进行TCP/IP协议解析并把数据交给用户程序。程序员需要做的仅仅就是调用类似read(socket)，write(socket)这样的函数就可以进行网络数据收发就可以了，这些都极大的解放了程序员生产力。



**提高系统稳定性**

作为用户程序和操作系统之间的一个屏障，系统调用保护了操作系统不受用户程序的干扰。通过系统调用，操作系统可以对用户请求进行权限以及合法性检查，这就阻止了用户程序随意使用系统资源。同时作为用户程序向操作系统发起请求的唯一合法途径，系统调用起到了类似海关的作用，这些无疑提高了系统稳定性。



**多任务以及虚拟内存成为可能**

系统调用的使用，使得操作系统可以不受干扰的控制系统资源，操作系统可以自己决定如何分配这些资源。正是因为系统调用，用户程序完全不需要知道操作系统是如何完成请求的，这种对上层的屏蔽使得多任务，虚拟内存等功能得以实现。

如果用户程序可以绕过操作系统随意控制系统资源(CPU、内存、磁盘，网卡、外设等)，那么整体系统的稳定性将荡然无存。操作系统根本就没有办法实现很酷的多任务、虚拟内存等功能。试想如果我们的音乐播放程序可以一直控制着使用CPU，那么我们还怎么能一边写代码一边听音乐呢？在这种情况下只要播放音乐，那么其它程序都将不能运行，这显然不是广大程序员们希望的 :) 。



>  总结

在这一节中我们详细的讲述了系统调用的整个过程，作为程序员我们需要意识到是操作系统帮我们完成了对计算资源的管理，作为用户程序，我们需要做的仅仅是通过系统调用来使用操作系统赋予我们的能力。同时程序员一般不需要直接进行系统调用，而是使用更为简单易用的API(C标准库或Win32 API)就可以了。操作系统通过系统调用对用户程序屏蔽了底层细节，通过这种机制，操作系统实现了多任务、虚拟内存等重要功能，在后续的章节中我们会详细讲解这些功能是如何实现的。





#  CPU并行程序设计

## MPI并行





## OpenMP并行





## Pthread并行



```c
pthread_mutex_init(&m_mutexNetRecv, NULL);
pthread_create(&m_UDPReceive_Thread_ID, NULL, vUDPReceiveManager, (void*)&err);

```







# GPU并行程序设计



#   Linux/C进程与线程



## 进程

全文脉络思维导图如下：

![图片](https://mmbiz.qpic.cn/mmbiz_png/PocakShgoGGwkgiaicj3v8lMl3EEUzpb9x0wgXBMLYGsibCWVjiaCOn0fHO06Cfj4jYuOEPWAUrKHJDJID222B328A/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

###   进程与线程的简单解释

进程（Process）和线程（Thread）是操作系统的基本概念，但是它们比较抽象，不容易掌握。以下这个解释出自阮一峰老师的博客（http://www.ruanyifeng.com/blog/2013/04/processes_and_threads.html），虽然**「不是非常严谨，但是足够形象」**，看完之后能对进程和线程有个非常直观的印象，这样也方便理解后文。

① 计算机的核心是 CPU，它承担了所有的计算任务。它就像一座工厂，时刻在运行。

![图片](https://mmbiz.qpic.cn/mmbiz_png/PocakShgoGGwkgiaicj3v8lMl3EEUzpb9xicmG6b8LWelyBbHPCexMQAjyrxlmbicQQBPM9k85s9P6mgnMGB9TvXYw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

② **「假定工厂的电力有限，一次只能供给一个车间使用」**。也就是说，一个车间开工的时候，其他车间都必须停工。背后的含义就是，单个 CPU 一次只能运行一个任务。

③ 进程就好比工厂的车间，它代表 CPU 所能处理的单个任务。任一时刻，CPU 总是运行一个进程，其他进程处于非运行状态。

![图片](https://mmbiz.qpic.cn/mmbiz_png/PocakShgoGGwkgiaicj3v8lMl3EEUzpb9xb5m6WEticW2ryEoqicHCFrrUVHH0APZiblkhJ6Hd6x1EFROjBYCj7o3wg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

④ 一个车间里，可以有很多工人。他们协同完成一个任务。

![图片](https://mmbiz.qpic.cn/mmbiz_png/PocakShgoGGwkgiaicj3v8lMl3EEUzpb9xl0QhD4F48lsq1mVdK9DaAauozRLMg7cGUlu5RF4VrLDdB56YpPibCqA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

⑤ 线程就好比车间里的工人。一个进程可以包括多个线程。

![图片](https://mmbiz.qpic.cn/mmbiz_png/PocakShgoGGwkgiaicj3v8lMl3EEUzpb9xDCI3aZDibXyldQrAKJggsUF70KXKHHwfTjNdYRwrUd7LHJ63Hz8qTeA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

⑥ 车间的空间是工人们共享的，比如许多房间是每个工人都可以进出的。这象征一个进程的内存空间是共享的，每个线程都可以使用这些共享内存。

![图片](https://mmbiz.qpic.cn/mmbiz_png/PocakShgoGGwkgiaicj3v8lMl3EEUzpb9xSutVmJvVke0rq1H5Flb4CgdGtFMbnyAaYby26xRkyagAKjMdrcQibCA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

⑦ 可是，每间房间的大小不同，有些房间最多只能容纳一个人，比如厕所。里面有人的时候，其他人就不能进去了。这代表一个线程使用某些共享内存时，其他线程必须等它结束，才能使用这一块内存。

![图片](https://mmbiz.qpic.cn/mmbiz_png/PocakShgoGGwkgiaicj3v8lMl3EEUzpb9xQY2mZxz5UJXfBvtB8xznKa3d2SKM6S4WxthT1ZDlRotffdrHFkjdOQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

⑧ 一个防止他人进入的简单方法，就是门口加一把锁。先到的人锁上门，后到的人看到上锁，就在门口排队，等锁打开再进去。这就叫"互斥锁"（Mutual exclusion，缩写 Mutex），防止多个线程同时读写某一块内存区域。

⑨ 还有些房间，可以同时容纳 n 个人，比如厨房。也就是说，如果人数大于 n，多出来的人只能在外面等着。这好比某些内存区域，只能供给固定数目的线程使用。

⑩ 这时的解决方法，就是在门口挂 n 把钥匙。进去的人就取一把钥匙，出来时再把钥匙挂回原处。后到的人发现钥匙架空了，就知道必须在门口排队等着了。这种做法叫做 "信号量"（Semaphore），用来保证多个线程不会互相冲突。

![图片](https://mmbiz.qpic.cn/mmbiz_png/PocakShgoGGwkgiaicj3v8lMl3EEUzpb9xO1CTXibJdxQF8R90O0aSQXib71MS1xemPZedtB0lej1bgCN2CiaX6aNZg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

不难看出，互斥锁 Mutex 是信号量 semaphore 的一种特殊情况（n = 1时）。也就是说，完全可以用后者替代前者。但是，因为 Mutex 较为简单，且效率高，所以在必须保证资源独占的情况下，还是采用这种设计。

###  什么是进程

结合上文的简单解释，下面给出进程的科学定义：**「进程是程序在某个数据集合上的一次运行活动，也是操作系统进行资源分配和保护的基本单位」**。

通俗来说，**「进程就是程序的一次执行过程」**，程序是静态的，它作为系统中的一种资源是永远存在的。而进程是动态的，它是动态的产生，变化和消亡的，拥有其自己的生命周期。

举个例子：同时挂三个 QQ 号，它们就对应三个 QQ 进程，退出一个就会杀死一个对应的进程。但是，就算你把这三个 QQ 全都退出了，QQ 这个程序死亡了吗？显然没有。

进程不仅包含正在运行的程序实体，并且包括这个运行的程序中占据的所有系统资源，比如说 CPU、内存、网络资源等。很多小伙伴在回答进程的概念的时候，往往只会说它是一个运行的实体，而会忽略掉进程所占据的资源。比如说，同样一个程序，同一时刻被两次运行了，那么他们就是两个独立的进程。

###  进程的组成

进程主要由三个部分组成：

1）**「进程控制块 PCB」**。包含如下几个部分：

- 进程描述信息
- 进程控制和管理信息
- 资源分配清单
- CPU 相关信息

2）**「数据段」**。即进程运行过程中各种数据（比如程序中定义的变量）

3）**「程序段」**。就是程序的代码（指令序列）

举个例子：同时挂三个 QQ 号，会对应三个 QQ 进程，它们的 PCB、数据段各不相同，但程序段的内容都是相同的 （都是运行着相同的 QQ 程序）

![图片](https://mmbiz.qpic.cn/mmbiz_png/PocakShgoGGwkgiaicj3v8lMl3EEUzpb9xTwjPiaGDH9nXbyQpAhU3tD0ZPVgmsVUbwIFOBxLOibwIv366rhS4yhDA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

PCB 是提供给操作系统用的，而程序段、数据段是给进程自己用的。

> 进程控制块 PCB

每个进程有且仅有一个进程控制块（Process Control Block，PCB），或称进程描述符，它是**「进程存在的唯一标识」**，是**「操作系统用来记录和刻画进程状态及环境信息的数据结构」**，也是操作系统掌握进程的唯一资料结构和管理进程的主要依据。所以说 PCB 是提供给操作系统使用的。

通俗的解释：操作系统需要对各个进程进行管理，但凡管理时所需要的信息，都会被放在 PCB 中，**「PCB 是进程存在的唯一标志」**。创建进程和撤销进程等都是指对 PCB 的操作，当进程被创建时，操作系统为其创建 PCB，当进程结束时，会回收其 PCB。

一般来说，PCB 会包含如下四类信息：

1）**「进程描述信息」**：用来让操作系统区分各个进程

- 当进程被创建时，操作系统会为该进程分配一个唯一的、不重复的 “身份证号”— **「PID」**（ProcessID，进程 ID）
- 另外，进程描述信息还包含进程所属的用户 ID（**「UID」**）

2）**「进程控制和管理信息」**：记录进程的运行情况。比如 CPU 的使用时间、磁盘使用情况、网络流量使用情况等。

3）**「资源分配清单」**：记录给进程分配了哪些资源。比如分配了多少内存、正在使用哪些 I/O 设备、正在使用哪些文件等。

4）**「CPU 相关信息」**：进程在让出 CPU 时，必须保存该进程在 CPU 中的各种信息，比如各种寄存器的值。用于实现进程切换，确保这个进程再次运行的时候恢复 CPU 现场，从断点处继续执行。这就是所谓的**「保存现场信息」**。

![图片](https://mmbiz.qpic.cn/mmbiz_png/PocakShgoGGwkgiaicj3v8lMl3EEUzpb9xBfLA3GdgUB9j2Xxh1WQkYXgqTZ14EtGwavicMrfgU42NyRcUv1nEq5A/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

###  进程的状态

尽管每一个进程都是独立的实体，有其自己的 PCB 和内部状态，但是进程之间经常需要相互作用。一个进程的输出结果可能是另一个进程的输入。假设进程 A 的输入依赖进程 B 的输出，那么在进程 B 的输出结果没有出来之前，进程 A 就无法执行，它就会被阻塞。这就是进程的阻塞态。

经典的进程三态模型如下：

- **「运行态」**（running）：进程占有 CPU 正在运行。
- **「就绪态」**（ready）：进程具备运行条件，等待系统分配 CPU 以便运行。
- **「阻塞态」** / 等待态（wait）：进程不具备运行条件，正在等待某个事件的完成。

![图片](https://mmbiz.qpic.cn/mmbiz_png/PocakShgoGGwkgiaicj3v8lMl3EEUzpb9xiaoKuicdsjCCD5Mibu4Rqp4JWewDyiaAqJ4Hf5nKBbPIib71LVqia0tHbgMA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

上图中的时间片用完，可以这样理解：

进程是并发执行的嘛，宏观上在一段时间内能同时运行多个程序，但其实微观上是交替发生的。也就是说 CPU 一般不会让一个进程一次性执行完，为了保证所有进程可以得到公平调度，CPU 时间被划分为一段段的时间片，这些时间片再被轮流分配给各个进程。某个进程的时间片用完后这个进程就会进入就绪态，而其他被分配到时间片的进程就会进入运行态。这个处于就绪态的进程就需要等待进程调度程序的下一次调度，为其分配 CPU 时间片后才能再次恢复运行。

需要注意的是：**「阻塞态是由于缺少需要的资源从而由运行态转换而来，但是该资源不包括 CPU 时间片，缺少 CPU 时间片会从运行态转换为就绪态」**。

很多系统中都增加了新建态（new）和终止态（exit），形成**「五态模型」**：

- **「新建态」**（new）：进程正在被创建时的状态
- **「终止态」**（exit）：进程正在从系统中消失时的状态

![图片](https://mmbiz.qpic.cn/mmbiz_png/PocakShgoGGwkgiaicj3v8lMl3EEUzpb9xM83HUYMBulpXicIj1JFVuueKv5pA1gDQFOZ4gAzbBLhdpujNaic8QGlQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

从上图可以发现，**「只有就绪态和运行态可以相互转换，其它的都是单向转换」**。

这些不同状态的进程操作系统是如何进行管理的呢？上文说过，PCB 是提供给操作系统使用的，是操作系统管理进程的主要依据。没错，操作就是通过 PCB 来管理这些拥有不同状态的进程的。

进程的 PCB 会通过某种方式组织起来，一般来说，操作系统会把处于同一状态的所有进程的 PCB 链接在一起，这种数据结构就称为**「进程队列」**（Process Queue）。

### 进程控制

所谓进程控制就是对系统中的所有进程实施有效的管理，**「实现进程状态转换」**功能。包括创建进程、阻塞进程、唤醒进程、终止进程等，这些功能均由**「原语」**来实现，操作系统通过原语来完成进程原理，包括进程的同步和互斥、进程的通信和管理。

**「什么是原语」**？原语是一种特殊的程序，它的执行具有**「原子性」**。也就是说，这段程序的运行必须一气呵成，不可中断。原语是操作系统内核里的一段程序：

![图片](https://mmbiz.qpic.cn/mmbiz_png/PocakShgoGGwkgiaicj3v8lMl3EEUzpb9x3zWqAWibnicaXaj5btBEG4NUUs7RZB6o8fdrlgaq9fbUxmqR67YDADpA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

思考一下：为什么进程控制（进程状态转换）的过程要一气呵成，不可中断？

答：如果进程状态转换的过程不能一气呵成，就有可能导致操作系统中的某些关键数据结构信息不统一，这会影响操作系统进行别的管理工作。

#### 进程的创建

操作系统初始启动时会创建承担系统资源分配和控制管理的一些系统进程，同时还会创建一个所有用户进程的祖先，其他用户进程是在应用程序运行时创建的。

操作系统允许一个进程创建另一个进程，而且允许子进程继承父进程所拥有的资源，当子进程被终止时，其在父进程处继承的资源应当还给父进程。同时，终止父进程时同时也会终止其所有的子进程。

创建进程的过程，也就是**「创建原语」**包含的内容如下：

- 在进程列表中增加一项，从 PCB 池中申请一个空闲的 PCB（PCB 是有限的，若申请失败则创建失败），为新进程分配一个唯一的进程标识符；
- 为新进程分配地址空间，由进程管理程序确定加载至进程地址空间中的程序；
- 为新进程分配各种资源；
- 初始化 PCB，如进程标识符、CPU 初始状态等；
- 把新进程的状态设置为就绪态，并将其移入就绪队列，等待被调度运行。

**「什么事件会触发进程的创建呢」**？有如下四种情况：

- 用户登录：分时系统中，用户登录成功，系统会为其建立一个新的进程
- 作业调度：多道批处理系统中，有新的作业放入内存中，会为其建立一个新的进程
- 提供服务：用户向操作系统提出某些请求时，会新建一个进程处理该请求
- 应用请求：由用户进程主动请求创建一个子进程

#### 进程的终止

进程的终止也称为撤销，进程完成特定工作或出现严重错误后必须被终止。引起进程终止的事件有三种：

- 正常结束：进程自己请求终止（exit 系统调用）
- 异常结束：比如整数除 0，非法使用特权指令，然后被操作系统强行终止
- 外界干预：Ctrl + Alt + delete 打开进程管理器，用户手动杀死进程

终止（撤销）进程的过程，也就是**「撤销原语」**包含的内容如下：

- 从 PCB 集合中找到终止进程的 PCB；
- 若进程处于运行态，则立即剥夺其 CPU，终止该进程的执行，然后将 CPU 资源分配给其他进程；
- 如果其还有子进程，则应将其所有子进程终止；
- 将该进程所拥有的全部资源都归还给父进程或操作系统；
- 回收 PCB 并将其归还至 PCB 池。

#### 进程的阻塞和唤醒

进程阻塞是指进程让出 CPU 资源转而等待一个事件，如等待资源、等待 I/O 操作完成等。进程通常使用阻塞原语来阻塞自己，所以阻塞是进程的自主行为，是一个同步事件。当等待事件完成时会产生一个中断，激活操作系统，在系统的控制下将被阻塞的进程唤醒，也就是唤醒原语。

进程的阻塞和唤醒显然是由进程切换来完成的。

进程的阻塞步骤，也就是**「阻塞原语」**的内容为：

- 找到将要被阻塞的进程对应的 PCB；
- 保护进程运行现场，将 PCB 状态信息设置为阻塞态，暂时停止进程运行；
- 将该 PCB 插入相应事件的阻塞队列（等待队列）。

进程的唤醒步骤，也就是**「唤醒原语」**的内容为：

- 在该事件的阻塞队列中找到相应进程的 PCB；
- 将该 PCB 从阻塞队列中移出，并将进程的状态设置为就绪态；
- 把该 PCB 插入到就绪队列中，等待被调度程序调度。

阻塞原语和唤醒原语的作用正好相反，**「阻塞原语使得进程从运行态转为阻塞态，而唤醒原语使得进程从阻塞态转为就绪态」**。如果某个进程使用阻塞原语来阻塞自己，那么他就必须使用唤醒原语来唤醒自己，因何事阻塞，就由何事唤醒，否则被阻塞的进程将永远处于阻塞态。因此，**「阻塞原语和唤醒原语是成对出现的」**。

###   进程上下文切换

所谓进程的上下文切换，就是说各个进程之间是共享 CPU 资源的，不可能一个进程永远占用着 CPU 资源，不同的时候进程之间需要切换，使得不同的进程被分配 CPU 资源，这个过程就是进程的上下文切换，**「一个进程切换到另一个进程运行」**。

因为进程是由内核进行管理和调度的，所以**「进程的上下文切换一定发生在内核态」**。

进程上下文的切换也是一个原语操作，称为**「切换原语」**，其内容如下：

- 首先，将进程 A 的运行环境信息存入 PCB，这个运行环境信息就是进程的上下文（Context）
- 然后，将 PCB 移入相应的进程队列；
- 选择另一个进程 B 进行执行，并更新其 PCB 中的状态为运行态
- 当进程 A 被恢复运行的时候，根据它的 PCB 恢复进程 A 所需的运行环境

引起进程上下文切换的事件，也就是某个占用 CPU 资源运行的当前进程被赶出 CPU 的原因有如下：

- 当前进程的时间片到
- 有更高优先级的进程到达
- 当前进程主动阻塞
- 当前进程终止





###  [Linux 中进程管理系统调用](https://mp.weixin.qq.com/s/Yb2AhzXVt-waLtQL9hVXEg)

现在关注一下 Linux 系统中与进程管理相关的系统调用。在了解之前你需要先知道一下什么是系统调用。

操作系统为我们屏蔽了硬件和软件的差异，它的最主要功能就是为用户提供一种抽象，隐藏内部实现，让用户只关心在 GUI 图形界面下如何使用即可。操作系统可以分为两种模式

- 内核态：操作系统内核使用的模式
- 用户态：用户应用程序所使用的模式

我们常说的`上下文切换` 指的就是内核态模式和用户态模式的频繁切换。而`系统调用`指的就是引起内核态和用户态切换的一种方式，系统调用通常在后台静默运行，表示计算机程序向其操作系统内核请求服务。

系统调用指令有很多，下面是一些与进程管理相关的最主要的系统调用。

> fork

fork 调用用于创建一个与父进程相同的子进程，创建完进程后的子进程拥有和父进程一样的程序计数器、相同的 CPU 寄存器、相同的打开文件。



> exec

exec 系统调用用于执行驻留在活动进程中的文件，调用 exec 后，新的可执行文件会替换先前的可执行文件并获得执行。也就是说，调用 exec 后，会将旧文件或程序替换为新文件或执行，然后执行文件或程序。新的执行程序被加载到相同的执行空间中，因此进程的 `PID`不会修改，因为我们**没有创建新进程，只是替换旧进程**。但是进程的数据、代码、堆栈都已经被修改。如果当前要被替换的进程包含多个线程，那么所有的线程将被终止，新的进程映像被加载执行。

这里需要解释一下`进程映像(Process image)` 的概念

**什么是进程映像呢**？进程映像是执行程序时所需要的可执行文件，通常会包括下面这些东西

- **代码段（codesegment/textsegment）**

又称文本段，用来存放指令，运行代码的一块内存空间

此空间大小在代码运行前就已经确定

内存空间一般属于只读，某些架构的代码也允许可写

在代码段中，也有可能包含一些只读的常数变量，例如字符串常量等。

- **数据段（datasegment）**

可读可写

存储初始化的全局变量和初始化的 static 变量

数据段中数据的生存期是随程序持续性（随进程持续性） 随进程持续性：进程创建就存在，进程死亡就消失

- **bss 段（bsssegment）：**

可读可写

存储未初始化的全局变量和未初始化的 static 变量

bss 段中的数据一般默认为 0

- **Data 段**

是可读写的，因为变量的值可以在运行时更改。此段的大小也固定。

- **栈（stack）：**

可读可写

存储的是函数或代码中的局部变量(非 static 变量)

栈的生存期随代码块持续性，代码块运行就给你分配空间，代码块结束，就自动回收空间

- **堆（heap）：**

可读可写

存储的是程序运行期间动态分配的 malloc/realloc 的空间

堆的生存期随进程持续性，从 malloc/realloc 到 free 一直存在

下面是这些区域的构成图

![图片](https://mmbiz.qpic.cn/mmbiz_png/A3ibcic1Xe0iaRQzFQFGQcETRPzYXnxtWOtSDG2dd5po5BOWDpFCIL9qSsvY2P7ZjBppD97UWKy6ic1eFzjKBcfwZw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

exec 系统调用是一些函数的集合，这些函数是

- execl
- execle
- execlp
- execv
- execve
- execvp

下面来看一下 exec 的工作原理

1. 当前进程映像被替换为新的进程映像
2. 新的进程映像是你做为 exec 传递的参数
3. 结束当前正在运行的进程
4. 新的进程映像有 PID，相同的环境和一些文件描述符(因为未替换进程，只是替换了进程映像)
5. CPU 状态和虚拟内存受到影响，当前进程映像的虚拟内存映射被新进程映像的虚拟内存代替。

> waitpid

等待子进程结束或终止

> exit

在许多计算机操作系统上，计算机进程的终止是通过执行 `exit` 系统调用命令执行的。0 表示进程能够正常结束，其他值表示进程以非正常的行为结束。

其他一些常见的系统调用如下

| 系统调用指令 | 描述                             |
| :----------- | :------------------------------- |
| pause        | 挂起信号                         |
| nice         | 改变分时进程的优先级             |
| ptrace       | 进程跟踪                         |
| kill         | 向进程发送信号                   |
| pipe         | 创建管道                         |
| mkfifo       | 创建 fifo 的特殊文件（命名管道） |
| sigaction    | 设置对指定信号的处理方法         |
| msgctl       | 消息控制操作                     |
| semctl       | 信号量控制                       |





## 线程

现在我们来讨论一下 Linux 中的线程，线程是轻量级的进程，想必这句话你已经听过很多次了，`轻量级`体现在所有的进程切换都需要清除所有的表、进程间的共享信息也比较麻烦，一般来说通过管道或者共享内存，如果是 fork 函数后的父子进程则使用共享文件，然而线程切换不需要像进程一样具有昂贵的开销，而且线程通信起来也更方便。线程分为两种：用户级线程和内核级线程

### 用户级线程

用户级线程避免使用内核，通常，每个线程会显示调用开关，发送信号或者执行某种切换操作来放弃 CPU，同样，计时器可以强制进行开关，用户线程的切换速度通常比内核线程快很多。在用户级别实现线程会有一个问题，即单个线程可能会垄断 CPU 时间片，导致其他线程无法执行从而 `饿死`。如果执行一个 I/O 操作，那么 I/O 会阻塞，其他线程也无法运行。

![图片](https://mmbiz.qpic.cn/mmbiz_png/A3ibcic1Xe0iaRQzFQFGQcETRPzYXnxtWOtKyhzS8jyhliakaUUuNxeFg6grxWyNkgBZsOss4WhgcvhhACdtjzhiaiaQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

一种解决方案是，一些用户级的线程包解决了这个问题。可以使用时钟周期的监视器来控制第一时间时间片独占。然后，一些库通过特殊的包装来解决系统调用的 I/O 阻塞问题，或者可以为非阻塞 I/O 编写任务。

### 内核级线程

内核级线程通常使用几个进程表在内核中实现，每个任务都会对应一个进程表。在这种情况下，内核会在每个进程的时间片内调度每个线程。

![图片](https://mmbiz.qpic.cn/mmbiz_png/A3ibcic1Xe0iaRQzFQFGQcETRPzYXnxtWOt5BCuQ5qDL0nic412MZLJXr7y6j8MkG15Wictw6PEmMPOoMK5A2t345dQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

所有能够阻塞的调用都会通过系统调用的方式来实现，当一个线程阻塞时，内核可以进行选择，是运行在同一个进程中的另一个线程（如果有就绪线程的话）还是运行一个另一个进程中的线程。

从用户空间 -> 内核空间 -> 用户空间的开销比较大，但是线程初始化的时间损耗可以忽略不计。这种实现的好处是由时钟决定线程切换时间，因此不太可能将时间片与任务中的其他线程占用时间绑定到一起。同样，I/O 阻塞也不是问题。

### 混合实现

结合用户空间和内核空间的优点，设计人员采用了一种`内核级线程`的方式，然后将用户级线程与某些或者全部内核线程多路复用起来

![图片](https://mmbiz.qpic.cn/mmbiz_png/A3ibcic1Xe0iaRQzFQFGQcETRPzYXnxtWOtibHUBALDR6SFTzX0AXrPmTzzyOjjBVRclyMNuO2IIooUQGAjjWN32jQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

在这种模型中，编程人员可以自由控制用户线程和内核线程的数量，具有很大的灵活度。采用这种方法，内核只识别内核级线程，并对其进行调度。其中一些内核级线程会被多个用户级线程多路复用。







## Linux 进程间通信

Linux 进程间的通信机制通常被称为 `Internel-Process communication,IPC`下面我们来说一说 Linux 进程间通信的机制，大致来说，Linux 进程间的通信机制可以分为 6 种

![图片](https://mmbiz.qpic.cn/mmbiz_png/A3ibcic1Xe0iaRQzFQFGQcETRPzYXnxtWOtP5IhMLrNop6iaPNJrKWw6zF8Cia97FdkkpXicVibwVWQYGkNXN6ibwnAPgA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

下面我们分别对其进行概述

### <span id="信号2">信号 signal</span>

信号是 UNIX 系统最先开始使用的进程间通信机制，因为 Linux 是继承于 UNIX 的，所以 Linux 也支持信号机制，通过向一个或多个进程发送`异步事件信号`来实现，信号可以从键盘或者访问不存在的位置等地方产生；信号通过 shell 将任务发送给子进程。

你可以在 Linux 系统上输入 `kill -l` 来列出系统使用的信号，下面是我提供的一些信号

![图片](https://mmbiz.qpic.cn/mmbiz_png/A3ibcic1Xe0iaRQzFQFGQcETRPzYXnxtWOt3hAicRftdqkewydkbQhb4ZA8xZTTUZVS3dHp4EpYsg3Pia2gSvkvzWSQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

进程可以选择忽略发送过来的信号，但是有两个是不能忽略的：`SIGSTOP` 和 `SIGKILL` 信号。SIGSTOP 信号会通知当前正在运行的进程执行关闭操作，SIGKILL 信号会通知当前进程应该被杀死。除此之外，进程可以选择它想要处理的信号，进程也可以选择阻止信号，如果不阻止，可以选择自行处理，也可以选择进行内核处理。如果选择交给内核进行处理，那么就执行默认处理。

操作系统会中断目标程序的进程来向其发送信号、在任何非原子指令中，执行都可以中断，如果进程已经注册了新号处理程序，那么就执行进程，如果没有注册，将采用默认处理的方式。

例如：当进程收到 `SIGFPE` 浮点异常的信号后，默认操作是对其进行 `dump(转储)`和退出。信号没有优先级的说法。如果同时为某个进程产生了两个信号，则可以将它们呈现给进程或者以任意的顺序进行处理。

下面我们就来看一下这些信号是干什么用的

- SIGABRT 和 SIGIOT

SIGABRT 和 SIGIOT 信号发送给进程，告诉其进行终止，这个 信号通常在调用 C标准库的`abort()`函数时由进程本身启动

- SIGALRM 、 SIGVTALRM、SIGPROF

当设置的时钟功能超时时会将 SIGALRM 、 SIGVTALRM、SIGPROF 发送给进程。当实际时间或时钟时间超时时，发送 SIGALRM。当进程使用的 CPU 时间超时时，将发送 SIGVTALRM。当进程和系统代表进程使用的CPU 时间超时时，将发送 SIGPROF。

- SIGBUS

SIGBUS 将造成`总线中断`错误时发送给进程

- SIGCHLD

当子进程终止、被中断或者被中断恢复，将 SIGCHLD 发送给进程。此信号的一种常见用法是指示操作系统在子进程终止后清除其使用的资源。

- SIGCONT

SIGCONT 信号指示操作系统继续执行先前由 SIGSTOP 或 SIGTSTP 信号暂停的进程。该信号的一个重要用途是在 Unix shell 中的作业控制中。

- SIGFPE

SIGFPE 信号在执行错误的算术运算（例如除以零）时将被发送到进程。

![图片](https://mmbiz.qpic.cn/mmbiz_png/A3ibcic1Xe0iaRQzFQFGQcETRPzYXnxtWOtrvAYhGPeUVoHxmrxKAIhhcfc3uXX3hfXqRLBozEjyh01qdkkOqjySw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

- SIGUP

当 SIGUP 信号控制的终端关闭时，会发送给进程。许多守护程序将重新加载其配置文件并重新打开其日志文件，而不是在收到此信号时退出。

- SIGILL

SIGILL 信号在尝试执行非法、格式错误、未知或者特权指令时发出

- SIGINT

当用户希望中断进程时，操作系统会向进程发送 SIGINT 信号。用户输入 ctrl - c 就是希望中断进程。

- SIGKILL

SIGKILL 信号发送到进程以使其马上进行终止。与 SIGTERM 和 SIGINT 相比，这个信号无法捕获和忽略执行，并且进程在接收到此信号后无法执行任何清理操作，下面是一些例外情况

僵尸进程无法杀死，因为僵尸进程已经死了，它在等待父进程对其进行捕获

处于阻塞状态的进程只有再次唤醒后才会被 kill 掉

`init` 进程是 Linux 的初始化进程，这个进程会忽略任何信号。

SIGKILL 通常是作为最后杀死进程的信号、它通常作用于 SIGTERM 没有响应时发送给进程。

- SIGPIPE

SIGPIPE 尝试写入进程管道时发现管道未连接无法写入时发送到进程

- SIGPOLL

当在明确监视的文件描述符上发生事件时，将发送 SIGPOLL 信号。

- SIGRTMIN 至 SIGRTMAX

SIGRTMIN 至 SIGRTMAX 是`实时信号`

- SIGQUIT

当用户请求退出进程并执行核心转储时，SIGQUIT 信号将由其控制终端发送给进程。

- SIGSEGV

当 SIGSEGV 信号做出无效的虚拟内存引用或分段错误时，即在执行分段违规时，将其发送到进程。

- SIGSTOP

SIGSTOP 指示操作系统终止以便以后进行恢复时

- SIGSYS

当 SIGSYS 信号将错误参数传递给系统调用时，该信号将发送到进程。

- SIGTERM

我们上面简单提到过了 SIGTERM 这个名词，这个信号发送给进程以请求终止。与 SIGKILL 信号不同，该信号可以被过程捕获或忽略。这允许进程执行良好的终止，从而释放资源并在适当时保存状态。SIGINT 与SIGTERM 几乎相同。

- SIGTSIP

SIGTSTP 信号由其控制终端发送到进程，以请求终端停止。

- SIGTTIN 和 SIGTTOU

当 SIGTTIN 和SIGTTOU 信号分别在后台尝试从 tty 读取或写入时，信号将发送到该进程。

- SIGTRAP

在发生异常或者 trap 时，将 SIGTRAP 信号发送到进程

- SIGURG

当套接字具有可读取的紧急或带外数据时，将 SIGURG 信号发送到进程。

- SIGUSR1 和 SIGUSR2

SIGUSR1 和 SIGUSR2 信号被发送到进程以指示用户定义的条件。

- SIGXCPU

当 SIGXCPU 信号耗尽 CPU 的时间超过某个用户可设置的预定值时，将其发送到进程

- SIGXFSZ

当 SIGXFSZ 信号增长超过最大允许大小的文件时，该信号将发送到该进程。

- SIGWINCH

SIGWINCH 信号在其控制终端更改其大小（窗口更改）时发送给进程。

### 管道 pipe

Linux 系统中的进程可以通过建立管道 pipe 进行通信

![图片](https://mmbiz.qpic.cn/mmbiz_png/A3ibcic1Xe0iaRQzFQFGQcETRPzYXnxtWOtrHFf4S6ibmfyq2N85zFxdW8IgnkMxyF2cvblHC3Kbbe3a8BMpH7pyqQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

在两个进程之间，可以建立一个通道，一个进程向这个通道里写入字节流，另一个进程从这个管道中读取字节流。管道是同步的，当进程尝试从空管道读取数据时，该进程会被阻塞，直到有可用数据为止。shell 中的`管线 pipelines` 就是用管道实现的，当 shell 发现输出

```
sort <f | head
```

它会创建两个进程，一个是 sort，一个是 head，sort，会在这两个应用程序之间建立一个管道使得 sort 进程的标准输出作为 head 程序的标准输入。sort 进程产生的输出就不用写到文件中了，如果管道满了系统会停止 sort 以等待 head 读出数据

![图片](https://mmbiz.qpic.cn/mmbiz_png/A3ibcic1Xe0iaRQzFQFGQcETRPzYXnxtWOtaXHx7mIsrAmdKzIBwv3vUHbiaXDwoo0L0yuGwehiaficonL2aWTDz3nibQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

管道实际上就是 `|`，两个应用程序不知道有管道的存在，一切都是由 shell 管理和控制的。

### 共享内存 shared memory

两个进程之间还可以通过共享内存进行进程间通信，其中两个或者多个进程可以访问公共内存空间。两个进程的共享工作是通过共享内存完成的，一个进程所作的修改可以对另一个进程可见(很像线程间的通信)。

![图片](https://mmbiz.qpic.cn/mmbiz_png/A3ibcic1Xe0iaRQzFQFGQcETRPzYXnxtWOtzOrlYsXSJnzIld0U6fYckSY3YJXF3OY5LFdwx6S81iaficb7WfFcxN4A/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

在使用共享内存前，需要经过一系列的调用流程，流程如下

- 创建共享内存段或者使用已创建的共享内存段`(shmget())`
- 将进程附加到已经创建的内存段中`(shmat())`
- 从已连接的共享内存段分离进程`(shmdt())`
- 对共享内存段执行控制操作`(shmctl())`

### 先入先出队列 FIFO

先入先出队列 FIFO 通常被称为 `命名管道(Named Pipes)`，命名管道的工作方式与常规管道非常相似，但是确实有一些明显的区别。未命名的管道没有备份文件：操作系统负责维护内存中的缓冲区，用来将字节从写入器传输到读取器。一旦写入或者输出终止的话，缓冲区将被回收，传输的数据会丢失。相比之下，命名管道具有支持文件和独特 API ，命名管道在文件系统中作为设备的专用文件存在。当所有的进程通信完成后，命名管道将保留在文件系统中以备后用。命名管道具有严格的 FIFO 行为

![图片](https://mmbiz.qpic.cn/mmbiz_png/A3ibcic1Xe0iaRQzFQFGQcETRPzYXnxtWOtqgBofiaKl3ic9yxKX5EQOZibbqpIiaddu5sicGCShAxRJ9xJLyR6CGHbrQQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

写入的第一个字节是读取的第一个字节，写入的第二个字节是读取的第二个字节，依此类推。

### 消息队列 Message Queue

一听到消息队列这个名词你可能不知道是什么意思，消息队列是用来描述内核寻址空间内的内部链接列表。可以按几种不同的方式将消息按顺序发送到队列并从队列中检索消息。每个消息队列由 IPC 标识符唯一标识。消息队列有两种模式，一种是`严格模式`， 严格模式就像是 FIFO 先入先出队列似的，消息顺序发送，顺序读取。还有一种模式是 `非严格模式`，消息的顺序性不是非常重要。

### 套接字 Socket

还有一种管理两个进程间通信的是使用 `socket`，socket 提供端到端的双相通信。一个套接字可以与一个或多个进程关联。就像管道有命令管道和未命名管道一样，套接字也有两种模式，套接字一般用于两个进程之间的网络通信，网络套接字需要来自诸如`TCP（传输控制协议）`或较低级别`UDP（用户数据报协议）`等基础协议的支持。

套接字有以下几种分类

- `顺序包套接字(Sequential Packet Socket)`：此类套接字为最大长度固定的数据报提供可靠的连接。此连接是双向的并且是顺序的。
- `数据报套接字(Datagram Socket)`：数据包套接字支持双向数据流。数据包套接字接受消息的顺序与发送者可能不同。
- `流式套接字(Stream Socket)`：流套接字的工作方式类似于电话对话，提供双向可靠的数据流。
- `原始套接字(Raw Socket)`：可以使用原始套接字访问基础通信协议。



##   进程间同步方式







##   线程通信方式







##   线程间同步方式





# 函数与内存

## [申请内存时底层发生了什么？](https://mp.weixin.qq.com/s/DN-ckM1YrPMeicN7P9FvXg)

识不到的东西却无比重要，申请过这么多内存，**你知道申请内存时底层都发生什么了吗**？

我们就从神话故事开始吧。



> **三界**

中国古代的神话故事通常有“三界”之说，一般指的是天、地、人三界，天界是神仙所在的地方，凡人无法企及；人界说的是就是人间；地界说的是阎罗王所在的地方，孙悟空上天入地无所不能就是说可以在这三界自由出入。

有的同学可能会问，这和计算机有什么关系呢？

原来，我们的代码也是分三六九等的，程序运行起来后也是有“三界”之说的，程序运行起来的“三界”就是这样的：

![图片](https://mmbiz.qpic.cn/mmbiz_png/8g3rwJPmya0PwrdKmE9AkGwaqia6icELPUzGCKPsIQwtHU6atfP23MvxZmrbIjjntNGxAZ3IXOfmBMibnzLDJghkA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

x86 CPU提供了“四界”：0,1,2,3，**这几个数字其实就是指CPU的几种工作状态**，数字越小表示CPU的特权越大，0号状态下CPU特权最大，可以执行任何指令，数字越大表示CPU特权越小，3号状态下CPU特权最小，不能执行一些特权指令。

一般情况下系统只使用0和3，因此确切的说是“两界”，这两界可不是说天、地，这两界指的是“用户态(3)”以及“内核态(0)”，接下来我们看看什么是内核态、什么是用户态。



> **内核态**

什么是内核态？当CPU执行操作系统代码时就处于内核态，**在内核态下CPU可以执行任何机器指令、访问所有地址空间、不受限制的访问任何硬件**，可以简单的认为内核态就是“天界”，在这里的代码(操作系统代码)无所不能。

![图片](https://mmbiz.qpic.cn/mmbiz_png/8g3rwJPmya0PwrdKmE9AkGwaqia6icELPUBWWX8bkrGllWY1YGO0z0D0bHjiaicMQ3icO8RZbvl58f7bQUFpg3YMblA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)



> **用户态**

什么是用户态？当CPU执行我们写的“普通”代码(非操作系统、驱动程序员)时就处于用户态，粗糙的划分方法就是除了操作系统之外的代码，就像我们写的HelloWorld程序。

用户态就好比“人界”，在用户态我们的代码处处受限，不能直接访问硬件、不能访问特定地址空间，否则神仙(操作系统)直接将你kill掉，这就是著名的Segmentation fault、不能执行特权指令，等等。

![图片](https://mmbiz.qpic.cn/mmbiz_png/8g3rwJPmya0PwrdKmE9AkGwaqia6icELPUkKk9tz3fgSF9ejzVWnxicNrRcviaZYjtqWicwJ3GuMpSBM2oWniacxlueQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

关于这一部分的详细讲解，请参见《[深入理解操作系统](https://mp.weixin.qq.com/mp/appmsgalbum?__biz=MzU2NTYyOTQ4OQ==&action=getalbum&album_id=1433368223499796481#wechat_redirect)》系列文章。



> **跨界**

孙悟空神通广大，一个跟斗就能从人间跑到天上去骂玉帝老儿，程序员就没有这个本领了。普通程序永远也去不了内核态，只能以通信的方式从用户态往内核态传递信息。

操作系统为普通程序员留了一些特定的暗号，这些暗号就和普通函数一样，程序员通过调用这些暗号就能向操作系统请求服务了，这些像普通函数一样的暗号就被称为[**系统调用**](http://mp.weixin.qq.com/s?__biz=MzU2NTYyOTQ4OQ==&mid=2247483880&idx=1&sn=26ab417ffdd46b2956e5dc07516477af&chksm=fcb986b6cbce0fa0e0959341ec9c7a0c2db0acd9f5a1250e5cbe33306da2f10f1f3cd08152aa&scene=21#wechat_redirect)，System Call，通过系统调用我们可以让操作系统代替我们完成一些事情，像打开文件、网络通信等等。

![图片](https://mmbiz.qpic.cn/mmbiz_png/8g3rwJPmya0PwrdKmE9AkGwaqia6icELPU6nDiazC6U7A9gLAwKOYTscricaKKicESIbiaAnXia3tqCs0upAjsictJXpiaw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

你可能有些疑惑，什么，还有系统调用这种东西，为什么我没调用过也可以打开文件、进行网络通信？



> **标准库**

虽然我们可以通过系统让操作系统替我们完成一些特定任务，但这些系统调用都是和操作系统强相关的，Linux和Windows的系统调用就完全不同。

如果你直接使用系统调用的话，那么Linux版本的程序就没有办法在Windows上运行，因此我们需要某种标准，该标准对程序员屏蔽底层差异，这样程序员写的程序就无需修改的在不同操作系统上运行了。

在C语言中，这就是所谓的**标准库**。

注意，标准库代码也是运行在用户态的，并不是神仙(操作系统)，一般来说，我们调用标准库去打开文件、网络通信等等，标准库再根据操作系统选择对应的系统调用。

从分层的角度看，我们的程序一般都是这样的汉堡包类型：

![图片](https://mmbiz.qpic.cn/mmbiz_png/8g3rwJPmya0PwrdKmE9AkGwaqia6icELPU6CicgEJETyCvCkQiaF9yZ6wickG6bmKqbOv5Rq9CXlGoExibic9Aedcniaiag/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

最上层是应用程序，应用程序一般只和标准库打交道(当然，我们也可以绕过标准库)，标准库通过系统调用和操作系统交互，操作系统管理底层硬件。

**这就是为什么在C语言下同样的open函数既能在Linux下打开文件也能在Windows下打开文件的原因**。

说了这么多，这和malloc又有什么关系呢？



> **主角登场**

原来，我们分配内存时使用的malloc函数其实不是实现在操作系统里的，而是在标准库中实现的。

![图片](https://mmbiz.qpic.cn/mmbiz_png/8g3rwJPmya0PwrdKmE9AkGwaqia6icELPUCaWAXfdCrP8Q9LqJQTJtxplt150VY6eKvQR2MO3iaUefbcQPmMS7Bjw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

现在我们知道了，malloc是标准库的一部分，当我们调用malloc时实际上是标准库在为我们申请内存。

这里值得注意的是，我们平时在C语言中使用malloc只是内存分配器的一种，实际上有很多内存分配器，像tcmalloc，jemalloc等等，它们都有各自适用的场景，对于高性能程序来说使用满足特定要求的内存分配器是至关重要的。

那么接下来的问题就是malloc又是怎么工作的呢？



> **malloc是如何工作的**

实际上你可以把malloc的工作理解为去停车场找停车位，停车场就是一片malloc持有的内存，可用的停车位就是可供malloc支配的空闲内存，停在停车场占用的车位就是已经分配出去的内存，特殊点在于停在该停车场的车宽度大小不一，malloc需要回答这样一个问题：当有一辆车来到停车场后该停到哪里？

通过上面的类比你应该能大体理解工作原理了，具体分析详见《[自己动手实现一个malloc内存分配器](http://mp.weixin.qq.com/s?__biz=MzU2NTYyOTQ4OQ==&mid=2247485171&idx=1&sn=d93f2f5e9d61b00515c043776d2f7330&chksm=fcb981adcbce08bb39d120d7bfd097308371fb4b4e4369ba9502ae4e4243028b450bd0fe3110&scene=21#wechat_redirect)》。

但是，请注意，**上面这****篇文章并不是故事的全部**，在这篇文章中有一个问题我们故意忽略了，这个问题就是**如果内存分配器中的空闲内存块不够用了该怎么办呢**？

在上面这篇文章中我们总是假定自己实现的malloc总能找到一块空闲内存，但实际上并不是这样的。



> **内存不够该怎么办？**

让我们再来看一下程序在内存中是什么样的：

![图片](https://mmbiz.qpic.cn/mmbiz_png/8g3rwJPmya0PwrdKmE9AkGwaqia6icELPU7M3vQH0jVRDWx2bZPgic6ATaHiapb3iaK7bh4YicEQGhNXvavQToeVHb6w/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

我们已经知道了，malloc管理的是堆区，注意，在堆区和栈区之间有一片空白区域，这片空白区域的目的是什么呢？

原来，栈区其实是可以增长的，随着调用深度的增加，相应的栈区占用的内存也会增加，关于栈区这一主题，你可以参考《[函数运行时在内存中是什么样子](http://mp.weixin.qq.com/s?__biz=MzU2NTYyOTQ4OQ==&mid=2247484963&idx=1&sn=542d3bec57c6a9dfc17c83005fd2c030&chksm=fcb9817dcbce086b10cb44cad7c9777b0088fb8d9d6baf71ae36a9b03e1f8ef5bec62b79d6f7&scene=21#wechat_redirect)》这篇文章。

栈区的增长就需要占用原来的空白区域。

相应的，堆区也可以增长：

![图片](https://mmbiz.qpic.cn/mmbiz_png/8g3rwJPmya0PwrdKmE9AkGwaqia6icELPUibCxiaHf5fONfge0DMCHZgP3XXncRoV2Dwhiarrw3uJERoZEgTJicdRkow/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

堆区增长后占用的内存就会变多，这就解决了内存分配器空闲内存不足的问题，那么很自然的，malloc该怎样让堆区增长呢？

原来malloc内存不足时要向操作系统申请内存，**操作系统才是真大佬**，malloc不过是小弟，对每个进程，操作系统(类Unix系统)都维护了一个叫做brk的变量，brk发音break，这个brk指向了堆区的顶部。

![图片](https://mmbiz.qpic.cn/mmbiz_png/8g3rwJPmya0PwrdKmE9AkGwaqia6icELPUBaN7diaLZy5Z7kAkYHvU8pV4BcaRFMV8NmcbNRgrW040KEWVDKtolbw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

将brk上移后堆区增大，那么我们该怎么样让堆区增大呢？

这就涉及到我们刚提到的系统调用了。



> **向操作系统申请内存**

操作系统专门提供了一个叫做brk的系统调用，还记得刚提到堆的顶部吧，这个brk()系统调用就是用来增加或者减小堆区的。

![图片](https://mmbiz.qpic.cn/mmbiz_png/8g3rwJPmya0PwrdKmE9AkGwaqia6icELPUuibpNKII4KyVk4FDs8I5mQglzJDanibSZfbibxn9cbJ3gY7zG2vs9rD9A/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

实际上不只brk系统调用，sbr、mmap系统调用也可以实现同样的目的，mmap也更为灵活，但该函数并不是本文重点，就不在这里详细讨论了。

现在我们知道了，如果malloc自己维护的内存空间不足将通过brk系统调用向操作系统申请内存。这样malloc就可以把这些从操作系统申请到的内存当做新的空闲内存块分配出去。



> **看起来已经讲完的故事**

现在我就可以简单总结一下了，当我们申请内存时，经历这样几个步骤：

1. 程序调用malloc申请内存，注意malloc实现在标准库中
2. malloc开始搜索空闲内存块，如果能找到一块大小合适的就分配出去，前两个步骤都是发生在用户态
3. 如果malloc没有找到空闲内存块那么就像操作系统发出请求来增大堆区，这是通过系统调用brk(sbrk、mmap也可以)实现的，注意，brk是操作系统的一部分，因此当brk开始执行时，此时就进入内核态了。brk增大进程的堆区后返回，malloc的空闲内存块增加，此时malloc又一次能找到合适的空闲内存块然后分配出去。

![图片](https://mmbiz.qpic.cn/mmbiz_png/8g3rwJPmya0PwrdKmE9AkGwaqia6icELPUO3f3ztpm8WRXfblEAvHOhydv8Zr1o58usmryw9GrhbI6YeIT5MQz5g/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

故事就到这里了吗？



> **冰山之下**

实际上到目前为止，我们接触到的仅仅是冰山一角。

![图片](https://mmbiz.qpic.cn/mmbiz_jpg/8g3rwJPmya0PwrdKmE9AkGwaqia6icELPUOXGKA1bG5ojicgS3cEI0MMVp54qOH46KQHb3BgApHXpQmlrKwjYKnWg/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

我们看到的冰山是这样的：我们向malloc申请内存，malloc内存不够时向操作系统申请内存，之后malloc找到一块空闲内存返回给调用者。

但是，你知道吗，**上述过程根本就没有涉及到哪怕一丁点物理内存**！！！

我们确实向malloc申请到内存了，malloc不够也确实从操作系统申请到内存了，但这些内存都不是真的物理内存，**NOT REAL**。

实际上，进程看到的内存都是假的，是操作系统给进程的一个幻象，这个幻象就是由著名的**虚拟内存**系统来维护的，我们经常说的这张图就是进程的虚拟内存。

![图片](https://mmbiz.qpic.cn/mmbiz_png/8g3rwJPmya0PwrdKmE9AkGwaqia6icELPU7M3vQH0jVRDWx2bZPgic6ATaHiapb3iaK7bh4YicEQGhNXvavQToeVHb6w/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

所谓虚拟内存就是假的、不是真正的物理内存，虚拟内存是给进程用的，操作系统维护了虚拟内存到物理内存的映射，当malloc返回后，程序员申请到的内存就是虚拟内存。

注意，**此时操作系统根本就没有真正的分配物理内存，程序员从malloc拿到的内存目前还只是一张空头支票**。

那么这张空头支票什么时候才能兑现呢？也就是什么时候操作系统才会真正的分配物理内存呢？

答案是当我们真正使用这段内存时，当我们真正使用这段内存时，这时会产生一个缺页错误，操作系统捕捉到该错误后开始真正的分配物理内存，操作系统处理完该错误后我们的程序才能真正的读写这块内存。

这里只是简略的提到了虚拟内存，实际上虚拟内存是当前操作系统内部极其重要的一部分，关于虚拟内存的工作原理将在《[深入理解操作系统](https://mp.weixin.qq.com/mp/appmsgalbum?__biz=MzU2NTYyOTQ4OQ==&action=getalbum&album_id=1433368223499796481#wechat_redirect)》系列文章中详细讨论。



> **完整的故事**

现在，这个故事就可以完整讲出来了，当我们调用malloc申请内存时：

1. malloc开始搜索空闲内存块，如果能找到一块大小合适的就分配出去
2. 如果malloc找不到一块合适的空闲内存，那么调用brk等系统调用扩大堆区从而获得更多的空闲内存
3. malloc调用brk后开始转入内核态，此时操作系统中的虚拟内存系统开始工作，扩大进程的堆区，注意额外扩大的这一部分内存仅仅是虚拟内存，操作系统并没有为此分配真正的物理内存
4. brk执行结束后返回到malloc，从内核态切换到用户态，malloc找到一块合适的空闲内存后返回
5. 程序员拿到新申请的内存，程序继续
6. 当有代码读写新申请的内存时系统内部出现缺页中断，此时再次由用户态切换到内核态，操作系统此时真正的分配物理内存，之后再次由内核态切换回用户态，程序继续。

![图片](https://mmbiz.qpic.cn/mmbiz_png/8g3rwJPmya0PwrdKmE9AkGwaqia6icELPUSVyLUeFf3bAv8kJ6IBKpERibQZqqiaJSGx6jdKllImYIC9NnlpY3ic2Ww/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

以上就是一次内存申请的完整过程，可以看到一次内存申请过程是非常复杂的。



> **总结**

怎么样，程序员申请内存使用的malloc虽然表面看上去非常简单，简单到就一行代码，但这行代码背后是非常复杂的。

有的同学可能会问，为什么我们要理解这背后的原理呢？理解了原理后我才能知道内存申请的复杂性，对于高性能程序来讲频繁的调用malloc对系统性能是有影响的，那么很自然的一个问题就是我们能否避免malloc？

##   [函数与内存](https://mp.weixin.qq.com/s/fyrnqiK8ucGjmUxuHeakNQ)



在开始本篇的内容前，我们先来思考几个问题。

1. 我们先来看一段简单的代码：

   ```c
   void func(int a) {
     if (a > 100000000) return;

     int arr[100] = {0};
     func(a + 1);
   }
   ```

2. 我们在上一篇文章《[**高性能高并发服务器是如何实现的**](http://mp.weixin.qq.com/s?__biz=MzU2NTYyOTQ4OQ==&mid=2247484933&idx=1&sn=c4112a54f5751f38e841baf3e3cc35bd&chksm=fcb9815bcbce084de2823467d3ba9d3e835663a6bb69df1fc7f71677aef099584f93b0e01809&scene=21#wechat_redirect)》中提到了一项关键技术——协程，你知道协程的本质是什么吗？有的同学可能会说是用户态线程，那么什么是用户态线程，这是怎么实现的？

3. 函数运行起来后在内存中是什么样子？

这几个问题看似没什么关联，但这背后都指向一样东西，这就是所谓的函数**运行时栈**，**run time stack**。

接下来我们就好好看看到底什么是函数运行时栈，为什么彻底理解函数运行时栈对程序员来说非常重要。

> **从进程、线程到函数调用**

汽车在高速上行驶时有很多信息，像速度、位置等等，通过这些信息我们可以直观的感受汽车的运行时状态。

里等等，通过这些信息我们可以直观的感受系统中程序运行的状态。

其中，我们创造了进程、线程这样的概念来记录有哪些程序正在运行，关于进程和线程的概念请参见《[**看完这篇还不懂进程、线程与线程池你来打我**](http://mp.weixin.qq.com/s?__biz=MzU2NTYyOTQ4OQ==&mid=2247484768&idx=1&sn=049db350af9e5eea5cf3523ceb83f447&chksm=fcb9823ecbce0b28ca28d021e68c78138cde4a1b86ea7209c0c667d3d544d223d8b2aecbccec&scene=21#wechat_redirect)》。

**进程和线程的运行体现在函数执行上**，函数的执行除了函数内部执行的顺序执行还有子函数调用的控制转移以及子函数执行完毕的返回。其中函数内部的顺序执行乏善可陈，重点是函数的调用。

因此接下来我们的视角将从宏观的进程和线程拉近到微观下的函数调用，重点来讨论一下函数调用是怎样实现的。



>  **函数执行的活动轨迹：栈**

玩过游戏的同学应该知道，有时你为了完成一项主线任务不得不去打一些支线的任务，支线任务中可能还有支线任务，当一个支线任务完成后退回到前一个支线任务，这是什么意思呢，举个例子你就明白了。

假设主线任务西天取经A依赖支线任务收服孙悟空B和收服猪八戒C，也就是说收服孙悟空B和收服猪八戒C完成后才能继续主线任务西天取经A；

支线任务收服孙悟空B依赖任务拿到紧箍咒D，只有当任务D完成后才能回到任务B；

整个任务的依赖关系如图所示：

![图片](https://mmbiz.qpic.cn/mmbiz_png/8g3rwJPmya0WkCbTt37ejcAKNVsIu7UM6nPtWzlRuA1EU8Inc4cp30xQCyiaGTAvatAOSrdKYM9UnrkChzVHKibQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

现在我们来模拟一下任务完成过程。

首先我们来到任务A，执行主线任务：

![图片](https://mmbiz.qpic.cn/mmbiz_png/8g3rwJPmya0WkCbTt37ejcAKNVsIu7UMWEicBfDADgsdzibPLyJoFiaGANL5OBYmMGoMmOys7ic209SbiazzSXguhlA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

执行任务A的过程中我们发现任务A依赖任务B，这时我们暂停任务A去执行任务B：

![图片](https://mmbiz.qpic.cn/mmbiz_png/8g3rwJPmya0WkCbTt37ejcAKNVsIu7UMlaT1C1nZ4Nib1AlhibdOTia1lrYSGb054xd20IFjWKCdXcX0HwicTicdLOQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

执行任务B的时候，我们又发现依赖任务D：

![图片](https://mmbiz.qpic.cn/mmbiz_png/8g3rwJPmya0WkCbTt37ejcAKNVsIu7UM2WGO1bReDXjbu4ts90MNoFY9s5pgUThvA7EATPjianYdIliaBlQGXaoQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

执行任务D的时候我们发现该任务不再依赖任何其它任务，因此C完成后我们可以会退到前一个任务，也就是B：

![图片](https://mmbiz.qpic.cn/mmbiz_png/8g3rwJPmya0WkCbTt37ejcAKNVsIu7UMlaT1C1nZ4Nib1AlhibdOTia1lrYSGb054xd20IFjWKCdXcX0HwicTicdLOQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

任务B除了依赖任务C外不再依赖其它任务，这样任务B完成后就可以回到任务A：

![图片](https://mmbiz.qpic.cn/mmbiz_png/8g3rwJPmya0WkCbTt37ejcAKNVsIu7UMWEicBfDADgsdzibPLyJoFiaGANL5OBYmMGoMmOys7ic209SbiazzSXguhlA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

现在我们回到了主线任务A，依赖的任务B执行完成，接下来是任务C：

![图片](https://mmbiz.qpic.cn/mmbiz_png/8g3rwJPmya0WkCbTt37ejcAKNVsIu7UMPZ0rRnEdnBsDPD4RTY08To0AicbHRoK7WhzFvu6XvzPjMo0O0JKZl7Q/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

和任务D一样，C不依赖任何其它其它任务，任务C完成后就可以再次回到任务A，再之后任务A执行完毕，整个任务执行完成。

让我们来看一下整个任务的活动轨迹：

![图片](https://mmbiz.qpic.cn/mmbiz_png/8g3rwJPmya0WkCbTt37ejcAKNVsIu7UMibA8lEicYT8Lor9AylgiaHXwaSC2wPpOZL0Tib4o1vk6ib9MeMTn0wzdchg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

仔细观察，实际上你会发现这是一个First In Last Out 的顺序，天然适用于栈这种数据结构来处理。

再仔细看一下栈顶的轨迹，也就是A、B、D、B、A、C、A，**实际上你会发现这里的轨迹就是任务依赖树的遍历过程**，是不是很神奇，这也是为什么树这种数据结构的遍历除了可以用递归也可以用栈来实现的原因。



>  **A Box**

函数调用也是同样的道理，你把上面的ABCD换成函数ABCD，本质不变。

因此，现在我们知道了，使用栈这种结构就可以用来保存函数调用信息。

和游戏中的每个任务一样，当函数在运行时每个函数也要有自己的一个“小盒子”，**这个小盒子中保存了函数运行时的各种信息**，这些小盒子通过栈这种结构组织起来，这个小盒子就被称为栈帧，stack frames，也有的称之为call stack，不管用什么命名方式，总之，就是这里所说的小盒子，这个小盒子就是函数运行起来后占用的内存，**这些小盒子构成了我们通常所说的栈区**。关于栈区详细的讲解你可以参考《[**深入理解操作系统：程序员应如何理解内存**](http://mp.weixin.qq.com/s?__biz=MzU2NTYyOTQ4OQ==&mid=2247483839&idx=1&sn=20276b1966b76530890e9f09c6f41892&chksm=fcb986e1cbce0ff73c50f1a3f943bf182d8db666b27b4e55d20bd7cbbdea3fadbfbcb7c68f26&scene=21#wechat_redirect)》一文。

那么函数调用时都有哪些信息呢？



>  **控制转移**

我们知道当函数A调用函数B的时候，控制从A转移到了B，所谓控制其实就是指CPU执行属于哪个函数的机器指令，CPU从开始执行属于函数A的指令切换到执行属于函数B的指令，我们就说控制从函数A转移到了函数B。

控制从函数A转移到函数B，那么我们需要有这样两个信息：

- 我从哪里来 (返回)
- 要到去哪里 (跳转)

是不是很简单，就好比你出去旅游，你需要知道去哪里，还需要记住回家的路。

函数调用也是同样的道理。

当函数A调用函数B时，我们只要知道：

- 函数A对于的机器指令执行到了哪里 (我从哪里来，返回)
- 函数B第一条机器指令所在的地址 (要到哪里去，跳转)

有这两条信息就足以让CPU开始执行函数B对应的机器指令，当函数B执行完毕后跳转回函数A。

那么这些信息是怎么获取并保持的呢？

现在我们就可以打开这个小盒子，看看是怎么使用的了。

假设函数A调用函数B，如图所示：

![图片](https://mmbiz.qpic.cn/mmbiz_png/8g3rwJPmya0WkCbTt37ejcAKNVsIu7UMVjgxLV2L5Nn0WhGZAhge7RzdD01u5CzR7RpbictDlDrb3719cLbKfeA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

当前，CPU执行函数A的机器指令，该指令的地址为0x400564，接下来CPU将执行下一条机器指令也就是:

-

```c
call 0x400540
```



这条机器指令是什么意思呢？

这条机器指令对应的就是我们在代码中所写的函数调用，注意call后有一条机器指令地址，注意观察上图你会看到，**该地址就是函数B的第一条机器指令**，从这条机器指令后CPU将跳转到函数B。

现在我们已经解决了控制跳转的“要到哪里去”问题，当函数B执行完毕后怎么跳转回来呢？

原来，call指令除了给出跳转地址之外还有这样一个作用，也就是**把call指令的下一条指令的地址，也就是0x40056a push到函数A的栈帧中**，如图所示：

![图片](https://mmbiz.qpic.cn/mmbiz_png/8g3rwJPmya0WkCbTt37ejcAKNVsIu7UMQsNg63AfUVOusvQkTa19wTBdmIK2l2h2AggIiceYQ1zf8pTPlomRSrQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1 =100x80)

现在，函数A的小盒子变大了一些，因为装入了返回地址：

![图片](https://mmbiz.qpic.cn/mmbiz_png/8g3rwJPmya0WkCbTt37ejcAKNVsIu7UMLHu7EPtcboFPDiciaZ2ZJFtGjdDRIYY5icialsYTjAnu2uUMI396iaNRQ6Q/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

现在CPU开始执行函数B对应的机器指令，注意观察，函数B也有一个属于自己的小盒子(栈帧)，可以往里面扔一些必要的信息。

![图片](https://mmbiz.qpic.cn/mmbiz_png/8g3rwJPmya0WkCbTt37ejcAKNVsIu7UMEFHVVgibvBicAtmibj1s06Vr5M7ibWk4rJoxcoiclYeFXcfXsXibQNkf1E4w/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

如果函数B中又调用了其它函数呢？

道理和函数A调用函数B是一样的。

让我们来看一下函数B最后一条机器指令ret，这条机器指令的作用是告诉CPU跳转到函数A保存在栈帧上的返回地址，这样当函数B执行完毕后就可以跳转到函数A继续执行了。

至此，我们解决了控制转移中“我从哪里来”的问题。



>  **传递参数与获取返回值**

函数调用与返回使得我们可以编写函数，进行函数调用。但调用函数除了提供函数名称之外还需要传递参数以及获取返回值，那么这又是怎样实现的呢？

在x86-64中，多数情况下参数的传递与获取返回值是通过**寄存器**来实现的。

假设函数A调用了函数B，函数A将一些参数写入相应的寄存器，当CPU执行函数B时就可以从这些寄存器中获取参数了。

同样的，函数B也可以将返回值写入寄存器，当函数B执行结束后函数A从该寄存器中就可以读取到返回值了。

我们知道寄存器的数量是有限的，当传递的参数个数多于寄存器的数量该怎么办呢？

这时那个属于函数的小盒子也就是栈帧又能发挥作用了。

原来，当参数个数多于寄存器数量时剩下的参数直接放到栈帧中，这样被调函数就可以**从前一个函数的栈帧中获取到参数了**。

现在栈帧的样子又可以进一步丰富了，如图所示：

![图片](https://mmbiz.qpic.cn/mmbiz_png/8g3rwJPmya0WkCbTt37ejcAKNVsIu7UMzLPnnNzHXLsYF3VAXoAsBCpibA17YRdaXiamLnfibpv0vURiaIQicLCTZdg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

从图中我们可以看到，调用函数B时有部分参数放到了函数A的栈帧中，同时函数A栈帧的顶部依然保存的是返回地址。



> **局部变量**

我们知道在函数内部定义的变量被称为局部变量，这些变量在函数运行时被放在了哪里呢？

原来，这些变量同样可以放在寄存器中，但是当局部变量的数量超过寄存器的时候这些变量就必须放到栈帧中了。

因此，我们的栈帧内容又一步丰富了。

![图片](https://mmbiz.qpic.cn/mmbiz_png/8g3rwJPmya0WkCbTt37ejcAKNVsIu7UMM3Q3LQdcQLJdtGktOKxoxqZeRxAgMsqFHuthohdnBBtN2hWDmIicL9Q/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

细心的同学可能会有这样的疑问，我们知道寄存器是共享资源可以被所有函数使用，既然可以将函数A的局部变量写入寄存器，那么当函数A调用函数B时，函数B的局部变量也可以写到寄存器，这样的话当函数B执行完毕回到函数A时寄存器的值已经被函数B修改过了，这样会有问题吧。

这样的确会有问题，因此我们在向寄存器中写入局部变量之前，**一定要先将寄存器中开始的值保存起来**，当寄存器使用完毕后再恢复原值就可以了。

那么我们要将寄存器中的原始值保存在哪里呢？

有的同学可能已经猜到了，没错，依然是函数的栈帧中。

![图片](https://mmbiz.qpic.cn/mmbiz_png/8g3rwJPmya0WkCbTt37ejcAKNVsIu7UMib8974tib4sQXTWY9kywBtvHjFxTanphsQsUb6IprbevNBTGJuySplYQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

最终，我们的小盒子就变成了如图所示的样子，当寄存器使用完毕后根据栈帧中保存的初始值恢复其内容就可以了。

现在你应该知道函数在运行时到底是什么样子了吧，以上就是问题3的答案。



>  **Big Picture**

需要再次强调的一点就是，上述讨论的栈帧就位于我们常说的栈区。

栈区，属于进程地址空间的一部分，如图所示，我们将栈区放大就是图左边的样子。

![图片](https://mmbiz.qpic.cn/mmbiz_png/8g3rwJPmya0WkCbTt37ejcAKNVsIu7UMkbNXU3lnWgRWhJMmtkFnTFxvicAXCuQTAibfGMkM0h57bdOe3E9YzQ9A/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1 =80x80)

关于栈区详细的讲解你可以参考《[**深入理解操作系统：程序员应如何理解内存**](http://mp.weixin.qq.com/s?__biz=MzU2NTYyOTQ4OQ==&mid=2247483829&idx=1&sn=b9c32f56e95bdc229315e2b5ffd365cd&chksm=fcb986ebcbce0ffdc6c087775b895f6a0c0b83c72a91ee5614e253bea005509de8abfab6fc0e&scene=21#wechat_redirect)》这篇。

最后，让我们回到文章开始的这段简单代码：

```c
void func(int a) {
    if (a > 100000000) return;

    int arr[100] = {0};
    func(a + 1);
}

void main(){
    func(0);
}
```



想一想这段代码会有什么问题？

原来，**栈区是有大小限制的**，当超过限制后就会出现著名的**栈溢出**问题，显然上述代码会导致这一问题的出现。

因此：

1. 不要创建过大的局部变量
2. 函数栈帧，也就是调用层次不能太多



**总结**

本章我们从几个看似没什么关联的问题出发，详细讲解了函数运行时栈是怎么一回事，为什么我们不能创建过多的局部变量。细心的同学会发现第2个问题我们没有解答，这个问题的讲解放到下一篇，也就是协程中讲解。





##   [线程间共享了哪些进程资源？](https://mp.weixin.qq.com/s/5Xq-uzbjCExWfws-czVYGQ)

进程和线程这两个话题是程序员绕不开的，操作系统提供的这两个抽象概念实在是太重要了。

关于进程和线程有一个**极其经典**的问题，那就是进程和线程的区别是什么？相信很多同学对答案似懂非懂。



>  **记住了不一定真懂**

关于这个问题有的同学可能已经“背得”滚瓜烂熟了：“进程是操作系统分配资源的单位，线程是调度的基本单位，**线程之间共享进程资源**”。

可是你真的理解了上面最后一句话吗？**到底线程之间共享了哪些进程资源，共享资源意味着什么？共享资源这种机制是如何实现的？**对此如果你没有答案的话，那么这意味着**你几乎很难写出能正确工作的多线程程序**，同时也意味着这篇文章就是为你准备的。



>  **逆向思考**

查理芒格经常说这样一句话：“反过来想，总是反过来想”，如果你对线程之间共享了哪些进程资源这个问题想不清楚的话那么也可以反过来思考，那就是**有哪些资源是线程私有的**。



> **线程私有资源**

线程运行的本质其实就是函数的执行，函数的执行总会有一个源头，这个源头就是所谓的入口函数，CPU从入口函数开始执行从而形成一个执行流，只不过我们人为的给执行流起一个名字，这个名字就叫线程。

既然线程运行的本质就是函数的执行，那么函数执行都有哪些信息呢？

在《[函数运行时在内存中是什么样子](http://mp.weixin.qq.com/s?__biz=MzU2NTYyOTQ4OQ==&mid=2247484963&idx=1&sn=542d3bec57c6a9dfc17c83005fd2c030&chksm=fcb9817dcbce086b10cb44cad7c9777b0088fb8d9d6baf71ae36a9b03e1f8ef5bec62b79d6f7&scene=21#wechat_redirect)》这篇文章中我们说过，函数运行时的信息保存在栈帧中，栈帧中保存了函数的返回值、调用其它函数的参数、该函数使用的局部变量以及该函数使用的寄存器信息，如图所示，假设函数A调用函数B：

![图片](https://mmbiz.qpic.cn/mmbiz_png/8g3rwJPmya2PNptmXEntM7gPKCnknI3UNsZhzaGdjEAX7UrhQ9Zm6c0bC3tHYkahgiatFD7M8EYibTQDiaJx5LtJQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

此外，CPU执行指令的信息保存在一个叫做程序计数器的寄存器中，通过这个寄存器我们就知道接下来要执行哪一条指令。由于操作系统随时可以暂停线程的运行，因此我们保存以及恢复程序计数器中的值就能知道线程是从哪里暂停的以及该从哪里继续运行了。

由于线程运行的本质就是函数运行，函数运行时信息是保存在栈帧中的，因此每个线程都有自己独立的、私有的栈区。

![图片](https://mmbiz.qpic.cn/mmbiz_png/8g3rwJPmya2PNptmXEntM7gPKCnknI3UKECTocZz5RBaFsSicibSeKLsmkKcalTic01yJicf9Ig1JpVH7JEYicTaic8Q/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

同时函数运行时需要额外的寄存器来保存一些信息，像部分局部变量之类，这些寄存器也是线程私有的，**一个线程不可能访问到另一个线程的这类寄存器信息**。

从上面的讨论中我们知道，到目前为止，所属线程的栈区、程序计数器、栈指针以及函数运行使用的寄存器是线程私有的。

以上这些信息有一个统一的名字，就是**线程上下文**，thread context。

我们也说过操作系统调度线程需要随时中断线程的运行并且需要线程被暂停后可以继续运行，操作系统之所以能实现这一点，依靠的就是线程上下文信息。

现在你应该知道哪些是线程私有的了吧。

除此之外，剩下的都是线程间共享资源。

那么剩下的还有什么呢？还有图中的这些。

![图片](https://mmbiz.qpic.cn/mmbiz_png/8g3rwJPmya2PNptmXEntM7gPKCnknI3U76WYIzjJfrQiciaicFja35ywtqpX8AduKvLrFBQPR51lepO5ZAvoYcDmw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

这其实就是进程地址空间的样子，也就是说线程共享进程地址空间中除线程上下文信息中的所有内容，意思就是说线程可以**直接读取**这些内容。

接下来我们分别来看一下这些区域。

> **代码区**

进程地址空间中的代码区，这里保存的是什么呢？从名字中有的同学可能已经猜到了，没错，这里保存的就是我们写的代码，**更准确的是编译后的可执行机器指令**。

那么这些机器指令又是从哪里来的呢？答案是从可执行文件中加载到内存的，可执行程序中的代码区就是用来初始化进程地址空间中的代码区的。

![图片](https://mmbiz.qpic.cn/mmbiz_png/8g3rwJPmya2PNptmXEntM7gPKCnknI3UtCicgiaqiaFU5qrMibe2XPcGXM9A7x3J69Cl2ncgf6afCn3iaYZCM2Y2FYw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

线程之间共享代码区，**这就意味着程序中的任何一个函数都可以放到线程中去执行，不存在某个函数只能被特定线程执行的情况**。

> **数据区**

进程地址空间中的数据区，这里存放的就是所谓的全局变量。

什么是全局变量？所谓全局变量就是那些你定义在函数之外的变量，在C语言中就像这样：



```c
char c; // 全局变量

void func() {

}
```

其中字符c就是全局变量，存放在进程地址空间中的数据区。

![图片](https://mmbiz.qpic.cn/mmbiz_png/8g3rwJPmya2PNptmXEntM7gPKCnknI3UEtibNTe7gC9X9ett0gA8EcDU9TTg93CqGQKoqc4e81RG9p8KDYZwgSQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

在程序员运行期间，也就是run time，**数据区中的全局变量有且仅有一个实例，所有的线程都可以访问到该全局变量**。

值得注意的是，在C语言中还有一类特殊的“全局变量”，那就是用static关键词修饰过的变量，就像这样：

```c
void func(){
    static int a = 10;
}
```

注意到，**虽然变量a定义在函数内部，但变量a依然具有全局变量的特性**，也就是说变量a放在了进程地址空间的数据区域，**即使函数执行完后该变量依然存在**，而普通的局部变量随着函数调用结束和函数栈帧一起被回收掉了，但这里的变量a不会被回收，因为其被放到了数据区。

这样的变量对每个线程来说也是可见的，也就是说每个线程都可以访问到该变量。

> **堆区**

堆区是程序员比较熟悉的，我们在C/C++中用malloc或者new出来的数据就存放在这个区域，很显然，**只要知道变量的地址，也就是指针，任何一个线程都可以访问指针指向的数据**，因此堆区也是线程共享的属于进程的资源。

![图片](https://mmbiz.qpic.cn/mmbiz_png/8g3rwJPmya2PNptmXEntM7gPKCnknI3UY1zP3O4nm60RaE1iaRj5mHaWAPLDYyRVaxuflc3QFzSl8RYlh9eK5hg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

> **栈区**

唉，等等！刚不是说栈区是线程私有资源吗，怎么这会儿又说起栈区了？

确实，从线程这个抽象的概念上来说，栈区是线程私有的，然而从实际的实现上看，**栈区属于线程私有这一规则并没有严格遵守**，这句话是什么意思？

通常来说，注意这里的用词是**通常**，通常来说栈区是线程私有，既然有通常就有不通常的时候。

不通常是因为不像进程地址空间之间的严格隔离，线程的栈区没有严格的隔离机制来保护，因此如果一个线程能拿到来自另一个线程栈帧上的指针，**那么该线程就可以改变另一个线程的栈区**，也就是说这些线程可以任意修改本属于另一个线程栈区中的变量。

![图片](https://mmbiz.qpic.cn/mmbiz_png/8g3rwJPmya2PNptmXEntM7gPKCnknI3U6DwtO4rCcxRGcu5opoTjgUjHbE0SplTJ5lDPiaghICz30vwkuy89qZg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

这从某种程度上给了程序员极大的便利，但同时，这也会导致极其难以排查到的bug。

试想一下你的程序运行的好好的，结果某个时刻突然出问题，定位到出问题代码行后根本就排查不到原因，你当然是排查不到问题原因的，因为你的程序本来就没有任何问题，是别人的问题导致你的函数栈帧数据被写坏从而产生bug，这样的问题通常很难排查到原因，需要对整体的项目代码非常熟悉，常用的一些debug工具这时可能已经没有多大作用了。

说了这么多，那么同学可能会问，一个线程是怎样修改本属于其它线程的数据呢？

接下来我们用一个代码示例讲解一下。

> **修改线程私有数据**

不要担心，以下代码足够简单：

```c
void thread(void* var) {
    int* p = (int*)var;
    *p = 2;
}

int main() {
    int a = 1;
    pthread_t tid;

    pthread_create(&tid, NULL, thread, (void*)&a);
    return 0;
}
```

这段代码是什么意思呢？

首先我们在主线程的栈区定义了一个局部变量，也就是 int a= 1这行代码，现在我们已经知道了，局部变量a属于主线程私有数据，但是，接下来我们创建了另外一个线程。

在新创建的这个线程中，我们将变量a的地址以参数的形式传给了新创建的线程，然后我来看一下thread函数。

在新创建的线程中，我们获取到了变量a的指针，然后将其修改为了2，也就是这行代码，我们在新创建的线程中修改了本属于主线程的私有数据。

![图片](https://mmbiz.qpic.cn/mmbiz_png/8g3rwJPmya2PNptmXEntM7gPKCnknI3U7JUibLhQnz6ULEKEPrgjHbhzAicXCiaPKboRRMvxNtIAkFPoSWtXYnicNg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

现在你应该看明白了吧，尽管栈区是线程的私有数据，但由于栈区没有添加任何保护机制，一个线程的栈区对其它线程是可以见的，也就是说我们可以修改属于任何一个线程的栈区。

就像我们上文说得到的，这给程序员带来了极大便利的同时也带来了无尽的麻烦，试想上面这段代码，如果确实是项目需要那么这样写代码无可厚非，但如果上述新创建线程是因bug修改了属于其它线程的私有数据的话，那么产生问题就很难定位了，**因为bug可能距离问题暴露的这行代码已经很远了**，这样的问题通常难以排查。

> **动态链接库**

进程地址空间中除了以上讨论的这些实际上还有其它内容，还有什么呢？

这就要从可执行程序说起了。

什么是可执行程序呢？在Windows中就是我们熟悉的exe文件，在Linux世界中就是ELF文件，这些可以被操作系统直接运行的程序就是我们所说的可执行程序。

那么可执行程序是怎么来的呢？

有的同学可能会说，废话，不就是编译器生成的吗？

实际上这个答案只答对了一半。

假设我们的项目比较简单只有几个源码文件，编译器是怎么把这几个源代码文件转换为最终的一个可执行程序呢？

原来，编译器在将可执行程序翻译成机器指令后，接下来还有一个重要的步骤，这就是链接，链接完成后生成的才是可执行程序。

完成链接这一过程的就是链接器。

![图片](https://mmbiz.qpic.cn/mmbiz_png/8g3rwJPmya2PNptmXEntM7gPKCnknI3UwvIibKUkp3PExcAMtf1mugIcwGYR0BLzkT3RHxFCLMon1SHteXyM3Lg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

其中链接器可以有两种链接方式，这就是**静态链接**和**动态链接**。

静态链接的意思是说把所有的机器指令一股脑全部打包到可执行程序中，动态链接的意思是我们不把动态链接的部分打包到可执行程序，而是在可执行程序运行起来后去内存中找动态链接的那部分代码，这就是所谓的静态链接和动态链接。

动态链接一个显而易见的好处就是可执行程序的大小会很小，就像我们在Windows下看一个exe文件可能很小，**那么该exe很可能是动态链接的方式生成的**。

而动态链接的部分生成的库就是我们熟悉的动态链接库，在Windows下是以DLL结尾的文件，在Linux下是以so结尾的文件。

说了这么多，这和线程共享资源有什么关系呢？

原来如果一个程序是动态链接生成的，**那么其地址空间中有一部分包含的就是动态链接库**，否则程序就运行不起来了，这一部分的地址空间也是被所有线程所共享的。

![图片](https://mmbiz.qpic.cn/mmbiz_png/8g3rwJPmya2PNptmXEntM7gPKCnknI3UJtnywWE8NIFjwsGwH6UV0EldGXnZjQ7p2fKVDAv3YKCicGP2MpBZsXg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

也就是说进程中的所有线程都可以使用动态链接库中的代码。

以上其实是关于链接这一主题的极简介绍，关于链接这一话题的详细讨论可以参考《[彻底理解链接器](http://mp.weixin.qq.com/s?__biz=MzU2NTYyOTQ4OQ==&mid=2247483677&idx=1&sn=09212e8e7ecf7d58bfee53fa04e74911&chksm=fcb98643cbce0f5503916804ffe95bdd30917c9829429bb7fca7191b84f8171409db83e6e28c&scene=21#wechat_redirect)》系列文章。

> **文件**

最后，如果程序在运行过程中打开了一些文件，那么进程地址空间中还保存有打开的文件信息，进程打开的文件也可以被所有的线程使用，这也属于线程间的共享资源。

![图片](https://mmbiz.qpic.cn/mmbiz_png/8g3rwJPmya2PNptmXEntM7gPKCnknI3UuuPsC5SXGR6nAEHyIsHiajCBxqicGP8bvTgwC1UBBTXFe4T0p6uYQPbg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

> **One More Thing：TLS**

本文就这些了吗？

实际上关于线程私有数据还有一项没有详细讲解，因为再讲下去本篇就撑爆了，而且本篇已经讲解的部分足够用了，剩下的这一点仅仅作为补充，也就是选学部分，如果你对此不感兴趣的话完全可以跳过，没有问题![图片](https://mmbiz.qpic.cn/mmbiz_png/8g3rwJPmya3FQ7uPTlj2CkqH7qdNaDEsEn3N6lhOGzAE3PSVvkoJHcxDF6bXFEKfwjqdN33YsLliaGLKC9oAgxw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)。

关于线程私有数据还有一项技术，那就是线程局部存储，Thread Local Storage，TLS。

这是什么意思呢？

其实从名字上也可以看出，所谓线程局部存储，是指存放在该区域中的变量有两个含义：

- 存放在该区域中的变量是全局变量，所有线程都可以访问
- 虽然看上去所有线程访问的都是同一个变量，但该全局变量独属于一个线程，一个线程对此变量的修改对其他线程不可见。

说了这么多还是没懂有没有？没关系，接下来看完这两段代码还不懂你来打我。

我们先来看第一段代码，不用担心，这段代码非常非常的简单：

```c
int a = 1; // 全局变量

void print_a() {
    cout<<a<<endl;
}

void run() {
    ++a;
    print_a();
}

void main() {
    thread t1(run);
    t1.join();

    thread t2(run);
    t2.join();
}
```

怎么样，这段代码足够简单吧，上述代码是用C++11写的，我来讲解下这段代码是什么意思。

- 首先我们创建了一个全局变量a，初始值为1
- 其次我们创建了两个线程，每个线程对变量a加1
- 线程的join函数表示该线程运行完毕后才继续运行接下来的代码

那么这段代码的运行起来会打印什么呢？

全局变量a的初始值为1，第一个线程加1后a变为2，因此会打印2；第二个线程再次加1后a变为3，因此会打印3，让我们来看一下运行结果：

```c
2
3
```

看来我们分析的没错，全局变量在两个线程分别加1后最终变为3。

接下来我们对变量a的定义稍作修改，其它代码不做改动：

```
__thread int a = 1; // 线程局部存储
```

我们看到全局变量a前面加了一个__thread关键词用来修饰，也就是说我们告诉编译器把变量a放在线程局部存储中，那这会对程序带来哪些改变呢？

简单运行一下就知道了：



```c
2
2
```

和你想的一样吗？有的同学可能会大吃一惊，为什么我们明明对变量a加了两次，但第二次运行为什么还是打印2而不是3呢？

想一想这是为什么。

原来，这就是线程局部存储的作用所在，线程t1对变量a的修改不会影响到线程t2，线程t1在将变量a加到1后变为2，但对于线程t2来说此时变量a依然是1，因此加1后依然是2。

因此，**线程局部存储可以让你使用一个独属于线程的全局变量**。也就是说，虽然该变量可以被所有线程访问，但该变量在每个线程中都有一个副本，一个线程对改变量的修改不会影响到其它线程。

![图片](https://mmbiz.qpic.cn/mmbiz_png/8g3rwJPmya2PNptmXEntM7gPKCnknI3UvpTAlrZbo2MKIiaZhuMJWOJIcE7N1Ahy8HgMARSwBTGzAzDm9LtW1ibg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

> **总结**

怎么样，没想到教科书上一句简单的“线程共享进程资源”背后竟然会有这么多的知识点吧，**教科书上的知识看似容易，但，并不简单**。