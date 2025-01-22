---
layout: post
title:  "Keepalived实现高可用"
date:   2025-01-22 21:35:00 +0800
categories: default
---

Keepalived实现了VRRP协议, 通过在多个机器之前争抢同一个虚拟IP, 从而实现了高可用. 

<img class="image" src="/assets/images/Keepalived.png">

在上图展示的局域网中, 三台机器争抢了同一个虚拟IP, 通过VRRP协议, 保证了只有一台机器能够获得虚拟IP: 192.168.0.100, 其他机器都是standby状态.
当master机器宕机时, 其他机器会争抢虚拟IP.

具体安装过程可以参考官方文档.

VRRP的原理是: 通过arp广播, 优先级高的机器告诉同一局域网内的所有机器, 192.168.0.100的mac地址是自己的mac地址, 从而实现抢占. 优先级低的机器主动放弃.

而在虚拟化平台上的虚拟机中应用Keepalived, 可能会有arp广播不生效的问题. 例如浪潮的虚拟化平台, 或者VMware的虚拟化平台, 模拟的交换机并不会尊重ARP广播的内容.
尤其是arp cache时间过长, 导致master下线之后, 虽然其他机器会获得虚拟IP, 但是交换机并不会更新arp cache, 从而导致无法访问虚拟IP.

解决方法是: 在Keepalived配置文件中, 使用use_vmac, 从而绕过交换机的arp cache. 

use_vmac的原理是使用唯一的虚拟mac地址, 绑定在虚拟IP上, 虽然多个机器均有相同虚拟mac的网卡, 但是只有master机器会处理相关的数据包:

```shell
vrrp_instance VI_1 {
    ...
    advert_int 1
    use_vmac
    ...
}
```