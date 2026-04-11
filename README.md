# 📊 AWS Cloud Observability: Monitoramento Estratégico com Prometheus & Grafana

![Status](https://img.shields.io/badge/Status-Concluído-success)
![AWS](https://img.shields.io/badge/AWS-EC2-orange)
![Docker](https://img.shields.io/badge/Docker-Containers-blue)

## 🎯 Por que este projeto existe?
No cenário de computação em nuvem, "quem não mede, não gerencia". Este projeto nasceu da necessidade de transformar dados brutos de infraestrutura em **inteligência de negócio**. O objetivo foi criar uma stack de monitoramento de baixo custo e alta performance para garantir a disponibilidade e a saúde de aplicações rodando em instâncias AWS EC2.

---

## 🏗️ Arquitetura do Projeto
Abaixo, o fluxo de dados e a estrutura de containers orquestrada na AWS:

```mermaid
graph TD
    subgraph AWS_Cloud [AWS Cloud - EC2 Instance]
        direction TB
        NE[Node Exporter <br/><i>Coletor de Métricas</i>]
        PR[Prometheus <br/><i>Time-Series DB</i>]
        GR[Grafana <br/><i>Dashboards</i>]
        
        NE -- "Expõe métricas (:9100)" --> PR
        PR -- "Consulta dados" --> GR
    end

    User((Usuário/Admin)) -- "Acessa Dashboards (:3000)" --> GR
    Stress[Container Stress] -- "Gera Carga de CPU" --> NE
```

---

## 🚀 Validação Técnica e Stress Test
Para provar a eficácia da ferramenta, realizei um **Stress Test** simulando uma carga pesada de processamento:

### 1️⃣ O Gatilho (Stress via Container)
![Terminal Stress](screenshots/01_terminal_stress.png)
> *Execução do container de stress via SSH para elevar o uso de recursos do sistema.*

### 2️⃣ A Resposta (Observabilidade em Ação)
![Dash CPU](screenshots/02_dashboard_cpu_spike.png)
> *O dashboard reagiu instantaneamente ao pico de CPU atingindo 100%.*

---

## 🧠 Desafios e Soluções (Raciocínio Lógico)

| Desafio | Solução e Raciocínio |
| :--- | :--- |
| **Persistência de dados** | O Prometheus perdia o histórico ao reiniciar. Implementei **Docker Volumes** para mapear os dados do container para o disco da EC2, garantindo durabilidade. |
| **Segurança e Exposição** | Abrir portas públicas é arriscado. Configurei **Security Groups na AWS** para restringir o tráfego e usei redes internas do Docker para a comunicação entre serviços. |
| **Monitoramento de Hardware** | O Prometheus não lê o hardware diretamente. Utilizei o **Node Exporter** como ponte, uma solução leve que não consome recursos excessivos da instância. |

---

## 💰 Benefícios e Redução de Custos (Business Value)
* **Economia Direta:** Substituição de ferramentas pagas (Datadog/New Relic) por uma stack Open Source, eliminando custos de licenciamento.
* **Right-sizing:** Com as métricas de uso real, a empresa pode reduzir o tamanho de instâncias subutilizadas na AWS, cortando custos operacionais.
* **Proatividade:** Alertas visuais permitem agir antes que a indisponibilidade cause prejuízo financeiro.

### Visualização de Infraestrutura (Rede e Disco)
![Visao Geral](screenshots/03_visao_geral.png)

---

## 🛠️ Ferramentas Utilizadas e "Porquês"
* **AWS EC2:** Flexibilidade e padrão de mercado para computação em nuvem.
* **Docker & Compose:** Garante portabilidade e que o ambiente seja idêntico em qualquer região AWS.
* **Prometheus:** Líder em monitoramento moderno com modelo de coleta eficiente.
* **Grafana:** A melhor ferramenta do mercado para visualização e tomada de decisão executiva.

---
**Desenvolvido por Gustavo - Cloud & DevOps.**
