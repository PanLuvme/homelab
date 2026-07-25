

<div align="center">

<div align="center">

<table>
  <tr>
    <td style="background-color: transparent !important; border: none;">
      <img width="2000" height="463" alt="Homelab" src="https://github.com/user-attachments/assets/26f30e85-5495-42a0-8df1-a287a500d95d">
    </td>
  </tr>
</table>


</div>

Servers Status:

![Oracle Server Status](https://img.shields.io/uptimerobot/status/m803111856-ab88ba770f497df925eedb84?label=Oracle%20VM)
![RPI Server Status](https://img.shields.io/uptimerobot/status/m803111877-c56b8c76d1ce6c784b38bfdd?label=%20RPI)
![ProxMox Status](https://img.shields.io/badge/ProxMox-WoL-orange)


```mermaid
flowchart TB

    %% ========== REMOTE ACCESS ==========
    subgraph remote["Remote access"]
        direction LR
        admin["Admin workstation<br/>MacBook M4"]
        phone["Mobile<br/>Tailscale + WoLow"]
    end

    ts{{"Tailscale mesh VPN<br/>100.x.x.x overlay"}}

    %% ========== HOME NETWORK ==========
    subgraph lan["Home network — 10.0.0.0/24"]
        direction LR
        gw["Xfinity gateway<br/>10.0.0.1"]
        pi["Raspberry Pi 4<br/>always-on WoL relay"]
    end

    %% ========== HYPERVISOR ==========
    subgraph pve["Proxmox VE 9.2.4 — 10.0.0.50"]
        hw["ASUS ROG Zephyrus M15 GU502LV<br/>i7-10750H · 16 GB RAM · 931 GB NVMe<br/>Wake-on-LAN · on-demand power"]

        subgraph netsvc["Network services"]
            opn["OPNsense — VM<br/>firewall · VLANs · DHCP"]
            adg["AdGuard Home — LXC<br/>DNS filtering · local records"]
            wg["WireGuard — LXC<br/>VPN endpoint"]
        end

        subgraph ident["Identity and endpoints"]
            dc["Windows Server 2022 — VM<br/>AD DS · DNS · DHCP · GPO"]
            cli["Windows 11 — VM<br/>domain-joined client"]
        end

        subgraph itops["IT operations"]
            mon["Zabbix + Grafana — LXC<br/>monitoring · dashboards · alerts"]
            tick["GLPI — LXC<br/>helpdesk ticketing · asset inventory"]
        end

        subgraph stor["Storage and backup"]
            lvm[("local-lvm<br/>794 GB LVM-thin")]
            pbs["Proxmox Backup Server<br/>scheduled backups · restore testing"]
        end
    end

    %% ========== AUTOMATION ==========
    subgraph auto["Automation and documentation"]
        direction LR
        code["Ansible + PowerShell<br/>provisioning · bulk user scripts"]
        repo["GitHub<br/>IaC · runbooks · diagrams"]
    end

    %% ========== FLOWS ==========
    admin --> ts
    phone --> ts
    ts -->|"encrypted tunnel"| hw
    phone -.->|"magic packet"| pi
    pi -.->|"Wake-on-LAN"| hw
    gw --> pi
    gw --> hw

    hw --> opn
    hw --> adg
    hw --> wg
    hw --> tick
    hw --> lvm
    opn --> dc
    opn --> mon
    dc --> cli
    adg --> cli
    lvm --> pbs
    mon -.->|"agents"| dc
    mon -.->|"agents"| cli

    code --> hw
    code --> repo

    %% ========== STYLES ==========
    classDef access fill:#1e3a8a,stroke:#3b82f6,color:#ffffff
    classDef network fill:#7c2d12,stroke:#ea580c,color:#ffffff
    classDef identity fill:#4c1d95,stroke:#8b5cf6,color:#ffffff
    classDef operations fill:#064e3b,stroke:#10b981,color:#ffffff
    classDef storage fill:#334155,stroke:#94a3b8,color:#ffffff
    classDef automation fill:#831843,stroke:#ec4899,color:#ffffff
    classDef hostnode fill:#1c1917,stroke:#a8a29e,color:#ffffff

    class admin,phone,ts,gw,pi access
    class opn,adg,wg network
    class dc,cli identity
    class mon,tick operations
    class lvm,pbs storage
    class code,repo automation
    class hw hostnode
```


<h2 align="center">Hardware:</h2>

<table width="100%" align="center">
  <tr>
    <!-- Main PC -->
    <td width="20%" align="center">
      <img src="https://api.iconify.design/lucide:monitor.svg?color=%2358a6ff&width=48" alt="Main PC"><br><br>
      <b>PC</b><br>
      Ryzen 5 5600<br>
      RX 9060 XT<br>
      16GB RAM
    </td>
    <!-- MacBook Pro -->
    <td width="20%" align="center">
      <img src="https://api.iconify.design/lucide:laptop.svg?color=%2358a6ff&width=48" alt="MacBook Pro"><br><br>
      <b>MacBook M4</b><br>
      M4 Chip<br>
      macOS
    </td>
    <!-- Proxmox Laptop -->
    <td width="20%" align="center">
      <img src="https://api.iconify.design/lucide:server.svg?color=%2358a6ff&width=48" alt="Proxmox Laptop"><br><br>
      <b>Proxmox</b><br>
      Zephyrus M15<br>
      16GB RAM
    </td>
    <!-- RPI -->
    <td width="20%" align="center">
      <img src="https://api.iconify.design/lucide:cpu.svg?color=%2358a6ff&width=48" alt="RPI"><br><br>
      <b>Raspberry Pi</b><br>
      DietPi OS<br>
      8GB RAM
    </td>
    <!-- Oracle Cloud Server -->
    <td width="20%" align="center">
      <img src="https://api.iconify.design/lucide:cloud.svg?color=%2358a6ff&width=48" alt="Oracle"><br><br>
      <b>Oracle Cloud</b><br>
      4 vCPU<br>
      24GB RAM
    </td>
  </tr>
</table>
