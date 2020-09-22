### 操作系统

<details>
    <summary>进程和线程的区别</summary>
    <ul>
        <li>进程是资源分配的最小单位，线程是程序执行的最小单位（资源调度的最小单位）</li>
        <li>进程有自己独立的地址空间，每启动一个进程，系统将为其分配地址空间，建立数据表来维护代码段，堆栈段和数据段。
            <ul>
                <li>而线程是共享进程中的数据，使用相同的地址空间，CPU开销小。</li>
            </ul>
        </li>
        <li>线程之间的通信更方便，同一进程下的线程共享全局变量，静态变量等数据，进程之间的通信则需要以通信的方式进行。</li>
        <li>多进程比多线程更健壮，多线程中一个线程死亡意味着进程的死亡。而多进程程序不同进程之间互不影响。</li>
    </ul>
</details>

<details>
    <summary>进程间的通信方式（IPC）</summary>
    <ul>
        <li><strong>管道</strong>：在内核中申请一块固定大小的缓冲区，程序拥有写入和读取的权利。
            <ul>
                <li>匿名管道：用于亲缘关系进程，父子进程。</li>
                <li>有名管道：可用于无亲缘关系的进程间。</li>
            </ul>
        </li>
        <li><strong>信号</strong>：信号是一种比较复杂的通信方式，用于通知接收进程某个事件已经发生。
        </li>
        <li><strong>信号量</strong>：在内核中创建一个信号量集合（本质是个数组），数组的元素（信号量）都是1，使用P操作进行-1，使用V操作+1。
            <ul>
                <li>PV操作用于同一进程实现互斥。</li>
                <li>PV操作用于不同进程实现同步。</li>
            </ul>
        </li>
        <li><strong>消息队列</strong>：在内核中创建一队列，队列中每个元素是一个数据报，不同的进程可以通过句柄去访问这个队列。<br>
            <em><strong>消息队列提供了⼀个从⼀个进程向另外⼀个进程发送⼀块数据的⽅法。</strong></em>
            <ul>
                <li>不足：消息长度有上限</li>
            </ul>
        </li>
        <li><strong>共享内容</strong>：将同一块物理内存一块映射到不同的进程的虚拟地址空间中，实现不同进程间对同一资源的共享。
            <ul>
                <li>共享内存可以说是最有用的进程间通信方式，也是最快的IPC形式。</li>
                <li>不用从用户态到内核态的频繁切换和拷贝数据，直接从内存中读取就可以。</li>
                <li>共享内存是临界资源，所以需要操作时必须要保证原子性。使用信号量或者互斥锁都可以。</li>
                <li>生命周期随内核。</li>
            </ul>
        </li>
        <li><strong>套接字</strong>：套解口也是一种进程间通信机制，与其他通信机制不同的是，它可用于不同及其间的进程通信。主要用于客户端和服务端之间的进程通信。
        </li>
    </ul>
</details>

<details>
    <summary>线程间的通信方式</summary>
    <ol>
        <li><strong>锁机制</strong>：包括互斥锁、条件变量、读写锁
            <ul>
                <li>互斥锁提供了以排他方式防止数据结构被并发修改的方法。</li>
                <li>读写锁允许多个线程同时读共享数据，而对写操作是互斥的。</li>
                <li>条件变量可以以原子的方式阻塞进程，直到某个特定条件为真为止。对条件的测试是在互斥锁的保护下进行的。<em><strong>条件变量始终与互斥锁一起使用。</strong></em></li>
            </ul>
        </li>
        <li><strong>信号量机制(Semaphore)</strong>：包括无名线程信号量和命名线程信号量</li>
        <li><strong>信号机制(Signal)</strong>：类似进程间的信号处理</li>
    </ol>
    <p>线程间的通信目的主要是用于线程同步，所以线程没有像进程通信中的用于数据交换的通信机制。</p>
</details>

<details>
    <summary>进程的状态和转换关系</summary>
    <p><img src="image/image01.jpg" alt=""></p>
    <ol>
        <li>
            <p>就绪——执行：对就绪状态的进程，当进程调度程序按一种选定的策略从中选中一个就绪进程，为之分配了处理机后，该进程便由就绪状态变为执行状态；</p>
        </li>
        <li>
            <p>执行——阻塞：正在执行的进程因发生某等待事件而无法执行，则进程由执行状态变为阻塞状态。如：</p>
            <ul>
                <li>进程提出输入/输出请求而变成等待外部设备传输信息的状态</li>
                <li>进程申请资源（主存空间或外部设备）得不到满足时变成等待资源状态</li>
                <li>进程运行中出现了故障（程序出错或主存储器读写错等）变成等待干预状态等等</li>
            </ul>
        </li>
        <li>
            <p>阻塞——就绪：处于阻塞状态的进程，在其等待的事件已经发生，如：</p>
            <ul>
                <li>输入/输出完成</li>
                <li>资源得到满足或错误处理完毕时</li>
            </ul>
            <p>处于等待状态的进程并不马上转入执行状态，而是先转入就绪状态，然后再由系统进程调度程序在适当的时候将该进程转为执行状态；</p>
        </li>
        <li>
            <p>执行——就绪：正在执行的进程，因时间片用完而被暂停执行，或在采用抢先式优先级调度算法的系统中,当有更高优先级的进程要运行而被迫让出处理机时，该进程便由执行状态转变为就绪状态。</p>
        </li>
    </ol>
</details>

<details>
    <summary>进程调度算法和比较</summary>
    <p><img src="image/image03.png" alt=""></p>
    <ol>
        <li>
            <p><strong>先来先去服务(FCFS)</strong><br>
                先来先去服务调度算法是一种最简单的调度算法，也称为先进先出或严格排队方案。当每个进程就绪后，它加入就绪队列。当前正运行的进程停止执行，选择在就绪队列中存在时间最长的进程运行。该算法既可以用于作业调度，也可以用于进程调度。先来先去服务比较适合于常作业（进程），而不利于段作业（进程）。
            </p>
        </li>
        <li>
            <p><strong>时间片轮转法(RR)</strong><br>
                轮转法是基于适中的抢占策略的，以一个周期性间隔产生时钟中断，当中断发生后，当前正在运行的进程被置于就绪队列中，然后基于先来先去服务策略选择下一个就绪作业的运行。这种技术也称为时间片，因为每个进程再被抢占之前都给定一片时间。
            </p>
            <ul>
                <li>过程：1、排成一个队列。2、每次调度时将CPU分派给队首进程。3、时间片结束时，发生时钟中断。4、暂停当前进程的执行，将其送到就绪队列的末尾，并通过上下文切换执行当前就绪的队首进程。</li>
                <li>说明：1、进程阻塞情况发生时，未用完时间片也要出让CPU。2、能够及时响应，但没有考虑作业长短等问题。3、系统的处理能力和系统的负载状态影响时间片长度。</li>
            </ul>
        </li>
        <li>
            <p><strong>最短进程优先(SJF)</strong><br>
                最短进程优先是一个非抢占策略，他的原则是下一次选择预计处理时间最短的进程，因此短进程将会越过长作业，跳至队列头。该算法即可用于作业调度，也可用于进程调度。但是他对长作业不利，不能保证紧迫性作业（进程）被及时处理，作业的长短只是被估算出来的。
            </p>
        </li>
        <li>
            <p><strong>最短剩余时间优先(SRTF)</strong><br>
                最短剩余时间是针对最短进程优先增加了抢占机制的版本。在这种情况下，进程调度总是选择预期剩余时间最短的进程。当一个进程加入到就绪队列时，他可能比当前运行的进程具有更短的剩余时间，因此只要新进程就绪，调度程序就能可能抢占当前正在运行的进程。像最短进程优先一样，调度程序正在执行选择函数是必须有关于处理时间的估计，并且存在长进程饥饿的危险。
            </p>
        </li>
        <li>
            <p><strong>高优先权优先调度算法HPF和高响应比优先调度算法HRRN</strong><br>
                （1）两种方式：非抢占式优先权算法、抢占式优先权算法（关键点：新作业产生时）<br>
                （2）类型:静态优先权：创建进程时确定，整个运行期间保持不变。动态优先权：创建进程时赋予的优先权可随进程的推进或随其等待时间的增加而改变。<br>
                （3）高响应比优先调度算法HRRN</p>
            <blockquote>
                <p>HRRN为每个作业引入动态优先权，使作业的优先级随着等待时间的增加而以速率a提高：优先权 =（等待时间+要求服务时间)/要求服务时间= 响应时间 / 要求服务时间。<br>
                    什么时候计算各进程的响应比优先权？（作业完成时、新作业产生时（抢占、非抢占）、时间片完成时、进程阻塞时）</p>
            </blockquote>
        </li>
        <li>
            <p><strong>多级反馈队列算法FB</strong></p>
            <ul>
                <li>过程：1、准备调度：先将它放入第一个队列的末尾，按FCFS原则排队等待调度。2、IF时间片内完成，便可准备撤离系统。3、IF时间片内未能完成，调度程序便将该进程转入第二队列的末尾等待再次被调度执行。4、当第一队列中的进程都执行完，系统再按FCFS原则调度第二队列。在第二队列的稍放长些的时间片内仍未完成，再依次将它放入第三队列。5、依次降到第n队列后，在第n队列中便采取按时间片轮转的方式运行。
                </li>
                <li>说明:1、设置多个就绪队列，各队列有不同的优先级,优先级从第一个队列依次降低。2、赋予各队列进程执行时间片大小不同,
                    优先权越高，时间片越短。3、仅当优先权高的队列（如第一队列）空闲时，调度程序才调度第二队列中的进程运行。4、高优先级抢占时，被抢占的进程放回原就绪队列末尾。</li>
            </ul>
        </li>
    </ol>
