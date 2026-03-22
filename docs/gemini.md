# Documentação: Configuração de Agente de IA do Google no Terminal (Git Bash / Windows)

## Objetivo
Configurar uma alternativa oficial do Google ao "Claude Code" para rodar diretamente no terminal. A ferramenta oficial do Google para este fim é o **Gemini CLI** (`@google/gemini-cli`), que é um agente de terminal baseado em Node.js.

## Histórico de Execução

### Fase 1: Tentativa de uso via Google Cloud SDK (gcloud)
Como o ambiente não possuía o gerenciador de pacotes `npm` (Node.js) instalado nativamente, tentamos utilizar o `gcloud` CLI como uma alternativa para interagir com a IA.

**Passos executados:**
1. Remoção de uma pasta de instalação antiga/conflitante:
   ```bash
   rm -rf /c/Users/paulo/google-cloud-sdk
   ```
2. Download e instalação limpa do Google Cloud SDK:
   ```bash
   curl https://sdk.cloud.google.com | bash
   ```
3. Validação da instalação:
   ```bash
   gcloud --version
   ```
   *Resultado:* Sucesso (Instalado `Google Cloud SDK 561.0.0` e dependências).

### Fase 2: Falha na execução do agente via gcloud
Tentativa de iniciar o chat interativo com a IA usando os componentes alpha do gcloud.
* **Comando executado:** `gcloud alpha genai chat`
* **Resultado:** Erro (`Invalid choice: 'genai'`). 
* **Conclusão:** O `gcloud` padrão não possui um agente de terminal interativo (capaz de ler diretórios e executar comandos) que seja equivalente ao Claude Code. A ferramenta correta é exclusivamente o `@google/gemini-cli`.

### Fase 3: Solução Definitiva (Acesso ao Gemini CLI)
Para ter a experiência completa do agente da Google no terminal, é necessário instalar o ecossistema Node.js. O plano de ação estabelecido para o Windows (Git Bash) foi:

1. Instalar o Node.js nativamente via Winget:
   ```bash
   winget install OpenJS.NodeJS
   ```
2. Reiniciar o terminal (fechar e abrir novamente) para carregar as variáveis de ambiente.
3. Instalar o pacote global do agente oficial da Google:
   ```bash
   npm install -g @google/gemini-cli
   ```
4. Autenticar e iniciar o agente:
   ```bash
   gemini
   ```
