
---

### **1.1 What are the three main purposes of an operating system?**
- 提供用户执行程序的环境（Provide an environment for program execution）
- 管理计算机硬件（Manage computer hardware）
- 作为用户和计算机硬件之间的中介（Act as an intermediary between the user and the hardware）

---

### **1.2 When is it appropriate for the operating system to forsake efficient use of computing hardware?**
当操作系统为了提高可用性、可靠性或用户体验而有意“浪费”资源时，是合理的。例如：
- 冗余存储（如RAID）以提高可靠性；
- 缓存机制虽然占用额外内存，但提高了性能；
- 图形用户界面（GUI）虽然消耗更多资源，但提供了更好的用户体验。

这种“浪费”实际上是为了更高层次的目标，因此并不是真正的浪费。

---

### **1.3 What is the main difficulty in writing an operating system for a real-time environment?**
主要困难是满足严格的**时间约束（time constraints）**。实时系统必须在规定时间内完成操作，否则可能导致系统失败（如机器人控制、医疗设备等）。这要求操作系统具有可预测的响应时间和高度可靠的调度机制。

---

### **1.4 Should the operating system include applications like web browsers and mail programs?**
**支持包括的观点：**
- 提供更完整的用户体验；
- 确保应用程序与系统更好地集成；
- 符合某些商业策略（如Microsoft Windows）。

**反对包括的观点：**
- 操作系统应专注于核心功能（如资源管理、进程调度）；
- 应用程序应独立于操作系统，以促进竞争和创新；
- 避免操作系统过于臃肿和复杂。

---

### **1.5 How does the distinction between kernel mode and user mode provide protection?**
- **用户模式（User Mode）**：限制用户程序直接访问硬件或执行特权指令。
- **内核模式（Kernel Mode）**：允许操作系统执行所有指令，包括特权指令。
- 通过**模式位（mode bit）** 硬件机制，确保用户程序不能直接执行可能破坏系统的操作，从而保护系统安全和稳定性。

---

### **1.6 Which instructions should be privileged?**
以下指令应为特权指令：
- a. Set value of timer.
- c. Clear memory.
- e. Turn off interrupts.
- f. Modify entries in device-status table.
- h. Access I/O device.

这些指令若由用户程序直接执行，可能破坏系统完整性或安全性。

---

### **1.7 Two difficulties with placing the OS in a read-only memory partition:**
1. 无法更新操作系统（如修复漏洞、添加新功能）；
2. 无法动态适应硬件变化（如新设备驱动无法加载）。

---

### **1.8 Two uses of multiple modes of operation:**
1. 提供不同级别的权限（如虚拟化管理器模式）；
2. 支持更细粒度的安全策略（如ARM的TrustZone）。

---

### **1.9 How can timers be used to compute the current time?**
操作系统可以维护一个计数器，每隔固定时间（如每次时钟中断）递增。通过记录系统启动以来的时钟中断次数，乘以每个中断的时间间隔，即可计算出当前时间。

---

### **1.10 Why are caches useful? What problems do they solve? What problems do they cause?**
- **用处**：提高数据访问速度，减少对慢速存储设备的访问。
- **解决的问题**：缓解CPU与主存之间的速度差异（速度不匹配）。
- **引起的问题**：
  - 一致性（Coherence）问题（多处理器环境中）；
  - 复杂度增加；
  - 成本较高。

即使缓存可以做得和磁盘一样大，也不能完全替代磁盘，因为：
- 缓存是易失性的（断电数据丢失）；
- 磁盘容量更大、成本更低。

---

### **1.11 Distinguish between client–server and peer-to-peer models.**
- **Client-Server**：有明确的客户端和服务端角色，服务集中管理。
- **Peer-to-Peer**：所有节点既是客户端也是服务端，没有中心节点，更去中心化。

---


### **1.12 How do clustered systems differ from multiprocessor systems? What is required for two machines to cooperate?**
- **区别**：
  - **多处理器系统 (Multiprocessor Systems)**：多个处理器（或核心）共享同一物理内存和系统总线，紧密耦合，形成一个单一的计算系统（如SMP, NUMA）。
  - **集群系统 (Clustered Systems)**：由两个或多个独立的、通常是完整的计算机系统（节点）通过网络连接而成，是**松散耦合 (loosely coupled)** 的。每个节点都有自己的操作系统和内存。