</details>

<details>
    <summary>死锁（死锁条件、避免死锁、死锁检测、死锁预防）</summary>
    <p><strong>死锁的定义：</strong><br>
        多个进行相互等待对方资源，在得到所有资源继续运行之前，都不会释放自己已有的资源，这样造成了循环等待的现象，称为死锁。</p>
    <p><strong>产生死锁的四大必要条件：</strong></p>
    <ol>
        <li>
            <p>资源互斥/资源不共享<br>
                每个资源要么已经分配给了一个进程，要么是可用的，只有这两种状态，资源不可以被共享使用，所以所谓的互斥是指：资源不共享，如果被使用，只能被一个进程使用。</p>
        </li>
        <li>
            <p>占有和等待/请求并保持<br>
                已经得到资源的进程还能继续请求新的资源，所以个人觉得叫占有并请求也许更好理解。</p>
        </li>
        <li>
            <p>资源不可剥夺<br>
                当一个资源分配给了一个进程后，其它需要该资源的进程不ß能强制性获得该资源，除非该资源的当前占有者显示地释放该资源。</p>
        </li>
        <li>
            <p>环路等待<br>
                死锁发生时，系统中一定有由两个或两个以上的进程组成的一条环路，环路上的每个进程都在等待下一个进程所占有的资源。</p>
        </li>
    </ol>
    <p><strong>防止死锁的方法：</strong></p>
    <ol>
        <li>
            <p>破坏互斥条件</p>
            <ul>
                <li>方法：如果允许系统资源都能共享使用，则系统不会进入死锁状态。</li>
                <li>缺点：有些资源根本不能同时访问，如打印机等临界资源只能互斥使用。所以，破坏互斥条件而预防死锁的方法不太可行，而且在有的场合应该保护这种互斥性。</li>
            </ul>
        </li>
        <li>
            <p>破坏请求并保持条件</p>
            <ul>
                <li>方法：釆用预先静态分配方法，即进程在运行前一次申请完它所需要的全部资源，在它的资源未满足前，不把它投入运行。一旦投入运行后，这些资源就一直归它所有，也不再提出其他资源请求，这样就可以保证系统不会发生死锁。
                </li>
                <li>缺点：系统资源被严重浪费，其中有些资源可能仅在运行初期或运行快结束时才使用，甚至根本不使用。而且还会导致“饥饿”现象，当由于个别资源长期被其他进程占用时，将致使等待该资源的进程迟迟不能开始运行。
                </li>
            </ul>
        </li>
        <li>
            <p>破坏不可剥夺条件</p>
            <ul>
                <li>方法：当一个已保持了某些不可剥夺资源的进程，请求新的资源而得不到满足时，它必须释放已经保持的所有资源，待以后需要时再重新申请。这意味着，一个进程已占有的资源会被暂时释放，或者说是被剥夺了，或从而破坏了不可剥夺条件。
                </li>
                <li>缺点：该策略实现起来比较复杂，释放已获得的资源可能造成前一阶段工作的失效，反复地申请和释放资源会增加系统开销，降低系统吞吐量。这种方法常用于状态易于保存和恢复的资源，如CPU的寄存器及内存资源，一般不能用于打印机之类的资源。
                </li>
            </ul>
        </li>
        <li>
            <p>破坏循环等待条件</p>
            <ul>
                <li>方法：为了破坏循环等待条件，可釆用顺序资源分配法。首先给系统中的资源编号，规定每个进程，必须按编号递增的顺序请求资源，同类资源一次申请完。也就是说，只要进程提出申请分配资源Ri，则该进程在以后的资源申请中，只能申请编号大于Ri的资源。
                </li>
                <li>缺点：这种方法存在的问题是，编号必须相对稳定，这就限制了新类型设备的增加；尽管在为资源编号时已考虑到大多数作业实际使用这些资源的顺序，但也经常会发生作业使用资源的顺序与系统规定顺序不同的情况，造成资源的浪费；此外，这种按规定次序申请资源的方法，也必然会给用户的编程带来麻烦。
                </li>
            </ul>
        </li>
    </ol>
    <p><strong>死锁的检测方式</strong><br>
        绘制资源分配图，进行简化。</p>
    <blockquote>
        <p>死锁定理：</p>
        <ol>
            <li>如果资源分配图中没有环路，则系统没有死锁。</li>
            <li>如果资源分配图中出现了环路，则系统可能有死锁。</li>
            <li>如果资源分配图不可完全简化，则系统死锁。</li>
        </ol>
    </blockquote>
    <p><strong>死锁的解除</strong></p>
    <ol>
        <li>资源剥夺法<br>挂起某些死锁进程，并抢占它的资源，将这些资源分配给其他的死锁进程。但应防止被挂起的进程长时间得不到资源，而处于资源匮乏的状态。</li>
        <li>撤销进程法<br>强制撤销部分、甚至全部死锁进程并剥夺这些进程的资源。撤销的原则可以按进程优先级和撤销进程代价的高低进行。</li>
        <li>进程回退法<br>让一（多）个进程回退到足以回避死锁的地步，进程回退时自愿释放资源而不是被剥夺。要求系统保持进程的历史信息，设置还原点。</li>
    </ol>
</details>

### 计算机网络

<details>
    <summary>浏览器输入一个地址到看到返回内容中间发生了什么?</summary>
    <ol>
        <li>查询DNS，获取域名对应的IP。<br>
            <strong>DNS解析机制：</strong>
            <ol>
                <li>检查本地hosts文件是否有这个网址的映射，如果有，就调用这个IP地址映射，解析完成。</li>
                <li>如果没有，则查找本地DNS解析器缓存是否有这个网址的映射，如果有，返回映射，解析完成。</li>
                <li>如果没有，则查找填写或分配的首选DNS服务器，称为本地DNS服务器。服务器接收到查询时：【如果要查询的域名包含在本地配置区域资源中，返回解析结果，查询结束，此解析具有权威性。】【如果要查询的域名不由本地DNS服务器区域解析，但服务器缓存了此网址的映射关系，返回解析结果，查询结束，此解析不具有权威性。】
                </li>
                <li>如果本地DNS服务器也失效：通过迭代的方式一层层向上级DNS服务器请求，在顶级DNS解析服务器后则通过迭代的方式来查找。</li>
            </ol>
        </li>
        <li>客户机发送HTTP请求报文：
            <ol>
                <li>应用层：客户端发送HTTP请求报文</li>
                <li>传输层：切分长数据，并确保可靠性。</li>
                <li>网络层：进行路由</li>
                <li>数据链路层：传输数据</li>
                <li>物理层：物理传输bit</li>
            </ol>
        </li>
        <li>服务器端经过物理层→数据链路层→网络层→传输层→应用层，解析请求报文，发送HTTP响应报文。</li>
        <li>客户端解析HTTP响应报文</li>
        <li>浏览器开始显示HTML</li>
        <li>浏览器重新发送请求获取图片、CSS、JS的数据。</li>
        <li>浏览器渲染页面<br>
            <strong>渲染机制：</strong><br>
            构建DOM树 -&gt; CSS解析 -&gt; 构建渲染树 -&gt; 渲染树布局 -&gt; 渲染树绘制
            <ul>
                <li>构建DOM树<br>
                    当浏览器客户端从服务器那接受到HTML文档后，就会遍历文档节点然后生成DOM树，DOM树结构和HTML标签一一对应。需要注意记下几点：
                    <ul>
                        <li>DOM树在构建的过程中可能会被CSS和JS的加载而执行阻塞。（这在后面会详细介绍。）</li>
                        <li>display:none 的元素也会在DOM树中。</li>
                        <li>注释也会在DOM树中</li>
                        <li>Script标签会在DOM树中</li>
                    </ul>
                </li>
                <li>CSS解析<br>
                    浏览器会解析CSS文件并生成CSS规则树，在过程中，每个CSS文件都会被分析成StyleSheet对象，每个对象都包括CSS规则，CSS规则对象包括对应的选择器和声明对象以及其他对象。在这个过程需要注意的是：
                    <ul>
                        <li>CSS解析可以与DOM解析同进行。</li>
                        <li>CSS解析与script的执行互斥。</li>
                        <li>在Webkit内核中进行了script执行优化，只有在JS访问CSS时才会发生互斥。</li>
                    </ul>
                </li>
                <li>构建渲染树<br>
                    通过DOM树和CSS规则树，浏览器就可以通过它两构建渲染树了。浏览器会先从DOM树的根节点开始遍历每个可见节点，让后对每个可见节点找到适配的CSS样式规则并应用。具体的规则有以下几点需要注意：
                    <ul>
                        <li>Render Tree和DOM Tree不完全对应。</li>
                        <li>display: none的元素不在Render Tree中</li>
                        <li>visibility: hidden的元素在Render Tree中</li>
                    </ul>
                </li>
                <li>渲染树布局<br>
                    布局阶段会从渲染树的更节点开始遍历，由于渲染树的每个节点都是一个Render
                    Object对象，包含宽高，位置，背景色等样式信息。所以浏览器就可以通过这些样式信息来确定每个节点对象在页面上的确切大小和位置，布局阶段的输出就是我们常说的盒子模型，它会精确地捕获每个元素在屏幕内的确切位置与大小。需要注意的是：
                    <ul>
                        <li>float元素，absoulte元素，fixed元素会发生位置偏移。</li>
                        <li>我们常说的脱离文档流，其实就是脱离Render Tree。</li>
                    </ul>
                </li>
                <li>渲染树绘制<br>
                    在绘制阶段，浏览器会遍历渲染树，调用渲染器的paint()方法在屏幕上显示其内容。渲染树的绘制工作是由浏览器的UI后端组件完成的。</li>
            </ul>
        </li>
    </ol>
