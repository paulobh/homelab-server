# Proxmox NAS Homelab Server - AOOSTAR WTR PRO

Documentação do servidor homelab rodando Proxmox, inspirado na estrutura do [TechHutTV](https://github.com/TechHutTV/homelab).

## 🚀 Status do Projeto
- **SO:** Proxmox VE
- **Objetivo:** Media Server (Arr stack), Home Assistant, Proxy, Monitoring e Vault (Storage).
- **Status:** Em desenvolvimento (Work in Progress).

## 🛠️ Hardware

### Compute & Storage (All-in-One)

| Componente | Detalhes |
| :--- | :--- |
| **Modelo** | [AOOSTAR WTR PRO (4-Bay)](https://aoostar.com/products/aoostar-wtr-pro-4-bay-90t-storage-amd-ryzen-7-5825u-nas-mini-pc) |
| **CPU** | AMD Ryzen 7 5825u (8C/16T) |
| **RAM** | 32GB (2x 16GB) DDR4-3200 SODIMM |
| **NVMe 0 (Boot)** | 1TB NVMe (Partições Root/LVM/Swap) |
| **NVMe 1 (Flash)** | 1TB NVMe (Pool ZFS `flash` - VMs/Containers) |
| **HDD Pool (Tank)** | 4x Seagate IronWolf 8TB (Pool ZFS `tank` - RAIDZ1, ~21TB livres) |
| **Rede** | 2x 2.5GbE LAN |

---

## 📂 Status dos Serviços no Proxmox (Atualizado em 22/03/2026)

| Host | IP Local | Função | Detalhes |
| :--- | :--- | :--- | :--- |
| **Proxmox Host** | `192.168.1.199` | Hipervisor | Gestão de VMs e ZFS. |
| **VM 101 (servarr)** | `192.168.1.101` | Docker / Media | 6 Cores, 24GB RAM. Roda a stack Arr. |
| **LXC 100 (media)** | `192.168.1.100` | NAS / Samba | Ponto de montagem de 5.4TB (`tank`). |

### 🛠️ O que foi feito hoje:
1.  **Acesso SSH:** Configurada chave `ED25519` do Windows para o Proxmox e VM `servarr`. Acesso agora é sem senha.
2.  **Auditoria ZFS:**
    *   Pool `tank` (HDDs): ~29TB total, RAIDZ1. Dataset de mídia está em `tank/subvol-100-disk-0` (5.36TB em uso).
    *   Pool `flash` (NVMe): ~928GB total. Usado para discos de sistema (Root e VM 101).
3.  **NordVPN Meshnet:**
    *   Reativada via Login Token na VM `servarr`.
    *   Hostname: `paulo.amcampolina-altai.nord`.
    *   Dispositivos locais autorizados (`atlas`, `everest`, `olympic`, etc).
4.  **Correção da Stack Docker (Gluetun):**
    *   Identificado erro de DNS/Conexão no Gluetun.
    *   Instalado `wireguard-tools` e extraída a `WIREGUARD_PRIVATE_KEY` do NordLynx.
    *   Arquivo `.env` atualizado e container reiniciado. **Status: Healthy**.
    *   qBittorrent e Prowlarr religados e operacionais.

---

## 📝 Próximos Passos (Para a próxima sessão)

- [ ] **Recuperar Senhas:** qBittorrent (tentar `admin`/`adminadmin`) e Jellyfin.
- [ ] **Samba no LXC 100:** Validar se a pasta `/data` (5.4TB) está acessível pelo Windows Explorer.
- [ ] **Configuração do Jellyfin:** Verificar se as bibliotecas de filmes/séries estão apontando corretamente para o mountpoint do LXC.
- [ ] **Backup:** Configurar Proxmox Backup Server para o pool `tank`.

## 📂 Estrutura de Serviços (Media Stack)

| Serviço | Porta | Descrição |
| :--- | :--- | :--- |
| **Jellyfin** | 8096 | Player de Mídia |
| **Jellyseerr** | 5055 | Gerenciador de Pedidos |
| **Radarr** | 7878 | Filmes |
| **Sonarr** | 8989 | Séries |
| **Lidarr** | 8686 | Livros/Música |
| **Bazarr** | 6767 | Legendas |
| **qBittorrent** | 8080 | Downloads |
| **Prowlarr** | 9696 | Indexadores (Torrents) |
| **FlareSolverr** | 8191 | Proxy para Desafios Cloudflare |

### 🌐 Rede & Acesso Remoto (NordVPN Meshnet)
O acesso externo é feito via Meshnet. Notas de configuração:
- Comando: `nordvpn meshnet peer local allow`
- Hostname do Servidor: `paulo.amcampolina-altai.nord`

**Dispositivos Conectados:**
- Pixel 9 (`atlas`)
- Note (`olympic`)
- Tablet (`alps`)
- Projetor (`sierranevada`)

---

## 📝 Próximos Passos (To-Do)

- [ ] **Configuração do Proxmox:** Revisar a instalação e otimizar o uso dos NVMe.
- [ ] **ZFS Pool:** Validar a saúde do Vault Pool (8TB x4).
- [ ] **Stacks Docker:** Começar a migrar/configurar os serviços na pasta `/DEV/homelab-server/`.
- [ ] **Backup:** Definir estratégia de backup (3-2-1) para o Vault Pool.
- [ ] **Network:** Configurar VLANs ou isolamento para IoT/Home Assistant se necessário.

---

## 🔗 Referências Úteis
- [Repo TechHutTV](https://github.com/TechHutTV/homelab)
- [Documentação Proxmox](https://pve.proxmox.com/wiki/Main_Page)
- [Wiki do AOOSTAR WTR PRO](https://aoostar.com/products/aoostar-wtr-pro-4-bay-90t-storage-amd-ryzen-7-5825u-nas-mini-pc)
