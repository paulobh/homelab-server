# Homelab Server - AOOSTAR WTR PRO

Bem-vindo ao repositório do meu homelab! Aqui organizo as configurações, documentação e stacks do meu servidor Proxmox.

## 🗺️ Navegação

*   **[Audit do Homelab](docs/homelab.md)** - Visão técnica atualizada (IPs, ZFS, VMs).
*   **[Setup do Gemini CLI](docs/gemini.md)** - Guia de como configurei este agente no Windows.
*   **[Media Stack](media/README.md)** - Configuração do Jellyfin e a stack Arr.
*   **[Storage & Backup](storage/README.md)** - Detalhes sobre ZFS, Samba e Proxmox.

---

## 🛠️ Hardware Principal

### [AOOSTAR WTR PRO](https://aoostar.com/products/aoostar-wtr-pro-4-bay-90t-storage-amd-ryzen-7-5825u-nas-mini-pc) (Proxmox VE)

*   **CPU:** AMD Ryzen 7 5825u
*   **RAM:** 32GB DDR4-3200
*   **Boot:** 1TB NVMe (OS Proxmox)
*   **Pool Flash:** 1TB NVMe (VMs e Containers)
*   **Pool Tank:** 4x Seagate IronWolf 8TB (RAIDZ1 - Armazenamento de Mídia)

---

## 🚀 Status Atual
O servidor está operacional com a stack de mídia protegida via VPN (Gluetun) e acesso remoto via NordVPN Meshnet.

---
*Este repositório é um trabalho em progresso.*
