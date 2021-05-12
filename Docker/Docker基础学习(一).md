#  Dokcer基础学习(一)

初识Docker。

[TOC]

## 1.  什么是Docker

### 1.1  Linux容器技术(LXC)

众所周知，CPU、内存、I/O、网络等都称之为系统资源，而Linux内核有一套机制来管理其所拥有的这些资源，这套机制的核心被称之为CGroups和Namespaces。

CGroups可以限制、记录、调整进程组所使用的物理资源。比如说：使用CGroups可以给某项进程组多分配一些CPU使用周期。同样也可以通过CGroups限制某项进程组可使用的内存上限，一旦达到上限，内核就会发出Out Of Memory错误。

Namespaces则是另外一个重要的资源隔离机制。Namespaces将进程、进程组、IPC、网络、内存等资源都变得不再是全局性资源，而是将这些资源从内核层面属于某个特定的Namespace。在不同的Namespace之间，这些资源是相互透明、不可见的。比如说，A用户登录系统后，可以查看到B用户的进程PID。虽说A用户不能杀死B用户的进程，但A和B却能相互感知。但假如A用户在Namespace-A中，B用户在Namespace-B中，虽然A和B仍然共存于同一个Linux操作系统当中，但A却无法感知到B。在这种情况下，Linux内核不但将Namespace相互隔离，而且将所分配的资源牢牢固定在各自空间之中。

而LXC就是基于Linux内核通过调用CGroups和Namespaces，来实现容器轻量级虚拟化的一项技术，与此同时，LXC也是一组面向Linux内核容器的用户态API接口。用户通过LXC提供的资源限制和隔离功能，可以创建一套完整并且相互隔离的虚拟应用运行环境

在LXC的基础上，Docker进一步优化了容器的使用体验，让它进入了寻常百姓家。

### 1.2  Docker的开源背景

Docker是基于Go语言实现的开源容器项目，诞生于2013年年初，最初发起者是dotCloud公司。dotCloud公司后来也直接改名为Docker Inc，并专注于Docker相关技术和产品的开发。

Docker项目已加入了Linux基金会，并遵循Apache2.0协议，全部开源代码均在 `https://github.com/docker/docker `上进行维护。

Docker的构想是要实现<b>“Build，Ship and Run Any App，Anywhere”</b>，即通过对应用的封装（Packaging）、分发（Distribution）、部署（Deployment）、运行（Runtime）生命周期进行管理，达到应用组件“一次封装，到处运行”的目的。

## 2. Docker现状

### 2.1  Docker与传统虚拟化比较

![alt DVC](D:\notes\Docker\DVC.png)

| 特性     | 容器               | 虚拟机     |
| -------- | ------------------ | ---------- |
| 启动速度 | 秒级               | 分钟级     |
| 性能     | 接近原生           | 较弱       |
| 内存代价 | 很小               | 较多       |
| 硬盘使用 | 一般为MB           | 一般为GB   |
| 运行密度 | 单机支持上千个容器 | 一般几十个 |
| 隔离性   | 安全隔离           | 完全隔离   |
| 迁移性   | 优秀               | 一般       |

### 2.2  应用范围

- 云平台

  Docker屏蔽硬件层的差异，提供了统一的用户应用层。通过Docker，企业用户所提供的产品可以自由地在“混合云”之间移动，而这种迁移所付出的成本却是极低的。

- Devops

  Docker能作为一个封闭的运行环境在各部门之间流转，这无形当中就降低了各部门之间的沟通成本。只要各部门使用相同的数据镜像，就不会出现环境差异，同样也就不会出现代码运行差异。这使得在企业内部，产品开发团队可以将精力最大化地集中于产品本身，而不是流程。

### 2.3  优缺点

<b>优点：</b>

- Docker资源利用率比传统虚拟机要高版本可控，组件可复用
- 共享镜像
- 更快速的交付和部署
- 更轻松的迁移和扩展

<b>缺点：</b>

- Docker是基于Linux 64bit的，无法在32bit的linux/Windows/unix环境下使用。
- LXC是基于Cgroup等Linux kernel功能的，因此Container的Guest系统只能是Linux base的。
- 隔离性相比KVM之类的虚拟化方案还是有些欠缺，所有container公用一部分的运行库。
- docker对disk的管理比较有限。
- container随着用户进程的停止而销毁，container中的log等用户数据不便收集。

