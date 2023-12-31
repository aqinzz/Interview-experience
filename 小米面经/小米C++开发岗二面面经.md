## 小米C++开发岗二面面经

### 1、手撕代码：合并两个有序数组A，B，将B合并到A里



### 2、static在C语言中的用法？

1. 静态变量： 在函数内部使用`static`声明的变量被称为静态变量。与普通的局部变量不同，静态变量在函数执行完毕后并不会被销毁，而是会一直存在于程序的整个生命周期内，并且其值在函数调用之间保持不变。

```
void exampleFunction() {
    static int staticVar = 0; // 静态变量
    staticVar++;
    printf("Static variable value: %d\n", staticVar);
}

int main() {
    exampleFunction(); // 输出 Static variable value: 1
    exampleFunction(); // 输出 Static variable value: 2
    return 0;
}

```

2. 静态函数： 使用`static`关键字声明的函数被称为静态函数。静态函数只能在声明它的文件中可见，不能被其他文件访问。这样可以避免函数名冲突，并且可以隐藏实现细节。

```
static void staticFunction() {
    printf("This is a static function.\n");
}

int main() {
    staticFunction(); // 可以正常调用
    return 0;
}
```

3. 静态全局变量： 在全局作用域中使用`static`声明的变量被称为静态全局变量。静态全局变量的作用域仅限于声明它的文件内部，不能被其他文件访问。

```
// File1.c
static int staticGlobalVar = 10;

// File2.c
extern int staticGlobalVar; // 编译错误，无法访问 staticGlobalVar
```

### 3、说一下extern的用法？

在C语言中，`extern`关键字用于声明一个变量或函数，指示它是在其他文件中定义的，从而允许当前文件访问其定义。

1. 声明外部变量： 当在一个文件中使用另一个文件中定义的全局变量时，可以在当前文件中使用`extern`来声明这个变量，以便编译器知道该变量是在其他文件中定义的。

```
// File1.c
int globalVar = 10;

// File2.c
extern int globalVar; // 声明全局变量 globalVar
void someFunction() {
    printf("%d\n", globalVar); // 可以在这里使用 globalVar
}
```

2. 声明外部函数： 当在一个文件中使用另一个文件中定义的函数时，可以在当前文件中使用`extern`来声明这个函数，以便编译器知道该函数是在其他文件中定义的。

```
// File1.c
void externalFunction(); // 函数声明

// File2.c
extern void externalFunction(); // 声明外部函数 externalFunction
void someOtherFunction() {
    externalFunction(); // 可以在这里调用 externalFunction
}
```

3. 在头文件中使用extern： 通常，在头文件中声明变量或函数时会使用`extern`关键字，因为头文件通常用于包含其他文件中定义的内容，而不是实际定义这些内容。

```
// SomeHeader.h
extern int globalVar; // 外部变量声明
extern void externalFunction(); // 外部函数声明
```

### 4、讲一下volatile的用法？

在C语言中，`volatile`关键字用于告诉编译器，所修饰的变量可能会在程序的执行过程中被意外地改变，因此编译器不应该对这个变量进行优化，以确保对变量的访问是准确的。

1. 硬件映射： 在嵌入式系统中，`volatile`通常用于声明与硬件相关的变量，因为这些变量的值可以被硬件异步地改变，而不是由程序控制。

```
volatile int *ptr = (int *)0x1000; // 指向硬件地址的指针
```

2. 多线程或信号处理： 当一个变量被多个线程或信号处理程序访问并可能被修改时，应该使用`volatile`关键字来声明这个变量，以确保每次访问都是实际的读取或写入操作，而不是从缓存中读取。

```
volatile int flag = 0; // 多线程或信号处理中的标志位
```

3. 防止优化： 当编译器对代码进行优化时，会尝试将变量的值缓存在寄存器中，而不是每次访问内存。但是对于`volatile`变量，编译器必须确保每次访问都是对内存的实际读取或写入，而不是使用寄存器中的缓存值。

```
volatile int value = 10; // 防止编译器对 value 进行优化
```