- **协作提供高可用性服务的要求**：集群节点必须共享存储（通常通过SAN），并运行集群软件来监控彼此的状态。如果一个节点失效，监控节点能够接管其存储并重启其应用程序。

---

### **1.13 Describe two ways for cluster software to manage disk access and discuss benefits/disadvantages.**
1.  **非对称集群 (Asymmetric Clustering)**：
    -   **方式**：一台机器处于**热备份模式 (hot-standby mode)**，只监控活动服务器。如果活动服务器失效，备份服务器接管。
    -   **优点**：简单。
    -   **缺点**：备份服务器在平时是闲置的，资源利用率低。

2.  **对称集群 (Symmetric Clustering)**：
    -   **方式**：两台或多台主机都运行应用程序，并互相监控。
    -   **优点**：所有硬件都被使用，效率高。
    -   **缺点**：更复杂，需要多个应用程序实例可用。

---

### **1.14 Purpose of interrupts? How does an interrupt differ from a trap? Can traps be generated intentionally?**
-   **中断的目的**：通知CPU需要立即处理某个事件（如I/O完成、硬件错误），实现CPU和设备的并行工作，并确保操作系统能接管控制权。
-   **中断与陷阱的区别**：
    -   **中断 (Interrupt)**：由**硬件设备**发出，是**异步**的（可以在指令执行的任何时刻发生）。
    -   **陷阱 (Trap)**：由**软件**（即正在执行的程序）产生，是**同步**的（由特定指令执行导致，如除零错误、系统调用请求）。
-   **陷阱可否故意生成**：**可以**。用户程序可以通过执行特定的指令（如系统调用`syscall`）来**故意生成陷阱**，目的是**请求操作系统为其提供服务**。

---

### **1.15 Explain how Linux kernel variables HZ and jiffies can be used to determine system uptime.**
-   `HZ`：内核配置参数，定义了每秒的定时器中断次数。
-   `jiffies`：内核变量，记录了系统启动以来发生的定时器中断总次数。
-   **计算运行时间**：系统运行时间（秒） = `jiffies / HZ`。

---

### **1.16 Direct memory access (DMA)**
a.  **CPU如何协调传输**：CPU通过设置DMA控制器的寄存器（如内存起始地址、传输字节数、设备地址）来启动传输，然后即可处理其他任务。
b.  **CPU如何知道操作完成**：DMA控制器在完成整个数据块的传输后，会向CPU发出一个**中断**信号。
c.  **是否会干扰用户程序**：**会**。DMA控制器和CPU**共享内存和系统总线**。当DMA控制器使用总线进行数据传输时，CPU对总线的访问会被暂时阻止（**cycle stealing**），这可能会**轻微减慢**用户程序的执行速度。

---

### **1.17 Is it possible to construct a secure OS without hardware privileged mode?**
-   **不可能**：没有硬件支持的特权模式，用户程序就可以执行任何指令，包括直接操作I/O设备、修改内存管理单元设置等，操作系统无法阻止恶意或错误程序破坏系统。硬件特权模式是实现**隔离和保护**的根本基础。
-   **可能（但极其困难且低效）**：理论上可以通过软件解释执行每一条指令来模拟特权模式，检查其合法性。但这会带来巨大的性能开销，并且其自身的安全性也难以保证。

---

### **1.18 Why do SMP systems have multiple levels of caches?**
这种设计是为了在**速度**和**容量/数据共享**之间取得平衡：
-   **每个核心私有的L1/L2缓存**：速度极快，减少了核心访问常用数据的延迟，避免了与其他核心争用缓存。
-   **所有核心共享的L3缓存**：容量更大，用于存储可能被多个核心共享的数据，减少了访问主内存的需要。它促进了核心间的数据共享和一致性。

---

### **1.19 Rank storage systems from slowest to fastest:**
从慢到快：
f. Magnetic tapes (磁带)
c. Optical disk (光盘)
a. Hard-disk drives (HDD, 硬盘)
e. Nonvolatile memory (NVM, 非易失性内存，如SSD)
d. Main memory (DRAM, 主存)
g. Cache (SRAM, 缓存)
b. Registers (寄存器)

---

### **1.20 Illustrate how cached data in an SMP could have different values.**
假设一个双核SMP系统，主内存中变量`X=5`。
1.  Core0 读取 `X`，在其本地缓存中存储 `X=5`。
2.  Core1 也读取 `X`，在其本地缓存中存储 `X=5`。
3.  Core0 将 `X` 改为 `10`，并写回其本地缓存。如果系统采用**写回（write-back）** 策略，这个新值可能尚未写回主存。
4.  此时，Core0的缓存中 `X=10`，Core1的缓存中仍然是 `X=5`，主内存中可能还是 `X=5`。数据**不一致**。