</details>

<details>
    <summary>TCP状态机以及三次握手和四次挥手</summary>
    <p><img src="image/image02.jpeg" alt=""></p>
    <p><strong>各个状态的解释</strong></p>
    <ul>
        <li>LISTEN：监听来自远方TCP端口的连接请求。</li>
        <li>SYN-SENT：发送连接请求后等待匹配的连接请求。</li>
        <li>SYN-RCVD：收到SYN并发送一个连接请求后等待双方连接确认。</li>
        <li>FIN-WAIT-1：等待远程TCP的连接中断请求，或先前的连接中断请求的确认。（等待接收ACK）</li>
        <li>FIN-WAIT-2：从远程TCP等待连接中断请求。（等待接收FIN）</li>
        <li>CLOSING-WAIT：等待从本地用户发来的连接中断请求。</li>
        <li>CLOSING：等待远程TCP对连接中断请求的确认。</li>
        <li>LAST-ACK：等待原来发向远程TCP的连接中断请求的确认。</li>
        <li>TIME-WAIT：等待足够时间确保远程TCP接受连接中断请求的确认。</li>
        <li>CLOSED：无连接状态。</li>
    </ul>
    <p><strong>三次握手</strong></p>
    <ul>
        <li>第一次握手：<br>
            Client什么都不能确认。<br>
            Server确认：对方发送正常。</li>
        <li>第二次握手：<br>
            Client确认：自己发送/接收正常，对方发送/接收正常。<br>
            Server确认：自己接受正常，对方发送正常。</li>
        <li>第三次握手：<br>
            Client确认：自己发送/接收正常，对方发送/接收正常。<br>
            Server确认：自己接受正常，对方发送正常。</li>
    </ul>
    <p><strong>四次挥手</strong><br>
        当收到对方的FIN报文时，仅表示对方不再发送数据但还能接收收据，我们也未必把全部数据都发给了对方，所以我们可以立即close，也可以发送一些数据给对方后，再发送FIN报文给对方表示同意关闭连接。因此我们的ACK和FIN一般会分开发送。
    </p>
    <p><strong>CLOSING状态出现的情况</strong><br>
    <p><img src="image/image04.png" alt=""></p>
    客户端和服务端同时发送FIN给对方，并都进入FIN_WAIT_1状态，接收到对方发来的FIN后，都由FIN_WAIT_1状态进入CLOSING状态并发送ACK给对方确认关闭连接。接收到对方的ACK后双方都进入TIME_WAIT状态等待关闭连接。
    </p>
    <p><strong>为什么需要TIME_WAIT状态</strong></p>
    <ol>
        <li>
            <p>为实现TCP这种全双工连接的可靠释放<br>
                这样可让TCP再次发送最后的ACK以防这个ACK丢失(另一端超时并重发最后的FIN)这种2MSL等待的另一个结果是这个TCP连接在2MSL等待期间，定义这个连接的插口(客户的IP地址和端口号，服务器的IP地址和端口号)不能再被使用。这个连接只能在2MSL结束后才能再被使用。
            </p>
        </li>
        <li>
            <p>为使旧的数据包在网络因过期而消失<br>
                每个具体TCP实现必须选择一个报文段最大生存时间MSL。它是任何报文段被丢弃前在网络内的最长时间。</p>
        </li>
    </ol>

</details>

<details>
    <summary>服务器大量处于TIME_WAIT状态的原因和解决</summary>
    <p><strong>原因</strong><br>
        TIME_WAIT是主动关闭连接的一方保持的状态，对于服务器来说他本身就是“客户端”，在完成某一个中请求之后，他就会发起主动关闭连接，从而进入TIME_WAIT的状态，然后在保持这个状态2MSL（max segment
        lifetime）时间之后，彻底关闭回收资源。</p>
    <p><strong>危害</strong></p>
    <ol>
        <li>高并发可以让服务器在短时间范围内同时占用大量端口，而端口有个0~65535的范围，并不是很多，刨除系统和其他服务要用的，剩下的就更少了。</li>
        <li>在这个场景中，短连接表示“业务处理+传输数据的时间 远远小于 TIMEWAIT超时的时间”的连接。</li>
    </ol>
    <p><strong>解决方案</strong><br>
        由于TIME_WAIT是主动发起一方的出现的问题，对网络配置参数进行优化</p>
    <ul>
        <li>开启SYN Cookies: <code>net.ipv4.tcp_syncookies = 1</code>表示开启SYN
            Cookies。当出现SYN等待队列溢出时，启用cookies来处理，可防范少量SYN攻击，默认为0，表示关闭；</li>
        <li>允许端口重用：<code>net.ipv4.tcp_tw_reuse = 1</code>表示开启重用。允许将TIME-WAIT sockets重新用于新的TCP连接，默认为0，表示关闭；</li>
        <li>开启快速回收机制：<code>net.ipv4.tcp_tw_recycle = 1</code>表示开启TCP连接中TIME-WAIT sockets的快速回收，默认为0，表示关闭。</li>
        <li>缩短TIMEOUT时间：<code>net.ipv4.tcp_fin_timeout</code>修改系統默认的TIMEOUT时间(30)</li>
    </ul>
</details>

<details>
    <summary>服务器大量处于CLOSE_WAIT状态的原因和解决</summary>
    <p><strong>原因</strong><br>
        一直保持在CLOSE_WAIT状态，那么只有一种情况，就是在对方关闭连接之后服务器程序自己没有进一步发出ack信号。换句话说，就是在对方连接关闭之后，程序里没有检测到，或者程序压根就忘记了这个时候需要关闭连接，于是这个资源就一直被程序占着。
    </p>
    <p><strong>解决方案</strong><br>
        检查服务器程序代码，尤其是在确认连接关闭等地方函数，是否有异常分支导致程序没有处理而无法正常进行关闭连接的情况。</p>
</details>

<details>
    <summary>TCP协议如何保证可靠性</summary>
    <div>
        <p><strong>确保传输可靠性的方式</strong><br>
            TCP协议保证数据传输可靠性的方式主要有：</p>
        <ul>
            <li>校验和</li>
            <li>序列号&amp;确认应答</li>
            <li>超时重传</li>
            <li>连接管理</li>
            <li>流量控制</li>
            <li>拥塞控制</li>
        </ul>
        <p><strong>校验和</strong><br>
            发送的数据包的二进制相加然后取反，目的是检测数据在传输过程中的任何变化。如果收到段的检验和有差错，TCP将丢弃这个报文段和不确认收到此报文段。 如果无差错，也不一定代表数据传输成功。</p>
        <ul>
            <li>计算方式：在数据传输的过程中，将发送的数据段都当做一个16位的整数。将这些整数加起来。并且前面的进位不能丢弃，补在尾端进行相加，最后取反，得到校验和。</li>
            <li>发送方：在发送数据之前计算检验和，并进行校验和的填充。</li>
            <li>接收方：收到数据后，对数据以同样的方式进行计算，求出校验和，与发送方的进行比对。</li>
        </ul>
        <p><strong>序列号&amp;确认应答</strong><br>
            TCP给发送的每一个包进行编号，接收方对数据包进行排序，把有序数据传送给应用层。</p>
        <ul>
            <li>序列号：TCP传输时将每个字节的数据都进行了编号，这就是序列号。</li>
            <li>确认应答：TCP传输的过程中，每次接收方收到数据后，都会对传输方进行确认应答。也就是发送ACK报文。这个ACK报文当中带有对应的确认序列号，告诉发送方，接收到了哪些数据，下一次的数据从哪里发。</li>
        </ul>
        <p><strong>超时重传</strong><br>
            当TCP发出一个段后，它启动一个定时器，等待目的端确认收到这个报文段。如果不能及时收到一个确认，将重发这个报文段。</p>
        <ul>
            <li><strong>原因</strong> 发送方没有介绍到响应的ACK报文原因：
                <ol>
                    <li>数据在传输过程中由于网络原因等直接全体丢包，接收方根本没有接收到。</li>
                    <li>接收方接收到了响应的数据，但是发送的ACK报文响应却由于网络原因丢包了。</li>
                </ol>
            </li>
        </ul>
        <p>简单理解就是发送方在发送完数据后等待一个时间，时间到达没有接收到ACK报文，那么对刚才发送的数据进行重新发送。如果是刚才第一个原因，接收方收到二次重发的数据后，便进行ACK应答。如果是第二个原因，接收方发现接收的数据已存在（判断存在的根据就是序列号，所以上面说序列号还有去除重复数据的作用），那么直接丢弃，仍旧发送ACK应答。
        </p>
        <p>由于TCP传输时保证能够在任何环境下都有一个高性能的通信，因此这个最大超时时间（发送方发送完毕后等待的时间）是动态计算的。一般是500ms。</p>
        <p><strong>连接管理</strong><br>
            连接管理就是三次握手与四次挥手的过程。所有可靠性的理论前提。</p>
        <p><strong>流量控制</strong><br>
            TCP连接的每一方都有固定大小的缓冲空间，<strong>TCP的接收端只允许发送端发送接收端缓冲区能接纳的数据</strong>。当接收方来不及处理发送方的数据，能提示发送方降低发送的速率，防止包丢失。TCP使用的流量控制协议是<em>可变大小的滑动窗口协议</em>。
        </p>
        <p>接收方有即时窗口（<strong>滑动窗口</strong>），随ACK报文发送。</p>
        <p>接收端在接收到数据后，对其进行处理。如果发送端的发送速度太快，导致接收端的结束缓冲区很快的填充满了。此时如果发送端仍旧发送数据，那么接下来发送的数据都会丢包，继而导致丢包的一系列连锁反应，超时重传呀什么的。而TCP根据接收端对数据的处理能力，决定发送端的发送速度，这个机制就是流量控制。
        </p>
        <p>在TCP协议的报头信息当中，有一个16位字段的窗口大小。在介绍这个窗口大小时我们知道，<strong>窗口大小的内容实际上是接收端接收数据缓冲区的剩余大小。这个数字越大，证明接收端接收缓冲区的剩余空间越大，网络的吞吐量越大</strong>。接收端会在确认应答发送ACK报文时，将自己的即时窗口大小填入，并跟随ACK报文一起发送过去。而<strong>发送方根据ACK报文里的窗口大小的值的改变进而改变自己的发送速度</strong>。如果接收到窗口大小的值为0，那么发送方将停止发送数据。并定期的向接收端发送窗口探测数据段，让接收端把窗口大小告诉发送端。
        </p>
        <blockquote>
            <p>16位的窗口大小最大能表示65535个字节（64K），但是TCP的窗口大小最大并不是64K。在TCP首部中40个字节的选项中还包含了一个窗口扩大因子M，实际的窗口大小就是16为窗口字段的值左移M位。每移一位，扩大两倍。
            </p>
        </blockquote>
        <p><strong>拥塞控制</strong><br>
            当网络拥塞时，减少数据的发送。<br>
            发送方有拥塞窗口，发送数据前比对接收方发过来的即使窗口，取小<br>
            <em>慢启动、拥塞避免、拥塞发送、快速恢复</em></p>
        <p>TCP传输的过程中，发送端开始发送数据的时候，如果刚开始就发送大量的数据，那么就可能造成一些问题。网络可能在开始的时候就很拥堵，如果给网络中在扔出大量数据，那么这个拥堵就会加剧。拥堵的加剧就会产生大量的丢包，就对大量的超时重传，严重影响传输。
        </p>
        <p>所以TCP引入了慢启动的机制，在开始发送数据时，先发送少量的数据探路。探清当前的网络状态如何，再决定多大的速度进行传输。这时候就引入一个叫做拥塞窗口的概念。发送刚开始定义拥塞窗口为
            1，每次收到ACK应答，拥塞窗口加1。在发送数据之前，首先将拥塞窗口与接收端反馈的窗口大小比对，取较小的值作为实际发送的窗口。</p>
        <p>拥塞窗口的增长是指数级别的。慢启动的机制只是说明在开始的时候发送的少，发送的慢，但是增长的速度是非常快的。为了控制拥塞窗口的增长，不能使拥塞窗口单纯的加倍，设置一个拥塞窗口的阈值，当拥塞窗口大小超过阈值时，不能再按照指数来增长，而是线性的增长。在慢启动开始的时候，慢启动的阈值等于窗口的最大值，一旦造成网络拥塞，发生超时重传时，慢启动的阈值会为原来的一半（这里的原来指的是发生网络拥塞时拥塞窗口的大小），同时拥塞窗口重置为1。
        </p>
    </div>