### 5、链表和数组的使用场景区别？

1. **内存分配方式：**
   - 数组： 数组在内存中是连续存储的，所有元素占用的内存空间是一块连续的区域。这意味着数组的大小在创建时就需要确定，并且在运行时无法改变数组的大小。
   - 链表： 链表的元素在内存中可以是分散存储的，每个元素通常包含一个指向下一个元素的指针。链表的大小可以动态地增长或缩小，因为它不需要一块连续的内存空间。
2. **插入和删除操作：**
   - 数组： 在数组中插入或删除元素可能需要移动其他元素，特别是在数组的中间或开头插入或删除元素时。这样的操作的时间复杂度是 O(n)，其中 n 是数组的大小。
   - 链表： 在链表中插入或删除元素的时间复杂度是 O(1)，因为只需要改变指针的指向，不需要移动其他元素。
3. **访问元素的效率：**
   - 数组： 通过索引可以在 O(1) 的时间内访问数组中的任何元素，因为数组的元素是连续存储的。
   - 链表： 访问链表中的元素需要从头开始逐个遍历，时间复杂度是 O(n)，其中 n 是链表的长度。
4. **适用场景：**
   - 数组： 适用于需要随机访问元素、元素个数固定且不经常进行插入和删除操作的场景。
   - 链表： 适用于需要经常插入和删除元素、元素个数不固定且不需要随机访问的场景。

### 6、Linux环境下socket编程一般流程？

之前文章也有画过相应的流程图，今天就给大家一个简单的流程。

1. 包含头文件： 首先，在程序中包含Socket编程相关的头文件，如`<sys/socket.h>`、`<netinet/in.h>`等。
2. 创建Socket： 使用`socket()`系统调用创建一个Socket，并指定Socket的类型（如TCP或UDP）以及协议族（如IPv4或IPv6）。
3. 绑定地址： 如果是服务器程序，需要使用`bind()`系统调用将Socket绑定到一个地址上，以便客户端可以连接到这个地址。
4. 监听连接（仅适用于服务器）： 如果是服务器程序，使用`listen()`系统调用开始监听来自客户端的连接请求。
5. 接受连接（仅适用于服务器）： 使用`accept()`系统调用接受客户端的连接请求，返回一个新的Socket用于与客户端通信。
6. 连接到服务器（仅适用于客户端）： 如果是客户端程序，使用`connect()`系统调用连接到服务器。
7. 收发数据： 使用`send()`和`recv()`系统调用发送和接收数据。对于UDP Socket，可以使用`sendto()`和`recvfrom()`。
8. 关闭Socket： 使用`close()`系统调用关闭Socket。

### 7、介绍下QT的信号槽机制？

Qt的信号槽（Signals and Slots）机制是Qt中一种非常强大且灵活的通信机制，用于对象之间的通信。通过信号槽机制，一个对象可以发出信号（Signal），而另一个对象可以捕获这个信号并做出相应的响应操作，这种机制可以实现对象之间的解耦和灵活的通信方式。

在Qt中，信号槽机制的特点：

1. 信号（Signal）： 信号是一种特殊的成员函数，用于表示某个事件的发生。信号可以没有参数，也可以带有参数。在对象中通过`emit`关键字发射信号。
2. 槽（Slot）： 槽是一种普通的成员函数，用于响应信号。槽可以与一个或多个信号关联起来，当关联的信号被发射时，关联的槽函数会被调用。
3. 连接（Connection）： 连接是信号和槽之间的关联关系。通过`connect`函数可以将一个信号连接到一个槽，这样当信号被发射时，连接的槽函数会被调用。连接可以是直接连接（同步调用）或者是队列连接（异步调用）。
4. 自动连接： Qt提供了自动连接的功能，可以通过`autoconnect`机制自动将信号与槽连接起来，无需手动编写`connect`语句。
5. 跨线程通信： 信号槽机制可以实现跨线程的通信，通过`Qt::QueuedConnection`连接方式可以实现在不同线程之间安全地发送信号和调用槽函数。

