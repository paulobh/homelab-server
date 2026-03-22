# Plano de Implementação: Monitoramento Homelab

Este documento detalha os passos para configurar a stack de monitoramento (Grafana, Prometheus, InfluxDB) no servidor Proxmox.

## 📌 Arquitetura Proposta
- **Central de Monitoramento:** VM 101 (`servarr` - 192.168.1.190).
- **Banco de Dados:** InfluxDB 2.x (Métricas de Longo Prazo) e Prometheus (Métricas de Sistema).
- **Visualização:** Grafana.

---

## 🛠️ Passo a Passo

### 1. Preparação da VM 101
Acessar a VM via SSH e criar o diretório de monitoramento:
```bash
mkdir -p /docker/monitoring/{grafana,prometheus,influxdb,telegraf}
```

### 2. Implantação da Stack (Docker Compose)
Criar o arquivo `compose.yaml` com os serviços Grafana, Prometheus e InfluxDB. 
*Nota: Usaremos a rede `servarrnetwork` já existente.*

### 3. Configuração do Proxmox (External Metrics)
O Proxmox pode enviar dados nativamente para o InfluxDB:
1. No Proxmox, vá em **Datacenter > Metrics Server**.
2. Adicione um novo servidor do tipo **InfluxDB**.
3. Aponte para o IP `192.168.1.190` e porta `8086`.

### 4. Instalação de Agentes (Node Exporters)
Em cada máquina (Host, VM 101, LXC 100), rodar:
```bash
sudo apt update && sudo apt install prometheus-node-exporter -y
```

### 5. Dashboards Recomendados
Após subir o Grafana (porta 3000), importar estes IDs:
- **10048:** Proxmox (via InfluxDB).
- **1860:** Node Exporter Full (via Prometheus).
- **18389:** Docker Monitoring (via Telegraf).

---

## 📝 Status das Tarefas
- [ ] Criar estrutura de arquivos local.
- [ ] Definir `compose.yaml` de monitoramento.
- [ ] Configurar InfluxDB e Bucket de métricas.
- [ ] Integrar Proxmox Metrics Server.
- [ ] Validar Dashboards no Grafana.