</details>

<details>
    <summary>UDP协议特点和如何实现可靠</summary>
    <div>
        <p>UDP协议全称是用户数据报协议，在网络中它与TCP协议一样用于处理数据包，是一种无连接的协议。在OSI模型中，在第四层——传输层，处于IP协议的上一层。UDP有不提供数据包分组、组装和不能对数据包进行排序的缺点，也就是说，当报文发送之后，是无法得知其是否安全完整到达的。
        </p>
        <p><strong>特点</strong></p>
        <ol>
            <li>
                <p><strong>面向无连接</strong><br>
                    首先 UDP 是不需要和 TCP一样在发送数据前进行三次握手建立连接的，想发数据就可以开始发送了。并且也只是数据报文的搬运工，不会对数据报文进行任何拆分和拼接操作。<br>
                    具体来说就是：</p>
                <ul>
                    <li>在发送端，应用层将数据传递给传输层的 UDP 协议，UDP 只会给数据增加一个 UDP 头标识下是 UDP 协议，然后就传递给网络层了</li>
                    <li>在接收端，网络层将数据传递给传输层，UDP 只去除 IP 报文头就传递给应用层，不会任何拼接操作</li>
                </ul>
            </li>
            <li>
                <p><strong>有单播，多播，广播的功能</strong><br>
                    UDP 不止支持一对一的传输方式，同样支持一对多，多对多，多对一的方式，也就是说 UDP 提供了单播，多播，广播的功能。</p>
            </li>
            <li>
                <p><strong>UDP是面向报文的</strong><br>
                    发送方的UDP对应用程序交下来的报文，在添加首部后就向下交付IP层。UDP对应用层交下来的报文，既不合并，也不拆分，而是保留这些报文的边界。因此，应用程序必须选择合适大小的报文</p>
            </li>
            <li>
                <p><strong>不可靠性</strong><br>
                    首先不可靠性体现在无连接上，通信都不需要建立连接，想发就发，这样的情况肯定不可靠。<br>
                    并且收到什么数据就传递什么数据，并且也不会备份数据，发送数据也不会关心对方是否已经正确接收到数据了。<br>
                    再者网络环境时好时坏，但是 UDP
                    因为没有拥塞控制，一直会以恒定的速度发送数据。即使网络条件不好，也不会对发送速率进行调整。这样实现的弊端就是在网络条件不好的情况下可能会导致丢包，但是优点也很明显，在某些实时性要求高的场景（比如电话会议）就需要使用
                    UDP 而不是 TCP。</p>
            </li>
            <li>
                <p><strong>头部开销小，传输数据报文时是很高效的。</strong><br>
                    UDP 头部包含了以下几个数据：</p>
                <ul>
                    <li>两个十六位的端口号，分别为源端口（可选字段）和目标端口</li>
                    <li>整个数据报文的长度</li>
                    <li>整个数据报文的检验和（IPv4 可选 字段），该字段用于发现头部信息和数据中的错误</li>
                </ul>
                <p>因此 UDP 的头部开销小，只有八字节，相比 TCP 的至少二十字节要少得多，在传输数据报文时是很高效的</p>
            </li>
        </ol>
        <p><strong>如何实现UDP的可靠性</strong></p>
        <p><strong>原理与简单实现</strong><br>
            传输层无法保证数据的可靠传输，只能通过应用层来实现了。实现的方式可以参照tcp可靠性传输的方式，只是实现不在传输层，实现转移到了应用层。<br>
            最简单的方式是在应用层模仿传输层TCP的可靠性传输。下面不考虑拥塞处理，可靠UDP的简单设计。</p>
        <ol>
            <li>添加seq/ack机制，确保数据发送到对端</li>
            <li>添加发送和接收缓冲区，主要是用户超时重传。</li>
            <li>添加超时重传机制。</li>
        </ol>
        <p><strong>开源程序</strong></p>
        <ol>
            <li>
                <p><strong>RUDP（Reliable User Datagram Protocol）</strong><br>
                    <em><strong>RUDP 提供一组数据服务质量增强机制，如拥塞控制的改进、重发机制及淡化服务器算法等</strong></em>，从而在包丢失和网络拥塞的情况下， RTP
                    客户机（实时位置）面前呈现的就是一个高质量的 RTP 流。在不干扰协议的实时特性的同时，可靠 UDP 的拥塞控制机制允许 TCP 方式下的流控制行为。</p>
            </li>
            <li>
                <p><strong>RTP（Real Time Protocol）</strong><br>
                    <em><strong>RTP为数据提供了具有实时特征的端对端传送服务</strong></em>，如在组播或单播网络服务下的交互式视频音频或模拟数据。<br>
                    应用程序通常在 UDP 上运行 RTP 以便使用其多路结点和校验服务；这两种协议都提供了传输层协议的功能。但是 RTP 可以与其它适合的底层网络或传输协议一起使用。如果底层网络提供组播方式，那么
                    RTP 可以使用该组播表传输数据到多个目的地。<br>
                    RTP 本身并没有提供按时发送机制或其它服务质量（QoS）保证，它依赖于底层服务去实现这一过程。 RTP 并不保证传送或防止无序传送，也不确定底层网络的可靠性。 RTP 实行有序传送， RTP
                    中的序列号允许接收方重组发送方的包序列，同时序列号也能用于决定适当的包位置，例如：在视频解码中，就不需要顺序解码。</p>
            </li>
            <li>
                <p><strong>UDT（UDP-based Data Transfer Protocol）</strong><br>
                    基于UDP的数据传输协议（UDP-basedData Transfer
                    Protocol，简称UDT）是一种互联网数据传输协议。<em><strong>UDT的主要目的是支持高速广域网上的海量数据传输</strong></em>，而互联网上的标准数据传输协议TCP在高带宽长距离网络上性能很差。
                </p>
            </li>
        </ol>
        <p>顾名思义，UDT建于UDP之上，并引入新的拥塞控制和数据可靠性控制机制。UDT是面向连接的双向的应用层协议。它同时支持可靠的数据流传输和部分可靠的数据报传输。由于UDT完全在UDP上实现，它也可以应用在除了高速数据传输之外的其它应用领域，例如点到点技术（P2P），防火墙穿透，多媒体数据传输等等。
        </p>
    </div>
</details>