### 8、如何保障线程安全？有哪些方法？

1. 互斥锁（Mutex）： 使用互斥锁可以保护共享资源，确保同一时刻只有一个线程可以访问该资源。线程在访问共享资源前先获取互斥锁，访问完成后释放互斥锁，这样可以避免多个线程同时修改共享资源导致的问题。
2. 条件变量（Condition Variable）： 条件变量通常与互斥锁一起使用，用于线程间的通信和同步。它可以让一个线程等待某个条件的发生，直到另一个线程发出信号通知条件的变化。
3. 原子操作（Atomic Operation）： 原子操作是不可分割的操作，可以保证在多线程环境下对共享变量的操作是原子性的。在C++11及以后的标准中，提供了`std::atomic`模板类来支持原子操作。
4. 读写锁（Read-Write Lock）： 读写锁允许多个线程同时读取共享资源，但是在写入时会阻塞其他线程的读取和写入。这种机制适用于读操作远远多于写操作的情况。
5. 信号量（Semaphore）： 信号量是一种计数器，用来控制对共享资源的访问。通过信号量可以限制同时访问共享资源的线程数量。
6. 使用不可变对象（Immutable Objects）： 不可变对象是指对象在创建后其状态不能被修改的对象。在多线程环境下，使用不可变对象可以避免线程安全问题。
7. 避免共享状态（Avoid Shared State）： 尽量避免多个线程共享可变的状态，而是通过消息传递等方式来进行线程间通信。
8. 使用线程安全的数据结构和算法： 在多线程环境下，选择使用线程安全的数据结构和算法可以减少需要手动管理线程安全的工作量。

### 9、加锁前需要考虑什么东西？

1. 并发访问情况： 需要考虑在多个线程同时访问某个共享资源时可能出现的情况，如读写冲突、数据竞争等。
2. 加锁粒度： 需要考虑在哪些代码段需要加锁以及锁的粒度。锁的粒度过细会导致频繁加锁解锁，影响性能；而锁的粒度过粗则可能导致锁竞争，降低并发性能。
3. 锁的选择： 需要根据实际情况选择合适的锁，如互斥锁、读写锁、自旋锁等，以及选择合适的锁策略，如短期互斥、长期等待等。
4. 锁的生命周期： 需要考虑锁的生命周期，确保锁的加锁和解锁操作成对出现，避免死锁等问题。
5. 锁的可重入性： 如果一个线程在持有锁的情况下再次尝试获取这个锁，不会造成死锁或其他异常情况，这种情况称为锁的可重入性。
6. 锁的性能影响： 加锁会引入额外的开销，需要评估加锁对程序性能的影响，尤其是在高并发场景下。
7. 资源管理： 在使用锁的过程中需要注意资源的管理，如避免内存泄漏、资源竞争等问题。
8. 异常处理： 需要考虑在加锁过程中可能发生的异常情况，确保异常安全性。

### 10、1G内存的计算机能不能malloc出比1G更大的内存？

在理论上，一个拥有1GB内存的计算机是可以调用`malloc`分配比1GB更大的内存块的。这是因为`malloc`函数在请求内存时并不是立即分配物理内存，而是向操作系统请求一段虚拟内存地址空间。这段虚拟内存地址空间的大小可以超过实际物理内存的大小。

然而，在实际情况下，当请求分配的内存超过了系统的物理内存大小时，操作系统可能会使用虚拟内存技术来满足这个请求。虚拟内存是一种通过将部分数据存储在磁盘上来扩展可用内存的技术，这样就可以在逻辑上扩展可用内存的大小。

虚拟内存虽然可以扩展可用内存的大小，但是由于磁盘的读写速度远远低于内存，因此使用虚拟内存会导致性能下降。此外，如果系统的虚拟内存空间也耗尽了，那么`malloc`可能会失败并返回`NULL`。

### 11、讲讲TCP的三次握手？

