# 在Linux主机上部署 EDB Postgres Distributed集群
可以通过多种方式进行PGD的安装和部署，比如可以通过TPA方式、可以通过ansible方式，也可以通过手工方式进行安装和部署，按照EDB官方介绍，建议的安装部署方式是通过TPA方式进行，为此本文我们就以TPA方式进行PGD的安装部署演示，其它方式的安装部署我们可以在后续的文章中再做介绍。
PGD可以运行在多种平台和环境中，PGD可以运行中docker环境中，可以运行在AWS云计算环境，可以运行在虚拟机环境中，也可以运行在裸机环境中。PGD支持的系统平台是主要是Linux，支持centos7（x86_64,ppc64）、Debian 10,11（x86_64），RHEL7～9（x86_64,ppc64），Rocky/Alam/Oracle Linux 8,9（x86_64），SLES12,15（x86_64,ppc64），Ubuntu 20.04 LTS（x86_64）,Ubuntu 22.04（x86_64）等。
## 一、	TPA 和 PGD 简介
我们创建 TPA 是为了轻松重复安装和管理各种 Postgres 配置。 TPA 协调创建和部署 Postgres。上篇文章中我们已经对TPA做过介绍。
PGD 是 Postgres 的多主复制实现，旨在实现高性能和可用性。 PGD 的安装由 TPA 编排。通过TPA 可以生成 PGD集群的配置文件。
TPA Linux 主机选项允许任何云或 VM 平台的用户使用 TPA 来配置PGD。TPA 所需要的只是为目标系统配置Linux 操作系统并使用 SSH 进行访问。与其他 TPA 平台（Docker 和 AWS）不同，Linux 主机配置不配置目标主机。

本集群示例使用linux主机来部署集群节点，这些节点包括三个复制的数据库节点、三个共存的连接代理proxy和一个备份节点。然后，TPA 可以向每个节点配置、准备和部署所需的PGD软件和配置。
## 二、	先决条件
### 配置 Linux 主机
准备四台linux主机，每台主机安装PGD支持的linux操作系统，每台主机配置为使用证书密钥对进行 SSH 访问，这样可以不用输入密码实现主机间的访问。
|主机名称 	|操作系统 	|公共IP地址	|私有IP 地址
|---------|---------|---------|---------
|linuxhost-1 	|Oracle linux 8.5 x86_64 	|192.168.31.247	|192.168.31.247
|linuxhost-2	|Oracle linux 8.5 x86_64 	|192.168.31.248 	|192.168.31.248 
|linuxhost-3 	|Oracle linux 8.5 x86_64 	|192.168.31.249 	|192.168.31.249 
|linuxhost-4 	|Oracle linux 8.5 x86_64	|192.168.31.250 	|192.168.31.250 


