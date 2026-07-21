<div align="center">
   <h1>Hi there, I'm Shuyang <img src="https://media.giphy.com/media/hvRJCLFzcasrR4ia7z/giphy.gif" width="25px"> </h1>
</div>

<div align="center">
<h3><img src="https://media.giphy.com/media/WUlplcMpOCEmTGBtBW/giphy.gif" width="30">  💻 Infra | DevOps Engineer <img src="https://media.giphy.com/media/WUlplcMpOCEmTGBtBW/giphy.gif" width="30"></h3>
</div>

<br />
<img align="right" height="448px" width="320px" alt="GIF" src="https://media0.giphy.com/media/v1.Y2lkPTc5MGI3NjExcHdveDk4MGFzZmt2cTQ0d29oaGZsdGU4aDI1bXpmaWp6bXFobmszdyZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/HUplkVCPY7jTW/giphy.gif" />

<br />
<br />
<br />
<br />

- Into HomeLab and Self-Hosted services.

- Try to automate All my work into CI/CD pipeline.

- I do DevSecOps, Infra and a bit of everything :heart:

- As heaven's movement is ever vigorous, so must a gentleman ceaselessly strive along.

- btw I use CachyOS. <img src="https://raw.githubusercontent.com/CachyOS/CachyOS-icons/refs/heads/master/Colored/CachyOS.svg" alt="CachyOS" height="22px">

<br />
<br />
<br />
<br />

<br />

<br />

### Academic Ability
- Invention Patent: [一种多终端多无人机分层调度辅助边缘计算资源分配方法](https://cpquery.cponline.cnipa.gov.cn/detail/index?zhuanlisqh=8x5aXikTaWvidHSvrrUcbg%253D%253D&anjianbh&searchType=1)
- IEEE Paper: [Optimization of energy consumption with hybrid cooperative NOMA for secure MEC](https://ieeexplore.ieee.org/document/10062073)

### Engineering Ability
#### HomeLab Overview
![](./images/overview1.png)
![](./images/overview2.png)
![](./images/overview3.png)

#### HomeLab Network Logic Architecture
```mermaid
flowchart TD
    %% 全局已经声明了 TD，去掉子图内部的 direction 避免 GitHub 渲染引擎的边界降级 Bug

    subgraph Cloud ["☁️ 云上基础设施 (SD-WAN 控制平面)"]
        LA["LA VPS<br>(Headscale / DERP / Trojan)"]
        SG["SG AWS EC2<br>(DERP / Reality)"]
        SZ["SZ 阿里云 ECS<br>(DERP)"]
        
        LA -.- SG
        LA -.- SZ
    end

    subgraph NJ01 ["📍 nj01 站点 (核心计算与 AI 节点)"]
        R1["H3C 主路由 (192.168.11.1)<br>PPPoE 拨号"]
        W1["nj01prwrt01 (192.168.11.2)<br>旁路网关 (OpenClash)<br>VLAN 卸载"]
        P1["nj01prpi01 (192.168.11.3)<br>DHCP / Technitium DNS / Tailscale Router"]
        
        Net11["PR 网段: 192.168.11.0/24"]
        Net12["DEV 网段: 192.168.12.0/24"]

        R1 --- W1
        W1 --- Net11
        W1 --- Net12
        P1 -. "1. 所有 DNS 请求" .-> W1
        W1 -. "2. *.wsy 解析回传" .-> P1
    end

    subgraph NJ02 ["📍 nj02 站点 (从站点与公司内网代理)"]
        WIFI["随身 WiFi (WAN)"]
        OP2["nj02propns01 (192.168.21.1)<br>主路由 / OPNSense / VLAN / Tailscale"]
        W2["nj02prwrt01 (192.168.21.2)<br>旁路网关 (OpenClash)"]
        AP2["nj02prwrt02 (192.168.21.3)<br>小米 AC2100 (AP)"]
        WCT["nj02devwct01<br>代理公司网关 (Namespace 隔离)"]
        
        Net21["PR 网段: 192.168.21.0/24"]
        Net22["DEV 网段: 192.168.22.0/24"]
        CorpNet(("🏢 公司内网"))

        WIFI --- OP2
        OP2 --- W2
        OP2 --- AP2
        AP2 --- Net21
        AP2 --- Net22
        OP2 --- Net21
        OP2 --- Net22
        Net22 -.- WCT
        WCT === CorpNet
        
        W2 -. "*.wsy 解析转发" .-> OP2
    end

    subgraph DL01 ["📍 dl01 站点 (异地容灾与备份)"]
        R3["华为 主路由 (192.168.31.1)<br>PPPoE 拨号"]
        W3["dl01prwrt01<br>旁路网关 (OpenClash) / Tailscale"]
        NAS["dl01x10dai<br>TrueNAS (容灾)"]
        
        Net31["PR 网段: 192.168.31.0/24"]

        R3 --- W3
        W3 --- Net31
        Net31 --- NAS
    end

    %% 穿透子图的三层互通连线
    P1 <-->|Tailscale 三层互通 & Split DNS| LA
    OP2 <-->|Tailscale 三层互通 & Split DNS| LA
    W3 <-->|Tailscale 三层互通 & Split DNS| LA

    classDef cloud fill:#e3f2fd,stroke:#1565c0,stroke-width:2px,color:#000
    classDef nj01 fill:#f3e5f5,stroke:#6a1b9a,stroke-width:2px,color:#000
    classDef nj02 fill:#e8f5e9,stroke:#2e7d32,stroke-width:2px,color:#000
    classDef dl01 fill:#fff3e0,stroke:#e65100,stroke-width:2px,color:#000
    
    class LA,SG,SZ cloud
    class R1,W1,P1,Net11,Net12 nj01
    class WIFI,OP2,W2,AP2,WCT,Net21,Net22 nj02
    class R3,W3,NAS,Net31 dl01
```

### Tech Stack

---

#### Languages & Env

<p align="left">
  <img src="./icons/ansible.svg" alt="ansible" height="28">
  <img src="./icons/terraform.svg" alt="terraform" height="28">
  <img src="./icons/packer.svg" alt="packer" height="28">
  <img src="./icons/bash.svg" alt="bash" height="28">
  <img src="./icons/python.svg" alt="python" height="28">
  <img src="./icons/neovim.svg" alt="neovim" height="28">
  <img src="./icons/tmux.svg" alt="tmux" height="28">
  <img src="./icons/opencode.svg" alt="opencode" height="14px">
</p>

#### Cloud

<p align="left">
  <img src="./icons/azure.svg" alt="azure" height="24">
  <img src="./icons/tencentcloud.svg" alt="tencentcloud" height="24">
  <img src="./icons/aws.svg" alt="aws" height="28">
  <img src="./icons/alibabacloud.svg" alt="alibabacloud" height="24">
  <img src="./icons/google-cloud.svg" alt="google-cloud" height="28">
  <img src="./icons/vsphere.svg" alt="vsphere" height="28">
  <img src="./icons/openstack.svg" alt="openstack" height="28">
</p>

#### IaaS & PaaS

<p align="left">
  <img src="./icons/azure.svg" alt="azure" height="28">
  <img src="./icons/tencentcloud.svg" alt="tencentcloud" height="28">
  <img src="./icons/aws.svg" alt="aws" height="28">
  <img src="./icons/alibabacloud.svg" alt="alibabacloud" height="28">
  <img src="./icons/google-cloud.svg" alt="google-cloud" height="28">
  <img src="./icons/vsphere.svg" alt="vsphere" height="28">
  <img src="./icons/openstack.svg" alt="openstack" height="28">
</p>
