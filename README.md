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
> L3 network and DNS interoperability

![](./images/overview2.png)
> Gitops: gitea + terraform + atlantis

![](./images/overview3.png)
> MkDocs + Jenkins hosted Markdown notes

![](./images/overview40.jpg)
> Master site main compute node & ai node


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

#### HomeLab Slave Site Physical Network Arch
```mermaid
flowchart TD
    subgraph ServerA ["ServerA: nj02j4125 (ESXi)"]
        vSwitch0["vSwitch0<br>(Uplink: eth1)"]
        
        subgraph OPNsenseVM ["nj02propns01"]
            OPN_WAN["WAN (来自 USB-MiFi)"]
            OPN_LAN_Bridge["LAN (Bridge)"]
            OPN_vNIC["vNIC (来自 OPN_PG)"]
            OPN_Passthrough["Passthrough eth2"]
        end

        vmk0["vmk0 (管理口 192.168.21.20)"]
        OPN_PG["PortGroup (OPN_Link)"]

        vSwitch0 --- vmk0
        vSwitch0 --- OPN_PG
        OPN_PG -. "虚拟连接" .-> OPN_vNIC
        OPN_vNIC --- OPN_LAN_Bridge
        OPN_Passthrough --- OPN_LAN_Bridge
    end

    subgraph ServerB ["ServerB: nj02x10dai (ESXi)"]
        vSwitch_Home["vSwitch_Home<br>(Uplink: eth0)"]
        vSwitch_Corp["vSwitch_Corp<br>(Uplink: eth1)"]
        Home_VMs["*.nj02.wsy:pr 网段<br>(192.168.21.0/24)"]
        Corp_VMs["*.nj02.wsy:dev 网段<br>(192.168.22.0/24)"]
        
        vSwitch_Home --- Home_VMs
        vSwitch_Corp --- Corp_VMs
    end

    subgraph PhysicalNet ["🌐 物理网络"]
        Internet["Internet"]
        MiFi["MiFi (USB)"]
        HomeLAN["nj02prwrt02<br>(Xiaomi AC2100)"]
        CorpNet["🏢 公司内网"]
    end

    %% 逻辑连接
    Internet --- MiFi
    MiFi --- OPN_WAN
    
    %% OPNsense LAN 的两个出口
    OPN_LAN_Bridge -- "(通过 vNIC)" --> vSwitch0
    OPN_LAN_Bridge -- "(通过 eth2)" --> HomeLAN
    
    %% vSwitch 的物理上联
    vSwitch0 -- "(Uplink eth1)" --> HomeLAN
    vSwitch_Home -- "(Uplink eth0)" --> HomeLAN
    vSwitch_Corp -- "(Uplink eth1)" --> CorpNet

    %% 样式定义 (莫兰迪/马卡龙配色，兼容 GitHub 暗黑/亮色模式)
    classDef serverA fill:#e3f2fd,stroke:#1565c0,stroke-width:2px,color:#000
    classDef opnVM fill:#bbdefb,stroke:#0d47a1,stroke-width:2px,color:#000
    classDef serverB fill:#f3e5f5,stroke:#6a1b9a,stroke-width:2px,color:#000
    classDef physNet fill:#e8f5e9,stroke:#2e7d32,stroke-width:2px,color:#000

    %% 应用样式
    class vSwitch0,vmk0,OPN_PG serverA
    class OPN_WAN,OPN_LAN_Bridge,OPN_vNIC,OPN_Passthrough opnVM
    class vSwitch_Home,vSwitch_Corp,Home_VMs,Corp_VMs serverB
    class Internet,MiFi,HomeLAN,CorpNet physNet
```

#### HomeLab DevSecOps Partial Design
```mermaid
graph TD
    %% Styling
    classDef source fill:#4CAF50,stroke:#388E3C,stroke-width:2px,color:#fff;
    classDef ci fill:#2196F3,stroke:#1976D2,stroke-width:2px,color:#fff;
    classDef sec fill:#F44336,stroke:#D32F2F,stroke-width:2px,color:#fff;
    classDef artifact fill:#FF9800,stroke:#F57C00,stroke-width:2px,color:#fff;
    classDef iac fill:#9C27B0,stroke:#7B1FA2,stroke-width:2px,color:#fff;
    classDef deploy fill:#607D8B,stroke:#455A64,stroke-width:2px,color:#fff;

    subgraph "1. Plan + Code (Version Control)"
        G[Gitea<br/>Monorepo: IaC, Dotfiles, Apps]:::source
    end

    subgraph "2. Continuous Integration & Security (Shift-Left)"
        GA[Gitea Actions<br/>Fast Checks & Linting]:::ci
        J[Jenkins<br/>Heavy Builds & Orchestration]:::ci
        Sec["(Security Scanning)<br/>tfsec, ansible-lint, secrets check"]:::sec
        
        G -->|Push / PR| GA
        G -->|Webhook Trigger| J
        GA -.-> Sec
        J -.-> Sec
    end

    subgraph "3. Artifact Management"
        AK[Artifact Keeper<br/>Docker Images, Binaries, ISOs]:::artifact
        
        GA -->|Push passing code| AK
        J -->|Push built artifacts| AK
    end

    subgraph "4. Infrastructure as Code & Config"
        P[Packer<br/>Golden Image Builds]:::iac
        T[Terraform<br/>Resource Provisioning]:::iac
        A[Ansible<br/>Configuration & Hardening]:::iac
        
        AK --> P
        J -->|Trigger Pipeline| P
        P -->|New Image ready| T
        J -->|Trigger Deployment| T
        T -->|Inventory Hand-off| A
    end

    subgraph "5. Operations & Target Environment"
        Target[vSphere / ESXi Clusters<br/>nj01, nj02, dl01]:::deploy
        
        T -->|Atlantis Provisions VMs/Networks| Target
        A -->|Deploys Services & Applies OS Configs| Target
    end
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

#### Cloud Native & BareMetal

<p align="left">
  <img src="./icons/linux.svg" alt="linux" height="28">
  <img src="./icons/docker.svg" alt="docker" height="28">
  <img src="./icons/kubernetes.svg" alt="kubernetes" height="28">
  <img src="./icons/haproxy.svg" alt="haproxy" height="28">
  <img src="./icons/nginx.svg" alt="nginx" height="28">
  <img src="./icons/keepalived.png" alt="keepalived" height="28">
  <img src="./icons/windows.svg" alt="windows" height="28">
</p>