1. 第一次握手： 客户端发送一个TCP报文段，其中包含一个SYN（同步）标志位，以及客户端的初始序列号（ISN，Initial Sequence Number）。这表示客户端请求建立连接，并设置了初始序列号，以便后续的数据传输。
2. 第二次握手： 服务器接收到客户端的SYN后，确认收到，并发送回一个带有SYN和ACK（确认）标志位的报文段。服务器也会为自己设置一个初始序列号。
3. 第三次握手： 客户端接收到服务器的SYN-ACK后，确认收到，并发送一个ACK标志位的报文段。这个报文段表示连接已经建立，双方可以开始进行数据传输。

此时，TCP连接已经建立，双方可以安全地传输数据。

### 12、TCP会丢包吗？

TCP（Transmission Control Protocol）是一种可靠的、面向连接的协议，它通过各种机制来保证数据的可靠传输，其中就包括对数据包的丢失进行重传，因此在理论上，TCP 是不会丢包的。

在 TCP 中，数据包在传输过程中会经过一系列的确认和重传机制，确保数据的可靠性。如果一个数据包在传输过程中丢失了，接收方会通过发送确认消息（ACK）来通知发送方数据包丢失，并且发送方会根据接收方的确认信息进行重传，直到接收方成功接收到数据为止。

然而，虽然 TCP 是一种可靠的协议，但在实际网络环境中，由于网络拥塞、丢包、延迟等因素，仍然有可能出现数据包丢失的情况。当发生数据包丢失时，TCP 会通过重传机制来尝试恢复丢失的数据，但这会增加网络的负载和延迟。因此，在实际应用中，为了提高 TCP 的性能和可靠性，通常会结合其他技术，如拥塞控制、流量控制、错误检测和纠正等机制来优化网络传输。

### 13、讲讲TCP/IP协议有几层？

1. 链路层：也称为网络接口层，负责通过物理介质（如以太网、Wi-Fi）传输数据帧，以及进行物理寻址、错误检测和纠正等操作。常见的协议包括以太网、无线局域网（Wi-Fi）、蓝牙等。
2. 网络层：负责在不同网络之间进行数据路由和转发，使得数据能够在全球范围内进行传输。常见的协议包括 IP（Internet Protocol）、ICMP（Internet Control Message Protocol）等。
3. 传输层：提供端到端的数据传输服务，负责在通信的两端之间建立、维护和结束数据传输的连接。常见的协议包括 TCP（Transmission Control Protocol）和 UDP（User Datagram Protocol）。
4. 应用层：包含了各种网络应用所使用的协议和服务，如 HTTP（HyperText Transfer Protocol）、FTP（File Transfer Protocol）、SMTP（Simple Mail Transfer Protocol）等。应用层协议负责定义数据的格式和交换方式，实现网络中不同应用程序之间的通信。

### 14、讲讲ARP表中记录了什么信息？

ARP（Address Resolution Protocol）表是用于存储主机或路由器在本地网络中的IP地址和MAC地址之间的映射关系的表格。当主机需要向另一个主机发送数据时，它首先会检查ARP表，查找目标IP地址对应的MAC地址，如果在ARP表中找到了对应关系，就可以直接发送数据；如果没有找到，则会发送 ARP 请求来获取目标主机的 MAC 地址。

ARP表中记录了以下信息：

1. IP地址：每一行记录都包含一个IP地址，用于标识网络上的主机或路由器。
2. MAC地址：与IP地址对应的MAC地址，用于在本地网络中唯一标识一个网络设备。
3. 接口：记录了每条ARP记录对应的网络接口，即数据包应该从哪个接口发送出去。
4. 类型：记录了ARP表项的类型，通常有两种类型：动态（Dynamic）和静态（Static）。动态类型的ARP表项是通过ARP协议动态学习而来的，而静态类型的ARP表项是管理员手动添加的。

### 15、讲讲路由表中记录了哪些信息？

路由表（Routing Table）是用于存储路由器或主机上的路由信息的表格，它记录了路由器或主机在进行数据包转发时需要用到的信息。

通常记录了以下信息：