<details>
    <summary>TCP和UDP的简单对比</summary>
    <table>
        <thead>
            <tr>
                <th style="text-align: left"></th>
                <th style="text-align: left">UDP</th>
                <th style="text-align: left">TCP</th>
            </tr>
        </thead>
        <tbody>
            <tr>
                <td style="text-align: left">是否连接</td>
                <td style="text-align: left">无连接</td>
                <td style="text-align: left">面向连接</td>
            </tr>
            <tr>
                <td style="text-align: left">是否可靠</td>
                <td style="text-align: left">不可靠传输，不使用流量控制和拥塞控制</td>
                <td style="text-align: left">可靠传输，使用流量控制和拥塞控制</td>
            </tr>
            <tr>
                <td style="text-align: left">连接对象个数</td>
                <td style="text-align: left">支持一对一，一对多，多对一和多对多交互通信</td>
                <td style="text-align: left">只能是一对一通信</td>
            </tr>
            <tr>
                <td style="text-align: left">传输方式</td>
                <td style="text-align: left">面向报文</td>
                <td style="text-align: left">面向字节流</td>
            </tr>
            <tr>
                <td style="text-align: left">首部开销</td>
                <td style="text-align: left">首部开销小，仅8字节</td>
                <td style="text-align: left">首部最小20字节，最大60字节</td>
            </tr>
            <tr>
                <td style="text-align: left">适用场景</td>
                <td style="text-align: left">适用于实时应用（IP电话、视频会议、直播等）</td>
                <td style="text-align: left">适用于要求可靠传输的应用，例如文件传输</td>
            </tr>
        </tbody>
    </table>
</details>

<details>
    <summary>TCP和UDP监听同一个端口的实现</summary>
    <div>
        <p>TCP监听办法：</p>
        <pre data-role="codeBlock" data-info="" class="language-"><code>SOCKET sock = socket(TCP)
  sockaddr_in sin
  sin.port = htons(xxx)
  bind(sock, sin)
  listen(sock)
  这样就进入了监听状态，在xxx端口，接下来可以accept了
  </code></pre>
        <p>UDP监听办法</p>
        <pre data-role="codeBlock" data-info="" class="language-"><code>SOCKET sock = socket(UDP)
  sockaddr_in sin
  sin.port = htons(xxx)
  bind(sock, sin)
  这样就进入了监听状态，在xxx端口，接下来可以recvfrom和sentto了
  </code></pre>
        <blockquote>
            <p>一般的，如果有另一个请求（无论是否在同一个进程，无论是tcp还是udp）也监听xxx端口，会在bind处报错（一般情况，具体不展开了）</p>
        </blockquote>
        <p>注意几点：</p>
        <ol>
            <li>端口不是物理概念，仅仅是协议栈中的两个字节</li>
            <li>TCP和UDP的端口完全没有任何关系，完全有可能又有一种XXP基于IP，也有端口的概念，这是完全可能的。</li>
            <li>TCP和UDP传输协议监听同一个端口后，接收数据互不影响，不冲突。因为数据接收时时根据五元组{传输协议，源IP，目的IP，源端口，目的端口}判断接受者的。</li>
        </ol>
    </div>
</details>

<details>
    <summary>HTTP详解之HTTP介绍</summary>
    <div>
        <p><strong>HTTP 简介</strong></p>
        <p>HTTP协议是Hyper Text Transfer Protocol（超文本传输协议）的缩写,是用于从万维网（WWW:World Wide Web ）服务器传输超文本到本地浏览器的传送协议。。</p>
        <p>HTTP是一个基于TCP/IP通信协议来传递数据（HTML 文件, 图片文件, 查询结果等）。</p>
        <p><strong>HTTP 工作原理</strong></p>
        <p>HTTP协议工作于客户端-服务端架构上。浏览器作为HTTP客户端通过URL向HTTP服务端即WEB服务器发送所有请求。</p>
        <p>Web服务器有：Apache服务器，IIS服务器（Internet Information Services）等。</p>
        <p>Web服务器根据接收到的请求后，向客户端发送响应信息。</p>
        <p>HTTP默认端口号为80，但是你也可以改为8080或者其他端口。</p>
        <p><strong>HTTP三点注意事项：</strong></p>
        <ul>
            <li><strong>HTTP是无连接</strong>：无连接的含义是限制每次连接只处理一个请求。服务器处理完客户的请求，并收到客户的应答后，即断开连接。采用这种方式可以节省传输时间。</li>
            <li><strong>HTTP是媒体独立的</strong>：这意味着，只要客户端和服务器知道如何处理的数据内容，任何类型的数据都可以通过HTTP发送。客户端以及服务器指定使用适合的MIME-type内容类型。
                <ul>
                    <li>MIME Type 是该资源的媒体类型，MIME Type 不是个人指定的，是经过互联网（IETF）组织协商，以 RFC（是一系列以编号排定的文件，几乎所有的互联网标准都有收录在其中）
                        的形式作为建议的标准发布在网上的，大多数的 Web 服务器和用户代理都会支持这个规范 (顺便说一句，Email 附件的类型也是通过 MIME Type 指定的)。</li>
                    <li>媒体类型通常通过 HTTP 协议，由 Web 服务器告知浏览器的，更准确地说，是通过 Content-Type 来表示的。例如：Content-Type：text/HTML。</li>
                    <li>通常只有一些卓哉互联网上获得广泛应用的格式才会获得一个 MIME Type，如果是某个客户端自己定义的格式，一般只能以 application/x- 开头。</li>
                </ul>
            </li>
            <li><strong>HTTP是无状态</strong>：HTTP协议是无状态协议。无状态是指协议对于事务处理没有记忆能力。缺少状态意味着如果后续处理需要前面的信息，则它必须重传，这样可能导致每次连接传送的数据量增大。另一方面，在服务器不需要先前信息时它的应答就较快。<br>
                <strong>HTTP流程图</strong></li>
        </ul>
        <p><img src="image/image05.gif" alt=""></p>
    </div>
</details>

