# Elastic Agent no Google Cloud

Este repositório documenta a instalação e configuração do Elastic Agent no Google Cloud Platform (GCP) para monitoramento de logs e métricas. Ele cobre desde a configuração inicial até o funcionamento pleno, com passos ilustrados para facilitar a replicação.

<p align=center>
  <img src="https://github.com/italobaracho/gcp_elastic/blob/main/imgs/Estrutura%20Completa.png" width="90%">
</p>

### Índice
- [Pré-requisitos](#pré-requisitos)
- [Configuração do Google Cloud](#configuração-do-google-cloud)
  - [Criar uma VM no Compute Engine](#criar-uma-vm-no-compute-engine)
  - [Habilitar APIs Necessárias](#habilitar-apis-necessárias)
- [Configuração do Elastic Cloud](#configuração-do-elastic-cloud)
  - [Adicionar Integração do Google Cloud no Kibana](#adicionar-integração-do-google-cloud-no-kibana)
- [Instalar Elastic Agent na VM](#instalar-elastic-agent-na-vm)
- [Validar e Monitorar no Kibana](#validar-e-monitorar-no-kibana)
- [Erros Comuns e Soluções](#erros-comuns-e-soluções)

---

## **Pré-requisitos**
Antes de começar, certifique-se de ter:
- Conta no [Google Cloud Platform](https://cloud.google.com/).
- Acesso ao [Elastic Cloud](https://cloud.elastic.co/).
- Permissões para gerenciar VMs e APIs no GCP.

---
- Página inicial do Console do Google Cloud com o projeto selecionado.
<p align=center>
  <img src="https://github.com/italobaracho/gcp_elastic/blob/main/imgs/1.png" width="80%">
</p>

## **Configuração do Google Cloud**

### **Criar uma VM no Compute Engine**
1. No Console do Google Cloud, vá para **Compute Engine > Instâncias de VM**.
2. Clique em **Criar Instância**.
3. Configure:
   - Nome: `elastic-agent-vm`
   - Região: `us-central1-a`
   - Tipo de máquina: `e2-micro`
   - Sistema operacional: Ubuntu 20.04 LTS.
4. Clique em **Criar**.

---
- Configuração de uma nova VM no Compute Engine. 
<p align=center>
  <img src="https://github.com/italobaracho/gcp_elastic/blob/main/imgs/2.png" width="80%">
</p>

### **Habilitar APIs Necessárias**
1. Vá para **APIs e Serviços > Biblioteca**.
2. Ative as seguintes APIs:
   - **Cloud Monitoring API**
   - **Cloud Logging API**
   - **Pub/Sub API**

---
- Tela da biblioteca de APIs no GCP, destacando as APIs habilitadas.
<p align=center>
  <img src="https://github.com/italobaracho/gcp_elastic/blob/main/imgs/3.png" width="80%">
</p>

## **Configuração do Elastic Cloud**

### **Adicionar Integração do Google Cloud no Kibana**
1. No Kibana, acesse **Management > Fleet > Integrations**.
2. Adicione a integração do **Google Cloud Platform**.
3. Configure:
   - Project ID: Insira o ID do projeto do GCP.
   - Credentials Json: Cole o conteúdo do arquivo JSON da conta de serviço.
4. Ative os inputs necessários, como:
   - Logs de Auditoria (Pub/Sub)
   - Métricas de Compute, Storage e outros serviços.

---
- Configuração da integração do Google Cloud no Kibana.
<p align=center>
  <img src="https://github.com/italobaracho/gcp_elastic/blob/main/imgs/4.png" width="80%">
</p>

## **Instalar Elastic Agent na VM**

### **Conectar-se à VM**
1. Acesse a VM via SSH.
   - Pelo Console do GCP: clique no botão **SSH** ao lado da VM.
   - Ou use o `gcloud CLI`:
     ```bash
     gcloud compute ssh elastic-agent-vm --zone=us-central1-a
     ```

---
- Conexão via SSH no Console do GCP.
<p align=center>
  <img src="https://github.com/italobaracho/gcp_elastic/blob/main/imgs/5.png" width="80%">
</p>

### **Instalar o Elastic Agent**
1. Baixe e instale o Elastic Agent:
   ```bash
   curl -L -O https://artifacts.elastic.co/downloads/beats/elastic-agent/elastic-agent-8.10.2-linux-x86_64.tar.gz
   tar xzvf elastic-agent-8.10.2-linux-x86_64.tar.gz
   cd elastic-agent-8.10.2-linux-x86_64
   sudo ./elastic-agent install --url=<Fleet_Server_URL> --enrollment-token=<Enrollment_Token>
2. Substitua <Fleet_Server_URL> e <Enrollment_Token> pelos valores obtidos no Kibana.

- Progresso na instalação do Elastic Agent.
<p align=center>
  <img src="https://github.com/italobaracho/gcp_elastic/blob/main/imgs/6.png">
</p>

### **Validar e Monitorar no Kibana**
1. No Kibana, acesse Fleet.
2. Verifique se o Elastic Agent aparece como Online.
3. Confira os dados em Observability > Logs e Metrics.

- Dashboard com as metricas e logs do GCP.
<p align=center>
  <img src="https://github.com/italobaracho/gcp_elastic/blob/main/imgs/7.png">
</p>


### **Erros Comuns e Soluções**
1. Invalid Project ID.
   - Certifique-se de que o Project ID está correto no Kibana.
     
2. Elastic Agent Offline.
   - Verifique a conexão da VM com o Fleet Server.
   - Verifique as credenciais.
  
3. Dados não aparecem no Kibana.
   - Confirme que as APIs necessárias estão habilitadas no GCP.
   - Verifique a configuração do Pub/Sub e Logs Router.
   