1. 目标网络地址：这是指数据包要到达的目标网络的地址，通常是一个IP地址和子网掩码的组合，用于确定目标网络的范围。
2. 下一跳地址：下一跳地址指示了数据包在当前路由器或主机上应该转发到的下一个网络设备的地址。如果数据包的目标地址在当前网络中，下一跳地址可能是目标主机的IP地址；如果数据包需要经过多个网络设备才能到达目标，下一跳地址可能是下一个中间路由器的IP地址。
3. 子网掩码：子网掩码用于确定IP地址中哪部分是网络地址，哪部分是主机地址。在路由表中，子网掩码通常与目标网络地址一起使用，以确定目标地址属于哪个网络。
4. 接口：路由表中记录了数据包应该从哪个网络接口发送出去，以便正确地将数据包转发到下一个网络设备。
5. 跃点数或距离：跃点数或距离表示了到达目标网络所需经过的路由器数量或者其他测量标准。通常情况下，跃点数越小，表示到达目标网络的路径越短，路由选择算法会优先选择跃点数更小的路径。
6. 优先级：在某些路由协议中，路由表可能包含了每条路由的优先级信息，用于在有多条路由到达同一目标网络时进行选择。

### 16、路由表中有一项的目标IP地址是default或者0.0.0.0，这是什么情况？

在路由表中，如果目标IP地址是`default`或者`0.0.0.0`，通常表示默认路由（Default Route）或者默认网关（Default Gateway）。

默认路由是指当主机或路由器无法确定数据包的目标网络时所使用的路由。当主机或路由器要发送数据包到一个它不知道如何直接到达的目标网络时，它会尝试使用默认路由。默认路由通常表示为`0.0.0.0/0`，它的存在意味着“如果不知道该数据包应该去往哪个网络，就把它发送到这里”。

在实际网络中，通常会配置一个默认网关，它是主机或者路由器连接到本地网络以外网络的出口地址。当主机或者路由器需要发送数据包到一个它不知道如何直接到达的目标网络时，它会使用默认网关来转发数据包到外部网络。

### 17、局域网里面，同一网段，需要做路由吗？

在局域网（LAN）中，如果所有设备都位于同一子网（即同一网段），通常情况下是不需要做路由的。

在同一子网中，设备之间可以直接通过MAC地址进行通信，无需通过路由器进行转发。当设备需要向同一子网中的其他设备发送数据时，它们会根据目标设备的IP地址和子网掩码来确定目标设备是否在同一子网中。如果目标设备在同一子网中，发送设备会直接发送数据到目标设备的MAC地址，而无需经过路由器。

只有当设备需要与不在同一子网中的设备进行通信时，才需要通过路由器进行通信。路由器负责在不同的子网之间进行数据包转发，将数据包从一个子网转发到另一个子网，这时才需要进行路由配置。

### 18、讲讲epoll怎么用？

`epoll` 是 Linux 下多路复用 I/O 事件通知接口，用于处理大量的文件描述符（包括 socket）上的 I/O 事件。`epoll` 提供了一种高效的 I/O 事件通知机制，可以帮助开发者实现高性能的 I/O 处理。

基本用法：

1. 创建 epoll 实例： 首先，需要使用 `epoll_create` 或者 `epoll_create1` 系统调用创建一个 epoll 实例，得到一个文件描述符，该文件描述符用于后续的 epoll 操作。
2. 添加文件描述符： 使用 `epoll_ctl` 系统调用向 epoll 实例中添加需要监控的文件描述符。需要指定添加的文件描述符、感兴趣的事件类型（如可读、可写等）、以及触发方式（如边缘触发 ET 或者水平触发 LT）等参数。
3. 等待事件： 使用 `epoll_wait` 系统调用等待文件描述符上的事件发生。`epoll_wait` 会阻塞直到有事件发生或者超时，然后返回已经就绪的文件描述符列表。
4. 处理事件： 得到就绪的文件描述符列表后，可以遍历列表，对每个就绪的文件描述符进行相应的 I/O 操作。在处理完事件后，需要根据事件类型和触发方式决定是否需要重新添加到 epoll 实例中。
5. 关闭 epoll 实例： 当不再需要 epoll 实例时，需要使用 `close` 系统调用关闭 epoll 实例对应的文件描述符。