<details>
    <summary>HTTP详解之HTTP请求报文</summary>
    <div>
        <p><strong>HTTP请求报文的组成</strong></p>
        <p>HTTP请求报文由3部分组成（请求行+请求头+请求体）</p>
        <p><img src="image/image06.jpeg" alt=""></p>
        <p><strong>请求行</strong><br>
            请求行由：<strong>请求方法</strong> + 空格 + <strong>请求URL</strong> + 空格 + <strong>HTTP协议及版本</strong>
            构成。<em>请求行必须在http请求格式的第一行。</em></p>
        <ul>
            <li><strong>请求方法表</strong></li>
        </ul>
        <table>
            <tbody>
                <tr>
                    <th width="5%">序号</th>
                    <th width="10%">方法</th>
                    <th>描述</th>
                </tr>
                <tr>
                    <td>1</td>
                    <td>GET</td>
                    <td>请求指定的页面信息，并返回实体主体。<br>将请求参数追加在url后面，不安全。<br>url长度限制get请求方式数据的大小。<br>没有请求体</td>
                </tr>
                <tr>
                    <td>2</td>
                    <td>HEAD</td>
                    <td>类似于 GET 请求，不过服务端接收到HEAD请求时只返回响应头，不发送响应内容。所以，如果只需要查看某个页面的状态时，用HEAD更高效，因为省去了传输页面内容的时间。</td>
                </tr>
                <tr>
                    <td>3</td>
                    <td>POST</td>
                    <td>向指定资源提交数据进行处理请求（例如提交表单或者上传文件）。数据被包含在请求体中。POST
                        请求可能会导致新的资源的建立和/或已有资源的修改。<br>请求参数在请求体处，较安全。<br>请求数据大小没有显示<br>只有表单设置为method=“post”才是post请求，其他都是get请求
                    </td>
                </tr>
                <tr>
                    <td>4</td>
                    <td>PUT</td>
                    <td>从客户端向服务器传送的数据取代指定的文档的内容。<br>本质上来讲，
                        PUT和POST极为相似，都是向服务器发送数据，但它们之间有一个重要区别，PUT通常指定了资源的存放位置，而POST则没有，POST的数据存放位置由服务器自己决定。</td>
                </tr>
                <tr>
                    <td>5</td>
                    <td>DELETE</td>
                    <td>请求服务器删除指定的页面。</td>
                </tr>
                <tr>
                    <td>6</td>
                    <td>CONNECT</td>
                    <td>HTTP/1.1 协议中预留给能够将连接改为管道方式的代理服务器。<br>通常用于SSL加密服务器的链接与非加密的HTTP代理服务器的通信。</td>
                </tr>
                <tr>
                    <td>7</td>
                    <td>OPTIONS </td>
                    <td>允许客户端查看服务器的性能。</td>
                </tr>
                <tr>
                    <td>8</td>
                    <td>TRACE</td>
                    <td>回显服务器收到的请求，主要用于测试或诊断。</td>
                </tr>
                <tr>
                    <td>9</td>
                    <td>PATCH</td>
                    <td>是对 PUT 方法的补充，用来对已知资源进行局部更新 。</td>
                </tr>
            </tbody>
        </table>
        <p><strong>请求头</strong><br>
            *<em>为常用请求头</em></p>
        <table>
            <thead>
                <tr>
                    <th>Header</th>
                    <th>解释</th>
                    <th>示例</th>
                </tr>
            </thead>
            <tbody>
                <tr>
                    <td><strong>*Accept</strong></td>
                    <td>指定客户端能够接收的内容类型。告诉服务端,该请求所能支持的响应数据类型,专业术语称为MIME 类型(文件类型的一种描述方式)</td>
                    <td>Accept: text/plain, text/html</td>
                </tr>
                <tr>
                    <td>Accept-Charset</td>
                    <td>浏览器可以接受的字符编码集。</td>
                    <td>Accept-Charset: iso-8859-5</td>
                </tr>
                <tr>
                    <td>*Accept-Encoding</td>
                    <td>指定浏览器可以支持的web服务器返回内容压缩编码类型。</td>
                    <td>Accept-Encoding: compress, gzip</td>
                </tr>
                <tr>
                    <td>*Accept-Language</td>
                    <td>浏览器可接受的语言</td>
                    <td>Accept-Language: en,zh</td>
                </tr>
                <tr>
                    <td>Accept-Ranges</td>
                    <td>可以请求网页实体的一个或者多个子范围字段</td>
                    <td>Accept-Ranges: bytes</td>
                </tr>
                <tr>
                    <td>Authorization</td>
                    <td>HTTP授权的授权证书</td>
                    <td>Authorization: Basic QWxhZGRpbjpvcGVuIHNlc2FtZQ==</td>
                </tr>
                <tr>
                    <td><strong>*Cache-Control</strong></td>
                    <td>指定请求和响应遵循的缓存机制</td>
                    <td>Cache-Control: no-cache</td>
                </tr>
                <tr>
                    <td><strong>*Connection</strong></td>
                    <td>表示是否需要持久连接。（HTTP 1.1默认进行持久连接）</td>
                    <td>Connection: close</td>
                </tr>
                <tr>
                    <td>Cookie</td>
                    <td>HTTP请求发送时，会把保存在该请求域名下的所有cookie值一起发送给web服务器。</td>
                    <td>Cookie: $Version=1; Skin=new;</td>
                </tr>
                <tr>
                    <td><strong>*Content-Length</strong></td>
                    <td>请求的内容长度</td>
                    <td>Content-Length: 348</td>
                </tr>
                <tr>
                    <td><strong>*Content-Type</strong></td>
                    <td>请求的与实体对应的MIME信息</td>
                    <td>Content-Type: application/x-www-form-urlencoded</td>
                </tr>
                <tr>
                    <td><strong>*Date</strong></td>
                    <td>请求发送的日期和时间</td>
                    <td>Date: Tue, 15 Nov&nbsp;2010 08:12:31 GMT</td>
                </tr>
                <tr>
                    <td>Expect</td>
                    <td>请求的特定的服务器行为</td>
                    <td>Expect: 100-continue</td>
                </tr>
                <tr>
                    <td>From</td>
                    <td>发出请求的用户的Email</td>
                    <td>From: user@email.com</td>
                </tr>
                <tr>
                    <td><strong>*Host</strong></td>
                    <td>指定请求的服务器的域名和端口号</td>
                    <td>Host: www.zcmhi.com</td>
                </tr>
                <tr>
                    <td>If-Match</td>
                    <td>只有请求内容与实体相匹配才有效</td>
                    <td>If-Match: “737060cd8c284d8af7ad3082f209582d”</td>
                </tr>
                <tr>
                    <td>*If-Modified-Since</td>
                    <td>如果请求的部分在指定时间之后被修改则请求成功，未被修改则返回304代码。控制浏览器缓存</td>
                    <td>If-Modified-Since: Sat, 29 Oct 2010 19:43:31 GMT</td>
                </tr>
                <tr>
                    <td>If-None-Match</td>
                    <td>如果内容未改变返回304代码，参数为服务器先前发送的Etag，与服务器回应的Etag比较判断是否改变</td>
                    <td>If-None-Match: “737060cd8c284d8af7ad3082f209582d”</td>
                </tr>
                <tr>
                    <td>If-Range</td>
                    <td>如果实体未改变，服务器发送客户端丢失的部分，否则发送整个实体。参数也为Etag</td>
                    <td>If-Range: “737060cd8c284d8af7ad3082f209582d”</td>
                </tr>
                <tr>
                    <td>If-Unmodified-Since</td>
                    <td>只在实体在指定时间之后未被修改才请求成功</td>
                    <td>If-Unmodified-Since: Sat, 29 Oct 2010 19:43:31 GMT</td>
                </tr>
                <tr>
                    <td>Max-Forwards</td>
                    <td>限制信息通过代理和网关传送的时间</td>
                    <td>Max-Forwards: 10</td>
                </tr>
                <tr>
                    <td>Pragma</td>
                    <td>用来包含实现特定的指令</td>
                    <td>Pragma: no-cache</td>
                </tr>
                <tr>
                    <td>Proxy-Authorization</td>
                    <td>连接到代理的授权证书</td>
                    <td>Proxy-Authorization: Basic QWxhZGRpbjpvcGVuIHNlc2FtZQ==</td>
                </tr>
                <tr>
                    <td>Range</td>
                    <td>只请求实体的一部分，指定范围</td>
                    <td>Range: bytes=500-999</td>
                </tr>
                <tr>
                    <td><strong>*Referer</strong></td>
                    <td>表示这个请求是从哪个url跳过来的,通过百度来搜索淘宝网,那么在进入淘宝网的请求报文中,Referer的值就是:www.baidu.com。如果是直接访问就不会有这个头。常用于防盗链</td>
                    <td>Referer: http://www.zcmhi.com/archives/71.html</td>
                </tr>
                <tr>
                    <td>TE</td>
                    <td>客户端愿意接受的传输编码，并通知服务器接受接受尾加头信息</td>
                    <td>TE: trailers,deflate;q=0.5</td>
                </tr>
                <tr>
                    <td>Upgrade</td>
                    <td>向服务器指定某种传输协议以便服务器进行转换（如果支持）</td>
                    <td>Upgrade: HTTP/2.0, SHTTP/1.3, IRC/6.9, RTA/x11</td>
                </tr>
                <tr>
                    <td><strong>*User-Agent</strong></td>
                    <td>浏览器通知服务器，客户端浏览器与操作系统相关信息</td>
                    <td>User-Agent: Mozilla/5.0 (Linux; X11)</td>
                </tr>
                <tr>
                    <td>Via</td>
                    <td>通知中间网关或代理服务器地址，通信协议</td>
                    <td>Via: 1.0 fred, 1.1 nowhere.com (Apache/1.1)</td>
                </tr>
                <tr>
                    <td>Warning</td>
                    <td>关于消息实体的警告信息</td>
                    <td>Warn: 199 Miscellaneous warning</td>
                </tr>
            </tbody>
        </table>
    </div>
</details>

<details>
    <summary>HTTP详解之HTTP响应报文</summary>
    <div>
        <p><strong>HTTP响应报文的组成</strong><br>
            HTTP的响应报文也由三部分组成（响应行+响应头+响应体）</p>
        <p><strong>响应行</strong></p>
        <p>响应行由 <strong>报文协议及版本</strong> + 空格 + <strong>HTTP响应状态码</strong> + 空格 + <strong>状态码名称（描述）</strong></p>
        <p><img src="image/image07.jpeg" alt=""></p>
        <p><strong>响应头</strong></p>
        <table>
            <thead>
                <tr>
                    <th>Header</th>
                    <th>解释</th>
                    <th>示例</th>
                </tr>
            </thead>
            <tbody>
                <tr>
                    <td>Accept-Ranges</td>
                    <td>表明服务器是否支持指定范围请求及哪种类型的分段请求</td>
                    <td>Accept-Ranges: bytes</td>
                </tr>
                <tr>
                    <td>Age</td>
                    <td>从原始服务器到代理缓存形成的估算时间（以秒计，非负）</td>
                    <td>Age: 12</td>
                </tr>
                <tr>
                    <td>Allow</td>
                    <td>对某网络资源的有效的请求行为，不允许则返回405</td>
                    <td>Allow: GET, HEAD</td>
                </tr>
                <tr>
                    <td>Cache-Control</td>
                    <td>告诉所有的缓存机制是否可以缓存及哪种类型</td>
                    <td>Cache-Control: no-cache</td>
                </tr>
                <tr>
                    <td>Content-Encoding</td>
                    <td>web服务器支持的返回内容压缩编码类型。</td>
                    <td>Content-Encoding: gzip</td>
                </tr>
                <tr>
                    <td>Content-Language</td>
                    <td>响应体的语言</td>
                    <td>Content-Language: en,zh</td>
                </tr>
                <tr>
                    <td>Content-Length</td>
                    <td>响应体的长度</td>
                    <td>Content-Length: 348</td>
                </tr>
                <tr>
                    <td>Content-Location</td>
                    <td>请求资源可替代的备用的另一地址</td>
                    <td>Content-Location: /index.htm</td>
                </tr>
                <tr>
                    <td>Content-MD5</td>
                    <td>返回资源的MD5校验值</td>
                    <td>Content-MD5: Q2hlY2sgSW50ZWdyaXR5IQ==</td>
                </tr>
                <tr>
                    <td>Content-Range</td>
                    <td>在整个返回体中本部分的字节位置</td>
                    <td>Content-Range: bytes 21010-47021/47022</td>
                </tr>
                <tr>
                    <td>Content-Type</td>
                    <td>返回内容的MIME类型</td>
                    <td>Content-Type: text/html; charset=utf-8</td>
                </tr>
                <tr>
                    <td>Date</td>
                    <td>原始服务器消息发出的时间</td>
                    <td>Date: Tue, 15 Nov 2010 08:12:31 GMT</td>
                </tr>
                <tr>
                    <td>ETag</td>
                    <td>请求变量的实体标签的当前值</td>
                    <td>ETag: “737060cd8c284d8af7ad3082f209582d”</td>
                </tr>
                <tr>
                    <td>Expires</td>
                    <td>响应过期的日期和时间</td>
                    <td>Expires: Thu, 01 Dec 2010 16:00:00 GMT</td>
                </tr>
                <tr>
                    <td>*Last-Modified</td>
                    <td>请求资源的最后修改时间<br>服务器通知浏览器，文件的最后修改时间。与If-Modified-Since一起使用。</td>
                    <td>Last-Modified: Tue, 15 Nov 2010 12:45:26 GMT</td>
                </tr>
                <tr>
                    <td><strong>*Location</strong></td>
                    <td>用来重定向接收方到非请求URL的位置来完成请求或标识新的资源<br>指定响应的路径，需要与状态码302配合使用，完成跳转。</td>
                    <td>Location: http://www.zcmhi.com/archives/94.html</td>
                </tr>
                <tr>
                    <td>Pragma</td>
                    <td>包括实现特定的指令，它可应用到响应链上的任何接收方</td>
                    <td>Pragma: no-cache</td>
                </tr>
                <tr>
                    <td>Proxy-Authenticate</td>
                    <td>它指出认证方案和可应用到代理的该URL上的参数</td>
                    <td>Proxy-Authenticate: Basic</td>
                </tr>
                <tr>
                    <td>refresh</td>
                    <td>应用于重定向或一个新的资源被创造，在5秒之后重定向（由网景提出，被大部分浏览器支持）</td>
                    <td>
                        <div>&nbsp;</div>
                        <p>&nbsp;</p>
                        <div id="_mcePaste">Refresh: 5; url=</div>
                        <div>http://www.zcmhi.com/archives/94.html</div>
                    </td>
                </tr>
                <tr>
                    <td>Retry-After</td>
                    <td>如果实体暂时不可取，通知客户端在指定时间之后再次尝试</td>
                    <td>Retry-After: 120</td>
                </tr>
                <tr>
                    <td>Server</td>
                    <td>web服务器软件名称</td>
                    <td>Server: Apache/1.3.27 (Unix) (Red-Hat/Linux)</td>
                </tr>
                <tr>
                    <td><strong>*Set-Cookie</strong></td>
                    <td>设置Http Cookie<br>与会话相关技术。服务器向浏览器写入cookie</td>
                    <td>Set-Cookie: UserID=JohnDoe; Max-Age=3600; Version=1</td>
                </tr>
                <tr>
                    <td>Trailer</td>
                    <td>指出头域在分块传输编码的尾部存在</td>
                    <td>Trailer: Max-Forwards</td>
                </tr>
                <tr>
                    <td>Transfer-Encoding</td>
                    <td>文件传输编码</td>
                    <td>Transfer-Encoding:chunked</td>
                </tr>
                <tr>
                    <td>Vary</td>
                    <td>告诉下游代理是使用缓存响应还是从原始服务器请求</td>
                    <td>Vary: *</td>
                </tr>
                <tr>
                    <td>Via</td>
                    <td>告知代理客户端响应是通过哪里发送的</td>
                    <td>Via: 1.0 fred, 1.1 nowhere.com (Apache/1.1)</td>
                </tr>
                <tr>
                    <td>Warning</td>
                    <td>警告实体可能存在的问题</td>
                    <td>Warning: 199 Miscellaneous warning</td>
                </tr>
                <tr>
                    <td>WWW-Authenticate</td>
                    <td>表明客户端请求实体应该使用的授权方案</td>
                    <td>WWW-Authenticate: Basic</td>
                </tr>
            </tbody>
        </table>
    </div>
