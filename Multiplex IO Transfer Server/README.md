三种模型性能分析
1. select
* select能监听的文件描述符个数受限于FD_SETSIZE,一般为1024，单纯改变进程打开的文件描述符个数并不能改变select监听文件个数
* 解决1024以下客户端时使用select是很合适的，但如果链接客户端过多，select采用的是轮询模型，会大大降低服务器响应效率，不应在select上投入更多精力
2. poll
* 与 select 不同的是，poll 使用链表保存文件描述符，没有了监视文件数量的限制
3. epoll
* epoll是Linux下多路复用IO接口select/poll的增强版本，能显著提高程序在大量并发连接中只有少量活跃的情况下的系统CPU利用率
* 复用文件描述符集合来传递结果，而不用迫使开发者每次等待事件之前都必须重新准备要被侦听的文件描述符集合
* 在获取事件时，无须遍历整个被侦听的描述符集，只要遍历那些被内核IO事件异步唤醒而加入Ready队列的描述符集合就行
* 目前epell是linux大规模并发网络程序中的热门首选模型
* epoll除了提供select/poll那种IO事件的电平触发（Level Triggered）外，还提供了边沿触发（Edge Triggered），这就使得用户空间程序有可能缓存IO状态，减少epoll_wait/epoll_pwait的调用，提高应用程序效率