---

### **1.21 Discuss cache coherence in different environments:**
a.  **单处理器系统**：不存在多缓存问题， coherence问题不突出。主要问题是确保缓存与主存的数据一致性（通过写直通/写回策略）。
b.  **多处理器系统**：是**缓存一致性**问题的核心场景（如上题1.20所示）。需要硬件协议（如MESI协议）来维护所有缓存中数据副本的一致性。
c.  **分布式系统**：数据可能在不同计算机的主存中都有副本（而不仅仅是缓存）。保持这些副本一致性的问题称为**复制一致性（replication consistency）**，通常通过软件协议（如分布式锁管理器）来实现，比硬件缓存一致性协议更复杂、更慢。

---

### **1.22 Describe a mechanism for enforcing memory protection.**
使用**基址-界限寄存器（Base and Limit Registers）**：
-   为每个进程分配一个连续的内存区域。
-   **基址寄存器**存放该进程物理内存的起始地址。
-   **界限寄存器**存放该进程内存区域的大小（或结束地址）。
-   CPU生成的每一个内存地址都会由内存管理单元（MMU）自动检查：必须满足 `基址 <= 物理地址 < (基址 + 界限)`。如果越界，则触发一个陷阱（保护错误），交操作系统处理。

---

### **1.23 Which network configuration for these environments?**
a.  **A campus student union**：**LAN (局域网)**。覆盖一个建筑物或园区。
b.  **Several campus locations across a state**：**WAN (广域网)**。覆盖地理范围广泛的多个地点。
c.  **A neighborhood**：**LAN (局域网)** 或 **PAN (个人区域网，如Wi-Fi)**。覆盖范围较小。

---

### **1.24 Challenges designing OS for mobile devices vs. traditional PCs.**
-   **资源严格受限**：更少的内存、存储空间和处理器能力。
-   **功耗限制**：必须极其注重节能，采用复杂的电源管理策略。
-   **不同的用户交互模式**：主要依赖触摸屏、语音、传感器（GPS，加速度计），而非键盘鼠标。
-   **连接性**：需要更好地管理不稳定、多变的网络连接（蜂窝网络、Wi-Fi、蓝牙）。
-   **安全模型**：应用沙盒（sandboxing）和应用商店审核模型更为普遍。

---

### **1.25 Advantages of P2P over client-server systems.**
-   **可扩展性 (Scalability)**：资源和服务分布在众多节点上，避免服务器成为瓶颈。更多节点加入通常会增加系统的整体能力和容量。
-   **健壮性/可靠性 (Robustness)**：没有单点故障。如果一个节点失效，其他节点可以接管其功能。
-   **成本**：可以利用边缘节点的资源，减少对昂贵中心服务器的依赖。

---

### **1.26 Describe distributed applications appropriate for P2P.**
-   文件共享系统（如早期的Napster, Gnutella，或BitTorrent）。
-   语音和视频通信（如Skype）。
-   区块链和加密货币网络（如Bitcoin, Ethereum）。
-   分布式计算项目（如SETI@home，利用空闲CPU周期分析天文数据）。

---

### **1.27 Advantages and disadvantages of open-source OS.**
-   **优点**：
    -   **可审计性**：任何人都可以检查代码的安全性、是否存在后门。
    -   **灵活性/可定制性**：可以根据特定需求修改和定制系统。
    -   **社区支持**：有庞大的开发者社区贡献代码、修复漏洞、提供支持。
    -   **低成本**：通常无需许可费用。
    -   **避免供应商锁定**。
-   **缺点**：
    -   **支持**：可能缺乏官方、统一的技术支持（尽管有社区和商业公司提供）。
    -   **易用性**：某些发行版可能对新手不够友好。
    -   **碎片化**（对Linux而言）：众多发行版可能导致软件兼容性问题。
    -   **商业软件兼容性**：某些专业或行业软件可能没有开源版本。

**认为某方面是优点/缺点的人群**：
-   **开发者、技术人员、公司**：通常认为可定制性、可控性、低成本是优点。
-   **普通用户、大型企业**：可能认为缺乏统一支持和易用性是缺点。