</details>

<details>
    <summary>HTTP状态码</summary>
    <div>
        <p><strong>HTTP状态码分类</strong><br>
            HTTP状态码由三个十进制数字组成，第一个十进制数字定义了状态码的类型，后两个数字没有分类的作用。HTTP状态码共分为5种类型：</p>
        <table>
            <tbody>
                <tr>
                    <th>分类</th>
                    <th>分类描述</th>
                </tr>
                <tr>
                    <td>1**</td>
                    <td>信息，服务器收到请求，需要请求者继续执行操作</td>
                </tr>
                <tr>
                    <td>2**</td>
                    <td>成功，操作被成功接收并处理</td>
                </tr>
                <tr>
                    <td>3**</td>
                    <td>重定向，需要进一步的操作以完成请求</td>
                </tr>
                <tr>
                    <td>4**</td>
                    <td>客户端错误，请求包含语法错误或无法完成请求</td>
                </tr>
                <tr>
                    <td>5**</td>
                    <td>服务器错误，服务器在处理请求的过程中发生了错误</td>
                </tr>
            </tbody>
        </table>
        <p><strong>HTTP状态码列表</strong></p>
        <table>
            <tbody>
                <tr>
                    <th>状态码</th>
                    <th>状态码英文名称</th>
                    <th>中文描述</th>
                </tr>
                <tr>
                    <td>100</td>
                    <td>Continue</td>
                    <td>继续。<a href="http://www.dreamdu.com/webbuild/client_vs_server/">客户端</a>应继续其请求</td>
                </tr>
                <tr>
                    <td>101</td>
                    <td>Switching Protocols</td>
                    <td>切换协议。服务器根据客户端的请求切换协议。只能切换到更高级的协议，例如，切换到HTTP的新版本协议</td>
                </tr>
                <tr>
                    <td colspan="3"></td>
                </tr>
                <tr>
                    <td>200</td>
                    <td>OK</td>
                    <td>请求成功。一般用于GET与POST请求</td>
                </tr>
                <tr>
                    <td>201</td>
                    <td>Created</td>
                    <td>已创建。成功请求并创建了新的资源</td>
                </tr>
                <tr>
                    <td>202</td>
                    <td>Accepted</td>
                    <td>已接受。已经接受请求，但未处理完成</td>
                </tr>
                <tr>
                    <td>203</td>
                    <td>Non-Authoritative Information</td>
                    <td>非授权信息。请求成功。但返回的meta信息不在原始的服务器，而是一个副本</td>
                </tr>
                <tr>
                    <td>204</td>
                    <td>No Content</td>
                    <td>无内容。服务器成功处理，但未返回内容。在未更新网页的情况下，可确保浏览器继续显示当前文档</td>
                </tr>
                <tr>
                    <td>205</td>
                    <td>Reset Content</td>
                    <td>重置内容。服务器处理成功，用户终端（例如：浏览器）应重置文档视图。可通过此返回码清除浏览器的表单域</td>
                </tr>
                <tr>
                    <td>206</td>
                    <td>Partial Content</td>
                    <td>部分内容。服务器成功处理了部分GET请求</td>
                </tr>
                <tr>
                    <td colspan="3"></td>
                </tr>
                <tr>
                    <td>300</td>
                    <td>Multiple Choices</td>
                    <td>多种选择。请求的资源可包括多个位置，相应可返回一个资源特征与地址的列表用于用户终端（例如：浏览器）选择</td>
                </tr>
                <tr>
                    <td>301</td>
                    <td>Moved Permanently</td>
                    <td>永久移动。请求的资源已被永久的移动到新URI，返回信息会包括新的URI，浏览器会自动定向到新URI。今后任何新的请求都应使用新的URI代替</td>
                </tr>
                <tr>
                    <td>302</td>
                    <td>Found</td>
                    <td>临时移动。与301类似。但资源只是临时被移动。客户端应继续使用原有URI</td>
                </tr>
                <tr>
                    <td>303</td>
                    <td>See Other</td>
                    <td>查看其它地址。与301类似。使用GET和POST请求查看</td>
                </tr>
                <tr>
                    <td>304</td>
                    <td>Not Modified</td>
                    <td>未修改。所请求的资源未修改，服务器返回此状态码时，不会返回任何资源。客户端通常会缓存访问过的资源，通过提供一个头信息指出客户端希望只返回在指定日期之后修改的资源</td>
                </tr>
                <tr>
                    <td>305</td>
                    <td>Use Proxy</td>
                    <td>使用代理。所请求的资源必须通过代理访问</td>
                </tr>
                <tr>
                    <td>306</td>
                    <td>Unused</td>
                    <td>已经被废弃的HTTP状态码</td>
                </tr>
                <tr>
                    <td>307</td>
                    <td>Temporary Redirect</td>
                    <td>临时重定向。与302类似。使用GET请求重定向</td>
                </tr>
                <tr>
                    <td colspan="3"></td>
                </tr>
                <tr>
                    <td>400</td>
                    <td>Bad Request</td>
                    <td>客户端请求的语法错误，服务器无法理解</td>
                </tr>
                <tr>
                    <td>401</td>
                    <td>Unauthorized</td>
                    <td>请求要求用户的身份认证</td>
                </tr>
                <tr>
                    <td>402</td>
                    <td>Payment Required</td>
                    <td>保留，将来使用</td>
                </tr>
                <tr>
                    <td>403</td>
                    <td>Forbidden</td>
                    <td>服务器理解请求客户端的请求，但是拒绝执行此请求</td>
                </tr>
                <tr>
                    <td>404</td>
                    <td>Not Found</td>
                    <td>服务器无法根据客户端的请求找到资源（网页）。通过此代码，网站设计人员可设置"您所请求的资源无法找到"的个性页面</td>
                </tr>
                <tr>
                    <td>405</td>
                    <td>Method Not Allowed</td>
                    <td>客户端请求中的方法被禁止</td>
                </tr>
                <tr>
                    <td>406</td>
                    <td>Not Acceptable</td>
                    <td>服务器无法根据客户端请求的内容特性完成请求</td>
                </tr>
                <tr>
                    <td>407</td>
                    <td>Proxy Authentication Required</td>
                    <td>请求要求代理的身份认证，与401类似，但请求者应当使用代理进行授权</td>
                </tr>
                <tr>
                    <td>408</td>
                    <td>Request Time-out</td>
                    <td>服务器等待客户端发送的请求时间过长，超时</td>
                </tr>
                <tr>
                    <td>409</td>
                    <td>Conflict</td>
                    <td>服务器完成客户端的 PUT 请求时可能返回此代码，服务器处理请求时发生了冲突</td>
                </tr>
                <tr>
                    <td>410</td>
                    <td>Gone</td>
                    <td>客户端请求的资源已经不存在。410不同于404，如果资源以前有现在被永久删除了可使用410代码，网站设计人员可通过301代码指定资源的新位置</td>
                </tr>
                <tr>
                    <td>411</td>
                    <td>Length Required</td>
                    <td>服务器无法处理客户端发送的不带Content-Length的请求信息</td>
                </tr>
                <tr>
                    <td>412</td>
                    <td>Precondition Failed</td>
                    <td>客户端请求信息的先决条件错误</td>
                </tr>
                <tr>
                    <td>413</td>
                    <td>Request Entity Too Large</td>
                    <td>由于请求的实体过大，服务器无法处理，因此拒绝请求。为防止客户端的连续请求，服务器可能会关闭连接。如果只是服务器暂时无法处理，则会包含一个Retry-After的响应信息</td>
                </tr>
                <tr>
                    <td>414</td>
                    <td>Request-URI Too Large</td>
                    <td>请求的URI过长（URI通常为网址），服务器无法处理</td>
                </tr>
                <tr>
                    <td>415</td>
                    <td>Unsupported Media Type</td>
                    <td>服务器无法处理请求附带的媒体格式</td>
                </tr>
                <tr>
                    <td>416</td>
                    <td>Requested range not satisfiable</td>
                    <td>客户端请求的范围无效</td>
                </tr>
                <tr>
                    <td>417</td>
                    <td>Expectation Failed</td>
                    <td>服务器无法满足Expect的请求头信息</td>
                </tr>
                <tr>
                    <td colspan="3"></td>
                </tr>
                <tr>
                    <td>500</td>
                    <td>Internal Server Error</td>
                    <td>服务器内部错误，无法完成请求</td>
                </tr>
                <tr>
                    <td>501</td>
                    <td>Not Implemented</td>
                    <td>服务器不支持请求的功能，无法完成请求</td>
                </tr>
                <tr>
                    <td>502</td>
                    <td>Bad Gateway</td>
                    <td>作为网关或者代理工作的服务器尝试执行请求时，从远程服务器接收到了一个无效的响应</td>
                </tr>
                <tr>
                    <td>503</td>
                    <td>Service Unavailable</td>
                    <td>由于超载或系统维护，服务器暂时的无法处理客户端的请求。延时的长度可包含在服务器的Retry-After头信息中</td>
                </tr>
                <tr>
                    <td>504</td>
                    <td>Gateway Time-out</td>
                    <td>充当网关或代理的服务器，未及时从远端服务器获取请求</td>
                </tr>
                <tr>
                    <td>505</td>
                    <td>HTTP Version not supported</td>
                    <td>服务器不支持请求的HTTP协议的版本，无法完成处理</td>
                </tr>
            </tbody>
        </table>
    </div>
