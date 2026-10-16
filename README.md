

# Laboratório AWS: Arquitetura Altamente Disponível e Escalável ☁️🚀

## 📌 Apresentação do Projeto
Este repositório contém a documentação e as evidências da execução prática do laboratório **"Auto Scaling e Elastic Load Balancer"**, realizado no curso da **[Escola da Nuvem](https://escoladanuvem.org/)**, sob instrução da professora **Josely Castro**.

O objetivo principal do laboratório é implementar e validar uma arquitetura web resiliente na AWS, utilizando **Auto Scaling Groups (ASG)**, **Launch Templates** e **Application Load Balancer (ALB)** em um ambiente controlado.

---

## 📐 Diagrama e Fluxo da Arquitetura


```text
                  [ Internet ]
                       │
                       ▼
         [ Application Load Balancer ]
                       │
        ┌──────────────┴──────────────┐
        ▼                             ▼
[ Subnet us-east-1a ]         [ Subnet us-east-1b ]
  └─► EC2 Instância 1           └─► EC2 Instância 2
```

## 🏗️ Arquitetura e Serviços Utilizados

- **Amazon EC2 (Elastic Compute Cloud):** Servidores virtuais executando o servidor web Apache (`httpd`).

- **AWS Launch Template:** Especificação padronizada da configuração das instâncias (AMI Amazon Linux 2023, tipo de instância, grupos de segurança e script de automação no *User Data*).

- **Auto Scaling Group (ASG):** Gerenciamento automático do ciclo de vida e da quantidade de instâncias EC2 distribuídas em múltiplas Zonas de Disponibilidade (AZs).

- **Application Load Balancer (ALB):** Distribuição inteligente do tráfego de entrada HTTP (porta 80) entre as instâncias saudáveis (*healthy*) registradas no Target Group.

- **Amazon VPC & Security Groups:** Configuração de rede virtual e regras de firewall liberando o tráfego de rede apropriado.

---

## 🛠️ Configuração Passo a Passo

### 1. Grupo de Segurança (Security Group)
- Configuração de regras de entrada (*Inbound Rules*) permitindo tráfego **HTTP (porta 80)** e **HTTPS (porta 443)** para a internet (`0.0.0.0/0`).

### 2. Launch Template
- **AMI:** Amazon Linux 2023
- **Tipo de Instância:** `t2.micro`
- **User Data (Automação de Boot):**
  ```bash
  #!/bin/bash
  dnf update -y
  dnf install -y httpd
  systemctl start httpd
  systemctl enable httpd
  echo "<h1>Servidor Web - Instancia: $(hostname -f) - Priscilla Ferreira</h1>" > /var/www/html/index.html
  ```

### 3. Auto Scaling Group & Application Load Balancer
- Configurado com distribuição em **múltiplas Subnets/AZs** (`us-east-1a` e `us-east-1b`) para garantir alta disponibilidade.
- **Capacidade do ASG:** `Desired: 2` | `Min: 2` | `Max: 2`.
- Integrado ao **Application Load Balancer** em conjunto com *Health Checks* ativos via Elastic Load Balancing.

---

## 📸 Evidências do Provisionamento na AWS

### 1. Modelo de Execução (Launch Template)
Configuração padronizada das instâncias EC2 (`LT-PriscillaFerreira`).
![Launch Template](imagens/Captura%20de%20Tela%20(2607).png)

### 2. Grupo do Auto Scaling (Auto Scaling Group)
Definição de capacidade e gerenciamento de instâncias em Múltiplas AZs (`ASG-PriscillaFerreira`).
![Auto Scaling Group](imagens/Captura%20de%20Tela%20(2609).png)

### 3. Application Load Balancer (ALB)
Status ativo e distribuição de tráfego configurada (`ALB-PriscillaFerreira`).
![Application Load Balancer](imagens/Captura%20de%20Tela%20(2611).png)

---

## 🔬 Validação do Balanceamento de Carga

Ao acessar o DNS público do **Application Load Balancer**, é possível comprovar que o tráfego é alternado dinamicamente entre as duas instâncias EC2 ativas no Auto Scaling Group:

#### Instância 1 (`ip-172-31-89-0`):
![Teste Instância 1](imagens/Captura%20de%20Tela%20(2612).png)

#### Instância 2 (`ip-172-31-3-0`):
![Teste Instância 2](imagens/Captura%20de%20Tela%20(2613).png)

---
## 🧠 Conhecimentos e Competências Adquiridas
* **Arquitetura de Alta Disponibilidade:** Dimensionamento de infraestrutura distribuída em múltiplas Zonas de Disponibilidade (Multi-AZ) para eliminação de Pontos Únicos de Falha (SPOF).

* **Automação e Bootstrap:** Configuração automatizada de servidores web Apache através de scripts em Shell/Bash (User Data) no momento do provisionamento.

* **Balanceamento de Carga:** Configuração de Application Load Balancer (ALB) com Target Groups e regras de roteamento HTTP.

* **Resiliência e Auto-recuperação:** Integração de Health Checks entre o ALB e o Auto Scaling Group para substituição automática de instâncias indisponíveis.

* **Segurança e Redes:** Gestão de regras de firewall virtual (Security Groups) e sub-redes públicas em VPCs.

* **Documentação Técnica:** Elaboração de documentação clara e padronizada de infraestrutura em nuvem para repositórios técnicos.
---