### 19、select和epoll的区别？

1. 可监控的文件描述符数量：
   - `select`：通常限制为 1024 个文件描述符，由 `FD_SETSIZE` 宏定义。
   - `epoll`：没有固定的限制，可以支持非常大的并发连接。
2. 效率和性能：
   - `select`：使用线性扫描的方式遍历所有被监控的文件描述符，当文件描述符数量较大时，性能下降明显。
   - `epoll`：使用事件通知机制，不需要遍历所有被监控的文件描述符，而是在有事件发生时通过回调通知应用程序，因此在大规模并发的场景下性能更好。
3. 事件触发方式：
   - `select`：采用水平触发模式（Level Triggered），即只要文件描述符就绪就会通知应用程序，如果应用程序没有处理完所有就绪事件，下次仍然会通知。
   - `epoll`：支持水平触发和边缘触发两种模式（Edge Triggered）。边缘触发模式只在状态变化时通知应用程序，如果应用程序没有处理完所有就绪事件，下次不会再次通知。
4. 系统调用开销：
   - `select`：每次调用 `select` 都需要将所有被监控的文件描述符集合从用户空间拷贝到内核空间，而且每次调用都会线性扫描所有文件描述符，开销较大。
   - `epoll`：利用 `epoll_ctl` 和 `epoll_wait` 这两个系统调用将文件描述符集合注册到内核，并在有事件发生时通过回调通知应用程序，避免了每次调用都要扫描所有文件描述符的开销。

### 20、页表中需要一个硬件处理单元，这个硬件处理单元叫什么？

页表中的硬件处理单元通常被称为 MMU（Memory Management Unit，内存管理单元）。MMU 是计算机系统中用于实现虚拟内存管理的关键组成部分，它负责将虚拟地址转换为物理地址，并执行访问权限检查等功能。

MMU 的主要作用：

1. 地址转换：将程序使用的虚拟地址转换为物理地址，使得程序可以访问到实际的物理内存。
2. 内存保护：根据操作系统的设置，MMU 可以检查每次内存访问的权限，确保程序只能访问其被授权的内存区域。
3. 地址空间隔离：MMU 可以将不同程序的虚拟地址空间隔离开来，使它们不会互相干扰，从而提高系统的安全性和稳定性。
4. 缓存控制：MMU 可以对内存访问进行缓存控制，包括缓存命中和缓存失效的处理，以提高内存访问的效率。

### 21、介绍下DMA工作原理？

DMA（Direct Memory Access，直接内存访问）是计算机系统中用于高速数据传输的一种技术。DMA 允许外部设备（如硬盘、网卡等）直接访问系统内存，而无需经过 CPU 的介入，从而提高了数据传输的效率和系统的整体性能。

工作原理如下：

1. 初始化 DMA 控制器：首先，CPU 会初始化 DMA 控制器，包括设置 DMA 通道、设置数据传输方向、设置内存地址等参数。DMA 控制器通常集成在主板的芯片组中，负责管理系统内存和外部设备之间的数据传输。
2. 请求 DMA 传输：外部设备（如硬盘）需要进行数据传输时，会向 DMA 控制器发送 DMA 请求。DMA 请求可以通过硬件中断、DMA 请求线等方式来触发。
3. DMA 控制器响应：DMA 控制器收到 DMA 请求后，会根据配置的参数和请求的特性进行相应的处理。这包括确定数据传输的方向（读取还是写入）、数据传输的长度、起始地址等。
4. DMA 传输：DMA 控制器根据配置的参数，直接从外部设备读取数据或者向外部设备写入数据，同时将数据直接传输到系统内存中，无需 CPU 的介入。这样可以减少 CPU 的负担，提高数据传输的效率。
5. DMA 完成：当数据传输完成时，DMA 控制器会向外部设备发送 DMA 完成信号，并可能触发相应的中断通知 CPU。