</details>

<details>
    <summary>HTTP协议以及如何让HTTP协议“有状态”？</summary>
    <div>
        <p><strong>一、HTTP协议的状态</strong><br>
            HTTP是一种无状态协议，即服务器不保留与客户交易时的任何状态。</p>
        <p>也就是说，上一次的请求对这次的请求没有任何影响，服务端也不会对客户端上一次的请求进行任何记录处理。</p>
        <p><strong>二、HTTP协议的无状态性带来的问题：</strong><br>
            用户登录后，切换到其他界面，进行操作，服务器端是无法判断是哪个用户登录的。 每次进行页面跳转的时候，得重新登录。</p>
        <p><strong>三、解决方案</strong><br>
            既然HTTP协议是无状态的，不会记录用户信息，那么怎么样才能让HTTP协议记录用户信息呢？换句话说，服务器怎么判断发来HTTP请求的是哪个用户？</p>
        <p>于是，两种用于保持HTTP状态的技术就应运而生了，一个是 Cookie，而另一个则是 Session。</p>
        <ol>
            <li>
                <p>Cookie<br>
                    Cookie 是客户端的存储空间，由浏览器来维持。具体来说 cookie 机制采用的是在客户端保持状态的方案。<br>
                    Cookie，有时也用其复数形式Cookies，指某些网站为了辨别用户身份、进行 session 跟踪而储存在用户本地终端上的数据（通常经过加密）。</p>
                <ul>
                    <li>Cookie 的实现过程：<br>
                        Cookie 会根据从服务器端发送的响应报文内的一个叫做 Set-Cookie 的首部字段信息，通知客户端保存 Cookie，当下次客户端再往该服务器发送请求时，客户端会自动在请求报文中加入
                        Cookie 值后发送出去。<br>
                        也就是 Cookie 是服务器生成的，但是发送给客户端，并且由客户端来保存。每次请求加上 Cookie就行了。服务器端发现客户端发送过来的 Cookie
                        后，会去检查究竟是从哪一个客户端发来的连接请求，然后对比服务器上的记录，最后得到之前的状态信息。</li>
                </ul>
            </li>
            <li>
                <p>Session<br>
                    Session，中文经常翻译为会话，其本来的含义是指有始有终的一系列动作/消息，比如打电话是从拿起电话拨号到挂断电话这中间的一系列过程可以称之为一个 Session。然而当 Session
                    一词与网络协议相关联时，它又往往隐含了“面向连接”或“保持状态”这样两个含义。<br>
                    Session 是另一种记录客户状态的机制，不同的是 Cookie 保存在客户端浏览器中，而 Session 保存在服务器上。<br>
                    客户端浏览器访问服务器的时候，服务器把客户端信息以某种形式记录在服务器上，这就是 Session。客户端浏览器再次访问时，只需要从该 Session 中查找该客户的状态就可以了。<br>
                    虽然 Session 保存在服务器，对客户端是透明的，它的正常运行仍然需要客户端浏览器的支持。这是因为 Session 需要使用Cookie 作为识别标志。HTTP协议是无状态的，Session
                    不能依据HTTP连接来判断是否为同一客户，因此服务器向客户端浏览器发送一个名为 JSESSIONID 的 Cookie，它的值为该 Session 的
                    id（即放在HTTP响应报文头部信息里的Set-Cookie）。Session依据该 Cookie 来识别是否为同一用户。</p>
            </li>
            <li>
                <p>Cookie 和 Session 的区别</p>
                <ul>
                    <li>
                        <p>Cookie 数据存放在客户的浏览器上，Session 数据放在服务器上；</p>
                    </li>
                    <li>
                        <p>Cookie 不是很安全，别人可以分析存放在本地的COOKIE并进行COOKIE欺骗，考虑到安全应当使用 Session ；</p>
                    </li>
                    <li>
                        <p>Session 会在一定时间内保存在服务器上。当访问增多，会比较占用你服务器的性能。考虑到减轻服务器性能方面，应当使用COOKIE；</p>
                    </li>
                    <li>
                        <p>单个Cookie 在客户端的限制是3K，就是说一个站点在客户端存放的COOKIE不能超过3K；</p>
                    </li>
                </ul>
            </li>
            <li>
                <p>总结<br>
                    如果说 Cookie 机制是通过检查客户身上的“通行证”来确定客户身份的话，那么Session机制就是通过检查服务器上的“客户明细表”来确认客户身份。Session
                    相当于程序在服务器上建立的一份客户档案，客户来访的时候只需要查询客户档案表就可以了。<br>
                    Cookie 和 Session 的方案虽然分别属于客户端和服务端，但是服务端的 Session 的实现对客户端的 Cookie 有依赖关系的，上面我讲到服务端执行 Session 机制时候会生成
                    Session 的 id 值，这个 id 值会发送给客户端，客户端每次请求都会把这个 id 值放到 http 请求的头部发送给服务端，而这个 id 值在客户端会保存下来，保存的容器就是
                    Cookie，因此当我们完全禁掉浏览器的Cookie的时候，服务端的Session也会不能正常使用。</p>
            </li>
        </ol>
        <p><strong>问：客户端浏览器将 Cookie 功能禁用，或者不支持 Cookie 怎么办？</strong></p>
        <p>一般这种情况下，会使用一种叫做 URL 重写的技术来进行会话跟踪，即每次HTTP交互，URL后面都会被附加上一个诸如 sid=xxxxx 这样的参数，服务端据此来识别用户。</p>
        <p><strong>Cookie 和 Session 应用场景：</strong></p>
        <ul>
            <li>
                <p>登录网站，今输入用户名密码登录了，第二天再打开很多情况下就直接打开了。这个时候用到的一个机制就是cookie。</p>
            </li>
            <li>
                <p>session一个场景是购物车，添加了商品之后客户端处可以知道添加了哪些商品，而服务器端如何判别呢，所以也需要存储一些信息就用到了session。</p>
            </li>
        </ul>
    </div>
</details>

<details>
    <summary>HTTP传输的文件过大怎么办</summary>
    <div>
        <p><strong>如果传输的文件过大怎么办?</strong></p>
        <p>服务器上返回的资源文件比较大，比如有些 js 文件大小可能就有几兆。文件过大就会影响传 输的效率，同时也会带来带宽的消耗。怎么办呢？</p>
        <ol>
            <li>
                <p>常见的手段是，对文件进行压缩，减少文件大小。那压缩和解压缩的流程怎么实现呢？ 首先服务端需要能支持文件的压缩功能，其次浏览器能够针对被压缩的文件进行解压缩。浏 览器可以指定
                    Accept-Encoding 来高速服务器我当前支持的编码类型 Accept-Encoding:gzip,deflate
                    那服务端会根据支持的编码类型，选择合适的类型进行压缩。常见的编码方式有：gzip/deflate</p>
            </li>
            <li>
                <p>分割传输 在传输大容量数据时，通过把数据分割成多块，能够让浏览器逐步显示页面。这种把实体主 体分块的功能称为分块传输编码（Chunked Transfer Coding）。</p>
            </li>
        </ol>
    </div>
</details>